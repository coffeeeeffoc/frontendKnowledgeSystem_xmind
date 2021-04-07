[toc]
## 最新版本
- process.env.PUBLIC_URL
- homepage
## 历史版本
- `react-app-rewired` + `config-overrides.js` + `servedPath`
  1.安装`react-app-rewired`
  2.`package.json`的`scripts`设置为
    ```
    "start": "react-app-rewired start",
    "build": "react-app-rewired build",
    "test": "react-app-rewired test",
    "eject": "react-scripts eject"
    ```
  其中若需要设置env可以通过`cross-env`设置，例如
    ```
    "start": "cross-env PORT=3006 react-app-rewired start"
    ```
  3.在`config-overrides.js`中设置
    ```
    module.exports = {
        paths: function(paths, env) {
            paths.appBuild = __dirname + '/dist'
            paths.servedPath = '/module_config/'
            return paths;
        },
    }
  ```