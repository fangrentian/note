<!--
 * @author: ares
 * @date: 2022-01-14 10:38:36
 * @lastEditTime: 2022-01-14 10:38:37
 * @lastEditors: ares
 * @description: 
 * 
-->
# package配置

```json
{
	"name": "ares_gulp",
	"version": "1.0.0",
	"main": "index.js",
	"license": "MIT",
	"scripts": {
		"mini": "node minifier.js",
		"clean": "gulp cleanOutputDir",
		"serve": "gulp webServer",
		"build": "gulp"
	},
	"dependencies": {
		"axios": "0.18.0",
		"clipboard": "2.0.4",
		"crypto": "^1.0.1",
		"dayjs": "^1.9.7",
		"jquery": "3.2.1",
		"js-cookie": "2.2.0",
		"lodash": "^4.17.20",
		"qs": "6.9.3",
		"store2": "2.10.0",
		"toast2": "2.1.1"
	},
	"devDependencies": {
		"@babel/core": "^7.12.10",
		"@babel/preset-env": "^7.12.11",
		"browser-sync": "^2.26.13",
		"clean-css": "^4.2.3",
		"del": "^6.0.0",
		"gulp": "^4.0.2",
		"gulp-autoprefixer": "^7.0.1",
		"gulp-babel": "^8.0.0",
		"gulp-clean": "^0.4.0",
		"gulp-clean-css": "^4.3.0",
		"gulp-concat": "^2.6.1",
		"gulp-copy": "^4.0.1",
		"gulp-htmlmin": "^5.0.1",
		"gulp-livereload": "^4.0.2",
		"gulp-mini-css": "^0.0.3",
		"gulp-notify": "^3.2.0",
		"gulp-rename": "^2.0.0",
		"gulp-requirejs-optimize": "^1.3.0",
		"gulp-server-livereload": "^1.9.2",
		"gulp-template": "^5.0.0",
		"gulp-uglify": "^3.0.2",
		"html-minifier": "^3.5.21",
		"readable-stream": "^3.6.0",
		"uglify-js": "^3.11.6"
	}
}
```
