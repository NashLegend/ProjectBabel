在你的Android应用中使用Material Design
==============

*译自 http://android-developers.blogspot.com/2014/10/implementing-material-design-in-your.html —— By [NashLegend](https://github.com/NashLegend)*

![](https://raw.githubusercontent.com/NashLegend/ProjectBabel/master/images/title1.png)

[Material Design](http://www.google.com/design/spec/#utm_campaign=L-Developer-launch)对于多屏幕的视觉、交互和动作设计作了很大的改进。Android 5.0和最新的支持包可以帮助你创建Material风格的UI。这里将概述新的Material Design风格设计、新的API和组件，你可以将它们用在你的app中。

### Tangible surfaces ###

在Material Design的世界中，UI是由[电子纸和电子墨水](https://www.youtube.com/watch?v=YaG_ljfzeUw)构建起来的。这些UI的表面和阴影为应用程序的结构提供视觉线索（也就是根据样式能看出层次关系等等，比如下图），你看他的样子就知道哪里是可触摸的部分、就可以知道它将以何种方式运动（同样如下图）。这种Didital Material可以移动、扩张和变形以创建灵活的UI界面。 
![](https://raw.githubusercontent.com/NashLegend/ProjectBabel/master/images/layering.gif)

#### 阴影 ####

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

#### 颜色 ####
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
当你触摸Material元素的时候，它也可以[抬起（链接里的Lift on touch）](http://www.google.com/design/spec/animation/responsive-interaction.html#responsive-interaction-material-response)以迎合你的手指，就像磁铁异性相吸一样。你可以通过translationZ属性动画来实现这种效果，translationZ属性与elevation相似，不过它的主要作用是做这些过渡效果。 Z = elevation + translationZ. 新的stateListAnimator属性轻松创建触摸时的Z轴动画(Buttons默认就有这效果):

示例代码：

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

#### Reveal —— 打开内容 ####

Material风格的app显示新内容的一个典型过渡效果是一个向外扩散的圆形遮罩。过渡动画以用户点击位置为圆心发起并[向外扩散](http://www.google.com/design/spec/animation/responsive-interaction.html#responsive-interaction-radial-action)，你可以使用下面这种Animator来实现：
```
Animator reveal = ViewAnimationUtils.createCircularReveal(
                    viewToReveal, // The new View to reveal
                    centerX,      // x co-ordinate to start the mask from
                    centerY,      // y co-ordinate to start the mask from
                    startRadius,  // radius of the starting mask
                    endRadius);   // radius of the final mask
reveal.start();
```

#### Interpolators —— 插值器 ####
动作应该是合理的（deliberate）、迅速的、精确的。与普通的缓动效果不同的是，在Material Design里，物体倾向于快速开始然后缓动到最终位置。在动画过程中，物体在最终位置附近时的运动使用了更多的时间，因此用户不必要为了等待动画结束花费更多时间，动画的负面效果被降到最低。这种动作使用的是一个新的快进慢出的[插值器](https://developer.android.com/reference/android/R.interpolator.html?utm_campaign=L-Developer-launch#fast_out_slow_in)。

下面是一图胜千言时间：
![](https://raw.githubusercontent.com/NashLegend/ProjectBabel/master/images/interpolators.gif)

对于进入和离开屏幕的元素(which [should do so at peak velocity](http://www.google.com/design/spec/animation/authentic-motion.html#authentic-motion-mass-weight))，分别使用[linear-out-slow-in](https://developer.android.com/reference/android/R.interpolator.html?utm_campaign=L-Developer-launch#linear_out_slow_in) 分 [fast-out-linear-in](https://developer.android.com/reference/android/R.interpolator.html?utm_campaign=L-Developer-launch#fast_out_linear_in) 插值器。

### Adaptive design —— 自适应设计###

Material Design最后一个核心概念是创建一个单独的自适应设计布局就可以适配小到手表大到电视的所有尺寸和形状（手表有圆有方）。自适应设计设计技术帮助我们实现用同一个系统在不同设备上展示不同的外观。每个view都会自适应不同设备的尺寸的交互方式。 颜色、iconography,、层次、空间关系保持不变。Material Design系统提供了灵活的组件和模式以帮助你做到这一点。

#### Toolbar ####

Toolbar是ActionBar模式的一般化形式，它提供相似的功能以及更高的灵活性。与标准的ActionBar不同，Toolbar就在你的view的结构层次中，你可以像操作其他的view完全一样的操作它。你可以使用Activity.setActionBar()方法让它变成你的ActionBar。

![](https://raw.githubusercontent.com/NashLegend/ProjectBabel/master/images/contacts_toolbars.png)

在这个例子里，蓝色的Toolbar很高，被屏幕内容覆盖并且提供了一个导航按钮。注意另外两个Toolbar分别用在List和详细内容界面（说这么多无非是为了说明Toolbar很灵活）。

欲知更多Toolbar的使用方式，看[这里]https://github.com/NashLegend/ProjectBabel/blob/master/Material%20Design%20for%20Pre-Lollipop%20Devices.md)。

#### 开始让你的应用Material化吧 ####
Material Design帮助你创建易于理解的、漂亮的、自适应的、动起来的活的应用。希望这篇文章能让你动心并将Material Design应用到了你的app里。