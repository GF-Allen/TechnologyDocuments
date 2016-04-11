## CardView

1. 引入CardView

	> compile 'com.android.support:cardview-v7:23.1.1'

2. 是个ViewGroup，继承至FrameLayout

		<android.support.v7.widget.CardView
	        app:cardCornerRadius="5dp"
	        app:cardElevation="2dp"
	        app:contentPadding="5dp"
	        app:cardPreventCornerOverlap="true"
	        app:cardUseCompatPadding="true"
	        android:layout_width="match_parent"
	        android:layout_height="match_parent">
	
	        <LinearLayout
	            android:layout_gravity="center"
	            android:layout_width="match_parent"
	            android:layout_height="wrap_content"
	            android:orientation="vertical">
	
	            <ImageView
	                android:id="@+id/iv_photo"
	                android:layout_width="match_parent"
	                android:layout_height="200dp"
	                android:scaleType="centerCrop"/>
	
	            <TextView
	                android:id="@+id/tv_title"
	                android:padding="5dp"
	                android:layout_width="wrap_content"
	                android:layout_height="wrap_content"
	                android:textSize="18sp"
	                android:layout_gravity="center"
	                android:textColor="@color/primary_text"
	                android:text="美女" />
	
	        </LinearLayout>
	
	    </android.support.v7.widget.CardView>

3. CardView常用属性：

		card_view:cardElevation 阴影的大小
		card_view:cardMaxElevation 阴影最大高度
		card_view:cardBackgroundColor 卡片的背景色
		card_view:cardCornerRadius 卡片的圆角大小
		card_view:contentPadding 卡片内容于边距的间隔
		card_view:contentPaddingBottom
		card_view:contentPaddingTop
		card_view:contentPaddingLeft
		card_view:contentPaddingRight
		card_view:contentPaddingStart
		card_view:contentPaddingEnd
		card_view:cardUseCompatPadding 设置内边距，V21+的版本和之前的版本仍旧具有一样的计算方式
		card_view:cardPreventConrerOverlap 在V20和之前的版本中添加内边距，这个属性为了防止内容和边角的重叠