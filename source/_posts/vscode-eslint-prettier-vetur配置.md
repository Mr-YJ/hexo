---
title: vscode eslint+prettier+vetur配置
date: 2019-07-24 18:21:16
tags:
---

## 需要安装的依赖

```bash
"@vue/cli-plugin-babel": "^3.0.3",
"@vue/cli-plugin-eslint": "^3.0.3",
"@vue/cli-service": "^3.0.3",
"@vue/eslint-config-prettier": "^4.0.1",
"babel-eslint": "^10.0.1",
"babel-polyfill": "^6.26.0",
"eslint": "^5.8.0",
"eslint-config-prettier": "^4.1.0",
"eslint-plugin-prettier": "^3.0.1",
"eslint-plugin-vue": "^5.0.0",
```

<!--more-->

## vscode 中设置

```bash
// tab两个空格
"editor.tabSize": 2,
"editor.tabCompletion": "on",
"editor.detectIndentation": false,
// 保存时自动格式化
"editor.formatOnSave": true,
// 使用prettier格式化js文件
"[javascript]": {
  "editor.defaultFormatter": "esbenp.prettier-vscode"
},
// 离开操作文件时候自动保存
"files.autoSave": "onFocusChange",

"eslint.enable": true,
"eslint.autoFixOnSave": true,

"eslint.validate": [
  "javascript",
  "javascriptreact",
  {
    "language": "vue",
    "autoFix": true
  }
],

// 使用单引号
"prettier.singleQuote": true,
// 去掉每行后的分好
"prettier.semi": false,

// 不检查模板
"vetur.validation.template": false,
"vetur.format.defaultFormatter.html": "js-beautify-html",
"vetur.format.defaultFormatterOptions": {
  // html属性不自动换行
  "js-beautify-html": {
    "wrap_attributes": "auto"
  },

  // 使得vue文件能够更加prettier格式化
  "prettier": {
    "semi": false,
    "singleQuote": true
  }
},
```

## eslint 配置文件需要创建.eslintrc.js

```bash
module.exports = {
  root: true,
  env: {
    node: true
  },
  extends: ['plugin:vue/essential'],
  rules: {
    'no-console': process.env.NODE_ENV === 'production' ? 'error' : 'off',
    'no-debugger': process.env.NODE_ENV === 'production' ? 'error' : 'off',
    'vue/no-parsing-error': [2, { 'x-invalid-end-tag': false }],
    quotes: ['error', 'single', { allowTemplateLiterals: true }],
    semi: ['error', 'never'],
    'space-before-function-paren': ['error', 'never']
  },
  parserOptions: {
    parser: 'babel-eslint'
  }
}
```
