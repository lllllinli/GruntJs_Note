Configuring tasks(任務配置)
==========================

##配置任務##

Grunt的任務，都被配置在 `Gruntfile` 中 `grunt.initConfig` 方法 (method)中。

```javascript

grunt.initConfig({
    concat: {
        // concat task configuration goes here.
        // “整合“ 任務配置放在這裡
    },
    uglify: {
        // uglify task configuration goes here.
        // “uglify“ 任務配置放在這裡
    },
    // Arbitrary non-task-specific properties.
    // 任意非特定任務的性質
    my_property: 'whatever',
    my_src_files: ['foo/*.js', 'bar/*.js'],
});

```

終端機執行 `concat` Task。


```
$grunt concat

```


##配置任務和目標##

已經學會配置Task，接下來是每個在 Task下 設定 Target。以下設定 ：<br>
`concat` 下有 `foo`、`bar` 兩個 Targets，`uglify` 下有 `bar`一個 Target。

```javascript

grunt.initConfig({
  concat: {
    foo: {
      // concat task "foo" target options and files go here.
    },
    bar: {
      // concat task "bar" target options and files go here.
    },
  },
  uglify: {
    bar: {
      // uglify task "bar" target options and files go here.
    },
  },
});

```

終端機執行 `concat` Task 的 `foo` Target。


```
$grunt concat:foo

```

##選項 Options ##

`options` 屬性也許是用來覆寫內建的預設規則。
你可以針對，`任務 Task` 、 `目標 Target` ，
分別設定不同的 `option` ，
設定規則：`目標 Target` > `任務 Task`。
另外，`option 選項`，可以不設。
<br>
```javascript
grunt.initConfig({
    concat: {
        options: {
            // 這是 Task 的 Options，覆盖 Task 的預設值.
        },
        foo: {
            options: {
                // 這是'foo' Target 的 options，它會覆蓋 Task 的options.
            }
        },
        bar: {
            // 没有指定options，這個 Target 將使用 Task 的 options.
        }
    }
});

```
##File檔案##

因為大部份的 `Task 任務` 都涉及操作檔案， Grunt 對於檔案的定義宣告與操作有非常強及高度的抽象化。它提供了一些方法定義 src-dest (來源-目的地)如何對應 ，就是取得來源檔案，然後處理後產生至目的地目錄。提供了不同程度的操作，任何任務都會針對某種格式的檔案來做操作，編譯等等。你可以選擇你需要的格式。
所有的檔案格式都支援 src 和 dest 兩種屬性設定，不過 `簡潔格式 Compact` 和 `檔案陣列 Files Array` 有一些額外的屬性可用。下面會說明什麼是 `簡潔格式 Compact` 和 `檔案陣列 Files Array`。額外支援的屬性如下：
<br>
+`filter` 任何一個 `fs.States` 的方法名稱如：
`stats.isFile()`你可以用'isFile'，
或者任何一個 function 輸入src` 路徑後可以判斷回傳 true/false ，
都可以拿來設定，用來過濾。

+`nonull` 當一個比對沒有被找到的時候，就會回傳一個包含其 pattern 的列表，否則就會回傳一個空的列表。
搭配 --verbose 可以用在關於檔案路徑的除錯。

+`dot` 允許檔案名稱的匹配模式用 . 開始 ，即使設定沒有在開頭使用 .
+`matchBase` 一旦設定這種匹配模式就不會比對 / 舉例來說會變成 *.js 等於 **/*.js 簡單來說就是只比對最後檔名的部分。
例如，a?b 會匹配 /xyz/123/acb 但不匹配 /xyz/acb/123。

+`expand` 處理動態的 src 和 dest 檔案對應。
其他屬性都會傳入底層當作匹配的參數




##簡潔格式 Compact Format##
這種格式允許每一個『目標』設定單一屬性的對應，比較常用在讀取檔案，
例如 [grunt-contrib-jshint](https://github.com/gruntjs/grunt-contrib-jshint) 只需要 src ，
而不需要 dest 。看下面範例會更清楚。

```javascript

grunt.initConfig({
    jshint: {
        foo: {
                src: ['src/aa.js', 'src/aaa.js']
        },
     },
    concat: {
        bar: {
        src: ['src/bb.js', 'src/bbb.js'],
          dest: 'dest/b.js',
       },
     },
    });

```
##檔案物件 Files Object Format ##
這種格式允許每個目標對應多個檔案，前面的屬性名稱會是目的地的檔案名稱。
然後來源檔案可以是多個。可以用這種方式設定多組檔案的對應。
一旦採用這種設定方式就不能再加入其他屬性了。看看下列程式碼：

```javascript
grunt.initConfig({
    concat: {
        foo: {
            files: {
                'dest/a.js': ['src/aa.js', 'src/aaa.js'],
                'dest/a1.js': ['src/aa1.js', 'src/aaa1.js'],
            }
        },
        bar: {
            files: {
                'dest/b.js': ['src/bb.js', 'src/bbb.js'],
                'dest/b1.js': ['src/bb1.js', 'src/bbb1.js'],
            }
        }
    }
});

```

##檔案陣列 Files Array Format ##

這種方式一樣支援多組 `src-dest` 對應，但可以多加入其他屬性。就跟一開始說的一樣 `簡潔格式` 和 `檔案陣列` 格式可以增加其他的屬性。
直接看範例比較清楚。

```javascript

grunt.initConfig({
    concat: {
        foo: {
            files: [
                {src: ['src/aa.js', 'src/aaa.js'], dest: 'dest/a.js'},
                {src: ['src/aa1.js', 'src/aaa1.js'], dest: 'dest/a1.js'},
            ],
        },
        bar: {
            files: [
                {src: ['src/bb.js', 'src/bbb.js'], dest: 'dest/b/', nonull: true},
                {src: ['src/bb1.js', 'src/bbb1.js'], dest: 'dest/b1/', filter: 'isFile'},
            ],
        },
    },
});

```
##舊版 Older Formats##

直接在 `Task任務` 底下，就是沒有 `files 屬性`是舊版本在執行`多檔案操作`的一種過渡期的方式。
直接讓最後目的地的路徑等於`目標(target) 名稱`，很不幸的因為目標名稱就是路徑，所以當你要執行 `grunt task:target` 的時候就顯得很糟糕。
而且你也不能在設定 `目標等級 的 options` 或`加入其他屬性`。
因此請儘量避免使用這種格式設定。

```javascript



```