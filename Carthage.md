# Carthage

[TOC]

github简介：
[Github readme](https://github.com/Carthage/Carthage)  
[readme翻译——Carthage：去中心化的Cocoa依赖管理器](http://www.cocoachina.com/ios/20141204/10528.html)

raywenderlich上的文章和翻译：
[Carthage Tutorial: Getting Started](https://www.raywenderlich.com/109330/carthage-tutorial-getting-started)  
[Carthage的安装和使用](http://www.jianshu.com/p/a734be794019)

Other：
[Carthage 包管理工具，另一种敏捷轻快的 iOS & MAC 开发体验](https://swiftcafe.io/2015/10/25/swift-daily-carthage-package/)

### Carthage与CocoaPods的区别

#### CocoaPods特点

1. 为提高第三方开源库的可见性和参与度，创建一个更中心化的生态系统。
2. CocoaPods默认会自动创建并更新你的应用程序和所有依赖的Xcode workspace。
3. CocoaPods项目同时还必须包含一个podspec文件，里面是项目的一些元数据，以及确定项目的编译方式。

#### Carthage特点

1. Carthage编译你的依赖，并提供框架的二进制文件，但你仍然保留对项目的结构和设置的完整控制。Carthage不会自动的修改你的项目文件或编译设置。
2. Carthage使用xcodebuild来编译框架的二进制文件，但如何集成它们将交由用户自己判断。CocoaPods的方法更易于使用，但Carthage更灵活并且是非侵入性的。
3. Carthage创建的是去中心化的依赖管理器。它没有总项目的列表，这能够减少维护工作并且避免任何中心化带来的问题（如中央服务器宕机）。不过，这样也有一些缺点，就是项目的发现将更困难，用户将依赖于Github的趋势页面或者类似的代码库来寻找项目。
4. 最后，我们创建Carthage的原因是想要一种尽可能简单的工具——一个只关心本职工作的依赖管理器，而不是取代部分Xcode的功能，或者需要让框架作者做一些额外的工作。

### Carthage使用注意事项

关于Carthage的安装与使用就不多说了，这里主要说说bootstrap与update的区别以及部分参数的使用。

#### bootstrap & update

> After you’ve finished the above steps and pushed your changes, other users of the project only need to fetch the repository and run carthage `bootstrap` to get started with the frameworks you’ve added.

这里提到，当你更新了某个三方库的使用后，其他人只需运行`bootstrap`即可。也就是说，运行`bootstrap`会拉取Cartfile.resolved中各框架当前的commit。
而`update`的使用则会读取Cartfile中各框架指定拉取的分支，并更新Catfile.resolved。

所以，如果你想要使用当前已拉取好的三方库，则使用`bootstrap`，如果想要尝试更新三方库，则使用`update`。

#### 参数

--cache-builds 使用可用缓存编译 
--use-submodules  

--no-skip-current
分享你的 Xcode schemes
Carthage 只构建从 .xcodeproj 分享出来的 Xcode schemes。可以通过运行 carthage build --no-skip-current 来检测所有的 intended schemes 是否构建成功，然后检查 Carthage/Build 文件夹。
如果运行命令的时候，一个重要的 scheme 没有构建成功，打开 Xcode 确保 scheme is marked as “Shared” ，这样 Carthage 可以发现它。

--no-use-binaries  
--verbose 显示运行信息



