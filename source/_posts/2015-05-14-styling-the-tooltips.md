title: "第二章 06.提示框的樣式"
date: 2015-05-14 04:00:00
tags:
  - Highcharts
categories:
  - Learning
  - Learning Highcharts
  - Highcharts configurations
---

# 科技零售(RetailSCM.com) Learning Highcharts中文教程

_<span style="color: #808080;">版權保留，網路轉載請註明來源，謝絕紙質媒體轉載，謝謝。</span>_

## 提示框的樣式

提示框在Highcharts中需要設定選項tooltip.enabled來手動開啟。提示框內容部分的呈現是很靈活的，可以使用一個callback函數或使用html樣式。我們還是繼續上一節中的例子，在圖表中現在有包含了多個柱圖和折線，首先我們開啟十字線提示框來幫助我們對齊數據點軸上的指數。crosshairs設定可以使用一個布爾值來開啟這項特性或使用一個對象來設定十字線樣式。下面的代碼是一個用x和y軸設定的數組來設定的十字線樣式：

    tooltip : {
        crosshairs: [{
            color: '#5D5D5D',
            dashStyle: 'dash',
            width: 2
        }, {
            color: '#5D5D5D',
            dashStyle: 'dash',
            width: 2
        }]
    },

下圖顯示鼠標移動到一個休市series的數據點上懸浮，我們可以按到一個灰色的十字線及其對應的提示框。

<!--more-->

[![chart2-4-3](/images/learning_highcharts/chart2-4-3.jpg)](/images/learning_highcharts/chart2-4-3.jpg)

## 使用HTML格式化提示框

Highcharts提供模板選項如headerFormat, pointFormat,footerFormat來構建提示框。我們可以在模板中使用這些變量如point.x,point.y,series.name,series.color，如我們使用pointFormat的默認提示框，則可參考如下代碼中默認值的部分：

    <span style="color:{series.color}">{series.name}</span>:
    <b>{point.y}</b><br/>

Highcarts會轉換這個表達式為SVG的文本標記，因此只能支持一小部分的HTML語法，如 &lt;b&gt;, &lt;br&gt;, &lt;strong&gt;, &lt;em&gt;, &lt;i&gt;, &lt;span&gt;, &lt;href&gt;, 以及css中的font style屬性。如果我們需要更多的彈性來美化樣式及使用圖片等，我們需要使用useHTML選項來設定完成HTML的提示框，這個選項允許我們做到以下特性：

*   在tooltip中使用其他的HTML標籤，如&lt;img&gt;等
*   使用真實的HTML內容來創建一個提示框，而不是使用SVG標籤

這裡我們回來使用一個HTML表格作為提示框，我們會使用headerFormat來創建一個標題，並使用其下邊框來分表頭與數據部分。圖片則是基於series.index的宏，所以不同的series會對應不同的圖標，我們使用series.color宏來高亮顯示series的名字並使用與圖表相同的顏色來應用到series.data宏去改變series的值。

    tooltip : {
        useHTML: true,
        headerFormat: '<table><thead><tr>' +
            '<th style="border-bottom: 2px solid #6678b1; color: #039" ' +
            'colspan=2 >{point.key}</th></tr></thead><tbody>',
        pointFormat: '<tr><td style="color: {series.color}">' +
            '<img src="./series_{series.index}.png" ' +
            'style="vertical-align:text-bottom; margin-right: 5px" >' +
            '{series.name}: </td><td style="text-align: right; color: #669;">' +
            '<b>{point.y}</b></td></tr>',
        footerFormat: '</tbody></table>'
    },

當我們將鼠標懸浮到一個數據點時，模板變量point會被替換為一個point對象，series會被一個包含數據點的series對象替換。

下圖是一個新的提示框，圖標在休市series名稱邊上：

[![chart2-4-4](/images/learning_highcharts/chart2-4-4.jpg)](/images/learning_highcharts/chart2-4-4.jpg)

## 使用回調函數

另外，我們可以使用JavaScript中的回調函數來優化提示框。提示框的手柄(handler)是使用formatter選項來宣告的，template選項與手柄最大的不同是當數據點設定條件返回false的時候，我們可以關閉提示框的顯示功能，而template選項則不可以做到。在回調函數的例子中我們使用this.series和 this.point來為鼠標懸浮的數據點取得其series名字與值。

    formatter: function() {
        return '<span style="color:#039;font-weight:bold">' +
                this.point.category +
                '</span><br/><span style="color:' +
                this.series.color + '">' + this.series.name +
                '</span>: <span style="color:#669;font-weight:bold">'
                + this.point.y + '</span>';
    }

上面的代碼返回一個SVG文本的提示框，樣式如下：

[![chart2-4-5](/images/learning_highcharts/chart2-4-5.jpg)](/images/learning_highcharts/chart2-4-5.jpg)

## 應用多series的提示框

另一個很有靈活性的提示框特性是允許全部的series數據可以顯示在同一個提示框。當用戶在查看一個多維圖的數據圖表時，這個行為可以讓使用者交互行為變的更簡單。如果要開啟這個特性，我們需要設定shared選項為ture。

我們繼續前面的例子來再增加一個多維度的提示框。

下面是新的提示框的代碼：

    tooltip : {
        shared: true,
        useHTML: true,
        headerFormat: '<table><thead><tr><th colspan=2 >' +
            '{point.key}</th></tr></thead><tbody>',
        pointFormat: '<tr><td style="color: {series.color}">' +
            '{series.name}: </td>' +
            '<td style="text-align: right; color: #669;"> '
            + '<b>{point.y}</b></td></tr>',
        footerFormat: '</tbody></table>'
    },

這段代碼會產生如下的效果：

[![chart2-4-6](/images/learning_highcharts/chart2-4-6.jpg)](/images/learning_highcharts/chart2-4-6.jpg)

如之前的討論，我們使用月度新高和月度新低的series畫出了堆積柱狀圖，實際上堆積柱狀圖是用來繪製相同類別的數據。因此月度新高的series對應的提示框的部分是減去了我們之前塞入的值。為了修正這個提示框，我們可以使用回調函數來為月度新高series應用不同的屬性：

    shared: true,
    formatter: function() {
        return '<span style="color:#039;font-weight:bold">' +
        this.x + '</span><br/>' +
        this.points.map(function(point, idx) {
            return '<span style="color:' + point.series.color +
                    '">' + point.series.name +
                    '</span>: <span style="color:#669;fontweight:
                    bold">' +
                    Highcharts.numberFormat((idx == 0) ? point.total
                    : point.y) + '</span>';
        }).join('<br/>');
    }

point.total是一個和月度新低的差異總量，下圖顯示的修正後的月度新高的值：

[![chart2-4-7](/images/learning_highcharts/chart2-4-7.jpg)](/images/learning_highcharts/chart2-4-7.jpg)