---
layout: post
title: 'BackstopJS -- 我心目中最好用的视觉回归测试工具'
date: 2018-06-04
author: 曾星鑫
cover: '/assets/img/backstopjs.png'
tags: BackstopJS css 测试 visual-regression-testing
---

本文介绍了什么是视觉回归测试，并且推荐了一个很好用的测试工具[`BackstopJS`](https://garris.github.io/BackstopJS/)，讲述了`BackstopJS`的使用方法和特点。

## 视觉回归测试

作为一个Web开发者，如果你还不知道视觉回归测试（`visual regression testing`），那你得赶紧补补课了。在[这篇文章](https://www.jianshu.com/p/36102b0d4b9a)中你可以快速了解视觉回归测试的简单定义。如果你感兴趣的话，可以在[这里](https://visualregressiontesting.com/)找到非常多关于是视觉回归测试的文章。

通常来说，视觉回归测试就是把我们开发的网站中的某些指定的部分（某些页面或者页面上某些元素）截图保存下载作为基线图。然后在需要的时候（比如网页有更改时）重新对这些指定的部分进行截图，将新得到的截图和基线图进行像素级的对比。如果像素级没有差别或者差别在一定的范围，我们认为测试通过，否则测试失败。

这种测试主要用在网站持续进行开发和维护的过程中，可以非常快速高效地对我们关心的页面进行回归测试，保证我们新的更改不会破坏已有的功能。

## 测试工具 - BackstopJS

[`BackstopJS`](https://garris.github.io/BackstopJS/)就是这样的一个视觉回归测试工具，它满足了我对一个测试工具的这些要求：

- 快速上手
- 测试代码简单
- 多种即开即用功能

## 快速上手

如果你阅读了`BackstopJS`入门指南，你会发现只需要这几步就能够创建一个`BackstopJS`的测试工程：

1. 安装`BackstopJS`

   我们可以使用npm安装`BackstopJS`:

   ```bash
   npm i -g backstopjs
   ```

2. 初始化测试工程

   使用这一条命令可以在当前文件夹中初始化一个示例测试工程：

   ```Bash
   backstop init
   ```

   其中，你的测试用例会被放在`backstop.json`文件中。

3. 开始测试

   好了，你的测试工程已经生成好了，试试使用这条命令来运行一次你的测试：

   ```bash
   backstop test
   ```

   测试结束时`BackstopJS`会自动打开一个测试报告的网页。由于这个时候我们的基线图都还没有生成，因此我们会看到测试失败的报告，报告中每个测试用例下会有具体的失败原因：

   ![测试失败](/assets/img/backstopjs/backstopjs_report_failed.png)

4. 更新基线图

   在正式的测试流程中，如果你的测试失败了，请在报告中仔细查看失败的原因、对比你的测试截图和基线图之间的区别。如果发现是错误的改动弄挂了测试，请去修复这个bug。而如果发现这个区别就是你的想要的变化，那么就可以更新基线图为当前的截图。所以我们现在可以使用这个命令来接受当前的截图为基线图：

   ```bash
   backstop approve
   ```

   当然，你也可以在确保你的环境正确的前提下，使用以下命令来运行所有的测试用例然后把截图直接作为基线图：

   ```bash
   baskstop reference
   ```

   在你的基线图更新之后，重新运行一次测试`backstop test`，你就可以看到测试通过的报告了：![测试成功](/assets/img/backstopjs/backstopjs_report.png)

好了，这就是我们创建一个`BackstopJS`测试工程的所有步骤了，接下来我们可以尝试修改一下测试页面，看看当测试页面和基线图有差异时测试报告会长什么样子：

![测试有差异](/assets/img/backstopjs/backstopjs_report_diff.png)

## 测试代码简单

我是一个软件开发，而不是测试。 但是为了保证我的代码质量，我在开发的过程中不得不写一些测试来确保别人不会无意中影响了我的功能。但是，我并不希望自己花太多时间在思考如何写测试上。因此，测试代码是否简单，是我衡量一个测试工具是否优良的非常重要的指标。

还记得你在初始化工程中创建出来的`backstop.json`文件吗？`BackstopJS`的测试用例都在这个文件中了，而其中定义具体的测试用例的部分在这里：

```json
  "scenarios": [
    {
      "label": "BackstopJS Homepage",
      "cookiePath": "backstop_data/engine_scripts/cookies.json",
      "url": "https://garris.github.io/BackstopJS/",
      "referenceUrl": "",
      "readyEvent": "",
      "readySelector": "",
      "delay": 0,
      "hideSelectors": [],
      "removeSelectors": [],
      "hoverSelector": "",
      "clickSelector": "",
      "postInteractionWait": 0,
      "selectors": [],
      "selectorExpansion": true,
      "misMatchThreshold" : 0.1,
      "requireSameDimensions": true
    }
  ],
```

其中大部分的属性都是可选的，我们可以暂时忽略它们。于是一个最简单的测试用例就是这样的：

```json
  "scenarios": [
    {
      "label": "BackstopJS Homepage",
      "url": "https://garris.github.io/BackstopJS/",
    }
  ],
```

是的，我们只需要指定这个测试用例的名字以及它需要访问的页面URL就可以了。

所以，在我开发的过程中，当我们新开发出一个页面之后，我只需要将新建的页面的URL加到这个测试用例的数组中即可：

```json
  "scenarios": [
    {
      "label": "BackstopJS Homepage",
      "url": "https://garris.github.io/BackstopJS/",
    },
    {
      "label": "新的页面",
      "url": "https://my-new-page-url",
	}
  ],
```

现在，视觉回归测试就覆盖了我新开发的页面了，我可以放心地提交代码了。

## 多种即开即用功能

当然，在实际地工作中一个测试用例并不总是直接打开一个页面，然后截图这么简单。而很有可能只需要截取页面中的一部分，或者在截图之前需要进行一些其他的操作等等。而`BackstopJS`也考虑到了这个问题，支持很多即开即用的功能。

### 截取页面中指定的元素

我们可以给测试用例的`selectors`参数设置上一个css选择器的数组，然后`BackstopJS`在截图时将会只去截取这个css选择器指定的元素。例如：

```json
  "scenarios": [
    {
      "label": "Selectors test",
      "url": "https://garris.github.io/BackstopJS/",
	  "selectors": [
	    ".moneyshot"
	  ]
    }
  ],
```

在这个测试用例中，测试将会找到有`moneyshot`作为class的HTML元素并且只截取这个元素。

通过这种方式我们就可以在每次测试的过程中只截取我们关心的那一部分内容，而不用太关心页面上别的干扰项。一方面是测试用例的指向性更精确，另一方面也节省了存储图片的空间。

### 让鼠标点击或者悬浮于某个元素

当我们的页面有某些交互功能时，我们可能需要在截图之前使用鼠标点击某个元素或者悬浮于这个元素上。在这种情况下，我们可以使用`clickSelector`或者`hoverSelector`属性来指定需要点击或者悬浮的元素。而通常情况下，我们会使用`postInteractionWait`属性来保证测试在截图时点击或者悬浮事件已经完成了。例如：

```json
  "scenarios": [
    {
      "label": "Click test",
      "url": "https://garris.github.io/BackstopJS/?click",
      "clickSelector": '#theLemur',
      "postInteractionWait": "._the_lemur_is_ready_to_see_you"
    },
    {
      "label": "Hover test",
      "url": "https://garris.github.io/BackstopJS/?click",
      "hoverSelector": '#theLemur',
      "postInteractionWait": 1000
    }
  ],
```

第一个测试会在元素`#theLemur`被点击后，等待到`._the_lemur_is_ready_to_see_you`出现时开始截图。第二个测试会在鼠标悬浮在`#theLemur`上1000毫秒之后开始截图。

### 设置截图前的等待

很多时候，我们的页面需要等待一段时间才能显示正确的内容的。比如当我们使用React或者Vue这种前端框架实现的话，我们的页面需要等待这些前端框架的JS执行完毕后才能正确地显示。于是在测试中，我们也需要等待到这些JS执行完毕后才开始截图。`BackstopJS`提供了几种设置等待的方式。

- `readySelector`：指定一个css选择器，`BackstopJS`会在这个选择器指定的元素出现时才开始截图。

  ```json
  "scenarios": [
      {
        "label": "readySelector",
        "url": "https://garris.github.io/BackstopJS/?delay",
        "readySelector": "._the_lemur_is_ready_to_see_you",
      },
    ],
  ```

  这个测试用例会等待到元素`._the_lemur_is_ready_to_see_you`出现时才开始截图。

- `readyEvent`: 指定一个字符串，当浏览器的控制台中出现这个字符串时才开始截图。

  ```json
  "scenarios": [
      {
        "label": "readyEvent",
        "url": "https://garris.github.io/BackstopJS/?delay",
        "readyEvent": "_the_lemur_is_ready_to_see_you",
      },
    ],
  ```

  这个测试用例会等待你的网页的控制台中出现字符串_the_lemur_is_ready_to_see_you时，也就是当你的页面执行到代码`console.log('the_lemur_is_ready_to_see_you')`时才开始截图。

- `delay`: 设置一个等待时间（单位为毫秒），测试会在页面加载好之后等待这个时间，再开始截图。

  ```json
  "scenarios": [
      {
        "label": "delay",
        "url": "https://garris.github.io/BackstopJS/?delay",
        "delay": 5000,
      },
    ],
  ```

  这个测试用例会在页面加载好5秒之后再开始截图。

### 隐藏某些元素

在很多情况下，我们的网页会有一些动态的元素，它在我们每一访问的时候可能都是不一样的，比如网站上的广告。因此，我们测试不能包括这些动态的内容。有一种最简单的方式处理它们，就是在截图之前隐藏这个元素。

`BackstopJS`提供了两种隐藏元素的方式：`hideSelectors`和`removeSelectors`。`hideSelectors`会给元素加上css样式`visibility:hidden`，而`removeSelectors`会直接将元素移除掉。例如：

```json
"scenarios": [
    {
      "label": "hideSelectors",
      "url": "https://garris.github.io/BackstopJS/",
      "hideSelectors": [".logo-link", "p"]
    },
    {
      "label": "removeSelectors",
      "url": "https://garris.github.io/BackstopJS/",
      "removeSelectors": [".logo-link", "p"]
    },
  ],

```

这两个测试用例的截图中都不会有满足css选择器`.logo-link`和`p`元素。测试用例`hideSelectors`中这些元素只是被隐藏了，页面的其他部分并没有发生变化。而测试用例`removeSelectors`中其他的元素会挤过来占据这些元素原来的空间。

### 其他功能

当然`BackstopJS`提供了更多的实用功能，我们可以在它的[github网页](https://github.com/SBeator/BackstopJS)上看到更多更全的功能。这些功能可以让我们在很容易地实现各种常用的页面操作，让我们的测试用例变得简单，也让开发可以专注于开发而不是花太多精力去学习和写测试。

## 后记

当然，`BackstopJS`也有一些缺点，比如支持的浏览器十分有限。但是当我们选择一个工具时，我们更应该考虑它是否能够解决我们当前工作中遇到的问题。

作为一个开发，我希望知道我的更改是不是破坏其他的功能，并且希望知道如何保证我的更改不会在以后的开发过程中被无意中破坏了，因此我需要一个视觉回归测试工具。而我在挑选工具时，考虑最多的因素就是在视觉测试能够正确地运行的前提下，如何最简单地创建测试，而不用让写测试占据我大量的时间和精力。这些考虑都是`BackstopJS`可以带给我的。因此，我想最后再说一次：`BackstopJS`是我心目中最好用的视觉回归测试工具。

