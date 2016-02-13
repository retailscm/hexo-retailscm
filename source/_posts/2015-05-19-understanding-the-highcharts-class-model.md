title: "第七章 01.了解Highcharts類模型"
date: 2015-05-19 02:00:00
tags:
  - Highcharts
categories:
  - Learning
  - Learning Highcharts
  - Highcharts APIs
---

# 科技零售(RetailSCM.com) Learning Highcharts中文教程

_<span style="color: #808080;">版權保留，網路轉載請註明來源，謝絕紙質媒體轉載，謝謝。</span>_

## 了解Highcharts類模型

Highcharts類和類之間的關係非常的簡單和明顯，一個圖表由5個不同的類所組成： Chart, Axis, Series, Point和Render。有一些類會包含一組由底層組件及一組對象屬性來反向引用到一個更高層的組件所組成，例如：Point class具有series屬性，可指回到一個Series的類。每個類也都有一組方法來管理和呈現他自己的層。下面的類圖即描述了這些類之間的關係。

[![7-1](/images/learning_highcharts/7-1.jpg)](/images/learning_highcharts/7-1.jpg)

<!--more-->

Chart是整個圖表對象對頂層的類，它包含調用方法來操作圖表，例如匯出圖表為SVG或其他的圖片格式及設定圖表的維度等。Chart類有多個軸線及序列的數組，也就是說一個圖表可以有一個或多個的x軸，y軸及序列。Renderer類是工具類，用來提供每個圖表一對一的關係及匯出SVG和VML的通用接口。

Series類包含一個Point對象的數組，這個類也有反向引用到Chart和Axis對象的屬性（參考上圖中的虛線連接線），並提供方法來管理Point對象數組的方法。yAxis和xAxis屬性在Series類中不是必要的屬性，當然一個圖表也可以有多個軸線。

Point類是一個簡單的對象，它包含X和Y兩種值及一個反向引用到它的series對象(參考虛線部分)，APIs用來管理圖表中的數據點。

## Highcharts構造函數 – Highcharts.Chart

無需多說也知道最重要的方法就是Highcharts.Chart，也是到目前位置我們看到最多次的方法。然而還有更多呼叫構造函數的方法。Highcharts.Chart創建和返回一個圖表對象，它還包含一個可選的參數回調函數。

    Chart(Object options, [ Function callback ])

當圖表創建及渲染之後，系統會呼叫回調函數。在函數內我們可以呼叫組件的方法也可以通過屬性來訪問圖表的對象。新創建出來的圖表對象只能通過使用回調函數取得圖表對象。我們也可以在回調函數中使用'this'關鍵字來關聯到圖表對象。我們也可chart.events.load handler中定義我們的代碼來達到與Highcharts.Chart相同的效果，這會在下一章中討論。

一般使用來說，我們可以在呼叫Highcharts.Chart之後就能夠訪問圖表對象了，嚴格的說，所有定義在回調函數中的代碼都應該符合以下兩個原因來關聯圖表對象：

* 有一個IE的問題是說$.ready方法在某些情況下會在腳本載入完成之前被調用（這是jQuery 1.8.0的一個Bug，在1.8.1中已經被修正）。在回調函數裡面放的代碼可以避免發生問題。
* 如果我們希望圖表上的JavaScript代碼需要並發，這會帶來一些好處。例如使用新的HTML5 Worker對象將比較繁重的工作分配給多個不同的線程來執行。

## Highcharts組件導覽

為了使用Highcharts API，我們必須在類層級中找到正確的對象，這裡有幾種方式去瀏覽圖表對象——使用圖表的類目層級模型，使用Chart.get直接檢索或兩者混合。

## 使用類層級

假設圖表對象已經如下的方式建立好了

    <script type="text/javascript">

    $(document).ready(function() {
        var chart = new Highcharts.Chart({
                ...
            xAxis: [{
                    ....
            }, {
                    ....
            }],
            series: [{
                data: [...]
            }, {
                data: [...]
            }],
            ...
        });
    }, function() {
            ...
    });

    </script>

我們可以從圖表中的回調處理程序獲得第一個series對象，如下：

    var series = this.series[0];

假設這個設定中有兩個x軸，要取得第二個x軸，我們可以如下所做：

    var xAxis = this.xAxis[1];

要從圖表的第二個series中取得第三個數據節點，代碼如下：

    var point = this.series[1].data[2];


## 使用Chart.get方法

不同於對象層級的級聯方式，我們可以通過Chart.get方法直接檢索組件（get方法只有在chart級才可用，不是所有的組件類都可以使用），為了做到這樣，組件在配置時必須定義id選項。

假設我們已經按照如下代碼配置了一個圖表：

    xAxis: {
        id: 'xAxis',
        categories: [...]
    },
    series: [{
        name: 'UK',
        id: 'uk',
        data: [4351, 4190, {
                y: 4028,
                id: 'thirdPoint'
            },
            ...
        ]
    }]

我們可以使用下面的方式直接檢索到組件：

    var series = this.get('uk');
    var point = this.get('thirdPoint');
    var xAxis = this.get('xAxis');


## 使用對象層級和Chart.get的混合模式

為圖表中所有的組件都定義id是非常繁複的，我們可以使用替代方案，使用兩種方法綜合來找到對應的組件，代碼如下：

    var point = this.get('uk').data[2];