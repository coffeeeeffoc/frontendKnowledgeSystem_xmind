[github](https://github.com/formatjs/formatjs)
[doc](https://formatjs.io/)

[toc]

# react-intl

## 应用环境
- Node
- React
- Vue
- 等
## 依赖
- IE11+
  - Intl
- < IE11
  - polyfill
    - polyfill.io
    - [formatjs内置的polyfills](https://formatjs.io/docs/polyfills)
## 配套设施
- cli
- eslint-plugin-formatjs
- babel-plugin-formatjs
- ts-transformer
- swc-plugin
## 语言格式化
- Intl MessageFormat
  ```
  const msg = new IntlMessageFormat(message, locales, [formats], [opts])
  const output = msg.format(values)
  ```
- ICU MessageFormat Parser
  文本会转换为ast
  ```
  import {parse} from '@formatjs/icu-messageformat-parser'
  const ast = parse(`this is {count, plural, 
    one{# dog} 
    other{# dogs}
  }`)
  ```