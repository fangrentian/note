# 作用

输出不同日志级别的彩色符号

# 安装

```shell
npm install log-symbols
```

# 用法

```js
import logSymbols from 'log-symbols';

console.log(logSymbols.success, 'Finished successfully!');
// Terminals with Unicode support:     ✔ Finished successfully!
// Terminals without Unicode support:  √ Finished successfully!
```

# 支持的api

* logSymbols.info

* logSymbols.success

* logSymbols.warning

* logSymbols.error
