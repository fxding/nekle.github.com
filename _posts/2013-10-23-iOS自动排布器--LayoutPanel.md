---
layout: post
title: iOS自动排布器--LayoutPanel
description: 这篇文章将描述一种自动排布器--LayoutPanel, 这种排布器将使开发者可以通过代码的方式完成界面元素的排布, 相对于 iOS 原生的自动排布方式, LayoutPanel 会有很多优势. 
tag: iOS 自动排布 LayoutPanel
---

# LayoutPanel
LayoutPanel 排布器包含三种排布方式, 分别为栈式(StackPanel),停靠式(DockPanel), 九宫格式(GridPanel), 这三种布局方式各有特色, 在应用中使用 LayoutPanel 进行元素排布时你有可能只需要使用一种排布方式, 也有可能需要同时使用两种或三种排布方式一起来完成一个复杂的界面元素排布. 完成同一种显示的排布你可以使用不同的排布方式完成. 比如完成一个列表的排布你可以使用 StackPanel, 或者单列的 GridPanel, 或者使用同一中 Dock 方式的 DockPanel. 比如下面的图展示了使用不同的LayoutPanel完成相同的列表排布.

![img](http://farm8.staticflickr.com/7332/10488276874_ed0774571d.jpg)

下图栈式了使用三种不同的 LayoutPanel 共同完成的排布:

**竖屏**

![img](http://farm4.staticflickr.com/3814/10424358723_1a98e1f271.jpg)

**横屏**

![img](http://farm8.staticflickr.com/7452/10424214796_0f325478d6.jpg)
 


## StackPanel
栈式排布方式就如算法中的栈一样, 元素将从左向右(从右向左), 从上到下(从下到上) 按照加入排布器的顺序依次进行排布.
下面为可用的StackPanel 的排布方向.

```
typedef enum _StackPanelFlowDirector
{
    eStackPanelFlowDirector_LeftToRight,
    eStackPanelFlowDirector_RightToLeft,
    eStackPanelFlowDirector_TopToBottom,
    eStackPanelFlowDirector_BottomToTop,
    
}StackPanelFlowDirector;
```

### StackPanel 支持的布局方式

```
typedef enum _StackPanelArchorType{
    eStackPanelArchorType_LeftTop       = 0x00,
    eStackPanelArchorType_LeftCenter    = 0x01,
    eStackPanelArchorType_LeftBottom    = 0x02,
    eStackPanelArchorType_CenterTop     = 0x03,
    eStackPanelArchorType_CenterCenter  = 0x04,
    eStackPanelArchorType_CenterBottom  = 0x05,
    eStackPanelArchorType_RightTop      = 0x06,
    eStackPanelArchorType_RightCenter   = 0x07,
    eStackPanelArchorType_RightBottom   = 0x08,    
    // Fill 属性
    eStackPanelArchorType_FillWidth     = 0x100, 
    eStackPanelArchorType_FillHeight    = 0x200,
    eStackPanelArchorType_FillWidthHeight  = 0x100 | 0x200,
    eStackPanelArchorType_Fill          = 0x8000,
}StackPanelArchorType;

```

前面的几个类型为布局类型, 后面四个为填充类型, 布局类型确定了元素在排布Panel上的位置, 填充属性确定元素如何填充Panel. 布局属性可以和填充属性同时使用, 但每次布局属性和填充属性都只能使用一个.
比如 `eStackPanelArchorType_LeftTop | eStackPanelArchorType_FillWidth` , 指定了元素排布在Panel的左上方, 元素的宽度为Panel的宽度.

`eStackPanelArchorType_Fill` 这个属性是一个特殊的填充类型, 当填充属性指定为这个类型时, 该元素将填充Panel剩余的所有空间. 比如上方`横屏`和`竖屏`的图中最下方棕色的元素就是指定了该填充类型.

## GridPanel
GridPanel 即九宫格排布方式, 九宫格排布方式在应用的元素排布中也是非常常用的, 比如手机的桌面就是一种九宫格排布. 九宫格排布的一大特点就是能够确定元素的大体位置然后再做特殊的调整.

### GridPanel 支持的排布方式

```
typedef enum _GridPanelArchorType
{
    eGridPanelArchorType_LeftTop        = 0x00,
    eGridPanelArchorType_LeftCenter     = 0x01,
    eGridPanelArchorType_LeftBottom     = 0x02,
    eGridPanelArchorType_CenterTop      = 0x03,
    eGridPanelArchorType_CenterCenter   = 0x04,
    eGridPanelArchorType_CenterBottom   = 0x05,
    eGridPanelArchorType_RightTop       = 0x06,
    eGridPanelArchorType_RightCenter    = 0x07,
    eGridPanelArchorType_RightBottom    = 0x08,
    // 填充
    eGridPanelArchorType_FillWidth      = 0x100,
    eGridPanelArchorType_FillHeight     = 0x200,
    eGridPanelArchorType_FillWidthHeight  = 0x100 | 0x200,
    
} GridPanelArchorType;
```

GridPanel 支持的排布方式和StackPanel支持的排布方式差不多一样, 但少了与Stackpanel 的`eStackPanelArchorType_Fill` 相似的填充类型.

## DockPanel
DockPanel 即停靠式排布方式, Dock 排布方式将 Panel 的四条边作为停靠点, 具有一种吸附的效果, 但也不是始终使用Panel的四条边作为Dock点, 应该说使用当前未被使用的Panel空间作为停靠. 这种排布方式也有很多应用方式, 比如说一个画图一个用的工具条.


### DockPanel 支持的排布方式

```
typedef enum _DockSideType {
    eDockSideType_Left,
    eDockSideType_Right,
    eDockSideType_Top,
    eDockSideType_Bottom,
    eDockSideType_Fill,
}DockSideType;
```

和 StackPanel 和 GridPanel 相比 DockPanel 只提供了四种停靠方式, 和一种填充方式,  `eDockSideType_Fill` 填充方式同样是将Panel中剩余的空间完全填充. 


### Demo

![img](http://farm4.staticflickr.com/3769/10488943405_e87320f41e.jpg)


**github**

[LayoutPanel](https://github.com/Nekle/StackPanel)






 







