# 日常工作中遇到的问题以及学习到的新东西
###用来记录日常学习总结
####1、从ActivityA跳转到ActivityB,并且ActivityA需要携带参数，ActivityB需要获取参数,可使用如下方法（不用再关注key了）
    public class ActivityA extends BaseActivity {
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
            findViewById(R.id.bt).setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View v) {
                    ActivityB.startActivityB(MainActivity.this,"Hello World");
                }
            });
        }
    }
    
    public class ActivityB extends BaseActivity {
        @Override
        protected void onCreate(@Nullable Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.second_actiivty);
            String key = getIntent().getStringExtra("key");
        }

        public static void startActivityB(Context context, String str) {
            Intent intent = new Intent(context, SecondActivity.class);
            intent.putExtra("key", str);
            context.startActivity(intent);
        }
    }
    
    如果有 startActivityForResult回调,道理同样:
    
     public static void startActivityBForResult(Activity activity, String str, int code) {
        Intent intent = new Intent(activity, SecondActivity.class);
        intent.putExtra("key", str);
        activity.startActivityForResult(intent, code);
    }
    
####2、TextView中使用tools:text = "我是显示的文本"代替android:text = "我是显示的文本",前者只出现在预览中，不会出现在编译后的代码里

####3、项目中统一使用如下库: 
######       自定义下拉刷新的统一使用android-Ultra-Pull-To-Refresh库
######       使用到上拉加载更多统一使用BaseRecyclerViewAdapterHelper库
######       图片请求统一使用Glide库
######       注解统一使用Butterknife库
######       网络请求统一使用Rxjava + Retrofit + okhttp库#
######       日志统一使用Logger库
######       滑动删除统一使用AndroidSwipeLayout库

####4、AppBarLayout结合下拉刷新遇到的坑及其解决
######        遇到的坑：当AppBarLayout包裹有下拉刷新控件时，会有滑动事件冲突，既AppBarLayout包裹的空间没有全部显示就会触发下拉刷新。
######        解决方案：监听AppBarLayout的OnOffsetChangedListener，当verticalOffset=0时设置可下拉刷新。
    @Override
    public void onOffsetChanged(AppBarLayout appBarLayout, int verticalOffset) {
       isPullToRefresh = verticalOffset == 0;
    }       
####5、使用ToolBar定义title布局时，toolbar左侧预留问题解决：
     <!-- 解决toolbar左边距问题-->
    <style name="ClubToolbar" parent="Widget.AppCompat.Toolbar">
        <item name="contentInsetStart">0dp</item><!-- 设置该属性解决空白部分-->
    </style>
####6、自定义可扩展的TabLayout，比官方可扩展性强
    ######FlycoTabLayout
    ######TabPagerIndicatorDemo
    ######MagicIndicator
    ######RecyclerTabLayout
    ######Dachshund-Tab-Layout 
    ######SmartTabLayout
    ######PagerSlidingTabStrip
    ######NavigationTabStrip
    ######Android-ViewPagerIndicator
    ######VerticalViewPagerWithTabLayout
    ######LivingTabs
    ######ScalableTabIndicator
