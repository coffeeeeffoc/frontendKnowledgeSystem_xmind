[toc]
# 三个方案
## 方案1 放弃ES6，使用Node中的require()

## 方案2 使用babel
注意，不能直接用node运行包含import的js文件，需要写启动文件

### 方案2.1 babel-register
- 首先，安装依赖babel-register、babel-preset-env
  ```
   <!-- package.json -->
   "devDependencies": {
    "babel-preset-env": "^1.7.0",
    "babel-register": "^6.26.0"
  }
  ```
- 其次，处理启动脚本
  ```
  <!-- start.js -->
  require('babel-register') ({
      presets: [ 'env' ]
  })
    
  module.exports = require('./index.js')
  ```
### 方案2.2 babel-node
  - 安装@babel/core、@babel/cli
  - 启动
    ```
    babel-node --presets env xxx.js
    ```
## 方案3 使用 Node 中的实验特性（node --experimental-modules | 推荐）
### 使用loader的情况下

### 不使用loader的情况下
  - node v9~v13.2，两种方式：
    - 方式1 不适用loader 需同时满足下面两个条件
      - 1.文件后缀名必须是`.mjs`
      - 2.指定flag，也就是`--experimental-modules`
      example
      ```
        node --experimental-modules xx.mjs
      ```
    - 方式2 使用loader，参考[v9官网](https://nodejs.org/dist/latest-v9.x/docs/api/esm.html#esm_resolve_hook)
      ```
      import url from 'url';
      import path from 'path';
      import process from 'process';
      import Module from 'module';

      const builtins = Module.builtinModules;
      const JS_EXTENSIONS = new Set(['.js', '.mjs']);

      const baseURL = new URL('file://');
      baseURL.pathname = `${process.cwd()}/`;

      export function resolve(specifier, parentModuleURL = baseURL, defaultResolve) {
        if (builtins.includes(specifier)) {
          return {
            url: specifier,
            format: 'builtin'
          };
        }
        if (/^\.{0,2}[/]/.test(specifier) !== true && !specifier.startsWith('file:')) {
          // For node_modules support:
          // return defaultResolve(specifier, parentModuleURL);
          throw new Error(
            `imports must begin with '/', './', or '../'; '${specifier}' does not`);
        }
        const resolved = new url.URL(specifier, parentModuleURL);
        const ext = path.extname(resolved.pathname);
        if (!JS_EXTENSIONS.has(ext)) {
          throw new Error(
            `Cannot load file with non-JavaScript file extension ${ext}.`);
        }
        return {
          url: resolved.href,
          format: 'esm'
        };
      }
      ```
      调用方式
      ```
      NODE_OPTIONS='--experimental-modules --loader ./custom-loader.mjs' node x.js
      ```
  - node v13.2~ 有三种方式可以指定按esm来处理，参照[官网](https://nodejs.org/api/packages.html#packages_determining_module_system)
    - 1.文件后缀.mjs
    - 2.最近的`package.json`中最外层的type属性指定，值为`"module"`.(commonjs则会指定为`"commonjs"`)
      - 注意，相对路径import的文件，必须有文件后缀(参见[官网](https://nodejs.org/api/esm.html#esm_terminology))，比如`import './x.js'`,而不能是`import './x'`.
      - webpack支持，但node和browser都不支持无后缀的文件，因为要精确对应文件。[参考](https://stackoverflow.com/questions/44481851/does-es6-import-export-need-js-extension)
    - 3.携带flag，`--input-type=module`.(commonjs则会指定为`--input-type=commonjs`)
      - 例子：
        ```
        node --input-type=module --eval "import { sep } from 'path'; console.log(sep);"
        ```