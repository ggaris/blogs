### 配置自动化项目

创建要给 `git` 项目对文件夹初始化，在 `.gitignore` 文件中配置忽略文件

```tex
node_modules
dist
yarn-error.log
yarn.lock
.vscode/settings.json
```

#### 首先最重要的肯定是要支持我们的 js 的babel

```
yarn add -D	babel-loader @babel/core @babel/preset-env
```

```javascript
// webpack.config.js
let mode = "development";

if (process.env.NODE_ENV === "production") {
  mode = "production";
}

module.exports = {
  mode: mode,
  module: {
    rules: [
      {
        test: /.js$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader",
        },
      },
    ],
  },

  devtool: "source-map",
  devServer: {
    contentBase: "./dist",
  },
};

```

```javascript
// babel.config.js
module.exports = {
  presets: ["@babel/preset-env"],
};
```

### 然后我们需要对  `CSS`  文件进行处理

```
yarn add -D mini-css-extract-plugin css-loader
```

```javascript
// webpack.config.js
const MiniCssExtractPlugin = require("mini-css-extract-plugin");

module.exports = {
  mode: mode,
  module: {
    rules: [
      {
        test: /\.css$/i,
        use: [MiniCssExtractPlugin.loader, "css-loader","sass-loader"],
      },
    ],
  },
  plugins: [new MiniCssExtractPlugin()],
};

```

### 添加对 `sass` 文件的loader

```
yarn add -D sass sass-loader
```

### 添加对现代 `css` 的loader支持,还会自动对浏览器进行兼容添加前缀等

```
yarn add -D postcss postcss-preset-env postcss-loader
```

创建 `postcss.config.js` 配置文件

```javascript
//postcss.config.js
module.exports = {
  pulgins: ["postcss-preset-env"],
};
```

### 好了现在到了一个重头戏那就是对浏览器的支持程度，决定了你的程序可以在那些浏览器达到你想要的效果

创建 `.browserslistrc` 文件，在这里面配置你需要支持的浏览器版本

```
last 2 versions
> 0.5%
IE 10 
```

### 对项目添加 `React`  babel

```
yarn add react react-dom
yarn add -D @babel/preset-react
```

进入`babel.config.js` 文件修改为以下内容

```javascript
module.exports = {
  presets: [
    "@babel/preset-env",
    ["@babel/preset-react", { runtime: "automatic" }],
  ],
};
```

进入 `webpack.config.js` 文件修改为以下内容

```javascript
const MiniCssExtractPlugin = require("mini-css-extract-plugin");

let mode = "development";
let target = "web";

if (process.env.NODE_ENV === "production") {
  mode = "production";
  target = "browserslist";
}

module.exports = {
  mode: mode,
  target: target,
  module: {
    rules: [
      {
        test: /\.jsx?$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader",
        },
      },
    ],
  },
  plugins: [new MiniCssExtractPlugin()],

  resolve: {
    extensions: [".js", ".jsx"],
  },
};
```

### 图片资源的babel

在 `webpack.config.js` 中的 `rules` 中添加

```javascript
{
  test: /\.(png|jpe?g|gif|svg)$/i,
  type: "asset",
  parser: {
    dataUrlCondition: {
      maxSize: 30 * 1024, //小于这个大小的图片将使用内敛的方式，不会打包出文件 默认是 8 * 1024
    },
  },
},
{
  test: /\.(s[ac]|c)ss$/i,
  use: [
    {
      loader: MiniCssExtractPlugin.loader,
      options: { publicPath: "" },
    },
      "css-loader",
      "postcss-loader",
      "sass-loader",
  ],
},
```

### 自动生成  `index.html` 文件

```
yarn add -D html-webpack-plugin
```

 ### 自动清理之前生产的文件夹

```
yarn add -D clean-webpack-plugin
```

```javascript
// webpack.config.js
const HtmlWebpackPlugin = require("html-webpack-plugin");
const { CleanWebpackPlugin } = require("clean-webpack-plugin");

output: {
    path: path.resolve(__dirname, "dist"),
    assetModuleFilename: "images/[hash][ext][query]",
},
plugins: [
    new CleanWebpackPlugin(),
    new MiniCssExtractPlugin(),
    new HtmlWebpackPlugin({
      template: "./src/index.html",
    }),
 ],
```

###  `React`  官方热更新插件

```
yarn add -D @pmmmwh/react-refresh-webpack-plugin react-refresh
```

然后在 `babel.config.js` 中配置插件

```javascript
module.exports = {
  presets: [
    "@babel/preset-env",
    ["@babel/preset-react", { runtime: "automatic" }],
  ],

  plugins: ["react-refresh/babel"],
};

```

在 `webpack` 中配置的选项

```
entry: "./src/index.js",
```

```javascript
entry: "./src/index.js", 
plugins: [
    new CleanWebpackPlugin(),
    new MiniCssExtractPlugin(),
    new HtmlWebpackPlugin({
      template: "./src/index.html",
    }),
    new ReactRefreshWebpackplugin(),
  ],
```

