## Menu
	<?xml version="1.0" encoding="utf-8"?>
	<menu xmlns:android="http://schemas.android.com/apk/res/android">
	    <group
	        android:id="@+id/menu_content"
	        android:checkableBehavior="single">
	        <item
	            android:id="@+id/menu_drawer_home"
	            android:icon="@drawable/ic_drawer_home"
	            android:title="首页" />
	        <item
	            android:id="@+id/menu_drawer_explore"
	            android:icon="@drawable/ic_drawer_explore"
	            android:title="发现" />
	        <item
	            android:id="@+id/menu_drawer_follow"
	            android:icon="@drawable/ic_drawer_follow"
	            android:title="收藏" />
	        <item
	            android:id="@+id/menu_drawer_chat"
	            android:icon="@drawable/ic_drawer_chat"
	            android:title="评论" />
	    </group>
	
	    <group
	        android:id="@+id/menu_general">
	        <item
	            android:id="@+id/menu_general_about"
	            android:title="关于" />
	        <item
	            android:id="@+id/menu_general_setting"
	            android:title="设置" />
	
	        <item
	            android:id="@+id/menu_general_logout"
	            android:title="退出" />
	    </group>
	</menu>