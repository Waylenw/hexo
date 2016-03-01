title: Android Fragment切换解决方案之FragmentTransaction（一）
date: 2015-10-09
tags:
- Android
---
 > fragment切换？不就是Viewpager+fragment吗？是的这是很大多数App采用的切换的组合方式。但是Viewpager+fragment切换的的生命周期十分混乱，这就是会引发很多的坑！如果在一些不需要切换的动画的情况下，采用FragmentTransaction动态切换是一个不错的选择

 # 拓展
 > 先来看看有哪些切换的方案吧。又有哪些问题
 
**（一）预先写在布局里面**
布局文件
```
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    xmlns:tools="http://schemas.android.com/tools"  
    android:layout_width="match_parent"  
    android:layout_height="match_parent"  
    android:background="@drawable/activity_bg"  
    android:orientation="vertical" >  
  
    <fragment  
        android:id="@+id/fragement_main"  
        android:name="net.loonggg.fragment.FragmentMain"  
        android:layout_width="fill_parent"  
        android:layout_height="fill_parent"  
        android:layout_weight="10" />  
  
    <fragment  
        android:id="@+id/fragement_search"  
        android:name="net.loonggg.fragment.FragmentSearch"  
        android:layout_width="fill_parent"  
        android:layout_height="fill_parent"  
        android:layout_weight="10" />  
  
    <fragment  
        android:id="@+id/fragement_setting"  
        android:name="net.loonggg.fragment.FragmentSetting"  
        android:layout_width="fill_parent"  
        android:layout_height="fill_parent"  
        android:layout_weight="10" />
```
java文件
```java
mFragments[0] = fragmentManager.findFragmentById(R.id.fragement_main);  
        mFragments[1] = fragmentManager.findFragmentById(R.id.fragement_search);  
        mFragments[2] = fragmentManager  
                .findFragmentById(R.id.fragement_setting);  
        fragmentTransaction = fragmentManager.beginTransaction()  
                .hide(mFragments[0]).hide(mFragments[1]).hide(mFragments[2]);  
        fragmentTransaction.show(mFragments[0]).commit();  
```
 这种方案的问题就是在如果我的某个Fragment更换了,改动的涉及到两个文件，而且也不能进行动态加载，局限性比较大。
 **（二）Replace替换**
```
 private void replaceFragment(Fragment newFragment) {

        FragmentTransaction trasection = getFragmentManager().beginTransaction();
        try {
            //FragmentTransaction trasection =
            getFragmentManager().beginTransaction();
            trasection.replace(R.id.linearLayout2, newFragment);
            trasection.addToBackStack(null);
            trasection.commit();

        } catch (Exception e) {
            // TODO: handle exception

        }
    }
```
 这样总没问题了吧？这种方案的问题在于每次调用replace方法时Fragment的生命周期会重走。这在现实开发中是不允许的。
 # 解决方案
 **build gradle**
```
 compile 'com.android.support:support-v4:20.0.0'
```
 
  **布局文件**
```
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <LinearLayout
        android:id="@+id/top"
        android:layout_width="match_parent"
        android:layout_height="50dp"
        android:orientation="horizontal">

        <LinearLayout
            android:id="@+id/tab1"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:layout_weight="1"
            android:background="@drawable/selector_tab"
            android:gravity="center_horizontal|center_vertical"
            android:orientation="horizontal">

            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="选项一" />

        </LinearLayout>

        <LinearLayout
            android:id="@+id/tab2"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:layout_weight="1"
            android:background="@drawable/selector_tab"
            android:gravity="center_horizontal|center_vertical"
            android:orientation="horizontal">

            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_gravity="center_vertical|center_horizontal"
                android:text="选项二" />

        </LinearLayout>

        <LinearLayout
            android:id="@+id/tab3"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:layout_weight="1"
            android:background="@drawable/selector_tab"
            android:gravity="center_horizontal|center_vertical"
            android:orientation="horizontal">

            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_gravity="center_vertical|center_horizontal"
                android:text="选项三" />

        </LinearLayout>

    </LinearLayout>

    <FrameLayout
        android:id="@+id/fragment"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_below="@+id/top" />


</RelativeLayout>
```
**Activity**
 以下采用的方式是预先将Fragment加入集合中，切换时从集合取出Fragment添加到FragmentTransaction事物中，每次加入的时候判断是否已经添加到FragmentTransaction中，存在则调用show，并将上一个fragment隐藏。
```
public class MainActivity extends FragmentActivity implements View.OnClickListener {
    private LinearLayout tab1, tab2, tab3;
    private int currentIndex = 0;
    private ArrayList<Fragment> fragmentArrayList;
    private Fragment mCurrentFrgment;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        initView();
        initFragment();
    }


    private void initView() {
        tab1 = (LinearLayout) findViewById(R.id.tab1);
        tab1.setOnClickListener(this);
        tab1.setTag(0);

        tab2 = (LinearLayout) findViewById(R.id.tab2);
        tab2.setOnClickListener(this);
        tab2.setTag(1);

        tab3 = (LinearLayout) findViewById(R.id.tab3);
        tab3.setOnClickListener(this);
        tab3.setTag(2);
    }

    private void initFragment() {
        fragmentArrayList = new ArrayList<Fragment>(3);
        fragmentArrayList.add(new Tab1Fragment());
        fragmentArrayList.add(new Tab2Fragment());
        fragmentArrayList.add(new Tab3Fragment());

        tab1.setSelected(true);
        changeTab(0);
    }


    @Override
    public void onClick(View v) {
        changeTab((Integer) v.getTag());

    }

    private void changeTab(int index) {
        currentIndex = index;
        tab1.setSelected(index == 0);
        tab2.setSelected(index == 1);
        tab3.setSelected(index == 2);

        FragmentTransaction ft = getSupportFragmentManager().beginTransaction();
        //判断当前的Fragment是否为空，不为空则隐藏
        if (null != mCurrentFrgment) {
            ft.hide(mCurrentFrgment);
        }
        //先根据Tag从FragmentTransaction事物获取之前添加的Fragment
        Fragment fragment = getSupportFragmentManager().findFragmentByTag(fragmentArrayList.get(currentIndex).getClass().getName());

        if (null == fragment) {
            //如fragment为空，则之前未添加此Fragment。便从集合中取出
            fragment = fragmentArrayList.get(index);
        }
        mCurrentFrgment = fragment;

        //判断此Fragment是否已经添加到FragmentTransaction事物中
        if (!fragment.isAdded()) {
            ft.add(R.id.fragment, fragment, fragment.getClass().getName());
        } else {
            ft.show(fragment);
        }
        ft.commit();
    }
}
```
**fragment**
```
public class Tab1Fragment extends Fragment {


    public Tab1Fragment() {
    }

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
    }

    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
        return inflater.inflate(R.layout.fragment_tab1, container, false);
    }

    @Override
    public void onViewCreated(View view, Bundle savedInstanceState) {
        super.onViewCreated(view, savedInstanceState);
        final ProgressBar progressBar = (ProgressBar) view.findViewById(R.id.progressbar);
        final TextView tv = (TextView) view.findViewById(R.id.tv);
        new Handler().postDelayed(new Runnable() {
            @Override
            public void run() {
                progressBar.setVisibility(View.GONE);
                tv.setVisibility(View.VISIBLE);
            }
        }, 2000);
    }

    @Override
    public void onAttach(Activity activity) {
        super.onAttach(activity);
    }

    @Override
    public void onDetach() {
        super.onDetach();
    }
}
```
这样做的好处是，每个Fragment切换只进行一次初始化。
 # 效果图
 ![此处输入图片的描述][1]
 
 # 源代码
 [github][2]
 
 

 
 
 


  [1]: https://raw.githubusercontent.com/Waylenw/AndroidDemo_Waylen/master/screen/fragmentchange.gif
  [2]: https://github.com/Waylenw/AndroidDemo_Waylen