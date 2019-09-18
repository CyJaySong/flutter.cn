---
title: Web FAQ
<<<<<<< HEAD
title: Web 常见问题
description: Some gotchas and differences when writing or running web apps in Flutter.
description: 在 Flutter 中编写或运行 Web 应用程序时遇到的一些问题，以及 Web 与不同之处。
=======
description: Some gotchas and differences when writing or running web apps in Flutter.
>>>>>>> d056f25fa99ed9f59c39ad841fa94c21b22ffa03
---

### Is the web version of Flutter ready for production?

<<<<<<< HEAD
### Web 版本的 Flutter 是否已经准备好投入生产环境中了呢？

As of the 1.9 release, the web version of Flutter is available as
a technical preview on the master/(dev by Sept?) channel.
=======
As of the 1.9 release, the web version of Flutter
is available as a technical preview on the master channel.
>>>>>>> d056f25fa99ed9f59c39ad841fa94c21b22ffa03
Web support has been added to the main Flutter repo,
but is missing features and has known performance issues.
**We don't recommend deploying a web app to production.**

<<<<<<< HEAD
截至 1.9 版本，Web 版本 Flutter 可以
在 master channel（dev 可能会是 9 月）上作为技术预览版本。
Web 支持已经被添加到 Flutter 的主仓库中，
但还缺少一些特性，且存在已知的性能问题。
**我们不建议将 Web 应用部署到生产环境中**

### How do I create an app that also runs on the web?

### 如何创建同时在 Web 上运行的应用？
=======
### How do I migrate a web app built using the flutter_web repo to the flutter repo?

See [Upgrading from package:flutter_web
to the Flutter SDK][].

### How do I create an app that also runs on the web?
>>>>>>> d056f25fa99ed9f59c39ad841fa94c21b22ffa03

See [building a web app with
Flutter](/docs/get-started/web).

<<<<<<< HEAD
请参见 [使用 Flutter 构建 Web 应用](/docs/get-started/web)。

### Does hot reload work with a web app?

### Web 应用上能热重载吗？

No.

不能。

### Can I use Flutter plugins?

### 我可以使用 Flutter plugin 吗？

Not yet.

还不能。

### How do I file an issue about web support?

### 我该如何提交关于 Web 支持的 issue？

=======
### Does hot reload work with a web app?

No.

### Can I use Flutter plugins?

Not yet.

### How do I file an issue about web support?

>>>>>>> d056f25fa99ed9f59c39ad841fa94c21b22ffa03
You can [file an issue](https://goo.gle/flutter_web_issue)
on the main Flutter repo. Make sure that "web" is
included in the title.

<<<<<<< HEAD
你可以在 Flutter 主仓库中
[提交一个 issue](https://goo.gle/flutter_web_issue)。
确保标题中包含 “Web” 字样。

### How do I refresh the app running in the browser?

### 我该如何在浏览器中刷新正在运行的应用？

Using the browser's refresh button doesn't work,
but you can enter "R" in the console where
"flutter run -d chrome" is running.

使用浏览器的刷新按钮不会起作用，
但你可以在执行 “flutter run -d chrome” 的控制台中输入“R” 进行刷新。

### Can I build, run, and deploy web apps in any of the IDEs?

### 我可以在任意 IDE 中，构建、运行并发布 web 应用吗？

You can select **Chrome** as the target device in
Android Studio/IntelliJ and VS Code if you are using the
master channel of Flutter and have enabled web support.
To enable support, run the following in the terminal:

如果你正在使用 master channel 的 Flutter，并已启用 web 支持，
你可以在 Android Studio、IntelliJ 和 VS Code 中选择 **Chrome** 作为目标设备。
要启用 web 支持，请在终端中执行以下命令：

=======
### How do I refresh the app running in the browser?

You can either use the browser's refresh button,
or you can enter "R" in the console where
"flutter run -d chrome" is running.

### Can I build, run, and deploy web apps in any of the IDEs?

You can select **Chrome** as the target device in
Android Studio/IntelliJ and VS Code if you are using the
latest master channel of Flutter and have enabled web support.
To enable support, run the following in the terminal:

>>>>>>> d056f25fa99ed9f59c39ad841fa94c21b22ffa03
```terminal
flutter config --enable-web
```

You need only run this once.
<<<<<<< HEAD

你仅需要执行一次。
=======
This command modifies (or creates) the
`~/.flutter_settings` file (on Mac/Linux)
with the following:

```shell
{
  "enable-web": true
}
```
>>>>>>> d056f25fa99ed9f59c39ad841fa94c21b22ffa03

If the IDE is already running, restart it.
The device pulldown should now include the **Chrome (web)**
option.

<<<<<<< HEAD
如果 IDE 已在运行，请重新启动它。
设备列表菜单现在应该包含 **Chrome（web）** 选项了。

### How do I build a responsive app for the web?

### 我该如何构建响应式 web 应用？
=======
### What version of the IDE and corresponding Flutter plugin is required?

Because web support is in active development,
we recommend that you use the latest version of
your IDE, and the latest version of the Flutter
and Dart plugins.

### How do I build a responsive app for the web?
>>>>>>> d056f25fa99ed9f59c39ad841fa94c21b22ffa03

See [Creating responsive
apps](/docs/development/ui/layout/responsive).

<<<<<<< HEAD
请参阅[创建响应式应用](/docs/development/ui/layout/responsive)。

## Can I use the `dart:io` package with a web app?

## 我该如何在 web 应用中使用 `dart:io` 这个 package？

不行。文件系统在浏览器中是无法访问的。
对于网络功能来说，请使用 [`http`][] package。
请注意，安全方面的工作有所不同，因为浏览器（而不是应用程序）控制 HTTP 请求上的标头。

## How are forward and backward buttons presented in the web UI?

## 如何在 Web 用户界面中显示前进和后退按钮？
=======
### Can I use the `dart:io` package with a web app?

No. The file system is not accessible from the browser.
For network functionality, use the [`http`][]
package. Note that security works somewhat
differently because the browser (and not the app)
controls the headers on an HTTP request.

### How are forward and backward buttons presented in the web UI?
>>>>>>> d056f25fa99ed9f59c39ad841fa94c21b22ffa03

The browser's back button is supported for web apps.
The forward button is not yet enabled.
For more information, see [Issue 32248][].

<<<<<<< HEAD
Web 应用支持浏览器的后退按钮。
前进按钮尚未支持。
有关更多信息，请参阅 [Issue 32248][]。

## How do copy/paste work?

## 复制和粘贴功能状况如何？
=======
### How do copy/paste work?
>>>>>>> d056f25fa99ed9f59c39ad841fa94c21b22ffa03

Copy/paste works on mobile. If you encounter problems,
please [file an issue][].

<<<<<<< HEAD
在移动设备上复制和粘贴可以使用。
如果你遇到了什么问题，请 [提一个 issue][]。

## How do I embed a web app in a web page?

## 我该如何把一个 web 应用嵌入到一个网页中？

IDK.

尚未有明确的解决方案。

## Implementing CORS

## 实现跨域资源共享

HTTP requests work on mobile, but not on the web.
=======
### How do I embed a Flutter web app in a web page?

You can embed a Flutter web app,
as you would embed other content,
in an [`iframe`][] tag of an HTML file.
In the following example, replace "URL"
with the location of your hosted HTML page:

```html
<iframe src="URL"></iframe>
```

### Implementing CORS

>>>>>>> d056f25fa99ed9f59c39ad841fa94c21b22ffa03
Web applications have special security restrictions.
If you experience problems,
check that the web server you are accessing is setting
CORS headers that accept requests from the domain
hosting your Flutter app.

<<<<<<< HEAD
HTTP 请求适用于移动设备，但不适用于网络。
Web 应用程序有特殊的安全限制。
如果你遇到问题，请检查正在访问的 Web
服务器是否设置了接受来自托管 Flutter 应用程序的域的请求的 CORS 标头。

## How do I avoid JS lockout with CORS?

## 我该如何使用 CORS 避免 JS 锁定？

IDK

尚未有明确的解决方案。

### How do I debug a web app?

### 我该如何调试一个 web 应用？

Use [Flutter DevTools][] for the following tasks:

使用 [Flutter DevTools][] 来尝试如下工作：

* [Debugging][]

  [调试][]
  
* [Logging][]

  [查看日志][]
  
* [Running Flutter inspector][]

  [运行 Flutter inspector][]

Use [Chrome DevTools][] for the following tasks:

使用 [Chrome DevTools][] 来尝试如下工作：

* [Generating event timeline][]

  [生成事件的时间线][]
  
* [Analyzing performance][]&mdash;make sure to use a
  profile build
  
  [分析性能][]&mdash;请确保应用使用的是 profile 构建

### How do I test a web app?

### 我该如何测试 Web 应用？

Use the normal [widget tests][].

跟常规的 [widget tests][] 通用。

Driver tests are not yet supported.

自动化测试尚不支持。

### How do I internationalize a web app?

### 我该如何国际化 Web 应用？

TBD

暂未完成

## How do I deploy a web app?

## 我该如何部署 Web 应用？
=======
### How do I avoid JavaScript lockout with CORS?

TBD

### How do I debug a web app?

Use [Flutter DevTools][] for the following tasks:

* [Debugging][]
* [Logging][]
* [Running Flutter inspector][]

Use [Chrome DevTools][] for the following tasks:

* [Generating event timeline][]
* [Analyzing performance][]&mdash;make sure to use a
  profile build

### How do I test a web app?

Use the normal [widget tests][].

Driver tests are not yet supported.

### How do I internationalize a web app?

TBD

### How do I deploy a web app?
>>>>>>> d056f25fa99ed9f59c39ad841fa94c21b22ffa03

See [Preparing a web app for
release](/docs/deployment/web).

<<<<<<< HEAD
请参阅 [打包并发布到 Web 平台](/docs/deployment/web)
=======
### Does `Platform.is` work on the web?

Not currently. 

### How can I compare notes others who are playing with this feature?

Check out the **#web** discussion board on [Discord][].
Flutter engineers routinely read and respond on Discord.


>>>>>>> d056f25fa99ed9f59c39ad841fa94c21b22ffa03

[Analyzing performance]: https://developers.google.com/web/tools/chrome-devtools/evaluate-performance
[Chrome DevTools]: https://developers.google.com/web/tools/chrome-devtools
[Debugging]: /docs/development/tools/devtools/debugger
<<<<<<< HEAD
=======
[Discord]: https://discord.gg/N7Yshp4
>>>>>>> d056f25fa99ed9f59c39ad841fa94c21b22ffa03
[file an issue]: https://goo.gle/flutter_web_issue
[Flutter DevTools]: /docs/development/tools/devtools/overview
[Generating event timeline]: https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/performance-reference
[`http`]: https://pub.dev/packages/http
<<<<<<< HEAD
[Issue 32248]: {{site.github}}/flutter/flutter/issues/32248
[Logging]: /docs/development/tools/devtools/logging
[Running Flutter inspector]: /docs/development/tools/devtools/inspector
=======
[`iframe`]: https://html.com/tags/iframe/
[Issue 32248]: {{site.github}}/flutter/flutter/issues/32248
[Logging]: /docs/development/tools/devtools/logging
[Running Flutter inspector]: /docs/development/tools/devtools/inspector
[Upgrading from package:flutter_web to the Flutter SDK]: https://github.com/flutter/flutter/wiki/Upgrading-from-package:flutter_web-to-the-Flutter-SDK
>>>>>>> d056f25fa99ed9f59c39ad841fa94c21b22ffa03
[widget tests]: /docs/testing#widget-tests