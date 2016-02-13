title: "第二章 07.動態圖表"
date: 2015-05-14 08:40:33
update: 2015-05-14 08:40:33
tags:
  - Highcharts
categories:
  - Learning
  - Learning Highcharts
  - Highcharts configurations
---

# 科技零售(RetailSCM.com) Learning Highcharts中文教程

_<span style="color: #808080;">版權保留，網路轉載請註明來源，謝絕紙質媒體轉載，謝謝。</span>_

## 動態圖表

動態圖表在Highcharts中有兩種動畫類型 – 初始化和更新動畫。初始化動畫發生在當series數據已經準備好並且圖表也已經顯示出來。更新動畫是在初始化之後，當series數據或圖表的任何部分被改變。

初始化動畫設定可以透過指定plotOptions.series.animation或plotOptions.{series-type}.animation的參數來配置，而更新動畫是透過設定chart.animation屬性。

所有的Highcharts動畫都是使用jQuery來實現的，animation屬性可以是一個布爾值也可以是一組動畫選項。Highcharts使用jQuery的swing動畫，下面是對應的選項：

*   duration：完成動畫的時間，毫秒為單位
*   easing：jQuery提供的動畫類型，各種動畫都可以透過導入jQuery UI plugin來擴展，下面可以找到一些不錯的資源：[http://plugindetector.com/demo/easing-jquery-ui/](http://plugindetector.com/demo/easing-jquery-ui/)

<!--more-->

這裡我們繼續前一節中的例子，我們會使用一些動畫設定到plotOptions.column 和plotOptions.line上，如下：

    plotOptions: {
        column: {
            ... ,
            animation: {
                duration: 2000,
                easing: 'swing'
            }
        },
        line: {
            .... ,
            animation: {
                duration: 3000,
                easing: 'linear'
            }
        }
    },

動畫的效果被調整到很慢，所以我們可以注意到linear和swing兩種動畫效果之間的差異。折線圖沿著x軸以線性的速度出現，而柱狀圖以線性的速度向上擴展，到快接近頂端時速度會劇減。下面的圖為正在進行中的動畫：

[![chart2-7-1](/images/learning_highcharts/chart2-7-1.jpg)](/images/learning_highcharts/chart2-7-1.jpg)