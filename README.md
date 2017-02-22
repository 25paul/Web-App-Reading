# HTML5 Web App 阅读器
学习慕课网的“webApp书城整站开发”课程<br/>

**项目用到的几个点：**

- Zepto基本使用
- Base64格式的图片
- HTML5的LocalStorange本地存储
- Ajax异步请求
- 使用jsonp跨域请求数据

**从设计图分析核心功能**

1. 从设计图分析核心功能<br/>
阅读器有：顶部导航、底部导航、内容展示区<br/>
顶部导航有返回键<br/>
底部导航有目录、唤出字体、夜间模式<br/>
内容展示区是内容的排版，把服务器传过来的数据做标签替换，还有翻页（翻页的目的是：1、希望一页放满内容，下一页进行数据加载；2、从性能方面考虑：一次无需加载过多）
2. 用到的技术：CSS3的一些样式、小图标用到base64图片、文本的排版会用JavaScript DOM节点操作的API、字号和背景保存用localStorage

## base64图片
为什么会有Base64编码呢？因为有些网络传送渠道并不支持所有的字节，例如传统的邮件只支持可见字符的传送，像ASCII码的控制字符就 不能通过邮件传送。这样用途就受到了很大的限制，比如图片二进制流的每个字节不可能全部是可见字符，所以就传送不了。最好的方法就是在不改变传统协议的情 况下，做一种扩展方案来支持二进制文件的传送。把不可打印的字符也能用可打印字符来表示，问题就解决了。Base64编码应运而生，Base64就是一种 基于64个可打印字符来表示二进制数据的表示方法。<br/>

Base64是一种基于64个可打印字符来表示二进制数据的表示方法。由于2的6次方等于64，所以每6个比特为一个单元，对应某个可打印字符。三个字节有24个比特，对应于4个Base64单元，即3个字节需要用4个可打印字符来表示。它可用来作为电子邮件的传输编码。在Base64中的可打印字符包括字母A-Z、a-z、数字0-9，这样共有62个字符，此外的两个可打印符号在不同的系统中而不同，一般为+和/。<br/>

http://web.jobbole.com/83294/<br/>
http://www.cnblogs.com/diligenceday/p/6002382.html<br/>

**优势**

 - 可以减少http请求。一般针对小图片，图片大的话可能会导致信息丢失<br/>
 平时在页面嵌入图片都是用url，base64不会用url地址，而是用一个字符串，这个字符串包括图片信息，相当于提取把图片信息放到css文件当中，这样急了减少请求。url(data:image/png;base64,{img_data})
 - 加快首屏数据的加载速度<br/>

**缺点**

- 体积会更大
- 不便于维护
 
base64格式图片生成：通过base64图片编辑器：http://imgbase64.duoshitong.com/

## 移动端性能陷阱和硬件加速

- 减少或避免repaint，reflow。<br/>
 重绘：比如颜色的变化<br/>
 回流：变化位置<br/>
- 减少对DOM的操作
- 尽量缓存所有可以缓存的数据<br/>
PC端时代用的比较多的还是http缓存，HTML5时代有localStorage（只能存字符串，遇到对象需要序列化stringify）、indexedDB。
- 使用CSS3 transform代替DOM操作

**一些非传统的方法：**

- 不要给非static定位元素增加CSS3动画
- 适当使用硬件加速<br/>
针对手机的GPU。比如用canvas画图片、使用transform

## 开发流程
- 交互解读：用户需求
- 设计解读
- 开发
- 测试

## 移动端开发技术选型的原则
- 轻量化
- 快速开发
- 维护简单（比如一个人请假了，另外一个人也可以完成他的工作）
- 高性能

## 代码的基本结构
![enter image description here](https://github.com/25paul/Web-App-Reading/blob/master/structure.png)

- reset.css：css初始化
- .json文件：模拟后台数据
- jquery.base64.js：编码与解码插件
- jquery.jsonp.js：做跨域处理的
- zepto.min.js：轻量级的JavaScript库，与jQuery类似
- index.html：主文件

## 源码
**项目的初始结构**
```
<!DOCTYPE html>
<html ng-app="app">
	<head>
		<meta charset="utf-8">
		<meta name="viewport" content="width=device-width,initial-scale=1,user-scalable=no,minimal-ui">
		<meta name="format-detection" content="telephone=no">
		<link rel="stylesheet" href="css/reset.css">
		<style type="text/css">
			
		</style>
	</head>
	<body>
		<div id="root" class="container">
			<div id="fiction_chapter_title"></div>
			<div id="fiction_container" class="m-read-content">
				<h4>传闻有误</h4>
				<p>允许异步读取页面的某些部分，允许其显示动态内容，根据时间和用户行为而有所不同。这是在 Ajax背后的技术。
				允许异步读取页面的某些部分，允许其显示动态内容，根据时间和用户行为而有所不同。这是在 Ajax背后的技术。
				允许异步读取页面的某些部分，允许其显示动态内容，根据时间和用户行为而有所不同。这是在 Ajax背后的技术。</p>
				<p>允许异步读取页面的某些部分，允许其显示动态内容，根据时间和用户行为而有所不同。这是在 Ajax背后的技术。</p>
				<p>允许异步读取页面的某些部分，允许其显示动态内容，根据时间和用户行为而有所不同。这是在 Ajax背后的技术。</p>
				<p>允许异步读取页面的某些部分，允许其显示动态内容，根据时间和用户行为而有所不同。这是在 Ajax背后的技术。</p>
			</div>
			<div class="m=button-bar">
				<ul class="u-tab">
					<li id="prev_button">上一章</li>
					<li id="next_button">下一章</li>
				</ul>
			</div>
		</div>
		<script src="lib/zepto.min.js"></script>
		<script>
			window.jQuery = $;
		</script>
		<script src="js/jquery.base64.js"></script>
		<script src="js/jquery.jsonp.js"></script>
		<script>
			  //...编写JavaScript交互代码
</script>
	</body>
</html>
```
在这基础上再继续添加上边栏、下边栏、中间点击层。<br/>

**交互部分**
```
(function () {
        var Util = (function () {
        })();
        function main() {
            //todo 入口函数
        }
        function ReaderModel() {
            //todo 实现和阅读器相关的数据交互的方法
        }
        function ReaderBaseFrame() {
            //todo 渲染基本的UI结构
        }
        function EventHanlder() {
            //todo 交互的事件绑定
        }
        main();//调用入口函数
    })()//闭包不影响全局,笔译避免变量被污染
```

**本地存储localStorage**
```
var prefix = 'fiction_reader_'    //给localStorage的key添加一个前缀，以免跟其他变量互相干扰
var StorageGetter = function (key) {
    return localStorage.getItem(prefix + key);
}
var StorageSetter = function (key, val) {
    return localStorage.setItem(prefix + key, val)
}
return {
      StorageGetter:StorageGetter,
      StorageSetter:StorageSetter
}
```

**跨域请求数据**
```
//数据解密
function getBSONP(url, callback) {
	return $.jsonp({
		url : url,
		cache : true,
		// 指定的值是duokan_fiction_chapter，服务端返回的json数据必须包含在duokan_fiction_chapter()里面。
		callback : "duokan_fiction_chapter",    //这个callback跟参数的callback不一样的
		success : function(result) {
			// debugger;
			var data = $.base64.decode(result);  
			var json = decodeURIComponent(escape(data));
			callback(json);
		}
	});

};
```

具体情况可查看代码。
