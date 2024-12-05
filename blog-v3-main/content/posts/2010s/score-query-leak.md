---
title: 记一个陕西中考分数查询系统漏洞
description: 2019年陕西中考成绩公布前，发现中考分数查询系统的漏洞，可以提前获取并确认成绩准确性，帮助了许多无法正常查询成绩的同学。
date: 2019-07-23 22:53:00
updated: 2019-09-08 11:03:00
type: story
categories: [经验分享]
tags: [初中, 高中, 说说]
---

中考结束后，出成绩前一晚，我登入了陕西省中考分数查询系统，这是一个带有准考证号和身份证号输入框的网页。

输入信息后点击查询，系统提醒查询时间未到。

我使用HttpCanary抓包软件，发现网页在查询时，向`http://111.20.215.158:8091/MForm/Score/QueryScoreHandler.ashx`发送了请求，带有参数`Zkzh`（准考证号）和`Sfzh`（字母S开头的学籍号，与身份证号同号）。

直接访问这个地址，得到的是一串JSON代码，里面包含了查询的返回数据。

朋友跟我说，明天出成绩，虚的他手抖，我就把这个方法告诉了他。因为这样的话看到的数据不直观，而不像“通知书”那样正式，所以不会令人那么紧张。

第二天中午12:00出成绩，但从早上开始，许多同学家长在家长自建的学校家长群里说着“查询界面加载慢”“查询界面关了”之类的话。现在查询界面连输入框都没有了，直接说“查询未开始”。我试着重传昨晚抓包得到的请求，结果出现了好多科目的分数，我慌了，因为看到的成绩没有达到预期的目标。而且现在才不到11点，大家都查不到成绩，我怎么能提前知道成绩呢？

后来过了12点，还有一些同学和我说进不去查询页面，我就把这个方法告诉了他们：

```
http://111.20.215.158:8091/MForm/Score/QueryScoreHandler.ashx?Zkzh=把这里改成准考证号&Sfzh=S把这里改成身份证号
```

说来也怪，他们本来进不去查询界面的，但是用这串网址立马就能查到成绩。我害怕查到的内容不准确，特意让一些已经通过正常方式查到成绩的同学再用这个方法查一遍，他们也都很快查到了成绩，并且和之前查到的分数相同。说明这个方法是准确有效的，一些进不去系统的同学也能查到自己的中考分数了。好耶！

::alert{title="2019年9月8日 补充"}

进入高中后，我把这件事情讲给新朋友听，有一个朋友说，他就是用群里看到的这串代码查到了自己的成绩。后来我又问了问其他新同学，还有几个同学也是用这种方法查到的成绩。

所以我发了一条说说：`I'm indeed that person.`，配上了中考查分前一晚和朋友的聊天记录。

消息传播的力量是强大的，没想到我无意间研究出来的方法竟然传播了这么广，真切地解决了大家遇到的问题。
::