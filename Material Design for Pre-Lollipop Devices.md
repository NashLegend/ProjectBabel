在低版本Android系统的上使用Material Design
===========================
*By Chris Banes, Android Developer Relations*

*译自http://android-developers.blogspot.com/2014/10/appcompat-v21-material-design-for-pre.html—— By [NashLegend](https://github.com/NashLegend)*

![](http://i.imgur.com/4Iz0yK2.png)

Android 5.0已经发布，带来了新的Material Design，这种新的设计语言提供了更好的视觉体验。为了使旧版本的Android系统也可以使用这种设计，我们扩展了支持包，对其中的[AppCompat](https://developer.android.com/tools/support-library/features.html#v7-appcompat)进行了重大更新，同时带来了新的[RecyclerVier](https://developer.android.com/tools/support-library/features.html#v7-recyclerview),[CardView](https://developer.android.com/tools/support-library/features.html#v7-recyclerview)以及[Palette](https://developer.android.com/tools/support-library/features.html#v7-palette)库。

这篇文章中我们会介绍一下AppCompat发生了哪些新变化以及如何用它设计Material Design应用.

#### What's new in AppCompat? ####

AppCompat（又叫ActionBarCompat）为4.0前的Android版本提供ActionBar后向兼容。而最新的AppCompat v21则可以提供Android 5.0特性支持。

在这个版本中，Android引入了新的 [ToolBar](https://developer.android.com/reference/android/support/v7/widget/Toolbar.html)控件。类似于ActionBar,但是提供了更多的控制权和灵活性。ToolBar就像其他的普通控件一样，因此你可以容易地对其布局、执行动画以及与滚动事件交互。你也可以把它设置成你的Activity的ActionBar——就和原先的ActionBar一样使用。

最新的AppCompat包含了多种Meterial Design样式更新。比如[Google I/O 2014应用](https://github.com/google/iosched)就使用了这些东东.

下面是如何使用它

#### 开始使用 ####
如果你使用Gradle，把appcompat加入到依赖库。修改你的build.gradle：
```
dependencies {
	compile "com.android.support:appcompat-v7:21.0.+"
}
```
##### 如果你的App首次使用AppCompat #####

如果你没用过AppCompat，那么下面教给你：

- 所有的Activity必须继承[ActionBarActivity](https://developer.android.com/reference/android/support/v7/app/ActionBarActivity.html)，ActionBarActivity继承自v4包的[FragmentActivity](https://developer.android.com/reference/android/support/v4/app/FragmentActivity.html)，因此你仍然可以使用Fragment.
- 所有theme必须继承`Theme.AppCompat`——这个主题包含一些不同的子主题，比如`Light`和`NoActionBar`。
- 如果你要inflate一些元素（比如说一个列表）以展示在ActionBar上的话，请确保你使用的是ActionBar的themed context——使用getSupportActionBar().getThemedContext()方法获得这个context。
- 对MenuItem进行相关操作时，必须[MenuItemCompat](https://developer.android.com/reference/android/support/v4/view/MenuItemCompat.html)的静态方法

For more information, see the Action Bar API guide which is a comprehensive guide on AppCompat.
欲知更多更深入的知识，请看 [ActionBar指南](http://developer.android.com/guide/topics/ui/actionbar.html)

##### 如果你的App之前就使用AppCompat #####

对于大多数的应用来说，你只要在`values/`里声明一个主题就可以了：
```
values/themes.xml:

<style name="Theme.MyTheme" parent="Theme.AppCompat.Light">
    <!-- Set AppCompat’s actionBarStyle -->
    <item name="actionBarStyle">@style/MyActionBarStyle</item>
    
    <!-- Set AppCompat’s color theming attrs -->
    <item name=”colorPrimary”>@color/my_awesome_red</item>
    <item name=”colorPrimaryDark”>@color/my_awesome_darker_red</item>
    
    <!-- The rest of your attributes -->
</style>
```

完了你就可以把`values-v14+`的ActionBar样式移除了。

#### 设置主题 ####

AppCompat已经支持新的[color palette theme](http://developer.android.com/training/material/theme.html#ColorPalette)属性，使用这些属性，你可以很轻易地讲你的应用色调与[primary and accent colors](http://www.google.com/design/spec/style/color.html#color-ui-color-application)搭配。举个栗子：
```
values/themes.xml:

<style name="Theme.MyTheme" parent="Theme.AppCompat.Light">
    <!-- colorPrimary is used for the default action bar background -->
    <item name=”colorPrimary”>@color/my_awesome_color</item>
    
    <!-- colorPrimaryDark is used for the status bar -->
    <item name=”colorPrimaryDark”>@color/my_awesome_darker_color</item>
    
    <!-- colorAccent is used as the default value for colorControlActivated,
         which is used to tint widgets -->
    <item name=”colorAccent”>@color/accent</item>
    
    <!-- You can also set colorControlNormal, colorControlActivated
     colorControlHighlight, and colorSwitchThumbNormal. -->
</style>
```

在Android 5.0以上系统上，设置了上面这些后，跟使用了Material主题一样了，你不必单独设置Material主题，它会自动修改状态栏、最近运行屏幕等的颜色。

而在早期版本中，AppCompat会尽可能的模拟Material色彩主题。现阶段时能作用于ActionBar和某些组件上——也就是说，对于以前版本的Android系统来说，就算使用了AppCompat也不可能实现所有的Android 5.0样式。

##### *Widget着色*（Widget tinting） #####

当设备的版本在Android 5.0以上时，所有的组件都会使用我们刚才设置的主题颜色进行着色，这是因为Android 5.0支持正面这两种特性（5.0之前系统的悲剧）：*drawable着色*（drawable tinting）和在drawable中引用主题属性(使用?attr/xxx方式)。

对于早期版本的Android系统，AppCompat为下列UI组件提供了类似的行为。

- AppCompat’s toolbar中的一切 (action modes等等)
- EditText
- Spinner
- CheckBox
- RadioButton
- Switch (要使用新的[android.support.v7.widget.SwitchCompat](https://developer.android.com/reference/android/support/v7/widget/SwitchCompat.html))
- CheckedTextView

AppCompat会帮助你实现这些行为，你完全不需要插手（有些要注意的地方，看下面的FAQ）。

#### Toolbar组件 ####

![](http://i.imgur.com/QH2tBHt.png)

AppCompat支持完全特性的`Toolbar`组件——[android.support.v7.widget.Toolbar](https://developer.android.com/reference/android/support/v7/widget/Toolbar.html)——和Android5.0系统的`Toolbar`完全一样。要两种使用ToolBar的方式：

- 如果你想要使用现有的ActionBar功能（比如使用菜单、ActionBarDrawerToggle等等）同时又想自己掌握它的样式，那么你可以把ToolBar用作ActionBar。
- 对于一些ActionBar所不支持的场景——比如说在同一屏幕上使用多个ToolBar、使用低于屏幕宽度的尺寸的ToolBar等等（看上图就知道了）——你就要使用ToolBar了。

##### Action Bar #####

要将ToolBar用作ActionBar，首先要禁用系统ActionBar，最方便的方式是使用`Theme.AppCompat.NoActionBar`主题（或者light版的NoActionBar）.

然后创建一个ToolBar实例，通常是写在XML里。

```
<android.support.v7.widget.Toolbar
    android:id=”@+id/my_awesome_toolbar”
    android:layout_height=”wrap_content”
    android:layout_width=”match_parent”
    android:minHeight=”?attr/actionBarSize”
    android:background=”?attr/colorPrimary” />
```

宽、高、背景等你完全可以自定义；由于ToolBar只是一个ViewGroup，因此你可以任意定义它的样式的位置。

然后在Activity里面将ToolBar设置为ActionBar就行了：

```
@Override
public void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.blah);

    Toolbar toolbar = (Toolbar) findViewById(R.id.my_awesome_toolbar);
    setSupportActionBar(toolbar);
}
```

然后options menu就会显示在ToolBar上了。

##### Standalone模式 #####

standalone模式与actionbar模式的不同是你不必setSupportActionBar了，把它当一个普通组件看待就好，也不必非得把ActionBar给禁用了。

standalone模式下，你就得手动设置ToolBar的内容和动作了。比如说，如果你想让它显示一个动作按钮（就像Actoinbar一样，但不是actionbar模式），你可以这么做：

```
@Override
public void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.blah);

    Toolbar toolbar = (Toolbar) findViewById(R.id.my_awesome_toolbar);

    // Set an OnMenuItemClickListener to handle menu item clicks
    toolbar.setOnMenuItemClickListener(new Toolbar.OnMenuItemClickListener() {
        @Override
        public boolean onMenuItemClick(MenuItem item) {
            // Handle the menu item
            return true;
        }
    });

    // Inflate a menu to be displayed in the toolbar
    toolbar.inflateMenu(R.menu.your_toolbar_menu);
}
```

ToolBar还支持很多其他功能，欲知更多，请查看[它的API](https://developer.android.com/reference/android/support/v7/widget/Toolbar.html)。

##### Styling #####

ToolBar的样式的设置方式和标准的ActionBar是不一样的，ToolBar的样式是直接设置在这个View上的。

下面是一个要作为ActionBar使用的ToolBar的基本样式。

```
<android.support.v7.widget.Toolbar  
    android:layout_height="wrap_content"
    android:layout_width="match_parent"
    android:minHeight="?attr/actionBarSize"
    app:theme="@style/ThemeOverlay.AppCompat.ActionBar" />
```

app:theme可以确保你的文本的里面的items使用solid colors。

###### DarkActionBar ######
你也可以直接通过而已属性设置ToolBar的Style。要得到一个看起来像'DarkActionBar'（深色内容、浅色菜单）样式的ToolBar,可以像下面设置theme和popupTheme属性：

```
<android.support.v7.widget.Toolbar
    android:layout_height=”wrap_content”
    android:layout_width=”match_parent”
    android:minHeight=”@dimen/triple_height_toolbar”
    app:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
    app:popupTheme="@style/ThemeOverlay.AppCompat.Light" />
```

#### SearchView Widget ####

AppCompat提供了最新与Lollipop一样的SearchView样式和API，使其具有更强的可定制性（此处应该有掌声）. 

你应该这样定义SearchView的样式：

```
values/themes.xml:
<style name=”Theme.MyTheme” parent=”Theme.AppCompat”>
    <item name=”searchViewStyle”>@style/MySearchViewStyle</item>
</style>
<style name=”MySearchViewStyle” parent=”Widget.AppCompat.SearchView”>
    <!-- Background for the search query section (e.g. EditText) -->
    <item name="queryBackground">...</item>
    <!-- Background for the actions section (e.g. voice, submit) -->
    <item name="submitBackground">...</item>
    <!-- Close button icon -->
    <item name="closeIcon">...</item>
    <!-- Search button icon -->
    <item name="searchIcon">...</item>
    <!-- Go/commit button icon -->
    <item name="goIcon">...</item>
    <!-- Voice search button icon -->
    <item name="voiceIcon">...</item>
    <!-- Commit icon shown in the query suggestion row -->
    <item name="commitIcon">...</item>
    <!-- Layout for query suggestion rows -->
    <item name="suggestionRowLayout">...</item>
</style>
```

你不必设置所有上面的属性，甚至一个都不设置也行，默认的就很不错。

#### 结语 ####

希望这篇文章能让你学会使用AppCompat并开发出牛逼的Material Design风格的app。

#### FAQ ####

##### 为啥我的老Android系统上的EditText（或者上面提到的支持的控件）长得不像Material风格呢？ #####

AppCompat的工作原理是拦截layout inflation并且在对应的位置插件一个特殊的*可使用Material样式的组件*（tint-aware version of the widget）来实现的。对于大部分人来说这应该没啥问题，但是下面几种情况有可能导致一些问题：
  - 你使用自己的自定义的widget（比如使用的自定义的继承了EditText的类）。
  - 你创建EditText的时候没有给它LayoutInflater（比如直接New EditText()）

这些特殊的组件目前被我们藏起来了，因为还没有完全做完。以后可能会好点……

#####为何某某组件在我的老Android系统上长得不像Material风格?#####

目前AppCompat只支持常用的组件的Material主题，其他的以后会有的。

#####为啥我的ActionBar在Android 5.0上的阴影，我已经把android:windowContentOverlay设置为null了啊？#####

Android 5.0上，ActionBar的阴影是由最新的elevation API提供的。如果不想要它，可以使用`getSupportActionBar().setElevation(0)`方法，或者在Action Bar style里面修改`elevation`属性。

#####为啥我的老版本Android没有波纹效果？#####

这东西只有用Android 5.0最新的RenderThread才能跑流畅，为了不让你卡出病来，我们现在还是先不给老版本支持这玩意儿。

#####我使用了AppCompat后还能用Preference么？#####

在API v11+版本的机器上，你仍然可以在ActionBarActivity中使用PreferenceFragment。对于更老的老爷机来说，你只能使用普通的非Material样式的PreferenceActivity。