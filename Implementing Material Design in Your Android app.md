Implementing Material Design in Your Android app
==============

*译自 http://android-developers.blogspot.com/2014/10/implementing-material-design-in-your.html —— By [NashLegend](https://github.com/NashLegend)*

![](https://raw.githubusercontent.com/NashLegend/ProjectBabel/master/images/title1.png)

[Material Design](http://www.google.com/design/spec/#utm_campaign=L-Developer-launch)对于多屏幕的视觉、交互和动作设计作了很大的改进。Android 5.0和最新的支持包可以帮助你创建Material风格的UI。这里将概述新的Material Design风格设计、新的API和组件，你可以将它们用在你的app中。

### Tangible surfaces ###

在Material Design的世界中，UI是由[电子纸和电子墨水](https://www.youtube.com/watch?v=YaG_ljfzeUw)构建起来的。这些UI的表面和阴影为应用程序的结构提供视觉线索（也就是根据样式能看出层次关系等等，比如下图），你看他的样子就知道哪里是可触摸的部分、就可以知道它将以何种方式运动（同样如下图）。这种Didital Material可以移动、扩张和变形以创建灵活的UI界面。 
![](https://raw.githubusercontent.com/NashLegend/ProjectBabel/master/images/layering.gif)

#### Shadows ####

一个surface的位置和深度决定了其光影的微妙变化。你可以使用新的elevation属性设置view在Z轴的位置，系统会依此在view后自动绘制出实时的动态阴影。你可以在layout XML里面设置elevation属性，单位是dip：
```
<ImageView …
    android:elevation="8dp" />
```
你也可以在代码中通过getElevation()/setElevation()（with shims in [ViewCompat](https://developer.android.com/reference/android/support/v4/view/ViewCompat.html?utm_campaign=L-Developer-launch)）方法操作elevation属性。View投身的阴影是由其轮廓决定的，而默认情况下这个轮廓是由background产生的。举个栗子：如果你为一个[浮动按钮](http://www.google.com/design/spec/patterns/promoted-actions.html?utm_campaign=L-Developer-launch#promoted-actions-floating-action-button)设置了一个圆形的shape drawable背景，那么它的阴影就是圆形的。如果你想能对阴影进行更详细的控制，你可以使用[ViewOutlineProvider](https://developer.android.com/reference/android/view/ViewOutlineProvider.html?utm_campaign=L-Developer-launch)来自定义其轮廓，通过getOutline()方法可以取得这个轮廓。

#### 卡片 ####
要显示一些不同的信息，使用[卡片](http://www.google.com/design/spec/components/cards.html#utm_campaign=L-Developer-launch)是一个常用模式。你可以使用新的支持包中的[CardView](https://developer.android.com/reference/android/support/v7/widget/CardView.html?utm_campaign=L-Developer-launch)轻松创建卡片式布局，新的CardView在低版本Android上也支持轮廓和阴影。
```
<android.support.v7.widget.CardView
    android:layout_width="match_parent"
    android:layout_height="wrap_content">
    <!-- Your card content -->
</android.support.v7.widget.CardView>
```
CardView继承了FrameLayout并提供了默认的elevation和圆角，这样卡片在不同版本的系统上就拥有统一的表现。如有必要，你还可以通过设置`cardElevation`和`cardCornerRadius`自定义这些属性。但是卡片并不是实现*维度效果*的不二法门，所以别滥用。

### *印刷样式的设计*（Print-like Design） ###
Material Design借鉴了印刷设计以创建简明清晰的布局，将你的内容部分突出在前面和中间。大胆的配色、刻意的留白、精致的排版和*强大的基线网格*（a strong baseline grid）产生了具有层次感、有意义、突出重点的布局.

#### 字体排版 ####
Android 5.0升级了系统Roboto字体，现在无论字体大小，都可以漂亮整洁地显示字体。系统还新增了一个新的适用于中等字号(android:fontFamily=”sans-serif-medium”)，新的文本显示样式使用了备受好评的[字体排版缩放](http://www.google.com/design/spec/style/typography.html#typography-standard-styles)以平衡内容密度的阅读舒适度。举个栗子：你可以通过设置`android:textAppearance=”@android:style/TextAppearance.Material.Title”`轻松地使用"Title"样式，可以通过AppCompat支持包在旧版本的Android系统上使用这种样式，举个栗子：`“@style/TextAppearance.AppCompat.Title”`。

#### Color ####
![](https://raw.githubusercontent.com/NashLegend/ProjectBabel/master/images/color_attribs.png)

应用的[*色调*(color palette)](http://www.google.com/design/spec/style/color.html#color-ui-color-application)会为你的应用带来品牌辨识度和个性化（比如一看见红色就想到可口可乐），现在你们可以通过设置下面的属性使得这些变得更加简单。

- colorPrimary. 应用的主色调、品牌颜色，用于ActionBar背景、最近运行任务界面标题以及边缘效果（比如说MIUI系统中可滚动元素滚动到边缘时出现的橙黄色）。
- colorAccent. *主色调的鲜亮补充色*（Vibrant complement to the primary branding color），用于框架控件比如说EditText和Switch。（前面几句真别扭，因此注：colorAccent，我不知道专业术语是啥，直译应该是突出出来的颜色，因此，EditText的颜色并不是colorAccent，它得到焦点后的颜色才是colorAccent，还有Switch在打开时的颜色也是colorAccent，如上图）。
- colorPrimaryDark. 主色调的暗色版本，用于状态栏。

除了上面这些之外，你还可以对颜色进行更深入精细的控制，参见`colorControlNormal, colorControlActivated, colorControlHighlight, colorButtonNormal, colorSwitchThumbNormal, colorEdgeEffect, statusBarColor `和` navigationBarColor`。

AppCompat支持上面功能的绝大部分，因此你可以将此应用在Android 5.0之前的系统中。

#### Dynamic color —— 动态颜色 ####
 
Material Design鼓励使用动态颜色，尤其是你的应用中有很多图片时。新的Palette支持库可以提取图片中的一部分颜色来设置你的UI的样式来使界面颜色互相搭配以提供一种沉浸式体验。提取出来的*调色板*（palette）包括*突出的和柔和的色调*（vibrant and muted tones，参见下图文字后面变化的背景色），同时可取得最佳阅读体验的文本前景色（参见下图的变化的文字颜色）。举个栗子：
```
Palette.generateAsync(bitmap,
        new Palette.PaletteAsyncListener() {
    @Override
    public void onGenerated(Palette palette) {
         Palette.Swatch vibrant =
                 palette.getVibrantSwatch();
          if (swatch != null) {
              // If we have a vibrant color
              // update the title TextView
              titleView.setBackgroundColor(
                  vibrant.getRgb());
              titleView.setTextColor(
                  vibrant.getTitleTextColor());
          }
    }
});
```
一图胜千言。
![](https://raw.githubusercontent.com/NashLegend/ProjectBabel/master/images/palette2.gif)

### Authentic Motion —— 真实的运动###

Tangible surfaces不会突兀地出现，他们出现的动作要引导用户注意力，要建立空间关联并保持连贯性。Material元素会对你的触摸事件做出响应以对本次交互进行确认，然后所有的变化将以你点击的位置为中心向外扩散。所有的动作都是有意义的和密切相关的，使用户易于理解。

#### Activity 和 Fragment 切换 ####
通过声明两屏之间通用的`共享元素`，你可以在两种状态间创建流畅的切换。


一图胜千言：

![](https://raw.githubusercontent.com/NashLegend/ProjectBabel/master/images/activity_transitions.gif)

示例代码：

```
album_grid.xml
…
    <ImageView
        …
        android:transitionName="@string/transition_album_cover" />
album_details.xml
…
    <ImageView
        …
        android:transitionName="@string/transition_album_cover" />

AlbumActivity.java
Intent intent = new Intent();
String transitionName = getString(R.string.transition_album_cover);
…
ActivityOptionsCompat options =
ActivityOptionsCompat.makeSceneTransitionAnimation(activity,
    albumCoverImageView,   // The view which starts the transition
    transitionName    // The transitionName of the view we’re transitioning to
    );
ActivityCompat.startActivity(activity, intent, options.toBundle());
```

我们在两屏之间定义了相同的`transitionName`。当打开新的Activity的时候，切换过程就自动开始了。除了共享元素之外，你也可以定义[进入](https://developer.android.com/reference/android/view/Window.html?utm_campaign=L-Developer-launch#setEnterTransition(android.transition.Transition))和[退出](https://developer.android.com/reference/android/view/Window.html?utm_campaign=L-Developer-launch#setExitTransition(android.transition.Transition))元素。

#### Ripples ——波纹效果####

![](https://raw.githubusercontent.com/NashLegend/ProjectBabel/master/images/ripple.gif)

Material风格的元素以一种波纹(ripple)扩散的方式响应用户的触摸。如果你使用了Theme.Material或者其派生的主题，那么交互性控件比如Button默认就会拥有这种效果（as will ?android:selectableItemBackground）。你也可以在你的drawable上使用这种效果——只要把它们放到ripple元素里，如下：
```
<ripple
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:color="@color/accent_dark">
    <item>
        <shape
            android:shape="oval">
            <solid android:color="?android:colorAccent" />
        </shape>
    </item>
</ripple>
```

自定义view通过[View#drawableHotspotChanged](http://developer.android.com/reference/android/view/View.html?utm_campaign=L-Developer-launch#drawableHotspotChanged(float,%20float))回调方法将点击位置传递过去，以便从点击的位置发起ripple效果。

#### StateListAnimator ####
Materials also respond to touch by raising up to meet your finger, like a magnetic attraction. You can achieve this effect by animating the translationZ attribute which is analogous to elevation but intended for transient use; such that Z = elevation + translationZ. The new stateListAnimator attribute allows you to easily animate the translationZ on touch (Buttons do this by default):
```
layout/your_layout.xml
<ImageButton …
    android:stateListAnimator="@anim/raise" />
anim/raise.xml
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:state_enabled="true" android:state_pressed="true">
        <objectAnimator
            android:duration="@android:integer/config_shortAnimTime"
            android:propertyName="translationZ"
            android:valueTo="@dimen/touch_raise"
            android:valueType="floatType" />
    </item>
    <item>
        <objectAnimator
            android:duration="@android:integer/config_shortAnimTime"
            android:propertyName="translationZ"
            android:valueTo="0dp"
            android:valueType="floatType" />
    </item>
</selector>
```

#### Reveal ####
A hallmark material transition for showing new content is to reveal it with an expanding circular mask. This helps to reinforce the user’s touchpoint as the start of all transitions, with its effects radiating outward radially. You can implement this using the following Animator:
```
Animator reveal = ViewAnimationUtils.createCircularReveal(
                    viewToReveal, // The new View to reveal
                    centerX,      // x co-ordinate to start the mask from
                    centerY,      // y co-ordinate to start the mask from
                    startRadius,  // radius of the starting mask
                    endRadius);   // radius of the final mask
reveal.start();
```

Interpolators
Motion should be deliberate, swift and precise. Unlike typical ease-in-ease-out transitions, in Material Design, objects tend to start quickly and ease into their final position. Over the course of the animation, the object spends more time near its final destination. As a result, the user isn’t left waiting for the animation to finish, and the negative effects of motion are minimized. A new fast-in-slow-out interpolator has been added to achieve this motion.
![](https://raw.githubusercontent.com/NashLegend/ProjectBabel/master/images/interpolators.gif)
For elements entering and exiting the screen (which [should do so at peak velocity](http://www.google.com/design/spec/animation/authentic-motion.html#authentic-motion-mass-weight)), check out the [linear-out-slow-in](https://developer.android.com/reference/android/R.interpolator.html?utm_campaign=L-Developer-launch#linear_out_slow_in) and [fast-out-linear-in](https://developer.android.com/reference/android/R.interpolator.html?utm_campaign=L-Developer-launch#fast_out_linear_in) interpolators respectively.