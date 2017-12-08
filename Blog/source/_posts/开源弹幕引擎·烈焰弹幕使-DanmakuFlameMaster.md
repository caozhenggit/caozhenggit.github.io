---
title: ' 开源弹幕引擎·烈焰弹幕使(DanmakuFlameMaster)'
date: 2017-07-11 13:35:13
categories: "Android"
---

## 简介 ##
DanmakuFlameMaster 是 Android 上开源弹幕解析绘制引擎项目，也是 Android 上最好的开源弹幕引擎·烈焰弹幕。其架构清晰，简单易用，支持多种高效绘制


- B站xml弹幕格式解析

- 基础弹幕精确还原绘制

- 支持mode7特殊弹幕

- 多核机型优化，高效的预缓存机制

- 支持多种显示效果选项实时切换

- 实时弹幕显示支持

- 换行弹幕支持/运动弹幕支持

- 支持自定义字体

- 支持多种弹幕参数设置

- 支持多种方式的弹幕屏蔽


## 项目地址 ##
github地址：[https://github.com/Bilibili/DanmakuFlameMaster](https://github.com/Bilibili/DanmakuFlameMaster)

## Android Studio集成方法 ##

build.gradle中添加


``` bash
dependencies {
    compile 'com.github.ctiao:dfm:0.4.2'
}
```

## 使用方法 ##

1.布局文件

``` java
<master.flame.danmaku.ui.widget.DanmakuView
	android:id="@+id/sv_danmaku"
	android:layout_width="match_parent"
	android:layout_height="match_parent" />
```

2.初始化

	private BaseDanmakuParser mParser;//解析器对象
	private IDanmakuView mDanmakuView;
	private DanmakuContext mContext;
	mDanmakuView = (IDanmakuView) findViewById(R.id.sv_danmaku);
	mContext = DanmakuContext.create();
	// 设置弹幕的最大显示行数
	HashMap<Integer, Integer> maxLinesPair = new HashMap<Integer, Integer>();
	// 滚动弹幕最大显示3行
	maxLinesPair.put(BaseDanmaku.TYPE_SCROLL_RL, 3); 
	// 设置是否禁止重叠
	HashMap<Integer, Boolean> overlappingEnablePair = new HashMap<Integer, Boolean>();
	overlappingEnablePair.put(BaseDanmaku.TYPE_SCROLL_LR, true);
	overlappingEnablePair.put(BaseDanmaku.TYPE_FIX_BOTTOM, true);
	mContext.setDanmakuStyle(IDisplayer.DANMAKU_STYLE_STROKEN, 3)
	        .setDuplicateMergingEnabled(false)
	        .setScrollSpeedFactor(1.2f) //是否启用合并重复弹幕
	        .setScaleTextSize(1.2f) //设置弹幕滚动速度系数,只对滚动弹幕有效
	        .setCacheStuffer(new SpannedCacheStuffer(), mCacheStufferAdapter) // 图文混排使用SpannedCacheStuffer  设置缓存绘制填充器，默认使用{@link SimpleTextCacheStuffer}只支持纯文字显示, 如果需要图文混排请设置{@link SpannedCacheStuffer}如果需要定制其他样式请扩展{@link SimpleTextCacheStuffer}|{@link SpannedCacheStuffer}
	        .setMaximumLines(maxLinesPair) //设置最大显示行数
	        .preventOverlapping(overlappingEnablePair); //设置防弹幕重叠，null为允许重叠
	if (mDanmakuView != null) {
	    mParser = createParser(this.getResources().openRawResource(R.raw.comments)); //创建解析器对象，从raw资源目录下解析comments.xml文本
	    mDanmakuView.setCallback(new master.flame.danmaku.controller.DrawHandler.Callback() {
	        @Override
	        public void updateTimer(DanmakuTimer timer) {
	        }
	
	        @Override
	        public void drawingFinished() {
	        }
	
	        @Override
	        public void danmakuShown(BaseDanmaku danmaku) {
	        }
	
	        @Override
	        public void prepared() {
	            mDanmakuView.start();
	        }
	    });
    mDanmakuView.prepare(mParser, mContext);
    mDanmakuView.showFPS(false); //是否显示FPS
    mDanmakuView.enableDanmakuDrawingCache(true);



3.创建解析器对象

	/**
	 * 创建解析器对象，解析输入流
	 * @param stream
	 * @return
	 */
	private BaseDanmakuParser createParser(InputStream stream) {
	    if (stream == null) {
	        return new BaseDanmakuParser() {
	            @Override
	            protected Danmakus parse() {
	                return new Danmakus();
	            }
	        };
	    }
	    ILoader loader = DanmakuLoaderFactory.create(DanmakuLoaderFactory.TAG_BILI);
	    try {
	        loader.load(stream);
	    } catch (IllegalDataException e) {
	        e.printStackTrace();
	    }
	    BaseDanmakuParser parser = new BiliDanmukuParser();
	    IDataSource<?> dataSource = loader.getDataSource();
	    parser.load(dataSource);
	    return parser;
	}
	注：
	DanmakuLoaderFactory.create(DanmakuLoaderFactory.TAG_BILI) //xml解析
	DanmakuLoaderFactory.create(DanmakuLoaderFactory.TAG_ACFUN) //json文件格式解析

4.自定义弹幕背景和边距

	private static class BackgroundCacheStuffer extends SpannedCacheStuffer {
	    // 通过扩展SimpleTextCacheStuffer或SpannedCacheStuffer个性化你的弹幕样式
	    final Paint paint = new Paint();
	    @Override
	    public void measure(BaseDanmaku danmaku, TextPaint paint) {
	        danmaku.padding = 10;  // 在背景绘制模式下增加padding
	        super.measure(danmaku, paint);
	    }
		
	    @Override
	    public void drawBackground(BaseDanmaku danmaku, Canvas canvas, float left, float top) {
	        paint.setColor(0x8125309b);  //弹幕背景颜色
	        canvas.drawRect(left + 2, top + 2, left + danmaku.paintWidth - 2, top + danmaku.paintHeight - 2, paint);
	    }
	
	    @Override
	    public void drawStroke(BaseDanmaku danmaku, String lineText, Canvas canvas, float left, float top, Paint paint) {
	        // 禁用描边绘制
	    }
	}