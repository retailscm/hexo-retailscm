title: "第三章 02.繪製面積圖"
date: 2015-05-16 04:00:00
tags:
  - Highcharts
categories:
  - Learning
  - Learning Highcharts
  - Line area and scatter charts
---

# 科技零售(RetailSCM.com) Learning Highcharts中文教程

_<span style="color: #808080;">版權保留，網路轉載請註明來源，謝絕紙質媒體轉載，謝謝。</span>_

## 繪製面積圖

在本節，我們會將前一個例子修改成一個設定有格調的面積曲線圖（基於Kristin Clute的風能海報設計風格）。 結合使用 area 和 spline 屬性將會生成面積曲線圖，主要的數據線被繪製成一個雲形曲線，曲線下的區域被漸變且有透明效果的類似顏色填充。 如圖：

[![chart3-2-1](/images/learning_highcharts/chart3-2-1.jpg)](/images/learning_highcharts/chart3-2-1.jpg)

<!--more-->

首先，我們希望圖表的使用者能夠方便的看到當前趨勢的數值，所有我們將y軸移動到最近年份（2010)的旁邊,也就是說放到了圖表的另一邊：

    yAxis: { ....
        opposite:true
    }

接下來，移除間隔線，並調整y軸軸線的寬度，獲得一個較細的軸線：

    yAxis: { ....
        gridLineWidth: 0,
        lineWidth: 1,
    }

然後用百分比標識（%）定義y軸的標題，並使之與軸線頂部對齊：

    yAxis:{
        title:{
            text:'(%)',
            rotation:0,
            x:10,
            y:5,
            align:'high'
        },
    }

至於x軸，我們使用紅色加粗軸線，並刪除軸線上的時間刻度：

    xAxis:{
        lineColor:'#CC2929',
        lineWidth:4,
        tickWidth:0,
        offset:2
    }

對於標題，將其移動到圖表的右側，增加標題與圖表的空白，並設置一個不同的字體：

    title:{
        text:'Population ages 65 and over (% of total)- Japan ',
        margin:40,
        align:'right',
        style:{
            fontFamily:'palatino'
        }
    }

在以上設置之後，我們開始修改全部series的表現形式，首先將 chart.type 屬性由 'line' 改為 'areaspline'。要注意的是，在series 物件中設置這個屬性會覆蓋plotOptions.series中定義的 plotOptions.areaspline屬性。

由於現在圖表中只有一個series，所以沒有必要顯示圖例(legend box),使用 showInLegend 屬性刪除它， 接下來使圖表變漂亮些，使用漸變的顏色填充面積區域，並為雲形曲線設置一個比較暗的顏色：

    series: [{
        showInLegend: false,
        lineColor: '#145252',
        fillColor: {
        linearGradient: {
            x1: 0, y1: 0,
            x2: 0, y2: 1
        },
        stops:[ [ 0.0, '#248F8F' ] ,
                [ 0.7, '#70DBDB' ],
                [ 1.0, '#EBFAFA' ] ]
        },
        data: [ ... ]
    }]

之後，我們沿著曲線引入一對數據標籤，以表明老年人口的排名隨著過去的時間而增長.使用series數據陣列中對應1995和2010年的數據，並轉換數位值為數據點物件。由於我們只想呈現這兩年的數據點，所以在全域屬性中關閉 plotOptions.series.marker.enabled 屬性並在數據點物件中的樣式設置中個別的開啟標識點：

    plotOptions: {
        series: {
            marker: {
                enabled: false
            }
        }
    },
    series: [{ ...,
        data:[ 9, 9, 9, ...,
            { marker: {
                radius: 2,
                lineColor: '#CC2929',
                lineWidth: 2,
                fillColor: '#CC2929',
                enabled: true
            },
            y: 14
            }, 15, 15, 16, ... ]
    }]

接下來，我們為數據標籤繪製一個外邊框，外邊框為一個圓角(borderRadius)紅色(borderColor)矩形。 使用x或y 選項可以很好的調整數據標籤的位置。最後，我們更改數據標籤的默認格式方法，使之返回人口在國家中的排名而不是數據點的值：

    series: [{ ...,
        data:[ 9, 9, 9, ...,
            { marker: {
                ...
                },
                dataLabels: {
                enabled: true,
                borderRadius: 3,
                borderColor: '#CC2929',
                borderWidth: 1,
                y: -23,
                formatter: function() {
                    return "Rank: 15th";
                }
            },
            y: 14
        }, 15, 15, 16, ... ]
    }]

最後一項操作時為圖表應用一個灰色背景並增加底部空白(spacingBottom),spacingbottom額外的空白是為了避免credit lable(RetailSCM.com) 與x軸lable太過靠近，因為我們禁用了圖例(legend box)。

    chart: {
        renderTo: 'container',
        spacingBottom: 30,
        backgroundColor: '#EAEAEA'
    },

當所有的配置都設置好後，就會顯示出本節開頭的截圖了。