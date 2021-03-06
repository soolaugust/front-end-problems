# Ant-Design-Pro

> *版本： 3.10.8*

* 1. [常见问题](#)
	* 1.1. [*@connect()* 引入的 *model* 是 *undefined* 的](#connectmodelundefined)
	* 1.2. [response命令台报错， 显示 'Unexpected token ... in Json ...'](#responseUnexpectedtoken...inJson...)
* 2. [自定义样式不起作用](#-1)

##  1. <a name=''></a>常见问题

###  1.1. <a name='connectmodelundefined'></a>*@connect()* 引入的 *model* 是 *undefined* 的

这个很有可能是因为 *model* 的配置不正确，由于 *ant-design* 使用 *umi-plugin-react*：

> **package.json**
```json
"devDependencies": {
    "umi-plugin-react": "^1.2.4"
}
```

而根据 [官方guide](https://umijs.org/zh/guide/with-dva.html#model-%E6%B3%A8%E5%86%8C)：

> *model 分两类，一是全局 model，二是页面 model。全局 model 存于 /src/models/ 目录，所有页面都可引用；页面 model 不能被其他页面所引用。*

所以很有可能是因为目前页面引用了其他页面的model

> **示例目录**

```tree
+ src
  + pages
    + a
      + models
        a.js
    + b
      b_page.js  // 在这里引用了a.js，导致connect引入的model是undefined
```

###  1.2. <a name='responseUnexpectedtoken...inJson...'></a>response命令台报错， 显示 `Unexpected token ... in Json ...`

如果直接查看response是正常的，那就需要查看response的返回类型，如果是其他类型，并且无法正常转换成json，则会报这个错误。

根本原因在 request.js 的逻辑中：

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

我们可以看到除了 `delete` 和 `204` 的情况，其他全部转换成json，所以需要在这段逻辑中加入自己的类型转换。

##  2. <a name='-1'></a>自定义样式不起作用

有些时候我们发现自己定义的样式没有起作用，原因很有可能是因为样式被 *ant-design* 默认的样式覆盖了：

> example.less

```css
.title {
  color: @heading-color;
  font-weight: 600;
  margin-bottom: 16px;
}
```

当你使用网页开发工具查时发现样式被别的样式覆盖了，例如 *ant-form*, 这时你可以修改成下面的样式：

>example.less

```css
.title:global(.ant-form) {
  color: @heading-color;
  font-weight: 600;
  margin-bottom: 16px;
}
```

*这是因为 **ant-design-pro** 使用 **CSS Modules**作为模块化方案, 这样你的自定义样式会被默认样式覆盖。