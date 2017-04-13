---
title: python基础语法总结
date: 2017-04-13 14:40:35
tags: python
categories: python
---

# 缘由：很多常用的语法，老是忘记，在此做个总结，相当于自己的三级缓存吧

<!--more-->

# 一、文件操作相关
## 1-1、os模块相关
* 获取当前的文件目录
```python
import os
print(os.getcwd())
```
* 如果不存在此目录，创建一个该文件夹
```python
if not os.path.exists(directory):
    os.makedirs(directory)
```

* open()函数写入文件到指定目录
```python
在mac上，亲测可以如下：
open('/Users/het/Desktop/jacob/xxx.html','a')
```

**demo:**
```python
from pprint import pprint
from goose import Goose
from goose.text import StopWordsChinese
import re
import os
source_file = open('source.md','r')
source_data = source_file.read()
source_array = re.split('--', source_data)

def getUrl(item):
	url_name = re.split('&&', item)
	url = url_name[1]
	name = url_name[0]
	print url
	print name
	html_name = name + '.html'
	print html_name
	g = Goose({'stopwords_class': StopWordsChinese})
	article = g.extract(url=url)
	# print article.raw_html
	currentDir = os.getcwd() + '/' + 'pages' + '/' + name
	if not os.path.exists(currentDir):
		os.makedirs(currentDir)
	f = open(currentDir + '/' +html_name,'a')
	f.write(article.raw_html)
	f.close()

	f_md = open(currentDir + '/' + name + '.md' ,'a')
	f_md.write(article.cleaned_text.encode('utf-8'))
	f_md.close()
# pprint (vars(article))

for item in source_array:
	getUrl(item)

```