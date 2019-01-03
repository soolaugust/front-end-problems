# Javascript

* 1. [Scenario](#Scenario)
	* 1.1. [Change Text to Image](#ChangeTexttoImage)
	* 1.2. [Read local json file synchronously](#Readlocaljsonfilesynchronously)

##  1. <a name='Scenario'></a>Scenario

###  1.1. <a name='ChangeTexttoImage'></a>Change Text to Image

Sometimes, we want to change a text an image, for best practice, I think better  way is  [canvas - MDN](https://developer.mozilla.org/kab/docs/Web/API/Canvas_API) :

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

Now we have created an *canvas* element，then we can change this element to *Base64 URL* then use it as *src* of *\<img />*：

```html
<img src= canvas.canvas.toDataURL()  />
```

###  1.2. <a name='Readlocaljsonfilesynchronously'></a>Read local json file synchronously

Sometimes we need to read json file synchronously, as we can use *require()* function in *NodeJS*, but if we are not developing project based on NodeJS, we could try following way: 

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