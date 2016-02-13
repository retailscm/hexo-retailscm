title: "第二章 01.配置選項"
date: 2015-05-13 19:46:33
update: 2015-05-13 19:46:33
tags:
  - Highcharts
categories:
  - Learning
  - Learning Highcharts
  - Highcharts configurations
---

# 科技零售(RetailSCM.com) Learning Highcharts中文教程

_<span style="color: #808080;">版權保留，網路轉載請註明來源，謝絕紙質媒體轉載，謝謝。</span>_

## 配置選項

在Highcharts配置對象中，組件在最前端相互配合作為呈現整個圖表的基礎。下面的列表是在整合Highcharts圖表中起主要作用的組件，如果需要參考全部配置的說明，可至Highcharts官網API說明 

[http://api.highcharts.com/highcharts](http://api.highcharts.com/highcharts)

下面是主要的組件列表：

*   chart: 配置圖表的最主要屬性，如佈局，尺寸大小，事件，動畫，用戶交互
*   series: 單個和多個series的數組對象（包含數據和配置設定），其中series的數據可以用多種方式來指定
*   xAxis/yAxis: 配置軸全部的屬性，如標籤，樣式，間隔，標示線，區域顏色帶和背景
*   tooltip: 數據結點上提示框的佈局與樣式設定
*   title/subtitle: 圖表中標題與副標題的佈局與樣式設定
*   legend: 圖表中圖例說明的佈局與樣式設定
*   plotOptions: 包含所有的繪圖選項，如顯示，動畫，一般series與指定series的用戶交互
*   exporting: 打印與導出的佈局及功能配置