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
Android 5.0 introduces support for 64-bit systems. The 64-bit enhancement increases address space and improves performance, while still supporting existing 32-bit apps fully. The 64-bit support also improves the performance of OpenSSL for cryptography. In addition, this release introduces new native media NDK APIs, as well as native OpenGL ES (GLES) 3.1 support.

To use the 64-bit support provided in Android 5.0, download and install NDK Revision 10c from the Android NDK page. Refer to the Revision 10c release notes for more information about important changes and bug fixes to the NDK.
