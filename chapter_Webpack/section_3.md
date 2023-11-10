# 第 3 题：webpack增量更新

webpack 打包时 hash 值有三种：fullhash（webpack4.x 版本及之前为 hash，webpack5.x 中使用 fullhash 和 hash 均可）、chunkhash 和 contenthash


### fullhash

fullhash 是全量的 hash，是整个项目级别的。只要项目中有任何一个的文件内容发生变化，打包后所有文件的hash值都会发生改变。


### chunkhash

chunkhash 根据不同的入口文件（entry）进行依赖文件解析、构建对应的 chunk、生成对应的哈希值。当某个文件内容发生变动时，再次执行打包，只有改文件以及依赖该文件的文件的打包结果 hash 值会发生改变

### contenthash

contenthash 是只有当文件自己的内容发生改变时，其打包的 hash 值才会发生改变


```
// ...
module.exports = {
  output: {
    path: path.resolve(__dirname, './dist'),
    filename: '[name].[chunkhash].js',
    clean: true,
  },
  // ...
  optimization: {
    splitChunks: {
      chunks: 'all',
    },
    minimizer: [
      new MiniCssExtractPlugin({
        filename: '[name].[contenthash].css',
      }),
    ],
  },
};
```


### 总结

在生产环境下，我们对 output 中打包的文件名一般采用 chunkhash，对于 css 等样式文件，采用 contenthash，这样可以使得各个模块最小范围的改变打包 hash 值。

一方面，可以最大程度地利用浏览器缓存机制，提升用户的体验；另一方面，合理利用 hash 也减少了 webpack 再次打包所要处理的文件数量，提升了打包速度。

[webpack中fullhash、chunkhash和contenthash的区别](https://juejin.cn/post/6971987696029794312)