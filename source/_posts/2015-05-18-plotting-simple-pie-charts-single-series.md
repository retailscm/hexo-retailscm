title: "第五章 02.繪製一個簡單的餅圖——單序列"
date: 2015-05-18 03:00:00
tags:
  - Highcharts
categories:
  - Learning
  - Learning Highcharts
  - Pie Charts
---

# 科技零售(RetailSCM.com) Learning Highcharts中文教程

_<span style="color: #808080;">版權保留，網路轉載請註明來源，謝絕紙質媒體轉載，謝謝。</span>_

## 繪製一個簡單的餅圖——單序列

在本章中我們會使用vgchartz (www.vgchartz.com)提供的電視遊戲公司數據。下面所使用的餅圖設定及數據是2011年溝通遊戲公司所販售的前100名的遊戲，Wii Sports會剔除在外，因為它是搭配Wii手柄免費提供的。

    chart: {
        renderTo: 'container',
        type: 'pie',
        borderWidth: 1
    },
    title: {
        text: 'Number of Software Games Sold in 2011 Grouped by Publishers',
    },
    credits: {
            ...
    },
    series: [{
        data: [
            ['Nintendo', 54030288],
            ['Electronic Arts', 31367739],
            ...
        ]
    }]

這是一個簡單的餅圖，順時鐘來看，第一個數據節點是任天堂，並且第一個數據結點的位置總是從12點的位置開始，這個沒有辦法設定。

<!--more-->

[![5-1](/images/learning_highcharts/5-1.png)](/images/learning_highcharts/5-1.png)

## 切出一個餅圖的數據序列應如何配置

我們可以改善一下上面的餅圖，讓他包含一個標籤來顯示數值的部分，將一些名字太長的遊戲公司的名稱來折行顯示。為了重新定義dataLabels.formatter選項，我們可以事先定義一個method: formatWithLineBreaks並植入在formatter選項中，這樣我們也可以在其他的例子中複用它。

    function formatWithLineBreaks(str) {
        var words = str.split(' ');
        var lines = [];
        var line = '';
        $.each(words, function(idx, word) {
            if (line.length + word.length &gt; 25) {
                lines.push(line);
                line = '';
            }
            line += word + ' ';
        });
        lines.push(line);
        return lines.join('
    ');
    }

下面的餅圖設定是對餅圖的序列使用allowPointSelect，可以允許使用者通過點擊餅圖的數據節點來實現互動，如餅圖的序列中可以切出一個區域。slicedOffset選項是調整切出的餅圖需要切出多遠的參數。

    plotOptions: {
        pie: {
            slicedOffset: 20,
            allowPointSelect: true,
            dataLabels: {
                formatter: function() {
                    var str = this.point.name + ': ' +
                        Highcharts.numberFormat(this.y, 0);
                    return formatWithLineBreaks(str);
                }
            }
        }
    },

另外，也許我們想要在餅圖載入的時候就將其中對大的一個數據序列切出，讓他的標籤顯示為粗體以便更加醒目。要做到這個效果，我們需要調整最大的數據節點對象的設定，我們將sliced屬性增加到對象中並修改它的預設值為true，然後我們將dataLabels屬性中的fontWeight設定為bold來替換原來的設定：

    series: [{
        data: [{
                name: 'Nintendo',
                y: 54030288,
                sliced: true,
                dataLabels: {
                    style: {
                        fontWeight: 'bold'
                    }
                }
            },
            ['Electronic Arts', 31367739],
            ['Activision', 30230170], ....
        ]
    }]

下圖即為我們重新定義後的圖表

[![5-2](/images/learning_highcharts/5-2.png)](/images/learning_highcharts/5-2.png)

前面有提到過slicedOffset參數可以控制切出的部分可以被切出多遠，預設是10個圖元，這個選項是應用到全部的被切出的部分，我們沒有辦法單獨控制某一個部分使用特別的切出距離，另一個不好的消息是連接線（連接切出部分和數據標籤的連線）看起來會變歪了。在下面的例子中，我們會演示sliced屬性可以被應用到多個數據節點上，並且會移除slicedOffset選項回到原始的預設狀態。

下圖是3個序列都使用任天堂的數據節點設定來將他們來從餅圖中切出。

[![5-3](/images/learning_highcharts/5-3.png)](/images/learning_highcharts/5-3.png)

我們注意到了連接線已經恢復為平滑的連線，而且sliced選項還有另外一個有趣的行為，這些切片的sliced屬性預設都是false，他們中只有一個切片會被切出。例如使用者任意點了屬性為false的一個切片，這個切片會被切出，然後在點Activision公司這個數據節點，Activision會切出，但是之前的切片會縮回到餅圖中，而三個設定了sliced為true的部分仍然會保留他們切出的位置，換句話說就是當我們將sliced設定為true時，能保持他們的狀態獨立與其他設定為false的部分。

## 在一個餅圖上應用圖例

到目前為止圖表上我們包含了很多的數字，但用數字來表示對我們理解每個切片到底應該佔多大其實很難理解，我們需要在切片中列出各切片的百分比。讓我們將所有的遊戲公司名稱列入在圖例中並將其對應的百分佔比顯示在切片中。

為了開啟圖例，我們需要將showInLegend參數設定為true，然後我們設定字體顯示為白色的粗體字，然後將formatter函數設定為this.percentage，這個設定只能使用在餅圖上。distance選項是數據標籤和餅圖外邊界之間的距離設定，正數表示距離餅圖外邊界向外延展的距離，負數則是反方向的距離。

    plotOptions: {
        pie: {
            showInLegend: true,
            dataLabels: {
                distance: -24,
                color: 'white',
                style: {
                    fontWeight: 'bold'
                },
                formatter: function() {
                    return Highcharts.numberFormat(this.percentage) + '%';
                }
            }
        }
    },

下面是圖例的設定，我們設定一下邊界的並將圖例框設定距離餅圖更近一些，如下：

    legend: {
        align: 'right',
        layout: 'vertical',
        verticalAlign: 'middle',
        itemMarginBottom: 4,
        itemMarginTop: 4,
        x: -40
    },

下面就是另外一個餅圖的呈現方式了。

[![5-4](/images/learning_highcharts/5-4.png)](/images/learning_highcharts/5-4.png)