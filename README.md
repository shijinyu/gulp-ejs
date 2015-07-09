# gulp-ejs-pipe

> ejs plugin for [gulp](https://github.com/wearefractal/gulp)

与`gulp-ejs`有何不同？

`gulp-ejs-pipe` 从 `gulp-ejs` fork 出来，除了保留了后者的主要API，还增加了从 `file stream` 流对象读取参数的能力。这样做，是为了可能出现的这样一种需求：

参数需要动态地生成，或者需要之前的在管道(pipe)中被处理。

## Usage

安装 `gulp-ejs-pipe`:

```shell
npm install gulp-ejs-pipe
```

在 `gulpfile.js` 这样写，和 `gulp-ejs` 一样:

```javascript
var ejs = require("gulp-ejs");

gulp.src("./templates/*.ejs")
	.pipe(ejs({
		msg: "Hello Gulp!"
	}))
	.pipe(gulp.dest("./dist"));
```

或者这样（需要配合 gulp-gdo）：

将需要传送给ejs的数据赋给file的ejsrender属性：

```javascript
var gulp = require('gulp');
var gutil = require('gulp-util');
var ejs = require('gulp-ejs');
var gdo = require('gulp-gdo');
var cfg = require('./config.json');

gulp.task('compile',function() {

    gulp.src("./html/**/[^_]*.ejs")
        .pipe(gdo(function(file,path){
            var name = path.dirname + "\\" +path.basename;

            // 判断相应的key是否存在
            if(cfg.hasOwnProperty(name)){
				file.ejsrender = cfg[name];
            }
            
            console.log(file.ejsrender);

        }))
        .pipe(ejs())
        .on('error',gutil.log)
        .pipe(gulp.dest('./dest'));
});
```
其中```config.json```可能是这样的：

```json
{
	"static/html/template_1" : {
		"data_1":"Hello",
		"data_2":"World"
	},
	"static/html/template_2" : {
		"data_3":"Foo",
		"data_4":"Bar"
	}
}
```

```html
<!--static/html/template_1.ejs-->
<div><%= data_1 %>,<%= data_2 %></div>


<!--static/html/template_2.ejs-->
<div><%= data_3 %>,<%= data_4 %></div>
	
```

## License

[MIT License](http://en.wikipedia.org/wiki/MIT_License)
