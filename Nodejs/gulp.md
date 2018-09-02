# gulp

## 1、install gulp

- 在终端输入：

  ```
  npm install -g gulp
  ```

  ​

## 2、gulp API

- **gulp.src(globs[,options])**--->引入

  ```js
  gulp.src('.src/js/*.js')
      .pipe(jshint())
      .pipe(concat('merge.min.js'))
      .pipe(uglify())
      .pipe(gulp.dest('./dist/js'));
  ```

- **gulp.dest(path[,options])**--->输出

  ```js
  gulp.src('.src/*.html')
      .pipe(htmlmin())
      .pipe(gulp.dest('./dist'))
  ```

- **gulp.task(name  [, deps], \[function])**--->执行

  - name--->任务的名称，不能有空格
  - deps--->该项任务所依赖的模块
  - function--->该项任务的操作

  ```js
  gulp.task('sonmename',function(){
      //do stuff
  });

  gulp.task('matask',['array','of','task','names'],function(){
      //do stuff
  });
  //or the underneath
  gulp.task('build', ['array', 'of', 'task', 'names']);//这些任务不会一次并发执行

  gulp.task('build:css',function(){
      return gulp.src('./src/css/*.css')
                 .pipe(concat('merge.min.css'))
                 .pipe(cssnano())
                 .pipe(gulp.dest('./dist/css'));
  });
  ```

- **gulp.watch(glob[,opts],tasks)**--->监控

  ```js
  var wathcer = gulp.watch('js/**/*.js',['uglify','reload']);
  watcher.on('change',function(event){
      //do stuff
  });
  ```

  ​


## 3、install plugin of gulp

- plugin

  ```
  npm install --save gulp		       //本地使用gulp
  npm install --save-dev gulp-htmlmin    //压缩html
  npm install --save-dev gulp-cssnano    //压缩css
  npm install --save-dev gulp-less       //less
  npm install --save-dev gulp-sass       //sass
  npm install --save-dev gulp-rubby-sass //sass
  npm install --save-dev gulp-jshint     //js代码规范检查
  npm install --save-dev gulp-uglify     //压缩js
  npm install --save-dev gulp-imagemin   //压缩图片
  npm install --save-dev gulp-concat     //合并文件
  npm install --save-dev gulp-clean      //清空文件夹
  npm install --save-dev browser-sync    //文件修改浏览器自动刷新
  npm install --save-dev run-sequence    //task顺序执行
  npm install --save-dev gulp-rev        //添加版本号
  npm install --save-dev gulp-rev-replace//版本号替换
  npm install --save-dev gulp-useref      //解析html资源定位
  npm install --save-dev gulp-autoprefixer//给css自动添加用于不同浏览器上的前缀
  npm install --save-dev gulp-connect
  npm install --save-dev gulp-rename
  npm install --save-dev gulp-shell      //执行shell命令
  npm install --save-dev gulp-ssh        //操作远程机器
  ```

- gulp-useref

  ```js
  //gulp-useref 作用的一个例子
  //gulp-useref之前
  <html>
  <head>
      <!-- build:css css/combined.css -->
      <link href="css/one.css" rel="stylesheet">
      <link href="css/two.css" rel="stylesheet">
      <!-- endbuild -->
  </head>
  <body>
      <!-- build:js scripts/combined.js -->
      <script type="text/javascript" src="scripts/one.js"></script>
      <script type="text/javascript" src="scripts/two.js"></script>
      <!-- endbuild -->
  </body>
  </html>

  //gulp-useref之后
  <html>
  <head>
      <link rel="stylesheet" href="css/combined.css"/>
  </head>
  <body>
      <script src="scripts/combined.js"></script>
  </body>
  </html>
  ```

  ​

## 4、gulp简单范例

- demo目录结构

  ```
  +demo
      +dist
          +css
              -merge.min.css
          +imgs
              -1.png
              -2.png
          +js
              -merge.min.js
          -index.html
      +src
          +css
              -a.css
              -b.css
          +imgs
              -1.png
              -2.png
          +js
              -a.js
              -b.js
          -index.html
  ```

- **gulpfile.js配置**

  ```js
  var gulp = require('gulp');

  //引入组件
  var useref = require('gulp-useref'),
      cssnano = require('gulp-cssnano'),
      jshint = require('gulp-jshint'),
      uglify = require('gulp-uglify'),
      imagemin = require('gulp-imagemin'),
      rename = require('gulp-rename'),
      rev = require('gulp-rev'),
      revReplace = reuire('gulp-rev-replace'),
      concat = require('gulp-concat'),
      autoprefixer = require('gulp-autoprefixer'),
      clean = require('gulp-clean');
  gulp.task('dist:css',function(){
  	gulp.src('./dist/css/*').pipe(clean());
  	return gulp.src('./src/css/*.css')
  	           .pipe(concat('merge.min.css'))
  	           .pipe(cssnano())
  	           .pipe(autoprefixer({
  	           	    browsers: ['last 2 versions'],
  	           	    cascade: false
  	           }))
  	           .pipe(gulp.dest('./dist/css'));
  });
  gulp.task('dist:js',function(){
  	gulp.src('./dist/js/*').pipe(clean());
  	return gulp.src('./src/js/*.js')
  	           .pipe(jshint())
  	           .pipe(jshint.reporter('default'))
  	           .pipe(uglify())
  	           .pipe(rename('merge.min.js'))
  	           .pipe(gulp.dest('./dist/js'));
  });
  gulp.task('rev',['dist:css','dist:js'],function(){
  	return gulp.src(['./dist/**/*.css','./dist/**/*.js'])
  	           .pipe(rev())
  	           .pipe(gulp.dest('./dist'))
  	           .pipe(rev.manifest())
  	           .pipe(gulp.dest('./dist'));
  });
  gulp.task('index',['rev'],function(){
  	var manifest = gulp.src('./dist/rev-manifest.json');
  	return gulp.src('./src/index.html')
  	           .pipe(revReplace({
  	           		manifest: manifest
  	           }))
  	           .pipe(useref())
  	           .pipe(gulp.dest('./dist'));
  });
  gulp.task('dist:img',function(){
  	return gulp.src('./src/imgs/*')
  	 		   .pipe(imagemin())
  	 		   .pipe(gulp.dest('./dist/imgs'));
  });
  gulp.task('build',['dist:css','dist:js','rev','index','dist:img']); 
  ```

  ​

  ## 5、监控项目文件变动，自动刷新浏览器，本地调试， 并且把本地代码同步到远程服务器

  ```js
      var gulp = require('gulp');

      // 引入组件
      var browserSync = require('browser-sync').create();   //用于浏览器自动刷新
      var scp = require('gulp-scp2');                       //用于scp到远程机器
      var fs = require('fs');             


      gulp.task('reload', function(){
          browserSync.reload();
      });

      gulp.task('server', function() {
          browserSync.init({
              server: {
                  baseDir: "./src"
              }
          });

          gulp.watch(['**/*.css', '**/*.js', '**/*.html'], ['reload', 'scp']);
      });



      gulp.task('scp', function() {
          return gulp.src('src/**/*')
              .pipe(scp({
                  host: '121.40.201.213',
                  username: 'root',
                  privateKey: fs.readFileSync('/Users/wingo/.ssh/id_rsa'),
                  dest: '/var/www/fe.xxx.com',
                  watch: function(client) {
                      client.on('write', function(o) {
                          console.log('write %s', o.destination);
                      });
                  }
              }))
              .on('error', function(err) {
                  console.log(err);
              });
      });
  ```

- 执行：

  ```
      gulp scp;  // 可把本地开发环境代码拷贝到服务器
      gulp server; //可在本地创建服务器，本地开发浏览器立刻刷新
  ```

  ​

