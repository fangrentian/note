# 简介

apidoc用来生成借口文档, 它能在c#， Go, Dart, Java, JavaScript, PHP, TypeScript和所有其他支持Javadoc的语言中使用.

# 安装

```shell
npm install apidoc -g
```

# 生成文档

```shell
apidoc -i src -o apidoc
```

* -i: 输入目录
* -o: 输出目录

# 命令行参数

* -c | --config: 指定要使用的配置文件路径。(默认:输入目录中的apidoc.Json或apidoc.js)
```shell
apidoc -c path/to/apidoc.json
```
* -e | --exclude-filters: 指定排除的文件或目录(默认: [])
```shell
apidoc -e node_modules
```
* -f | --file-filters: 指定需要解析的文件, 默认是 `.`
```shell
apidoc -f ".*\\.js$" -f ".*\\.ts$"
```
* -i | --input: 输入目录
```shell
apidoc -i myapp/
```
* -o | --output: 输出目录
```shell
apidoc -o apidoc/
```
* -t | --template: 使用模板输出文件。您可以创建和使用自己的模板
```shell
apidoc -t mytemplate/
```

# grunt 模块

支持grunt模块, 参考[github.com/apidoc/grunt-apidoc](github.com/apidoc/grunt-apidoc)

```shell
npm install grunt-apidoc --save-dev
```

# 模板

apidoc默认模板使用handlebars, Bootstrap, RequireJS and jQuery生成文档.

默认模板支持:

* 版本控制: 查看API的不同版本
* 比较: 两个版本API之间的变化

[访问默认模板](https://apidocjs.com/example/)

## 自定义模板

您可以用apiDoc生成的文件创建自己的模板(api_data.js,api_project.js), 或使用json配置文件(api_data.json,api_project.json).

参考[https://github.com/apidoc/apidoc/tree/master/template](https://github.com/apidoc/apidoc/tree/master/template)

# 扩展

参考 [https://github.com/apidoc/apidoc-core](https://github.com/apidoc/apidoc-core)
项目中的lib/parsers/、lib/workers/和lib/filters/.

# 配置 apidoc.json

参考 [https://github.com/apidoc/apidoc/tree/master/example](https://github.com/apidoc/apidoc/tree/master/example)

```json
{
  "name": "AcmeCorp Api documentation",
  "version": "0.3.0",
  "description": "Documentation for the REST api access provided at AcmeCorp",
  "title": "Custom apiDoc browser title",
  "url" : "https://api.example.com",
  "sampleUrl": "https://apidoc.free.beeceptor.com",
  "output": "/tmp/apidoc-output",
  "input": [
    "example"
  ],
  "order": [
    "User",
    "PostUser",
    "GetUser",
    "City",
    "Category (official)"
  ],
  "header": {
    "title": "Introduction",
    "filename": "header.md"
  },
  "footer": {
    "title": "Best practices",
    "filename": "footer.md"
  },
  "template": {
    "showRequiredLabels": false,
    "withCompare": true,
    "withGenerator": true,
    "aloneDisplay": false
  }
}
```

# 配置参数

## @api {method} path title

请求方法

* method: 请求方法
* path: 请求路径
* title: 标题说明

```js
/**
 * @api {get} /user/:id Get user info
 */
```

## @apiBody [{type}] [field=defaultValue] [description]

请求体

* {type}: 参数类型, Boolean,Number,String,Object,String[]...
* {type{size}}: 参数长度
    * {string{..5}}: 最大长度5
    * {string{2..5}}: 最小长度2, 最大长度5
    * {number{100-999}}: 数值在100和999之间
* {type=allowedValues}: 参数允许的值
    * {string="small"}: 只能包含'small'
    * {string="small","huge"}: 只能包含'small' 或 'huge'
    * {number=1,2,3,99}: 只接受1,2,3,99几个数值
    * {string {..5}="small","huge"}: 字符长度最大5, 只能包含 'small' 或 'huge'
* field: 字段名
* `[field]`: 可选字段
* field`[nestedField]`: 嵌套字段
* field=defaultValue: 字段默认值
* description: field详细描述

```js
/**
 * @api {post} /user/
 * @apiBody {String} [firstname]       Optional Firstname of the User.
 * @apiBody {String} lastname          Mandatory Lastname.
 * @apiBody {String} country="DE"      Mandatory with default value "DE".
 * @apiBody {Number} [age=18]          Optional Age with default 18.
 *
 * @apiBody (Login) {String} pass      Only logged in users can post this.
 *                                     In generated documentation a separate
 *                                     "Login" Block will be generated.
 *
 * @apiBody {Object} [address]         Optional nested address object.
 * @apiBody {String} [address[street]] Optional street and number.
 * @apiBody {String} [address[zip]]    Optional zip code.
 * @apiBody {String} [address[city]]   Optional city.
 */
```

## @apiDefine name [title] [description]

定义文档块

* name: 文档块唯一名称, 可以通过@apiUse,@apiPermission引用
* title: 短标题,可以通过@apiPermission,@apiParam (name)引用
* description: 详细描述, 另起一行输入,可输入多行, 可通过@apiPermission引用

```js
/**
 * @apiDefine MyError
 * @apiError UserNotFound The <code>id</code> of the User was not found.
 */

/**
 * @api {get} /user/:id
 * @apiUse MyError
 */


/**
 * @apiDefine admin User access only
 * This optional description belong to to the group admin.
 */

/**
 * @api {get} /user/:id
 * @apiPermission admin
 */
```

[更多详情信息](https://apidocjs.com/#example-inherit)

## @apiDeprecated [text]

将API方法标记为已弃用

```js
/**
 * @apiDeprecated
 */

/**
 * @apiDeprecated use now (#Group:Name).
 *
 * Example: to set a link to the GetDetails method of your group User
 * write (#User:GetDetails)
 */
```

## @apiDescription text

对api详细描述

```js
/**
 * @apiDescription This is the Description.
 * It is multiline capable.
 *
 * Last line of Description.
 */
```

## @apiError [(group)] [{type}] field [description]

参数错误提示

* (group): 所有参数都将按此名称分组,如果没有组，则设置默认值,可设置一个 @apiDefine 定义的title
* {type}: Boolean,Number,String,Object,String[]...
* field: 错误标识符,错误码
* description: field的详细描述

```js
/**
 * @api {get} /user/:id
 * @apiError UserNotFound The <code>id</code> of the User was not found.
 */
```

## @apiErrorExample [{type}] [title] example

错误返回消息的示例，输出为预格式化的代码

* type: 响应格式
* title: 短标题
* example: 示例详情, 可多行输入

```js
/**
 * @api {get} /user/:id
 * @apiErrorExample {json} Error-Response:
 *     HTTP/1.1 404 Not Found
 *     {
 *       "error": "UserNotFound"
 *     }
 */
```

## @apiExample [{type}] title example

api使用示例

* type: 编程语言, 因为apidoc支持多种语言
* title: 短标题
* example: 示例详情, 可多行输入

```js
/**
 * @api {get} /user/:id
 * @apiExample {curl} Example usage:
 *     curl -i http://localhost/user/4711
 */
```

## @apiGroup name

api分组

```js
/**
 * @api {get} /user/:id
 * @apiGroup User
 */
```

## @apiHeader [(group)] [{type}] [field=defaultValue] [description]

描述传递给你API-Header的参数，例如授权,类似于@apiParam的操作，位置在参数之上

* (group): 所有参数都将按此名称分组,如果没有组，则设置默认值,可设置一个 @apiDefine 定义的title
* {type}: Boolean,Number,String,Object,String[]...
* field: 变量
* `[field]`: 可选变量
* field=defaultValue:  变量默认值 
* description: field的详细描述

```js
/**
 * @api {get} /user/:id
 * @apiHeader {String} access-key Users unique access-key.
 */
```

## @apiHeaderExample [{type}] [title] example

apiHeader使用示例

* type: 请求格式
* title: 短标题
* example: 示例详情, 可多行输入

```js
/**
 * @api {get} /user/:id
 * @apiHeaderExample {json} Header-Example:
 *     {
 *       "Accept-Encoding": "Accept-Encoding: gzip, deflate"
 *     }
 */
```

## @apiIgnore [hint]

带有的块将不会被解析。如果您在源代码中留下过时或未完成的方法，并且您不想将其发布到文档中，那么它是有用的, 它位置在注释最上面

```js
/**
 * @apiIgnore Not finished Method
 * @api {get} /user/:id
 */
```

## @apiName name

定义方法文档块的名称,Sub-Navigation会用到

```js
/**
 * @api {get} /user/:id
 * @apiName GetUser
 */
```

## @apiParam [(group)] [{type}] [field=defaultValue] [description]

请求参数

* (group): 所有参数都将按此名称分组,如果没有组，则设置默认值,可设置一个 @apiDefine 定义的title
* {type}: 参数类型, Boolean,Number,String,Object,String[]...
* {type{size}}: 参数长度
    * {string{..5}}: 最大长度5
    * {string{2..5}}: 最小长度2, 最大长度5
    * {number{100-999}}: 数值在100和999之间
* {type=allowedValues}: 参数允许的值
    * {string="small"}: 只能包含'small'
    * {string="small","huge"}: 只能包含'small' 或 'huge'
    * {number=1,2,3,99}: 只接受1,2,3,99几个数值
    * {string {..5}="small","huge"}: 字符长度最大5, 只能包含 'small' 或 'huge'
* field: 字段名
* `[field]`: 可选字段
* field`[nestedField]`: 嵌套字段
* field=defaultValue: 字段默认值
* description: field详细描述

```js
/**
 * @api {get} /user/:id
 * @apiParam {Number} id Users unique ID.
 */

/**
 * @api {post} /user/
 * @apiParam {String} [firstname]       Optional Firstname of the User.
 * @apiParam {String} lastname          Mandatory Lastname.
 * @apiParam {String} country="DE"      Mandatory with default value "DE".
 * @apiParam {Number} [age=18]          Optional Age with default 18.
 *
 * @apiParam (Login) {String} pass      Only logged in users can post this.
 *                                      In generated documentation a separate
 *                                      "Login" Block will be generated.
 *
 * @apiParam {Object} [address]         Optional nested address object.
 * @apiParam {String} [address[street]] Optional street and number.
 * @apiParam {String} [address[zip]]    Optional zip code.
 * @apiParam {String} [address[city]]   Optional city.
 */
```

## @apiParamExample [{type}] [title] example

请求参数示例

* type: 请求格式
* title: 短标题
* example: 示例详情, 可多行输入

```js
/**
 * @api {get} /user/:id
 * @apiParamExample {json} Request-Example:
 *     {
 *       "id": 4711
 *     }
 */
```

## @apiPermission name

输出权限名称, 如果使用@apiDefine定义的名称, 则包含title和description

```js
/**
 * @api {get} /user/:id
 * @apiPermission none
 */
```

## @apiPrivate

将API定义为私有的，以允许创建两个API规范文档:一个排除私有API，另一个包含私有API。

命令行使用`——private false|true`来排除/包含私有api

```js
/**
 * @api {get} /user/:id
 * @apiPrivate
 */
```

## @apiQuery [{type}] [field=defaultValue] [description]

查询参数

* {type}: 参数类型, Boolean,Number,String,Object,String[]...
* {type{size}}: 参数长度
	* {string{..5}}: 最大长度5
	* {string{2..5}}: 最小长度2, 最大长度5
	* {number{100-999}}: 数值在100和999之间
* {type=allowedValues}: 参数允许的值
	* {string="small"}: 只能包含'small'
	* {string="small","huge"}: 只能包含'small' 或 'huge'
	* {number=1,2,3,99}: 只接受1,2,3,99几个数值
	* {string {..5}="small","huge"}: 字符长度最大5, 只能包含 'small' 或 'huge'
* field: 字段名
* `[field]`: 可选字段
* field`[nestedField]`: 嵌套字段
* field=defaultValue: 字段默认值
* description: field详细描述

```js
/**
* @api {get} /user/:id
* @apiQuery admin
*/
```

## @apiSampleRequest url

API请求路径前缀, 配合 apidoc.json 中的 sampleUrl 使用

```js
// Configuration parameter sampleUrl: "http://api.github.com"

//实际访问http://api.github.com/user/:id
/**
 * @api {get} /user/:id
 */

// 实际访问http://test.github.com/some_path/user/:id
/**
 * @api {get} /user/:id
 * @apiSampleRequest http://test.github.com/some_path/
 */

// 实际访问http://api.github.com/test/user/:id
/**
 * @api {get} /user/:id
 * @apiSampleRequest /test
 */

// 禁止访问该api
/**
 * @api {get} /user/:id
 * @apiSampleRequest off
 */

// Configuration parameter sampleUrl is not set
// 实际访问http://api.github.com/some_path/user/:id
/**
 * @api {get} /user/:id
 * @apiSampleRequest http://api.github.com/some_path/
 */
```

## @apiSuccess [(group)] [{type}] field [description]

请求成功

* (group): 所有参数都将按此名称分组,如果没有组，则设置默认值,可设置一个 @apiDefine 定义的title
* {type}: 参数类型, Boolean,Number,String,Object,String[]...
* field: 字段名
* description: field详细描述

```js
/**
 * @api {get} /user/:id
 * @apiSuccess {String} firstname Firstname of the User.
 * @apiSuccess {String} lastname  Lastname of the User.
 */

/**
 * @api {get} /user/:id
 * @apiSuccess (200) {String} firstname Firstname of the User.
 * @apiSuccess (200) {String} lastname  Lastname of the User.
 */

/**
 * @api {get} /user/:id
 * @apiSuccess {Boolean} active        Specify if the account is active.
 * @apiSuccess {Object}  profile       User profile information.
 * @apiSuccess {Number}  profile.age   Users age.
 * @apiSuccess {String}  profile.image Avatar-Image.
 */

/**
 * @api {get} /users
 * @apiSuccess {Object[]} profiles       List of user profiles.
 * @apiSuccess {Number}   profiles.age   Users age.
 * @apiSuccess {String}   profiles.image Avatar-Image.
 */
```

## @apiSuccessExample [{type}] [title] example

请求成功示例

* type: 响应格式
* title: 短标题
* example: 示例详情, 可多行输入

```js
/**
 * @api {get} /user/:id
 * @apiSuccessExample {json} Success-Response:
 *     HTTP/1.1 200 OK
 *     {
 *       "firstname": "John",
 *       "lastname": "Doe"
 *     }
 */
```

## @apiUse name

引用一个定义的文档块

```js
/**
 * @apiDefine MySuccess
 * @apiSuccess {string} firstname The users firstname.
 * @apiSuccess {number} age The users age.
 */

/**
 * @api {get} /user/:id
 * @apiUse MySuccess
 */
```

## @apiVersion version

设置版本

```js
/**
 * @api {get} /user/:id
 * @apiVersion 1.6.2
 */
```
