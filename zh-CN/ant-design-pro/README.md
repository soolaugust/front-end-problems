# Ant-Design-Pro

> *版本： 3.10.8*

* 1. [常见问题](#)
	* 1.1. [*@connect()* 引入的 *model* 是 *undefined* 的](#connectmodelundefined)

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