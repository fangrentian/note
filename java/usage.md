# 给现有jar包里添加文件

例如有个a.jar包, 想给`BOOT-INF/classes/static/`目录下添加一个`b.txt`文件, 首先在a.jar所在位置按'BOOT-INF/classes/static/'目录层级创建文件夹,再将b.txt复制进刚创建的目录, 最后通过以下命令更新a.jar包:

```shell
jar -uf .\a.jar BOOT-INF/classes/static/b.txt
```
