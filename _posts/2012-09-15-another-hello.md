---
layout: post
title: Another hello
description: test
tags: Jekyll
---
## 这只是也个测试 `code`

this is a `code` block.

{% highlight ruby %}
	var initAreaKey = 'kinki01';
var areaList = new (require('area'))();
var map = new (require('xmap'))(areaList, initAreaKey);
var appWindow = Titanium.UI.createWindow({
    title : 'XRaderView',
    backgroundColor : '#fff',
    tabBarHidden : true
});

var sideView = Ti.UI.createView({
    backgroundColor : Ti.UI.iOS.COLOR_SCROLLVIEW_BACKGROUND,
    width : 202,
    left : -202,
    zIndex : 100
});

var miniMap = Ti.UI.createImageView({
    image : 'http://www.river.go.jp/xbandradar/map/kinki01/minimap.png',
    width : areaList[initAreaKey].mwidth,
    height : areaList[initAreaKey].mheight,
    top : 0,
    left : 0
});

var location = Ti.UI.createView({
    backgroundColor : 'gray',
    opacity : 0.5,
    zIndex : 200,
    width : 30,
    height : 30,
    borderWidth : 2,
    borderColor : 'black'
});

miniMap.addEventListener('click', function(e) {
    var x = (areaList[initAreaKey].dwidth / areaList[initAreaKey].mwidth) * e.x - (Ti.Platform.displayCaps.platformWidth / 2);
    var y = (areaList[initAreaKey].dheight / areaList[initAreaKey].mheight) * e.y - (Ti.Platform.displayCaps.platformHeight / 2);
    map.view.scrollTo(x, y);
});

var legend = Ti.UI.createImageView({
    image : 'http://www.river.go.jp/xbandradar/common/img/legend_cast.gif',
    width : 122,
    height : 170,
    bottom : 8
});
sideView.add(miniMap);
sideView.add(legend);
sideView.add(location);

var isSideViewOpen = false;

appWindow.leftNavButton = (function() {
    var button = Ti.UI.createButton({
        systemButton : Ti.UI.iPhone.SystemButton.BOOKMARKS
    });
    button.addEventListener('click', function() {
        if (isSideViewOpen) {
            sideView.animate({
                left : -202,
                duration : 300
            });
        }
        else {
            sideView.animate({
                left : 0,
                duration : 300
            });
        }
        isSideViewOpen = !isSideViewOpen;
    });
    return button;
})();

appWindow.rightNavButton = (function() {
    var button = Ti.UI.createButton({
        systemButton : Ti.UI.iPhone.SystemButton.REFRESH
    });
    button.addEventListener('click', function() {
        map.refreshDetail(initAreaKey);
    });
    return button;
})();
appWindow.add(map.view);
appWindow.add(sideView);

map.view.addEventListener('scroll', function(e) {
    var x = (areaList[initAreaKey].mwidth / areaList[initAreaKey].dwidth) * e.x;
    var y = (areaList[initAreaKey].mheight / areaList[initAreaKey].dheight) * e.y;
    location.left = x;
    location.top  = y;
});
map.view.addEventListener('singletap', function(e){
    if(isSideViewOpen){
        isSideViewOpen = false;
        sideView.animate({
            left : -202,
            duration : 300
        });
    }
});
var tabGroup = Titanium.UI.createTabGroup();
tabGroup.addTab(Titanium.UI.createTab({
    window : appWindow
}));
tabGroup.open();

{% endhighlight %}
