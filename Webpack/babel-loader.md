### JS 浏览器兼容

```
yarn add babel-loader @babel/core @babel/preset-env
```

```javascript
module: {
    rules: [
      //规则定义
      {
        test: /\.js$/, //正则
        exclude: /node_modules/,
        use: {
          loader: "babel-loader", //如果有.babelrc 文件则参数 .babelrc 文件规则
        },
      }
    ],
  },
```

以上已经可以很好的对 IE11 以上的浏览器做出很好的兼容性了，如果你的产品需要兼容到IE 7 甚至更老的版本，那么请看下面的介绍

------

#### `core-js` 插件

使用   `core-js`  是对源文件大小影响比较小的一个方式，但是缺点是每个方法需要单独导入,当然如果你想你也可以直接通过  `import "core-js"; `  直接导入所有的方法，但是这会使你生产的文件异常臃肿。

```
yarn add core-js regenerator-runtime
```

```javascript
import "core-js/modules/es.object.values"; //引入该方法

const elvenShieldRecipe = {
  leatherStrips: 2,
  ironIngot: 1,
  refineMoonstone: 4,
};

const elvenGauntletsRecipe = {
  ...elvenShieldRecipe,
  leather: 1,
  refineMoonstone: 1,
};

console.log("ES7 -->", elvenGauntletsRecipe);

console.log("ES8 -->", Object.values(elvenGauntletsRecipe));
```

但是我可以通过设置  `.babelrc`  文件来配置我们的插件，使其方便快捷并且文件大小变化微小

```json
// .babelrc
{
  "presets": [
    ["@babel/preset-env",{ 
      "debug": true,  //在cmd窗口打印对应引用信息
      "useBuiltIns": "usage",  // 这里配置usage 会自动根据你使用的方法以及你配置的浏览器支持版本引入对于的方法。
      "corejs":3  // 指定 corejs 版本
      }
    ]
  ]
}
```

