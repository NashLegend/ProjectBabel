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

### “最近运行”界面上的多开的文档和activity ###

以前的版本中，“[最近运行](http://developer.android.com/intl/zh-cn/guide/components/recents.html)”界面对于一个app来说只能显示用户最近交互过的一个task。现在你的应用可以打开更多task以同时打开不同的文档。这种新的多任务特性可以让用户在最近运行界面中快速在activity们和打开的文档们之间任意切换。有可能使用这种并发任务的情景示例：浏览器标签多开、看比赛多开、生产力工具（比如Word、PPT等）文档多开、多窗口与多个妹子聊天等等。你的app可以通过[ActivityManager.AppTask](http://developer.android.com/reference/android/app/ActivityManager.AppTask.html)来管理这些task。

要让系统把你的activity当成一个新的task,在startActivity()的时候使用[FLAG_ACTIVITY_NEW_DOCUMENT](http://developer.android.com/reference/android/content/Intent.html#FLAG_ACTIVITY_NEW_DOCUMENT)，你也可以在manifest文件中把activity的```documentLaunchMode```属性设置成```"intoExisting"``` 或者 ```"always"```来实现这一点。

为了避免“最近运行”界面太多太乱，你可以设置你的app可以显示在此界面上的最大任务数量————设置manifest文件中<application> 的属性```android:maxRecents```，目前的最大数量是每个用户50个，RAM较小的手机则为25个。

最近运行界面上的task可以设置为*重启时常驻*（persist across reboots），可以设置[android:persistableMode](http://developer.android.com/reference/android/R.attr.html#persistableMode)属性以控制常驻行为。你也可以通过[setTaskDescription()](http://developer.android.com/reference/android/app/Activity.html#setTaskDescription(android.app.ActivityManager.TaskDescription))方法修改activity在最近运行界面上的颜色、标签和图标等可见元素。

