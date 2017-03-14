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
######    FlycoTabLayout
######    TabPagerIndicatorDemo
######    MagicIndicator
######    RecyclerTabLayout
######    Dachshund-Tab-Layout 
######    SmartTabLayout
######    PagerSlidingTabStrip
######    NavigationTabStrip
######    Android-ViewPagerIndicator
######    VerticalViewPagerWithTabLayout
######    LivingTabs
######    ScalableTabIndicator

####7、Java保留指定小数位数

    public class DecimalUtils {

        /** 
         * 1.按四舍五入保留指定小数位数，位数不够用0补充
         * @param o 格式化前的小数
         * @param newScale 保留小数位数
         * @return 格式化后的小数
         */
        public static String formatDecimal1(Object o, int newScale) {
            return String.format("%." + newScale + "f", o);
        }

        /**
         * 2.1按四舍五入保留指定小数位数，位数不够用0补充
         * @param d 格式化前的小数
         * @param newScale 保留小数位数
         * @return 格式化后的小数
         */
        public static String formatDecimal21(double d, int newScale) {
            String pattern = "0.";
            for (int i = 0; i < newScale; i++) {
                pattern += "0";
            }
            DecimalFormat df = new DecimalFormat(pattern);
            return df.format(d);
        }

        /**
         * 2.2按四舍五入保留指定小数位数，小数点后仅保留有效位数
         * @param d 格式化前的小数
         * @param newScale 保留小数位数
         * @return 格式化后的小数
         */
        public static String formatDecimal22(double d, int newScale) {
            String pattern = "#.";
            for (int i = 0; i < newScale; i++) {
                pattern += "#";
            }
            DecimalFormat df = new DecimalFormat(pattern);
            return df.format(d);
        }

        /**
         * 3.按指定舍入模式保留指定小数位数
         * @param d 格式化前的小数
         * @param newScale 保留小数位数
         * @param roundingMode 舍入模式
         *                         (RoundingMode.UP始终进一/DOWN直接舍弃/
         *                                     CEILING正进负舍/FLOOR正舍负进/
         *                                    HALF_UP四舍五入/HALF_DOWN五舍六进/
         *                                    HALF_EVEN银行家舍入法/UNNECESSARY抛出异常)
         * @return 格式化后的小数
         */
        public static double formatDecimal3(double d, int newScale, RoundingMode roundingMode) {
            BigDecimal bd = new BigDecimal(d).setScale(newScale, roundingMode);
            return bd.doubleValue();
        }

    }
    
####8、String中占位符的使用
        <string name="loading">离下班回家还剩%1$s分钟</string>
      
        String temp = getResources().getString(R.string.loading);
        String timeTip = String.format(temp,38);
        结果：离下班回家还剩38分钟

####9、Glide加载设置圆角、圆形图：
######圆角:
    public class GlideRoundTransform extends BitmapTransformation {

        private static float radius = 0f;

        public GlideRoundTransform(Context context) {
            this(context, 4);
        }

        public GlideRoundTransform(Context context, int dp) {
            super(context);
            this.radius = Resources.getSystem().getDisplayMetrics().density * dp;
        }

        @Override protected Bitmap transform(BitmapPool pool, Bitmap toTransform, int outWidth, int outHeight) {
            return roundCrop(pool, toTransform);
        }

        private static Bitmap roundCrop(BitmapPool pool, Bitmap source) {
            if (source == null) return null;

            Bitmap result = pool.get(source.getWidth(), source.getHeight(), Bitmap.Config.ARGB_8888);
            if (result == null) {
                result = Bitmap.createBitmap(source.getWidth(), source.getHeight(), Bitmap.Config.ARGB_8888);
            }

            Canvas canvas = new Canvas(result);
            Paint paint = new Paint();
            paint.setShader(new BitmapShader(source, BitmapShader.TileMode.CLAMP, BitmapShader.TileMode.CLAMP));
            paint.setAntiAlias(true);
            RectF rectF = new RectF(0f, 0f, source.getWidth(), source.getHeight());
            canvas.drawRoundRect(rectF, radius, radius, paint);
            return result;
        }

        @Override public String getId() {
            return getClass().getName() + Math.round(radius);
        }
    }
######圆形：
    public class GlideCircleTransform extends BitmapTransformation {
        public GlideCircleTransform(Context context) {
            super(context);
        }

        @Override protected Bitmap transform(BitmapPool pool, Bitmap toTransform, int outWidth, int outHeight) {
            return circleCrop(pool, toTransform);
        }

        private static Bitmap circleCrop(BitmapPool pool, Bitmap source) {
            if (source == null) return null;

            int size = Math.min(source.getWidth(), source.getHeight());
            int x = (source.getWidth() - size) / 2;
            int y = (source.getHeight() - size) / 2;

            // TODO this could be acquired from the pool too
            Bitmap squared = Bitmap.createBitmap(source, x, y, size, size);

            Bitmap result = pool.get(size, size, Bitmap.Config.ARGB_8888);
            if (result == null) {
                result = Bitmap.createBitmap(size, size, Bitmap.Config.ARGB_8888);
            }

            Canvas canvas = new Canvas(result);
            Paint paint = new Paint();
            paint.setShader(new BitmapShader(squared, BitmapShader.TileMode.CLAMP, BitmapShader.TileMode.CLAMP));
            paint.setAntiAlias(true);
            float r = size / 2f;
            canvas.drawCircle(r, r, r, paint);
            return result;
        }

        @Override public String getId() {
            return getClass().getName();
        }
    }
######使用
    Glide.with(context)
            .load(url)
            .placeholder(emptyImg)
            .error(erroImg)
            .transform(new GlideCircleTransform(context))
            .into(iv);
