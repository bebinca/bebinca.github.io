---
layout: post
title:  写个样例防止自己忘了
date:   2020-07-02 01:08:00 +0800
categories: blog
tag: 模板
---

* content
{:toc}

标题示例                                                        {#title}
================

这就是第一部分了

+ 这样可以打出点
+ 点
+ 点
  + 这个也算


* 这样也可以把
  * 还可以这样
    * 这样

1. 序号也可以用
2. 如下

在`LessOrMore/_posts`目录下新建一个文件，可以创建文件夹并在文件夹中添加文件，方便维护。在新建文件中粘贴如下信息，并修改以下的`titile`,`date`,`categories`,`tag`的相关信息，添加`* content {:toc}`为目录相关信息，在进行正文书写前需要在目录和正文之间输入至少2行空行。然后按照正常的Markdown语法书写正文。

```bash
---
layout: post
#标题配置
title:  标题
#时间配置
date:   2016-08-27 01:08:00 +0800
#大类配置
categories: document
#小类配置
tag: 教程
---

* content
{:toc}


我是正文。我是正文。我是正文。我是正文。我是正文。我是正文。
```

```c++
int main() {
	printf("oh my gosh!");
}
```

下一级标题                                                                                   {#result}
-

# 如果用md格式

## 如果用md格式
### 如果用md格式
#### 如果用md格式
##### 如果用md格式
###### 如果用md格式
