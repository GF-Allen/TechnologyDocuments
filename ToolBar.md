### toolbar

1. toolbar的使用需要将现有的ActionBar取消
		
		android:theme="@style/AppTheme.NoActionBar" //通过设置主题


		//在Activity中设置
		setSupportActionBar(toolbar);

2. toolbar遮盖内容（CoordinatorLayout）
			
		/在内容的直接子View中增加
		app:layout_behavior="@string/appbar_scrolling_view_behavior"

3. CollapsingToolbarLayout折叠ToolBar布局
4. 使用CollapsingToolbarLayout实现折叠效果，需要注意3点 
	
	* AppBarLayout的高度固定 
	* CollapsingToolbarLayout的子视图设置，layout_collapseMode属性 
	* 关联悬浮视图设置app:layout_anchor，app:layout_anchorGravity属性
		
			<android.support.design.widget.FloatingActionButton
	        android:layout_height="wrap_content"
	        android:layout_width="wrap_content"
	        app:layout_anchor="@id/appbar"//设置锚点在什么布局
	        app:layout_anchorGravity="bottom|right|end"//设置锚点在该布局的什么位置
	        android:src="@drawable/ic_done"
	        android:layout_margin="@dimen/fab_margin"
	        android:clickable="true"/>
			
5. 使用Toolbar

		private void initToolBar() {
			toolbar.setTitle("");//设置标题，需要设置actionBar之前
			setSupportActionBar(toolbar);
			ActionBar bar = getSupportActionBar();
			bar.setDisplayHomeAsUpEnabled(true);//设置返回箭头
		}
		
		/**通过actionBar设置的返回箭头，相应的点击事件**/
		@Override
		public boolean onOptionsItemSelected(MenuItem item) {

			switch (item.getItemId()) {
				case android.R.id.home:
					finish();
					break;
			}
			return true;
		}
		
		/**设置toolBar上的menu的点击监听**/
		toolbar.setOnMenuItemClickListener(new Toolbar.OnMenuItemClickListener() {
            @Override
            public boolean onMenuItemClick(MenuItem item) {
                return false;
            }
        });

6. menu

		<menu xmlns:android="http://schemas.android.com/apk/res/android"
	    xmlns:app="http://schemas.android.com/apk/res-auto"
	    xmlns:tools="http://schemas.android.com/tools"
	    tools:context="com.alen.menu.Activity.MainActivity">

		<%-- 搜索框 --%>
	    <item
	        android:icon="@android:drawable/ic_menu_search"
	        android:imeOptions="actionSearch"
	        android:inputType="textCapWords"
	        android:title="search"
	        app:actionViewClass="android.support.v7.widget.SearchView"
	        app:showAsAction="ifRoom|collapseActionView" />

		<item
	        android:id="@+id/menu_main_search"
	        android:orderInCategory="100" //同一种类的顺序
	        android:icon="@android:drawable/ic_menu_search"
	        android:title="搜索"
	        app:showAsAction="ifRoom" /> //在哪里显示
		</menu>

	* app:showAsAction
		1. ifRoom表示当toolBar空间足够时，显示图标在标题栏中，否则将它隐藏到ToolBar末端的overFlow中，点开overFlow只显示item的title
		2. CollapseActionView表示当前空间点开之后占据整个ToolBar空间
		3. always表示总是显示在标题栏中，当我们长按该item后，就会以Toast的方式显示出它的title
		4. never表示总是隐藏在overFlow中

	* 代码中使用SearchView
		1. 先找到item，再getView
		
			MenuItem searchItem = menu.findItem(R.id.menu_main_search);
        	SearchView searchView = (SearchView) MenuItemCompat.getActionView(searchItem);

		2. searchView的setOnQueryTextListener提交两次解决办法,
			
				searchView.setIconified(true);//将文本清空

				searchView.setOnQueryTextListener(new SearchView.OnQueryTextListener() {
	            	@Override
	            	public boolean onQueryTextSubmit(String query) {
	               		searchView.setIconified(true);
	                	Log.d(TAG,"onQueryTextSubmit"+query);
	               	 	return false;
	            	}
	
	            	@Override
	            	public boolean onQueryTextChange(String newText) {
	                	Log.d(TAG,"onQueryTextChange");
	                	return false;
	            	}
       			});