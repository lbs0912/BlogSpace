---
title: React Native 实现下拉自动切换分类
date: 2019-06-08 10:35:26
categories: React Native
tags: [React Native]
keywords: React Native
toc: true
password:
comments: true
top:
---

* 记录 `React Native` 中如何实现下拉自动切换分类
* 针对 IOS 和 Android 平台差异，给出不同的切换分类触发条件

<!--more-->

## 需求描述

如图所示，整体为 `FlatList`，顶部分类栏吸顶，底部为 `feed` 流。要实现下拉商品列表到底后，继续下拉，自动切换到一下个分类的效果。


![rn-flatlist-autochange-tab-android](https://image-bed-20181207-1257458714.cos.ap-shanghai.myqcloud.com/front-end-2019/rn-flatlist-autochange-tab-android.png)



## 实现原理

代码层面，可以在  `FlatList` 的 `onScrollEndDrag` 中添加自动 Tab 切换函数，借助 `FlatList` 实例的内容区高度 `contentLength`，滑动偏移量 `offset` 和可视区高度 `visibleLength` 三者关系，实现下拉自动切换Tab功能。


## 代码实现

### IOS 平台实现

```
//页面布局相关
 <FlatList
    data = {[{banner:[]},{tab:[]},{goodList:[]}]}
    renderItem={this.renderItem}
    stickyHeaderIndices={(Platform.OS !== 'web')?[1]:null}
    ListFooterComponent={this._renderFooter}
    onScroll={this._onScroll} //滑动监听
    ref={this._setScrollRef}
    keyExtractor = {(item, index) => { `hScrollView-${index}` }}
    refreshing={this.state.isRefreshing}
    onRefresh={this._onRefresh.bind(this)} //下拉刷新
    getItemLayout={(data, index) => (
        {length: 305, offset: 305 * index, index}
    )}
    onScrollEndDrag = {()=>{  //滑动到底监听函数
        if(Platform.OS != 'web'){
            this._onScrollEndDragFun();
        }
    }}
/>

/**
 * 获取flatList 实例
 * @param ref
 * @private
 */
_setScrollRef = (ref) => {
    this._secondGoodFlatListRef = ref;
};
```

```
import isEmpty from "lodash/isEmpty";
import {Platform} from "react-native";
import {JDDevice} from "@jdreact/jdreact-core-lib";

/**
 * 底部列表页滑动事件 实现上拉切换品类功能
 * @param e
 * @private
 */
_onScrollEndDragFun = (e) => {
    let scrollMetrics = (this._secondGoodFlatListRef && this._secondGoodFlatListRef._listRef
        && this._secondGoodFlatListRef._listRef._scrollMetrics) || null;

    let {contentLength = 0, offset = 0, visibleLength = 0} = scrollMetrics;
    // console.log('===scrollMetrics',scrollMetrics);
    // //判断是否最后一Tab 如果是就不却换下个目录
    // console.log('===this.props',this.props);
    // console.log('===this.state',this.state);

    if (contentLength && offset && visibleLength) {
        let {selectedIndex = 0} = this.state; //当前选中的三级分类index
        let {tabListData = []} = this.props;


        if (!isEmpty(tabListData) && (selectedIndex + 1) < tabListData.length) {  //排除最后一个分类
            let item = tabListData[selectedIndex + 1];
            if (Platform.OS === 'ios') {
                //IOS 系统存在弹性上拉
                if (offset + visibleLength > contentLength + JDDevice.getRpx(100)) {
                    this._secondGoodFlatListRef.scrollToIndex({
                        animated: false,
                        index: 0,
                        viewOffset: 1,
                        viewPosition: 0
                    });
                    this.ItemCategory._clickCategoryTab2 && this.ItemCategory._clickCategoryTab2(item, selectedIndex + 1);
                }
            } else {
                //android 无弹性滑动，滑动到底时，offset + visibleLength = contentLength。IOS有弹性滑动，需要改变判断条件  lbs 2019-03-10
                if (offset + visibleLength > contentLength - JDDevice.getRpx(10)) {
                    this._secondGoodFlatListRef.scrollToIndex({
                        animated: false,
                        index: 0,
                        viewOffset: 1,
                        viewPosition: 0
                    });
                    this.ItemCategory._clickCategoryTab2 && this.ItemCategory._clickCategoryTab2(item, selectedIndex + 1);
                }
            }
        }


    }
};

```


**其中，`contentLength` 为内容区高度，`offset` 为滑动偏移量，`visibleLength` 为可视区高度。**







> 关于<span id="inline-purple">三种高度定义</span>，可参考 [React Native中ListView和ScrollView实现上拉加载](https://www.jianshu.com/p/33ec6ceeb638)

```
let scrollMetrics = (this._secondGoodFlatListRef && this._secondGoodFlatListRef._listRef
        && this._secondGoodFlatListRef._listRef._scrollMetrics) || null;

let {
    contentLength = 0,  // 内容区高度
    offset = 0,         // 滑动偏移量
    visibleLength = 0   // 可视区高度
} = scrollMetrics;
```


### Android 平台实现

对于 Android 平台，当 `offset + visibleLength = contentLength` 时，表示滑动到底部。为了以前进行切换，对条件进行修正，当滑动到距离底部 10px 时，触发切换 Tab 函数，如下代码所示

```
//android 无弹性滑动，滑动到底时，offset + visibleLength = contentLength。IOS有弹性滑动，需要改变判断条件  lbs 2019-03-10
if (offset + visibleLength > contentLength - JDDevice.getRpx(10)) {
    this._secondGoodFlatListRef.scrollToIndex({
        animated: false,
        index: 0, 
        viewOffset: 1,
        viewPosition: 0
    });
    //切换Tab
    this.ItemCategory._clickCategoryTab2 && this.ItemCategory._clickCategoryTab2(item, selectedIndex + 1);
}
```

对于 IOS 平台，因为 IOS 系统存在弹性上拉，如下图所示。因此对滑动到底条件修正为  `offset + visibleLength > contentLength + JDDevice.getRpx(100)`。

其中，`JDDevice.getRpx(100)` 表示弹性上拉的高度，即下图中红色框的高度。



![rn-flatlist-autochange-tab-ios](https://image-bed-20181207-1257458714.cos.ap-shanghai.myqcloud.com/front-end-2019/rn-flatlist-autochange-tab-ios.png)




```
if (Platform.OS === 'ios') {
    if (offset + visibleLength > contentLength + JDDevice.getRpx(100)) {
        this._secondGoodFlatListRef.scrollToIndex({
            animated: false,
            index: 0,
            viewOffset: 1,
            viewPosition: 0
        });
        this.ItemCategory._clickCategoryTab2 && this.ItemCategory._clickCategoryTab2(item, selectedIndex + 1);
    }
}
```