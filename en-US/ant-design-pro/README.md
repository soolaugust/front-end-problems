# Ant-Design-Pro

> *Version： 3.10.8*

* 1. [Q & A](#QA)
	* 1.1. [**model** in **@connect()** is **undefined**](#modelinconnectisundefined)

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