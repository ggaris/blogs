### loader 解释器

```javascript
//webpack.config.js
module: {
    rules: [ //在这里面定义指定文件对应的解释器
      //规则定义
      {
        test: /\.js$/, //正则
        exclude: /node_modules/,
        use: {
          loader: "babel-loader", //根据 .babelrc 文件艰辛规则渲染
        },
      },
      {
        test: /\.(s[ac]|c)ss$/i, //正则
        use: ["style-loader", "css-loader", "sass-loader"],
      },
    ]
},
```

------

### css 文件抽离插件

```
yarn add -D mini-css-extract-plugin
```

```javascript
//webpack.config.js
const MiniCssExtractPlugin = require("mini-css-extract-plugin"); //引入插件
plugins: [new MiniCssExtractPlugin()], 加载插件
 module: {
    rules: [
      {
        test: /\.(s[ac]|c)ss$/i,
        use: [MiniCssExtractPlugin.loader, "css-loader", "sass-loader"], //应用插件
      },
    ],
},
```

------

### CSS 浏览器版本兼容插件

```
yarn add -D postcss postcss-loader postcss-preset-env
```

```javascript
//webpack.config.js
module: {
    rules: [
      //规则定义
      {
        test: /\.(s[ac]|c)ss$/i,
        use: [
          MiniCssExtractPlugin.loader,
          "css-loader",
          "sass-loader",
          "post-loader", //添加post-loader加载器
        ],
      },
    ],
  },
```

创建postcss配置文件 `postcss.config.js`

```javascript
//postcss.config.js
module.exports = {
  plugins: [require("postcss-preset-env")],
};
```

创建 `postcss-preset-env` 配置文件 `.browserslistrc`  

[说明链接]: https://www.jianshu.com/p/bd9cb7861b85

| **例子**        | **说明**                                              |
| --------------- | ----------------------------------------------------- |
| last 2 versions | 所有浏览器兼容到最后两个版本根据CanIUse.com追踪的版本 |
| \> 1%           | 全球超过1%人使用的浏览器                              |
| Firefox ESR     | 火狐最新版本                                          |
| Firefox > 20    | 指定浏览器的版本范围                                  |
| not ie <=8      | 方向排除部分版本                                      |

```
// 示例
last 2 versions // 回退两个浏览器版本
> 0.5% // 全球超过0.5%人使用的浏览器
IE 10 //兼容IE 10
```

