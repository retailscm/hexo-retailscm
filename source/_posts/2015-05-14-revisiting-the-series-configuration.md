title: "第二章 04.重溫series配置"
date: 2015-05-14 02:00:00
tags:
  - Highcharts
categories:
  - Learning
  - Learning Highcharts
  - Highcharts configurations
---

# 科技零售(RetailSCM.com) Learning Highcharts中文教程

_<span style="color: #808080;">版權保留，網路轉載請註明來源，謝絕紙質媒體轉載，謝謝。</span>_

## 重溫series配置

現在關於series屬性可以做什麼，我們應該已經有一些概念了，在本節中，我們繼續來測試更多的細節。

series屬性是一組包含了數據和series特定選項的配置對象數組，它允許我們指定一個series數據也可以指定多個series數據。Series對象的目的是為了通知Highcharts要格式化數據並確認怎樣在圖表上顯示這些數據。

圖表上所有的數據都是通過data的屬性來指定，data屬性非常有彈性，可以接受以下多種形式的數組：

*   數值
*   有x和y值的矩陣數組
*   使用數據點(data point)描述屬性的點對象(point object)

<!--more-->

前兩種選項在“訪問軸的數據類型”一節中有介紹過了，在本節中我們會在看一下第三種選項。我們先使用納斯達克單軸的例子，在此例子中我們會指定一組混合數字與對象的series data：

    series: [{
        name: 'Nasdaq',
        pointStart: Date.UTC(2012, 4, 11, 9, 30),
        pointInterval: 30 * 60 * 1000,
        data: [{
            // First data point
            y: 2606.01,
            marker: {
                symbol: 'url(./sun.png)'
            }
        }, 2622.08, 2636.03, 2637.78,
        {
            // Highest data point
            y: 2639.15,
            dataLabels: {
                enabled: true
            },
            marker: {
                fillColor: '#33CC33',
                radius: 5
            }
        }, 2637.09, 2633.38, 2632.23, 2632.33,
           2632.59, 2630.34, 2626.89, 2624.59,
        {
            // Last data point
            y: 2615.98,
            marker: {
                symbol: 'url(./moon.png)'
            }
        }]
    }]

第一個和最後一個數據點是一個對象，包含了y軸的值和一個標誌著開市和休市的圖標。最高點的數據點則設定了不同的顏色及增加一個數據標籤。數據點的大小也比默認配置要更大一些，其他的數據數組只是簡單的數值類型，結果如下圖所示：

[![chart2-4-1](/images/learning_highcharts/chart2-4-1.jpg)](/images/learning_highcharts/chart2-4-1.jpg)

目前在對齊單個數據點上的圖像有一個小缺陷，但指定全部數據點的圖像是沒有問題的。([http://github.com/highslide-software/highcharts.com/issues/969](http://github.com/highslide-software/highcharts.com/issues/969)).