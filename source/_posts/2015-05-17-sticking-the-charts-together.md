title: "第四章 05.複合圖表"
date: 2015-05-17 06:00:00
tags:
  - Highcharts
categories:
  - Learning
  - Learning Highcharts
  - Bar and column charts
---

# 科技零售(RetailSCM.com) Learning Highcharts中文教程

_<span style="color: #808080;">版權保留，網路轉載請註明來源，謝絕紙質媒體轉載，謝謝。</span>_

## 複合圖表

在本節中我們會建立一個有多種圖表混合顯示的網頁，主要的圖表會顯示在左邊，其他三個小的圖表會在右邊自上而下依次顯示，佈局使用HTML的div標籤及css樣式表來控制。

左邊的圖表使用我們之前討論的多種顏色的柱狀圖例子，在右邊的小圖表我們統一設定其軸線與標籤都不顯示。

第一個小圖表是有兩個series的折線圖，且dataLabels只有在最有一個數據點上才開啟顯示，這代表著最後一個點是一個數據對象。標籤的顏色與series的顏色相同，在y軸插入一個50%標記的plotLine。下面是第一個小圖表的代碼：

<!--more-->

    pointStart: 2001,
    marker: {
        enabled: false
    },
    data: [ 53.6, 52.7, 52.7, 51.9, 52.4,
            52.1, 51.2, 49.7, 49.5, 49.6,
            {   y: 48.9,
                name: 'US',
                dataLabels: {
                color: '#4572A7',
                enabled: true,
                x: -10,
                y: 14,
                formatter: function() {
                    return
                    this.point.name + ": " + this.y + '%';
                }
            }
    }]

第二個小圖表是一個簡單的長條圖，數據標籤在長條圖對應的右邊，數據標籤字體被設定為larger, bold

最後一個小圖表是基本上來說是一個散點圖，每一個series都只有一個點，所以在圖表的右邊可以顯示圖例，然後我們設定每一個series的x為0，這樣我們就可以有不同大小的數據點並且彼此堆疊在一起，下面是這個散點圖的代碼：

    zIndex: 1,
    legendIndex: 0,
    color: {
        linearGradient:
            { x1: 0, y1: 0, x2: 0, y2: 1 },
              stops: [ [ 0, '#FF6600' ],
                       [ 0.6, '#FFB280' ] ]
    },
    name: 'America - 49%',
    marker: {
        symbol: 'circle',
        lineColor: '#B24700',
        lineWidth: 1
    },
    data: [
        { x: 0, y: 49, name: 'America',
          marker: { radius: 74 }
    } ]

下圖是複合圖表最終呈現的結果：

[![chart4-5-1](/images/learning_highcharts/chart4-5-1.jpg)](/images/learning_highcharts/chart4-5-1.jpg)

## 總結

在這一章我們學習到如何使用柱形圖及長條圖。我們利用柱形圖和長條圖的比較例子輕鬆的實現了各種的演示。我們也進一步的設定不同的圖表達到如鏡圖或水平刻度圖，在下一章中我們會再了解餅圖的使用和設定。