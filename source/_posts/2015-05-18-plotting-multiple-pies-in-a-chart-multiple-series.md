title: "第五章 03.在一個圖表中繪製多個餅圖——複合序列"
date: 2015-05-18 04:00:00
tags:
  - Highcharts
categories:
  - Learning
  - Learning Highcharts
  - Pie Charts
---

# 科技零售(RetailSCM.com) Learning Highcharts中文教程

_<span style="color: #808080;">版權保留，網路轉載請註明來源，謝絕紙質媒體轉載，謝謝。</span>_

## 在一個圖表中繪製多個餅圖——複合序列

在餅圖中，我們可以將另一個對照的餅圖擺在原來餅圖的邊上，讓數據對比顯示的更翔實。要做到這一點我們只要簡單的定義兩個序列設定在序列數組中即可。

我們繼續使用上一個例子中左手邊的餅圖，然後我們使用相同的資料集再新增一個新的類別序列，但這次我們使用遊戲平臺來分組，序列設定如下配置：

    series: [{
            center: ['25%', '50%'],
            data: [
                ['Nintendo', 54030288],
                ['Electronic Arts', 31367739],
                ....
            ]
        }, {
            center: ['75%', '50%'],
            dataLabels: {
                formatter: function() {
                    var str = this.point.name + ': ' +
                        Highcharts.numberFormat(this.percentage, 0) + '%';
                    return formatWithLineBreaks(str);
                }
            },
            data: [
                ['Xbox', 80627548],
                ['PS3', 64788830],
                ...
            ]]
    }]

<!--more-->

如同我們所看到的，我們在這裡使用了一個新的參數center來定位餅圖。這個參數的數組中包含了兩個百分比的數值，第一個餅圖橫向坐標在整個報表容器的橫向百分比，第二個則表示縱向的坐標佔比。預設的值是['50%', '50%']，表示在整個圖表容器的正中間。在這個例子中，我們特別設定了橫向坐標軸分別為'25%' 和 '75%'，來讓這兩個餅圖對稱在容器的左邊和右邊。

在第二個序列中，我們選擇將第二個餅圖中的數值部分改以百分比的方式來呈現，兩個餅圖的圖表如下：

[![5-5](/images/learning_highcharts/5-5.png)](/images/learning_highcharts/5-5.png)

表面上，除了兩個餅圖共用一個標題之外，這和使用標籤來分別繪製兩個圖表沒有什麼太大的差異。我們這麼做主要的好處是我們可以將不同的序列合併在同一個圖表中來顯示，例如我們想要在餅圖中的多序列分組上直接呈現銷售佔比，我們會在下面的章節來學習到如何做。