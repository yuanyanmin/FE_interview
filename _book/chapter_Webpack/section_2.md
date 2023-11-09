# 第 2 题：webpack哪些常用的配置

### 配置项

* entry：入口文件，可以是一个或多个 JavaScript 文件，通过这个文件，Webpack 将会处理整个应用。

* output：输出配置选项，用于告诉 Webpack 将打包后的文件输出到哪里。可以定义 output.path、output.filename 和 output.publicPath 等。

* module：模块配置选项，使用不同的 loader 可以让 Webpack 处理不同类型的文件。例如，对于 CSS 文件，可以使用 css-loader 和 style-loader 处理。

* resolve：解析配置选项，用于告诉 Webpack 该如何解析模块依赖。可以设置 resolve.alias、resolve.extensions、resolve.modules 等。

* plugins：插件配置选项，使用不同的插件可以增强 Webpack 的功能。例如，使用 html-webpack-plugin 可以将打包后的 js 文件自动引用到 HTML 文件中。

* devServer：开发服务器配置选项，提供了一个简单的 web 服务器和实时重载功能，可以通过 devServer.contentBase、devServer.port、devServer.proxy 等进行配置。

* optimization：优化配置选项，可以使用 optimization.splitChunks 和 optimization.runtimeChunk 配置代码拆分和运行时代码提取等优化策略。

* externals：外置扩展配置选项，用于配置排除打包的模块。例如，可以将 jQuery 作为外置扩展，避免将其打包到应用程序中。

### loader

* file-loader

* url-loader

* css-loader

* style-loader

* scss-loader

* less-loader

* postcss-loader

* eslint-loader


### plugins

* HtmlWebpackPlugin

* clear-webpack-plugin

* copy-webpack-plugin

* mini-css-extract-plugin

* terser-webpack-plugin：压缩代码

* optimize-css-assets-webpack-plugin：压缩css代码

* image-webpack-loader 或 img-loader：压缩图片

* postcss-sprities 活 webpack-spritesmith：合并图片

* webpack-merge：用于优化配置文件

* splitChunksPlugin：代码分隔

* webpack-bundle-analyzer：可视化的打包优化插件

* webpack-dev-serve

* HRM：热更新

* babel：将es678转为es5

* babel-preset-env：兼容哪些浏览器

* babel/polyfill：没有对应关系就是指E5中根本就没有对应的语法, 例如Promise, includes等方法是ES678新增的。ES5中根本就没有对应的实现, 这个时候就需要再增加一些额外配置, 让babel自己帮我们实现对应的语法

* babel/parser：将js代码转换为ast抽象语法树

* babel/generator：将ast抽象语法树转换为js代码

* babel/traverse：遍历抽象语法树

* babel/types：创建ast抽象语法树

* html-withing-loader：实现html中图片的打包