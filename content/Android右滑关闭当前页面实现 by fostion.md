+++
Categories = ["Development", "GoLang"]
Description = ""
Tags = ["Development", "golang"]
date = "2015-09-11T16:00:16+08:00"
menu = "main"
title = "Android右滑关闭当前页面实现 by fostion"

+++

## ViewDragHelper介绍
- ViewDragHelper 是官方提供我们简单处理ViewGroup内部拖动某个子view处理，同时还可以多手指处理、加速度检测
等,用于代替onInterceptTouchEvent和onTouchEvent处理一些复杂的问题.

## ViewDragHelper使用

#### 拦截触碰事件：
```
    @Override
    public boolean onInterceptTouchEvent(MotionEvent ev) {
        return mViewDragHelper != null ? mViewDragHelper.shouldInterceptTouchEvent(ev) 
                                                          : super.onInterceptTouchEvent(ev);
    }

    @Override
    public boolean onTouchEvent(MotionEvent event) {
        if (mViewDragHelper != null) {
            mViewDragHelper.processTouchEvent(event);
        }
        return true;
    }
```
- 这里我们拦截所有事件，其实可以根据实际需要拦截相应的事件。

#### 事件处理
```
    @Override
      public boolean tryCaptureView(View child, int pointerId) {
       return child == mContentView;//只有第一个子view才可以移动
     }
```
- 截取事件后我们可根据需要判断是否处理，返回true就是处理view

```

    @Override
    protected void onLayout(boolean changed, int l, int t, int r, int b) {
        super.onLayout(changed, l, t, r, b);
		//获取需要拖动的宽度
        mContentWidth = mContentView.getWidth();
    }

    @Override
    protected void onFinishInflate() {
        super.onFinishInflate();
        if (getChildCount() != 1) {
            throw new IllegalStateException("SwipeBackLayout only have one child view");
        }
        mContentView = getChildAt(0);
    }
```
- 获得滑动的view,同时确保必须要只包含一个子view，去报拖动整个view

```
    /**
     * 记录向左移动的值
     */
    @Override
    public void onViewPositionChanged(View changedView, int left, int top, int dx, int dy) {
    mMoveLeft = left;
    if (isClose && (left == mContentWidth)) {
    if (mDragCallBack != null) {
        mDragCallBack.onDrag();
            }
          }
        }
```
- 如果当前状态是关闭状态且左边的值等于滑动的View的宽度，也就是说当前的界面已经滑出屏幕后，通知activity可以finish了

```
    /**
     * 手指释放view触发效果：
     * 若是距离不够就弹回去
     */
    @Override
      public void onViewReleased(View releasedChild, float xvel, float yvel) {
      //借助computeScroll方法 移动距离大于四分之一将会关闭否则弹回
      if (mMoveLeft >= (mContentWidth / 4)) {
          //划出屏幕
          isClose = true;
          mViewDragHelper.settleCapturedViewAt(mContentWidth, releasedChild.getTop());
       } else {
           //返回原来位置
            isClose = false;
            mViewDragHelper.settleCapturedViewAt(0, releasedChild.getTop());
            }
            invalidate();
       }

      @Override
      public int clampViewPositionHorizontal(View child, int left, int dx) {
         return Math.min(mContentWidth, Math.max(left, 0));
      }

      @Override
         public int getViewHorizontalDragRange(View child) {
         return mContentWidth;
      }
```
- 设置滑动的区域和手指离开屏幕后的处理


```
    @Override
    public void computeScroll() {
        super.computeScroll();
        if (mViewDragHelper != null && mViewDragHelper.continueSettling(true)) {
            invalidate();
        }
    }
```
- 计算根据抬手后的位置滑动到需要滑动的位置，并刷新


```
    backLayout.setDragCallBack(new SwipeBackLayout.DragCallBack() {
            @Override
            public void onDrag() {
                finish();
            }
     });
```
- 处理回调，结束当前页面

- 最后Activity主题添加
```
<item name="android:windowIsTranslucent">true</item>
<item name="android:windowBackground">@android:color/transparent</item>
```
- 显示就没有白色背景了

- 效果图

![xiaoguotupian](../fostion_result1.jpg)

