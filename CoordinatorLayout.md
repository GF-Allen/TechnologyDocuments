### CoordinatorLayout

1. CoordinatorLayout 协调布局

2. CoordinatorLayout 的神奇之处就在于 Behavior 对象。怎么理解呢？CoordinatorLayout使得子view之间知道了彼此的存在，一个子view的变化可以通知到另一个子view，CoordinatorLayout 所做的事情就是当成一个通信的桥梁，连接不同的view，使用 Behavior 对象进行通信。
比如：在CoordinatorLayout中使用AppBarLayout，如果AppBarLayout的子View（如ToolBar、TabLayout）标记了app:layout_scrollFlags滚动事件,那么在CoordinatorLayout布局里其它标记了app:layout_behavior的子View（LinearLayout、RecyclerView、NestedScrollView等）就能够响应（如ToolBar、TabLayout）控件被标记的滚动事件


		<android.support.design.widget.CoordinatorLayout  
		    xmlns:android="http://schemas.android.com/apk/res/android"  
		    xmlns:app="http://schemas.android.com/apk/res-auto"  
		    android:id="@+id/coordinator_layout"  
		    android:layout_width="match_parent"  
		    android:layout_height="match_parent">  
		  
		    <android.support.design.widget.AppBarLayout  
		        android:id="@+id/appbar_layout"  
		        android:layout_width="match_parent"  
		        android:layout_height="wrap_content"  
		        android:fitsSystemWindows="true">  
		        <android.support.v7.widget.Toolbar  
		            android:id="@+id/toolBar"  
		            android:layout_width="match_parent"  
		            android:layout_height="?attr/actionBarSize"  
		            android:background="#30469b"  
		            app:layout_scrollFlags="scroll|enterAlways" />  
		        <android.support.design.widget.TabLayout  
		            ......  
		             />  
		    </android.support.design.widget.AppBarLayout>  
		  
		    <LinearLayout  
		        android:layout_width="match_parent"  
		        android:layout_height="match_parent"  
		        android:orientation="vertical"  
		        android:scrollbars="none"  
		        app:layout_behavior="@string/appbar_scrolling_view_behavior">   
		    </LinearLayout>  
		  
		</android.support.design.widget.CoordinatorLayout>

3. 上面这段代码中，ToolBar标记了layout_scrollFlags滚动事件，那么当LinearLayout滚动时便可触发ToolBar中的layout_scrollFlags效果。即往上滑动隐藏ToolBar，下滑出现ToolBar，而不会隐藏TabLayout，因为TabLayout没有标记scrollFlags事件，相反，如果TabLayout也标记了ScrollFlags事件，那么LinearLayout的下滑时ToolBar和TabLayout都会隐藏了。
4. layout_scrollFlags中的几个值：
	* scroll: 所有想滚动出屏幕的view都需要设置这个flag， 没有设置这个flag的view将被固定在屏幕顶部。
	* enterAlways:这个flag让任意向下的滚动都会导致该view变为可见，启用快速“返回模式”。
	* enterAlwaysCollapsed:当你的视图已经设置minHeight属性又使用此标志时，你的视图只能已最小高度进入，只有当滚动视图到达顶部时才扩大到完整高度。 
	* exitUntilCollapsed:滚动退出屏幕，最后折叠在顶端。  