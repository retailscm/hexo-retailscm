title: "第四章 02.介紹長條圖"
date: 2015-05-17 03:00:00
tags:
  - Highcharts
categories:
  - Learning
  - Learning Highcharts
  - Bar and column charts
---

# 科技零售(RetailSCM.com) Learning Highcharts中文教程

_<span style="color: #808080;">版權保留，網路轉載請註明來源，謝絕紙質媒體轉載，謝謝。</span>_

## 介紹長條圖

Highcharts提供了2種方式來定義長條圖 – 設置series的type為bar或設置柱形圖的chart.inverted選項為true（當然也可以以此選項來將長條圖轉為柱形圖）。轉換柱狀圖與長條圖只是簡單的轉換y軸和x軸的顯示方向，標籤朝向的設定並不會改變，x和y軸的實際設定也會保留，這代表著我們可以沿用上一個例子並設定為inverted選項為true來產生長條圖，如下：

    chart: {
        .... ,
        type: 'column',
        inverted: true
    },

代碼的效果如下圖所示：

<!--more-->

[![chart4-2-1](/images/learning_highcharts/chart4-2-1.jpg)](/images/learning_highcharts/chart4-2-1.jpg)

國家名稱的標籤朝向和對數軸標籤都有被保留下來，但實際上數值標籤已經彼此覆蓋，並且category名字也沒有對齊到正確的長條圖上。下一步我們要重設標籤的朝向來將圖形恢復正常，我們需要先簡單的轉換一下y軸和x軸的標籤朝向：

    xAxis: {
        categories: [ 'United States',
                      'Japan', 'South Korea', ... ]
    },
    yAxis: {
        .... ,
        labels: {
            rotation: -45,
            align: 'right'
        }
    },

然後我們要繼續重置column的dataLabel設定為默認設定以便於移除rotation的選項設定，並重新調整x和y軸的定位來對齊到長條圖上：

    plotOptions: {
        column: {
        ..... ,
        dataLabels: {
            enabled: true,
            color: '#F4F4F4',
            x: -40,
            y: 5,
            formatter: ....
            style: ...
        }
    }

下圖是修正後的結果：

[![chart4-2-2](/images/learning_highcharts/chart4-2-2.jpg)](/images/learning_highcharts/chart4-2-2.jpg)

## 讓長條圖變得更簡潔一些

我們來將軸背景移除，讓圖表沒有任何裝飾效果使其最簡單。我們移除整個y軸並調整category名字到長條圖本身上，代碼如下：

    yAxis: {
        title: {
            text: null
        },
        labels: {
            enabled: false
        },
        gridLineWidth: 0,
        type: 'logarithmic'
    },

然後我們將國家名標籤移動到長條圖上，同時移除軸線和間隔線，然後調整標籤的對齊方式及他們的x和y定位：

    xAxis: {
        categories: [ 'United States', 'Japan',
                      'South Korea', ... ],
        lineWidth: 0,
        tickLength: 0,
        labels: {
            align: 'left',
            x: 0,
            y: -13,
            style: {
                fontWeight: 'bold'
            }
        }
    },

當我們改變了標籤的對齊方式並將其移動到長條圖上之後，長條圖(繪圖區域)的橫向定位為了考慮舊標籤的定位已經切換到了左邊界，因此我們需要增加左邊界的spacing設定避免圖表看起來太擁擠。

最後我們給繪圖區域增加一個圖片背景來填補其空白的部分，如下：

    chart: {
        renderTo: 'container',
        type: 'column',
        spacingLeft: 20,
        plotBackgroundImage: 'chartBg.png',
        inverted: true
    },
    title: {
        text: null
    },

現在我們的長條圖有了一個更簡潔的外觀：

[![chart4-2-3](/images/learning_highcharts/chart4-2-3.jpg)](/images/learning_highcharts/chart4-2-3.jpg)