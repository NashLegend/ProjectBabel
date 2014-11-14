Android 5.0 APIs
============

*译自 http://developer.android.com/intl/zh-cn/about/versions/android-5.0.html —— By [NashLegend](https://github.com/NashLegend)*

### API Level: 21 ###

Android 5.0 ([LOLLIPOP](http://developer.android.com/intl/zh-cn/reference/android/os/Build.VERSION_CODES.html#LOLLIPOP)) 为用户和开发人员提供了一些新特性，这篇文章将重点介绍一些值得注意的新增API。

如果你已经发布了一款app，请查看这里 [Android 5.0 系统行为变化](https://github.com/NashLegend/ProjectBabel/blob/master/Android%205.0%20Changes.md) 以适配你的app. 在Android5.0上，即使你没有使用最新API或者新功能，这些新的系统行为仍可能会影响你的app。

如果想看一些新平台的更高级的特性，请看[这里](http://developer.android.com/intl/zh-cn/about/versions/lollipop.html)

### 开始开发 ###

要为Android 5.0开发app，请先使用SDK Mnager下载最新的SDK和系统镜像。

### Update your target API level ###

为使得你的app在Android获得更好的表现，请将你的targetSdkVersion设置成21。调用最新的Android 5.0 API的时候要注意在调用前判断系统版本号以兼容之前的系统版本。不能使用低于minSdkVersion的API。详见[Android后向兼容性](http://developer.android.com/training/basics/supporting-devices/platforms.html)

欲知更多有关API级别的事儿，看这里：[啥是API级别](http://developer.android.com/guide/topics/manifest/uses-sdk-element.html#ApiLevels)

## 用户界面 ##

### Material design 支持 ###

Android 5.0 新增了material design样式的支持. 你可以通过material design创建具有自然的动态效果和过渡风格的app. 系统支持包括以下方面:

- The material theme
- 系统自带Material design主题
- 组件阴影
- RecyclerView组件以取代ListView
- *Drawable动画和样式效果*。（这里应该是指Ripple Drawable）（Drawable animation and styling effects）
- Material design风格的动画和activity过渡效果
- *基于组件状态的Animator*。（Animators for view properties based on the state of the view）
- 可定制的UI组件和工具栏（这里指的应该是ToolBar）
- 基于XML的矢量动画和图形（Animated and non-animated drawables）

欲知更多有关Material Design的事儿，看[这里](http://developer.android.com/training/material/index.html)。

### “最近运行”界面上的多开的文档和activity（相当于MFC等的多文档） ###

以前的版本中，“[最近运行](http://developer.android.com/intl/zh-cn/guide/components/recents.html)”界面对于一个app来说只能显示用户最近交互过的一个task。现在你的应用可以打开更多task以同时打开不同的文档。这种新的多任务特性可以让用户在最近运行界面中快速在activity们和打开的文档们之间任意切换。有可能使用这种并发任务的情景示例：浏览器标签多开、看比赛多开、生产力工具（比如Word、PPT等）文档多开、多窗口与多个妹子聊天等等。你的app可以通过[ActivityManager.AppTask](http://developer.android.com/reference/android/app/ActivityManager.AppTask.html)来管理这些task。

要让系统把你的activity当成一个新的task,在startActivity()的时候使用[FLAG_ACTIVITY_NEW_DOCUMENT](http://developer.android.com/reference/android/content/Intent.html#FLAG_ACTIVITY_NEW_DOCUMENT)，你也可以在manifest文件中把activity的```documentLaunchMode```属性设置成```"intoExisting"``` 或者 ```"always"```来实现这一点。

为了避免“最近运行”界面太多太乱，你可以设置你的app可以显示在此界面上的最大任务数量——设置manifest文件中<application> 的属性```android:maxRecents```，目前的最大数量是每个用户50个，RAM较小的手机则为25个。

最近运行界面上的task可以设置为*重启时常驻*（persist across reboots），可以设置[android:persistableMode](http://developer.android.com/reference/android/R.attr.html#persistableMode)属性以控制常驻行为。你也可以通过[setTaskDescription()](http://developer.android.com/reference/android/app/Activity.html#setTaskDescription(android.app.ActivityManager.TaskDescription))方法修改activity在最近运行界面上的颜色、标签和图标等可见元素。

### WebView 更新 ###

Android 5.0的WebView升级到了Chromium M37，修复了诸多bug以及带来了安全和稳定性的加强，默认的user-agent也已经升级到了37.0.0.0。

新的WebView引入了[PermissionRequest](http://developer.android.com/reference/android/webkit/PermissionRequest.html)类，可以允许你的app通过类似[getUserMedia()](https://developer.mozilla.org/en-US/docs/NavigatorUserMedia.getUserMedia)赋予WebView摄像头和麦克风的权限——当然前提是你的app也有相应的权限。

使用最新的[onShowFileChooser()](http://developer.android.com/reference/android/webkit/WebChromeClient.html#onShowFileChooser(android.webkit.WebView, android.webkit.ValueCallback<android.net.Uri[]>, android.webkit.WebChromeClient.FileChooserParams))方法，你可以通过一个input选择设备里的图片和文件了。

此外，新的WebView还带来了对WebAudio，WebGL，WebRTC的支持。欲知更多WebView的新特性，请看[这里](https://developer.chrome.com/multidevice/webview/overview)。

### 屏幕捕获和分享 ###

Android 5.0 lets you add screen capturing and screen sharing capabilities to your app with the new android.media.projection APIs. This functionality is useful, for example, if you want to enable screen sharing in a video conferencing app.

Android 5.0新增[android.media.projection](http://developer.android.com/reference/android/media/projection/package-summary.html) API以让你拥有捕获和屏幕分享功能。举个例子，如果你要在视频会议app中添加屏幕分享功能的话，就可以使用这个功能。

新的 [createVirtualDisplay()](http://developer.android.com/reference/android/media/projection/MediaProjection.html#createVirtualDisplay(java.lang.String, int, int, int, int, android.view.Surface, android.hardware.display.VirtualDisplay.Callback, android.os.Handler)) 方法 允许你的app将主屏幕内容(the default display)捕获到一个Surface对象上，这样你的app就可以通过网络对此进行分享。这个API只允许捕获非敏感屏幕内容，不能捕获声音。要进行屏幕捕获，你的app必须要先发起一个对话框请求用户同意，此请求通过发送[createScreenCaptureIntent()](http://developer.android.com/reference/android/media/projection/MediaProjectionManager.html#createScreenCaptureIntent()) 方法产生的Intent实现。

你可以查看示例项目的```MediaProjectionDemo```来学习如何使用新的API。（注：在SDK Manager里下载）。


## 通知 ##

### 锁屏通知 ###

从Android 5.0开始可以在锁屏界面上显示通知。用户可以通过设置选择是否允许敏感通知内容显示在安全锁屏界面（secure lock screen）上。

你的应用可以控制通知内容的具体显示级别，通过调用[setVisibility()](http://developer.android.com/reference/android/app/Notification.Builder.html#setVisibility(int))方法传入下面值中的一个：

- VISIBILITY_PRIVATE: 显示基本信息，比如说icon，但是隐藏具体内容。
- VISIBILITY_PUBLIC: 显示通知的所有内容.
- VISIBILITY_SECRET: 不显示任何东西，icon也不显示.

如果你设置的是VISIBILITY_PRIVATE，你可以设置显示敏感内容的替代信息，比如“收到了3条QQ消息”，但是不显示具体消息的联系人。要提供这种显示，首先用Notification.Builder创建一个替代通知。当创建private通知的时候，通过 [setPublicVersion()](http://developer.android.com/reference/android/app/Notification.Builder.html#setPublicVersion(android.app.Notification)) 方法将这个替代通知关联到这个隐私通知上。

### Notifications 元数据 ###

Android 5.0通过关联在你的通知上的元数据对通知进行智能排序。你可以通过Notification.Builder的下面这些方法设置这些元数据：

- [setCategory()](http://developer.android.com/reference/android/app/Notification.Builder.html#setCategory(java.lang.String)): 告诉系统当设备处于*优先*模式（比如这个通知表明来电、即时消息或者闹钟）时如何处理通知。
- [setPriority()](http://developer.android.com/reference/android/app/Notification.Builder.html#setPriority(int)): 标记此通知的重要程度——是否比普通通知要高或者低。拥有[PRIORITY_MAX](http://developer.android.com/reference/android/app/Notification.html#PRIORITY_MAX) 或者 [PRIORITY_HIGH](http://developer.android.com/reference/android/app/Notification.html#PRIORITY_HIGH) 级别的通知在**有声音或者振动**的情况下，会弹出一个浮动窗口。
- [addPerson()](http://developer.android.com/reference/android/app/Notification.Builder.html#addPerson(java.lang.String)): 允许你添加一个或者多个与此通知相关联的人。这样系统可以根据不同的人把通知分开，并按人物重要性排序。

## 图形 ##

### 对OpenGL ES 3.1的支持 ###

Android 5.0为OpenGL ES 3.1增加java接口和native支持。3.1重要的新增功能包括：

(*正面几条不懂，找的别人的翻译*)

- 计算着色器(Compute Shaders)
- 独立的着色器对象
- 间接呼叫指令
- 多重采样和模版纹理
- 着色语言改进
- 高级混合模式和调试扩展。
- 对OpenGL ES 2.0 和 3.0和后向兼容性

OpenGL ES 3.1 的java接口是[GLES31](http://developer.android.com/reference/android/opengl/GLES31.html)。使用OpenGL ES 3.1的时候，请在manifest里面使用<uses-feature>标签及```android:glEsVersion```属性声明之，例如：

```
<manifest>
    <uses-feature android:glEsVersion="0x00030001" />
    ...
</manifest>
```

欲知更多OpenGL ES的信息，包括设备对OpenGL支持的版本，请看[OpenGL ES指南](http://developer.android.com/guide/topics/graphics/opengl.html)。

### Android 扩展包 ###

除了OpenGL ES 3.1，这个版本还提供了拥有java接口和native支持的扩展包以提供高级图形功能。这个扩展包作为一个独立的包发布

In addition to OpenGL ES 3.1, this release provides an extension pack with Java interfaces and native support for advanced graphics functionality. These extensions are treated as a single package by Android. (If the ANDROID_extension_pack_es31a extension is present, your app can assume all extensions in the package are present and enable the shading language features with a single #extension statement.)

扩展包支持:

Guaranteed fragment shader support for shader storage buffers, images, and atomics (Fragment shader support is optional in OpenGL ES 3.1.)
Tessellation and geometry shaders
ASTC (LDR) texture compression format
Per-sample interpolation and shading
Different blend modes for each color attachment in a frame buffer
The Java interface for the extension pack is provided with GLES31Ext. In your app manifest, you can declare that your app must be installed only on devices that support the extension pack. For example:

```
<manifest>
    <uses-feature android:name=“android.hardware.opengles.aep”
        android:required="true" />
    ...
</manifest>
```