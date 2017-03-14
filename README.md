# Record
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
######       网络请求统一使用Rxjava + Retrofit + okhttp库
             
