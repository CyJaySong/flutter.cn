---
title: Performance best practices
title: Flutter 应用性能优化最佳实践
short-title: Best practices
short-title: 最佳实践
description: How to ensure that your Flutter app is performant.
description: 确保 Flutter 应用的性能。
tags: Flutter性能
keywords: 最佳实践
---

{% include docs/performance.md %}

Generally, Flutter applications are performant by default,
so you only need to avoid common pitfalls to get excellent
performance. These best recommendations will help you
write the most performant Flutter app possible.

通常来说，Flutter 构建的应用程序在默认情况下都是高性能的。
所以你只需要避开常见的陷阱，不需要使用复杂的分析工具
对细节做优化，就可以获得优异的性能，
这些最佳建议将帮助你编写性能最佳的 Flutter 应用程序。

If you are writing web apps in Flutter, you might be interested
in a series of articles, written by the Flutter Material team,
after they modified the [Flutter Gallery][] app to make it more
performant on the web:

如果你在用 Flutter 编写 Web 应用，你可能会对下面的系列文章感兴趣，
他们由 Flutter Material 团队撰写，记录了对 [Flutter Gallery][] 应用的修改，
使其在 Web 上的性能更好:

* [Optimizing performance in Flutter web apps with tree
   shaking and deferred loading][web-perf-1]

  [通过 tree shaking 和延迟加载来优化 Flutter Web 应用的性能
  (Optimizing performance in Flutter web apps with 
  tree shaking and deferred loading)][web-perf-1]

* [Improving perceived performance with image placeholders,
   precaching, and disabled navigation transitions][web-perf-2]

  [通过使用图像占位符、预缓存和禁用导航效果来提高性能
  (Improving perceived performance with image placeholders, 
  precaching, and disabled navigation transitions)][web-perf-2]

* [Building performant Flutter widgets][web-perf-3]

  [高效构建 Flutter widgets (Building performant Flutter widgets)][web-perf-3]

[Flutter Gallery]: {{site.gallery}}
[web-perf-1]: {{site.flutter-medium}}/optimizing-performance-in-flutter-web-apps-with-tree-shaking-and-deferred-loading-535fbe3cd674
[web-perf-2]: {{site.flutter-medium}}/improving-perceived-performance-with-image-placeholders-precaching-and-disabled-navigation-6b3601087a2b
[web-perf-3]: {{site.flutter-medium}}/building-performant-flutter-widgets-3b2558aa08fa

## Best practices

## 最佳实践

How do you design a Flutter app to most efficiently
render your scenes? In particular, how do you ensure
that the painting code generated by the
framework is as efficient as possible?
Here are a few things to consider
when designing your app:

如何设计一个能最有效地渲染页面的 Flutter 应用程序？
特别是如何确保底层框架生成的绘图代码尽可能高效？
这里有几件需要你在设计应用时考虑的事情：

### Controlling build() cost

### 控制 build() 方法的耗时

* Avoid repetitive and costly work in `build()` methods
  since `build()` can be invoked frequently when
  ancestor Widgets rebuild.

  避免在 `build()` 方法中进行重复且耗时的工作，
  因为当父 widget 重建时，子 Wdiget 的 `build()` 方法会被频繁地调用。

* Avoid overly large single Widgets with a large `build()` function.
  Split them into different Widgets based on encapsulation
  but also on how they change:

  避免在一个超长的 `build()` 方法中返回一个过于庞大的 widget。
  把他们分拆成不同的 widget，并进行封装，另外他们要这样改变：

  * When `setState()` is called on a State, all descendent widgets
    rebuild. Therefore, localize the `setState()` call to the part of
    the subtree whose UI actually needs to change. Avoid calling
    setState() high up in the tree if the change is contained
    to a small part of the tree.

    当在 State 上调用 `setState()`时，所有后代 widget 都将重建。
    因此，将 `setState()` 的调用转移到其 UI 实际需要更改的 widget 子树部分。
    如果改变的部分仅包含在 widget 树的一小部分中，
    请避免在 widget 树的更高层级中调用 `setState()`。

  * The traversal to rebuild all descendents stops when the
    same instance of the child widget as the previous
    frame is re-encountered. This technique is heavily
    used inside the framework for optimizing
    animations where the animation doesn't affect the child subtree.
    See the [`TransitionBuilder`][] pattern and
    the [source code for `SlideTransition`][],
    which uses this principle to avoid rebuilding its
    descendents when animating.

    当重新遇到与前一帧相同的子 widget 实例时，将停止遍历。
    这种技术在框架内部大量使用，用于优化动画不影响子树的动画。
    请参阅 [`TransitionBuilder`][] 模式和遵循此原则的
    [SlideTransition 代码][source code for `SlideTransition`]，
    以避免在动画过程中重建其后代 widget。

Use `const` constructors on widgets as much as possible, since
they allow Flutter to short-circuit most of the rebuild work. To create
reusable pieces of UIs, prefer using a [`StatelessWidget`][] rather than
a function.

请尽可能的在 widget 上使用 `const` 构造函数，
这将让 Flutter 的 widget 重建时间大幅缩短。
在构建可复用的 UI 代码时，最好使用 [`StatelessWidget`][] 而不是函数。

Also see:

另请参考：

* [Performance considerations][], part of the [`StatefulWidget`][] API doc

  [`StatefulWidget`][] API 文档中的 [Performance considerations][] 部分。

* [Widgets vs helper methods][], a video from the official Flutter YouTube
  channel that explains why widgets (especially widgets with `const` constructors)
  are more performant than functions.

  [Widget 与 helper 方法][Widgets vs helper methods]，
  Flutter YouTube 频道上发布的一个视频，解释为什么使用 widget 是更好的做法。
  （特别是使用 `const` 构造的 widget）

[Performance considerations]: {{site.api}}/flutter/widgets/StatefulWidget-class.html#performance-considerations
[source code for `SlideTransition`]: {{site.repo.flutter}}/blob/master/packages/flutter/lib/src/widgets/transitions.dart#L168
[`StatefulWidget`]: {{site.api}}/flutter/widgets/StatefulWidget-class.html
[`StatelessWidget`]: {{site.api}}/flutter/widgets/StatelessWidget-class.html
[`TransitionBuilder`]: {{site.api}}/flutter/widgets/TransitionBuilder.html
[Widgets vs helper methods]: {{site.youtube-site}}/watch?v=IOyq-eTRhvo

### Apply effects only when needed

### 仅当需要的时候才应用效果

Use effects carefully, as they can be expensive.
Some of them invoke `saveLayer()` behind the scenes,
which can be an expensive operation.

由于代价很大，请谨慎使用效果。
一些效果的背后调用了性能代价很大的 `saveLayer()` 方法。

{{site.alert.secondary}}

  <h4><t>Why is savelayer expensive?</t><t>为什么 saveLayer 代价很大？</t></h4><a name="why-is-savelayer-expensive"></a>

  Calling `saveLayer()` allocates an offscreen buffer.
  Drawing content into the offscreen buffer might trigger
  render target switches that are particularly slow in
  older GPUs.

  调用 `saveLayer()` 会开辟一片离屏缓冲区。
  将内容绘制到离屏缓冲区可能会触发渲染目标切换，这些切换在较早期的 GPU 中特别慢。

{{site.alert.end}}

Some general rules when applying specific effects:

一些在使用效果时的通用规则：

* Use the [`Opacity`][] widget only when necessary.
  See the [Transparent image][] section in the Opacity
  API page for an example of applying opacity directly
  to an image, which is faster than using the Opacity
  widget.

  能不用 [`Opacity`][] widget，
  就尽量不要用。有关将透明度直接应用于图像的示例，
  请参见 [Transparent image][]，
  这比使用 Opacity widget 更快。

* **Clipping** doesn’t call `saveLayer()` (unless
  explicitly requested with `Clip.antiAliasWithSaveLayer`)
  so these operations aren’t as expensive as Opacity,
  but clipping is still costly, so use with caution.
  By default, clipping is disabled (`Clip.none`),
  so you must explicitly enable it when needed.

  **Clipping** 不会调用 `saveLayer()`（除非明确使用 `Clip.antiAliasWithSaveLayer`），
  因此这些操作没有 Opacity 那么耗时，但仍然很耗时，所以请谨慎使用。

Other widgets that might trigger `saveLayer()`
and are potentially costly:

其他会触发 `saveLayer()` 的 widget，可能也会代价高昂。

* [`ShaderMask`][]
* [`ColorFilter`][]
* [`Chip`][]&mdash;might cause call to `saveLayer()` if
  `disabledColorAlpha != 0xff`
  
  [`Chip`][]&mdash;
  当 `disabledColorAlpha != 0xff` 的时候，会调用 `saveLayer()`  

* [`Text`][]&mdash;might cause call to `saveLayer()`
  if there's an `overflowShader`
  
  [`Text`][]&mdash;
  当有 `overflowShader` 时，会调用`saveLayer()`

Ways to avoid calls to `saveLayer()`:

避免调用 `saveLayer()` 的方式：

* To implement fading in an image, consider using the
  [`FadeInImage`][] widget,
  which applies a gradual opacity using the GPU’s fragment shader.
  For more information, see the [`Opacity`][] docs.

  要在图像中实现淡入淡出，请考虑使用 [`FadeInImage`][] widget，
  该 widget 使用 GPU 的片段着色器应用渐变不透明度。
  了解更多详情，请参见 [`Opacity`][] 文档。

* To create a rectangle with rounded corners, instead of applying a
  clipping rectangle, consider using the `borderRadius` property offered
  by many of the widget classes.

  要创建带圆角的矩形，而不是应用剪切矩形，
  请考虑使用很多 widget 都提供的 `borderRadius`属性。

[`Chip`]: {{site.api}}/flutter/material/Chip-class.html
[`ColorFilter`]: {{site.api}}/flutter/dart-ui/ColorFilter-class.html
[`FadeInImage`]: {{site.api}}/flutter/widgets/FadeInImage-class.html
[`Opacity`]: {{site.api}}/flutter/widgets/Opacity-class.html
[`ShaderMask`]: {{site.api}}/flutter/widgets/ShaderMask-class.html
[`Text`]: {{site.api}}/flutter/widgets/Text-class.html
[Transparent image]: {{site.api}}/flutter/widgets/Opacity-class.html#transparent-image

### Render grids and lists lazily

### 对列表和网格列表懒加载

Use the lazy methods, with callbacks,
when building large grids or lists.
That way only the visible portion of the
screen is built at startup time.

在构建大型网格或列表时，使用带有回调的惰性方法。
这样，只有屏幕的可见部分是在开始时构建的。

Also see:

请参阅：

* [Working with long lists][] in the [Cookbook][]

  [实用教程][Cookbook] 里的 [长列表的处理][Working with long lists] 文档

* [Creating a ListView that loads one page at a time][]
  a community article by AbdulRahman AlHamali

  来自社区的 AbdulRahman AlHamali 撰写的文章
  [Creating a ListView that loads one page at a time]

* [`Listview.builder`][] API

[Cookbook]: {{site.url}}/cookbook
[Creating a ListView that loads one page at a time]: {{site.medium}}/saugo360/flutter-creating-a-listview-that-loads-one-page-at-a-time-c5c91b6fabd3
[`Listview.builder`]: {{site.api}}/flutter/widgets/ListView/ListView.builder.html
[Working with long lists]: {{site.url}}/cookbook/lists/long-lists

###  Build and display frames in 16ms

### 在 16ms 内渲染完成每一帧

Since there are two separate threads for building
and rendering, you have 16ms for building,
and 16ms for rendering on a 60Hz display.
If latency is a concern,
build and display a frame in 16ms _or less_.
Note that means built in 8ms or less,
and rendered in 8ms or less,
for a total of 16ms or less.
If missing frames (jankyness) is a concern,
then 16ms for each of
the build and render stages is OK.

由于构建和渲染有两个独立的线程，因此构建时间为 16ms，60Hz 显示器上渲染时间为 16ms。
如果需要考虑延迟，就要在 16ms **或更短** 的时间内构建和显示帧。
请注意，这意味着构建需要少于 8ms，渲染也需要少于 8ms，总计 16ms 或更短。
如果需要考虑丢帧（jankyness），那么每个构建和渲染阶段的 16ms 都可以。

If your frames are rendering in well under
16ms total in [profile mode][],
you likely don’t have to worry about performance even if some
performance pitfalls apply,
but you should still aim to build and
render a frame as fast as possible. Why?

如果在 [profile 构建][profile mode] 状态下，
每一帧渲染时间低于 16ms，你可能不必担心性能问题以及一些性能陷阱，
但仍然应该致力于尽可能快地渲染每一帧。为什么？


* Lowering the frame render time below 16ms might not make a visual
  difference, but it **improves battery life** and thermal issues.

  将帧渲染时间降低到 16ms 以下可能在视觉上看不出来什么变化，
  但可以**延长电池寿命**以及避免发热问题。

* It might run fine on your device, but consider performance for the
  lowest device you are targeting.

  可能在你当前测试设备上运行良好，但请考虑在应用所支持的最低端设备上的情况。

* When 120fps devices become widely available,
  you’ll want to render frames in under 8ms (total)
  in order to provide the smoothest experience.

  当 120fps 的设备普及之后，便需要在 8ms 之内完成每一帧的渲染来保证流畅平滑的体验。

If you are wondering why 60fps leads to a smooth visual experience,
see the video [Why 60fps?][]

[profile mode]: {{site.url}}/testing/build-modes#profile
[Why 60fps?]: {{site.youtube-site}}/watch?v=CaMTIgxCSqU

如果你想弄明白为什么 60fps 会带来平滑的视觉体验，
请看视频 [Why 60fps?](https://www.bilibili.com/video/av55811304/)

## Pitfalls

## 陷阱

If you need to tune your app’s performance,
or perhaps the UI isn't as smooth as you expect,
the Flutter plugin for your IDE can help.
In the Flutter Performance window,
enable the **Show widget rebuild information** check box.
This feature helps you detect when frames are
being rendered and displayed in more than 16ms.
Where possible,
the plugin provides a link to a relevant tip.

如果你需要改善应用程序的性能，或者 UI 流畅度没达到你的预期，
那么 IDE 的 Flutter plugin 可以提供帮助。
在 Flutter Performance 窗口中，勾选 **Show widget rebuild information** 复选框。 
此功能可帮助你检测帧的渲染和显示时间是否超过 16ms。
在可能的情况下，插件也会提供指向相关提示的链接。

The following behaviors might negatively impact
your app's performance.

以下行为可能会对您应用的性能产生负面影响。

* Avoid using the `Opacity` widget,
  and particularly avoid it in an animation.
  Use `AnimatedOpacity` or `FadeInImage` instead.
  For more information, see
  [Performance considerations for opacity animation][].

  避免使用 `Opacity` widget，尤其是在动画中避免使用。
  可以使用 `AnimatedOpacity` 或 `FadeInImage` 代替该操作。
  更多信息，请参阅
  [Performance considerations for opacity animation][]。

* When using an AnimatedBuilder,
  avoid putting a subtree in the builder
  function that builds widgets that don’t
  depend on the animation. This subtree is
  rebuilt for every tick of the animation.
  Instead, build that part of the subtree
  once and pass it as a child to
  the AnimatedBuilder. For more information,
  see [Performance optimizations][].

  使用 AnimatedBuilder 时，请避免在不依赖于动画的 widget 的构造方法中构建 widget 树，
  不然，动画的每次变动都会重建这个 widget 树，
  应当将这部分子树作为 child 传递给 AnimatedBuilder，从而只构建一次。
  更多内容，请查看 [这个文档][Performance optimizations]。

* Avoid clipping in an animation.
  If possible, pre-clip the image before animating it.

  避免在动画中剪裁，尽可能的在动画开始之前预先剪裁图像。

* Avoid using constructors with a concrete `List`
  of children (such as `Column()` or `ListView()`)
  if most of the children are not visible
  on screen to avoid the build cost.

  如果大多数 children widget 在屏幕上不可见，
  请避免使用返回具体列表的构造函数
  （例如 `Column()` 或 `ListView()`），以避免构建成本。

## Resources

## 参考资料

For more performance info, see the following resources:

要了解更多性能信息，请参见以下资源：

* [Performance optimizations][] in the AnimatedBuilder API page

  AnimatedBuilder API 页面的 [Performance optimizations][] 部分；

* [Performance considerations for opacity animation][]
  in the Opacity API page

  Opacity API 页面的 [Performance considerations for opacity animation][] 部分；

* [Child elements' lifecycle][] and how to load them efficiently,
  in the ListView API page

  ListView API 页面中 [Child elements' lifecycle][]，以及如何高效加载元素；
  
* [Performance considerations][] of a `StatefulWidget`

  `StatefulWidget` 的 [Performance considerations][] API 文档。
  
  
[Child elements' lifecycle]: {{site.api}}/flutter/widgets/ListView-class.html#child-elements-lifecycle
[Performance optimizations]: {{site.api}}/flutter/widgets/AnimatedBuilder-class.html#performance-optimizations
[Performance considerations for opacity animation]: {{site.api}}/flutter/widgets/Opacity-class.html#performance-considerations-for-opacity-animation