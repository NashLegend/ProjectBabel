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

### Print-like Design ###
Material utilises classic principles from print design to create clean, simple layouts that put your content front and center. Bold deliberate color choices, intentional whitespace, tasteful typography and a strong baseline grid create hierarchy, meaning and focus.

#### Typography ####
Android 5.0 updates the system font Roboto to beautifully and clearly display text no matter the display size. A new medium weight has been added (android:fontFamily=”sans-serif-medium”) and new TextAppearance styles implement the recommended typographic scale for balancing content density and reading comfort. For instance you can easily use the ‘Title’ style by setting android:textAppearance=”@android:style/TextAppearance.Material.Title”. These styles are available on older platforms through the AppCompat support library, e.g. “@style/TextAppearance.AppCompat.Title”.

#### Color ####
![](https://raw.githubusercontent.com/NashLegend/ProjectBabel/master/images/color_attribs.png)

Your application’s color palette brings branding and personality to your app so we’ve made it simple to colorize UI controls by using the following theme attributes:

- colorPrimary. The primary branding color for the app; used as the action bar background, recents task title and in edge effects.
- colorAccent. Vibrant complement to the primary branding color. Applied to framework controls such as EditText and Switch.
- colorPrimaryDark. Darker variant of the primary branding color; applied to the status bar.

Further attributes give fine grained control over colorizing controls, see: `colorControlNormal, colorControlActivated, colorControlHighlight, colorButtonNormal, colorSwitchThumbNormal, colorEdgeEffect, statusBarColor `和` navigationBarColor.`

AppCompat provides a large subset of the functionality above, allowing you to colorize controls on pre-Lollipop platforms.

#### Dynamic color ####
 
Material Design encourages dynamic use of color, especially when you have rich images to work with. The new Palette support library lets you extract a small set of colors from an image to style your UI controls to match; creating an immersive experience. The extracted palette will include vibrant and muted tones as well as foreground text colors for optimal legibility. For example:
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
![](https://raw.githubusercontent.com/NashLegend/ProjectBabel/master/images/palette2.gif)

### Authentic Motion ###
Tangible surfaces don’t just appear out of nowhere like a jump-cut in a movie; they move into place helping to focus attention, establish spatial relationships and maintain continuity. Materials respond to touch to confirm your interaction and all changes radiate outward from your touch point. All motion is meaningful and intimate, aiding the user’s comprehension.

#### Activity + Fragment Transitions ####
By declaring ‘shared elements’ that are common across two screens you can create a smooth transition between the two states.
![](https://raw.githubusercontent.com/NashLegend/ProjectBabel/master/images/activity_transitions.gif)
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

Here we define the same transitionName in two screens. When starting the new Activity and this transition is animated automatically. In addition to shared elements, you can now also choreograph entering and exiting elements.

#### Ripples ####