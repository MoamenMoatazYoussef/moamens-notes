Gulp:
=====
Gulp is a task runner, which basically means it automates tasks like compiling css, minifying, outputting, etc.

npm install gulp-cli
npm install gulp -D

- Make a folder
- cd the-folder
- npm init
- Put index.html in it.
- create some html.
- Put some sass files.
- create some sass.
- put the sass files in a folder called sass.
- npm install gulp-sass
- create a file in the root folder gulpfile.js
``` js
var gulp = require('gulp');
var sass = require('gulp-sass');

// this is a task, this will run and automate some stuff.
// 
gulp.task('sass', () => {
	// we want to compile the sass files into css, then minify it.
	// steps:
	// 1) get the sass files.
	// 2) compile them in css. (.sass())
	// 3) output the compiled css. (gulp.dest)
	return gulp.src("./sass/*.sass")
		.pipe(sass().on('error', sass.logError))
		.pipe(gulp.dest('./css'));
});
```
- go in cmd, write:
``` sh
>> gulp sass
```
- see the output :)

Now, we have a css output, we need to minify it.

- npm install gulp-uglifycss
- Go into gulpfile.js
``` js
// previous code

var uglifycss = require('gulp-uglifycss');

// steps:
// 1) get the css files that are the output of last task.
// 2) run uglifycss on them, give uglifycss some props to configure it.
// 3) output in dest/

gulp.task('css', () => {
	gulp.src("./css/*.css)
		.pipe(uglifycss({
			"uglyComments": true
		}))
		.pipe(gulp.dest("./dist/'));
});
```

- gulp sass
- gulp css

But, we need to automate that, instead of running the tasks manually.

- gulpfile.js:
``` js
// previous code

// this will run all tasks at once
gulp.task('run', ['sass', 'css']);

// this will watch files and auto-run the corresponding task
// which means we can watch different files and run different tasks automatically for each
gulp.task('watch', () => {
	// arguments are the files to watch
	gulp.watch('./sass/*.sass', ['sass']);
	gulp.watch('./css/*.css', ['css']);
});

// this default task runs when we type the command >> gulp
// without anything, like npm start keda
gulp.task('default', ['run', 'watch']);
```

As for basics, that's it :)
You can do a ton of automation things with gulp, check it out :)