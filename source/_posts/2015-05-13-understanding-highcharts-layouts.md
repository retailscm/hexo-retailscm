title: "第二章 02.了解圖表佈局(layout)"
date: 2015-05-13 20:56:33
update: 2015-05-13 20:56:33
tags:
  - Highcharts
categories:
  - Learning
  - Learning Highcharts
  - Highcharts configurations
---

# 科技零售(RetailSCM.com) Learning Highcharts中文教程

_<span style="color: #808080;">版權保留，網路轉載請註明來源，謝絕紙質媒體轉載，謝謝。</span>_

## 了解圖表佈局(layout)

在我們開始學習Highcharts佈局如何工作之前，我們需要事先了解一些基礎的概念。為此，讓我們先來重新找出來第一章 網頁圖表中用過的例子並且給它設定一對邊框。首先來給圖形區域周圍設定一組邊框，為了設定，我們需要在chart中設定plotBorderWidth和plotBorderColor選項，如下：

    chart: {
      renderTo: 'container',
      type: 'spline',
      plotBorderWidth: 1,
      plotBorderColor: '#3F4044'
    },

第二組邊框我們設定它圍繞著圖表的container標籤，所以我們擴展前面chart的部分增加額外的設定：

<!--more-->

    chart: {
      renderTo: 'container',
      ....
      borderColor: '#a1a1a1',
      borderWidth: 2,
      borderRadius: 3
    },

基本上，這部分會將container標籤的邊框設定為2個像素的寬度及半徑為3個像素的圓角表格。

如我們所看到，這個邊框在container容器的外圍，這個邊框也是Highcharts所有元素所處的範圍邊界。

[![understanding_highchart_layout1](/images/learning_highcharts/understanding_highchart_layout1.jpg)](/images/learning_highcharts/understanding_highchart_layout1.jpg)

默認情況下，Highcharts會顯示3個區域——間隔區域，標籤區域，圖表區域。圖表區域是在最內層的矩形部分，包含了所有的圖表的圖樣。標籤區域則是在圖表區域的外圍，它包含了標題，副標題，軸標題，圖例說明及版權說明的部分。間隔區域則是在容器的內部及標籤區域外部的這個部分，下面的圖示標註了三種不同的區域，用灰色的虛線來區分間隔區域和標籤區域。

[![chart2-2](/images/learning_highcharts/chart2-2.jpg)](/images/learning_highcharts/chart2-2.jpg)

每一個標籤的定位都可以使用下面的兩種佈局來處理：

*   自動佈局：Highcharts會依據標籤在標籤區域的位置來自動調整圖表區域的大小，這意味著標籤無法顯示在圖表區域內（無法達成將標籤與圖表重疊的效果），自動佈局是一種很簡單的配置方式，但是對圖表的調整程度比較低。自動佈局是Highcharts的默認佈局方式。
*   固定佈局：這種佈局中不再存在標籤區域這個區域，圖表標籤是指定固定的位置，使標籤顯示在圖表區域最上層。換句話說，圖表區域並不會依據標籤的位置來進行自動調整，這種方式可以讓我們完全的控制圖表應如何呈現。

間隔區域控制著Highcharts在各邊界的偏移量。間隔區域是全局變量，如果圖表沒有定義邊界大小(margin)，則增加和減少間隔區域會影響全部佈局（自動或固定佈局）。

## 圖表邊界(margin)與間隔(spacing)

在這一節中我們會看到設定邊界與間隔在圖表中所表現出來的效果。圖表邊界可以在屬性margin, marginTop, marginLeft, marginRight和marginBottom中設定，默認狀況下邊界的屬性並不會被設定。設定圖表的margin屬性在整個圖表區域中是作為全局變量出現的。

margin屬性可以是一個數組，比較類似css的方法，從上面開始順時針計算其他的位置。此外margin的優先級比直接指定方向的margin優先級要低，優先級也不會因為在chart中出現的順序比較靠前而提高。

Spacing的設定會在設定任何一個方向的spacing時自動開啟，如設定屬性spacingTop, spacingLeft, spacingBottom或spacingRight。

在本例中我們會為每一條邊來增加或修改margin或spacing屬性並觀察其效果，下面是設定：

    chart: {
      renderTo: 'container',
      type: ...
      marginTop: 10,
      marginRight: 0,
      spacingLeft: 30,
      spacingBottom: 0
    },

下面是觀察到的圖表樣子：

[![chart2-4](/images/learning_highcharts/chart2-4.jpg)](/images/learning_highcharts/chart2-4.jpg)

marginTop屬性固定了圖表區域的上邊界距離容器的邊框為10個像素的距離，同時它也使得佈局變為了固定佈局，因而圖表的標題與副標題都會重疊顯示在圖表區域上了。spacingLeft屬性增加了左邊的間隔區域距離，因此可以進一步調整Y軸標籤，這裡因為是自動佈局（沒有聲明marginLeft），所以也同步調整了左邊的圖表邊框。設置marginRight為0則會覆蓋右邊的spacing屬性，並將佈局調整為固定佈局模式。最後，設定spacingBottom為0讓圖例的部分放置到容器的底部。雖然spacingBottom設置為0，但是這部分仍然是自動佈局，因此圖表區域也會自動隨著圖例區域而延伸。

## 圖表標籤屬性

圖表標籤如xAxis.title（X軸標籤）, yAxis.title（Y軸標籤）, legend（圖例）, title（標題）, subtitle（副標題）, credits（網站說明，類似與power by的概念）公用一些相同的屬性名，如下：

*   align: 水平對齊標籤，屬性內容有left, center, right，在軸標籤則有low, middle, high
*   floating: 讓標籤具有在圖表區域浮動(floating effect)顯示。
*   Margin: 邊界設定，只有某些標籤有此選項，用來設定標籤到圖表邊界的距離。
*   verticalAlign: 垂直對齊標籤，屬性內容有top, middle, bottom
*   x: 水平對齊中的定位
*   y: 垂直對齊中的定位

在標籤的x和y的定位，是用來調整標籤在對齊中的位置，因此通常不會用在表達圖表中的絕對定位。下圖表示坐標的方向，圖中間位置為標籤的地方：

[![chart2-3](/images/learning_highcharts/chart2-3.jpg)](/images/learning_highcharts/chart2-3.jpg)

我們可以在一個簡單的例子，如將圖表中的標題與副標題放置在相鄰的位置，以此來試驗一下align屬性和y的定位設定。

標題使用align設定為left，將其移動到左側，然後將副標題align設定為right，將其移動到右側。為了讓標題與副標題可以在同一行顯示，我們還要調整副標題的y定位屬性到15，使之與標題默認的y定位呈現相同的高度。

    title: {
      text: 'Web browsers ...',
      align: 'left'
    },
    subtitle: {
      text: 'From 2008 to present',
      align: 'right',
      y: 15
    },

下圖為標題與副標題在同一行上的效果：

[![chart2-5](/images/learning_highcharts/chart2-5.jpg)](/images/learning_highcharts/chart2-5.jpg)

在下面的一節中，我們會繼續嘗試修改各個不同的對象的標籤來觀察其對應的效果。

## 標題和副標題的對齊

標題和副標題具有相同的屬性設定，唯一的差別為默認狀況下標題具有margin的設定值。指定verticalAlign可以改變默認的自動佈局為固定佈局（內部會自動將floating修改為true）。然而手動設定副標題的floating屬性為false並不會將其修改回自動佈局，下面的例子是將標題設置為自動佈局而副標題設置為固定佈局：

    title: {
      text: 'Web browsers statistics'
    },
    subtitle: {
      text: 'From 2008 to present',
      verticalAlign: 'top',
      y: 60 
    },

副標題verticalAlign屬性被設置為top，所以副標題的佈局被轉換為固定佈局，並且將y定位設置為60個像素的高度，於是副標題的定位被向下移動。在此例中，標題仍然為自動佈局，所以還在圖表區域的上方，但副標題因為為固定佈局，並且設定了固定位置為距離上邊距60像素，因此它浮動於圖表區域並重疊顯示：

[![chart2-6](/images/learning_highcharts/chart2-6.jpg)](/images/learning_highcharts/chart2-6.jpg)

_目前如果標題與副標題全部使用verticalAlign對齊時，highcharts還有一個bug，_
_可以參考官網的bug報告_
_http://github.com/highslide-software/highcharts.com/issues/962._

## 圖例（legend）對齊

圖例對verticalAlign和align這兩種屬性具有不同的設置。除了設置對齊方式為center之外，其他的對齊方式設定仍為自動定位，下面使用一個圖例靠右的例子，verticalAlign屬性設置為middle，align設置為right

    legend: {
      align: 'right',
      verticalAlign: 'middle',
      layout: 'vertical',
    },

Layout的屬性設定為vertical則圖例框中的元素都是垂直顯示，如下圖，圖表區域已經自動重設了圖例框的尺寸：

[![chart2-7](/images/learning_highcharts/chart2-7.jpg)](/images/learning_highcharts/chart2-7.jpg)

## 軸標題對齊

軸標題無法使用verticalAlign屬性，它使用align屬性，即使屬性的內容為low, middle或high(y軸的align)。軸標題的邊界設定是設定軸標題和軸線之間的距離，下面的例子中會展示y軸的標題由垂直設定轉為水平設定（默認情況下），會將軸標題顯示在軸線的上面而不是邊上，此外，我們使用y屬性來調整標題的位置：

    yAxis: {
      title: {
        text: 'Percentage %',
        rotation: 0,
        y: -15,
        margin: -70,
        align: 'high'
      },
      min: 0
    },

下面圖中的左上角可以看到圖表中y軸的標題已經從邊上移動到了y軸的上面，另一種方法為我們可以使用offset選項來代替margin達到相同的結果。

[![chart2-8](/images/learning_highcharts/chart2-8.jpg)](/images/learning_highcharts/chart2-8.jpg)

## credits（網站說明，類似與power by的概念）對齊

credits與其他標籤元素的設定有一些不同，它只支持align, verticalAlign, x和y這四種屬性在credits.position中(比較簡單的寫法為credits:{position:…})，同時它也不支持任何spacing的設定。假設我們有一個沒有圖例的圖表，並且我們要移動credits到圖表左下的部分，下面為我們的做法：

    legend: {
      enabled: false
    },
    credits: {
      position: {
        align: 'left'
      },
      text: 'RetailSCM',
      href: 'http://www.retailscm.com'
    },

然而，credits的text部分已經超出了圖表的邊界，如下圖所示：

[![chart2-9](/images/learning_highcharts/chart2-9.jpg)](/images/learning_highcharts/chart2-9.jpg)

即使我們將credits標籤移動到右邊並設定x定位，但是標籤還是有一些太過靠近x軸標籤或x軸標題，我們在這裡要特別額外介紹spacingBottom來在標籤中設定間隔的部分，如下：

    chart: {
      spacingBottom: 30,
      ....
    },
    credits: {
      position: {
        align: 'left',
        x: 20,
        y: -7
      },

下圖為調整之後的結果：

[![chart2-10](/images/learning_highcharts/chart2-10.jpg)](/images/learning_highcharts/chart2-10.jpg)

## 嘗試自動佈局

在這一節中，我們會測試自動佈局的特性，在這個看起來比較簡單的例子中，我們開始將圖表標題與圖表不在設定spacing的值：

    chart: {
      renderTo: 'container',
      // border and plotBorder settings
      .....
    },
    title: {
      text: 'Web browsers statistics,
    },

從前面的例子中，圖表的標題應該如預期般出現在容器與圖表區域之間的部分

[![chart2-11](/images/learning_highcharts/chart2-11.jpg)](/images/learning_highcharts/chart2-11.jpg)

標題和容器頂端邊框的部分默認設定spacingTop為10像素高，標題和圖表區域頂端邊框的高度在默認設置title.margin為15個像素高。

如果設定spacingTop為0，則圖表的標題會向上移動到容器的頂端邊框，因此圖表區域也會自動向上延展，如下圖：

[![chart2-12](/images/learning_highcharts/chart2-12.jpg)](/images/learning_highcharts/chart2-12.jpg)

我們接著再將title.margin設置為0，於是圖表的邊框也向上移動，圖表區域也會對應變得更高，如下圖：

[![chart2-13](/images/learning_highcharts/chart2-13.jpg)](/images/learning_highcharts/chart2-13.jpg)

你也許有注意到了，目前頂部的邊框和圖表標題時間還是有幾個像素的差距，這其實是因為標題上默認15個像素高度的y定位設定，足夠默認大小字體的高度設定。

下面圖表設定中設定了的容器與圖表區域之間的大小為0

    chart: {
      renderTo: 'container',
      // border and plotBorder settings
      .....
      spacingTop: 0
    },
    title: {
      text: 'Web browsers statistics',
      margin: 0,
      y: 0
    }

如果我們設定title.y為0，圖表頂端的邊界和容器頂端的邊界將重合，下面圖為最終效果，圖表的標題已經因為移到了容器邊框之外，所以已經看不到了。

[![chart2-14](/images/learning_highcharts/chart2-14.jpg)](/images/learning_highcharts/chart2-14.jpg)

有趣的是，如果我們回到第一個例子中，圖表區域頂邊與容器頂邊默認的距離是按照如下公式計算的：

spacingTop + title.margin + title.y = 10 + 15 + 15 = 40

因此改變任何這三個變量中的一個都會使圖表區自動調整與容器頂邊的距離。這些偏移量在自動佈局中都有他們不同的作用。

Spacing是用來控制容器與圖表之間的距離，如果我們希望控制圖表具有比較大的空間，就可以使用這個屬性來控制。同樣，如果我們希望改變標籤字體的大小，則應該考慮調整y定位屬性。設定標籤在圖表中的這些屬性並不會影響到其他的組件。

## 嘗試固定佈局

在上一節中我們了解到如何讓圖表進行動態的自動調整，在這一節中我們會來看一下如何手動調整圖表標籤的位置。首先，我們再拿上一節的示例代碼來使用，並將其中圖表標籤的verticalAlign屬性修改為bottom，如下：

    chart: {
      renderTo: 'container',
      // border and plotBorder settings 
      .....
    },
    title: {
      text: 'Web browsers statistics',
      verticalAlign: 'bottom'
    },

圖表標題移動到了圖表的底部，緊貼著容器的底邊。注意此設定會將標題修改為floating模式，其他更重要的設定在圖表區域中則仍然保持為自動佈局：

[![chart2-15](/images/learning_highcharts/chart2-15.jpg)](/images/learning_highcharts/chart2-15.jpg)

要注意的是我們並沒有設置spacingBottom屬性，但是會默認設定15個像素的高度，因為在title.y的默認值是15，這也意味著標題和容器底邊會有15個像素的距離。按照之間圖表標籤屬性一節中所提到的例圖中，y的正值會將標題向容器底部方向執行位移。

讓我們來將y的位移做一個大一點的設定，這樣我們就可以看到verticalAlign的設定使標題浮動在圖表上。

    title: {
      text: 'Web browsers statistics',
      verticalAlign: 'bottom',
      y: -90
    },

y的負值會將標題向容器頂部的方向進行位移，如圖：

[![chart2-16](/images/learning_highcharts/chart2-16.jpg)](/images/learning_highcharts/chart2-16.jpg)

現在標題和圖表是重疊的，這意味著標籤可以在自動佈局中向圖表移動，這裡我們改變了標籤的y屬性和邊距設定，我們在軸標籤上增加一個距離：

    legend: {
      margin: 70,
      y: -10
    },

這樣會向上提升圖表的底邊，然而圖表標題仍然保留為固定佈局並且其在圖表中的位置也沒有因為軸標籤的新設定而做了任何的改變，如圖：

[![chart2-17](/images/learning_highcharts/chart2-17.jpg)](/images/learning_highcharts/chart2-17.jpg)

到目前為止，我們已經可以更好的理解如何對標籤元素進行定位及他們的佈局策略與圖表之間的關係了。