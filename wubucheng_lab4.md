#            Material Design简单使用

#### 一、滑动菜单DrawerLayout的使用
- **1.1DrawerLayout定义**
	>DrawerLayout是实现炫酷的滑动菜单的控件，常见的类似QQ的向右滑出菜单栏。
- **1.2在项目中的使用**
		
   （1）首先新建一个布局，它放入两个子控件，一个是主屏幕显示的内容-，另一个是滑动菜单中显示的内容

 ```xml
	    <?xml version="1.0" encoding="utf-8"?>
	    <android.support.v4.widget.DrawerLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    xmlns:app="http://schemas.android.com/apk/res-auto"
	    android:layout_width="match_parent"
	    android:layout_height="match_parent"
	    android:id="@+id/dl_left">


	    <include layout="@layout/activity_net1414080903220_rg"/>
	
	    <android.support.design.widget.NavigationView
	        android:layout_width="match_parent"
	        android:layout_height="match_parent"
	        android:layout_gravity="start"
	        app:menu="@menu/nav_menu"
	        app:headerLayout="@layout/net1414080903220_nav_header"
	        >
	
	    </android.support.design.widget.NavigationView>
	
	
	    </android.support.v4.widget.DrawerLayout>
```
上述布局中将`layout_gravity`设置为start，这个是根据系统语言来判断滑动菜单是在左边还是右边的。布局中引用了一个布局`activity_net1414080903220_rg`，此布局正是主屏幕显示的内容，这里不再赘述，下面重点来讲NavigationView这个Design Support库中的控件，一般来讲项目中直接就可以使用即可，如果报错，可以在build.gradle中添加如下内容：
`compile 'com.android.support:design:25.2.0'`
在NavigationView中可以看到`app:menu="@menu/nav_menu"`和`app:headerLayout`这项内容，而nav_menu正是我们的菜单列表，代码如下：
	

```xml	
		<?xml version="1.0" encoding="utf-8"?>
		<menu xmlns:android="http://schemas.android.com/apk/res/android">
		<group android:checkableBehavior="single">
			<item
			android:id="@+id/nav_about"
			android:icon="@drawable/vector_drawable_icon_about_net1414080903220"
			android:title="@string/about"
		/>
		</group>
		</menu>
```		
	该控件定义菜单列表条目，这里仅仅只做了一个“关于”的选项
	app:headerLayout的代码如下

```xml
		<?xml version="1.0" encoding="utf-8"?>
		<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
		android:layout_width="match_parent"
		android:layout_height="180dp"
		android:padding="10dp"
		android:background="?attr/colorPrimary"
		>
			<ImageView
			android:layout_width="60dp"
			android:layout_height="60dp"
			android:src="@drawable/img_head_net1414080903220"
			android:layout_centerInParent="true"
			/>
			<TextView
			android:id="@+id/tv_number"
			style="@style/nav_text"
			android:text="@string/studentNumber"
			/>
			
			<TextView
			android:id="@+id/tv_author"
			style="@style/nav_text_author"
			android:text="@string/author"
			android:layout_above="@id/tv_number"/>
		
		</RelativeLayout>
```
(2)在Java代码中可以打开DraweLayout的方法为openDrawer,参数为Gravity参数，这里需要与xml中保持一致，因此在这里就是这样：`drawerLayout.openDrawer(GravityCompat.START);`

实现的效果如下：


![image](http://oq3l0bzx8.bkt.clouddn.com/2.png)


-------------------

#### 二、TabLayout的使用
- **2.1TabLayout的定义**
>TabLayout是实现Material Design效果的选项卡的控件。

- **2.2在项目中的使用**
	
（1）新建一个给Fragment使用的布局，代码如下：

```xml
		<?xml version="1.0" encoding="utf-8"?>
		<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
		xmlns:app="http://schemas.android.com/apk/res-auto"
		android:layout_width="match_parent"
		android:layout_height="match_parent"
		android:id="@+id/fragment_dev_article"
		android:orientation="vertical">
		
			<edu.hzuapps.androidlabs.homeworks.net1414080903220.knowdev.widget.KnowDevToolbar
			android:id="@+id/knowDevToolbar"
			android:layout_width="match_parent"
			android:layout_height="wrap_content"
			android:background="?attr/colorPrimary"
			android:minHeight="?attr/actionBarSize"
			android:layout_alignParentTop="true"
			android:layout_alignParentStart="true"
			app:isHideShareBt="true"
			app:title="@string/dev_article"
			>
			</edu.hzuapps.androidlabs.homeworks.net1414080903220.knowdev.widget.KnowDevToolbar>
			
			<android.support.design.widget.TabLayout
			android:id="@+id/tab_dev_layout"
			android:layout_width="match_parent"
			android:layout_height="wrap_content"
			style="@style/customTabLayout"
			app:tabGravity="fill"
			app:tabMode="fixed"
			android:layout_alignParentTop="true"
			android:layout_alignParentStart="true">
			
			</android.support.design.widget.TabLayout>
			
		
			<android.support.v4.view.ViewPager
			android:id="@+id/vpager"
			android:layout_width="match_parent"
			android:layout_height="match_parent"		
			/>
		
		</LinearLayout>

```
(2)上面的KnowDevToolbar控件是自定义的一个Toolbar，暂时可以不用管，重点来看TabLayout这个控件。在TabLayout中指定style样式为customTabLayout，代码如下：

```xml
    <style name="customTabLayout" parent="Widget.Design.TabLayout">
        <item name="tabIndicatorColor">?attr/colorPrimary</item>
        <item name="tabIndicatorHeight">2dp</item>
        <item name="tabPaddingStart">12dp</item>
        <item name="tabPaddingEnd">12dp</item>
        <item name="tabBackground">?attr/colorPrimary</item>
        <item name="android:textSize">14sp</item>
        <item name="textAllCaps">true</item>
        <item name="tabSelectedTextColor">#fff</item>
    </style>
  ```
(3)它指定了指示器颜色，tab的高度、内容边距及大小、点击颜色效果等，下面重点来看Java代码：
在fragment代码中先定义了tablayout中的每个tab的内容：

```java
			int texts[]={R.string.android,R.string.ios,R.string.web,R.string.expand};
			int tags[]={TAG_ANDROID,TAG_IOS,TAG_WEB,TAG_EXPAND};
```			
后初始化tablayout,给每个tab设置内容和标签：

```java
    private void initTab(View view) {
        devTypeTabLayout=(TabLayout)view.findViewById(R.id.tab_dev_layout);

        for(int i=0;i<texts.length;i++){
            TabLayout.Tab tab=devTypeTabLayout.newTab();
            tab.setText(texts[i]);
            tab.setTag(tags[i]);
            devTypeTabLayout.addTab(tab);
        }
        devTypeTabLayout.addOnTabSelectedListener(this);
    }
 ```
注意上面代码中添加了tablayout的点击事件addOnTabSelectedListener，因此需要实现`TabLayout.OnTabSelectedListener`，有三个方法需要实现：tab点击时：`onTabSelected(TabLayout.Tab tab)`,tab没由点击时：`onTabUnselected(TabLayout.Tab tab)`和在当前tab页再次点击时：`onTabReselected(TabLayout.Tab tab)`，我们可以根据项目选择性的实现其中的方法就好了。


![image](http://oq3l0bzx8.bkt.clouddn.com/3.png)


-------------------	
#### 三、CardView的使用
- **3.1CardView的定义**
>CardView是实现卡片式布局效果的控件，实际上是一个FrameLayout，添加了圆角、阴影等效果，看起有立体的效果

- **3.2在项目的使用**

（1）以每日头条中的布局为例，代码中只使用了cardview的三个属性，而cardview属性不仅仅这有这些，具体可以参考官方文档。代码如下：

```xml
<?xml version="1.0" encoding="utf-8"?>
	<android.support.v7.widget.CardView xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:card_view="http://schemas.android.com/apk/res-auto"
    android:id="@+id/card_view"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:layout_marginLeft="2dp"
    android:layout_marginRight="2dp"
    android:layout_marginTop="2dp"
    card_view:cardBackgroundColor="@android:color/white"
    card_view:cardCornerRadius="5dp"
    card_view:cardElevation="2dp">

        <RelativeLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:paddingTop="5dp"
            >
            <ImageView
                android:id="@+id/img_devarticle"
                android:layout_width="300px"
                android:layout_height="300px"
                android:src="@drawable/example_dev_thumb1414080903220"/>

            <TextView
                android:id="@+id/title_article"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_toRightOf="@id/img_devarticle"
                android:paddingTop="15dp"
                android:paddingRight="3dp"
                android:maxEms="15"
                android:maxLines="5"
                android:ellipsize="end"
                android:hint="基于 RxSwift 实现的 State 状态管理工具"
                android:layout_gravity="center"
                android:gravity="center"
                />

            <TextView
                android:id="@+id/tv_author"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="Allen"
                android:textSize="10sp"
                android:paddingLeft="5dp"
                android:layout_marginBottom="5dp"
                android:layout_toRightOf="@id/img_devarticle"
                android:layout_alignBottom="@id/img_devarticle"
                />

            <TextView
                android:id="@+id/time_article"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="于2017-05-15"
                android:textSize="10sp"
                android:layout_marginBottom="5dp"
                android:layout_toRightOf="@id/tv_author"
                android:layout_alignBottom="@+id/img_devarticle"
                 />
            <TextView
                android:id="@+id/tv_type"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_alignParentRight="true"
                android:layout_alignParentEnd="true"
                android:layout_alignBottom="@id/img_devarticle"
                android:layout_marginBottom="5dp"
                android:textSize="10sp"
                android:paddingRight="5dp"
                android:text="@string/android"/>
        </RelativeLayout>

	</android.support.v7.widget.CardView>
```
（2）上面的代码关注CardView，它在这里是作为父布局的，注意到要使用cardview的属性方法，要添加这行代码：` xmlns:card_view="http://schemas.android.com/apk/res-auto"`其中`card_view`这个名字是可以替换的，在使用cardview的属性时，在前面加上对应的名字就好啦。这里我只设置CardView的背景颜色、圆角大小和阴影。具体效果如下：


![image](http://oq3l0bzx8.bkt.clouddn.com/4.png)


-------------------	



#### 四、Toolbar的使用
- **4.1Toolbar定义**

	> Toolbar是用来设置标题栏的，类似于actionbar，但比action更为强大，可以实现material design效果

	
- **4.2在项目中的使用**
	
(1)在本次项目中，我自定义了toolbar，以符合我项目中的效果，下面来简单介绍如何简单自定义了toolbar.

 ```xml   
    <?xml version="1.0" encoding="utf-8"?>
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

	    <Button
	        android:id="@+id/toolbar_menu"
	        android:layout_width="30dp"
	        android:layout_height="30dp"
	        android:layout_alignParentLeft="true"
	        android:layout_marginLeft="10dp"
	        android:layout_marginTop="5dp"
	        android:background="@drawable/icon_menu_net1414080903220"
	        android:visibility="visible"/>
	
	    <TextView
	        android:id="@+id/toolbar_title"
	        android:layout_width="0dp"
	        android:layout_height="wrap_content"
	        android:layout_weight="1"
	        android:textSize="23sp"
	        android:gravity="center"
	        android:layout_marginTop="1dp"
	        android:text="@string/news" />
	
	    <Button
	        android:id="@+id/toolbar_share"
	        android:layout_width="30dp"
	        android:layout_height="30dp"
	        android:layout_alignParentRight="true"
	        android:layout_marginRight="10dp"
	        android:background="@drawable/vector_drawable_icon_share"
	        android:visibility="visible"/>
    </LinearLayout>`
 ```   
布局中定义了两个Button和一个Textiew,分别作为菜单键，分享键和标题显示，可以看到目前效果如下


![image](http://oq3l0bzx8.bkt.clouddn.com/1.png)


(2)在values文件夹下新建attr文件，代码如下：
```xml
		<?xml version="1.0" encoding="utf-8"?>
		<resources>
		<declare-styleable name="KnowDevToolBar">
		<attr name="toolbarMenuIcon" format="reference"/>
		<attr name="toolbarTile" format="reference"/>
		<attr name="toolbarShareIcon" format="reference"/>
		<attr name="isHideShareBt" format="boolean"/>
		</declare-styleable>
		</resources>
```		
  这里上面自定义toolbar的属性，及其format，format为reference，代表资源文件的引用，如引用图片等，boolean为true或false值。

(3)在Java代码中，新建一个类叫KnowDevToolbar继承Toolbar，对其构造方法进行操作：

```java
    public KnowDevToolbar(Context context) {
        this(context,null);
    }

    public KnowDevToolbar(Context context, @Nullable AttributeSet attrs) {
        this(context, attrs, 0);
    }

    public KnowDevToolbar(Context context, @Nullable AttributeSet attrs, int defStyleAttr) {}
```
重点来看第三个构造方法：第一个参数为上下文，这个没有什么好讲，第二个参数为属性集合，简单的理解为你使用自定义控件时，给其中的属性进行的赋值，产生的集合，第三个参数为默认样式集合，这里可以不管，第三个构造方法的具体内容如下：

```java
    public KnowDevToolbar(Context context, @Nullable AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        initView();
       if(attrs!=null){

           final TintTypedArray tintTypedArray = TintTypedArray.obtainStyledAttributes(getContext(), attrs, R.styleable.KnowDevToolBar, defStyleAttr, 0);
           final Drawable menuIcon=tintTypedArray.getDrawable(R.styleable.KnowDevToolBar_toolbarMenuIcon);
           final Drawable shareIcon=tintTypedArray.getDrawable(R.styleable.KnowDevToolBar_toolbarShareIcon);
           boolean isHideShareBt=tintTypedArray.getBoolean(R.styleable.KnowDevToolBar_isHideShareBt,false);
           if(menuIcon!=null){
               setMenuIcon(menuIcon);
           }
           if(shareIcon!=null){
               setShareIcon(shareIcon);
           }
           if(isHideShareBt){
               isHideShareBt(true);
           }
           tintTypedArray.recycle();
       }

    }
```
可以看到我们使用`TintTypedArray`对`styleable`进行读取，styleable的名称就是前面attr中`declare-styleable`的name，实例化TintTypedArray后，就可以用其对象使用相应方法读取相应的属性值，传进去的参数正是styleable的attr name，注意，在后面需要将TintTypedArray回收，回收方法为recycle。上面的三个判断是否为空的方法是用来进行相应的设置操作的，代码如下:

```java	
	//设置菜单按钮图标
    public void setMenuIcon(int id){
        menuBt.setBackgroundResource(id);
    }

    public void setMenuIcon(Drawable menuIcon) {
        if(menuBt!=null){
            menuBt.setBackground(menuIcon);
        }
    }
	
	//设置title内容
    @Override
    public void setTitle(int toolTile) {
        setTitle(getContext().getText(toolTile));

    }

    @Override
    public void setTitle(CharSequence tooltitle){
        initView();
        if(title!=null){
            title.setText(tooltitle);
        }

    }
	
	//设置分享按钮图标
    public void setShareIcon(Drawable menuIcon) {
        if (shareBt != null) {
            shareBt.setBackground(menuIcon);
        }
    }
	
	//设置是否隐藏分享图标
    public void isHideShareBt(boolean flag){
        if(shareBt!=null){
            if(flag==true){
                shareBt.setVisibility(GONE);
            }else{
                shareBt.setVisibility(VISIBLE);
            }

        }
    }
	
	//设置菜单按钮点击事件
    public  void setMenuButtonOnClickListener(OnClickListener listen){
        menuBt.setOnClickListener(listen);

    }

	//设置分享按钮点击事件
    public  void setShareButtonOnClickListener(OnClickListener listen){
        shareBt.setOnClickListener(listen);
    }
```
上面的代码比较简单，主要作用就是暴露方法给调用。
(3)下面来讲一下如何使用,拿2.2中的布局为例：

```xml
	 <edu.hzuapps.androidlabs.homeworks.net1414080903220.knowdev.widget.KnowDevToolbar
	        android:id="@+id/knowDevToolbar"
	        android:layout_width="match_parent"
	        android:layout_height="wrap_content"
	        android:background="?attr/colorPrimary"
	        android:minHeight="?attr/actionBarSize"
	        android:layout_alignParentTop="true"
	        android:layout_alignParentStart="true"
	        app:isHideShareBt="true"
	        app:title="@string/dev_article"
	        >
	    </edu.hzuapps.androidlabs.homeworks.net1414080903220.knowdev.widget.KnowDevToolbar>
```	    
在布局文件中加上包名和自定义控件的名称，形成一个全新的控件，我们在这里只对其隐藏掉分享按钮，还有设置toolbar的标题，而background和minHeight我们引用了系统自身的attr。
在Java代码中，我们可以这样使用：

```java
		//以回调的形式打开DrawerLayout
        knowDevToolbar.setMenuButtonOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                drawerLayout.openDrawer(GravityCompat.START);
            }
        })
```
由于在布局中已经可以设置了相应的属性，因此可以不必调用设置是否隐藏图标的方法，当然你也可以这样用，例如：

```java
        knowDevToolbar.setMenuIcon(R.drawable.vector_back_net1414080903220);
        knowDevToolbar.setTitle("头条新闻");
```


-------------------	
#### 五、总结
以上内容就是本次项目中使用到的Material Design的知识了，由于本人水平有限，上述内容可能会有纰漏，大家以官方文档为准，且上面的代码是本人实际项目中用到的，并不一定适合其他项目，但是知识都是相通，最后附上几个Material Design的配色和图片网站：

[https://material.io/icons/](https://material.io/icons/)

[https://www.materialpalette.com/](https://www.materialpalette.com/)

[https://dribbble.com/](https://dribbble.com/)

[https://www.toptal.com/designers/subtlepatterns/](https://www.toptal.com/designers/subtlepatterns/)

pull request地址：[https://github.com/hzuapps/android-labs-2017/pull/338](https://github.com/hzuapps/android-labs-2017/pull/338)

**By:wubucheng**

**2017.6.23**

