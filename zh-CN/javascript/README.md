# Javascript

* 1. [场景](#)
	* 1.1. [将文字变成转变成相应的图片](#-1)
	* 1.2. [同步读取本地Json文件](#Json)
	* 1.3. [设置循环动画](#-1)
	* 1.4. [处理 `content-type: application/octet-stream` 的 返回类型 <small>[React]</small>](#content-type:applicationoctet-streamsmallReactsmall)

##  1. <a name=''></a>场景

###  1.1. <a name='-1'></a>将文字变成转变成相应的图片

有些时候，我们需要将文字转化成图片，然后进行使用。而我推荐的方法是使用 [canvas - MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Canvas_API/Tutorial/Basic_usage) :

```javascript
var canvasElement = document.createElement('canvas');
canvasElement.width = 186;
canvasElement.height = 222;
var canvas = canvasElement.getContext('2d');
canvas.fillStyle = "#f6d021";
canvas.textAlign = "center";
canvas.textBaseline = "middle";
canvas.font = "bold 40px KaiTi, arial, helvetica, sans-serif";
canvas.fillText("This will be animage",0,0);
```

这个时候我们已经基于文字创建了一个 *canvas* 元素，如果需要按照作为 *\<img />* 元素的 *src* 使用，则需要将 *canvas* 转化为 *Base64 URL* ：

```html
<img src= canvas.canvas.toDataURL()  />
```

###  1.2. <a name='Json'></a>同步读取本地Json文件

在开发过程中，有些时候我们需要读取本地的 *Json* 文件， 但是如果使用 *Jquery* 或者其他方法时，一般提供的都是异步操作，因为文件读取操作就一般来说是个异步过程。当然如果可以的话， 我们直接使用 *NodeJS* 中的 *require('\*\*\*.json')* 也是可以的， 如果项目不是基于 *NodeJS* 开发的话，我们可以使用下面的方法实现：

> demo.html

```html
<script type="text/javascript" src=".../***.json"> </script>
```
> **\*\*\*.json**

```json
jsonContent = `[json content]`
```

> demo.js

```javascript
var readedJson = jsonContent;
```

###  1.3. <a name='-1'></a>设置循环动画

有时，我们需要设置一个循环播放的动画。而网上这方面的插件其实很多，但是如果我们想要使用原生的 *JavaScript* 来实现这个效果，也是可以的。

> *demo.html*

```html
...
<div class="animation">
	...
</div> 

<div class="start"></div> // 开始按钮
<div class="stop"></div> // 结束按钮
...
```

> *demo.js*

```javascript
var internal; // 定义的循环播放的动画

Jquery('.start').on('click', function() {
	var end_status = {opacity:'0.5'};  // 动画的结束效果
	internal = setInterval(function() {
		$('.animation').animate(css_chison_robot, 200, function() {
			if(end_status.opacity==='0.5') end_status.opacity = '1';  // 动画结束后回到初始状态
			else end_status.opacity='0.5';
		})
	}, 200);
})

Jquery('.stop').on('click', function() {
	clearInterval(internal);
	$('.animation').animate({
		opacity:'1' // 结束时回到初始状态
	});
}

```

###  1.4. <a name='content-type:applicationoctet-streamsmallReactsmall'></a>处理 `content-type: application/octet-stream` 的 返回类型 <small>[React]</small>

有时服务器提供的下载接口返回类型是 `application/octet-stream`, 这个是通用的二进制流响应，那么如何用 js 来处理这个请求呢？

其实很简单，我们只需要转换成下载内容就可以了。

比如 返回的头部信息是下面这种：

```json
cache-control: no-cache, no-store, max-age=0, must-revalidate
connection: close
content-disposition: attachment; filename=111.txt
content-type: application/octet-stream
Date: Mon, 28 Jan 2019 08:31:00 GMT
expires: 0
pragma: no-cache
referrer-policy: no-referrer
transfer-encoding: chunked
x-content-type-options: nosniff
x-frame-options: DENY
X-Powered-By: Express
x-xss-protection: 1 ; mode=block
```

那么在代码中只需要这样处理：

```javascript
function handleResponse = response => {
	response.blob().then(blob => {
		const link = document.createElement('a');
		const url = URL.createObjectURL(blob);
		console.log(url);
		link.href = url;
		link.download = '111.txt';
		link.click();
	})
}

```

这样就会自动调用浏览器的下载行为。