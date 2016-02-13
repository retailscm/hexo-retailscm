title: "第二章 05.探索PlotOptions"
date: 2015-05-14 03:00:00
tags:
  - Highcharts
categories:
  - Learning
  - Learning Highcharts
  - Highcharts configurations
---

# 科技零售(RetailSCM.com) Learning Highcharts中文教程

_<span style="color: #808080;">版權保留，網路轉載請註明來源，謝絕紙質媒體轉載，謝謝。</span>_

## 探索PlotOptions

poltOption是一個包含了設定對象與所有series類型(area, areaspline, bar, column, pie, scatter, spline gauge, range)的封裝對象。這些配置擁有一些通用類型可使用的屬性如plotOptions.line.lineWidth，也有特定類型可使用的配置，如餅圖的plotOptions.pie.center，這其中plotOptions.series是一個通用繪圖選項，被整個series所公用。

前面提到的plotOptions可以在plotOptions.series，plotOptions.{series-type}以及其他的series配置中形成一個優先序。舉例如series[x].shadow (這裡的 series[x].type是 'pie')比plotOptions.pie.shadow具有更高的優先級，plotOptions.pie.shadow又比plotOptions.series.shadow具有更高的優先級。這樣做的目的是為了可以在圖表中同時呈現不同的圖表類型。

如一個多維度的柱狀圖和一個折線圖同時在一張圖表中呈現，柱狀圖與折線圖通用的屬性可以定義在plotOptions.series.\*中，而如果希望可以保持他們自己特定的屬性值，則可以使用plotOptions.column和plotOptions.line來設定屬性。此外在plotOptions.{series-type}.\*中的屬性可以進一步再覆蓋掉指定在相同圖表類型中series數組裡面的屬性設定。

<!--more-->

    chart.defaultSeriesType or chart.type
        series[x].type

    plotOptions.series.{seriesProperty}
        plotOptions.{series-type}.{seriesProperty}
            series[x].{seriesProperty}

    plotOptions.points.events.*
        series[x].data[y].events.*

    plotOptions.series.marker.*
        series[x].data[y].marker.*

plotOptions包含的屬性控制圖表類型如何對應的顯示在圖表中，如反轉圖表(inverted charts)，series顏色，堆積柱形圖，series上的使用者交互等。這些選項在我們學習每一類圖表如何使用的時候都會分別介紹。同時，我們會透過納斯達克的月線圖來探索plotOptions的概念，在月線圖中我們會有5個不同的series數據：開市，休市，最高，最低及交易量。正常情況下這些數據是用於繪製每日股票走勢圖來(OHLCV)。我們整合這些內容到一個圖表中來展示plotOptions的用法。

[![chart2-4-2](/images/learning_highcharts/chart2-4-2.jpg)](/images/learning_highcharts/chart2-4-2.jpg)

下面是產生上面圖例的圖表設定代碼：

    chart: {
        renderTo: 'container',
        height: 250,
        spacingRight: 30
    },
    title: {
        text: 'Market Data: Nasdaq 100'
    },
    subtitle: {
        text: '2011 - 2012'
    },
    xAxis: {
        categories: [ 'Jan', 'Feb', 'Mar', 'Apr',
                      'May', 'Jun', 'Jul', 'Aug',
                      'Sep', 'Oct', 'Nov', 'Dec' ],
        labels: {
            y: 17
        },
        gridLineDashStyle: 'dot',
        gridLineWidth: 1,
        lineWidth: 2,
        lineColor: '#92A8CD',
        tickWidth: 3,
        tickLength: 6,
        tickColor: '#92A8CD',
        minPadding: 0.04,
        offset: 1
    },
    yAxis: [{
        title: {
            text: 'Nasdaq index'
        },
        min: 2000,
        minorGridLineColor: '#D8D8D8',
        minorGridLineDashStyle: 'dashdot',
        gridLineColor: '#8AB8E6',
        alternateGridColor: {
            linearGradient: {
                x1: 0, y1: 1,
                x2: 1, y2: 1
            },
            stops: [ [0, '#FAFCFF' ],
                     [0.5, '#F5FAFF'] ,
                     [0.8, '#E0F0FF'] ,
                     [1, '#D6EBFF'] ]
        },
        lineWidth: 2,
        lineColor: '#92A8CD',
        tickWidth: 3,
        tickLength: 6,
        tickColor: '#92A8CD'
    }, {
        title: {
            text: 'Volume'
        },
        lineWidth: 2,
        lineColor: '#3D96AE',
        tickWidth: 3,
        tickLength: 6,
        tickColor: '#3D96AE',
        opposite: true
    }],
    credits: {
        enabled: false
    },
    plotOptions: {
        column: {
            stacking: 'normal'
        },
        line: {
            zIndex: 2,
            marker: {
                radius: 3,
                lineColor: '#D9D9D9',
                lineWidth: 1
            },
            dashStyle: 'ShortDot'
        }
    },
    series: [{
        name: 'Monthly High',
        // Use stacking column chart - values on
        // top of monthly low to simulate monthly
        // high
        data: [ 98.31, 118.08, 142.55, 160.68, ... ],
        type: 'column'
    }, {
        name: 'Monthly Low',
        data: [ 2237.73, 2285.44, 2217.43, ... ],
        type: 'column'
    }, {
        name: 'Open (Start of the month)',
        data: [ 2238.66, 2298.37, 2359.78, ... ]
    }, {
        name: 'Close (End of the month)',
        data: [ 2336.04, 2350.99, 2338.99, ... ]
    }, {
        name: 'Volume',
        data: [ 1630203800, 1944674700, 2121923300, ... ],
        yAxis: 1,
        type: 'column',
        stacking: null
    }]

雖然圖表看起來有一些複雜，但是我們一步一步來看一遍代碼並逐步釐清。

首先，這裡有兩個yAxis數組，第一個是納斯達克的指數，第二個右邊的y軸是交易量(opposite: true)。在series數組方面，第一個和第二個series都指定為柱狀圖類型(type: 'column')，以覆蓋掉默認的圖表類型折線圖。然後在plotOptions.column中的stacking選項定義為normal，因此可以將月度新高堆積在月度新低上方（藍色和紅色的柱子）。嚴格說起來堆積柱形圖是用來顯示類似分類的比例，為了展示plotOptions的緣故，我們使用堆積柱形圖用來顯示每月的交易高，為了展示我們比較高低之間的差異並用此來代替掉月度新高的series，因此我們會看到數據呈現中高點的值要比低點的值小了很多。

第三個和第四個series是開市和休市的指數，這兩個series都是使用默認的折線圖並且從plotOptions.line中繼承了所有的配置選項。zIndex選項設為2是為了讓這兩條折線可以保持在第五個series-交易量圖層上面顯示，否則與交易量重疊的部分這兩條線就會被交易量覆蓋掉了。Marker對象的配置是為了減小默認的數據點尺寸大小，因為整個圖表有太多的柱狀圖與折線圖，因此表現的組件都需要適當縮小。

最後一個柱狀圖series是交易量，stacking選項在這裡被設置為null，用來覆蓋掉繼承自plotOptions.column中的設定。這個設定會重置series為沒有堆積的柱狀圖，因此顯示的時候就會將其獨立為一個單獨的柱圖來表示，最終，yAxis的索引設置與交易量的y軸看齊(yAxis: 1)