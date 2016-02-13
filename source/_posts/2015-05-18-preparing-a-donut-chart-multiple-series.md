title: "第五章 04.準備一個甜甜圈圖——複合序列"
date: 2015-05-18 05:00:00
tags:
  - Highcharts
categories:
  - Learning
  - Learning Highcharts
  - Pie Charts
---

# 科技零售(RetailSCM.com) Learning Highcharts中文教程

_<span style="color: #808080;">版權保留，網路轉載請註明來源，謝絕紙質媒體轉載，謝謝。</span>_

## 準備一個甜甜圈圖——複合序列

Highcharts提供了另一種類型的餅圖-甜甜圈圖。它具有特別的鑽取(drill-down)可以將類別展細到子類別，可以更方便的查看到更明細的資料，鑽取效果也可以用到多層級上。在本節中，我們會創建一個簡單的甜甜圈圖並在外環來呈現遊戲名稱，在內環上顯示遊戲公司。

為簡單起見，我們只是用三家遊戲公司的資料在內圈，下面是甜甜圈圖序列數組的設定。

<!--more-->

    series: [{
            name: 'Publishers',
            dataLabels: {
                distance: -70,
                color: 'white',
                formatter: function() {
                    return this.point.name + ': ' + Highcharts.numberFormat(this.y / 1000000, 2);
                },
                style: {
                    fontWeight: 'bold'
                }
            },
            data: [
                ['Nintendo', 54030288],
                ['Electronic Arts',
                    31367739
                ],
                ['Activision', 30230170]
            ]
        }, {
            name: 'Titles',
            innerSize: '60%',
            dataLabels: {
                formatter: function() {
                    var str = '' + this.point.name ': ' + Highcharts.numberFormat(this.y / 1000000, 2);
                    return formatWithLineBreaks(str);
                }
            },
            data: [ // Nintendo
                {
                    name: 'Pokemon B&W',
                    y: 8541422,
                    color: colorBrightness("#4572A7",
                        0.05)
                }, {
                    name: 'Mario Kart',
                    y: 5349103,
                    color: colorBrightness('#4572A7',
                        0.1)
                },
                ....
                // EA
                {
                    name: 'Battlefield 3',
                    y: 11178806,
                    color: colorBrightness('#AA4643',
                        0.05)
                },
                ....
                // Activision
                {
                    name: 'COD: Modern Warfare 3',
                    y: 23981182,
                    color: colorBrightness('#89A54E',
                        0.1)
                },
                ....
            }]
    }]

首先，我們有兩個序列-內圈的餅圖序列（遊戲公司）和外圈的序列（遊戲名稱）。遊戲名稱的序列包含全部的子類別數據並且與其對應的遊戲發行公司的名稱對齊。而順序是任天堂的順序在EA前面，子類別的順序則是按照數值來進行排序（可以參考遊戲名稱在數據數組中的排序）

每一個子類別序列上的數據節點都被聲明為一個數據節點對象來分配一個他們所屬的主類別的色系。

Highcharts demo所演示的顏色亮點變化，可如此實現：

    color: Highcharts.Color(color).brighten(brightness).get()

基本上來說，我們通過使用主類別的顏色來創建一個顏色對象，然後再根據子類別的數值來調整顏色的亮度漸層參數。我們加入了一個colorBrightness的方法，並將其配置到圖表的設定中來重寫這個例子。

    function colorBrightness(color, brightness) {
        return Highcharts.Color(color).brighten(brightness).get();
    }

下面的部分會制定哪一個序列會在甜甜圈圖的內部餅圖呈現，哪一個會在外環上呈現。innerSize的選項會使用在外環的序列上（這裡是遊戲名稱）來確認內圈的大小。如我們的設定結果，遊戲名稱的序列形成了一個同心環，innerSize的數值可以為尺寸大小也可以為百分比來確認甜甜圈圖的大小。

最後的部分則使用數據標籤來美化圖表，我們希望將內部餅圖的數據標籤顯示在餅圖上面，所以我們使用負值來設定在dataLabels.distance上來實現。為了代替列印一長串的數值，我們定義formatter來轉換這些數值為百萬進行單位來顯示。

下圖即為產出的甜甜圈圖

[![5-6](/images/learning_highcharts/5-6.png)](/images/learning_highcharts/5-6.png)

需要注意的是Highcharts並不強制將甜甜圈圖中間呈現為餅圖，這裡只是為了呈現這個例子才這樣做。我們可以使用多個同心環來代替，下面的的圖表與上一個示例完全一樣，只是使用innerSize選項來設定內環序列的遊戲公司：

[![5-7](/images/learning_highcharts/5-7.png)](/images/learning_highcharts/5-7.png)

我們可以在給甜甜圈圖上增加第三個序列，這個示例簡單的增加一個新的序列及更多的資料，代碼的部分可以參考[http://joekuan.org/Learning_Highcharts/Chapter_5/](http://joekuan.org/Learning_Highcharts/Chapter_5/)，兩個外環序列使用innerSize選項來定義，內部的圓形圖更小了以至於無法放下資料標籤，因此我們使用showInLegend選項開啟圖例框來呈現內部餅圖的標籤。

[![5-8](/images/learning_highcharts/5-8.png)](/images/learning_highcharts/5-8.png)