#GrountJs_Note_01_GettingStarted#

##Node.js版本##

`````
>= 0.8.0

`````

##安裝grunt-cli (Grunt 命令工作列)##

安裝：

`````
$sudo npm install -g grunt-cli

`````


##建立一個新的Grunt專案##

必須建立兩個檔案：
  
1.package.json。
2.Gruntfile.js。


###1.建立 package.json 檔###
建立檔案的目的，設定grunt相依套件。
<br>
加入套件：
<br>
`````
/*
* --save-dev 選項，會更新 package.json。
*/

$npm install <module> --save-dev

`````


###2.建立 Gruntfile 檔###

完成一個 Gruntfile 有四個部分：

+包裝函示（wrapper function）。
+配置計劃和任務(task)。
+載入plugin和task。
+客製化任務。





##參考資料##

1.gruntjs - 官方網站 <br>
[Link](http://gruntjs.com/)<br>

2.AndyYou's Blog - grunt-init 使用教學與 grunt-init-simple-server <br>
[Link](http://andyyou.logdown.com/posts/177346-grunt-init)<br>

gruntjs - 官方網站（簡體）<br>
[Link](http://www.gruntjs.org/docs/sample-gruntfile.html)<br>

