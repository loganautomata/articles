# JAVA SE

## 关键字

### private protected public

#### 访问控制

| 访问权限  | 当前包 | 其他包 |
| --------- | ------ | ------ |
| public    | Y      | Y      |
| protected | Y      | N      |
| private   | N      | N      |

> PS: JAVA中控制符缺省时:
>
> | 访问权限 | 当前类 | 当前包 | 子类 | 其他包 |
> | -------- | ------ | ------ | ---- | ------ |
> | 缺省     | Y      | Y      | N    | N      |

### abstract

声明抽象类和抽象方法.

## 标识符

### 通用命名规范1.0(JAVA补)

1. 包名所有字母一律小写. 例如: com.hpe.example.