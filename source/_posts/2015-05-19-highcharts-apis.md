title: "第七章 Highcharts APIs"
date: 2015-05-19 01:00:00
tags:
  - Highcharts
categories:
  - Learning
  - Learning Highcharts
  - Highcharts APIs
---

# 科技零售(RetailSCM.com) Learning Highcharts中文教程

_<span style="color: #808080;">版權保留，網路轉載請註明來源，謝絕紙質媒體轉載，謝謝。</span>_

## 第七章 Highcharts APIs

Highcharts為了繪製動態交互的圖表提供了一組API供我們使用。為了理解這些API是如何運作的，我們需要先了解圖表內的對象以及他們是如何組織在一個圖表中的。在這一章中，我們會學習Highcharts的類以及如何通過相應的對象來呼叫API。然後我們通過PHP，jQuery及 jQuery UI建立一個簡單的股票價格應用來顯示如何使用Highcharts APIs。完成這些之後，我們將焦點移往更新序列的四種不同的方式，我們會嘗試所有的更新序列方法，並建立應用程序來觀察他們之間的視覺效果和CPU性能的變化。最後我們會觀察主流的瀏覽器上不同大小數據集對應更新序列所帶來的效能影響。

*   了解Highcharts類模型
*   使用Chart.addSeries通過Ajax獲得資料並呈現為一個新的序列
*   同時呼叫多個Ajax並顯示為多個序列
*   使用Chart.getSVG將SVG數據轉換為一個圖片
*   使用Chart.renderer方法
*   探索不同方法更新序列時的效能影響
*   探索海量資料的執行效能

[了解Highcharts類模型](/Learning/Learning-Highcharts/Highcharts-APIs/understanding-the-highcharts-class-model/ "01.了解Highcharts類模型")

[使用Highcharts APIs](/Learning/Learning-Highcharts/Highcharts-APIs/using-highcharts-apis/ "02.使用Highcharts APIs")