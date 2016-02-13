title: "第二章 08.擴展色彩漸變(colors with gradients)"
date: 2015-05-14 14:00:00
tags:
  - Highcharts
categories:
  - Learning
  - Learning Highcharts
  - Highcharts configurations
---

# 科技零售(RetailSCM.com) Learning Highcharts中文教程

_<span style="color: #808080;">版權保留，網路轉載請註明來源，謝絕紙質媒體轉載，謝謝。</span>_

## 擴展色彩漸變(colors with gradients)

Highcharts不僅支持一種顏色，同時也允許複雜的色彩漸變定義。Highcharts支持線性的色彩漸變可以是單一方向的色彩漸變也可以是輻射（或圓形）的色彩漸變，在本節中，我們嘗試驗線性的漸變，而輻射漸變會在第六章中再討論。

在Highcharts中色彩的漸變是基於SVG的線性色彩漸變標準來實現的，它由以下兩種組合所構成：

*   linearGradient：給出了x和y軸兩套坐標的顏色範圍作為漸變的方向，比率在0到1之間，或者為一個百分數
*   stops：給出一組顏色的序列來填充他們在漸變方向的某一位置

我們使用前面股市的例子中交易量的series，並重新定義yAxis.alternateGridColor，如下：

<!--more-->

    yAxis: [{
        title: { text: 'Nasdaq index' },
        ....
        alternateGridColor: {
            linearGradient: [ 10, 250, 400, 250 ],
            stops:  [
                    [ 0, 'red' ],
                    [ 0.2, 'orange' ],
                    [ 0.5, 'yellow' ] ,
                    [ 0.8, 'green' ] ,
                    [ 1, 'lime' ] ]
        }

linearGradient是一個坐標值數據，被指定在x1,y1,x2,y2。這些值可以使用固定的定位來表示也可以使用百分比來表示，差別是固定的值可能會被圖表大小影響而百分比則不會。

_固定位置的設定已經不建議使用了，因為他們在SVG和VML中無法正常使用。並且也與不同大小的圖表無法很好的配合。_

stops屬性有一個元素集數組，第一個值是0～1的位移範圍，第二個值是顏色的定義。位移和顏色的值定義為顏色在範圍中的位置，如[0, 'red' ] 和[0.2, 'orange' ]意味著從紅色開始然橫向後漸變橘色，方向朝向的位置是x = 80 (0.2 * 400)，然後從 x = 80到 x = 200的地方顏色由橘色變為黃色等，下圖為多色漸變：

[![chart2-8-1](/images/learning_highcharts/chart2-8-1.jpg)](/images/learning_highcharts/chart2-8-1.jpg)

如果我們所看到的，紅色和橘色並為顯示在圖表中，因為顏色漸變是依據坐標，因而受到圖表大小的影響，x軸在這個例子中的位置已經超出了紅色和橘色的坐標。另一種方法，我們可以指定linearGradient為一個百分比的範圍，如下：

    linearGradient: [ '20%', 250, '90%', 250 ]

這意味著linearGradient從20%的寬度部分延伸90%的地方，所以色帶不會再受限與圖表的大小，下圖為新的linearGradient設定後的效果：

[![chart2-8-2](/images/learning_highcharts/chart2-8-2.jpg)](/images/learning_highcharts/chart2-8-2.jpg)

這個圖表的背景現在已經有完整的色彩區域，用於指定0～1之間的比率時linearGradient必須定義一個樣式對象，否則這些值會被作為坐標來看待。注意這些值只有使用在繪圖的區域內，而不是全局的設定。

    linearGradient: { x1: 0, y1: 0, x2: 1, y2: 0 }

此設定是另一種設定橫向色彩漸變的方式：

[![chart2-8-3](/images/learning_highcharts/chart2-8-3.jpg)](/images/learning_highcharts/chart2-8-3.jpg)

下面的代碼會調整為縱向的色彩漸變：

    linearGradient: { x1: 0, y1: 0, x2: 0, y2: 1 }

它會將背景的色彩漸變調整為縱向的方向，我們可以將'Jan' 和 'Jul'的數據點設置為point object，並使用線性的方式實現縱向陰影漸變。

[![chart2-8-4](/images/learning_highcharts/chart2-8-4.jpg)](/images/learning_highcharts/chart2-8-4.jpg)

此外，我們可以通過Highcharts的標準顏色來觸發series上的色彩漸變，它可以讓圖表看起來類似3D圖表的效果，在畫圖表之間，我們需要先覆蓋默認的series顏色為一個漸變色彩，下面的代碼片段使用一個藍色的漸變陰影來替代原本的series顏色，注意漸變的比率數值在本例中參考了柱圖的寬度：

    $(document).ready(function() {
        Highcharts.getOptions().colors[0] = {
            linearGradient: { x1: 0, y1: 0, x2: 1, y2: 0 },
            stops:  [ [ 0, '#4572A7' ],
                    [ 0.7, '#CCFFFF' ],
                    [ 1, '#4572A7' ] ]
        };
        var chart = new Highcharts.Chart({ ...

下圖為使用了色彩陰影漸變的柱圖：

[![chart2-8-5](/images/learning_highcharts/chart2-8-5.jpg)](/images/learning_highcharts/chart2-8-5.jpg)

## 總結

在本章，我們使用案例來討論及試驗了重要的組件設定。現在我們應該可以知道如何繪製一些基礎的圖形及應用對應的樣式。在下一章中，我們會了解Highcharts的折線圖，區域圖，散點圖，我們會在這一章中了解及學到series-specific樣式選項的配置來繪製一些有精美樣式的圖表。