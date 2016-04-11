### RecyclerView

1. 常见方法

		mRecyclerView = findView(R.id.id_recyclerview);
		//设置布局管理器（三种，线性，网格，交错）
		mRecyclerView.setLayoutManager(layout);
		//设置adapter
		mRecyclerView.setAdapter(adapter)
		//设置Item增加、移除动画
		mRecyclerView.setItemAnimator(new DefaultItemAnimator());
		//添加分割线
		mRecyclerView.addItemDecoration(new DividerItemDecoration(getActivity(), DividerItemDecoration.HORIZONTAL_LIST));

2. RecyclerView 瀑布流layoutManager判断获取最后一个Item
 
		rvPhoto.addOnScrollListener(new RecyclerView.OnScrollListener() {

            @Override
            public void onScrollStateChanged(RecyclerView recyclerView, int newState) {
                if (newState == RecyclerView.SCROLL_STATE_IDLE && !isRefresh) {
                    int[] positions = new int[layoutManager.getSpanCount()];
                    layoutManager.findLastVisibleItemPositions(positions);
                    for (int position : positions) {
                        if (position == adapter.getItemCount() - 1) {
                            setRefreshing();
                            refreshData(++limit);
                        }
                    }
                }

            }
        });

3. Adapter

	1. 定义一个VieHolder继承至RecyclerView.ViewHolder
	2. 实现其内部的方法

			public class RecommRecyclerAdapter extends RecyclerView.Adapter<RecommRecyclerAdapter.RecommRecyclerViewHolder> {
		
		
			//创建ViewHolder
		    @Override
		    public RecommRecyclerViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
		        return null;
		    }
		
			//绑定数据
		    @Override
		    public void onBindViewHolder(RecommRecyclerViewHolder holder, int position) {
		
		    }
		
		    @Override
		    public int getItemCount() {
		        return 0;
		    }
		
		    class RecommRecyclerViewHolder extends RecyclerView.ViewHolder {
		
		        public RecommRecyclerViewHolder(View itemView) {
		            super(itemView);
		        }
		    }
		}