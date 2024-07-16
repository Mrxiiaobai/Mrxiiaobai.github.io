---
categories: react
tags: react配置装饰器
title: react配置装饰器
---

# react配置装饰器

```javascript
npm i --save-dev customize-cra react-app-rewired
```

## 1.  在根目录下新建 config-overrides.js 文件并写入以下内容

```javascript
const {
    override,
    addDecoratorsLegacy,
    disableEsLint
} = require("customize-cra");

module.exports = override(
    // enable legacy decorators babel plugin
    addDecoratorsLegacy(),

    // disable eslint in webpack
    disableEsLint()
);
```

## 2.  修改 package.json 文件

```javascript
"scripts": {
  "start": "react-app-rewired start",
  "build": "react-app-rewired build",
  "test": "react-app-rewired test",
  "eject": "react-scripts eject"
}
```

```javascript
npm i --save-dev @babel/plugin-proposal-decorators
```

## 3.  创建.babelrc

```javascript
{
  "presets": ["@babel/preset-env", "react-app"],
  "plugins": [
      // ["@babel/plugin-syntax-dynamic-import"],
      ["@babel/plugin-proposal-decorators", { "legacy": true }],
      ["@babel/plugin-proposal-class-properties", { "loose": true }],
      [
        "import",
        {
          "libraryName": "antd",
          "style": "css"
        }
      ]
  ]
}
```

## 4 重启


