title: Android RecyclerView三部曲之基础篇(一)
date: 2016-01-18
tags:
- Android
---
 # 简介和准备
> 相信大家对RecyclerView都不陌生了。自RecyclerView的出现开始，它就慢慢占据了我们日常开发使用的频率。所以掌握它就显得格外重要了。ReclclerView将各个模块的操作进行了拆解。很好的解决耦合问题。正是因为这种解耦让你做的事情更多，也更强大。逻辑也格外的清晰。

 RecyclerView是Support-V7架包中得一个组件。所以在使用前必须先升级support lib，然后导入support-v7。
 
 # 实例
 首先在build.gradle文件添加依赖库
 
 ``` 
   dependencies {
    compile fileTree(include: ['*.jar'], dir: 'libs')
    compile 'com.android.support:recyclerview-v7:23.1.0'
   }
 ```
MainAcvtivity

``` 
public class MainActivity extends Activity {
    private GridVeiwAdapter gridAdapter;
    private ListViewAdapter listAdapter;
    private FlowViewAdapter flowViewAdapter;
    private RecyclerView recyclerView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        initView();

    }

    private void initView() {
        recyclerView = (RecyclerView) findViewById(R.id.main_recyclerView);
        gridAdapter = new GridVeiwAdapter(getApplication());
        listAdapter = new ListViewAdapter(getApplication());
        flowViewAdapter = new FlowViewAdapter(getApplication());
        TabLayout tabLayout = (TabLayout) findViewById(R.id.tabs);
        tabLayout.setTabMode(TabLayout.MODE_FIXED);

        tabLayout.addTab(tabLayout.newTab().setText("ListView效果"));
        tabLayout.addTab(tabLayout.newTab().setText("GridView效果"));
        tabLayout.addTab(tabLayout.newTab().setText("Flow效果"));
        tabLayout.setOnTabSelectedListener(new TabLayout.OnTabSelectedListener() {
            @Override
            public void onTabSelected(TabLayout.Tab tab) {
                if (tab.getPosition() == 0) {//ListView效果
                    recyclerView.setLayoutManager(new LinearLayoutManager(MainActivity.this));
                    recyclerView.setAdapter(listAdapter);
                }
                if (tab.getPosition() == 1) {//GridView效果
                    recyclerView.setLayoutManager(new GridLayoutManager(MainActivity.this, 3));
                    recyclerView.setAdapter(gridAdapter);
                }
                if (tab.getPosition() == 2) {//Flow效果
                    //StaggeredGridLayoutManager.VERTICAL此处表示有多少列
                    recyclerView.setLayoutManager(new StaggeredGridLayoutManager(3, StaggeredGridLayoutManager.VERTICAL));
                    recyclerView.setAdapter(flowViewAdapter);
                }
            }

            @Override
            public void onTabUnselected(TabLayout.Tab tab) {

            }

            @Override
            public void onTabReselected(TabLayout.Tab tab) {

            }
        });

        recyclerView.setLayoutManager(new LinearLayoutManager(this));
        recyclerView.setAdapter(listAdapter);


    }
}
```

**setLayoutManager()**
该方法是给每个Item设置一个布局管理器。有三种布局布局管理器
- LinearLayoutManager线性布局管理器
- GridLayoutManager网格布局管理器
- StaggeredGridLayoutManager瀑布网格布局管理器
使用上面的布局管理器可以很轻松的实现ListView和GriView以及瀑布流的效果。既然这么吊还用ListView和GridView吗!

---

FlowViewAdapter
```
public class ListViewAdapter extends RecyclerView.Adapter<ListViewAdapter.MyViewHolder> {
    List<String> list=new ArrayList<String>();

    private Context context;
    public ListViewAdapter(Context context){
        for(int i=0;i<100;i++){
            list.add("列表:"+i);
        }
        this.context=context;
    }



    @Override
    public int getItemCount() {
        return list.size();
    }

    @Override
    public MyViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
    //创建Item的ViewHodler
        MyViewHolder holder = new MyViewHolder(LayoutInflater.from(context).inflate(R.layout.recycler_item_list_view, parent,
                false));
        return holder;
    }

    @Override
    public void onBindViewHolder(MyViewHolder holder, int position) {
		//在此处完成布局的展示设值	
        holder.tv.setText(list.get(position));
        holder.tv.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Toast.makeText(context,""+position,Toast.LENGTH_SHORT);
            }
        });


    }

    class MyViewHolder extends RecyclerView.ViewHolder {

        TextView tv;

        public MyViewHolder(View view) {
            super(view);
            tv = (TextView) view.findViewById(R.id.textView);
        }
    }}
```




> RecyclerView的Adapter的不同之处是你在继承RecyclerView.Adaper必需现实一个ViewHolder。在创建的时候会将这个每个Item_layour转化成Viewholder。在传递给onBindViewHolder方法去做布局展示设值。还有一点和ListView不同的是每个控件的点击事件都需要自己去实现,包括Item的点击事件。所以recyclerView.setOnclickListener()就不能用了。

**布局文件**
```
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical" android:layout_width="match_parent"
    android:layout_height="match_parent">
    <ImageView
        android:layout_marginTop="5dp"
        android:layout_marginLeft="5dp"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:id="@+id/imageView"
        android:gravity="center"
        android:background="@mipmap/a"
        android:layout_gravity="center_horizontal" />
</LinearLayout>
```

基本上就这些了。有些Apdater的代码没有贴出来，大家可以去源代码中去查看
**[源码地址](https://github.com/Waylenw/AndroidDemo_Waylen)**

 # 效果图
基本效果就是这样了,三个按钮分别对应三个Adpater.只需一个控件即可实现ListView和GridView和瀑布流的效果
![](https://raw.githubusercontent.com/Waylenw/AndroidDemo_Waylen/master/screen/recyclerView.gif)

  
