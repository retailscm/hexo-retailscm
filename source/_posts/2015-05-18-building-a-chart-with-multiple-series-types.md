title: "第五章 05.繪製一個包含多個序列的圖表"
date: 2015-05-18 06:00:00
tags:
  - Highcharts
categories:
  - Learning
  - Learning Highcharts
  - Pie Charts
---

# 科技零售(RetailSCM.com) Learning Highcharts中文教程

_<span style="color: #808080;">版權保留，網路轉載請註明來源，謝絕紙質媒體轉載，謝謝。</span>_

## 繪製一個包含多個序列的圖表

到目前為止我們學習了線圖，柱圖以及餅圖，我們將看一下如何將這幾種不同的序列整合到同一個圖表中。在這一節中，我們使用2008年到2011年的年度資料來繪製三個不同類型的序列：線圖，柱圖及餅圖。柱圖顯示每種遊戲終端的遊戲每年的銷售數量，餅圖顯示每個遊戲終端的廠商的年銷售量，最後線圖則顯示每年一共有多少款遊戲發布。

為了確保每種圖形中的遊戲終端都使用相同的顏色，我們需要手動的給餅圖和柱圖的每一個數據節點指定顏色。

    var wiiColor = '#BBBBBB';
    var x360Color = '#89A54E';
    var ps3Color = '#4572A7';
    var splineColor = '#FF66CC';

再來我們想要把我們的圖表弄的更炫一些，首先我們給圖表加上一個漸進式的暗色背景：

<!--more-->

    var chart = new Highcharts.Chart({
            chart: {
                renderTo: 'container',
                borderWidth: 1,
                spacingTop: 40,
                backgroundColor: {
                    linearGradient: {
                        x1: 0,
                        y1: 0,
                        x2: 0,
                        y2: 1
                    },
                    stops: [
                        [0, '#0A0A0A'],
                        [1, '#303030']
                    ]
                }
            },

然後我們希望將柱圖向右邊移動一些，這樣我們可以有空間放置一張圖片（晚點再說這個圖片）在圖表的左上角。

    xAxis: {
        minPadding: 0.2,
        tickInterval: 1,
        labels: {
            formatter: function() {
                return this.value;
            },
            style: {
                color: '#CFCFCF'
            }
        }
    }

下一步是要在柱圖上面準備足夠的空間來防止我們的餅圖，這可以通過在所有的Y軸上都設定maxPadding選項來達到。

    yAxis: [{
        title: {
            text: 'Number of games sold',
            align: 'low',
            style: {
                color: '#CFCFCF'
            }
        },
        labels: {
            style: {
                color: '#CFCFCF'
            }
        },
        maxPadding: 0.5
    }, {
        title: {
            text: 'Number of games released',
            style: {
                color: splineColor
            }
        },
        labels: {
            style: {
                color: splineColor
            }
        },
        maxPadding: 0.5,
        opposite: true
    }],

每一個餅圖都獨立的顯示在柱圖之上，再通過設定餅圖的center選項，還可以調整餅圖的中心與柱圖對齊。我們也希望減小餅圖的尺寸以便於可以顯示其他更多的餅圖，所以我們可以給size選項設定一個百分比，百分比是餅圖的直徑所佔圖表的數值。

    series: [{
            type: 'pie',
            name: 'Hardware 2011',
            size: '25%',
            center: ['88%', '20%'],
            data: [{
                name: 'PS3',
                y: 14128407,
                color: ps3Color
            }, {
                name: 'X360',
                y: 13808365,
                color: x360Color
            }, {
                name: 'Wii',
                y: 11567105,
                color: wiiColor
            }],
            .....

線圖則對應到另一條Y軸，為了明確的表達線圖對應到第二條Y軸，我們為線圖，軸標題和標籤使用相同的顏色：

    {
        name: "Game released",
        type: 'spline',
        showInLegend: false,
        lineWidth: 3,
        yAxis: 1,
        color: splineColor,
        pointStart: 2008,
        pointInterval: 1,
        data: [1170, 2076, 1551, 1378]
    },

我們使用renderer.image方法在圖表中插入一個圖片，並使用ZIndex來確保圖片在最頂層，所以軸線並不會顯示在圖片之上。我這裡我們使用一個SVG來代替PNG類型的圖片，主要是SVG矢量圖可以隨圖表的尺寸變化而仍然保持它的清晰。

    chart.renderer.image('./pacman.svg', 0,
        0, 200, 200).attr({
        'zIndex': 10
    }).add();

下面是圖表的最終效果，我們加了一個吃豆人的圖片，使得圖表有了一個遊戲風格相呼應。

[![5-9](/images/learning_highcharts/5-9.png)](/images/learning_highcharts/5-9.png)

## 總結

在這一章中我們學習了如何來畫一個餅圖及它的延伸圖表甜甜圈圖，我們也在這一章中學習如何將我們目前已經學到的幾種圖合併畫在同一張圖表中。

在下一章中我們會學到Highcharts中新的序列，如儀錶盤，雷達圖及範圍圖，我們也會學會如何在Highcharts中學會使用輻射漸變。