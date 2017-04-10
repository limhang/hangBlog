---
title: sublime3安装使用
date: 2017-04-10 11:36:59
tags: 编辑器
categories: 通用技能
---

# 缘由：换一个电脑，换一个工作环境，都需要重新配置一次sublime，这里做下记录，以便以后方便移植配置

<!--more-->

# 一、偏好设置
## 1-1、setting user的配置
```
{
	"font_size": 16.0,
	"ignored_packages":
	[
		"Vintage"
	]
}
```

## 1-2、key bindings user的配置
```
[
	{ "keys": ["super+shift+a"], "command": "show_overlay", "args": {"overlay": "command_palette"} },
  { "keys": ["super+g"], "command": "show_overlay", "args": {"overlay": "goto", "text": ":"} },


	   {
      "keys": [
        "super+e"
      ],
      "args": {
        "action": "expand_abbreviation"
      },
      "command": "run_emmet_action",
      "context": [{
        "key": "emmet_action_enabled.expand_abbreviation"
      }]
    },
    {
      "keys": ["tab"],
      "command": "expand_abbreviation_by_tab",
      "context": [{
        "operand": "source.js",
        "operator": "equal",
        "match_all": true,
        "key": "selector"
      }, {
        "key": "preceding_text",
        "operator": "regex_contains",
        "operand": "(\\b(a\\b|div|span|p\\b|button)(\\.\\w*|>\\w*)?([^}]*?}$)?)",
        "match_all": true
      }, {
        "key": "selection_empty",
        "operator": "equal",
        "operand": true,
        "match_all": true
      }]
    }
]
```

## 1-3、建立软链
```
ln -s "/Applications/Sublime Text 2.app/Contents/SharedSupport/bin/subl" /usr/local/bin/sublime
```
如果之前有过软链，终端会提示exit，直接删除即可，rm XXX

# 二、插件相关
* package control

sublime安装插件的包管理工具
安装：[参考](https://www.imjeff.cn/blog/62/)、[我的简书](http://www.jianshu.com/p/1f0fe9476e0f)

* emmet

前端编写神器

* sublimeCodeIntel

代码自动补全插件，[配置](http://blog.csdn.net/hehexiaoxia/article/details/54134756)

* sublimelinter

代码检错插件，[配置1](http://www.cnblogs.com/lhb25/archive/2013/05/01/sublimelinter-for-js-css-coding.html)、[配置2](https://segmentfault.com/a/1190000000389188)

* babel

支持es6与jsx语法高亮
