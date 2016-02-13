title: "第一章 01.網頁圖表的發展歷史"
date: 2015-05-11 23:02:26
update: 2015-05-12 00:06:33
tags:
  - Highcharts
categories:
  - Learning
  - Learning Highcharts
  - Web Charts
---

# 科技零售(RetailSCM.com) Learning Highcharts中文教程

_<span style="color: #808080;">版權保留，網路轉載請註明來源，謝絕紙質媒體轉載，謝謝。</span>_

## 網頁圖表的發展歷史

在正式開始介紹Highcharts之前，值得為大家先介紹一下網頁圖表是如何從服務器端的純HTML報表發展到目前的客戶端報表。

## HTML中嵌入圖片

這種技術是比較早先主要由服務器端來進行控制，客戶端只是HTML顯示的時候使用。這時的圖表往往只是一個JPEG的圖片。

在服務器端腳本語言如PHP發展之前，通用的方法一般是使用Common Gateway Interface (CGI)來產生圖片，之後當PHP成為主流之後，更多的則是使用GD的圖形庫來產生圖表，當時的圖表實現比較像是：

    <img src="pie_chart.php" border=0 align="left">
    
產生圖表的腳本文件直接寫在HTML的img標籤中作為圖片來源，當頁面載入時，PHP的圖表產生腳本會生成圖表JPEG文件並作為圖片返回HTML中。

<!--more-->

這種方式有以下優點：

*   服務器端產生報表，因此報表產生不需要客戶端的操作或初始化。
*   適合自動產生報表，如排程執行或設定監控報表自動作為附件發郵件給管理者
*   不需要Javascript，單純的HTML即可滿足，對客戶端來說最輕量

這種方式有以下缺點：

*   完全在服務器端產生報表，對服務器的壓力很大
*   純HTML，與使用者互動有限，並且報表無法以動畫方式呈現

## Java的動態技術(Java applet & Servlet)

Java applet可以在瀏覽器上做到一些HTML無法做到的動態效果，例如繪製圖形，產生動畫，更好的使用者交互，它是第一種將服務器端的任務擴展到客戶端執行的嘗試。為了在HTML頁面中嵌入並執行Java applet，瀏覽器需要安裝Java的插件。

通常來說Java的2D圖表依靠Java Abstract Window Toolkit (AWT)中的java.awt.Graphics2D的類庫和java.awt.geom的類庫來建立。

使用Java applet來做圖表的產品是JFreeChart，非商業應用可以免費的使用這個產品，JFreeChart可以在客戶端applet中或服務器端的Servlet中執行，或者也可以寫成一個單獨的應用程式。

這種方式有以下優點：

*   相對前一種方式更好一些的圖形，動畫與使用者交互
*   代碼可以在客戶端，服務器端或應用程式中復用

這種方式有以下缺點：

*   Applet的安全性問題
*   如果插件出現異常，會導致瀏覽器死掉或強制關閉
*   對客戶端的CPU要求很高
*   需要Java插件
*   每次查看報表都需要比較久的時間以啟動Java applet
*   標準化問題

## Adobe Shockwave Flash

Flash因為可以在網頁中提供聲音，圖形，動畫，視頻等功能，在前一段時間廣泛的使用，但瀏覽器因此需要安裝一個Adobe Flash Player的插件。

在繪製圖表方面，在HTML5流行之前Flash成為了大多數人的選擇（當然那時候也沒什麼其他的選擇）

Flash基本上主要使用它自己的文件格式Shockwave Flash (SWF)，在SWF中會包含壓縮的矢量圖，編譯後的ActionScript腳本，以此來產生所需要的圖表。為了在網頁中呈現圖表，需要使用object將SWF嵌入在網頁中，語法通常會是：

    <object id="yuiswf1" type="..." data="charts.swf" width="100%"
    height="100%">
      <param name="allowscriptaccess" value="always">
      <param name="flashVars" value="param1=value1&param2=value2">
    </object>

這種方式有以下優點：

*   漂亮的圖片和動畫，豐富的使用者交互

這種方式有以下缺點：

*   和Java Applet很類似