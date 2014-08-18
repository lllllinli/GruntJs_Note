#GruntJs_Note_02_UseGruntInit#

####(1)安裝 grunt-init:####

1.安裝 grunt-init ：
`````
$sudo npm install grunt-init 

`````
2.安裝 templetes :

`````
// 比如安裝:jquery templete

$git clone https://github.com/gruntjs/grunt-init-jquery.git ~/.grunt-init/jquery

// templetes 會被建立在，目前使用者目錄/.grunt-init

$cd ~/.grunt-init

//即可看到 jquery 目錄


3.執行

`````
/*
* 1.列出所有的 templete
* 2.建立指定類型的專案
* 3.建立指定路徑樣板的專案
*/

$grunt-init --help
$grunt-init [templete]
$grunt-init /path/to/TEMPLATE


`````

4.其他別人寫好的 templetes<br>

[Link](http://gruntjs.com/project-scaffolding#installing-templates)<br>

5.客製自己的 Templete


##參考資料##
