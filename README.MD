# Parcel Example - React Counter

使用 [Parcel](https://parceljs.org/) 打包的 React Counter 应用。
    中文官网:(http://www.css88.com/doc/parcel/)
    中文文档:(https://github.com/Mcbai/parcel-doc)

## 特别注意:
    node 8.X以下的版本是不支持异步函数的,要运行此demo需要升级到8.x的版本

安装
    npm install -g parcel-bundler
    
    npm 或 cnpm i

运行

    yarn start

或

    npm start

在浏览器中打开 <http://localhost:1234>

原理:
    parcel将一个入口点作为输入，可以是任何类型的：JS文件，HTML，CSS，图片等。在parcel中定义了各种资源类型，它们知道如何处理特定的资源类型。资源文件被解析，它的依赖关系被提取，并转换成最终的编译形式。这创建了一个资源树.

值得吹逼的地方:
    1.速度是 ParcelJS 主要卖点。体验下来确实快，原因是多核（通过 worker 平行构建）和文件系统缓存（二次构建会快，和 webpack 的 dll 异曲同工）。不过 webpack 也有多核处理 loader 和压缩的插件，没对比过，不知道差异如何。另外 webpack 的构建速度慢在 dev 模式下还是可以的，主要还是压缩慢，在压缩速度上没有突破，仅提升编译速度，只能解决一部分问题。

坑爹的地方:
    1.关于 0 配置。ParcelJS 本身是 0 配置的，但 HTML、JS 和 CSS 分别是通过 posthtml、babel 和 postcss 处理的，所以我们得分别配 .posthtmlrc、.babelrc 和 .postcssrc ;

    2.修改html文件，页面不自动更新
    这个不是什么bug，官网很明确的写了，热更新只针对css和JavaScript。

    3.修改css文件，页面不自动更新
    这个就是个问题了，在一个比webpack快10倍的打包工具这篇文章里也有提到，需要使用PostCSS，这个得后面试试。

    4.更换图片，内容不更新
    换图片不换图片名字的情况下，手动刷新页面，页面中的图片也不会更换

    5.MOZ_TO_ME[node.type] is not a function
    如果直接parcel build index.html就会报错：
    (https://github.com/parcel-bundler/parcel/issues/11#issuecomment-349491572)
    在回答里说跟Windows有关，不过我试了下，并不是Windows的问题，然后照着他的答案，加了后缀parcel build index.html --no-minify。
    加完就好用了，所以应该是uglify-js的原因，上网搜了下，uglify-js的确不支持直接压缩包含ES6语法的代码。然后我又安装了babel-preset-env，发现并没有像文档里说的那样直接就能编译

总结
    总体而言，对于parcel的初步尝试还是很满意的，就现在看来，如果就是单纯的压缩打包，并且JavaScript用的是ES6以下的语法，的确能做到零配置。后面还得加上PostCss和Babel再看看，但是按照官网的介绍也都很简单，全都集成好的，只是用法上还需要看看。
    比较适合用在平台一些边角料的项目上

打包过程会生产 .cache 和 dist 两个目录，如果是 git 工程，可以新建 .gitignore 文件忽略这两个目录：

```
.cache
dist
node_modules
```
