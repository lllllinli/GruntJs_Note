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
+ `filter` 任何一個 `fs.States` 的方法名稱如：
`stats.isFile()`你可以用'isFile'，或者任何一個 function 輸入`src` 路徑後可以判斷回傳 true/false ，都可以拿來設定，用來過濾。
 + `nonull` 當一個比對沒有被找到的時候，就會回傳一個包含其 pattern 的列表，否則就會回傳一個空的列表。
搭配 --verbose 可以用在關於檔案路徑的除錯。
 + `dot` 允許檔案名稱的匹配模式用 . 開始 ，即使設定沒有在開頭使用 .
 + `matchBase` 一旦設定這種匹配模式就不會比對 / 舉例來說會變成 *.js 等於 **/*.js 簡單來說就是只比對最後檔名的部分。
例如，a?b 會匹配 /xyz/123/acb 但不匹配 /xyz/acb/123。
 + `expand` 處理動態的 src 和 dest 檔案對應。
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
grunt.initConfig({
    concat: {
        'dest/a.js': ['src/aa.js', 'src/aaa.js'],
        'dest/b.js': ['src/bb.js', 'src/bbb.js'],
    },
});

```
##客製過濾函式 Custom Filter Function##

`filter` 這個屬性可以讓你針對檔案設定更多細節，最簡單的方式就是使用 `fs.States` 的 method 名稱來設定，
例如下面的程式碼我們就可以輕鬆的確認是否為一個實體檔案，然後把 `tmp` 暫存的檔案都清光。

```javascript

grunt.initConfig({
    clean: {
        foo: {
            src: ['temp/**/*'],
            filter: 'isFile'
        }
    }
});

```

或者建立你自己的 `filter函式` 根據文件是否匹配來返回 true 或者 false。

```javascript

grunt.initConfig({
    clean: {
        foo: {
            src: ['tmp/**/*'],
            filter: function(filepath) {
                return (grunt.file.isDir(filepath) && require('fs').readdirSync(filepath).length === 0);
            },
        },
    },
});

```
##模式匹配與符號說明(Global Pattern)##

一般來說一個路徑一個路徑設定在實務上來說是不切實際的，非常麻煩。
所以 Grunt 支援透過內建的函式庫來做檔名路徑的匹配，
或者說透過設定 Pattern 規則來選取檔案。
這並不是專門的模式匹配的教學，我們只需要學習常用的符號即可。

+ `*` 任意數量的字元，但不包含 `/`。
+ `?` 單一字元，不包含 `/`。
+ `**` 任何數量的字元，包含 `/` 只要是路徑的一部分就可以。
+ `{}` 用 , 分隔列出清單，只要其中一個符合即可(OR)。
+ `!` 用在開頭，排除的意思。

大部份的人都知道 `/foo/*.js` 會選取到所有在 `/foo` 目錄底下的 `js` 檔案。
但是 `foo/**/*.js` 則會包含任何在 `/foo` 目錄底下子目錄的 `js` 檔案。
要注意 `/foo/*.js` 只有一層，就是在 `/foo` 底下，至於其它如 `/foo/subfolider` 就沒有。
此外，為了簡化複雜的匹配， Grunt 也允許你使用檔案陣列的方式如 `['a.js', '/foo/b.js']` ，
模式匹配是按照順序的，`!` 會排除匹配的檔案，其結果是唯一的。

```javascript
// 指定單一檔案
{src: 'foo/this.js', dest: …}
// 使用檔案陣列指定多個檔案
{src: ['foo/this.js', 'foo/that.js', 'foo/the-other.js'], dest: …}
// 使用模式匹配來選取檔案
{src: 'foo/th*.js', dest: …}

// node-glob 模式，對應 foo 目錄底下 a 或 b 開頭的 js
{src: 'foo/{a,b}*.js', dest: …}
// 上面的例子也可以這樣寫。
{src: ['foo/a*.js', 'foo/b*.js'], dest: …}

// foo 目錄下所有的 .js ，但不包含子目錄下的
{src: ['foo/*.js'], dest: …}

{src: ['foo/bar.js', 'foo/*.js'], dest: …}

// 除了 bar.js 以外所有的 .js ，會按照字母排列。
{src: ['foo/*.js', '!foo/bar.js'], dest: …}
// 所有的 .js 檔案， bar.js 先被排除後，又被加入了
{src: ['foo/*.js', '!foo/bar.js', 'foo/bar.js'], dest: …}

// 模版也可以用在檔案路徑或模式匹配中。
{src: ['src/<%= basename %>.js'], dest: 'build/<%= basename %>.min.js'}
// 也可以使用其他設定檔中配置的屬性值。
{src: ['foo/*.js', '<%= jshint.all.src %>'], dest: …}

```
##動態建立檔案物件##

當你需要處理很多個別的檔案的時候，有些額外的屬性可以協助你動態的建立檔案清單。
這些屬性都可以用在`簡潔格式 Compact`和`檔案陣列格式 File Array Format` 中。

+ `expand` 設成 `true` 用來啓用後面的屬性設定。
+ `cwd` (Current Working Directory) 設定一個目標路徑，注意這個不是檔案路徑或者 Pattern。
+ `src` 在根據上面的 cwd 做 Pattern 或檔案的選取。
+ `dest` 目的地目錄
+ `ext` 在 dest 目錄設定新的附檔名取代原始附檔名。
+ `flatten` 移除 dest 設定的路徑，設定值為 true/false
+ `rename` 對每個符合規則的 src 檔案調用這個 function （在執行完 ext 和 flatten 之後）。接著傳遞 dest 和 src 的值給 function 最後回傳一個新的 dest 路徑。

對每個匹配的src文件调用這個函数(在执行ext和flatten之后)。
傳遞 `dest`和 `匹配的src路徑` 给它，這個函数應該返回一个新的 `dest` 值。 如果相同的 `dest` 返回不止一次，每个使用它的 `src来源` 都將被添加到一個數字陣列中。如果傳回的 `dest` 重複則會被加入 sources 的陣列。

下面是關於 `minify` 的例子，我們會看到 s`tatic_mappings` 和 `dynamic_mappings` 目標雖然設定不同，
卻處理了相同的檔案列表， `static_mappings` 是一條一條設定，而 `dynamic_mappings` 則是動態，
先切換到一個路徑底下，再根據同樣的規則去執行。

```javascript

grunt.initConfig({
    minify: {
        static_mappings: {
            // 由於 src-dest 檔案路徑是寫死的, 每次新增或刪除，Gruntfile 都要更新。
            files: [
                {src: 'lib/a.js', dest: 'build/a.min.js'},
                {src: 'lib/b.js', dest: 'build/b.min.js'},
                {src: 'lib/subdir/c.js', dest: 'build/subdir/c.min.js'},
                {src: 'lib/subdir/d.js', dest: 'build/subdir/d.min.js'}
            ]
        },
        dynamic_mappings: {
            // 執行任務時 Grunt 會自動在當下的工作目錄 "lib/" 下搜尋 Pettern "**/*.js", 接著建立 src-dest 檔案對應，因此你就不需要在新增或刪除檔案時更新 Gruntfile。
            files: [
                {
                    expand: true,   // 啓用動態對應
                    cwd: 'lib/',    // 匹配切換成當前的工作目錄 lib
                    src: '**/*.js', // 根據上面的目錄限制，套用 Pettern
                    dest: 'build/', // 檔案產生目的地目錄
                    ext: '.min.js'  // 取代原本的附檔名
                }
            ]
        }
    }
});

```

##樣板（Templates）##

直接翻譯為樣板可能不太恰當，主要的用法是可以透過 `<% %>` 嵌入變數，而這些變數可以是來自讀取設定檔的資料。模板本身會不斷遞回讀取變數直到裡面不再存在任何模板。
整個設定物件中由外到內的屬性都會被解析，此外 grunt 的方法或屬性都可以被嵌入樣板中。例如 `<%= grunt.template.today('yyyy-mm-dd') %>`

+ `<%= prop.subprop %>` 取得 `prop.subprop` 設定屬性的值。
只要正規的型別他都能使用，不僅是字串，陣列貨，物件也可以。
這樣說明你可能會有點模糊，讓我們看下列的範例。

```javascript
module.exports = function(grunt){
    grunt.initConfig({
        prop: {subprop:"tmp/lib/"},
        clean: {
            src: "<%= prop.subprop %>*.js"
        }
    });
}

```

+ `<% %>` 在分隔符號中您可以使用 Javascript。對於做一些基本的邏輯判斷是非常有用的。

讓我們來一些範例觀察，下面使用了 `concat` 任務來整合檔案，執行 `grunt concat:sample` ，
透過 `banner` 會在檔案加入 `/* abcde */` ，而這隻檔案是由 `foo/*.js`, `bar/*.js`, `baz/*.js` 這三個規則搜尋來的檔案組合而成，
最後結合的檔案則生成 `build/abcde.js` 。

```javascript

grunt.initConfig({
    concat: {
        sample: {
            options: {
                banner: '/* <%= baz %> */\n' // '/* abcde */\n'
            },
            src: ['<%= qux %>', 'baz/*.js'], // [['foo/*js', 'bar/*.js'], 'baz/*.js']
            dest: 'build/<%= baz %>.js'
        }
    },
    // 同樣在 initConfig 中可以直接存取屬性。
    foo: 'c',
    bar: 'b<%= foo %>d', //'bcd'
    baz: 'a<%= bar %>e', //'abcde'
    qux: ['foo/*.js', 'bar/*.js']
});

```

##匯入外部資料##
在下面的 `Gruntfile` 中，專案本身的 `metadata` 可以透過 `package.json` 匯入到設定物件中。
接著 `uglify` 任務就可以透過 `pkg.name` 取得該專案名稱的 `js`。 `src/pkg.name.js` ，然後產生到 `dest` 中並加上 `.min.js`。

```javascript

grunt.initConfig({
    // 載入 package.json 指定屬性 pkg，利用 <%= pkg.屬性 %> 取得 package.json 內的值。
    pkg: grunt.file.readJSON('package.json'),
    uglify: {
        options: {
            banner: '/* <%= pkg.name %> <%= grunt.template.today("yyyy-mm-dd") %> */\n'
        },
        dist: {
            src: 'src/<%= pkg.name %>.js',
            dest: 'dist/<%= pkg.name %>.min.js'
        }
    }
});

```


##參考資料##
1.[http://gruntjs.com/configuring-tasks](http://gruntjs.com/configuring-tasks)
2.[http://www.gruntjs.org/docs/configuring-tasks.html](http://www.gruntjs.org/docs/configuring-tasks.html)
3.[http://andyyou.logdown.com/posts/142728-grunt-set-2](http://andyyou.logdown.com/posts/142728-grunt-set-2)