# Javascript

* 1. [Scenario](#Scenario)
	* 1.1. [Change Text to Image](#ChangeTexttoImage)
	* 1.2. [Read local json file synchronously](#Readlocaljsonfilesynchronously)
	* 1.3. [Create loop animation](#LoopAnimation)

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

### 1.3. <a name='LoopAnimation'></a>Create loop animation

Sometimes we need create a loop animation, of course, we could implement it with other plugins, we could also use oringinal *JavaScript* :

> *demo.html*

```html
...
<div class="animation">
	...
</div> 

<div class="start"></div> // start button
<div class="stop"></div> // end button
...
```

> *demo.js*

```javascript
var internal; // this is for stop animation manually

Jquery('.start').on('click', function() {
	var end_status = {opacity:'0.5'};  // end status of one animation
	internal = setInterval(function() {
		$('.animation').animate(css_chison_robot, 200, function() {
			if(end_status.opacity==='0.5') end_status.opacity = '1';  // after one animation end, back to start status 
			else end_status.opacity='0.5';
		})
	}, 200);
})

Jquery('.stop').on('click', function() {
	clearInterval(internal);
	$('.animation').animate({
		opacity:'1' // after manully stop, status should be reset
	});
}

```