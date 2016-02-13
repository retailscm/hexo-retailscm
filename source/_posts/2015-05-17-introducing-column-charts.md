title: "第四章 01.介紹柱形圖"
date: 2015-05-17 02:00:00
tags:
  - Highcharts
categories:
  - Learning
  - Learning Highcharts
  - Bar and column charts
---

# 科技零售(RetailSCM.com) Learning Highcharts中文教程

_<span style="color: #808080;">版權保留，網路轉載請註明來源，謝絕紙質媒體轉載，謝謝。</span>_

## 介紹柱形圖

柱形圖與長條圖的差異是極微小的，柱形圖的數據是垂直呈現而長條圖是水平呈現，兩者的數據都是沿著x軸來繪製並產生圖表。在本節中我們使用的數據由美國專利和商標局提供（U.S. Patent and Trademark Office）。下面的代碼是最近10年來英國的專利授權數以圖表來呈現的相關設定代碼：

    chart: {
        renderTo: 'container',
        type: 'column',
        borderWidth: 1
    },
    title: {
        text: 'Number of Patents Granted',
    },
    credits: {
        position: {
            align: 'left',
            x: 20
        },
        href: 'http://www.uspto.gov',
        text: 'Source: U.S. Patent & Trademark Office'
    },
    xAxis: {
        categories: [
            '2001', '2002', '2003', '2004', '2005',
            '2006', '2007', '2008', '2009', '2010',
            '2011' ]
    },
    yAxis: {
        title: {
            text: 'No. of Patents'
        }
    },
    plotOptions: {
    },
    series: [{
        name: 'UK',
        data: [ 4351, 4190, 4028, 3895, 3553,
                4323, 4029, 3834, 4009, 5038, 4924 ]
    }]

上面的代碼效果如下圖所示：

<!--more-->

[![chart4-1-1](/images/learning_highcharts/chart4-1-1.jpg)](/images/learning_highcharts/chart4-1-1.jpg)

我們再來增加一個series – 法國(France)，如下圖：

[![chart4-1-2](/images/learning_highcharts/chart4-1-2.jpg)](/images/learning_highcharts/chart4-1-2.jpg)

## 重疊的柱形圖

重疊柱形圖是另一個顯示多維度柱形圖的方式，主要原因是可以避免當series太多的時候，柱圖變的太細。柱形圖最難的部分就是當series太多時如何比較及顯示，而重疊柱形圖可以為category之間提供更多的空間，因而柱圖可以保留其基本寬度。

我們可以將兩個series的一部分進行重疊，代碼如下：

    plotOptions: {
        series: {
            pointPadding: -0.2,
            groupPadding: 0.3
        }
    },

默認的柱形圖或長條圖在不同series之間的寬度為0.2%，在本例中我們設定pointPadding為一個負數，表示我們希望原本柱圖(或長條圖)之間的間距改為相互重疊顯示。groupPadding是category之間的寬度設定，在本例中則表示為2005年和2006年之間的寬度，我們在上面將其設定為了0.3已確保柱圖之間不會自動變寬，因為重疊的設定已經為我們節省了更多的空間出來。下面是設定的效果：

[![chart4-1-3](/images/learning_highcharts/chart4-1-3.jpg)](/images/learning_highcharts/chart4-1-3.jpg)

## 堆積柱形圖及分組柱形圖

我們可以使用堆積柱形圖來代替柱圖直接並排對齊，雖然這樣可以讓我們比較難以看清每一個series的值，但我們可以很容易的看出每一個categroy的小計並且可以輕易看出series的比例。另一個強大的特性是當我們有超過兩個的series時，我們可以選擇我們需要產生堆積柱形圖的series。

讓我們來增加一個新圖表，包含UK, Germany, Japan, South Korea這四個國家。

[![chart4-1-4](/images/learning_highcharts/chart4-1-4.jpg)](/images/learning_highcharts/chart4-1-4.jpg)

可以看出日本的專利授權數遠高於其他的國家，所以讓我們再來產生一個複合的柱圖比對：歐洲VS亞洲，下面為代碼：

    plotOptions: {
        column: {
            stacking: 'normal'
        }
    },
    series: [{
        name: 'UK',
        data: [ 4351, 4190, 4028, .... ],
        stack: 'Europe'
    }, {
        name: 'Germany',
        data: [ 11894, 11957, 12140, ... ],
        stack: 'Europe'
    }, {
        name: 'S.Korea',
        data: [ 3763, 4009, 4132, ... ],
        stack: 'Asia'
    }, {
        name: 'Japan',
        data: [ 34890, 36339, 37248, ... ],
        stack: 'Asia'
    }]

我們聲明plotOptions中的stacking選項為normal，然後為每一個series設定一個堆棧分組名稱如Europe和Asia，設定會產生如下圖表：

[![chart4-1-5](/images/learning_highcharts/chart4-1-5.jpg)](/images/learning_highcharts/chart4-1-5.jpg)

我們可以看到圖表中由4個柱圖減少為2個，並且每個柱圖中都合併了2個series，第一個柱圖是歐洲組，第二個柱圖是亞洲組。

## 同時顯示堆積柱形圖與單柱圖

在上一節中我們大家都認為堆積柱形圖與分組柱形圖在數據呈現面是有它的優勢的，當有時單獨的一個series自成分組並與多個series形成的分組來組成一個category時也具有相同的優勢。Highcharts提供我們這種可能性來混合由多個series分組的堆積柱形圖與單個series的柱圖。

讓我們來看一個將多個柱圖分組的堆積圖和單獨柱圖混合的例子。首先移除每一個series的堆積分組指定，這意味著所有的series會形成一個堆積在一起的柱圖。然後我們加入一個新的series – 美國，然後手動聲明stacking選項為null來覆蓋掉plotOptions中的默認的全局設定：

    plotOptions: {
        column: {
            stacking: 'normal'
        }
    },
    series: [{
        name: 'UK',
        data: [ 4351, 4190, 4028, .... ]
    }, {
        name: 'Germany',
        data: [ 11894, 11957, 12140, ... ]
    }, {
        name: 'S.Korea',
        data: [ 3763, 4009, 4132, ... ]
    }, {
        name: 'Japan',
        data: [ 34890, 36339, 37248, ... ]
    }, {
        name: 'US',
        data: [ 98655, 97125, 98590, ... ],
        stacking: null
    }]

新的series組合效果如下圖：

[![chart4-1-6](/images/learning_highcharts/chart4-1-6.jpg)](/images/learning_highcharts/chart4-1-6.jpg)

前四個series – 英國，德國，韓國和日本彼此堆積在一起成為一個獨立的堆積柱圖，美國則獨立為一根柱圖。我們可以容易的觀察到四個國家加起來的專利數量還不到美國的三分之二（美國幾乎是英國專利數量的25倍）

## 百分數版本的堆積柱形圖

另一種方式是我們可以看到用每個國家的百分佔比來組成堆積柱形圖。我們手動移除美國series的stacking設定之後，再設定柱形圖的全局變量stacking為percent則可達成此效果：

    plotOptions: {
        column: {
            stacking: 'percent'
        }
    }

全部的series都堆積為一個單獨的柱圖，並且他們的值都被轉為百分比，如下圖所示：

[![chart4-1-7](/images/learning_highcharts/chart4-1-7.jpg)](/images/learning_highcharts/chart4-1-7.jpg)

## 調整柱形圖的顏色及標籤

讓我們來新建一個柱形圖，這次我們將專利授權數前十名的國家都加入其中，代碼如下：

    chart: {
        renderTo: 'container',
        type: 'column',
        borderWidth: 1
    },
    title: {
        text: 'Number of Patents Filed in 2011'
    },
    credits: { ... },
    xAxis: {
        categories: [
            'United States', 'Japan',
            'South Korea', 'Germany', 'Taiwan',
            'Canada', 'France', 'United Kingdom',
            'China', 'Italy' ]
    },
    yAxis: {
        title: {
            text: 'No. of Patents'
        }
    },
    series: [{
        showInLegend: false,
        data: [ 121261, 48256, 13239, 12968, 9907,
                5754, 5022, 4924, 3786, 2333 ]
    }]

代碼中的效果如下圖所示：

[![chart4-1-8](/images/learning_highcharts/chart4-1-8.jpg)](/images/learning_highcharts/chart4-1-8.jpg)

在這張圖中我們希望改變幾個部分：

首先部分國家名被折行，為了避免這種情況，我們可以在x軸標籤上使用rotation選項，如下：

    xAxis: {
        categories: [
            'United States', 'Japan',
            'South Korea', ... ],
        labels: {
            rotation: -45,
            align: 'right'
        }
    },

其次，最大值按照美國的規模來與其他國家進行比較，所以我們不能真的這樣去定義它們的值，為了解決這個問題，我們可以在y軸上定義刻度為logarithmic，如下：

    yAxis: {
        title: ... ,
        type: 'logarithmic'
    },

最後，我們希望可以演著柱圖將各國的數值都顯示在上面，並對每個國家的柱圖使用不同的顏色來美化，如下：

    plotOptions: {
        column: {
            colorByPoint: true,
            dataLabels: {
                enabled: true,
                rotation: -90,
                y: 25,
                color: '#F4F4F4',
                formatter: function() {
                    return
                    Highcharts.numberFormat(this.y, 0);
                },
                x: 10,
                style: {
                    fontWeight: 'bold'
                }
            }
        }
    },

下圖是經過了我們的優化後的圖表：

[![chart4-1-9](/images/learning_highcharts/chart4-1-9.jpg)](/images/learning_highcharts/chart4-1-9.jpg)