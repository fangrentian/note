# 安装think-cli

```shell
npm install -g think-cli
```

## 查看think-cli版本

```shell
thinkjs -V
```
 
能正常显示think-cli版本号(我用的版本是2.2.9),说明工具安装成功, 这里的版本号是工具的版本号,不是thinkjs的版本号.

# 用think-cli工具创建项目

```shell
thinkjs new thinkjsdemo
```

## 进入工程目录,安装依赖

```shell
cd thinkjsdemo
npm install
```

## 试运行下工程

```shell
npm start
```

出现以下提示,说明服务器正常运行起来了

```shell
> thinkjsdemo@1.0.0 start F:\nodeProjects\thinkjsdemo
> node development.js

[2022-01-20T10:52:57.094] [63612] [INFO] - Server running at http://127.0.0.1:8360
[2022-01-20T10:52:57.151] [63612] [INFO] - ThinkJS version: 3.2.14
[2022-01-20T10:52:57.152] [63612] [INFO] - Environment: development
[2022-01-20T10:52:57.152] [63612] [INFO] - Workers: 1
```

# 示例代码

[https://gitee.com/fangrentian/thinkjs_patient](https://gitee.com/fangrentian/thinkjs_patient)

[https://gitee.com/fangrentian/thinkjs_restful_api](https://gitee.com/fangrentian/thinkjs_restful_api)