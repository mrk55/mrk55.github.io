---
layout: post
title: 使用Font-Icon自定义list style
tags: css
categories: css
---

原始答案请戳[这里](https://stackoverflow.com/questions/13354578/custom-li-list-style-with-font-awesome-icon/13354689#13354689?newreg=45915f03d1ac4f2cbf25b504e58817d3)

当你定义自己的list style时，想使用Font Icon但又不想将表现掺入进内容时，该怎么办？这里有一个很好的解决办法。

```css
ul {
  list-style: none;
  padding: 0;
}
li {
  padding-left: 1.3em;
}
li:before {
  content: "\f00c"; /* FontAwesome Unicode */
  font-family: FontAwesome;
  display: inline-block;
  margin-left: -1.3em; /* same as padding-left set on li */
  width: 1.3em; /* same as padding-left set on li */
}
```

还可以通过添加`color`属性，来设置list style的颜色。