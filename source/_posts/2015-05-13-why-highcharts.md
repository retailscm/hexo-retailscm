title: "第一章 04.為什麼選擇Highcharts"
date: 2015-05-13 12:27:13
update: 2015-05-13 12:27:13
tags:
  - Highcharts
categories:
  - Learning
  - Learning Highcharts
  - Web Charts
---
# 科技零售(RetailSCM.com) Learning Highcharts中文教程

_<span style="color: #808080;">版權保留，網路轉載請註明來源，謝絕紙質媒體轉載，謝謝。</span>_

## 什麼選擇Highcharts

雖然Highcharts只有提供基礎的2D圖表，但是它提供了在諸多商業或開源圖表庫中非常令人心動並且看起來非常專業的圖表樣式。這是一個依靠著注重細節而不是只靠外表與展現來脫穎而出的產品。Highcharts由一家名叫Highsoft Solutions AS的挪威公司於2009年開發並發布。Highcharts不是他們家第一個產品，但卻是目前為止銷售最好的產品。

## Highcharts和JavaScript框架

雖然Highcharts使用JavaScript的框架來開發，但是他並沒有完全依賴於某一個特定的框架，Highcharts使用適配器來可以與JS框架做契合，是一種鬆散耦合的結構。

因此Highcharts可以使用 MooTools, Prototype或jQuery中的任一種來作為它的JavaScript框架基礎。這可以讓用戶針對他們已經開發的產品無需為此妥協，或允許他們決定使用最適合他們項目的框架。Highcharts默認使用jQuery框架，因而需要用戶先在載入Highcharts圖表庫之前先載入jQuery框架。

如果需要將Highcharts置於MooTools的環境中執行，用戶只需要簡單的按照下面的步驟進行：

<!--more-->

	<script src="//ajax.googleapis.com/ajax/libs/mootools/1.4.5/mootoolsyui-compressed.js"></script>
	<script type="text/javascript" src="Highcharts-2.2.2/js/adapters/mootools-adapter.js"></script>
	<script type="text/javascript" src="Highcharts-2.2.2/js/highcharts.js"></script>

如果要將Highcharts運行在Prototype下，用戶需要按照下面步驟進行：

	<script src="//ajax.googleapis.com/ajax/libs/prototype/1.7.1.0/prototype.js"></script>
	<script type="text/javascript" src="Highcharts-2.2.2/js/adapters/prototype-adapter.js"></script>
	<script type="text/javascript" src="Highcharts-2.2.2/js/highcharts.js"></script>

## 圖表呈現

Highcharts圖表有效的取得了美觀度的平衡，它在做到非常不錯的視覺化效果的同時還兼顧了樣式的簡潔。默認設定的顏色設定依靠著微妙的陰影和白邊框效果達成了平滑和緩的風格，不論是在字體顏色還是軸線的顏色都沒有黑色或暗色，這樣可以使得用戶得以聚焦在特別以顏色來標註的資料面上。

[![highcharts-chart1-presentation1](/images/learning_highcharts/highcharts-chart1-presentation1.jpg)](/images/learning_highcharts/highcharts-chart1-presentation1.jpg)

在Highcharts中所有的動畫效果（初始化，更新，動態提示）都經過了特別的優化 – 平滑及阻尼般慢速停止的效果。例如環形圖（嵌套餅圖）的動畫效果非常令人印象深刻。這個領域顯然Highcharts是做的更好的，其他的一些圖表庫的動畫相比則太過僵硬，有時則做的太多，甚至有部分的動畫會讓你倒進盡胃口。

[![highcharts-chart1-presentation2](/images/learning_highcharts/highcharts-chart1-presentation2.jpg)](/images/learning_highcharts/highcharts-chart1-presentation2.jpg)

在提示與圖示中都使用到了圓角表格，簡潔的樣式不會喧賓奪主，非常貼切的融合在圖表當中，下面就是一個圓角提示框的例子：

[![highcharts-chart1-presentation3](/images/learning_highcharts/highcharts-chart1-presentation3.jpg)](/images/learning_highcharts/highcharts-chart1-presentation3.jpg)

下面是圖示的例子：

[![highcharts-chart1-presentation4](/images/learning_highcharts/highcharts-chart1-presentation4.jpg)](/images/learning_highcharts/highcharts-chart1-presentation4.jpg)

簡單來說，在Highcharts中的每一個元素共同努力的構建出了完美的圖表來，元素之間並不會特別表現明暗或顏色來單獨突出自己。

## 使用授權許可

Highcharts對非商業應用提供免費的使用許可和付費的商業使用許可。對個人使用者及非營利項目中的授權許可是Creative Commons組織中的Attribution NonCommercial 3.0許可。

Highcharts提供的商業授權有針對不同需求提供不同的授權，有按照網站數量計算以及按照開發人員計算，對於工作室來說開發人員授權顯然比起網站授權或價格更高的OEM授權許可要來的合算。開發者授權許可的好處如下：

*   在工作室或軟件開發公司中，可以提供給客人一個更好的報價
*   不用擔心開發的產品數量太多而沒有提供給客人對應的授權許可

如一般狀況，開發者授權許可是有版本更新期限，並不會自動續延，授權僅允許在獲取授權的一年內中的Highcharts版本可以無限制使用，如果超過一年，當開發者希望為自己的客戶更新到最新版的Highcharts的話，需要再更新開發者授權許可。

此外，在任何時候都可以和Highsoft討論OEM的許可，報價通常是根據項目的開發和部署數量來決定。

## 簡單的API模式

Highcharts擁有非常簡單的API接口，如果要建立一個圖表，API的構造函數只需要傳入一個有相關設置的對象即可。如果需要動態更新圖表，Highcharts也提供了一組API來進行，這些設置可以在第二章 Highcharts設定進行討論，呼叫API的部分則是會在第七章 Highcharts API中進行討論。

## 文件的水準

Highcharts的在線幫助比起其他的同類圖表庫來說可以說是一枝獨秀，它的在線幫助並不只是簡單的將定義和例子一股腦的放在頁面上，他的在線幫助是有經過深思熟慮後用心建立的，以下是這麼說的原因：

在線幫助頁面左邊的部分是已經將如何建立圖表的對象進行了組織化之後的列表，你可以點開這些折疊菜單來進一步檢視對象中的參數設定，這對幫助用戶可以快速的熟悉及使用Highcharts來說是非常有幫助的。

[![highcharts-chart1-doucuments1](/images/learning_highcharts/highcharts-chart1-doucuments1.jpg)](/images/learning_highcharts/highcharts-chart1-doucuments1.jpg)

右手邊則是Attribute的定義，這部分也是經過了深思熟慮之後才寫下來的。每一個定義都有對應的說明及一個鏈接到jsFiddle(一個非常常用且好用的JS&amp;CSS在線示範網站)網站的在線示範。

[![highcharts-chart1-doucuments2](/images/learning_highcharts/highcharts-chart1-doucuments2.jpg)](/images/learning_highcharts/highcharts-chart1-doucuments2.jpg)

在線的示範可以讓用戶可以自己修改不同的屬性值來觀察會對圖表有什麼樣的變化效果，因此，在整個在線幫助的瀏覽過程中都會非常的流暢及有效率。

## 開放性（用戶可以要求自己希望增加的特性）

一種不太通常的方式是Highcharts可以由用戶來決定每一個主要版本中的新特性（對開源項目來說這是不太尋常的做法，但這種做法讓Highcharts比同類圖表庫做的更好）。用戶可以提交一些希望具有的新特性，也可以投票給別人所提出的新特性，Highsoft公司會審視獲得最高得票數的新特性並將其列入在新特性的開發計劃中，這些詳細的計劃會被公佈在Highcharts的網站中。

此外，Highcharts還進駐在Github中，這是一個開源的版控服務，可以讓JavaScript的開發者提交自己的修正與優化。

[![highcharts-chart1-openness1](/images/learning_highcharts/highcharts-chart1-openness1.jpg)](/images/learning_highcharts/highcharts-chart1-openness1.jpg)