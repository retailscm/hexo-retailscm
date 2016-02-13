title: "第三章 01.折線圖介紹"
date: 2015-05-16 03:00:00
tags:
  - Highcharts
categories:
  - Learning
  - Learning Highcharts
  - Line area and scatter charts
---

# 科技零售(RetailSCM.com) Learning Highcharts中文教程

_<span style="color: #808080;">版權保留，網路轉載請註明來源，謝絕紙質媒體轉載，謝謝。</span>_

## 折線圖介紹

首先我們從一個折線圖表開始，來選用World Bank 組織（www.worldbank.org)提供的數據之一。下面的代碼片段創建了一個簡單的折線圖，表現了日本過去三十年中65歲以上人口所佔全部人口的百分比：

    var chart = new Highcharts.Chart({
        chart: {
            renderTo: 'container'
        },
        title: {
            text: 'Population ages 65 and over (% of total)',
        },
        credits: {
            position: {
                align: 'left',
                x: 20
            },
            text: 'Data from The World Bank'
        },
        yAxis: {
            title: {
                text: 'Percentage %'
            }
        },
        xAxis: {
            categories: ['1980', '1981', '1982', ... ],
            labels: {
                step: 5
            }
        },
        series: [{
            name: 'Japan - 65 and over',
            data: [ 9, 9, 9, 10, 10, 10, 10 ... ]
        }]
    });

如下圖所呈現：

<!--more-->

[![chart3-1-1](/images/learning_highcharts/chart3-1-1.jpg)](/images/learning_highcharts/chart3-1-1.jpg)

除了在 categories屬性中手動定義年份，還可以使用 series中的 pointStart選項來初始化x軸的第一個點的值。因此我們在 xAxis中無任何配置但在seies中定義了 pointStart，代碼如下：

    xAxis：{
    },
    series: [{
        pointStart : 1980, 
        name: 'Japan - 65 and over',
        data: [ 9, 9, 9, 10, 10, 10, 10 ... ]
    }]

雖然這是個簡單的例子，但 Highcharts仍會使用 numberFormat方法來格式化x軸的標籤增加千分位符號，下面是x軸千分位符號的呈現結果：

[![chart3-1-2](/images/learning_highcharts/chart3-1-2.jpg)](/images/learning_highcharts/chart3-1-2.jpg)

為瞭解決這個問題，我們需要重寫標籤的 formatter選項，這很簡單，只要返回一個標籤值來繞開默認 numberFormat方法的調用就可以了。 此外，還需要將 allowDecimal選項置為false.因為，當調整圖表大小時會延長x軸，小數就會顯示出來。

下面是使用 pointStart來控制x軸年份值得最終版本：

    xAxis: {
        labels:{
            formatter: function() {
                // 'this' keyword is the label object
                return this.value;
            }
        },
        allowDecimals: false
    },
    series: [{
        pointStart: 1980,
        name: 'Japan - 65 and over',
        data: [ 9, 9, 9, 10, 10, 10, 10 ... ]
    }]

## 擴展到多個series的折線圖

下面我們新增加幾個series折線，並把日本的折線增加到6個像素寬，代碼如下：

    series: [{
        lineWidth: 6,
        name: 'Japan',
        data: [ 9, 9, 9, 10, 10, 10, 10 ... ]
    }, {
        Name: 'Singapore',
        data: [ 5, 5, 5, 5, ... ]
    }, {
        ...
    }]

日本series的人口數成為了圖表中的關注焦點，如圖：

[![chart3-1-3](/images/learning_highcharts/chart3-1-3.jpg)](/images/learning_highcharts/chart3-1-3.jpg)

我們來繼續更加複雜的一些的折線圖。為了展示反轉的折線圖，使用 chart.inverted 選項將y軸和x軸置為相反的方向。 然後改變坐標軸的顏色匹配相同series的顏色。同時，禁用了所有series的數據點標記，最後添加 yAxis：1 屬性用來使第二個series坐標軸與第二個y軸陣列數據匹配，這樣就得到了上下兩個不同顏色，不同刻度的y坐標軸，如下：

    chart: {
        renderTo: 'container',
        inverted: true,
    },
    yAxis: [{
        title: {
            text: 'Percentage %'
        },
        lineWidth: 2,
        lineColor: '#4572A7'
    }, {
        title: {
            text: 'Age'
        },
        opposite: true,
        lineWidth: 2,
        lineColor: '#AA4643'
    }],
    plotOptions: {
        series: {
            marker: {
                enabled: false
            }
        }
    },
    series: [{
        name: 'Japan - 65 and over',
        type: 'spline',
        data: [ 9, 9, 9, ... ]
    }, {
        name: 'Japan - Life Expectancy',
        yAxis: 1,
        data: [ 76, 76, 77, ... ]
    }]

下圖是反轉並有2個Y軸的例子：

[![chart3-1-4](/images/learning_highcharts/chart3-1-4.jpg)](/images/learning_highcharts/chart3-1-4.jpg)

上圖中數據的表示看起來可能有點奇怪，因為我們通常對於把的時間標籤交換成到了y軸並且數據的走勢不容易理解。其實 inverted 選項 一般是用來展示不連續的數據格式或用於柱狀圖中。我們從上圖中獲取的資訊是這樣的： 12%的日本人口在65歲及以上，1990年的平均壽命是79歲。

通過設置 plotOptions.series.marker.enabled 為 false 可以關閉所有數據點的標記，如果想為一個特定的series展示數據點標記，我們可以關閉全域的標記並打開那個特定series的標記屬性。

    plotOptions: {
        series: {
            marker: {
                enabled: false
            }
        }
    },
    series: [{
        marker: {
            enabled: true
        },
        name: 'Japan - 65 and over',
        type: 'spline',
        data: [ 9, 9, 9, ... ]
    }, {

下面的圖表展示了只有65及以上的series才會有數據點標記的例子：

[![chart3-1-5](/images/learning_highcharts/chart3-1-5.jpg)](/images/learning_highcharts/chart3-1-5.jpg)