title: "第三章 04.散點圖和面積圖的組合"
date: 2015-05-16 06:00:00
tags:
  - Highcharts
categories:
  - Learning
  - Learning Highcharts
  - Line area and scatter charts
---

# 科技零售(RetailSCM.com) Learning Highcharts中文教程

_<span style="color: #808080;">版權保留，網路轉載請註明來源，謝絕紙質媒體轉載，謝謝。</span>_

## 散點圖和面積圖的組合

Highcharts也支援散點圖表用以繪製數據的走勢。在這裡我們使用不同的散點series使我們的圖表有點像一張海報。

首先，我們使用0-14年齡組數據的一個子集，並設置scatter類型：

    name: 'Ages 0 to 14',
    type: 'scatter',
    data: [ [ 1982, 23 ], [ 1989, 19 ],
            [ 2007, 14 ], [ 2004, 14 ],
            [ 1997, 15 ], [ 2002, 14 ],
            [ 2009, 13 ], [ 2010, 13 ] ]

接下來設置散點series數據標籤可用，並確保標記形狀一直為 'circle'，如下：

<!--more-->

    plotOptions: {
        scatter: {
            marker: {
                symbol: 'circle'
            },
            dataLabels: {
                enabled: true
            }
        }
    }

前面的代碼片段的效果如下圖：

[![chart3-4-1](/images/learning_highcharts/chart3-4-1.jpg)](/images/learning_highcharts/chart3-4-1.jpg)

_Highcharts提供了一series的標記符號，並支援使用者使用他們自己提供的標記圖示（見第二章 Highcharts配置）。支援的標記符號有：圓形，正方形，菱形，三角形和倒三角。_


## 使用風格美化圖表

下一步使用 radius 屬性將每個離散的點美化為成氣泡樣式，並手動設置數據標籤的字體大小與數據成比例。然後使用verticalAlign將標籤在變大了的散點內部居中顯示。我們可以使用數據點的屬性來調整每個散點為不同的大小顯示效果。要做到以上的部分則我們需要改變series數據中點對象的定義，如下：

    plotOptions: {
        scatter: {
            marker: {
                symbol: 'circle'
            },
            dataLabels: {
                enabled: true,
                verticalAlign: 'middle'
            }
        }
    },
    data: [ {
        dataLabels: {
            style: {
                fontSize: '25px'
            }
        },
        marker: { radius: 31 },
        y: 23,
        x: 1982
    }, {
        dataLabels: {
            style: {
                fontSize: '22px'
            }
        },
        marker: { radius: 23 },
        y: 19,
        x: 1989
    }, .....

下面的截圖顯示了數據點序列隨著百分比數值的變化，從一個大的標記和字體逐漸變得小：

[![chart3-4-2](/images/learning_highcharts/chart3-4-2.jpg)](/images/learning_highcharts/chart3-4-2.jpg)

上面的圖仍有兩個問題。1.散點series的顏色與標記中灰色的文本標籤不協調導致難以分辨。

為瞭解決這一問題，我們用如下梯度設置將散點series改為更清爽的顏色。

    color: {
        linearGradient: { x1: 0, y1: 0, x2: 0, y2: 1 },
        stops: [ [ 0, '#FF944D' ],
                 [ 1, '#FFC299' ] ]
    },

然後在plotOptions中為離散點設置一個比較深的輪廓，如下：

    plotOptions: {
        scatter: {
            marker: {
                symbol: 'circle',
                lineColor: '#E65C00',
                lineWidth: 1
            },

第二個問題是，數據點在坐標軸結束時被遮擋住，這個問題可以通過增加兩個坐標軸額外的空白解決：

    yAxis: {
        .....,
        maxPadding: 0.09
    },
    xAxis: {
        .....,
        maxPadding: 0.02
    }

以下是新的圖表一覽：

[![chart3-4-3](/images/learning_highcharts/chart3-4-3.jpg)](/images/learning_highcharts/chart3-4-3.jpg)

下一部分我們會新增一個logo和一些裝飾的文字。向圖表引入圖片有兩種途徑：plotBackgroundImage選項和renderer.image API調用。

plotBackgroundImage會將整個圖片設為圖表的背景，這並不是我們打算要做的。

renderer.image方法在圖片位置和大小上提供了更多的控制可能性。下面是當創建好圖表後的調用：

    var chart = new Highcharts.Chart({
    ...
    });
    chart.renderer.image('logo.png', 240, 10, 187, 92).add();

logo.png是logo圖片檔的路徑，接下來的兩個參數是logo將顯示在圖表中的x和y的位置（從0開始，0是左上角）；最後兩個參數是圖片檔的寬度和高度。image主要返回一個元素物件，而後面的.add將返回的圖片物件放入到renderer中。
至於裝飾的文字，樣式為一個紅色圓圈中的大小不同的白色加粗文字。他們都是通過renderer創建的。下面的程式片段中第一個renderer的調用創建了一個紅色的圓圈，x和y的位置以及半徑。然後直接通過attr方法設置SVG屬性。接下來的三個renderer調用用來創建暈圈中的文字，使用了css方法設置字體大小，樣式和樣色。我們還會在第七章- Highcahrts APIs中再次見到charts.renderer方法。

    // Red circle at the back
    chart.renderer.circle(220, 65, 45).attr({
        fill: '#FF7575',
        'fill-opacity': 0.6,
        stroke: '#B24747',
        'stroke-width': 1
    }).add();
    // Large percentage text with special font
    chart.renderer.text('37.5%', 182, 63).css({
        fontWeight: 'bold',
        color: #FFFFFF',
        fontSize: '30px',
        fontFamily: 'palatino'
    }).add();
    // Align subject in the circle
    chart.renderer.text('65 and over', 184, 82).css({
        'fontWeight': 'bold',
    }).add();
    chart.renderer.text('by 2050', 193, 96).css({
        'fontWeight': 'bold',
    }).add();

最後我們將圖例（legend box）移到圖表的頂部。為了將圖例定位到繪圖區域，我們需要設置floating屬性為true，這樣會強制圖例為固定佈局模式。然後移除默認的邊框線，並設置圖例中的條目垂直排列：

    legend: {
        floating: true,
        verticalAlign: 'top',
        align: 'center',
        x: 130,
        y: 40,
        borderWidth: 0,
        layout: 'vertical',
    },

下面是加上裝飾後的最終效果：

[![chart3-4-4](/images/learning_highcharts/chart3-4-4.jpg)](/images/learning_highcharts/chart3-4-4.jpg)

## 總結

在本章中我們探索了曲線圖，面積圖和散點圖的用法。我們可以發現Highcharts能夠提供非常大的靈活性以便做出類似海報的圖表。在下一章中，我們將會學到如何繪製條狀圖和柱狀圖，以及他們的相關選項。