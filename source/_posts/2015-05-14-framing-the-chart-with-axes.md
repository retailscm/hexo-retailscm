title: "第二章 03.軸在圖表中的應用"
date: 2015-05-14 01:00:00
tags:
  - Highcharts
categories:
  - Learning
  - Learning Highcharts
  - Highcharts configurations
---

# 科技零售(RetailSCM.com) Learning Highcharts中文教程

_<span style="color: #808080;">版權保留，網路轉載請註明來源，謝絕紙質媒體轉載，謝謝。</span>_

## 軸在圖表中的應用

在本節中，我們會來看一下軸的設定，會從一個普通的折線圖開始，然後逐漸增加一些選項到圖表上，再來驗證其效果。

## 訪問軸的數據類型

有兩種方法為圖表指定數據 - categories數組和series數據。對顯示特定名稱的間隔，我們應該用一個字串數組賦值給對應的categories，每一個categories都關聯一個series數據數組中的一個值。另一種方式是在categories中都嵌入series數據數組的值。之後Highcharts會取出每一個軸上的series數據，並解釋數據類型，適當的對數據進行格式化。

<!--more-->

下面是一個使用categories的簡單例子：

    chart: {
      renderTo: 'container',
      height: 250,
      spacingRight: 20
    },
    title: {
      text: 'Market Data: Nasdaq 100'
    },
    subtitle: {
      text: 'May 11, 2012'
    },
    xAxis: {
      categories: [ 
        '9:30 am', '10:00 am', '10:30 am',
        '11:00 am', '11:30 am', '12:00 pm',
        '12:30 pm', '1:00 pm', '1:30 pm',
        '2:00 pm', '2:30 pm', '3:00 pm',
        '3:30 pm', '4:00 pm' ],
      labels: {
        step: 3
      }
    },
    yAxis: {
      title: {
        text: null
      }
    },
    legend: {
      enabled: false
    },
    credits: {
      enabled: false
    },
    series: [{
      name: 'Nasdaq',
      data: [ 
        2606.01, 2622.08, 2636.03, 2637.78, 2639.15,
        2637.09, 2633.38, 2632.23, 2632.33, 2632.59,
        2630.34, 2626.89, 2624.59, 2615.98 ]
    }]

上面的代碼片段會產生如下圖所示的圖表：

[![chart2-3-1](/images/learning_highcharts/chart2-3-1.jpg)](/images/learning_highcharts/chart2-3-1.jpg)

Categories數組中的第一個時間9:30am對應到series數據數組中第一個值2606.01，整個圖表就是以此類推。

另外，我們可以在series數據中指定時間的值，並在x軸上設定type屬性來格式化時間。Type屬性支持linear(默認), logarithmic或datetime。datetime的設定會自動識別series數據中的時間並轉化為特定的時間格式。此外，我們可以使用dateTimeLabelFormats屬性來預定義日期的格式，參數的部分可以允許不同時間維度的格式，這樣當我們未來改變報表中時間維度的查詢條件時，系統可以自動的去匹配合適的時間維度，如每天，每月，每年等。下面的例子會展示圖表如何預定義小時和分鐘的格式（時間格式化語法部分請參考PHP的strftime函數）。

    xAxis: {
        type: 'datetime',
        // Format 24 hour time to AM/PM
        dateTimeLabelFormats: {
            hour: '%I:%M %P',
            minute: '%I %M'
        }
    },
    series: [{
        name: 'Nasdaq',
        data: [ 
                [ Date.UTC(2012, 4, 11, 9, 30), 2606.01 ],
                [ Date.UTC(2012, 4, 11, 10), 2622.08 ],
                [ Date.UTC(2012, 4, 11, 10, 30), 2636.03 ],
                .....
              ]
    }]

注意x軸上12小時制的時間格式：

[![chart2-3-2](/images/learning_highcharts/chart2-3-2.jpg)](/images/learning_highcharts/chart2-3-2.jpg)

作為替代方案，我們也可以定義xAxis.labels.formatter屬性的格式化字串來達到類似的效果。Highcharts提供一個常用工具Highcharts.dateFormat，可以將timestamp格式的日期轉換為可讀的特定日期格式。在下面的代碼片段中，我們使用dateFormat及this.value定義了formatter功能，關鍵詞this是軸上interval的對象，而this.value是軸上interval的實例（UTC日期格式）

    xAxis: {
        type: 'datetime',
        labels: {
            formatter: function() {
                return Highcharts.dateFormat('%I:%M %P', this.value);
            }
        }
    },

我們也可以定義一個開始時間pointStart及一個等距間隔點pointInterval，則我們就可以自我們定義好的開始時間和間距來產生一個精簡版的圖表。

    series: [{
        name: 'Nasdaq',
        pointStart: Date.UTC(2012, 4, 11, 9, 30),
        pointInterval: 30 * 60 * 1000,
        data: [ 2606.01, 2622.08, 2636.03, 2637.78,
                2639.15, 2637.09, 2633.38, 2632.23,
                2632.33, 2632.59, 2630.34, 2626.89,
                2624.59, 2615.98 ]
    }]

## 調整interval和背景

在上一節中我們了解到了如何使用categories和series數據數組，在本節中我們會了解如何格式化間隔線(interval lines)及背景樣式來使得產出的圖表更清晰易懂。

我們先繼續上一節的例子，首先我們會沿著y軸建立間隔線，在圖表中間隔會被自動設置為20，然而如果將間隔線的密度增加一倍會讓圖表更容易讀懂，為了做到這點，我們可以將tickInterval設定為0，然後我們使用minorTickInterval來將一條半間隔線至於兩條間隔線中間。為了區別新增的半間隔線，我們設置minorGridLineDashStyle來將新增的間隔線設置為虛線或點狀線的樣式。

依據第一步來創建一個新設定：

    yAxis: {
        title: {
            text: null
        },
        tickInterval: 10,
        minorTickInterval: 5,
        minorGridLineColor: '#ADADAD',
        minorGridLineDashStyle: 'dashdot'
    }

間隔線看起來會如下圖所示：

[![chart2-3-3](/images/learning_highcharts/chart2-3-3.jpg)](/images/learning_highcharts/chart2-3-3.jpg)

為了讓圖表看起來更正式，我們再使用alternateGridColor增加一些明暗間隔行的效果，並使用gridLineColor來修改間隔線的顏色。將如下代碼增加到yAxis配置中：

    gridLineColor: '#8AB8E6',
    alternateGridColor: {
        linearGradient: {
            x1: 0, y1: 1,
            x2: 1, y2: 1
        },
        stops: [[0, '#FAFCFF' ],
                [0.5, '#F5FAFF'] ,
                [0.8, '#E0F0FF'] ,
                [1, '#D6EBFF']]
    }

我們在下一章中會再討論顏色漸變的部分，下圖使用了新的背景：

[![chart2-3-4](/images/learning_highcharts/chart2-3-4.jpg)](/images/learning_highcharts/chart2-3-4.jpg)

下一步來增加更多專業的效果到y軸的軸線上。我們先使用lineWidth屬性畫一條線在y軸上，並沿著間隔線增加一些刻度，代碼如下：

    lineWidth: 2,
    lineColor: '#92A8CD',
    tickWidth: 3,
    tickLength: 6,
    tickColor: '#92A8CD',
    minorTickLength: 3,
    minorTickWidth: 1,
    minorTickColor: '#D8D8D8'

tickWidth和tickLength在每個分割線的左端增加了一些標記，並在在軸線上使用了與分割線相同的顏色，然後增加了minorTickLength和minorTickWidth – 在半分割線上使用比較小的尺寸。這些設定沿著軸線產生了比較漂亮的效果，如下圖：

[![chart2-3-5](/images/learning_highcharts/chart2-3-5.jpg)](/images/learning_highcharts/chart2-3-5.jpg)

現在我們再來簡單的裝飾一下x軸的配置，如下：

    xAxis: {
        type: 'datetime',
        labels: {
            formatter: function() {
                return Highcharts.dateFormat('%I:%M %P', this.value);
            },
        },
        gridLineDashStyle: 'dot',
        gridLineWidth: 1,
        tickInterval: 60 * 60 * 1000,
        lineWidth: 2,
        lineColor: '#92A8CD',
        tickWidth: 3,
        tickLength: 6,
        tickColor: '#92A8CD',
    },

我們使用小時來設定了x軸的分割線並設置其為點狀線，然後我們再沿用y軸的設定來對x軸間隔線設定相同的顏色，粗細，效果如下圖：

[![chart2-3-6](/images/learning_highcharts/chart2-3-6.jpg)](/images/learning_highcharts/chart2-3-6.jpg)

但是，這樣在x軸線上有一些缺陷，首先，x和y軸交界點沒有對齊；其次，x軸的間隔標記點距離間隔標籤太近，已經重疊；最後，第一個數據點已經與y軸有部分重疊了。

下面這張圖特別突出了有問題的這些部分：

[![chart2-3-7](/images/learning_highcharts/chart2-3-7.jpg)](/images/learning_highcharts/chart2-3-7.jpg)

有兩種方法可以解決軸線對齊的問題：

*   將圖表向上移動一個像素，可以將xAxis的offset屬性設置為1來實現。
*   增加x軸線的寬度到3個像素，已保持與y軸間隔標記點相同的寬度。

對於x軸標籤，我們可以用之前介紹的y定位位移來簡單的解決。

最後，為了避免第一個數據點碰到y軸，我們可以在x軸上使用minPadding來解決，minPadding會增加按照設定的最小值來給第一個數據點增加一個填充空間。minPadding是使用圖表寬度的比率來設定，按照本例來說設置屬性為0.02等於沿著x軸向右移動了5個像素的距離(250px * 0.02)，下面是讓圖表變得更整潔的調整代碼：

    xAxis: {
        ....
        labels: {
            formatter: ...,
            y: 17
        },
        .....
        minPadding: 0.02,
        offset: 1
    }

下圖可以看到之前的問題都已經被解決了：

[![chart2-3-8](/images/learning_highcharts/chart2-3-8.jpg)](/images/learning_highcharts/chart2-3-8.jpg)

如我們所見，Highcharts有著一整組靈活變化又可配置的設定。

## 使用標示線plot lines和區域色帶(plot bands)

在本節中我們會看到我們可以沿著軸線放置任何的線或色帶(bands)，我們繼續使用上一節中的例子。讓我們在y軸上來畫一組當日數據點的最高及最低標示線。每一個標示線的plotLines參數都允許一個設定的數組對象，但寬度和顏色沒有默認設定，所以為了可以看到標示線，我們需要特別來指定它們，下面是標示線的代碼部分：

    yAxis: {
        ... ,
        plotLines: [{
            value: 2606.01,
            width: 2,
            color: '#821740',
            label: {
                text: 'Lowest: 2606.01',
                style: {
                    color: '#898989'
                }
            }
        }, {
            value: 2639.15,
            width: 2,
            color: '#4A9338',
            label: {
                text: 'Highest: 2639.15',
                style: {
                    color: '#898989'
                }
            }
        }]
    }

下圖是圖表看起來的樣子：

[![chart2-3-9](/images/learning_highcharts/chart2-3-9.jpg)](/images/learning_highcharts/chart2-3-9.jpg)

我們可以略微改善一下圖表的美觀度。首先，上限標示線的文字說明應該在最高點的邊上；其次，折線和間隔線不應該遮住下限標示線的文字說明，如下圖：

[![chart2-3-10](/images/learning_highcharts/chart2-3-10.jpg)](/images/learning_highcharts/chart2-3-10.jpg)

為了解決這些問題，我們可以修改標示線的zIndex為1，這樣可以確保文字說明會在標示線之上，而不會被遮蓋。我們也可以修改文字說明的x定位已移動到極點的後面，下面是這些對應的改變：

    plotLines: [{
        ... ,
        label: {
            ... ,
            x: 25
        },
        zIndex: 1
        }, {
        ... ,
        label: {
            ... ,
            x: 130
        },
        zIndex: 1
    }]

此時說明文字已經被移動到極點之後，並且也現實在間隔線的上層，避免被遮住，如下圖：

[![chart2-3-11](/images/learning_highcharts/chart2-3-11.jpg)](/images/learning_highcharts/chart2-3-11.jpg)

現在我們再來改變一下上一個例子，我們使用區域色帶來顯示出股市的開盤和收市的點數，區域色帶的配置與標示線很相似，除了區域色帶使用to和form屬性及顏色可以選擇使用漸變或純色。我們創建一個帶三角符號及相關上升數值的區域色帶，代替使用x和y來調整標籤的位置，我們使用align選項來將標籤居中顯示。

    plotBands: [{
        from: 2606.01,
        to: 2615.98,
        label: {
                text: '▲ 9.97 (0.38%)',
                align: 'center',
            style: {
                color: '#007A3D'
            }
        },
        zIndex: 1,
        color: {
            linearGradient: {
                x1: 0, y1: 1,
                x2: 1, y2: 1
            },
            stops: [[0, '#EBFAEB' ],
                    [0.5, '#C2F0C2'] ,
                    [0.8, '#ADEBAD'] ,
                    [1, '#99E699']]
        }
    }]

上面的代碼產生了一個綠色的色帶，表示股市是以上漲收市，如下圖：

[![chart2-3-12](/images/learning_highcharts/chart2-3-12.jpg)](/images/learning_highcharts/chart2-3-12.jpg)

## 擴展到多軸

前面我們已經看過了大多關於軸的設定，在這裡我們會再看一下如何使用一個包含軸設定的數組來實現多軸。

我們繼續使用前面的股市的例子，假設我們現在希望在圖表中納斯達克指數之後再增加一個道瓊斯指數，但是他們的本質不同因而他們的股指範圍也有很大的差異。

首先我們來解釋一下這兩個指數公用y軸的結果，我們會修改y軸的標題，移除y軸上的固定間隔設定，再引入另外一組series數據：

    chart: ... ,
    title: {
        text: 'Market Data: Nasdaq & Dow Jones'
    },
    subtitle: ... ,
    xAxis: ... ,
    credits: ... ,
    yAxis: {
        title: {
            text: null
        },
    minorGridLineColor: '#D8D8D8',
    minorGridLineDashStyle: 'dashdot',
    gridLineColor: '#8AB8E6',
    alternateGridColor: {
        linearGradient: {
            x1: 0, y1: 1,
            x2: 1, y2: 1
        },
        stops: [ [0, '#FAFCFF' ],
                [0.5, '#F5FAFF'] ,
                [0.8, '#E0F0FF'] ,
                [1, '#D6EBFF'] ]
        },
        lineWidth: 2,
        lineColor: '#92A8CD',
        tickWidth: 3,
        tickLength: 6,
        tickColor: '#92A8CD',
        minorTickLength: 3,
        minorTickWidth: 1,
        minorTickColor: '#D8D8D8',
    },
    series: [{
        name: 'Nasdaq',
        data: [ [ Date.UTC(2012, 4, 11, 9, 30), 2606.01 ],
                [ Date.UTC(2012, 4, 11, 10), 2622.08 ],
                [ Date.UTC(2012, 4, 11, 10, 30), 2636.03 ],
                ...
                ]
    }, {
    name: 'Dow Jones',
    data: [ [ Date.UTC(2012, 4, 11, 9, 30), 12598.32 ],
            [ Date.UTC(2012, 4, 11, 10), 12538.61 ],
            [ Date.UTC(2012, 4, 11, 10, 30), 12549.89 ],
            ...
            ]
    }]

下面為兩種市場指數的圖表

[![chart2-3-13](/images/learning_highcharts/chart2-3-13.jpg)](/images/learning_highcharts/chart2-3-13.jpg)

如同之前的猜測，兩個指數具有非常大的差異，造成兩條線幾乎是平的，這會造成一種誤解：股指並沒有什麼變化。

讓我們來看一下如果將兩個不同的股指放在不同的y軸上會怎麼樣。因為我們需要將不同股指範圍的數據顯示在同一個背景中，因此我們需移除y軸上所有的裝飾性的設置。

下面是y軸的設定：

    yAxis: [{
        title: {
            text: 'Nasdaq'
        },
    }, {
        title: {
            text: 'Dow Jones'
        },
        opposite: true
    }],

現在yAxis有一個存有軸設定的數組，第一個鍵值中是納斯達克，第二個鍵值是道瓊斯，現在我們可以區別的顯示兩者的軸標題了。opposite屬性將道瓊斯指數的y軸放在圖表的另一側，如果不設定這個屬性，則兩個y軸都會在左側。下一步則設定y軸上的series數據數組，如下：

    series: [{
        name: 'Nasdaq',
        yAxis: 0,
        data: [ ... ]
    }, {
        name: 'Dow Jones',
        yAxis: 1,
        data: [ ... ]
    }]

現在我們可以在圖表上清楚的看到兩種指數的波動了，如下：

[![chart2-3-14](/images/learning_highcharts/chart2-3-14.jpg)](/images/learning_highcharts/chart2-3-14.jpg)

此外，我們可以優化這個圖表，將series的圖例顏色引用到軸線上。Highcharts.colors屬性包含了series默認顏色的一組列表，稍後我們為我們的指數使用其中前兩個鍵值。另一個優化是為x軸設定maxPadding，因為新的y軸軸線與數據點有一些重疊了，代碼如下：

    xAxis: {
        ... ,
        minPadding: 0.02,
        maxPadding: 0.02
    },
    yAxis: [{
        title: {
            text: 'Nasdaq'
        },
        lineWidth: 2,
        lineColor: '#4572A7',
        tickWidth: 3,
        tickLength: 6,
        tickColor: '#4572A7'
    }, {
        title: {
            text: 'Dow Jones'
        },
        opposite: true,
        lineWidth: 2,
        lineColor: '#AA4643',
        tickWidth: 3,
        tickLength: 6,
        tickColor: '#AA4643'
    }],

改善後的圖表效果如下圖：

[![chart2-3-15](/images/learning_highcharts/chart2-3-15.jpg)](/images/learning_highcharts/chart2-3-15.jpg)

我們可以擴展前一個例子，再繼續增加更多的軸，只要在yAxis增加新的鍵值並匹配series數組即可，下圖是四個軸的圖表：

[![chart2-3-16](/images/learning_highcharts/chart2-3-16.jpg)](/images/learning_highcharts/chart2-3-16.jpg)