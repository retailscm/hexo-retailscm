title: "第一章 05.Highcharts快速入門手冊"
date: 2015-05-13 14:38:45
update: 2015-05-13 14:38:45
tags:
  - Highcharts
categories:
  - Learning
  - Learning Highcharts
  - Web Charts
---
# 科技零售(RetailSCM.com) Learning Highcharts中文教程

_<span style="color: #808080;">版權保留，網路轉載請註明來源，謝絕紙質媒體轉載，謝謝。</span>_

## Highcharts – 快速入門手冊

在這一節中你將看到如何做出第一份Highcharts圖表

首先，去Highcharts的網站上下載最新版的Highcharts圖表庫

## 目錄結構

把下載回來的ZIP文件解壓之後，你會看到有如下目錄結構在Highcharts-2.x.x或Highcharts-3.x.x的根目錄下：

<!--more-->

[![highcharts-chart1-quick1](/images/learning_highcharts/highcharts-chart1-quick1.jpg)](/images/learning_highcharts/highcharts-chart1-quick1.jpg)

下面會為大家說明每一個目錄中的包含的內容及用處：

*   index.html: 這是Demo的頁面，與在線幫助的Demo頁面類似，所以你可以離線來檢視Highcharts的樣式。
*   examples: 這個目錄包含所有的示例文件
*   graphics: 這個目錄包含所有示例中的圖片文件
*   exporting-server: 這個目錄是服務器端功能所需文件，是用來通過Batik來導出圖表成為一個圖片文件的服務。Batik是導出文件服務中的一種，它是管理SVG的Java的工具。
*   js: 這個目錄是Highcharts代碼的主要目錄，裡面的文件都有兩份，一份是以.scr.js來結尾的源代碼文件，一份是以.js來結尾的精簡版js文件
*   adapters: 這個目錄裡面是提供給MooTools或Prototype框架所使用的模組，包含有兩個文件：
    *   exporting.js: 這是客戶端需要導出圖表或列印圖表所需的文件
    *   canvas-tools.js 我們需要這個第三方工具來支持Android 2.x的瀏覽器可以顯示圖表(Android 2.x的原生並不支持SVG，但可以顯示canvas)
*   themes: 這個目錄中有一組預設的JavaScript文件，如背景颜色，字体样式，轴布局等，當用戶需要不同的風格時就可以加載這些文件。

你所需要做的就是將Highcharts的根目錄整個複製到你的網站根目錄下面即可。

為了使用Highcharts，你需要在你的HTML文件中引入jQuery庫文件和Highcharts-2.x.x/js/highcharts。

下面會使用一個例子來展示某一個網站來訪用戶的瀏覽器種類百分比，這個例子為了讓你可以快速上手，使用了最精簡的設定配置，以下是例子的一部分：

	<!DOCTYPE HTML>
	<html>
	<head>
		<meta http-equiv="Content-Type" content="text/html; charset=utf-8">
		<title>Highcharts First Example</title>
		<script src="//ajax.googleapis.com/ajax/libs/jquery/1.7.1/jquery.min.js"></script>
		<script type="text/javascript" src="Highcharts-2.2.2/js/highcharts.js"></script>

我們使用Google的公開jQuery庫的服務來載入jQuery 1.7.1的框架。注意jQuery需要在Highcharts之前被載入。

剩下的部分則是Highcharts的主要部分，如下：

	<script type="text/javascript">
	$(document).ready(function() {
	    var chart = new Highcharts.Chart({
	        chart: {
	        renderTo: 'container',
	        type: 'spline'
	    },
	    title: {
	        text: 'Web browsers statistics'
	    },
	    subtitle: {
	        text: 'From 2008 to present'
	    },
	    xAxis: {
	        categories: [ 'Jan 2008', 'Feb', .... ],
	        tickInterval: 3
	    },
	    yAxis: {
	        title: {
	            text: 'Percentage %'
	        },
	        min: 0
	    },
	    plotOptions: {
	        series: {
	            lineWidth: 2
	        }
	    },
	    series: 
	        [{
	            name: 'Internet Explorer',
	            data: [54.7, 54.7, 53.9, 54.8, 54.4, ... ]
	        }, {
	            name: 'FireFox',
	            data: [36.4, 36.5, 37.0, 39.1, 39.8, ... ]
	        }, {
	            // Chrome started until late 2008
	            name: 'Chrome',
	            data: [ null, null, null, null, null, null,
	            null, null, 3.1, 3.0, 3.1, 3.6, ... ]
	        }, {
	            name: 'Safari',
	            data: [ 1.9, 2.0, 2.1, 2.2, 2.4, 2.6, ... ]
	        }, {
	            name: 'Opera',
	            data: [ 1.4, 1.4, 1.4, 1.4, 1.5, 1.7, ... ]
	        }]
	    });
	});
	</script>
	</head>
	<body>
		<div>
			<!-- Highcharts rendering takes place inside this DIV -->
			<div id="container"></div>
		</div>
	</body>
	</html>

通過一個包含全部屬性及資料的對象，我們可以創建出一個曲線圖，當圖表被建立好之後，就會通過瀏覽器顯示出來。在這個對像中有包含了圖表所需要的主要的組件及結構。

	var chart = new Highcharts.Chart({
	    chart: {
	        ...
	    },
	    title: '...'
	    ...
	});

在HTML的&lt;body&gt;部分我們需要定義一個id為container的div的標籤作為圖表的容器，並將這個標籤告訴renderTo選項，以使得Highcharts去進行渲染的動作。在樣式的部分會為所有的資料呈現默認設置為曲線圖，相關代碼如下：

	chart: {
	    renderTo: 'container',
	    type: 'spline'
	}

下面會為圖表設置標題和子標題，這部分會出現在圖表的頂部中間的位置：

	title: {
	    text: 'Web browsers ... '
	},
	subtitle: {
	    text: 'From 2008 to present'
	},

xAxix中包含的categories的選項是一個將會被標註在x軸上作為各節點刻度標籤的數組。由於該圖具有至少50個數據節點，顯示全部x軸的標籤會導致他們相互重疊覆蓋，即使將標籤旋轉至垂直，顯示在標籤上也是會顯得很滿，最好的折中方式是每隔三個標籤再進行顯示(tickIntervals: 3)，這樣標籤之間的間隔空間才會看起來比較友善易讀。

_为了简单起见，我们使用50个切點的xAxis.categories的来表示时间維度。然而，我们在下一章将看到一个更加优化和更具逻辑性的方式来显示日期和时间。_

	xAxis: {
	    categories: [ 'Jan 2008', 'Feb', .... ],
	    tickInterval: 3
	},

yAxis中的option則是會標註在y軸上的刻度標籤，並設定最小值為0。否則Highcharts將在y軸上顯示負向的百分比，這不是本例中所希望表達的樣式。

	yAxis: {
	    title: {
	        text: 'Percentage %'
	    },
	    min: 0
	},

plotOption屬性是控制如何根據圖表類型(曲線圖，餅圖，柱狀圖等)來將資料顯示出來。plotOption.series選項是一個通用選項，而不是針對不同的series來進行單獨設定的選項，在本例中，所有的series中的曲線寬度都默認設定為2個像素的寬度，如下：

	plotOptions: {
	    series: {
	        lineWidth: 2
	    }
	},

series屬性是整個Highcharts設定的核心，它以數組的形式定義了所有的series資料。如本例，name選項是這個series在圖表中的圖示及提示的名字，data是y軸的值，長度與x軸相同（這些data中的值分別對應到x軸上每一個刻度）。Categories數組則對應於陣列(x,y)的數據節點。

	series: 
	    [{
	        name: 'Internet Explorer',
	        data: [54.7, 54.7, 53.9, 54.8, 54.4, ... ]
	    }, {
	        name: 'FireFox',
	        data: [36.4, 36.5, 37.0, 39.1, 39.8, ... ]
	    }, {

下面是在Safari瀏覽器上所看到Highcharts圖表最終的樣式：

[![highcharts-chart1-example-safari](/images/learning_highcharts/highcharts-chart1-example-safari.jpg)](/images/learning_highcharts/highcharts-chart1-example-safari.jpg)

下面是在IE9上看到的樣式：

[![highcharts-chart1-example-ie](/images/learning_highcharts/highcharts-chart1-example-ie.jpg)](/images/learning_highcharts/highcharts-chart1-example-ie.jpg)

下面是在Google的Chrome上看到的樣式：

[![highcharts-chart1-example-chrome](/images/learning_highcharts/highcharts-chart1-example-chrome.jpg)](/images/learning_highcharts/highcharts-chart1-example-chrome.jpg)

下面是在Firefox上看到的樣式：

[![highcharts-chart1-example-firefox](/images/learning_highcharts/highcharts-chart1-example-firefox.jpg)](/images/learning_highcharts/highcharts-chart1-example-firefox.jpg)

## 總結：

網頁圖表從早期的HTML時代即出現，逐漸從服務器端的技術發展到客戶端的技術，在此期間，有數種方案出現並得以發展用來解決HTML本身的局限，現在隨著HTML5的發展和豐富的功能，網頁圖表又回歸到HTML的技術來實現，這一次隨著HTML5配合JavaScript的支援，它將表現的越來越好。