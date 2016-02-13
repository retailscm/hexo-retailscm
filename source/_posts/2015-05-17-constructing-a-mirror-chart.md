title: "第四章 03.創建一個鏡圖(mirror chart)"
date: 2015-05-17 04:00:00
tags:
  - Highcharts
categories:
  - Learning
  - Learning Highcharts
  - Bar and column charts
---

# 科技零售(RetailSCM.com) Learning Highcharts中文教程

_<span style="color: #808080;">版權保留，網路轉載請註明來源，謝絕紙質媒體轉載，謝謝。</span>_

## 創建一個鏡圖(mirror chart)

另一種比較兩個柱圖的方式是使用鏡圖。與柱狀圖根據臨近柱圖來對齊的方式不同，鏡圖是與其相反的的另一端來對齊。有時這是一種展示兩個series不同趨勢的完美途徑。

在Highcharts中我們可以使用堆積長條圖並略微改造來產生一個鏡圖用以對兩個不同的數據進行橫向比較，為了做到這一點，我們開始用一個新的series來比較英國和中國在過去十年中的專利授權數。

使用這種方式我們先來配置一個堆積長條圖，一組數據為正數另一組則手動轉為負數，使得零值在圖表的中間。然後我們設定invert反轉柱形圖為長條圖並標記負值的範圍為正數，為了呈現上面所說的效果，我們先建立一個堆積柱形圖，讓每個堆積柱圖為一個正數和一個手動轉換的負數組成，代碼如下：

<!--more-->

    chart: {
        renderTo: 'container',
        type: 'column',
        borderWidth: 1
    },
    title: {
        text: 'Number of Patents Granted',
    },
    credits: { ... },
    xAxis: {
        categories: [ '2001', '2002', '2003', ... ],
    },
    yAxis: {
        title: {
            text: 'No. of Patents'
        }
    },
    plotOptions: {
        series: {
            stacking: 'normal'
        }
    },
    series: [{
        name: 'UK',
        data: [ 4351, 4190, 4028, ... ]
    }, {
        name: 'China',
        data: [ -265, -391, -424, ... ]
    }]

下圖為堆積柱形圖且零值在y軸的中間位置：

[![chart4-3-1](/images/learning_highcharts/chart4-3-1.jpg)](/images/learning_highcharts/chart4-3-1.jpg)

然後我們修改設定轉換為長條圖，並增加一個x軸來顯示兩邊都有相同的範圍，最後一步為定義y軸標籤的formatter函數來將負值的標籤轉換為正數顯示，如下：

    chart: {
        .... ,
        type: 'bar',
    },
    xAxis: [{
        categories: [ '2001', '2002', '2003', ... ],
    }, {
        categories: [ '2001', '2002', '2003', ... ],
        opposite: true,
        linkedTo: 0,
    }],
    yAxis: {
        .... ,
        labels: {
            formatter: function() {
                return
                Highcharts.numberFormat(Math.abs(this.value), 0);
            }
        }
    },

下圖為過去十年中國和英國的專利授權書對比圖的最終版本：

[![chart4-3-2](/images/learning_highcharts/chart4-3-2.jpg)](/images/learning_highcharts/chart4-3-2.jpg)

## 擴展為堆積鏡圖

我們可以將堆積柱形圖與分組柱形圖的例子應用到鏡圖上，兩者是相同的規則。代替分組堆積柱形圖的做法在鏡圖中我們可以將它們都堆積在一起，並使用零值來將這兩組堆積圖分開，下圖為歐洲和亞洲使用長條圖來堆積分組的效果：

[![chart4-3-3](/images/learning_highcharts/chart4-3-3.jpg)](/images/learning_highcharts/chart4-3-3.jpg)

南韓和日本的series堆積在左邊（負值的一邊），而英國和德國分組堆積在右邊(正值的一邊)。與前一個圖比起來唯一有一些棘手的是如何來呈現數據標籤。

首先南韓和日本的series數據手動的設定為了負值，其次整個分組都是在要顯示的series之外（因為已經合併為同一個series，但只要顯示部分），我們為這些series開啟數據標籤，並使用如下代碼來進行設定：

    series: [{
        name: 'UK',
        data: [ 4351, 4190, 4028, ... ],
        dataLabels : {
            enabled: true,
            backgroundColor: '#FFFFFF',
            x: 40,
            formatter: function() {
                return
                Highcharts.numberFormat(Math.abs(this.total), 0);
            },
            style: {
                fontWeight: 'bold'
            }
        }
    }, {
        name: 'Germany',
        data: [ 11894, 11957, 12140, ... ],
    }, {
        name: 'S.Korea',
        data: [ -3763, -4009, -4132, ... ],
        dataLabels : {
            enabled: true,
            x: -48,
            backgroundColor: '#FFFFFF',
            formatter: function() {
                return
                Highcharts.numberFormat(Math.abs(this.total), 0);
            },
            style: {
                fontWeight: 'bold'
            }
        }
    }, {
        name: 'Japan',
        data: [ -34890, -36339, -37248, ... ],
    }]

要注意formatter中的定義使用的是this.total而不是this.y，因為我們使用series之外的正值來顯示分組的合計。白色的背景設定可以讓我們避免數據標籤被y軸的間隔線打擾。