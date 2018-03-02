# Wedge
可配置的小说下载工具
## 快速开始
```Javascript
#!/usr/bin/env node
const Wedge = require("./wedge");
const app = new Wedge("./library");
app.start()
```
## 使用教程
```Javascript
//引入wedge模块
const Wedge = require("./wedge");
//创建App，参数为工作路径
var App = new Wedge(workdir);
//新建书籍
App.newBook(url);
//更新书籍
App.updateBook(uuid);
//批量新建书籍
App.newBooks([url1,url2,url3,...]);
//批量更新书籍
App.updateBooks([uuid1,uuid2,uuid3,...]);
//终端提示输入界面
App.start()
```

### 配置参数[可选]
参数配置，初次运行时会在工作路径新建json格式的配置文件，可手动修改

#### 网络请求参数
```Javascript
//请求失败重连次数
App.config.set('request.reconnect',5);
//请求超时时间
App.config.set('request.timeout',5000);
//设置代理
App.config.set('request.proxy','127.0.0.1:8087');
App.config.set('request.proxy',{host:'localhost',port:'8087'});
//设置代理认证
App.config.set('request.proxyAuth',{username:'###',password:'###'});
```

#### 电子书参数
```Javascript
//生成电子书保存路径
App.config.set('ebook.directory','E:/MyBooks/Library/ebook');
//电子书格式，默认epub
/*常用电子书格式
txt,
fb2,
epub,
~~chm~~(作者比较懒，非window系统下要自己实现LZX压缩算法，不兼容*nix系统),
~~mobi~~(找不到电子书的结构定义，推荐用kindlegen或者calibre转换),
~~ebk3~~(掌阅的专用格式，正在破解)
umd,
docx,
odt,
rtf,
html,
htmlz,
json,
txt.zip(txt格式的压缩文件),
fb2.zip(fb2格式的压缩文件),
html.zip(分章节的html压缩文件),
wbk(作者自定义的电子书格式,高压缩率，主要用于导出备份书籍，可转换成其他书籍格式)
*/
App.config.set('ebook.formation','epub');
/*
是否创建电子书，子进程命令
-1——发生修改后自动创建新的电子书(包括书籍metadata变动或有下载新章节)
1——每次调用生成命令都会创建新的电子书
0——关闭该功能
*/
App.config.set('ebook.activated',-1);
//电子书生成后自动打开文件目录
App.config.set('ebook.opendirectory',false);
//电子书生成后自动打开文件
App.config.set('ebook.openebookfile',false);
```

#### 线程参数
```Javascript
//执行章节下载时的并行数目
App.config.set('thread.execute',5);
//批量新建小说时的并行数目
App.config.set('thread.new',5);
//批量更新小说时的并行数目
App.config.set('thread.update',5);
//合并目录时的并行数目
App.config.set('thread.merge',5);
//下载图片时的并行数目
App.config.set('thread.image',5);
```

#### 数据库参数
```Javascript
//实时同步本地
App.config.set('database.sync',true);
```

#### 书籍参数
```Javascript
//同步加载
App.config.set('book.sync',true);
//异步加载
App.config.set('book.sync',false);
//自动检查目录，添加已有的章节信息或移除已被删除的章节
App.config.set('book.check',true);
//如果存在相同小说时，是否切换新的的源
App.config.set('book.changesource',false);
//切换新的的源后是否覆盖旧章节
App.config.set('book.override',false);
//章节中发现图片链接是是否下载图片到本地
App.config.set('book.imagelocalization',false);
//是否从起点等原创网站搜索书籍信息
//建议开启，当下载网站不在规则库中能够辅助搜集书籍信息
App.config.set('book.searchmeta',true);
//深度下载，如果章节内容中存在超链接，将下载该链接内容
App.config.set("book.deepdownload",true);
//设置最大下载深度
App.config.set("book.maxdepth",2);
```

#### App参数
```Javascript
//输出运行日志
App.config.set('app.log',true);
//输出运行日志到本地文件
App.config.set('app.log','LOG.txt');
//输出debug信息
App.config.set('app.debug',true);
```

### plugins小插件
```Javascript
//启用或添加插件
//修改工作路径的setting.json文件后重启程序
{
  "name": "deepQuest",
  "activated": false,//未启动
  "func": "./plugins/deepQuest"//插件脚本路径
}
//将书籍文件发送到手机阅读器；参数不齐全时自动进入终端界面提示输入
//QQ阅读器（默认开启）
App.sendToQQ(ip,[files]);
App.sendToQQ();
//iReader阅读器（默认开启）
App.sendToiReader();
//搜索引擎（默认开启）
App.Engine.search(title,callback);
//设置搜索引擎;默认为sogou
App.Engine.setEngine("360");
App.Engine.setEngine("baidu");
//简单的控制台输入界面（默认开启）
App.start()
/*
  0) 返回
  1) 新建书籍
  2) 更新书籍
  3) 重新下载书籍
  4) 刷新书籍信息
  5) 生成电子书
  6) 导入书籍
  7) 导出书籍
  8) 删除书籍
  9) 删除书籍记录
  10) 导入书籍记录
  11) 修改书籍信息
  12) 批量操作
      0) 返回上一级菜单
      1) 新建书籍
      2) 更新书籍
      3) 重新下载书籍
      4) 刷新书籍信息
      5) 导出书籍
      6) 生成电子书
      7) 转换电子书
      8) 删除书籍
      9) 删除书籍记录
      10) 导入书籍记录
      11) 退出
  13) 数据库检索
      0) 返回上一级菜单
      1) 关键字检索
      2) query查询
      3) SQL查询
      4) 退出
          0) 返回上一级菜单
          1) 返回主菜单
          2) 显示表格
          3) 显示详情
          4) 过滤数据
          5) 导出数据
          6) 批量操作
              0) 返回上一级菜单
              1) 返回主菜单
              2) 更新书籍
              3) 刷新书籍信息
              4) 重新下载书籍
              5) 导出书籍
              6) 生成电子书
              7) 删除书籍
              8) 删除书籍记录
              9) 退出
          7) 退出
  14) 电子书格式转换
  15) 发送到手机[需在同一局域网下]
  16) 使用引擎搜索
  17) 修改配置
  18) 测试规则
  19) 退出
*/
```

### 添加网站规则
#### 临时添加
App.Sites.inject(rule);
#### 永久添加
在wedge/lib/sites路径下
defaut为原创小说网站，
plugins为盗链网站
![host]命名的为匹配多个网站的规则

规则文件由三部分组成：index.js, selector.js(json), replacer.json

####主文件（index.js）
```Javascript
module.exports = {
    "host":"www.23zw.com",//站点host
    "match":[//其他相同规则的站点host
        "www.23zw.com",
        "www.23zw.me"
    ],
    "charset":"gbk",//网页编码，不填的话程序会尝试自动解码
    "selector":require("./selector"),
    "replacer":require("./replacer")
}
```

####选择器
按照jquery函数填写规则，启动时自动编译成函数
添加一个$.location()函数，返回当前页面的location 或 resolve url地址
```Javascript
{
  "infoPage": {
    "match": "!!$('.btnlinks > a.read').length",//是否匹配书籍信息页面
    "indexPage": "$.location($('.btnlinks > a.read').attr('href'))",//书籍目录页的链接
    "footer": "$('.footer').length > 0",//页面footer,防止网络延迟下的页面内容缺失，可直接填true
    "bookInfos": {//采集书籍信息
      "title": "$('#content h1').text()",//书籍名称
      "author": "$('th:contains(作者)').next('td').text()",//书籍作者
      "classes": "$('th:contains(类别)').next('td').text()",//书籍分类
      "isend": "$('th:contains(状态)').next('td').text()",//书籍完结状态
      "cover": "$.location($('a.hst img').attr('src'))",//书籍封面链接
      "brief": " $('p:contains(内容简介)').nextAll('p').html()"//书籍简介
    }
  },
  "indexPage": {
    "match": "!!$('.tags').length",//是否匹配书籍目录页
    "infoPage": "$.location($('.bdsub > dl > dt > a').last().attr('href'))",//书籍信息页的链接
    "footer": "!!$('.tags').length",
    "filter": "$('td:contains(更新重要通告)').remove()",//过滤信息
    "bookIndexs": "$('td.L a').map((i,v)=>({href:$.location($(v).attr('href')),text:$(v).text()})).toArray()"//书籍章节链接，返回{href:,text:}格式的数组
  },
  "contentPage": {
    "match": "!!$('#footlink').length",//是否匹配章节页
    "footer": "!!$('#footlink').length",
    "chapterInfos": {
      "source": "$.location()",//页面地址
      "content": "$('#contents').html()"//页面内容
    }
  }
}
```

#### 替换器
结构同上，可省略
字符串会编译成正则表达式进行替换
{key:value}对象格式会将key编译成正则表达式并替换成value
```Javascript
{
  "infoPage": {
    "bookInfos": {
      "title": ["全文阅读"]
    }
  },
  "contentPage": {
    "chapterInfos": {
      "content": ["看最快更新","一秒记住.*?免费阅读！"]
    }
  }
}
```
#### 全局替换器
./lib/classes/Filter.json
```Javascript
{
    "chapterTitle":[],//全局章节标题过滤规则
    "chapterBeforeFilter":[],//内置过滤前的原始html文件的过滤规则
    "chapterAfterFilter":[
        "readx();",
        "天才壹秒记住.*为您提供精彩小说阅读。",
        "天才一秒记住.*为您提供精彩小说阅读。",
        "^<<",
        "手机用户请浏览.*",
        "公告：本站推荐一款免费小说APP.*"
    ]//内置过滤后进一步的过滤
}
```
