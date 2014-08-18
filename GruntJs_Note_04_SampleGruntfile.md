Gruntfile Sample (範例)
======================

範例使用了五個 grunt 套件：

* [grunt-contrib-uglify](https://github.com/gruntjs/grunt-contrib-uglify) - 檔案壓縮工具。
* [grunt-contrib-qunit](https://github.com/gruntjs/grunt-contrib-qunit) - 單元測試。
* [grunt-contrib-concat](https://github.com/gruntjs/grunt-contrib-concat) - 檔案合併。
* [grunt-contrib-jshint](https://github.com/gruntjs/grunt-contrib-jshint) - javascript 語法檢查工具。
* [grunt-contrib-watch](https://github.com/gruntjs/grunt-contrib-watch) - 觀察檔案變更。

1.安裝 `Grunt` 套件：

使用 `--save-dev 參數` ，會順便更新 `package.json` 檔。 

```javascript

//依序執行

$sudo npm grunt-contrib-uglify --save-dev 

$sudo npm grunt-contrib-qunit --save-dev 

$sudo npm grunt-contrib-concat --save-dev
 
$sudo npm grunt-contrib-jshint --save-dev
 
$sudo npm grunt-contrib-watch --save-dev 


```

2. `Gruntfile` 設定：

(1).`wrapper` 開始：

```javascript

module.exports=function(grunt){
    //這裡執行 grunt 
    
};

```

(2). `grunt.initConfig()` 函式：

```javascript
module.exports=function(grunt){

    //這裡執行 grunt initConfig()
     
     grunt.initConfig=function(){
        
     };
};


```

(3). 載入 `package.json` 檔，並起合併到 `pkg` 屬性中。

```javascript
module.exports=function(grunt){

    grunt.initConfig({
        //將 package.json 屬性存入 pkg ，可以以 <%= pkg.屬性 %> 取得。
        pkg: grunt.file.readJSON('package.json')
    });

};

```

(4). 配置 Task - `concat` 、 `uglify` 、 `qunit` 、 `jshint` 、 `watch`。

```javascript

module.exports=function(grunt){
    grunt.initConfig({
        //將 package.json 屬性存入 pkg ，可以以 <%= pkg.屬性 %> 取得。
        pkg: grunt.file.readJSON('package.json'),
        //concat 套件串接檔案成一個檔。
        concat:{
            options:{
                // 設定檔案與檔案之間的串接方式，以 ';' 做分隔。
                separator:';',
                // 產生一組註解，在檔案的開始。
                banner:'/*! <%= pkg.name %> <%= grunt.template.today("dd-mm-yyyy") %>*/\n'
            },
            dist: {
                    //用于连接的文件
                    src: ['src/**/*.js'],
                    //返回的JS文件位置
                    dest: 'dist/<%= pkg.name %>.js'
                }
        },
        //ugLify 壓縮 js 檔工具。
        uglify: {
            options: {
                banner: '/*! <%= pkg.name %> <%= grunt.template.today("dd-mm-yyyy") %> */\n'
            },
            dist: {
                files: {
                    'dist/<%= pkg.name %>.min.js': ['<%= concat.dist.dest %>']
                }
            }
        },
         // qunit 測試 js 套件。
        qunit: {
            files: ['test/**/*.html']
        },
        jshint: {
            //定義要用檢測的文件
            files: ['gruntfile.js', 'src/**/*.js', 'test/**/*.js'],
            //配置JSHint (参考文件:http://www.jshint.com/docs)
            options: {
                //設定 jshint 預設設定
                globals: {
                    jQuery: true,
                    console: true,
                    module: true
                }
            }
        },
        //檢查 jshint.files 檔案發生變化，就執行 jshint 、quint Tasks
        watch: {
            files: ['<%= jshint.files %>'],
            //當檔案發現改變，執行 jshint、qunit
            tasks: ['jshint', 'qunit']
        }
    });
}


```

(5).讀取 grunt plugin 

```javascript

grunt.loadNpmTasks('grunt-contrib-uglify');
grunt.loadNpmTasks('grunt-contrib-jshint');
grunt.loadNpmTasks('grunt-contrib-qunit');
grunt.loadNpmTasks('grunt-contrib-watch');
grunt.loadNpmTasks('grunt-contrib-concat');

```

(6).設定任務 `Task` ，最重要的是 `default` 任務 `Task`。

##### 在命令列中直接執行， `$grunt` ，預設會執行的任務 `Task` 為， `default` Task 。##### 

```javascript

// this would be run by typing "grunt test" on the command line
// 這是其他自己設定的 Task
grunt.registerTask('test', ['jshint', 'qunit']);

// the default task can be run just by typing "grunt" on the command line
// 這是下 $grunt ，預設的 task
grunt.registerTask('default', ['jshint', 'qunit', 'concat', 'uglify']);

```




