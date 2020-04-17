---
title: R Markdown的使用技巧
author: Songqi Duan
date: '2020-04-17'
slug: r_markdown
categories:
  - 生物信息学
tags:
  - R
lastmod: '2020-04-17T11:42:23+08:00'
keywords: []
description: ''
comment: no
autoCollapseToc: no
postMetaInFooter: true
hiddenFromHomePage: no
contentCopyright: no
reward: no
mathjax: true
mathjaxEnableSingleDollar: true
mathjaxEnableAutoNumber: true
hideHeaderAndFooter: no
flowchartDiagrams:
  enable: no
  options: ''
sequenceDiagrams:
  enable: no
  options: ''
---
`R markdown`通过R代码创建交互式报告。这篇文章提供了一些我每天用来改善输出文档外观的技巧。
<!--more-->
此文介绍`R markdown`的一些使用技巧和常用快捷键[^1][^2]。
# 图片居中
```
<center>
![]()
</center>
```
<center>
![Naruto](/post/2020-04-17-r-markdown的使用技巧_files/bg_footer_1.jpg)
</center>

# 图注
可以在`{r}`中增添以下信息：
```
{r, 
  fig.align="center", fig.width=6, fig.height=6,
  fig.cap="Here is a really important caption."
}
```

```{r, fig.align="center", fig.width=6, fig.height=6, fig.cap="Here is a really important caption."}
plot(c(1:10))
```

# 数学公式

块级公式：
```
$$E=mc^2$$
```
$$E=mc^2$$
行内公式：
```
$c\approx 299,792,458\space m/s$
```
其中$c\approx 299,792,458\space m/s$

# 并排放两张图

可以在`{r}`中增添以下信息：
```
{r out.width=c('50%', '50%'), fig.show='hold'}
```

```{r out.width=c('50%', '50%'), fig.show='hold'}
boxplot(rnorm(10))
plot(rnorm(10))
```

# 交互式图表

可使用`plotly`[^3]构建交互式图表，充分利用`html`可交互的特性。

```{r, fig.align="center", fig.cap="交互式图表"}

suppressPackageStartupMessages(library(ggplot2))
suppressPackageStartupMessages(library(plotly))
library(gapminder)

p <- gapminder %>%
  filter(year==1977) %>%
  ggplot(aes(gdpPercap, lifeExp, size = pop, color=continent)) +
  geom_point() +
  scale_x_log10() +
  theme_bw()

ggplotly(p)
```


[^1]: [Pimp my RMD: a few tips for R Markdown](https://holtzy.github.io/Pimp-my-rmd/#)
[^2]: [第 5 章 R Markdown](https://bookdown.org/xiao/RAnalysisBook/r-markdown.html)
[^3]: [plotly](https://plotly.com/)
