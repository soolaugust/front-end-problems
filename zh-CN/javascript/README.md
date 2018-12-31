# Javascript

* 1. [场景](#)
	* 1.1. [将文字变成转变成相应的图片](#-1)
	* 1.2. [同步读取本地Json文件](#Json)

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

这个时候我们已经基于文字创建了一个 *canvas* 元素，如果需要按照作为 *<img>* 元素的 *src* 使用，则需要将 *canvas* 转化为 *Base64 URL* ：

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
var readedJson = jsonContent;1
```