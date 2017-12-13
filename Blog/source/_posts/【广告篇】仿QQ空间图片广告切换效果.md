---
title: 【广告篇】仿QQ空间图片广告切换效果
date: 2017-11-13 21:37:15
categories: "Android"
---

#### 一、概述 ####
最近看到很多创意的广告效果，今天挑了一个QQ空间里面的广告切换效果，在将列表中的item显示广告，随着列表的滚动切换图片。

效果图如下：

![](https://camo.githubusercontent.com/ff0462b1f4b266dea6be30957903e67a5d806cfc/68747470733a2f2f692e696d6775722e636f6d2f314742784364732e676966)


#### 二、实现 ####

**思路：**


1. 捕获列表滚动的滑动距离，不管是ListView还是RecyclerView都可以
2. 自定义View, 绘制两个bitmap到画布上，通过传入的列表滑动的值确定其中一张图片被遮挡的值，这里需要了解一下[Xfermode](http://blog.csdn.net/j_bing/article/details/46289977)的PorterDuff.Mode.DST_IN属性，即显示两个图层的交集，底层图层位于顶层图层上面。

**实现：**

1.初始化画笔

    private void init(){
        mPaint = new Paint();
        mPaint.setAlpha(0);
        //取两层交集的内容，显示最下层绘制
        mPaint.setXfermode(new PorterDuffXfermode(PorterDuff.Mode.DST_IN));
        mPaint.setStyle(Paint.Style.FILL);
        mPaint.setAntiAlias(true);
        mPaint.setStrokeWidth(0);
        mPaint.setStrokeJoin(Paint.Join.ROUND);
        mPaint.setStrokeCap(Paint.Cap.ROUND);
    }

2.初始化布局

    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        super.onMeasure(widthMeasureSpec, heightMeasureSpec);
        width = MeasureSpec.getSize(widthMeasureSpec);
        height = MeasureSpec.getSize(heightMeasureSpec);

        rectF = new RectF(0, 0, width, height);
        //创建空bitmap
        frontBg = Bitmap.createBitmap(width, height, Bitmap.Config.ARGB_8888);
        //创建前景画布
        frontCanvas = new Canvas(frontBg);
    }

3.绘制

    @Override
    protected void onDraw(Canvas behindCanvas) {
        super.onDraw(behindCanvas);

        //绘制背景图片至背景画布
        behindCanvas.drawBitmap(getBitmap(mBehindImage), null, rectF, null);
        //初始时背景图片不可见，防止遮挡前景画布
        behindCanvas.drawBitmap(frontBg, null, rectF, null);

        //绘制前景图片至前景画布
        frontCanvas.drawBitmap(getBitmap(mFrontImage), null, rectF, null);
        //绘制圆遮挡前景图片
        frontCanvas.drawCircle(width - offsetX, height - offsetY, radius, mPaint);
    }

4.绑定ViewGroup获取列表滚动的距离并不断重新绘制

    public void bindView(ViewGroup parent){
        if(parent instanceof RecyclerView){
            ((RecyclerView) parent).addOnScrollListener(new RecyclerView.OnScrollListener() {
                @Override
                public void onScrolled(RecyclerView recyclerView, int dx, int dy) {
                    super.onScrolled(recyclerView, dx, dy);
                    getLocation();
                }
            });
        }else if(parent instanceof ListView){
            ((ListView) parent).setOnScrollChangeListener(new OnScrollChangeListener() {
                @Override
                public void onScrollChange(View view, int i, int i1, int i2, int i3) {
                    getLocation();
                }
            });
        }else {
            Log.i(TAG, "不支持的ViewGroup类型");
        }
    }

    private void getLocation() {
        int[] location = new int[2];
        //获取view在屏幕中的坐标
        this.getLocationOnScreen(location);
        int y = location[1];
        //view距离屏幕顶部的高度 + view自身高度
        int heightTotal = y + getHeight();

        //向上滑动, 放大圆的半径
        //向下滑动, 缩小圆的半径
        if (y > 0 && getScreenHeight() >= heightTotal) {
            radius = (float) ((getScreenHeight() - heightTotal) * 1.5);
            frontCanvas.drawCircle(width - offsetX, heightTotal - offsetY, radius, mPaint);
        } else {
            if (radius < width) {
                radius = 0;
            }
        }

        invalidate();
    }

**总结：**

到这里就已经基本的实现我们想要的View了，大致思路就是搞两块画布，随着滚动不停的在其中一块画布上扩大圆的半径遮挡住画布实现。


#### 开源大法好 ####

[源代码地址](https://github.com/caozhenggit/Advertise)