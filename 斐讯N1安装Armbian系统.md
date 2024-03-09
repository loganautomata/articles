# 斐讯N1安装Armbian5.77

## 制作Armbian5.77启动盘

1. 使用rufus或其它软件制作启动盘.

> 建议使用USB2.0U盘.
>
> [zjw939057120]("https://www.right.com.cn/forum/space-uid-197745.html")提供的Armbian5.77[地址]("https://pan.baidu.com/s/1v_aBEkdU4Y3R011MMSNLDg"),密码cxeo.

2. 替换dtb文件.

    将你自己的或下面的dtb文件复制粘贴到/boot/dtb目录, 修改/boot/uEnv.ini指向的dtb文件.

   > [xiangsm]("https://www.right.com.cn/forum/space-uid-487618.html")的dtb文件下载[地址]("https://www.right.com.cn/forum/forum.php?mod=attachment&aid=Mjc3MDU5fGI1MGIwYjZmfDE1OTkxMjA4MTl8MHw1MTA0MjM%3D").

3. 安装Armbian5.77

   系统初始用户名root, 密码1234.

   ```bash
   nand–sata-install
   ```

   

## 补充

设置命令(设置apt源, 时区, wifi等).

```bash
armbian-config
```

