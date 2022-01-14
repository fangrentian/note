<!--
 * @author: ares
 * @date: 2022-01-14 10:45:12
 * @lastEditTime: 2022-01-14 10:46:24
 * @lastEditors: ares
 * @description: 
 * 
-->
## gulpfile配置

```js
const { task, series, parallel, watch, src, dest } = require("gulp");
const copy = require("gulp-copy"); //拷贝目录
const del = require('del');//删除文件
const uglify = require("gulp-uglify"); //js压缩
const babel = require("gulp-babel"); //js babel转换
const htmlmin = require("gulp-htmlmin"); //html压缩
const cleanCSS = require("gulp-clean-css"); //css压缩
const autoprefixer = require("gulp-autoprefixer"); //css autoprefixer
const notify = require('gulp-notify'); //提示
const concat = require('gulp-concat');   //合并文件
const rename = require('gulp-rename');   //文件重命名
const server = require('gulp-server-livereload');  //webServer

const rootDir = "yueyangyiyuan/";
const outputDir = "output/";
const ignoreTarget = [`${rootDir}src/**`, `${rootDir}plugins/**`, `${rootDir}legacy/**`, `${rootDir}therapist/**`];

/**
 * @author: ares
 * @date: 2021/4/9 15:22
 * @description: 清除输出目录
 * @param {function}cb
 */
const cleanOutputDir = (cb) => {
	const deletedPaths = del.sync(outputDir);
	console.log('Deleted files and directories:', deletedPaths.join('\n'));
	cb();
}

/**
 * @author: ares
 * @date: 2021/4/9 15:22
 * @description: 压缩html
 */
const htmlMin = () => {
	return src(`${rootDir}**/**.html`, { ignore: ignoreTarget })
		.pipe(htmlmin({
			collapseWhitespace: true, //从字面意思应该可以看出来，清除空格，压缩html，这一条比较重要，作用比较大，引起的改变压缩量也特别大。
			collapseBooleanAttributes: true, //省略布尔属性的值，比如：<input checked="checked"/>,那么设置这个属性后，就会变成 <input checked/>。
			removeComments: true, //清除html中注释的部分，我们应该减少html页面中的注释。
			removeEmptyAttributes: true, //清除所有的空属性。
			removeScriptTypeAttributes: true, //清除所有script标签中的type="text/javascript"属性。
			removeStyleLinkTypeAttributes: true, //清除所有Link标签上的type属性。
			minifyJS: true, //压缩html中的javascript代码。
			minifyCSS: true, //压缩html中的css代码。
		}))
		.pipe(dest(outputDir))
		.pipe(notify('HTML文件压缩完成——<%= file.relative %>'));
}

/**
 * @author: ares
 * @date: 2021/4/9 15:23
 * @description: css压缩
 */
const cssMin = () => {
	return src(`${rootDir}**/**.css`, { ignore: ignoreTarget })
		.pipe(autoprefixer())
		.pipe(cleanCSS({ compatibility: "ie10" }))
		.pipe(dest(outputDir))
		.pipe(notify('CSS文件压缩完成——<%= file.relative %>'));
}

/**
 * @author: ares
 * @date: 2021/4/9 15:30
 * @description: 合并css
 */
const concatCss = () => {
	return src(
		[
			`${rootDir}css/theme.css`,
			`${rootDir}css/header.css`,
			`${rootDir}css/header.css`,
			`${rootDir}css/menu.css`,
			`${rootDir}css/breadcrumb.css`,
			`${rootDir}css/bodytab.css`,
			`${rootDir}css/footer.css`,
			`${rootDir}css/app.css`
		]
	)
		.pipe(concat(`${rootDir}css/styles.css`))
		.pipe(dest('./'))
		.pipe(notify('CSS文件合并完成——<%= file.relative %>'));
}

/**
 * @author: ares
 * @date: 2021/4/9 15:23
 * @description: 压缩js
 */
const jsMin = () => {
	return src(`${rootDir}**/**.js`, { ignore: ignoreTarget })
		.pipe(
			babel({
				presets: ["@babel/env"],
			})
		)
		.pipe(uglify())
		.pipe(dest(outputDir))
		.pipe(notify('JS文件压缩完成——<%= file.relative %>'));
}

/**
 * @author: ares
 * @date: 2021/4/9 15:23
 * @description: 拷贝图片
 */
const copyImages = () => {
	return src(`${rootDir}images/**`)
		.pipe(copy(outputDir, { prefix: 1 }))
		.pipe(notify('图片文件拷贝完成——<%= file.relative %>'));
}

/**
 * @author: ares
 * @date: 2021/4/9 15:23
 * @description: 拷贝插件
 */
const copyPlugins = () => {
	return src(`${rootDir}plugins/**`)
		.pipe(copy(outputDir, { prefix: 1 }))
		.pipe(notify('插件文件拷贝完成——<%= file.relative %>'));
}

/**
 * @author: ares
 * @date: 2021/4/9 15:23
 * @description: 拷贝老文件
 */
const copyLegacy = () => {
	return src(`${rootDir}legacy/**`)
		.pipe(copy(outputDir, { prefix: 1 }))
		.pipe(notify('老文件拷贝完成——<%= file.relative %>'));
}

/**
 * @author: ares
 * @date: 2021/4/9 15:23
 * @description: 拷贝font-awesome
 */
const copyAwesome = () => {
	return src(`${rootDir}css/font-awesome-4.7.0/**`)
		.pipe(copy(outputDir, { prefix: 1 }))
		.pipe(notify('font-awesome拷贝完成——<%= file.relative %>'));
}

/**
 * @author: ares
 * @date: 2021/4/9 15:23
 * @description: 拷贝治疗师端文件
 */
const copyTherapist = () => {
	return src(`${rootDir}therapist/**`)
		.pipe(copy(outputDir, { prefix: 1 }))
		.pipe(notify('治疗师端文件拷贝完成——<%= file.relative %>'));
}

/**
 * @author: ares
 * @date: 2021/4/9 15:23
 * @description: 拷贝favicon.icon
 */
const copyFavicon = () => {
	return src(`${rootDir}favicon.ico`)
		.pipe(copy(outputDir, { prefix: 1 }))
		.pipe(notify('favicon.ico拷贝完成'));
}

/**
 * @author: ares
 * @date: 2021/4/10 9:18
 * @description: 启动web server
 */
const webServer = () => {
	return src(rootDir)
		.pipe(server({
			port: 8380,
			open: true,
			livereload: {
				port: 35730,
				enable: true
			}
		}));
}

exports.cleanOutputDir = cleanOutputDir;
exports.htmlMin = htmlMin;
exports.cssMin = cssMin;
exports.concatCss = concatCss;
exports.jsMin = jsMin;
exports.copyImages = copyImages;
exports.copyPlugins = copyPlugins;
exports.copyLegacy = copyLegacy;
exports.copyAwesome = copyAwesome;
exports.copyTherapist = copyTherapist;
exports.copyFavicon = copyFavicon;
exports.webServer = webServer;

exports.default = series(cleanOutputDir, concatCss, parallel(cssMin, jsMin, htmlMin, copyImages, copyPlugins, copyLegacy, copyAwesome, copyTherapist, copyFavicon));

```
