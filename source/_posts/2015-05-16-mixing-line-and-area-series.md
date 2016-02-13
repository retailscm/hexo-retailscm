title: "第三章 03.折線圖和面積圖的組合"
date: 2015-05-16 05:00:00
tags:
  - Highcharts
categories:
  - Learning
  - Learning Highcharts
  - Line area and scatter charts
---

# 科技零售(RetailSCM.com) Learning Highcharts中文教程

_<span style="color: #808080;">版權保留，網路轉載請註明來源，謝絕紙質媒體轉載，謝謝。</span>_

##折線圖和面積圖的組合

在這一部分中我們將探索將不同的類型圖綜合在一起，即混合折線和面積圖：

*   投影圖
*   混合面積曲線圖和階梯線圖
*   研究堆積面積曲線圖圖，將兩個面積曲線圖從上部累加

## 模擬一個投影圖

投影圖分含使用真實數據的面積曲線部分和投影數據的虛線延續部分。所以我們將數據分為兩個series，一個是真實數據，另一個是投影數據。下面是series配置的代碼。數據基於國家研究所的人口和社會安全研究報告([http://www.ipss.go.jp/pp-newest/e/ppfj02/ppfj02.pdf](http://www.ipss.go.jp/pp-newest/e/ppfj02/ppfj02.pdf))

<!--more-->

    series: [{
        name: 'project data',
        type: 'spline',
        showInLegend: false,
        lineColor: '#145252',
        dashStyle: 'Dash',
        data: [ [ 2010, 23 ], [ 2011, 22.8 ],
            ... [ 2024, 28.5 ] ]
    }]

未來的series配置為虛線曲線，並且禁用了圖例，因為我們想將這兩個series呈現為同一個series，即一個真實數據，一個未來投影數據。將未來series的顏色設置為與第一個series相同，最後再結構化數據。由於我們用 pointStart 屬性定義了x軸時間的部分，所以需要對齊2010年後的投影數據。下面有兩個途徑定義時間數據為連續的形式，如下：

*   為第二個series輸入null值，用以對齊真實數據
*   用元素集定義第二series，包括時間和投影數據
下面使用第二種途徑，因為呈現比較簡單。下面是未來數據series的截圖：

[![chart3-3-1](/images/learning_highcharts/chart3-3-1.jpg)](/images/learning_highcharts/chart3-3-1.jpg)

真實數據series與繪製面積圖章節開始的圖形基本一致，除了沒有數據點標記和數據標籤的美化。下一步就是聯合兩個series，如下：

    series: [{
        name: 'real data',
        type: 'areaspline',
        ....
    }, {
        name: 'project data',
        type: 'spline',
        ....
    }]

由於兩個series的數據沒有重疊，因此產生一個平滑的投影圖表：

[![chart3-3-2](/images/learning_highcharts/chart3-3-2.jpg)](/images/learning_highcharts/chart3-3-2.jpg)

## 曲線圖與階梯線圖對比

在這部分中我們將要繪製一個曲線圖和一個階梯線圖。階梯線圖是水準和垂直走向的，僅僅按照數據的變化產生變化。通常用來展現離散的數據，即數據不是連續平緩的變動。

為了展示階梯線圖，我們繼續用之前的曲線圖例子。首先，移除 showInLegend 和 dataLables 屬性以便顯示圖例和數據標籤。

接下來加入一個新的series - 年齡0-14歲 ，樣式為預設折線圖。然後修改為階梯線圖。下面是配置的代碼：

    series: [{
        name: 'Ages 65 and over',
        type: 'areaspline',
        lineColor: '#145252',
        pointStart: 1980,
        fillColor: {
            ....
        },
        data: [ 9, 9, 9, 10, ...., 23 ]
    }, {
        name: 'Ages 0 to 14',
        // default type is line series
        step: true,
        pointStart: 1980,
        data: [ 24, 23, 23, 23, 22, 22, 21,
                20, 20, 19, 18, 18, 17, 17, 16, 16, 16,
                15, 15, 15, 15, 14, 14, 14, 14, 14, 14,
                14, 14, 13, 13 ]
    }]

下面是階梯線圖的例子：

[![chart3-3-3](/images/learning_highcharts/chart3-3-3.jpg)](/images/learning_highcharts/chart3-3-3.jpg)

## 擴展到一個堆積面積圖

本節中我們會將兩個series轉變為面積曲線圖，並將他們從上部堆積在一起，創建一個堆積面積圖。 使我們能夠在此圖表中粗略的觀察到數量，比例和總幅度。

讓我們來把第二個series改為另一個 areaspline 類型：

    name: 'Ages 0 to 14',
    type: 'areaspline',
    pointStart: 1980,
    data: [ 24, 23, 23, ... ]

將 stacking 選項設為 'normal' 做為 areaspline的默認設置，如下：

    plotOptions:{
        areaspline:{
            stacking:'normal'
        }
    }

這樣就使得兩個面積圖由上部互相堆疊起來。以此我們可以觀察到兩個年齡組的人口互相補償達到總人口的33%左右，而65歲及以上的年齡組將會在稍後階段快速增長。

[![chart3-3-4](/images/learning_highcharts/chart3-3-4.jpg)](/images/learning_highcharts/chart3-3-4.jpg)

設想下，如果有三組面積曲線圖，但我們只想將其中兩個疊加。（儘管使用柱狀圖比這樣更加清晰）正如在Highcharts配置第二章 探索plotOption中所述，我們可以在plotOptions.series中設置stacking='normal'，然後手動關閉第三個series的stacking配置，下面是具體的配置方法：

    plotOptions: {
        series: {
            marker: {
                enabled: false
            },
            stacking: 'normal'
        }
    },
    series: [{
    name: 'Ages 65 and over',
    ....
    }, {
        name: 'Ages 0 to 14',
        ....
    }, {
        name: 'Ages 15 to 64',
        type: 'areaspline',
        pointStart: 1980,
        stacking: null,
        data: [ 67, 67, 68, 68, .... ]
    }]

這樣就利用第三個series15-64年齡組生成了一個面積曲線圖，並涵蓋了前兩個堆積的series，如下：

[![chart3-3-5](/images/learning_highcharts/chart3-3-5.jpg)](/images/learning_highcharts/chart3-3-5.jpg)

## 繪製有缺失數據的圖表

當一個series有缺失的數據時，Highcharts會預設將其顯示為間斷的線。而 connectNulls 選項可以使得當series中有缺失的數據時仍保持折線的連續，此選項的預設值是 false 。可以設置兩個存在空數據的曲線圖來檢驗一下其預設動作並設定數據標記為可用狀態，以便能夠清晰的觀察到缺失的數據點。

    series: [{
        name: 'Ages 65 and over',
        connectNulls: true,
        ....,
        // Missing data from 2004 - 2009
        data: [ 9, 9, 9, ...., 23 ]
    }, {
        name: 'Ages 0 to 14',
        ....,
        // Missing data from 1989 - 1994
        data: [ 24, 23, 23, ...., 13 ]
    }]

下面的圖表通過樣曲線圖呈現了缺失的數據點的兩種樣式：

[![chart3-3-6](/images/learning_highcharts/chart3-3-6.jpg)](/images/learning_highcharts/chart3-3-6.jpg)

正如我們所看到的年齡0-14歲的series具有明顯的間斷，而65歲及以上series因配置 connectNulls 選項為 true，就將缺失的數據點用曲線連接起來了。如果數據標記不可用，我們就不能注意到這樣的差別。

然而，我們應該小心使用這個選項，並特別注意在使用 stacking選項時不要使其可用。設想下，我們有兩個堆積的面積圖，其中只有在底部的年齡0-14歲的series有缺失的數據。那麼程式對缺失數據的預設動作會令圖表看上去如下圖：

[![chart3-3-7](/images/learning_highcharts/chart3-3-7.jpg)](/images/learning_highcharts/chart3-3-7.jpg)

儘管底部的series顯示了缺失間斷的部分，總體的堆積圖也是正確的。在同一區域的上層series下降至單一series值，整體的比例也保持不變。 問題所在是當 connectNulls 為true時，如果沒有注意到series中有缺失的數據，結果將是前後矛盾的圖表，如下：

[![chart3-3-8](/images/learning_highcharts/chart3-3-8.jpg)](/images/learning_highcharts/chart3-3-8.jpg)

底部的series覆蓋了上部series留下的空白部分，使堆積圖的比例錯誤。