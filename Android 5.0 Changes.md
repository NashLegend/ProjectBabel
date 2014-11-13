*Translated from http://developer.android.com/intl/zh-cn/about/versions/android-5.0-changes.html*

### API Level: 21 ###

除了新增特性和功能之外，Android 5.0还包含了一系列的变化，包括API变化、行为变化，系统增强以及bug修复。这篇文档将重点阐述一些你应该知道并应用到你的app里面的关键的变化。

如果你之前已经发布过一个Android app，请注意您的app有可能会受到Android 5.0的这些新变化的影响。


如果想看一些新平台的更高级的特性，请看[这里](http://developer.android.com/intl/zh-cn/about/versions/lollipop.html)

## Android Runtime (ART) ##
在Android 5.0中，ART运行时取代Dalvik成为默认运行环境，ART于Android 4.4时代作为试验特性引入

如果想看ART的新特性概述，请看[这里](https://source.android.com/devices/tech/dalvik/art.html)，几个主要的特性如下：

- 提前编译（AOT）
- 增强的垃圾回收
- 增强的debug支持

多数Android应用地ART模式下运行是没有问题的，但是有些情况下却不能。要查看适配ART的注意事项，请看[这里](http://developer.android.com/intl/zh-cn/guide/practices/verifying-apps-art.html)，尤其要注意的是下面的情况：

- 你的app使用了JNI来运行C/C++代码
- 你使用了产生非标准代码的（比如一些代码混淆工具）
- 你使用了不兼容compacting garbage collection的技术

## 通知 ##
请确保你的通知将Android 5.0的最新变化考虑了进来，要获知更多如何为Android 5.0以及更高版本系统设计你的通知的信息，请看[这里](http://developer.android.com/design/patterns/notifications.html)

**Material design样式**

通知使用深色文字以及白色（或者很浅色）的背景以适配material design样式的桌面插件。请确保你的所有通知样式统一，如果你的通知看起来有问题，请修正它们:
- 使用 [setColor()](http://developer.android.com/reference/android/app/Notification.Builder.html#setColor(int))设置通知图标的背景（基准）色。
- 修改或者移除包含颜色的资源。系统在action icons和通知栏图标中了忽略所有非透明通道。你应该认为这些图标只有alpha通道。系统会把通知图标绘制到白色背景，把action icons绘制到深灰色背景。

**声音和震动**

如果你现在在通知里通过Ringtone, MediaPlayer, or Vibrator 类添加了声音和震动，请移除这些代码以便系统能够以优先模式正确的执行声音和震动行为。你应该使用Notification.Builder方法添加声音和震动才对。

将设备设置为静音模式将使设备进入这种新的优先模式。如果你将设备设置为普通模式或者震动模式，设置将离开这种优先模式。

以前Android使用[STREAM_MUSIC](http://http://developer.android.com/reference/android/media/AudioManager.html#STREAM_MUSIC)作为master stream以控制平板设备的音量。在Android 5.0上，平板和手机设备的master volume stream已经统一了起来，[由STREAM_RING](http://http://developer.android.com/reference/android/media/AudioManager.html#STREAM_RING) 或者 [STREAM_NOTIFICATION](http://developer.android.com/reference/android/media/AudioManager.html#STREAM_NOTIFICATION)控制。

**锁屏可视性**

现在，默认情况下，通知将显示在用户的锁屏界面上，用户出于保护隐私可以选择不展示这些信息，系统会使用其他提示来表示通知的文字内容，如果想自定义这些隐私化的文字，请使用[setPublicVersion()](http://developer.android.com/reference/android/app/Notification.Builder.html#setPublicVersion(android.app.Notification))方法

如果通知不包含私人信息或者你想要允许在通知上控制媒体，请调用 [setVisibility()](http://developer.android.com/reference/android/app/Notification.Builder.html#setVisibility(int))方法并奖通知的可见级别设置为[VISIBILITY_PUBLIC](http://developer.android.com/reference/android/app/Notification.html#VISIBILITY_PUBLIC)

**媒体播放**

如果你使用展示和控制媒体播放的通知，请考虑使用最新的[Notification.MediaStyle](http://developer.android.com/reference/android/app/Notification.MediaStyle.html)模版以取代自定义的[RemoteViews.RemoteView](http://developer.android.com/reference/android/widget/RemoteViews.RemoteView.html)对象，无论你使用哪种方法，确保通知的可见性为[VISIBILITY_PUBLIC](http://developer.android.com/reference/android/app/Notification.html#VISIBILITY_PUBLIC)，这样你可以在锁屏界面进行控制。请注意，自Android 5.0以后，系统不会将RemoteControlClient对象展示在锁屏界面上，要获知更多信息，请查看[如果你的app使用了RemoteControlClient](http://developer.android.com/intl/zh-cn/about/versions/android-5.0-changes.html#BehaviorMediaControl)（其实就在下面）

**浮动通知**

现在通知可以以一个悬浮小窗口的形式出现了，当然设备得是未锁屏且点亮屏幕状态。这些通知看起来像是你的通知的精简版一样，除了这些通知也可以使用action buttons。用户可以在当前正在使用的app里面选择执行也可以忽略通知动作而不必离开当前app。

下面是有可能触发浮动通知的情况。
- 用户的activity处于全屏状态。
- 通知具有高优先级并且使用了铃声或者震动。

这种情况下就要使用浮动通知。

## 媒体控制和 RemoteControlClient ##
[RemoteControlClient](http://developer.android.com/reference/android/media/RemoteControlClient.html)类现在已经废弃，请尽快改用[MediaSession](http://developer.android.com/reference/android/media/session/MediaSession.html)API.

Android 5.0的锁屏界面并不会显示你的MediaSession 或者 RemoteControlClient的控制界面。你的应用应该通过通知提供一个锁屏界面的控制界面，这样，在拥有锁屏和未锁屏状态下的连贯的用户体验的同时给予你的应用更多对于媒体按钮的控制权。

Android 5.0提供了一个新的[Notification.MediaStyle](http://developer.android.com/reference/android/app/Notification.MediaStyle.html)模版以实现上述目的。你可以在Notification.Builder.addAction()为Notification.MediaStyle添加动作。通过[setSettion()](http://developer.android.com/reference/android/app/Notification.MediaStyle.html#setMediaSession(android.media.session.MediaSession.Token))方法告诉系统这个通知控制一个媒体播放会话。

请确保将通知的可见性设置为VISIBILITY_PUBLIC以确保能显示在通知界面。要查看更多信息，请看[锁屏界面通知](http://developer.android.com/intl/zh-cn/about/versions/android-5.0-changes.html#LockscreenNotifications)。

如果你的app运行在Androdi TV或者可穿戴设备上，若要展示媒体控制，请使用MediaSession类，如果你的应用要接收Android设备的媒体按钮事件的话，请使用MediaSession类。

## getRecentTasks() ##

随着Android 5.0 concurrent documents and activities tasks特性的引入，ActivityManager.getRecentTasks()方法被废弃以保护用户隐私。为了后向兼容性，这个方法仍然后返回一小部分结果，比如调用此方法的应用的的task以及一些非敏感任务（比如桌面）。如果你的app使用这个方法获得自身的task的话，请使用[getAppTasks()](http://developer.android.com/reference/android/app/ActivityManager.html#getAppTasks())

## Android NDK的64位支持 ##

Android 5.0引入了64位支持，增加了寻址空间和系统表现的同时完美兼容现在的32位系统。64位支持同样增强了OpenSSL的加密性能。此外，这个版本还增加了新的多媒体NDK API以及OpenGL ES3.1支持。

要使用64位支持，到[这里](http://developer.android.com/tools/sdk/ndk/index.html)下载NDK Revision 10c，查看[这里](http://developer.android.com/tools/sdk/ndk/index.html#Revisions)以获取更多此版NDK的信息。

## Binding to a Service ##
Context.bindService()方法现在需要指定explicit Intent。如果implicit intent的话将抛出一个错误。为确保app安全性，请在启动或者绑定service的时候使用explicit intent，不要为service定义intent filters。

## WebView ##
Android 5.0 改变了app的默认行为。

**如果你的系统target api为21以上:**

- 系统默认禁止了[mixed content](https://developer.mozilla.org/en-US/docs/Security/MixedContent)和第三方cookie。可以使用[setMixedContentMode()](http://developer.android.com/reference/android/webkit/WebSettings.html#setMixedContentMode(int)) 和 [setAcceptThirdPartyCookies()](http://developer.android.com/reference/android/webkit/CookieManager.html#setAcceptThirdPartyCookies(android.webkit.WebView, boolean))以分别启用。
- The system now intelligently chooses portions of the HTML document to draw. This new default behavior helps to reduce memory footprint and increase performance. If you want to render the whole document at once, disable this optimization by calling enableSlowWholeDocumentDraw().
- 系统现在可以智能选择HTML文档的portion来绘制。这种新特性可以减少内存footprint并改进性能。若要一次性渲染整个HTML文档，可以调用这个方法[enableSlowWholeDocumentDraw()](http://developer.android.com/reference/android/webkit/WebView.html#enableSlowWholeDocumentDraw())

**如果你的app的target api低于21: **系统允许mixed content和第三方cookie，并且总是一次性渲染整个HTML文档。

## 自定义Permissions的唯一性要求 ##
如[Permissions](http://developer.android.com/guide/topics/manifest/manifest-intro.html#perms)文档所述，Android应用可以自定义permissions以限定调用某个组件的方式。应用在manifest文件中定义这此自定义permissions。

只有少数情况下我们才需要自定义permissions，很多情况下使用自定义permissions是没有必要且容易引发潜在危险的，这取决于这些permossioms的保护级别。

Android系统同时引入了一种新特性以确保一个特定的自定义permissions只能被一个app定义，除非其他app有相同的签名。

**对于那些使用相同的自定义permissions的APP们**

任何app都可以自定义任何permissions，所以有可能不同的app正好使用了重名的自定义permissions。比如两个有相似功能的app就有可能这么干，或者App们可能会引入拥有相同的自定义权限的公共库或者代码示例。

在Android 4.4之前这样做是没有问题的。从Android 5.0开始，系统加入了**不同签名的应用的自定义权限必有具有唯一性**的限制，如果用户想要安装一个**拥有和某个已安装app有相同自定义权限但是签名不同**的app，系统将禁止安装。

**Considerations for your app**

In Android 5.0 and later, apps can continue to define their own custom permissions just as before and to request custom permissions from other apps through the <uses-permission> mechanism. However with the new requirement introduced in Android 5.0, you should carefully assess possible impacts on your app.
Android 5.0以后，app仍然可以像以前一样自定义permissions和通过<uses-permission>请求其他app的权限。但是有了现在的限制之后，你应该认真考虑一下这些变化对你的app的影响。

下面几点是你要考虑的:

- 你的应用是否在manifest声明了<permission>元素。如果是的话，它们是否对于你的app或者service是必须的，或者，是否可以用系统默认permissions代替。
- 如果你使用了<permission>元素，你知道它们是哪里来的吗。
- 你是否真的希望别人通过<uses-permission>请求你的自定义权限。
- 你是否只是复制粘贴了别人的包含<permission>元素的代码，这些permissions是否必须。
- 你的app是否使用了别人有可能使用的permissions名字。

**安装和升级软件**

如上所述，Android 5.0以后的设备对于哪些有相同自定义permissions却没有相同签名的app是禁止安装的（文档真啰嗦，这里直接省了，具体看上面）

**一些建议**

在运行Android 5.0或者更高版本的设备上，我们建议您立即检查、作出修改、发布新版……

- 如果你使用了你不必须的自定义permissions，删除它们。
- 如果你的app必须使用自定义permissions的话，请修改它们以确保权限名的唯一性，比如可以以包名开头。
- 如果你有一套的不同签名的app使用一个自定义permissions以共用一个组件，请确保这个自定义permissions只被定义了一次。
- 如果你的一套app重命名相同，那么随意，同名无所谓。

*下面的不懂就不翻译了*

## TLS/SSL Default Configuration Changes ##
Android 5.0 introduces changes the default TLS/SSL configuration used by apps for HTTPS and other TLS/SSL traffic:

- TLSv1.2 and TLSv1.1 protocols are now enabled,
- AES-GCM (AEAD) cipher suites are now enabled,
- MD5, 3DES, export, and static key ECDH cipher suites are now disabled,
- Forward Secrecy cipher suites (ECDHE and DHE) are preferred.

These changes may lead to breakages in HTTPS or TLS/SSL connectivity in a small number of cases listed below.

Note that the security ProviderInstaller from Google Play services already offers these changes across Android platform versions back to Android 2.3.

**Server does not support any of the enabled ciphers suites**

For example, a server might support only 3DES or MD5 cipher suites. The preferred fix is to improve the server’s configuration to enable stronger and more modern cipher suites and protocols. Ideally, TLSv1.2 and AES-GCM should be enabled, and Forward Secrecy cipher suites (ECDHE, DHE) should be enabled and preferred.

An alternative is to modify the app to use a custom SSLSocketFactory to communicate with the server. The factory should be designed to create SSLSocket instances which have some of the cipher suites required by the server enabled in addition to default cipher suites.

**App is making wrong assumptions about cipher suites used to connect to server**

For example, some apps contain a custom X509TrustManager that breaks because it expects the authType parameter to be RSA but encounters ECDHE_RSA or DHE_RSA.

**Server is intolerant to TLSv1.1, TLSv1.2 or new TLS extensions**

For example, the TLS/SSL handshake with a server is erroneously rejected or stalls. The preferred fix is to upgrade the server to comply with the TLS/SSL protocol. This will make the server successfully negotiate these newer protocols or negotiate TLSv1 or older protocols and ignore TLS extensions it does not understand. In some cases disabling TLSv1.1 and TLSv1.2 on the server may work as a stopgap measure until the server software is upgraded.

An alternative is to modify the app to use a custom SSLSocketFactory to communicate with the server. The factory should be designed to create SSLSocket instances with only those protocols enabled which are correctly supported by the server.