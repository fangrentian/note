<!--
 * @author: ares
 * @date: 2022-01-13 12:11:42
 * @lastEditTime: 2022-01-13 14:06:38
 * @lastEditors: ares
 * @description: 
 * 
-->
## 异常1 
### Cannot read property 'name' of nullat Array.forEach (<anonymous>)

一个用yarn workspaces控制的vue项目,单独copy出来后, **yarn build** 时报错, 怪异的地方是, 项目原来开发环境,正式环境都没报错,
移植出来后,开发环境OK, 打包时就报错, 报错信息如下: 

```
 error  in ./src/services/productServices.js

Syntax Error: Thread Loader (Worker 0)
Cannot read property 'name' of null
    at Array.forEach (<anonymous>)


 @ ./node_modules/cache-loader/dist/cjs.js??ref--12-0!./node_modules/thread-loader/dist/cjs.js!./node_modules/@vue/cli-plugin-babel/node_modules/babel-loader/lib!./node_modules/cache-loader/dist/cjs.js??ref--0-0!.
/node_modules/vue-loader/lib??vue-loader-options!./src/pages/order/mixins/OrderMixins.vue?vue&type=script&lang=js& 16:0-74 186:6-27
 @ ./src/pages/order/mixins/OrderMixins.vue?vue&type=script&lang=js&
 @ ./src/pages/order/mixins/OrderMixins.vue
 @ ./node_modules/cache-loader/dist/cjs.js??ref--12-0!./node_modules/thread-loader/dist/cjs.js!./node_modules/@vue/cli-plugin-babel/node_modules/babel-loader/lib!./node_modules/cache-loader/dist/cjs.js??ref--0-0!.
/node_modules/vue-loader/lib??vue-loader-options!./src/pages/order/components/List.vue?vue&type=script&lang=js&
 @ ./src/pages/order/components/List.vue?vue&type=script&lang=js&
 @ ./src/pages/order/components/List.vue
 @ ./node_modules/cache-loader/dist/cjs.js??ref--12-0!./node_modules/thread-loader/dist/cjs.js!./node_modules/@vue/cli-plugin-babel/node_modules/babel-loader/lib!./node_modules/cache-loader/dist/cjs.js??ref--0-0!.
/node_modules/vue-loader/lib??vue-loader-options!./src/pages/order/index.vue?vue&type=script&lang=js&
 @ ./src/pages/order/index.vue?vue&type=script&lang=js&
 @ ./src/pages/order/index.vue
 @ ./src/router/modules/order.js
 @ ./src/router/modules sync \.js$
 @ ./src/router/index.js
 @ ./src/router/guarder.js
 @ ./src/main.js
 @ multi ./src/main.js

 ERROR  Build failed with errors.
error Command failed with exit code 1.
info Visit https://yarnpkg.com/en/docs/cli/run for documentation about this command.

```

因为报 **Cannot read property 'name' of nullat Array.forEach (<anonymous>)** 这样的错误,开始一直以为是数组循环报错,一直没
排查出问题;

后来又排查node版本,依赖版本,都没有问题, 再看到第一行错误信息 **error  in ./src/services/productServices.js**, 在该文件中逐个
排查,最后发现问题点:

```js
export const getDetailService = (id) => {
    const params = { id }
    return service(product.getDetail, { params }).then(res => {
        if(!res.data.specAttrMap) {
            try {
                res.data.specAttrMap = JSON.parse(res.data.pSpu.specAttrList)
            } catch {
                res.data.specAttrMap = {}
            }
        }
        res.data.pSpu.labelNames = res.data.pSpu.labelName ? res.data.pSpu.labelName.split(',') : [];
        return res;
    });
};
```

原因是其中的try catch语句中 **catch** 后面少了圆括号导致报错,但是这和Array.forEach有啥关系? 而且 **yarn workspace**下没报错啊,
**yarn serve** 也没有报错啊, 莫名其妙!