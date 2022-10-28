---
# type: posts 
title: "捕获TextView超链接"
date: 2015-10-03T22:18:17+0800
authors: ["Jianan"]
summary: "Android的TextView是个很强大的控件，通过Html类处理html文本后可以支持部分html标签。有时候需要捕获TextView中<a>标签的点击事件进行自己的超链接点击处理，下面的代码用于捕获TextView中<a>标签点击后的响应事件：        CharSequence charSequence = Html.fromHtml(strHtml);
        Spannabl"
series: ["Android"]
categories: ["Android"]
tags: ["android", "textview", "html", "超链接", "a"]
images: []
featured: false
comment: true
toc: true
reward: true
pinned: false
carousel: false
draft: false
read_num: 1266
comment_num: 0
---

Android的TextView是个很强大的控件，通过Html类处理html文本后可以支持部分html标签。有时候需要捕获TextView中&lt;a>标签的点击事件进行自己的超链接点击处理，下面的代码用于捕获TextView中&lt;a>标签点击后的响应事件：

```java
		CharSequence charSequence = Html.fromHtml(strHtml);
        SpannableStringBuilder builder = new SpannableStringBuilder(charSequence);
        URLSpan[] urlSpans = builder.getSpans(0, charSequence.length(), URLSpan.class);
        for(URLSpan span : urlSpans){
            int start = builder.getSpanStart(span);
            int end = builder.getSpanEnd(span);
            int flag = builder.getSpanFlags(span);
            final String link = span.getURL();
            builder.setSpan(new ClickableSpan() {
                @Override
                public void onClick(View widget) {
                    //捕获<a>标签点击事件，及对应超链接link
                }
            }, start, end, flag);
            builder.removeSpan(span);
        }

        textView.setLinksClickable(true);
		textView.setMovementMethod(LinkMovementMethod.getInstance());
        textView.setText(charSequence);
```

总结为三个步骤：

1. 使用Html.fromHtml(String strHtml)转换html标签字符串，fromHtml()方法中会对html标签进行替换，并html标签封装成对应的格式对象。其中每一个&lt;a>标签都会对应一个URLSpan对象。
2. 获取文本中所有的URLSpan对象，取出URLSpan对象的对应的位置、标识、以及对应的url地址后，使用ClickableSpan对象进行替换，并做自己的超链接逻辑处理。
3. Textview设置链接可点击，以及点击响应处理属性。