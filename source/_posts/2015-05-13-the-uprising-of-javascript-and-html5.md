title: "第一章 02.JavaScript和HTML5的崛起"
date: 2015-05-13 07:38:45
update: 2015-05-13 07:38:45
tags:
  - Highcharts
categories:
  - Learning
  - Learning Highcharts
  - Web Charts
---
# 科技零售(RetailSCM.com) Learning Highcharts中文教程

_<span style="color: #808080;">網路轉載請註明來源，謝絕紙質媒體轉載，謝謝。</span>_

## JavaScript和HTML5的崛起

JavaScript的角色從一個簡單的用戶端語言逐漸佔據了主導，成為創建和管理web使用者介面的首選語言。然而十年之前，程式設計技術並不是這樣。有一群開發人員，像Douglas Crockford 他的書《JavaScript: The Good Parts》(O'Reilly Medial/Yahoo Press)，使JavaScript成為更好的語言；Sam Stephenson，創建了prototype JavaScript庫（[http://www.prototypejs.org](http://www.prototypejs.org))，還有John Resig，JQuery庫的作者，是他們把JavaScript帶入到構建複雜Web前端軟體的框架中。

介紹新的程式設計風格已經超出了本書的範圍。然而，在本書中，我們將看到用JQuery寫成的例子（因為Highcharts使用JQuery庫作為默認選擇，而且JQuery本身也是最流行的JavaScript框架）。期望讀者對JQuery和CSS選擇器的語法有基本的瞭解。此外，讀者還應該熟悉在《JavaScript：The Good Parts》中講到的JavaSCript進階部分：原型（prototypes），閉包（closure），繼承（inheritance），以及函數物件（function objects）。

<!--more-->

## HTML5（SVG 和 canvas）

在這一節中，將介紹兩個HTML5技術：SVG和canvas，以及幾個例子。

### SVG（Scalable Vector Graphics 可縮放向量圖）

HTML5是目前HTML標準的重大改進。採用HTML5技術的領域也迅速擴大（蘋果移動設備不再支援Adobe Flash，也助長了HTML5的應用）。HTML5也帶來了許多新的特性。許多新特性已經超出了本書的討論範圍，其中和本書關係最大的就是可縮放向量圖（SVG）。SVG是使用XML描述的數量圖，由路徑，文本，形狀，顏色等元件組成。這和PostScript很像，不過PostScript是基於堆疊的程式設計語言。SVG的名字隱含了一個主要優點，就是無損技術（和PostScript相同）；不會因為圖像的放大出現區塊，也不會因為縮小圖片尺寸丟失原始內容。

此外，SVG可以使用SMIL（同步多媒體集成語言）編寫動畫腳本，進行事件處理。目前的主流流覽器都支援SVG技術。

下面是一個簡單的例子，在兩點之間畫一個曲線：

	<svg xmlns="http://www.w3.org/2000/svg" version="1.1">
	  <path id="curveAB" d="M 100 350 q 150 -300 300 0" stroke="blue" stroke-width="5" fill="none" />
	  <!-- Mark relevant points -->
	  <g stroke="black" stroke-width="3" fill="black">
	    <circle id="pointA" cx="100" cy="350" r="3" />
	    <circle id="pointB" cx="400" cy="350" r="3" />
	  </g>
	  <!-- Label the points -->
	  <g font-size="30" font="sans-serif" fill="black" stroke="none" text-anchor="middle">
	    <text x="100" y="350" dx="-30">A</text>
	    <text x="400" y="350" dx="30">B</text>
	  </g>
	</svg>

上面的SVG代碼，執行了這些操作：

1.  繪製一個路徑，id為"curveAB"，資料為d。首先，移動（M）到座標點（100，350），以此為起點，畫一個貝塞爾二次曲線，過（150，-300）到（300，0）。

2. 組合（g）兩個圓圈元素——點A和點B，以他們的座標（100，350）和（400，350）為圓心，畫一個半徑為3圖元的圓，然後填充黑色。

3. 組合（g）兩個文本元素A和B，以（100，350）和（400，350）為起點，使用黑色sans-serif字體顯示。並分別沿x軸（dx）向兩邊偏移30圖元。

最後如圖：

[![01.02.01.svg](/images/learning_highcharts/01.02.01.svg_.png)](/images/learning_highcharts/01.02.01.svg_.png)

### Canvas

Canvas是另外一個HTML5新標準，被一些JavaScript繪圖軟體使用。如它的名字，你需要使用canvas標籤聲明一塊繪圖區域，然後使用新的JavaScript API以圖元為單位繪製線或者圖形。Canvas沒有內建對動畫的支援，可以以順序調用API的方式來類比動畫。另外，Canvas也不支援事件處理，開發者需要手動將事件處理控制碼掛載到畫布中的某個區域上。因此，一些花哨的圖示動畫的實現非常複雜。

下面是一段canvas代碼，實現和前面SVG一樣的效果：

	<canvas id="myCanvas" width="500" height="300" style="border:1px solid #d3d3d3;">Canvas tag not supported</canvas>
	<script type="text/javascript">
	var c=document.getElementById("myCanvas");
	var ctx=c.getContext("2d");
	// Draw the quadratic curve from Point A to B
	ctx.beginPath();
	ctx.moveTo(100, 250);
	ctx.quadraticCurveTo(250, 0, 400, 250);
	ctx.strokeStyle="blue";
	ctx.lineWidth=5;
	ctx.stroke();
	// Draw a black circle attached to the start of the curve
	ctx.fillStyle="black";
	ctx.strokeStyle="black";
	ctx.lineWidth=3;
	ctx.beginPath();
	ctx.arc(100,250,3, 0, 2* Math.PI);
	ctx.stroke();
	ctx.fill();
	// Draw a black circle attached to the end of the curve
	ctx.beginPath();
	ctx.arc(400,250,3, 0, 2* Math.PI);
	ctx.stroke();
	ctx.fill();
	// Display 'A' and 'B' text next to the points
	ctx.font="30px 'sans-serif'";
	ctx.textAlign="center";
	ctx.fillText("A", 70, 250);
	ctx.fillText("B", 430, 250);
	</script>

如你所見，Canvas和SVG完成同樣的工作，canvas需要更多的代碼：

[![01.02.02.canvas](/images/learning_highcharts/01.02.02.canvas.png)](/images/learning_highcharts/01.02.02.canvas.png)

與SVG使用一個連續的路徑描述不同，canvas使用了一系列調用JavaScript繪圖方法。調用屬性設置函數設置屬性，而不在標籤中聲明屬性。SVG主要是聲明，而canvas則是運行程式。