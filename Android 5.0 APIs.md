Android 5.0 APIs
============

*译自 http://developer.android.com/intl/zh-cn/about/versions/android-5.0.html —— By [NashLegend](https://github.com/NashLegend)*

Sample示例在这里找：https://github.com/googlesamples/

### API Level: 21 ###

Android 5.0 ([LOLLIPOP](http://developer.android.com/intl/zh-cn/reference/android/os/Build.VERSION_CODES.html#LOLLIPOP)) 为用户和开发人员提供了一些新特性，这篇文章将重点介绍一些值得注意的新增API。

如果你已经发布了一款app，请查看这里 [Android 5.0 系统行为变化](https://github.com/NashLegend/ProjectBabel/blob/master/Android%205.0%20Changes.md) 以适配你的app. 在Android5.0上，即使你没有使用最新API或者新功能，这些新的系统行为仍可能会影响你的app。

如果想看一些新平台的更高级的特性，请看[这里](http://developer.android.com/intl/zh-cn/about/versions/lollipop.html)

### 开始开发 ###

要为Android 5.0开发app，请先使用SDK Mnager下载最新的SDK和系统镜像。

### 升级你的target API ###

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

Android 5.0新增[android.media.projection](http://developer.android.com/reference/android/media/projection/package-summary.html) API以让你拥有捕获和屏幕分享功能。举个例子，如果你要在视频会议app中添加屏幕分享功能的话，就可以使用这个功能。

新的 [createVirtualDisplay()](http://developer.android.com/reference/android/media/projection/MediaProjection.html#createVirtualDisplay(java.lang.String, int, int, int, int, android.view.Surface, android.hardware.display.VirtualDisplay.Callback, android.os.Handler)) 方法 允许你的app将主屏幕内容(the default display)捕获到一个Surface对象上，这样你的app就可以通过网络对此进行分享。这个API只允许捕获非敏感屏幕内容，不能捕获声音。要进行屏幕捕获，你的app必须要先发起一个对话框请求用户同意，此请求通过发送[createScreenCaptureIntent()](http://developer.android.com/reference/android/media/projection/MediaProjectionManager.html#createScreenCaptureIntent()) 方法产生的Intent实现。

你可以查看示例项目的```MediaProjectionDemo```来学习如何使用新的API。


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

- [setCategory()](http://developer.android.com/reference/android/app/Notification.Builder.html#setCategory(java.lang.String)): 告诉系统当设备处于*优先模式*（比如这个通知表明来电、即时消息或者闹钟）时如何处理通知。
- [setPriority()](http://developer.android.com/reference/android/app/Notification.Builder.html#setPriority(int)): 标记此通知的重要程度——是否比普通通知要高或者低。拥有[PRIORITY_MAX](http://developer.android.com/reference/android/app/Notification.html#PRIORITY_MAX) 或者 [PRIORITY_HIGH](http://developer.android.com/reference/android/app/Notification.html#PRIORITY_HIGH) 级别的通知在**有声音或者振动**的情况下，会弹出一个浮动窗口。
- [addPerson()](http://developer.android.com/reference/android/app/Notification.Builder.html#addPerson(java.lang.String)): 允许你添加一个或者多个与此通知相关联的人。这样系统可以根据不同的人把通知分开，并按人物重要性排序。

## 图形 ##

### 对OpenGL ES 3.1的支持 ###

Android 5.0为OpenGL ES 3.1增加java接口和native支持。3.1重要的新增功能包括：

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

扩展包支持:

*这块儿不懂*

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

## 媒体 ##

### 高级相机功能的相机API ###

Android 5.0引入了新的[android.hardware.camera2](http://developer.android.com/reference/android/hardware/camera2/package-summary.html) API以帮助fine-grain照片捕捉和图像处理，你可以编程的方式通过调用[getCameraIdList()](http://developer.android.com/reference/android/hardware/camera2/CameraManager.html#getCameraIdList()) 获取系统的可用相机设备列表并通过。你可以通过 [openCamera()](http://developer.android.com/reference/android/hardware/camera2/CameraManager.html#openCamera(java.lang.String, android.hardware.camera2.CameraDevice.StateCallback, android.os.Handler)) 方法指定其中一个相机设备。要捕捉图像，创建一个[CameraCaptureSession](http://developer.android.com/reference/android/hardware/camera2/CameraCaptureSession.html)并将捕获到的图像绘制到一个Surface对象上。 CameraCaptureSession可设置为单拍或者一次性连拍多张（take single shots or multiple images in a burst）。

需要继承[CameraCaptureSession.CaptureCallback](http://developer.android.com/reference/android/hardware/camera2/CameraCaptureSession.CaptureCallback.html)类并设置到图像捕获请求里以获得图像捕获完成事件。当系统完成图像捕获的时候，CameraCaptureSession.CaptureCallback将接到一个[onCaptureCompleted()](http://developer.android.com/reference/android/hardware/camera2/CameraCaptureSession.CaptureCallback.html#onCaptureCompleted(android.hardware.camera2.CameraCaptureSession, android.hardware.camera2.CaptureRequest, android.hardware.camera2.TotalCaptureResult))回调，返回给你一个包含图像元数据的 [CaptureResult](http://developer.android.com/reference/android/hardware/camera2/CaptureResult.html)。

[CameraCharacteristics](http://developer.android.com/reference/android/hardware/camera2/CameraCharacteristics.html)类可以让你的app检查此设备的相机支持哪些特性。此对象的[INFO_SUPPORTED_HARDWARE_LEVEL](http://developer.android.com/reference/android/hardware/camera2/CameraCharacteristics.html#INFO_SUPPORTED_HARDWARE_LEVEL)属性表示相机功能级别。

- 所有的设备至少可达到[INFO_SUPPORTED_HARDWARE_LEVEL_LEGACY](http://developer.android.com/reference/android/hardware/camera2/CameraMetadata.html#INFO_SUPPORTED_HARDWARE_LEVEL_LEGACY)级别的硬件支持，此级别功能大致相当于已弃用的[Camera](http://developer.android.com/reference/android/hardware/Camera.html) API（*注：此API在API21开始弃用*）。
- 达到[INFO_SUPPORTED_HARDWARE_LEVEL_FULL](http://developer.android.com/reference/android/hardware/camera2/CameraMetadata.html#INFO_SUPPORTED_HARDWARE_LEVEL_FULL)级别硬件支持的设备可以手动控制图像的捕捉和后期处理以及以高帧频捕获高分辨率的图像。

要查看如何使用最新的camera2 API，请查看SDK示例中的```Camera2Basic``` 和 ```Camera2Video```

### 音频回放 ###

此版本包含[AudioTrack](http://developer.android.com/reference/android/media/AudioTrack.html)的以下变化：

- 你的app现在可以用浮点格式([ENCODING_PCM_FLOAT](http://developer.android.com/reference/android/media/AudioFormat.html#ENCODING_PCM_FLOAT))提供音频数据。可以获得更大的动态范围，more consistent precision和greater headroom。浮点运算在*中间值计算*（intermediate calculation）的时候尤其有用。Playback endpoints use integer format for audio data, and with lower bit depth. (In Android 5.0, portions of the internal pipeline are not yet floating point.)
- 你现在可以ByteBuffer方式提供音频数据，就像提供给[MediaCodec](http://developer.android.com/reference/android/media/MediaCodec.html)的数据一样。
- [WRITE_NON_BLOCKING](http://developer.android.com/reference/android/media/AudioTrack.html#WRITE_NON_BLOCKING)模式可以帮助某些app简化缓冲和多线程工作（simplify buffering and multithreading）。

### 媒体播放控制 ###

使用新的通知和媒体API以确保系统UI知道你的媒体播放情况并提取和显示专辑信息。使用新的[MediaSession](http://developer.android.com/reference/android/media/session/MediaSession.html) 和 [MediaController](http://developer.android.com/reference/android/media/session/MediaController.html)类可使得通过UI和service控制播放变得更加简单。

新的MediaSession类取代了已弃用的[RemoteControlClient](http://developer.android.com/reference/android/media/RemoteControlClient.html)，它提供一套回调方法以处理*各种播放行为(差不多这么翻译吧，无非是快进快退暂停以及其他控制等等)* （transport controls and media buttons）。如果你的app提供媒体播放功能并且运行在Android TV或者Wear平台上，也可以通过MediaSession类使用相同的回调方法处理*播放行为*（transport controls）。

现在你可以使用[MediaController](http://developer.android.com/reference/android/media/session/MediaController.html)类创建自己的媒体控制器app。这个类提供了一个线程安全的方式以在你的UI线程上监控和控制媒体的播放行为。创建控制器的时候，指定一个[MediaSession.Token](http://developer.android.com/reference/android/media/session/MediaSession.Token.html)对象以便与给定的MediaSession交互。

通过使用[MediaController.TransportControls](http://developer.android.com/reference/android/media/session/MediaController.TransportControls.html)方法，你可以传达诸如 [play()](http://developer.android.com/reference/android/media/session/MediaController.TransportControls.html#play()), [stop()](http://developer.android.com/reference/android/media/session/MediaController.TransportControls.html#stop()), [skipToNext()](http://developer.android.com/reference/android/media/session/MediaController.TransportControls.html#skipToNext()), 和 [setRating()](http://developer.android.com/reference/android/media/session/MediaController.TransportControls.html#setRating(android.media.Rating))命令以控制MediaSession上的媒体播放。你也可以注册一个[MediaController.Callback](http://developer.android.com/reference/android/media/session/MediaController.Callback.html)回调对象以监听session上的*元数据和状态变化*（metadata and state changes）。

此外，你还可以通过最新的[Notification.MediaStyle](http://developer.android.com/reference/android/app/Notification.MediaStyle.html)类创建rich notification以控制mediasession播放。

### 媒体浏览 ###

Android 5.0引入了新的[android.media.browse](http://developer.android.com/reference/android/media/browse/package-summary.html) API，你的app可以使用此api浏览其他app的媒体库。继承[MediaBrowserService](http://developer.android.com/reference/android/service/media/MediaBrowserService.html)类以对外暴露你的app的媒体内容。你继承的MediaBrowserService应该提供MediaSession.Token的接入口以便其他应用可以通过它播放你提供的媒体内容。

若要与媒体浏览服务交互，请使用[MediaBrowser](http://developer.android.com/reference/android/media/browse/MediaBrowser.html)类。创建MediaBrowser实例时，请为MediaSession指定一个组件名。通过这个MediaBrowser实例，你的app可以连接到关联的service并获得一个暴露出来的MediaSession.Token对象。

## 存储 ##

### 目录选择 ###

Android 5.0扩展了*存储框架*（Storage Access Framework），用户可以借此将一个文件夹（包括其子文件和文件夹）的读写权限赋予一个app。

要选择一个文件夹，请发出一条[OPEN_DOCUMENT_TREE](http://developer.android.com/reference/android/content/Intent.html#ACTION_OPEN_DOCUMENT_TREE) intent 即可。系统会列出所有支持文件夹选择的[DocumentsProvider](http://developer.android.com/reference/android/provider/DocumentsProvider.html)来让用户浏览并选择一个文件夹，返回值是选中的文件夹的URI。然后你就可以使用 [buildChildDocumentsUriUsingTree()](http://developer.android.com/reference/android/provider/DocumentsContract.html#buildChildDocumentsUriUsingTree(android.net.Uri, java.lang.String)) 、 [buildDocumentUriUsingTree()](http://developer.android.com/reference/android/provider/DocumentsContract.html#buildDocumentUriUsingTree(android.net.Uri, java.lang.String)) 和 [query()](http://developer.android.com/reference/android/content/ContentResolver.html#query(android.net.Uri, java.lang.String[], java.lang.String, java.lang.String[], java.lang.String)) 浏览此文件夹的子目录了。

新的 [createDocument()](http://developer.android.com/reference/android/provider/DocumentsContract.html#createDocument(android.content.ContentResolver, android.net.Uri, java.lang.String, java.lang.String)) 方法使得你可以在上面选择的文件夹及其子文件夹下面创建新文档或者文件夹。要操作已经存在的文件，请使用 [renameDocument()](http://developer.android.com/reference/android/provider/DocumentsContract.html#renameDocument(android.content.ContentResolver, android.net.Uri, java.lang.String)) 和 [deleteDocument()](http://developer.android.com/reference/android/provider/DocumentsProvider.html#deleteDocument(java.lang.String)). 调用这此方法之前先检查 [COLUMN_FLAGS](http://developer.android.com/reference/android/provider/DocumentsContract.Document.html#COLUMN_FLAGS) 以确定provider对这些方法是否。分别是：[FLAG_SUPPORTS_WRITE](http://developer.android.com/reference/android/provider/DocumentsContract.Document.html#FLAG_SUPPORTS_WRITE)，[FLAG_SUPPORTS_DELETE](http://developer.android.com/reference/android/provider/DocumentsContract.Document.html#FLAG_SUPPORTS_DELETE)，[FLAG_SUPPORTS_THUMBNAIL](http://developer.android.com/reference/android/provider/DocumentsContract.Document.html#FLAG_SUPPORTS_THUMBNAIL)，[FLAG_DIR_PREFERS_GRID](http://developer.android.com/reference/android/provider/DocumentsContract.Document.html#FLAG_DIR_PREFERS_GRID)，[FLAG_DIR_PREFERS_LAST_MODIFIED](http://developer.android.com/reference/android/provider/DocumentsContract.Document.html#FLAG_DIR_PREFERS_LAST_MODIFIED)）。

如果你实现了一个[DocumentsProvider](http://developer.android.com/reference/android/provider/DocumentsProvider.html)并且想要支持子目录选择，请实现[isChildDocument()](http://developer.android.com/reference/android/provider/DocumentsProvider.html#isChildDocument(java.lang.String, java.lang.String))方法并将[FLAG_SUPPORTS_IS_CHILD](http://developer.android.com/reference/android/provider/DocumentsContract.Root.html#FLAG_SUPPORTS_IS_CHILD)放到[COLUMN_FLAGS](http://developer.android.com/reference/android/provider/DocumentsContract.Root.html#COLUMN_FLAGS)里。

Android 5.0同时也引入了新的共享存储区上的package-specific目录，你可以在为里存储媒体文件，这些媒体文件可以被包含进[MediaStore](http://developer.android.com/reference/android/provider/MediaStore.html)里，新的 [getExternalMediaDirs()](http://developer.android.com/reference/android/content/Context.html#getExternalMediaDirs())方法返回你的app在所有共享存储设备上的媒体存储目录。像[getExternalFilesDir()](http://developer.android.com/reference/android/content/Context.html#getExternalFilesDir(java.lang.String))一样不需要特殊权限。系统会定时扫描这些文件夹中的媒体内容，当然你也可以使用[MediaScannerConnection](http://developer.android.com/reference/android/media/MediaScannerConnection.html)自行扫描新内容。*（大哥们不要把缓存的图片放这儿啊，~~好想把那些将缓存图片直接放到sd卡某个目录下的人拉出来打一顿~~）*

## 无线连接 ##

### 多网络连接（Multiple network connections） ###

Android 5.0支持新的多网络连接API以使你的app可以*根据特定功能*（with specific capabilities）动态扫描可用的网络并建立连接。当你的app需要指定网络——SUPL（无线位置服务）, 彩信或者运营商计费网络——才能用或者要通过一个特定的协议才能传输你的数据的时候，这个功能就派上用场了。

你的app动态选择并连接一个网络连接的步骤如下：

1. 新建一个[ConnectivityManager](http://developer.android.com/reference/android/net/ConnectivityManager.html).
2. 使用[NetworkRequest.Builder](http://developer.android.com/reference/android/net/NetworkRequest.Builder.html) 类创建一个[NetworkRequest](http://developer.android.com/reference/android/net/NetworkRequest.html)对象并指定你的app需要的网络特性和传输类型。 
3. 要扫描合适的网络，请调用[requestNetwork()](http://developer.android.com/reference/android/net/ConnectivityManager.html#requestNetwork(android.net.NetworkRequest, android.net.ConnectivityManager.NetworkCallback)) 或者 [registerNetworkCallback()](http://developer.android.com/reference/android/net/ConnectivityManager.html#registerNetworkCallback(android.net.NetworkRequest, android.net.ConnectivityManager.NetworkCallback)), 并将NetworkRequest对象和一个 [ConnectivityManager.NetworkCallback](http://developer.android.com/reference/android/net/ConnectivityManager.NetworkCallback.html)作为参数传过去。如果你要在合适的网络被扫描到之后就切换到这个网络，请调用用 [requestNetwork()](http://developer.android.com/reference/android/net/ConnectivityManager.html#requestNetwork(android.net.NetworkRequest, android.net.ConnectivityManager.NetworkCallback)) 方法 如果仅仅接收扫描结果而不切换网络的话，请使用[registerNetworkCallback()](http://developer.android.com/reference/android/net/ConnectivityManager.html#registerNetworkCallback(android.net.NetworkRequest, android.net.ConnectivityManager.NetworkCallback)) 方法.
当系统探测到一个合适的网络时连接到这个网络并调用[onAvailable()](http://developer.android.com/reference/android/net/ConnectivityManager.NetworkCallback.html#onAvailable(android.net.Network))方法。你可以使用这个方法传进来的[Network](http://developer.android.com/reference/android/net/Network.html)对象得到这个网络更多的信息或者使用此网络。

### 低功耗蓝牙 ###

（*表示不懂……*）

Android 4.3引入了对Bluetooth Low Energy (Bluetooth LE)的平台支持in the central role(咋理解)。从Android 5.0开始，Android设备可以像低功耗蓝牙外设一样了。应用可以使用些功能使得附近的设备探测到你的存在。比如说，你可以创建一个计步器应用或者健康状况监视应用并与另外一个低功耗蓝牙外设建立数据连接。

使用新的[android.bluetooth.le](http://developer.android.com/reference/android/bluetooth/le/package-summary.html) API，你的app可以*广播广告*（broadcast advertisements）、*扫描响应*（scan for responses）并与附近的低功耗蓝牙设备连接。要使用新的广播和扫描特性，请在manifest文件中添加[BLUETOOTH_ADMIN](http://developer.android.com/reference/android/Manifest.permission.html#BLUETOOTH_ADMIN)权限。当用户下载或者更新你的app时，会被请求允许这些权限。

要开始Bluetooth LE advertising以便别的设备可以发现你的app，请调用[startAdvertising()](http://developer.android.com/reference/android/bluetooth/le/BluetoothLeAdvertiser.html#startAdvertising(android.bluetooth.le.AdvertiseSettings, android.bluetooth.le.AdvertiseData, android.bluetooth.le.AdvertiseCallback))将一个[AdvertiseCallback](http://developer.android.com/reference/android/bluetooth/le/AdvertiseCallback.html)作为参数传进去。这个callback对象会接收advertising功能或者失败的消息。

Android 5.0 引入了[ScanFilter](http://developer.android.com/reference/android/bluetooth/le/ScanFilter.html)，这样你的app就可以只搜索你需要的特定类型的设备。调用[startScan()](http://developer.android.com/reference/android/bluetooth/le/BluetoothLeScanner.html#startScan(android.bluetooth.le.ScanCallback))方法并传递进一个filter列表以扫描低功耗蓝牙设备——你必须提供一个[ScanCallback](http://developer.android.com/reference/android/bluetooth/le/ScanCallback.html)以在Bluetooth LE advertisement被发现后可以报告。（............）

### NFC增强 ###

Android 5.0对NFC进行了以下增强以使其得以更广泛和灵活的应用：

- Android Beam 可以在分享按钮中使用了。
- 你的应用可以通过[invokeBeam()](http://developer.android.com/reference/android/nfc/NfcAdapter.html#invokeBeam(android.app.Activity))调用Android Beam以分享数据。避免了用户必须自己手动操作设备以来分享数据的麻烦。
- 你现在可以使用[createTextRecord()](http://developer.android.com/reference/android/nfc/NdefRecord.html#createTextRecord(java.lang.String, java.lang.String))方法创建包含UTF-8文本格式数据的NDEF记录。
- 如果你在开发一款支付类应用，你现在可以对过调用[registerAidsForService()](http://developer.android.com/reference/android/nfc/cardemulation/CardEmulation.html#registerAidsForService(android.content.ComponentName, java.lang.String, java.util.List<java.lang.String>))以动态地注册一个NFC应用ID（AID）。你也可以使用[setPreferredService()](http://developer.android.com/reference/android/nfc/cardemulation/CardEmulation.html#setPreferredService(android.app.Activity, android.content.ComponentName))方法用于在某个特定的acitivy处于前台时指定一个偏好的Card Emulation服务。

## Project Volta ##

除了新特性之外（？），Android 5.0还*重点突出了对电池寿命的提升*（emphasizes improvements in battery life）。使用新的API和工具可以查看并优化你的app的电量使用。

### Scheduling jobs ###

Android 5.0提供一个新的[JobScheduler](http://developer.android.com/reference/android/app/job/JobScheduler.html) API以让你通过使系统推迟一些时间或者在特定条件下（比如充电中）异步执行某些任务以优化电池寿命。在下面情况下这很有用。

- 应用有可延后执行的后台任务。
- 应用有你想在充电时才执行的任务。
- 应用有需要网络或者WIFI才能执行的任务。
- 应用有一些要*定期统一执行*（run as a batch on a regular schedule）的任务。

*一批任务*（A unit of work）同一个[JobInfo](http://developer.android.com/reference/android/app/job/JobInfo.html)对象封装，这个对象指定了任务如何安排。

使用[JobInfo.Builder](http://developer.android.com/reference/android/app/job/JobInfo.Builder.html)类来设置如何安排这些任务的运行时刻表，你可以安排任务在正面情况下运行，比如：

- 设备充电时开始执行。
- 设备连接到非计费网络时开始执行。
- 设置空闲时开始执行。
- 在某个deadline前或者某个delay后结束执行。

举例，如果你想在设备连接到非计费网络时执行，可以这样做：

```
JobInfo uploadTask = new JobInfo.Builder(mJobId,
                                         mServiceComponent /* JobService component */)
        .setRequiredNetworkCapabilities(JobInfo.NetworkType.UNMETERED)
        .build();
JobScheduler jobScheduler =
        (JobScheduler) context.getSystemService(Context.JOB_SCHEDULER_SERVICE);
jobScheduler.schedule(uploadTask);
```

如果设备有一个稳定的电源（进入充电状态超过两分钟并且电量处于[健康水平](http://developer.android.com/reference/android/content/Intent.html#ACTION_BATTERY_OKAY)），系统就会执行被安排好的任务，*即使该任务的deadline还没有过期*（？？？even if the job’s deadline has not expired）。

要查看如何使用JobScheduler API，请查看Sample中的JobSchedulerSample。

### 电量使用开发工具 ###

新的```dumpsys batterystats```命令可以返回你感兴趣的按唯一的UID组织的电量使用数据。数据包括以下几方面：

- 电池相关事件历史。
- 设备的全局数据。
- 每个UID和系统组件的粗略的电量使用。
- Per-app mobile ms per packet
- 系统UID总数据。
- 应用UID总数据。

使用```--help```可以学习更多的参数选项以输出你想要的数据。比如，要输出上次充电后某个指定app的电量使用数据，执行如下命令：

```
$ adb shell dumpsys batterystats --charged <package-name>
```

你可以对上面的命令的输出数据使用[Battery Historian](https://github.com/google/battery-historian)工具来生成HTML页面以方便查看。

## Android在办公和教育中的应用 ##

（*不知所云，一片胡扯，译者处于昏迷状态*）

### Managed provisioning ###

Android 5.0 为在办公环境中运行的app提供了新的功能。如果用户已经在设备上有了一个个人账户，设备管理员可以启动一个*管理配置进程*（managed provisioning process）以再添加一个共存但是相互独立的profile。受管理的profiles关联的app与非受管理的app并列出现在Launcher、最近任务和通知里面。

要启动*管理配置进程*，发起一个[ACTION_PROVISION_MANAGED_PROFILE](http://developer.android.com/reference/android/app/admin/DevicePolicyManager.html#ACTION_PROVISION_MANAGED_PROFILE) Intent。如果调用成功的话，系统回调[onProfileProvisioningComplete()](http://developer.android.com/reference/android/app/admin/DeviceAdminReceiver.html#onProfileProvisioningComplete(android.content.Context, android.content.Intent))。然后你可以调用[setProfileEnabled() ](http://developer.android.com/reference/android/app/admin/DevicePolicyManager.html#setProfileEnabled(android.content.ComponentName))来启动这个受管理的profile。

默认情况下，在受管理的profile里面只有很少的app可用。你可以在受管理profile里面调用[enableSystemApp()](http://developer.android.com/reference/android/app/admin/DevicePolicyManager.html#enableSystemApp(android.content.ComponentName, android.content.Intent))来使其他app在包含进profile中。

如果你在开发一款Launcher程序，可以使用新的[LauncherApps](http://developer.android.com/reference/android/content/pm/LauncherApps.html)类来获取可展示到Launcher上的的activity列表——当然只能是属于当前用户和相关的受管理的profiles的。你的Launcher可以通过加入一个工作标志来使得使受管理的app突出显示出来，通过[getUserBadgedIcon()](http://developer.android.com/reference/android/content/pm/PackageManager.html#getUserBadgedIcon(android.graphics.drawable.Drawable, android.os.UserHandle))方法可以取得这种带标志的图标。

查看Sample中的```BasicManagedProfile```来学习如何使用这些新功能。

### *设备所有者*（Device owner） ###

Android 5.0 引入了可以部署```设备所有者```app的能力，```设备所有者```是一个拥有创建和删除子用户以及配置全局设置的*特殊类型的*（specialized type）设备管理员。你的所有者应用可以使用[DevicePolicyManager](http://developer.android.com/reference/android/app/admin/DevicePolicyManager.html)里面的方法的对设备配置、安全策略的应用*进行细粒度的控制*（take fine-grain control）。一个设备在同一时间只能有一个活动的设备所有者。

要部署并激活设备所有者，在设备的unprovisioned状态下，进行从一个*编程应用*（programming app）到设备NFC数据传输。传输的数据和上面刚刚提到的provisioning intent中的数据相同。

### 屏幕固定 ###

Android 5.0 引入了新的屏幕固定API，可以让用户暂时限制在一个任务中无法离开，此时也不会被通知所干扰。如果你正在开发*一款教育应用以在Android支持高风险的评估要求或者目的单一的或者Kiosk应用程序*（an education app to support high stakes assessment requirements on Android, or a single-purpose or kiosk application——*这啥意思，口吐白沫中*）的时候，你就可以考虑使用这个API。一旦你的app启动了屏幕固定，用户就将看不到通知、打开其他app或者返回桌面，直到退出这种模式。

有两种方式启动屏幕固定：

- **手动固定**：用户可以拖动开启屏幕固定。设置>安全>屏幕固定，然后选择在最近任务界面选择在固定的任务。
- **编程固定**：要通过编码实现屏幕固定，在你的app中调用[startLockTask()](http://developer.android.com/reference/android/app/Activity.html#startLockTask())方法。如果请求的app不是*设备所有者*（device owner），用户会被弹出一个询问提示。设备所有者可以调用[setLockTaskPackages()](http://developer.android.com/reference/android/app/admin/DevicePolicyManager.html#setLockTaskPackages(android.content.ComponentName, java.lang.String[]))方法以使得某个app可以不经过用户确认就进步屏幕固定状态。

任务锁定后，会：

- 状态栏变空，用户通知和状态信息被隐藏。
- 主屏幕和最近任务按钮被隐藏。
- 其他app打不开新的activity。
- 只要不开启新的task,当前app可以打开新的activity。
- 如果屏幕固定是由设备所有者启动，用户仍旧会锁定在你的app下直到调用了[stopLockTask()](http://developer.android.com/reference/android/app/admin/DevicePolicyManager.html#setLockTaskPackages(android.content.ComponentName, java.lang.String[]))。
- 如果屏幕固定由非设备所有者启动或者由用户手动启动，*用户可以通过同时按住返回的最近任务按钮退出*（the user can exit by holding both the Back and Recent buttons）

## 打印框架 ##

### 以bitmap渲染PDF ###

现在可以用新的[PdfRenderer](http://developer.android.com/reference/android/graphics/pdf/PdfRenderer.html)类将PDF页面渲染成bitmap来渲染。必须指定一个可搜索（内容可以随机访问）的[ParcelFileDescriptor](http://developer.android.com/reference/android/os/ParcelFileDescriptor.html)，系统会在它上面写入可打印数据。通过调用[openPage()](http://developer.android.com/reference/android/graphics/pdf/PdfRenderer.html#openPage(int))方法，你的app可以得到一个待渲染页面，然后调用[render()](http://developer.android.com/reference/android/graphics/pdf/PdfRenderer.Page.html#render(android.graphics.Bitmap, android.graphics.Rect, android.graphics.Matrix, int))以将打开的[PdfRenderer.Page](http://developer.android.com/reference/android/graphics/pdf/PdfRenderer.Page.html)渲染到一个bitmap上。如果你想只转换此文档的一部分的话，要传入额外的一些参数。

要查看如何使用新的API，请查看Sample里面的```PdfRendererBasic ```。

## 系统 ##

### 应用使用数据 ###

现在你可以使用[android.app.usage](http://developer.android.com/reference/android/app/usage/package-summary.html) API获取Android设备的app使用历史。这个API提供了比已经弃用的[getRecentTasks()](http://developer.android.com/reference/android/app/ActivityManager.html#getRecentTasks(int, int))方法更详细的使用数据。要使用这个API，首先要在manifest中添加```android.permission.PACKAGE_USAGE_STATS```权限，用户可以通过Settings > Security > Apps赋予此app的读取app使用数据的权限.

系统按应用分别收集使用数据，并且按天、周、月、年整合数据。系统保存数据的最长时间如下：

- Daily data: 7天
- Weekly data: 4周
- Monthly data: 6个月
- Yearly data: 2年 

对于每个应用，系统记录如下数据：

- 应用上次使用时间。
- 对应时间段内应用前台运行总时间(by day, week, month, or year)。
- 一个组件（按包名和activity名区分）在一天内被移动到前台或者后台的Timestamp capturing。
- 设备设置改变（比如屏幕方向改变）的Timestamp capturing。

## 测试 & 辅助功能 ##

### 测试和可访问性改进 ###

Android 5.0为测试和可访问性增加如下支持：

- 新的[getWindowAnimationFrameStats()](http://developer.android.com/reference/android/app/UiAutomation.html#getWindowAnimationFrameStats())和[getWindowContentFrameStats()](http://developer.android.com/reference/android/app/UiAutomation.html#getWindowContentFrameStats(int))方法可以捕获窗口动画和内容的帧数据。这些方法使你可以编写instrumentation tests以评估app是否流畅。
- 新的[executeShellCommand()](http://developer.android.com/reference/android/app/UiAutomation.html#executeShellCommand(java.lang.String))方法让你可以在instrumentation test中执行shell命令。类似于执行 adb shell，这样你可以使用一些shell工具比如```dumpsys, am, content``` 和``` pm```.
- 使用accessibility APIs(比如[UiAutomator](http://developer.android.com/tools/help/uiautomator/index.html))的Accessibility Service和测试工具现在可以取得屏幕上能够进行可见交互的窗口的详细信息。要获得[AccessibilityWindowInfo](http://developer.android.com/reference/android/view/accessibility/AccessibilityWindowInfo.html)对象列表，请调用 [getWindows()](http://developer.android.com/reference/android/accessibilityservice/AccessibilityService.html#getWindows())方法。
- 新的[AccessibilityNodeInfo.AccessibilityAction](http://developer.android.com/reference/android/view/accessibility/AccessibilityNodeInfo.AccessibilityAction.html)类让你可以在[AccessibilityNodeInfo](http://developer.android.com/reference/android/view/accessibility/AccessibilityNodeInfo.html)上执行标准的或者自定义的动作。新的 [AccessibilityNodeInfo.AccessibilityAction](http://developer.android.com/reference/android/view/accessibility/AccessibilityNodeInfo.AccessibilityAction.html)类取代了AccessibilityNodeInfo中的早期action API。
- Android 5.0使你的app可以对文字转语音（text-to-speech synthesis）进行更细粒度的控制。有了新的Voice类，你的App可以通过指定地区, 质量和延迟率来设置声音，也可以使用*文字转语音引擎相关的特定特性*（text-to-speech engine-specific parameters）。

## IME ##

### 更容易地切换输入语言 ###

这块不翻译了，标题说的很明确了，但是输入法右下角那个切换按钮总是误触好蛋疼啊~摔~

## Manifest 声明 ##

### Declarable required features ###

下面的一些特性已经开始在```<uses-feature>```中支持，所以你可以确认你的app是否安装在支持你所需特性的设备上。

- FEATURE_AUDIO_OUTPUT
- FEATURE_CAMERA_CAPABILITY_MANUAL_POST_PROCESSING
- FEATURE_CAMERA_CAPABILITY_MANUAL_SENSOR
- FEATURE_CAMERA_CAPABILITY_RAW
- FEATURE_CAMERA_LEVEL_FULL
- FEATURE_GAMEPAD
- FEATURE_LIVE_TV
- FEATURE_MANAGED_USERS
- FEATURE_LEANBACK
- FEATURE_OPENGLES_EXTENSION_PACK
- FEATURE_SECURELY_REMOVES_USERS
- FEATURE_SENSOR_AMBIENT_TEMPERATURE
- FEATURE_SENSOR_HEART_RATE_ECG
- FEATURE_SENSOR_RELATIVE_HUMIDITY
- FEATURE_VERIFIED_BOOT
- FEATURE_WEBVIEW

### User permissions ###

现在```<uses-permission>```已经支持下面的权限，如果你需要的话就加上它吧。

BIND_DREAM_SERVICE: 如果目标API是21或更高, [Daydream](http://developer.android.com/about/versions/android-4.2.html#Daydream)服务需要使用这个权限。（When targeting API level 21 and higher, this permission is required by a Daydream service, to ensure that only the system can bind to it.）
