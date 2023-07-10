[toc]
参考链接：https://segmentfault.com/a/1190000022561279

# 分析工具
## 体积分析
### stats.json
webpack --profile --json > stats.json
### webpack-bundle-analyzer
## 速度分析
### speed-measure-webpack-plugin
# 优化策略
## 使用新版本
## 体积优化
### js压缩
terser-webpack-plugin
### css压缩
optimize-css-assets-webpack-plugin
### 移除无用的css
使用 PurgeCSS 来完成对无用 css 的擦除，它需要和 mini-css-extract-plugin 配合使用。
### 图片压缩
image-webpack-loader
### 拆分代码
- tree-shaking
- splitChunksPlugin 大文件分割为小文件
## 速度优化

### 多线程
- thread-loader
- ParallelUglifyPlugin
### 预先编译资源模块（DllPlugin）
`webpack.DllPlugin`
在第一次打包的时候去打包一下第三方模块，并将第三方模块打包到一个特定的文件中，当第二次 webpack 进行打包的时候，就不需要去 node_modules 中去引入第三方模块，而是直接使用我们第一次打包的第三方模块的文件就行

接着我们需要去修改公共配置文件 webpack.common.js，将我们之前生成的 dll 文件导入到 html 中去，如果我们不想自己手动向 html 文件去添加 dll 文件的时候，我们可以借助一个插件 add-asset-html-webpack-plugin，此插件顾名思义，就是将一些文件加到 html 中去。

同时我们需要使用 webpack 自带的 DllReferencePlugin 插件对 mainfest.json 映射文件进行分析。
### 缓存 Cache 相关
可以开启相应 loader 或者 plugin 的缓存，来提升二次构建的速度
- `babel-loader` 开启缓存
- `terser-webpack-plugin` 开启缓存
- 使用 `cache-loader` 或者 `hard-source-webpack-plugin`

### 合理使用 sourceMap
# 另外
## 合理配置 chunk 的哈希值
在生产环境打包，一定要配置文件的 hash，这样有助于浏览器缓存我们的文件，当我们的代码文件没变化的时候，用户就只需要读取浏览器缓存的文件即可。**一般来说 javascript 文件使用 [chunkhash]、css 文件使用 [contenthash]、其他资源（例如图片、字体等）使用 [hash]。**
