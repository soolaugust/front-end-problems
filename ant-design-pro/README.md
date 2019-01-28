# Ant-Design-Pro

> *Version： 3.10.8*

* 1. [Q & A](#QA)
	* 1.1. [**model** in **@connect()** is **undefined**](#modelinconnectisundefined)
	* 1.2. [console report error `Unexpected token ... in Json ...` with correct response.](#consolereporterrorUnexpectedtoken...inJson...withcorrectresponse.)
* 2. [custom styles doesn't work](#customstylesdoesntwork)

##  1. <a name='QA'></a>Q & A

###  1.1. <a name='modelinconnectisundefined'></a>**model** in **@connect()** is **undefined** 

The possible reason is that you use model defined in another page. As **ant-design-pro** use **umi-plugin-react**, so this would cause **undefined** problem.

> **package.json**
```json
"devDependencies": {
    "umi-plugin-react": "^1.2.4"
}
```

According to [guide](https://umijs.org/guide/with-dva.html#registering-models)：

> *There are two types of models: the globally registered (global) model, and the page-level model.*
>
> *The global model should be defined in /src/models/, and can be referenced in all other pages.*
>
> *The page-level model should not be used in any other page.*

So if you use model defined in another page, you will got **undefined** problem.

> **Example**

```tree
+ src
  + pages
    + a
      + models
        a.js
    + b
      b_page.js  // here you connect model defined in a.js, you will got undefined problem.
```

###  1.2. <a name='consolereporterrorUnexpectedtoken...inJson...withcorrectresponse.'></a>console report error `Unexpected token ... in Json ...` with correct response.

If you are ensure your response is correct, you may need to check the type of your response, console will report error when response type is not `json` and cannot convert response body to `json`.

This is because `request.js` handle response like following：

```javascript
return fetch(url, newOptions)
.then(checkStatus)
.then(response => cachedSave(response, hashcode))
.then(response => {
  // DELETE and 204 do not return data by default
  // using .json will report an error.
  if (newOptions.method === 'DELETE' || response.status === 204) {
    return response.text();
  }
  return response.json();
})
```

So if not `DELETE` method or status code is not `204`, all response will be converted to json. that's the reason console reporting error.

##  2. <a name='customstylesdoesntwork'></a>custom styles doesn't work

When we defined our custom styles, sometime we found they doesn't work. the possible reason maybe your styles are override by *ant design* default styles.

> example.less

```css
.title {
  color: @heading-color;
  font-weight: 600;
  margin-bottom: 16px;
}
```

But when you check styles by Web Developer tools, you may found attributes are override by other style, such as **ant-form**, you should change your styles like following:

>example.less

```css
.title:global(.ant-form) {
  color: @heading-color;
  font-weight: 600;
  margin-bottom: 16px;
}
```

*The reason is **ant-design-pro** using **CSS Modules** as a nodular solution, so your styles will be override by default styles.