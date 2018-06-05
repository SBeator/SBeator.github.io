---
layout: post
title: 'BackstopJS -- 我心目中最好用的视觉回归测试工具'
date: 2018-06-04
author: 曾星鑫
cover: '/assets/img/backstopjs.png'
tags: BackstopJS css 测试 visual-regression-testing
---

## 视觉回归测试

作为一个Web开发者，如果你还不知道视觉回归测试（`visual regression testing`），那你得赶紧补补课了，你可以在[这篇文章](https://www.jianshu.com/p/36102b0d4b9a)中快速学习到视觉回归测试的简单定义。而如果你感兴趣的话，可以在[这里](https://visualregressiontesting.com/)找到非常多关于是视觉回归测试的文章。

通常来说，视觉回归测试就是把我们开发的网页中的某些指定的部分（某些页面后者某些页面的部分）截图保存下载作为基线图，然后在需要的时候（比如网页有更改时）重新对这些指定的部分进行截图，并将这次的截图和基线图进行像素级别的对比。如果像素级别没有差别或者差别在一定的范围，我们认为测试通过，否则测试失败。

这种测试主要用在网站持续进行开发和维护的过程中，可以让非常快速地对我们关心的页面进行回归测试，保证我们新的更改不会破坏已有的功能。

## 测试工具 - BackstopJS

当然，为了实现视觉回归测试，我们需要一个测试工具的帮助。现在我们有很多工具可以选择。而这篇文章，我想推荐一个工具，它是到目前为止，我心目中最好的视觉回归测试工具：[`BackstopJS`](https://garris.github.io/BackstopJS/)

它满足了我对一个测试工具的这些要求：

- 快速上手
- 测试代码简单
- 多种即开即用功能

## 快速上手

如果你阅读了`BackstopJS`入门指南，你会发现你只需要这几步就能够创建一个`BackstopJS`的测试工程：

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

   好啦，你的功能已经生成好了，试试使用这条命令来运行一次你的测试：

   ```bash
   backstop test
   ```

   然后你可以在命令行中或者自动打开的HTML页面中看到测试结果。由于这个时候我们的基线图都还没有生成，因此我们会看到测试失败的报告，你可以在每个测试用例的报告下面看到具体的失败原因：

   ![测试失败](/assets/img/backstopjs/backstopjs_report_failed.png)

4. 更新基线图

   如果你的测试失败了，请在HTML页面中仔细对比你的测试截图和基线图之间的区别。如果发现是你弄挂了测试，请去修复这个bug，而如果发现这个区别就是你的想要的变化，那么就可以使用这条命令来更新基线图：

   ```bash
   backstop approve
   ```

   当然，你也可以在确保你的环境正确的前提下，直接把所有的截图直接作为基线图，使用这个命令即可：

   ```bash
   baskstop reference
   ```

   在你的基线图更新之后，重新运行一次测试，你就可以看到测试通过的报告了：![测试成功](/assets/img/backstopjs/backstopjs_report.png)

好了，这就是我们创建一个`BackstopJS`测试工程的所有步骤了，接下来我们可以尝试修改一下测试页面，看看当测试页面和基线图有差异时测试报告会长什么样子：

![测试有差异](/assets/img/backstopjs/backstopjs_report_diff.png)

## 测试代码简单

我是一个软件开发，而不是测试。 但是为了保证我的代码质量，我在开发的过程中不得不写一些测试来确保别人不会无意中影响了我的功能。但是，我并不希望自己花太多时间在思考如何写测试上。因此，测试代码是否简单，是我衡量一个测试工具是否优良的非常重要的指标。

还记得你在初始化工程中创建出来的`backstop.json`文件吗？`BackstopJS`的测试用例都在这个文件中了，而具体的测试用例长成这个样子：

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

所以，在我开发的过程中，当我心开发出一个页面之后，我只需要将新建的页面的URL加到这个测试用例中即可：

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

现在，我的新页面已经被视觉回归测试覆盖了，我可以放心地提交代码了。

## 多种即开即用功能

当然，在实际地工作中一个测试用例不会总是直接地截取一个完整地页面，而可能只是截图页面中的一部分，或者在截图之前需要进行一些其他的操作。而`BackstopJS`也考虑到了这个问题，因而支持很多即开即用的功能：

### 截取特定的部分

我们可以给测试用例的`selectors`参数设置上一个css选择器的数组，于是我们的测试截图将会只去截取这个css选择器指定的元素。例如：

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

这个测试用例中，测试将会找到有`moneyshot`作为class的HTML元素并且截图。

通过这种方式我们就可以在每次测试的过程中只截图我们关心的那一部分内容。

### 让鼠标点击或者悬浮于某个元素

当我们的页面有某些交互功能是，我们可能需要在截图之前使用鼠标点击或者悬浮于某个元素。这种情况下，我们可以使用`clickSelector`或者`hoverSelector`属性。而通常情况下，我们可能会使用`postInteractionWait`属性来配合点击后者悬浮属性，来保证测试在截图时，点击或者悬浮事件已经完成了。例如：

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

这两个测试用例分别测试了当元素`theLemur`在被点击后和被鼠标悬浮后的页面。

### 设置截图前的等待

很多时候，我们的页面是需要等待一段时间才能显示正确的内容的，比如如果我们的页面使用的是React或者Vue这种前框框架加载的话，很多时候我们需要等待这些前端JS执行完毕才能开始截图。而`BackstopJS`提供了几种设置等待的方式。

- `readySelector`：可以指定一个css选择器，`BackstopJS`会在这个选择器选择到的元素存在时才开始截图。

  ```json
  "scenarios": [
      {
        "label": "readySelector",
        "url": "https://garris.github.io/BackstopJS/?delay",
        "readySelector": "._the_lemur_is_ready_to_see_you",
      },
    ],
  ```

  这个测试用例会等待有class`_the_lemur_is_ready_to_see_you`的元素出现时才开始截图。

- `readyEvent`: 可以指定一个字符串，当浏览器的控制台中出现这个字符串时才开始截图。

  ```json
  "scenarios": [
      {
        "label": "readyEvent",
        "url": "https://garris.github.io/BackstopJS/?delay",
        "readyEvent": "_the_lemur_is_ready_to_see_you",
      },
    ],
  ```

  这个测试用例会等待你的网页的控制台中出现字符串_the_lemur_is_ready_to_see_you时，也就是当你的页面指定到代码`console.log('the_lemur_is_ready_to_see_you')`时才开始截图。

- `delay`: 这是最好理解的一种等待方式，只需要设置一个时间（单位为毫秒），测试会在页面加载好之后等待这个时间，才开始截图。

  ```json
  "scenarios": [
      {
        "label": "delay",
        "url": "https://garris.github.io/BackstopJS/?delay",
        "delay": 5000,
      },
    ],
  ```

  这个测试用例会在页面加载好5秒后在开始截图。

### 隐藏某些元素

在很多情况，我们的网页会有一些动态的元素，它在我们每一访问的时候都是不一样的，比如网站上的广告。在测试这种页面时，我们需要在每次进行视觉回归测试时mock这个动态的元素，或者更简单的方式，直接隐藏这个元素。

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

这两个测试用例的截图中都不再有满足css选择器`.logo-link`和`p`元素。测试用例`hideSelectors`中这些元素只是被隐藏了，页面的其他部分并没有发生变化。而测试用例`removeSelectors`中其他的元素会挤过来占据这些元素原来的空间。

### 其他功能

当然`BackstopJS`提供了更多的实用功能，我们可以在它的[github网页](https://github.com/SBeator/BackstopJS)上看到更多更全的功能。这些功能可以让我们在很容易地实现各种常用的页面操作，让我们的测试用例变得简单，也让开发可以专注于开发而不是花太多精力去学习和写测试。

## 后记

当然，`BackstopJS`也有一些缺点，比如支持的浏览器十分有限。但是当我们选择一个工具时，我们更应该考虑它能够解决的主要问题：如何高效地进行视觉回归测试。

作为一个开发，我希望能够用最简单的方式创建测试，而不用花太多的经历去研究他们，我们也希望知道我的更改是不是可以安全地提交。这些都是`BackstopJS`可以带给我的。因此，我想说：`BackstopJS`是我心目中最好用的视觉回归测试工具。

