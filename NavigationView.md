## DrawLayout和NavigationView

* fitsSystemWindows布局的时候是否考虑系统的比如状态栏，虚拟按键
---

1. 布局

		<android.support.v4.widget.DrawerLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    xmlns:app="http://schemas.android.com/apk/res-auto"
	    xmlns:tools="http://schemas.android.com/tools"
	    android:id="@+id/drawer_layout"
	    android:layout_width="match_parent"
	    android:layout_height="match_parent"
	    android:fitsSystemWindows="true"
	    tools:context=".MainActivity">
	    <LinearLayout
	        android:layout_width="match_parent"
	        android:layout_height="match_parent"
	        android:orientation="vertical">
	
	        <android.support.v7.widget.Toolbar
	            android:id="@+id/toolbar"
	            android:layout_width="match_parent"
	            android:layout_height="?attr/actionBarSize"
	            android:background="#30469b"/>
	        <!--内容显示布局-->
	        <FrameLayout
	            android:id="@+id/frame_content"
	            android:layout_width="match_parent"
	            android:layout_height="match_parent"/>
	    </LinearLayout>
	
	    <android.support.design.widget.NavigationView
	        android:id="@+id/navigation_view"
	        android:layout_width="wrap_content"
	        android:layout_height="match_parent"
	        android:layout_gravity="start"
	        app:headerLayout="@layout/navigation_header"
	        app:menu="@menu/drawer" />
	</android.support.v4.widget.DrawerLayout>

2. 代码

		//设置抽屉DrawerLayout 
        final DrawerLayout mDrawerLayout = (DrawerLayout) findViewById(R.id.drawer_layout);
		//绑定ActionBar上的按钮
        ActionBarDrawerToggle mDrawerToggle = new ActionBarDrawerToggle(this, mDrawerLayout, mToolbar,
                R.string.drawer_open, R.string.drawer_close);
        mDrawerToggle.syncState();//初始化状态
        mDrawerLayout.setDrawerListener(mDrawerToggle);

        //设置导航栏NavigationView的点击事件
        NavigationView mNavigationView = (NavigationView) findViewById(R.id.navigation_view);
        mNavigationView.setNavigationItemSelectedListener
