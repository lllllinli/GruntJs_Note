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
    //這裡執行 grunt
     grunt.
};


```

