title: "第四章 04.將一個長條圖轉為水平刻度圖"
date: 2015-05-17 05:00:00
tags:
  - Highcharts
categories:
  - Learning
  - Learning Highcharts
  - Bar and column charts
---

# 科技零售(RetailSCM.com) Learning Highcharts中文教程

_<span style="color: #808080;">版權保留，網路轉載請註明來源，謝絕紙質媒體轉載，謝謝。</span>_

## 將一個長條圖轉為水平刻度圖

水平刻度圖通常用於表示當前閥值，因此意味著y軸上的極值是固定在兩端的。另一個特點是x軸上只能顯示當前時間的一個值(一個維度)。

下面我們會來學習如何將一個長條圖轉換為一個水平刻度圖。基本的概念是將繪圖區域減小到與長條圖為相同大小，因此我們要同時為繪圖區域和長條圖設置固定的大小，並且不要考慮容器的尺寸。為了達成這點，我們需要設定chart.width和chart.height為一些需要的值，然後我們對繪圖區的邊框和背景色美化，使之看起來類似與一個容器壓力表：

    chart: {
        renderTo: 'container',
        type: 'bar',
        plotBorderWidth: 2,
        plotBackgroundColor: '#D6D6EB',
        plotBorderColor: '#D8D8D8',
        plotShadow: true,
        spacingBottom: 43,
        width: 350,
        height: 120
    },

然後我們設定y軸的標籤為不顯示，並設定一個固定的百分比間隔：

<!--more-->

    xAxis: {
        categories: [ 'US' ],
        tickLength: 0
    },
    yAxis: {
        title: {
            text: null
        },
        labels: {
            y: 20
        },
        min: 0,
        max: 100,
        tickInterval: 20,
        minorTickInterval: 10,
        tickWidth: 1,
        tickLength: 8,
        minorTickLength: 5,
        minorTickWidth: 1,
        minorGridLineWidth: 0
    },

最後一步是設置長條圖的寬度以完美的貼合繪圖區域，其餘的設定是為了給長條圖設定一個SVG的漸變效果，如下：

    series: [{
        borderColor: '#7070B8',
        borderRadius: 3,
        borderWidth: 1,
        color: {
            linearGradient:
                { x1: 0, y1: 0, x2: 1, y2: 0 },
            stops: [
                [ 0, '#D6D6EB' ],
                [ 0.3, '#5C5CAD' ],
                [ 0.45, '#5C5C9C' ],
                [ 0.55, '#5C5C9C' ],
                [ 0.7, '#5C5CAD' ],
                [ 1, '#D6D6EB'] ]
        },
        pointWidth: 50,
        data: [ 48.9 ]
    }]

下面為裝飾過的最終刻度圖效果：

[![chart4-4-1](/images/learning_highcharts/chart4-4-1.jpg)](/images/learning_highcharts/chart4-4-1.jpg)