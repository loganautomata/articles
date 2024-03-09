# 安装NextCloud

## LNMP环境

PHP: 7.4

> 推荐7.3, 7.4中日志会不断反馈#5230错误, 暂未解决.

Nginx: 1.17.6

MySQL: 5.6

> 建议5.6及以上版本.

## 建立站点

1. 在宝塔下建立网站, 需要同时建立数据库.
2.  部署SSL证书, 开启强制https. 
3. 关闭跨站保护, 清空站点文件夹下所有文件.

## 安装NextCloud

1. [下载](https://download.nextcloud.com/server/releases/nextcloud-18.0.4.zip)NextCloud, 上传压缩包到服务器, 解压后将nextcloud文件夹下所有文件移动到你上一步建立的站点文件夹下.
2. 打开你站点的网址, 开始安装. 数据库选择MySQL, 填入数据库名字, 账号, 密码. 如果服务器在国内, 不勾选推荐安装的选项, 国外服务器自行选择.

## 安全与设置警告

1. 点开网站设置, 点击概览.
2. 逐一解决警告选项.

### 您的数据目录和文件可以从互联网直接访问。.htaccess 文件不起作用。强烈建议您配置 Web 服务器，以便数据目录不再可访问，或者你可以将数据目录移动到 Web 服务器文档根目录。

将以下禁止访问的目录代码加入到网站配置中.

```bash
    location ~ ^/(?:build|tests|config|lib|3rdparty|templates|data)/ {
        deny all;
    }
```

### PHP 的安装似乎不正确，无法访问系统环境变量。getenv("PATH") 函数测试返回了一个空值。 请参照安装说明文档 中的 PHP 配置说明查阅您服务器的PHP配置信息，特别是在使用 php-fpm 时。

打开/www/server/php/74/etc/php-fpm.conf，在其尾部添加以下代码. 保存并重启php-fpm-74.

```bash
env[PATH] = /usr/local/bin:/usr/bin:/bin:/usr/local/php/bin
```

### PHP内存低于建议值512M

在PHP配置修改中将memory_limit设置为512M即可.

### 所使用的数据库为MySQL但没有对4字节字符的支持。为正确处理文件名或评论中使用的4字节字符（比如emoji表情），建议开启MySQL的4字节字符支持。详细信息请阅读相关文档页面。

修改mysql的配置, 在 [mysqld] 配置段里加入以下内容.

```bash
innodb_large_prefix=true
innodb_file_format=barracuda
innodb_file_per_table=1
```

ssh连接服务器, 依次键入以下代码

```bash
#nextcloud替换为你数据库的账号名
mysql -u nextcloud -p
#nextcloud替换为你的数据库名
ALTER DATABASE nextcloud CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;
exit
cd /www/wwwroot/nextcloud && sudo -u www php occ config:system:set mysql.utf8mb4 --type boolean --value="true" && sudo -u www php occ maintenance:repair
```

#### 手动修复索引长度问题.

本来到这里已经结束了, 但如果你的问题仍然未被解决, 那你就需要以下步骤了. 正常安装不需要以下步骤, 正常安装不需要以下步骤, 正常安装不需要以下步骤!!!

```bash
mysql -u nextcloud -p
use nextcloud
```

复制键入以下所有.

```bash
ALTER TABLE `oc_addressbooks`
MODIFY COLUMN `principaluri`  varchar(191) CHARACTER SET utf8 COLLATE utf8_bin NULL DEFAULT NULL AFTER `id`
MODIFY COLUMN `uri`  varchar(191) CHARACTER SET utf8 COLLATE utf8_bin NULL DEFAULT NULL AFTER `displayname`;

ALTER TABLE `oc_authtoken`
MODIFY COLUMN `token`  varchar(191) CHARACTER SET utf8 COLLATE utf8_bin NOT NULL DEFAULT '' AFTER `name`;

ALTER TABLE `oc_calendarobjects`
MODIFY COLUMN `uri`  varchar(191) CHARACTER SET utf8 COLLATE utf8_bin NULL DEFAULT NULL AFTER `calendardata`;

ALTER TABLE `oc_calendars`
MODIFY COLUMN `principaluri`  varchar(191) CHARACTER SET utf8 COLLATE utf8_bin NULL DEFAULT NULL AFTER `id`,
MODIFY COLUMN `uri`  varchar(191) CHARACTER SET utf8 COLLATE utf8_bin NULL DEFAULT NULL AFTER `displayname`;

ALTER TABLE `oc_calendarsubscriptions`
MODIFY COLUMN `uri`  varchar(191) CHARACTER SET utf8 COLLATE utf8_bin NULL DEFAULT NULL AFTER `id`,
MODIFY COLUMN `principaluri`  varchar(191) CHARACTER SET utf8 COLLATE utf8_bin NULL DEFAULT NULL AFTER `uri`;

ALTER TABLE `oc_dav_shares`
MODIFY COLUMN `principaluri`  varchar(191) CHARACTER SET utf8 COLLATE utf8_bin NULL DEFAULT NULL AFTER `id`,
MODIFY COLUMN `type`  varchar(191) CHARACTER SET utf8 COLLATE utf8_bin NULL DEFAULT NULL AFTER `principaluri`,
MODIFY COLUMN `publicuri`  varchar(191) CHARACTER SET utf8 COLLATE utf8_bin NULL DEFAULT NULL AFTER `resourceid`;

ALTER TABLE `oc_login_flow_v2`
MODIFY COLUMN `poll_token`  varchar(191) CHARACTER SET utf8 COLLATE utf8_bin NOT NULL AFTER `started`,
MODIFY COLUMN `login_token`  varchar(191) CHARACTER SET utf8 COLLATE utf8_bin NOT NULL AFTER `poll_token`;

ALTER TABLE `oc_migrations`
MODIFY COLUMN `app`  varchar(191) CHARACTER SET utf8 COLLATE utf8_bin NOT NULL FIRST ,
MODIFY COLUMN `version`  varchar(191) CHARACTER SET utf8 COLLATE utf8_bin NOT NULL AFTER `app`;

ALTER TABLE `oc_mimetypes`
MODIFY COLUMN `mimetype`  varchar(191) CHARACTER SET utf8 COLLATE utf8_bin NOT NULL DEFAULT '' AFTER `id`;

ALTER TABLE `oc_systemtag_group`
MODIFY COLUMN `gid`  varchar(191) CHARACTER SET utf8 COLLATE utf8_bin NOT NULL AFTER `systemtagid`;

ALTER TABLE `oc_trusted_servers`
MODIFY COLUMN `url_hash`  varchar(191) CHARACTER SET utf8 COLLATE utf8_bin NOT NULL DEFAULT '' COMMENT 'sha1 hash of the url without the protocol' AFTER `url`;
```

```bash
exit
```

如果你的nextcloud主页面进入了维护模式, 还需要以下步骤.

#### .HTTP的请求头 “Strict-Transport-Security” 未设置为至少 “15552000” 秒。为了提高安全性，建议参照security tips 中的说明启用HSTS

在网站配置中加入以下代码.

```bash
add_header Strict-Transport-Security "max-age=63072000;";
```

### 您的网页服务器未正确设置以解析“/.well-known/caldav”。更多信息请参见文档。您的网页服务器未正确设置以解析“/.well-known/carddav”。更多信息请参见文档。

网站配置中添加两行重定向配置.

```bash
rewrite /.well-known/carddav /remote.php/dav permanent;
rewrite /.well-known/caldav /remote.php/dav permanent;
```

### 未找到 PHP 的 "fileinfo" 模块。强烈推荐启用该模块，从而获得更好的 MIME 类型探测结果。

安装php的fileinfo模块。

### 内存缓存未配置，为了提升使用体验，请尽量配置内存缓存。更多信息请参见文档。

安装redis并且安装php的redis模块. 在站点文件夹根目录/config/config.php下的config array内加入以下代码.

```bash
  'memcache.local' => '\OC\Memcache\Redis',
  'memcache.locking' => '\OC\Memcache\Redis',
  'memcache.locking' => '\OC\Memcache\Redis',
    'redis' => array(
    'host' => 'localhost',
    'port' => 6379,
  ),
```



### PHP 的 OPcache 模块未载入。推荐开启获得更好的性能。

安装php的OPcache模块, 并在php配置中修改以下参数.

```bash
opcache.max_accelerated_files=10000
```

添加以下参数.

```
opcache.save_comments=1
```

### 数据库丢失了一些索引。由于给大的数据表添加索引会耗费一些时间，因此程序没有自动对其进行修复。您可以在 Nextcloud 运行时通过命令行手动执行 "occ db:add-missing-indices" 命令修复丢失的索引。索引修复后会大大提高相应表的查询速度。数据库中的一些列由于进行长整型转换而缺失。由于在较大的数据表重改变列类型会耗费一些时间，因此程序没有自动对其更改。您可以通过命令行手动执行 "occ db:convert-filecache-bigint" 命令以应用挂起的更改。该操作需要当整个实例变为离线状态后执行。查阅[相关文档](https://docs.nextcloud.com/server/18/go.php?to=admin-bigint-conversion)以获得更多详情。

ssh连接服务器, 运行以下命令.

```bash
cd /www/wwwroot/nextcloud && sudo -u www php occ db:add-missing-indices && sudo -u www php occ db:convert-filecache-bigint
```

> PS: cd到你自己的站点文件夹下.

## 补充

解除exec和shell_exec函数禁用.