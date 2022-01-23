# 作用

download-git-repo 是个npm包，用来从一个代码仓库中下载代码用的。

# 基本用法

`download(repository, destination, options, callback)`

## repository参数

支持四中写法

* GitHub 或 simply: `github:owner/nameowner/name`
* GitLab: `gitlab:owner/name`
* Bitbucket: `bitbucket:owner/name`
* Direct: `direct:url`

## destination参数

将仓库下载指定目录

## options参数

* clone: 布尔值
* 下载配置: [https://github.com/kevva/download#options](https://github.com/kevva/download#options)
* 复制配置: [https://github.com/jaz303/git-clone#clonerepo-targetpath-options-cb](https://github.com/jaz303/git-clone#clonerepo-targetpath-options-cb)

## callback回调函数

# 范例

## 使用http从Github仓库master下载

```js
download('flippidippi/download-git-repo-fixture', 'test/tmp', function (err) {
	console.log(err ? 'Error' : 'Success')
})
```

## 使用git从Bitbucket仓库的my-branch克隆

```js
download('bitbucket:flippidippi/download-git-repo-fixture#my-branch', 'test/tmp', { clone: true }, function (err) {
	console.log(err ? 'Error' : 'Success')
})
```

## 使用http配合自定义的源和token从GitLab仓库下载

```js
download('gitlab:mygitlab.com:flippidippi/download-git-repo-fixture#my-branch', 'test/tmp', { headers: { 'PRIVATE-TOKEN': '1234' } } function (err) {
	console.log(err ? 'Error' : 'Success')
})
```

## 使用git配合自定义的源和协议从GitLab仓库克隆

```js
download('https://mygitlab.com:flippidippi/download-git-repo-fixture#my-branch', 'test/tmp', { clone: true }, function (err) {
	console.log(err ? 'Error' : 'Success')
})
```

## 使用http从直连url下载

```js
download('direct:https://gitlab.com/flippidippi/download-git-repo-fixture/repository/archive.zip', 'test/tmp', function (err) {
	console.log(err ? 'Error' : 'Success')
})
```

使用git从直连url的master克隆
## 

```js
download('direct:https://gitlab.com/flippidippi/download-git-repo-fixture.git', 'test/tmp', { clone: true }, function (err) {
	console.log(err ? 'Error' : 'Success')
})
```

## 使用git从直连url的my-branch克隆

```js
download('direct:https://gitlab.com/flippidippi/download-git-repo-fixture.git#my-branch', 'test/tmp', { clone: true }, function (err) {
	console.log(err ? 'Error' : 'Success')
})
```