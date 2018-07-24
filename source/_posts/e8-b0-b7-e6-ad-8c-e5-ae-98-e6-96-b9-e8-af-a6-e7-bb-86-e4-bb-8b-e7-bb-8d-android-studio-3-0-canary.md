---
title: 谷歌官方详细介绍 Android Studio 3.0 Canary
url: 510.html
id: 510
categories:
  - Android
date: 2017-05-25 18:36:31
tags:
---

  

由于微信限制外链，如果想看详情图，建议在浏览器中打开

  

借 Google I/O 2017 的东风，Android Studio 3.0 今天就可以在我们的 Canary 发布渠道上下载。Android Studio 是我们专为 Android 而构建的官方 IDE，我们不断加大投入，改进该 IDE。Android Studio 中的功能集侧重于加快您的应用开发流并提供针对 Android 平台构建的最新工具。

  

为加快您的开发流，Android Studio 3.0 包含了三大主要功能：

  

*   一套全新的应用性能分析工具，用于快速诊断性能问题；
    
*   支持 Kotlin 编程语言；
    
*   加快大规模应用项目的 Gradle 构建速度。
    

  

Android Studio 3.0 还紧密集成了 Android 平台开发工具，提供以下附加的重要功能：

  

*   支持免安装应用的开发；
    
*   在 Android O 模拟器系统映像中包含 Google Play 商店；
    
*   以及全新的 Android O 开发向导。
    

  

总而言之，Android Studio 3.0 的第一个 Canary 版本包含 20 多项新功能。

  

我们一直在 Android Studio 2.4 的各个 Canary 版本中默默地引入其中许多功能。直到今天，我们发现，我们已经添加了许多重要功能，并且，为了改善可伸缩性和缩短构建时间，我们必须在 Android Gradle 插件中引入一项罕见的突破性改动，于是，我们将此版本重新编号为 Android Studio 3.0。如果您希望针对 Android O 开发应用，希望创建免安装应用，希望使用 Kotlin 语言开始开发，或者希望使用最新的 Android 应用性能工具来提升应用质量，那么，您应立即下载 Android Studio 3.0 Canary 1。

下面是按主要开发流编排的功能列表，从中可以查阅 Android Studio 3.0 的第一个 Canary 版本中各项新增功能的详细信息。

  

下面是按主要开发流编排的功能列表，从中可以查阅 Android Studio 3.0 的第一个 Canary 版本中各项新增功能的详细信息。

  

**开发**

  

**Kotlin 编程语言**

  

应广大开发者的热切呼声，Android Studio 3.0 现在包含对 Kotlin 的支持。凭借对此新语言的支持，您可以在现有 Android 应用代码的旁边无缝添加 Kotlin 代码，还可访问 Android Studio 中提供的所有优秀开发工具。您可以选择使用 Code → Convert Java File to Kotlin File 中提供的内置转换工具将 Kotlin 添加到您的项目，也可以选择使用 New Project 向导创建启用 Kotlin 的项目。详细了解 Android 和 Android Studio 中的 Kotlin 语言支持。

  

![](https://mmbiz.qpic.cn/mmbiz_jpg/FoiciaVBBCfia78hpwjGA1AleIrxftP6SGT7MhviaCIqJHENE6OyIoobkEp74nZTARGARmdmR1ra0U01jN51EKkgug/640?wx_fmt=jpeg&wxfrom=5&wx_lazy=1)

  

▲ AS 中的 Kotlin 语言转换

  

**Java 8 语言功能**

  

我们继续完善对 Java 8 语言功能和 API 的支持。由于最近弃用 Jack 工具链并迁移到基于 javac 的工具链，对于使用 Java 8 语言功能的项目，您可以在 Android Studio 中访问许多新功能，例如 Instant Run。要更新您的项目以支持全新 Java 8 语言工具链，只需在 Project Structure 对话框中将您的 源代码 和 目标代码 兼容性级别更新至 1.8。

  

![](https://mmbiz.qpic.cn/mmbiz_jpg/FoiciaVBBCfia78hpwjGA1AleIrxftP6SGTBfXwGntEPEGeenSz6deOmcISr9HrOBJ0ft1BUCrJibx4bvuTyZwLXYQ/640?wx_fmt=jpeg&wxfrom=5&wx_lazy=1)

  

▲ 更新 Java 8 语言的 Project Structure 对话框

  

**布局编辑器**

  

在此版本 Android Studio 中，您会发现针对布局编辑器的更多增强功能。我们更新了组件树，提供更好用的拖拽式视图插入功能以及全新的错误面板。为配合对 ConstraintLayout 的更新，布局编辑器还支持创建视图 Barrier 和 Group，并增强了链创建功能。

  

![](https://mmbiz.qpic.cn/mmbiz_jpg/FoiciaVBBCfia78hpwjGA1AleIrxftP6SGT9zwxibQoYIdgpwaV8uibJflBqCBmib35kpvZbQAXhl8MHoOfT2ng8CllQ/640?wx_fmt=jpeg&wxfrom=5&wx_lazy=1)

  

▲ 布局编辑器组件树和警告面板

  

**Adaptive Icon 向导**

  

![](https://mmbiz.qpic.cn/mmbiz_jpg/FoiciaVBBCfia78hpwjGA1AleIrxftP6SGTGTZDVnMwVmCQxAv3meAyzxA4cYIhZPrkMpO4Nq4FEd6Wic9DztsGjtg/640?wx_fmt=jpeg&wxfrom=5&wx_lazy=1)

  

Android O 引入了自适应启动器图标，其可以在不同的 Android 设备上显示为不同的形状。全新 Adaptive Launcher Icon 向导可创建新旧两种版本的启动器图标资源并可预览自适应图标在不同启动器屏幕图标蒙版上的外观。创建新资源的方法是：右键点击项目中的 /res 文件夹，然后导航至 → New → Image Asset → Launcher Icons (Adaptive and Legacy) 。

  

**XML 字体和可下载字体**

  

现在，使用 Android Studio 中的 XML 字体预览和字体选择工具可以为您的应用（针对 Android O 的应用）更轻松地添加自定义字体。您也可以为您的应用创建可下载的字体资源。使用可下载的字体，您将可以在应用中使用自定义字体，同时又不需要在 APK 中捆绑字体资源。要使用可下载字体，请确保您的设备或模拟器运行的是 Google Play 服务 11.2.63 版或更高版本。

  

![](https://mmbiz.qpic.cn/mmbiz_jpg/FoiciaVBBCfia78hpwjGA1AleIrxftP6SGTUTc6R0GscT7OwOga9Zb5rGkiacXrg7Kxq3ZZjm8ic7S7UOYgu75OuURA/640?wx_fmt=jpeg&wxfrom=5&wx_lazy=1)  

▲ 可下载字体资源选取器

  

![](https://mmbiz.qpic.cn/mmbiz_jpg/FoiciaVBBCfia78hpwjGA1AleIrxftP6SGTpbL6ula5QE5icPqSuyhXxB4MKh41Pkf3Sk8kNpCXxDjCZGicxgt2iaYvA/640?wx_fmt=jpeg&wxfrom=5&wx_lazy=1)

  

▲ XML 字体预览

  

**Android Things 支持**

  

借助于 Android Studio 3.0，您可以使用 New Project 向导和 New Module 向导中的一组新模板来开始开发 Android Things。Android Things 允许您将自己的 Android 开发知识拓展到物联网 (IoT) 设备类别。

  

![](https://mmbiz.qpic.cn/mmbiz_jpg/FoiciaVBBCfia78hpwjGA1AleIrxftP6SGTdowe7iaicpUnDGa0uXRqiaDGDHUC8q0lX5pRFugrYicQt1hrM9ZkicGibOWQ/640?wx_fmt=jpeg&wxfrom=5&wx_lazy=1)

  

▲ Android Things New Module 向导

  

**IntelliJ 平台更新：**

  

Android Studio 3.0 Canary 1 包含 IntelliJ 2017.1 版本，其包含 Java 8 语言重构、参数提示、语义突出显示、可拖拽的断点、增强的版本控制搜索等功能。

  

**版本**

  

**免安装应用支持**

  

利用 Android Studio 3.0，您可以在项目中创建免安装应用。免安装应用是一种轻巧的 Android 应用，用户无需安装即可直接运行该应用。为支持免安装应用，Android Studio 引入了两种新模块类型：免安装应用和功能。结合全新的“模块化”重构操作和应用链接助手，Android Studio 可以帮助您将现有应用拓展为免安装应用。为使用此功能，您可以使用 New Module 向导或右键点击某个类并导航至：Refactor → Modularize。

  

![](https://mmbiz.qpic.cn/mmbiz_jpg/FoiciaVBBCfia78hpwjGA1AleIrxftP6SGTcPLLRcPwq0z8KiaFM28n1kwtHSFa3aY6F7ET88QhUqfXYm4XrJf5Qfw/640?wx_fmt=jpeg&wxfrom=5&wx_lazy=1)

  

▲ Instant App Module 向导

  

**构建速度的提升**

  

我们继续努力提升构建速度。在此版本中，我们侧重于提升包含大量模块的项目的构建速度。为提升这些项目的构建速度并支持未来的增强功能， 我们对 Android Studio 所使用的 Android Gradle 插件的 API 做出了颠覆性的改动。如果您依赖于之前版本的插件所提供的 API，您应验证与新插件的兼容性并迁移到适用的 API。在您的 build.gradle 文件中测试和更新插件版本。

  

build.gradle

  

> dependencies{
> 
> classpath'com.android.tools.build:gradle:3.0.0-alpha1'}

  

**Google 的 Maven 存储区**

  

此外，应广大开发者的热切呼声，现在，我们以全新 Maven 存储区的形式在 Android SDK 管理器外部分发 Android 支持库的 Maven 依赖项。对于使用持续集成 (CI) 系统开发的人来说，这样可以简化 Maven 依赖项的管理。结合最新的命令行 SDK 管理器工具和 Gradle 使用时，使用 Google 的 Maven 存储区应该能够简化 CI 构建的管理。要使用全新 Maven 的位置，请在应用模块的 build.gradle 文件中添加以下网址。

  

build.gradle

  

> repositories{
> 
> maven{
> 
> url"https://maven.google.com"
> 
> }}

  

**测试和调试**

  

**Google Play 系统映像**

  

在更新 Android O Beta 版本的同时，我们也更新了 Android Emulator O 系统映像，使之包含 Google Play 商店。捆绑 Google Play 商店让您能够使用 Google Play 端到端地测试应用，同时也方便您在 Android Virtual Device (AVD) 中使 Google Play 服务保持最新状态。就像实际设备上的 Google Play 服务更新一样，您也可以在 AVD 上启用同样的更新。

  

![](https://mmbiz.qpic.cn/mmbiz_jpg/FoiciaVBBCfia78hpwjGA1AleIrxftP6SGT0g1ia0acEFIlSpBgo6O5M7tqI0AaMaECiaCnWGskTDTTscLov5a3BDWg/640?wx_fmt=jpeg&wxfrom=5&wx_lazy=1)

  

▲ Android Emulator 中的 Google Play 商店

  

![](https://mmbiz.qpic.cn/mmbiz_jpg/FoiciaVBBCfia78hpwjGA1AleIrxftP6SGTicQhU3oRNopFgoiaBmkbEpqbGcWj1LveuoU1e9AvhKWREQdSxjto1xEQ/640?wx_fmt=jpeg&wxfrom=5&wx_lazy=1)

  

▲ 更新 Android Emulator 中的 Google Play 服务

  

为确保应用安全性以及与实际设备一致的体验，包含 Google Play 商店的模拟器系统映像已使用发布密钥签名。这意味着您将无法获得高级权限。如果您要求使用高级权限 (root) 来帮助您排查应用问题，您可以使用不包含 Google 应用或服务的 Android 开放源代码项目 (AOSP) 模拟器系统映像。要开始，请确保您使用的是 Android Emulator v26.1+ 和最新的系统映像 API 24+，然后使用设备定义旁边的 Google Play 图标创建一个新 AVD。

  

![](https://mmbiz.qpic.cn/mmbiz_jpg/FoiciaVBBCfia78hpwjGA1AleIrxftP6SGTTqvSuF26u5GPqGKqnJypOUwibYnthBhMlJLoiaeAPpBa6S0bD0rnhUag/640?wx_fmt=jpeg&wxfrom=5&wx_lazy=1)

  

▲ 包含 Google Play 商店支持的 Android Virtual Device 管理器

  

**Android Emulator 中的 OpenGL ES 3.0 支持**

  

我们不断投资，努力给您带来快速开发体验，最新版的 Android Emulator 针对 Android O 系统映像引入了 OpenGL ES 3.0 支持，针对旧版模拟器系统映像，则大幅增强了 OpenGL ES 2.0 的图形性能。在所有操作系统中，大多数最新的显卡均支持 OpenGL ES 2.0 加速。要将 OpenGL ES 3.0 与 Android Emulator 配合使用，开发计算机需要在 Microsoft® Windows® 或 Linux（即将支持 Apple MacOS®）中支持 OpenGL 3.2 或更高版本的主机 GPU 显卡。

  

![](https://mmbiz.qpic.cn/mmbiz_jpg/FoiciaVBBCfia78hpwjGA1AleIrxftP6SGT5jRCm7kOYfCnVTlHVlXCZWJ2RGvDJbLiaFuUricXknZzUHaQBp8NUM1w/640?wx_fmt=jpeg&wxfrom=5&wx_lazy=1)

  

▲ Android Emulator 中的 OpenGL ES 3.0

  

**Android Emulator 中的应用错误报告程序**

  

为帮助记录应用中的错误，我们新增了一种更简便的错误报告生成方法，该报告程序提供所有必要的配置设置以及捕获重现步骤的空间。另外，我们还新增了一个链接，以便您在想要与 Android 团队分享特定模拟器错误时，能够在 Android Issue Tracker 中快速生成错误。要使用此功能，请导航至 Emulator Tool Bar → Extended Controls → Help → Emulator Help → File a Bug。

  

![](https://mmbiz.qpic.cn/mmbiz_jpg/FoiciaVBBCfia78hpwjGA1AleIrxftP6SGTJvicWe4dia7OcaToz4grCiaVrcF6ialiatdLbaicQ3kYzmecF9PPibyJoj8Cw/640?wx_fmt=jpeg&wxfrom=5&wx_lazy=1)

  

▲ Android Emulator 中的应用错误报告

  

**Android 中的代理支持**

  

针对那些需要使用 HTTP 代理访问互联网的用户，我们新增了一个用户界面，可用于管理模拟器所使用的代理设置。现在，默认情况下，Android Emulator 会使用 Android Studio 中的设置，但您可以在您的网络设置中替换这些设置。要进行配置，请导航至 Extended Controls → Settings → Proxy。

  

![](https://mmbiz.qpic.cn/mmbiz_jpg/FoiciaVBBCfia78hpwjGA1AleIrxftP6SGTqQh8MZxMjhkBiaqKNDtOQlicllianRPM83zts4schSrtkeEncK4scffuQ/640?wx_fmt=jpeg&wxfrom=5&wx_lazy=1)

  

▲ Android Emulator 代理设置

  

**Android Emulator 中的 Android Wear 旋转控件**

  

现在，Android Emulator 支持 Android Wear 2.0 模拟器系统映像的旋转控件。现在，对于针对包含旋转输入滚动功能的 Android Wear 设备的应用，测试将更加简单。要启用此功能，请创建针对 Android Wear 的 Emulator AVD，Rotary Input 面板应出现在扩展控件下面。

  

![](https://mmbiz.qpic.cn/mmbiz_jpg/FoiciaVBBCfia78hpwjGA1AleIrxftP6SGTcOx4LaXCjV1fQ7ajFjhKC3tGtVR0KbVrlpTbxjIroTKia5eiapxshyVQ/640?wx_fmt=jpeg&wxfrom=5&wx_lazy=1)

  

▲ Android Emulator 中的旋转输入

  

**APK 调试**

  

现在，针对不想在 Android Studio 中构建项目、只想在其中调试 APK 的开发者，Android Studio 3.0 版本加入了调试任意 APK的功能。对于在其他开发环境编写 Android C++ 代码而想在 Android Studio 环境中调试和分析 APK 的用户而言，此功能尤为有用。只要您有可调试版本的 APK，您就可以使用新的 APK 调试功能来静态分析、动态分析和调试 APK。而且，如果您可以访问 APK 的源代码，您可以将此源代码链接到 APK 调试流，以提高调试流程的保真度。只需在 Android Studio Welcome Screen 中选择 Profile or debug APK或选择File → Profile or debug APK，即可使用此功能。

  

![](https://mmbiz.qpic.cn/mmbiz_jpg/FoiciaVBBCfia78hpwjGA1AleIrxftP6SGT53tz9yiaNz5ngSYKrc9PPsT7THLMCACficYES7OCkWcupHFCm501SaBg/640?wx_fmt=jpeg&wxfrom=5&wx_lazy=1)

  

▲ 分析或调试 APK

  

![](https://mmbiz.qpic.cn/mmbiz_jpg/FoiciaVBBCfia78hpwjGA1AleIrxftP6SGTssSaicDTYlxNc39TOIXicU4RhfWPLTwx7BkHfFbQnFI4A381MumgNIiaw/640?wx_fmt=jpeg&wxfrom=5&wx_lazy=1)

  

▲ APK 调试

  

**布局检查器**

  

您会发现，Android Studio 3.0 中的布局检查器提供几项增强功能，简化了应用布局问题的调试。这几项增强功能包括更好地将属性分组到常用分类中，以及 View Tree 和 Properties 面板中的搜索功能等。在应用运行时，通过 Tools → Android → Layout Inspector 访问布局检查器。

  

![](https://mmbiz.qpic.cn/mmbiz_jpg/FoiciaVBBCfia78hpwjGA1AleIrxftP6SGTZCXkfZ9MicPc5hGoJNdPVTxk7q63AQ0VKUo7nct0mHKqVmolpjAWeoA/640?wx_fmt=jpeg&wxfrom=5&wx_lazy=1)

  

▲ 布局检查器

  

**设备文件浏览器**

  

应广大用户的热切呼声，我们将设备文件浏览器从 DDMS 移植到 Android Studio 中，新的浏览器允许查看 Android 设备或模拟器的文件和目录结构。现在，您在测试应用时，可以直接在 Android Studio 中快速预览和修改应用数据文件。

  

![](https://mmbiz.qpic.cn/mmbiz_jpg/FoiciaVBBCfia78hpwjGA1AleIrxftP6SGTb7Q6LRmiaic5wplJkFdS9OXrEIT6pbjic4XTUkFxY6BpeAydKkRzRO4sw/640?wx_fmt=jpeg&wxfrom=5&wx_lazy=1)

  

▲ 设备文件浏览器

  

**优化工具**

  

**Android 分析器**

  

Android Studio 3.0 包含全新的工具包，以帮助调试应用的性能问题。我们对之前的 Android Monitor 工具集进行彻底重写，代之以 Android 分析器。您将应用部署到正在运行的设备或模拟器后，点击 Android Profiler 标签，即可在实时、统一的视图中访问应用的 CPU、内存和网络活动。每个性能事件映射到 UI 事件时间线中，该时间线突出显示触摸事件、按键和活动变更，以便您更清楚地了解特定事件发生的时间和原因。 点击每个时间线，深入了解应用的性能情况。

  

![](https://mmbiz.qpic.cn/mmbiz_jpg/FoiciaVBBCfia78hpwjGA1AleIrxftP6SGTPGv57vX5Yelx4t7AEcccu1ZdbLpCy1rOUkTrSTiaMhWnbo8n5xDbA3A/640?wx_fmt=jpeg&wxfrom=5&wx_lazy=1)

  

▲ Android 分析器 – 时间线组合视图

  

**CPU 分析器**

  

不必要的 CPU 处理和负载峰值是应用性能不佳的征兆。有了 CPU 分析器，您可以触发一个样本或测试的 CPU 跟踪文件，分析应用的 CPU 线程使用情况。然后，您可以使用 CPU 分析器中内置的各种数据视图和过滤器排查 CPU 性能问题。了解详情。

  

![](https://mmbiz.qpic.cn/mmbiz_jpg/FoiciaVBBCfia78hpwjGA1AleIrxftP6SGTnK0zVCtniag93bibcgf7LvhSdo92YX4Qic5X7TnTOcoIrhU76J47LE9BA/640?wx_fmt=jpeg&wxfrom=5&wx_lazy=1)

  

▲ CPU 分析器

  

**内存分析器**

  

内存使用效率低，可能导致许多设备问题，包括 UI 反应迟钝和内存不足事件等。内存分析器将之前的堆查看器和分配跟踪器的功能集成到一个丰富的界面中，帮助调试应用中的内存使用问题。您可以通过分析内存分配、堆转储等来诊断各种内存问题。

  

![](https://mmbiz.qpic.cn/mmbiz_jpg/FoiciaVBBCfia78hpwjGA1AleIrxftP6SGTEIvJiagw7deCatvyTSibqMgpvIEv6XAEzUX08Ba9Bq1WM1tDB5TBCsRA/640?wx_fmt=jpeg&wxfrom=5&wx_lazy=1)

  

▲ 内存分析器

  

**网络分析器**

  

通过优化应用的前台和后台网络使用情况，可以提高应用性能和减少应用流量消耗。通过网络分析器，您可以监控应用的网络活动，检查每个网络请求的有效负载，链接回生成网络请求的源代码行。现在，网络分析器可与 HttpURLConnection、OkHttp 及 Volley 网络库配合使用。网络分析器是一项高级分析功能，可在 Android O 之前版本的设备和模拟器上启用，方法是：在 Run Configuration 框的 Profiling 标签中选中 Enable Advanced Profiling 。除了启用网络请求和有效负载分析外，此复选框还可以启用最高等级事件收集、内存对象计数和内存垃圾回收。对于基于 Android O 的设备和模拟器，只需部署应用即可。

  

![](https://mmbiz.qpic.cn/mmbiz_jpg/FoiciaVBBCfia78hpwjGA1AleIrxftP6SGTsnyiaUbYU53kxEiaDdLhhsqY7asKibz5czXDBa71aXu7H5S6K91HUBLJA/640?wx_fmt=jpeg&wxfrom=5&wx_lazy=1)

  

▲ 网络分析器

  

![](https://mmbiz.qpic.cn/mmbiz_jpg/FoiciaVBBCfia78hpwjGA1AleIrxftP6SGT9hoyWLQqDl2NqwbGdaP5vwyjFWth30MO4FAzxB4C2ezvw3qlZKbSeA/640?wx_fmt=jpeg&wxfrom=5&wx_lazy=1)

▲ Android O 之前版本的设备中的网络分析器设置

  

**APK 分析器增强功能**

  

在 Android Studio 3.0 中，我们对 APK 分析器新增了一些额外的增强功能，以帮助您进一步减小 APK 的大小。通过此功能更新，您现在可以分析免安装应用的 Zip 文件和 AAR，查看类和方法的 dex 字节码。您还可以生成 Proguard 配置规则和在 dex 查看器中加载 Proguard 映射文件。

  

![](https://mmbiz.qpic.cn/mmbiz_jpg/FoiciaVBBCfia78hpwjGA1AleIrxftP6SGTibpInj9Jc6TpsCNdDQW6SzyOic4icDicOqvCgxgzSUgwtw8z5LBs8yrx7g/640?wx_fmt=jpeg&wxfrom=5&wx_lazy=1)

  

▲ APK 分析器

  

概言之，Android Studio 3.0 Canary 1 包含以下重要的新功能：

  

**开发**

  

*   Kotlin 语言
    
*   Java 8 语言
    
*   布局编辑器增强功能
    
*   Adaptive Icon 向导
    
*   XML 字体和可下载字体
    
*   Android Things
    
*   Intellij 平台更新 2017.1
    

  

**构建**

  

*   免安装应用支持
    
*   构建速度的提升
    
*   Google 的 Maven 存储区变更
    

  

**测试和调试**

  

*   Emulator Google Play 系统映像
    
*   Emulator OpenGL ES 3.0 支持
    
*   Emulator 代理支持
    
*   应用错误报告程序
    
*   Android Wear 旋转输入
    
*   APK 调试
    
*   布局检查器
    
*   设备文件浏览器
    

  

**优化工具**

  

*   CPU 分析器
    
*   内存分析器
    
*   网络分析器
    
*   APK 分析器增强功能
    

  

**入门指南**

  

**下载**

  

如果您使用的是之前版本的 Android Studio，您可以与稳定版并行安装 Android Studio 3.0 Canary 1。您可以从官方 Android Studio 预览版下载页面下载此更新。如本博文所述，为了支持此 IDE 中的一些新功能，对 Gradle Plugin API 做出了一些颠覆性的改动。因此，您也应在当前项目中将 Android Gradle 插件版本更新至 3.0.0-alpha1，测试和验证您的应用项目设置。

  

我们感谢您提供有关您喜欢的特性、存在的问题或希望看到的功能的任何反馈意见。如果您发现错误或问题，欢迎随时向我们提交问题。在我们的 Google+ 信息页或 Twitter 上与我们（Android Studio 开发团队）联系。