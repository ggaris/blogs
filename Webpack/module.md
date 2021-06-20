# 一看就会系列——webpack 中的Module 配置

##### Q：Module 是什么？

A：首先我们从字面意思来解读以下 `Module` 就是模块，从宏观角度说，在 `Webpack` 中一切都是模块，相信都听过这句话，所以那么就很容易理解了， `Module` 就是文件呀！

##### Q：Module 只能通过 `webpack.config.js` 文件进行配置吗？

A：当然不是，只是说上面的那个方式是我们最常用也是官方最推荐的方式，配置 `loader` 一共有三种方式

- [配置](https://www.webpackjs.com/concepts/loaders/#configuration)（推荐）：在 **webpack.config.js** 文件中指定 loader。
- [内联](https://www.webpackjs.com/concepts/loaders/#inline)：在每个 `import` 语句中显式指定 loader。
- [CLI](https://www.webpackjs.com/concepts/loaders/#cli)：在 shell 命令中指定它们。

我们来看看三种方式的用法：

```javascript
//webpack.config.js
module: {
  rules: [
    {
      test: /\.css$/, //以 .js 结尾的文件。
      use: [
          	"style-loader",
            {loader:"css-loader",options:{modules:true}}
      ]
    }
  ]
}

// 内联
import Styles from 'style-loader!css-loader?modules!./styles.css';

//CLI
webpack --module-bind jade-loader --module-bind 'css=style-loader!css-loader'
```

上面三段代码的意义对于 `./styles.css` 来说是一样的配置，都是先通过 `css-loader` 进行解析，然后通过 `style-loader` 进行解析 ，而上面的 `options` 表示该 `loader` 可配置的设置。可以看到上面虽然 `webpack.config.js` 的方式代码量最多，但是在项目级别里面他是代码量最少的，可追溯性最强的一种方式，所以下面我们全都使用该方式进行举例。

##### Q：Module 配置可以做什么？

A：`Module` 中可以配置对指定的文件类型进行指定的 `Loader` 解析规则。

- 我们先来看看基本使用的示例：

```javascript
module: {
  rules: [
    {
      test: /\.js$/, //以 .js 结尾的文件。
      use: ["babel-loader"], //使用babel-loader 对以.js结尾的文件进行解析。
      include: path.resolve(__dirname, 'src') //通过include 规定只匹配src目录下的js文件。
    },
    {
      test: /\.css$/, //匹配以 .css结尾的文件
      use: ['style-loader', 'css-loader'], //可以看到 use 后跟的是一个数组，所很明显我们可以使用一组 loader 来对某一种文件进行解析，但是这里需要注意的是，解析顺序是从右到左，也就是：css文件 -> css-loader->style-loader，完成解析。
      exclude: path.resolve(__dirname, 'node_modules'), //通过exclude排除 node_modules 目录下的所有文件。
    },
    {
      test: /\.(gif|png|jpe?g|eot|woff|ttf|svg|pdf)$/, // 匹配非文本文件
      use: ['file-loader'], //使用 file-loader 来进行解析。
    },
  ]
}
```

<font color="red">上面，是对于 `rules` 基本配置，这里需要说一句的是 `include`  和 `exclude`  对于打包效率有很大的提升，一定要注意，因为我注意到很多地方忽略了这两个配置的配置，这是最简单也是最好提升你生产效率的方式。</font>

- 在 `use` 中我们除了使用一个字符串来设置 `loader`  此外还可以通过一个对象来表示对 `loader` 的配置，下面来看示例：

  ```javascript
  //方式1
  module: {
    rules: [
      {
        test: /\.css$/, //匹配以 .css结尾的文件
        use: ['style-loader', 'css-loader?modules'],
      },
    ]
  }
  
  // 方式2
  module: {
    rules: [
      {
        test: /\.css$/, //匹配以 .css结尾的文件
        use: [
            	{loader:'style-loader'}, 
              {loader:'css-loader?modules',opions:{modules:true}}
        ],
      },
    ]
  }
  ```

  在上面两种配置方式里面，表示的配置是完全一样的，通常我们在对基本使用的时候会使用字符串的方式，但是在需要进行较多自定义配置的时候，我们会通过对象的方式对配置项进行设置。

##### 总结一下：我们可以简单的将 Module 的配置理解成对于文件处理的方式配置。

-----------

好了，今天对于 webpack 中的 Module 配置的已经进行了简单的总结，希望对大家有帮助，持续关注，一起走进 webpack 的世界，我们一起真正掌握我们每天都在使用的工具。