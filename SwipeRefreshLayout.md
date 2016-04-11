### SwipeRefreshLayout

1. 布局文件

		<?xml version="1.0" encoding="utf-8"?>
		<android.support.v4.widget.SwipeRefreshLayout xmlns:android="http://schemas.android.com/apk/res/android"
		    android:id="@+id/refresh"
		    android:layout_width="match_parent"
		    android:layout_height="match_parent"
		    android:orientation="vertical">
		
		    <LinearLayout
		        android:layout_width="match_parent"
		        android:layout_height="match_parent">
		
		        <android.support.v7.widget.RecyclerView
		            android:id="@+id/rv_photo"
		            android:layout_width="match_parent"
		            android:layout_height="match_parent" />
		
		    </LinearLayout>
		</android.support.v4.widget.SwipeRefreshLayout>

2. 方法

		refresh.setColorSchemeColors(Color.BLUE, Color.RED);//设置转动时的颜色变化
        refresh.setOnRefreshListener(new SwipeRefreshLayout.OnRefreshListener() {
            @Override
            public void onRefresh() {
                refreshData();
            }
        });//正在刷新时，执行的操作

3. 注意
	
	*. 要刚进页面是显示刷新

		refresh.post(new Runnable() {
            @Override
            public void run() {
                refresh.setRefreshing(true);
            }
        });

	*. 加载完数据后，设置不显示
	
 		refresh.setRefreshing(false);