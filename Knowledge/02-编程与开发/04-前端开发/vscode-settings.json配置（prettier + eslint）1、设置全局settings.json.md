---
url: https://juejin.im/post/5cbda5b65188250aa645a135
source: juejin.im
date_saved: 2026-04-23
tags: LLM, 前端, 后端
content_type: article
word_count: 1305
status: 已处理
---

# vscode-settings.json配置（prettier + eslint）1、设置全局settings.json - 掘金

> source: [juejin.im](https://juejin.im/post/5cbda5b65188250aa645a135)

## 核心要点

1. iconTheme": "vscode-icons-mac",
2. title": "${activeEditorLong}${separator}${rootName}",
3. LastModifiedBy": "niuchunling",
4. //eslint 格式化插件，保存时应用eslint规则自动格式化后保存
5. fontFamily": "Microsoft YaHei，Menlo, Monaco, 'Courier New', monospace",
## 内容预览

vscode-settings.json配置（prettier + eslint）

向日葵同志44330

2019-04-22

13,019

阅读1分钟

1、设置全局settings.json文件

command + p 搜索到settings.json文件，文件配置及注释如下

"workbench.iconTheme": "vscode-icons-mac",
"editor.renderIndentGuides": false,
"cSpell.ignoreWords": ["antd"],
//编辑器失去焦点时自动保存更新后的文件
"files.autoSave": "onFocusChange",
"workbench.colorTheme": "Monokai",
"git.confirmSync": false,
"window.title": "${activeEditorLong}${separator}${rootName}",
"window.zoomLevel": 0,
"editor.fontSize": 14,
//为了符合eslint的两个空格间隔原则
"editor.tabSize": 2,
// 文件头部注释
"fileheader.Author": "niuchunling",
"fileheader.LastModifiedBy": "niuchunling",
//关闭编辑器默认代码检查,为了不跟eslint配置冲突
"editor.formatOnSave": false,
"javascript.format.enable": false,
//eslint 格式化插件，保存时应用eslint规则自动格式化后保存
"eslint.autoFixOnSave": true,
"prettier.eslintIntegration": true,
// 去掉代码结尾分号
"prettier.semi": false,
"git.path": "/usr/bin/git",
"editor.fontFamily": "Microsoft YaHei，Menlo, Monaco, 'Courier New', monospace",
"editor.fontWeight": "bold",
"javascript.updateImportsOnFileMove.enabled": "never",
"explorer.confirmDragAndDrop": false
}
2、 设置项目自己的规则

在项目根目录新建.vscode文件夹，文件夹中新建settings.json覆盖全局的setings.json文件，配置同上

tips: 以上都是基于几个前提

编辑器安装了Prettier - Code formatter 插件并启用

项目配置了使用eslint

fileheader相关配置是安装了Document This插件

向日葵同志44330

前端开发

11

文章

37k

阅读

0

粉丝
## 我的判断

- 价值：待评估
- 适用场景：
- 可信度：
