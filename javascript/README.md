# Javascript

* 1. [Scenario](#Scenario)
	* 1.1. [Change Text to Image](#ChangeTexttoImage)
	* 1.2. [Read local json file synchronously](#Readlocaljsonfilesynchronously)
	* 1.3. [Create loop animation](#Createloopanimation)
	* 1.4. [handle Response with  `content-type: application/octet-stream` <small>[React]</small>](#handleResponsewithcontent-type:applicationoctet-streamsmallReactsmall)

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

###  1.3. <a name='Createloopanimation'></a>Create loop animation

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

###  1.4. <a name='handleResponsewithcontent-type:applicationoctet-streamsmallReactsmall'></a>handle Response with  `content-type: application/octet-stream` <small>[React]</small>

Sometimes server will provide an interface with `application/octet-stream`, usually it's related with file operator, so how we handle this using js?

Actually it's very easy, make it as a download content will be okay.

e.g.

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

We can handle this response by following codes:

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

Then you'll see browser will call download action.