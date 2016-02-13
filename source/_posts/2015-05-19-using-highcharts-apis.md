title: "第七章 02.使用Highcharts APIs"
date: 2015-05-19 03:00:00
tags:
  - Highcharts
categories:
  - Learning
  - Learning Highcharts
  - Highcharts APIs
---

# 科技零售(RetailSCM.com) Learning Highcharts中文教程

_<span style="color: #808080;">版權保留，網路轉載請註明來源，謝絕紙質媒體轉載，謝謝。</span>_

## 使用Highcharts APIs

在這一節，我們會通過使用jQuery, jQuery UI及Highcharts建立一個例子來探索每一個組件的APIs。所有示例中的代碼都使用對象層級的方式來取得圖表組件（如chart.series[0].data[0]）

使用界面的部分會非常簡單，並不會刻意美化，我們主要的目的是練習Highcharts的APIs。

首先，讓我們來說明一下使用界面的用法然後解釋代碼的部分來理解操作是如何被執行的。下面是使用前端的截圖：

[![7-2](/images/learning_highcharts/7-2.png)](/images/learning_highcharts/7-2.png)

<!--more-->

這是一個非常簡單的前端Web頁面，用來繪製過去30天的股票數據圖表。最上面的一組按鈕是用來輸入股票代碼，得到股票價格以及將圖表的圖片下載或使用e-mail來發送。Add to list按鈕是可以將股票代碼直接加入到列表中，而不需要再取得股票價格及繪製這些數據。Plot All按鈕是啟動多個股票價格查詢，將股票代碼清單中的股票價格查出來並同時將這些數據繪製出來，另一個Add & Plot按鈕則是快速的繪製一個單個股票代碼的數據。

底部的部分則是包含了一個我們已經創建的圖表，圖表的數據部分是空的，只有顯示軸線和標題（注意showAxes選項為true），主要的想法是復用已經存在的圖表而非每次有新的資料時就重新創建一個新的圖表對象。因此這裡在報表被結束及新增時沒有絢麗的效果（flickering），只是會設定為一個平滑的動畫進行更新。當然這樣也會讓我們使用代碼來重新產生圖表對象時獲得更高的效能。

這個示例可以訪問[http://joekuan.org/Learning_Highcharts/Chapter_7/Example_1.html](http://joekuan.org/Learning_Highcharts/Chapter_7/Example_1.html)，因為安全限制，e-mail及下載功能在這個線上示例中會關閉掉。

    var chart = new Highcharts.Chart({
        chart: {
            renderTo: 'container',
            showAxes: true,
            borderWidth: 1
        },
        title: {
            text: 'Last 30 days stock price'
        },
        credits: {
            text: 'Learning Highcharts'
        },
        xAxis: {
            type: 'datetime',
            tickInterval: 24 * 3600 * 1000,
            dateTimeLabelFormats: {
                day: '%Y-%m-%d'
            },
            title: {
                text: 'Date',
                align: 'high'
            },
            labels: {
                rotation: -45,
                align: 'center',
                step: 2,
                y: 40,
                x: -20
            }
        },
        yAxis: {
            title: {
                text: 'Price ($)'
            }
        },
        plotOptions: {
            line: {
                allowPointSelect: true
            }
        }
    });


## 使用Chart.addSeries通過Ajax獲得資料並呈現為一個新的序列

讓我們來測試點擊Add & Polt按鈕之後的行為， HTML如下

    <input id="plotStock" type="button" value="Add & Plot" />

按鈕動作的jQuery代碼如下所列：

    $('#plotStock').button().click(
        function(evt) {
            // Get the input stock symbol, empty the
            // list andinsert the new symbol into the list
            $('#stocklist').empty();
            var symbol = $('#symbol').val();
            $('#stocklist').append($("").append(symbol));
            // Kick off the loading screen
            chart.showLoading("Getting stock data ....");
            // Launch the stock query
            $.getJSON('./stockQuery.php?symbol=' +
                symbol.toLowerCase(),
                function(stockData) {
                    // parse JSON response here
                    .....
                }
            );
        }
    );

上面的代碼定義了Add & Plot按鈕點擊後的事件處理程序。首先，它會清空id為stocklist的股票代碼列表清單選框。然後他會從股票代碼的輸入框中取得到股票代碼並增加到股票代碼清單中。下一步會在圖表區使用chart.showLoading方法初始化載入時的提示內容，載入時的畫面如下所示：

[![7-3](/images/learning_highcharts/7-3.png)](/images/learning_highcharts/7-3.png)

下一步會啟動jQuery Ajax呼叫$.getJSON來查詢股票價格。服務器端的腳本stockQuery.php（當然你也可以使用任何一種其他的服務器端語言來實現）會執行兩個任務——將股票代碼解析為公司或組織的全名並從另一個網站(http://ichart.finance.yahoo.com/ )來取得最後的股票價格，然後將這些數據併入在同一行數據並將其編碼為JSON格式。下面是stockQuery.php的文件內容：

    <?php
        $ch = curl_init();
        curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
        // Get the stock symbol name
        curl_setopt($ch, CURLOPT_URL, "http://download.finance.yahoo.com/d/
            quotes.csv?s={$symbol}&f=n");
        $result = curl_exec($ch);
        $name = trim(trim($result), '"');
        // Get from now to 30 days ago
        $now = time();
        $toDate = localtime($now, true);
        $toDate['tm_year'] += 1900;
        $fromDate = localtime($now - (86400 * 30), true);
        $fromDate['tm_year'] += 1900;
        $dateParams = "a={$fromDate['tm_mon']}&b={$fromDate['tm_
            mday']}&c={$fromDate['tm_year']}" ."&d={$toDate['tm_
            mday']}&e={$toDate['tm_mday']}&f={$toDate['tm_year']}";
        curl_setopt($ch, CURLOPT_URL, "http://ichart.finance.yahoo.com/
            table.csv?s={$symbol}&{$dateParams}&g=d");
        $result = curl_exec($ch);
        curl_close($ch);
        // Remove the header row
        $lines = explode("\n", $result);
        array_shift($lines);
        $stockResult['rows'] = array();
        // Parse the result into dates and close value
        foreach((array) $lines as $ln) {
            if (!strlen(trim($ln))) {
                continue;
            }
            list($date, $o, $h, $l, $c, $v, $ac) =
            explode(",", $ln, 7);
            list($year, $month, $day) = explode('-', $date, 3);
            $tm = mktime(12, 0, 0, $month, $day, $year);
            $stockResult['rows'][] =
            array('date' => $tm * 1000,
            'price' => floatval($c));
        }
        $stockResult['name'] = $name;
        echo json_encode($stockResult);
    ?>

下面是從Server端返回的JSON格式：

    {
        "rows": [{
                "date": 1348138800000,
                "price": 698.7
            }, {
                "date": 1348225200000,
                "price": 700.09
            },
            ...
        ],
        "name": "Apple Inc."
    }

當我們收到了JSON的返回結果後，資料會通過getJSON處理程序將其轉化為一個數組。下面是處理程序的部分：

    $.getJSON('./stockQuery.php?symbol=' +
        symbol.toLowerCase(),
        function(stockData) {
            // Remove all the chart existing series
            while (chart.series.length) {
                chart.series[0].remove()
            }
            // Construct series data and add the series
            $.each(stockData.rows,
                function(idx, data) {
                    $.histStock.push([data.date,
                        data.price
                    ]);
                }
            );
            var seriesOpts = {
                name: stockData.name + ' - (' + symbol + ')',
                data: $.histStock,
                // This is to stop Highcharts rotating
                // the color and data point symbol for
                // the series
                color: chart.options.colors[0],
                marker: {
                    symbol: chart.options.symbols[0]
                }
            };
            chart.hideLoading();
            chart.addSeries(seriesOpts);
        }
    );

首先，我們通過呼叫Series.remove來移除圖表上已存在的序列，然後為數據數組中的時間(UTC格式)和價格來設定序列的選項。完成後我們就可以使用Chart.hideLoading將圖表的載入訊息移除並使用Chart.addSeries來載入一個新的序列，只是會在序列重新載入的時候有一點小問題：在chart.options.colors和chart.options.symbols的值會在圖表移除和重新加回來的時候一直累加，我們可以明確的指定這兩個值來避免這個問題。

我們也可以通過Series.setData來達到相同的結果，但是序列的名字/主題在序列建立的時候一旦制定好之後，就不允許再被更改了。因此在這個示例中，我們仍然會使用Chart.addSeries 和 Series.remove。

下面是一個單個股票的查詢：

[![7-4](/images/learning_highcharts/7-4.png)](/images/learning_highcharts/7-4.png)

## 同時呼叫多個Ajax並顯示為多個序列

下面的部分會討論同時啟動多個Ajax查詢，並在所有數據都返回的時候繪出對應的序列。它實現的方式與查詢並繪製單個股票的方式非常相似，除了我們是在收集到全部的返回結果並在最後一個結果返回之後才開始建立這個序列數組。

    // Query all the stocks simultaneously and
    // plot multipleseries in one go
    $('#plotAll').button().click(
        function(evt) {
            // Kick off the loading screen
            chart.showLoading("Getting multiple stock data ....");
            // Get the list of stock symbols and launch
            // the query foreach symbol
            var total = $('#stocklist').children().length;
            // start Ajax request for each of the items separately
            $.each($('#stocklist').children(),
                function(idx, item) {
                    var symbol = $(item).text();
                    $.getJSON('./stockQuery.php?symbol=' +
                        symbol.toLowerCase(),
                        function(stockData) {
                            // data arrives, buildup the series array
                            $.each(stockData.rows,
                                function(idx, data) {
                                    $.histStock.push([data.date,
                                        data.price
                                    ]);
                                }
                            );
                            seriesOpts.push({
                                name: stockData.name + ' - (' + symbol + ')',
                                data: $.histStock,
                                // This is to stop Highcharts
                                // rotating the colorfor the series
                                color: chart.options.colors[idx],
                                marker: {
                                    symbol: chart.options.symbols[idx]
                                }
                            });
                            // Plot the series if this result
                            // is the last one
                            if (seriesOpts.length == total) {
                                // Remove all the chart existing series
                                while (chart.series.length) {
                                    chart.series[0].remove()
                                }
                                chart.hideLoading();
                                $.each(seriesOpts,
                                    function(idx, hcOption) {
                                        chart.addSeries(hcOption,
                                            false);
                                    }
                                );
                                chart.redraw();
                            } // else – do nothing,
                            // not all results came yet
                        } // function(stockData)
                    ); // getJSON
                }); // $.each($('#stocklist')
        }); // on('click'

Chart.addSeries, redraw的第二個參數使用false來代替，我們在Chart.redraw的一個呼叫中完成全部的更新以減少CPU的執行時間。下面是多個股票的查詢：

[![7-5](/images/learning_highcharts/7-5.png)](/images/learning_highcharts/7-5.png)

## 使用Chart.getSVG提取SVG數據

在這一節中我們會學習到將一個圖表匯出並使用email傳送或文件下載。雖然我們可以依靠匯出模塊及呼叫exportChart方法來匯出圖表到所需的圖片格式，但通過這個過程我們我可以更深入的了解到圖表是如何從原始的SVG內容轉為一個圖片文件，之後就會再簡單的呼叫一個不同的工具將圖片文件保存到服務器端。

當匯出模組被載入後，可以呼叫getSVG方法從圖表中提取數據到SVG中，這個方法對exportChart來說非常簡單，設定chartOptions參數這個用於匯出圖表的就可以完成。

下面是用來處理下載和email發送的客戶端jQuery代碼，在這裡我們使用protocol變量來定義圖表和兩個按鈕呼叫所預設的共用方法deliverChart:

// Export chart into SVG and deliver it to the server

    function deliverChart(chart, protocol, target) {
        // First extracts the SVG markup content from the
        // displayed chart
        var svg = chart.getSVG();
        // Send the whole SVG to the server and url
        $.post('./deliverChart.php', {
                svg: svg,
                protocol: protocol,
                target: target
            },
            function(result) {
                var message = null;
                var title = null;
                switch (protocol) {
                    // Create a dialog box to show the
                    // sent status
                    case 'mailto':
                        message = result.success ?
                            'The mail has been sent successfully' :
                            result.message;
                        title = 'Email Chart';
                        break;
                        // Uses hidden frame to download the
                        // image file created on the server side
                    case 'file':
                        // Only popup a message if error occurs
                        if (result.success) {
                            $('#hidden_iframe').attr("src",
                                "dlChart.php");
                        } else {
                            message = result.message;
                            title = 'Download Chart';
                        }
                        break;
                }
                if (message) {
                    var msgDialog = $('#dialog');
                    msgDialog.dialog({
                        autoOpen: false,
                        modal: true,
                        title: title
                    });
                    msgDialog.text(message);
                    msgDialog.dialog('open');
                }
            }, 'json');
    }

deliverChart方法首先會呼叫Highcharts API getSVG來提取SVG的內容，然後會啟動一個POST請求並將SVG數據和執行方式作為參數傳入。當$.post作為一個任務狀態值返回時，頁面會彈出一個對話框。至於圖表下載的部分，我們會創建一個隱藏表單域

    <iframe height="240" width="320"></iframe>

下面是一個很簡單的服務器端腳本，以用來處理將SVG數據產出為一個文件。

    <?php
        $svg = $_POST['svg'];
        $protocol = $_POST['protocol'];
        $target = $_POST['target'];
        function returnError($output) {
            $result['success'] = false;
            $result['error'] = implode("
    ", $output);
            echo json_encode($result);
            exit(1);
        }
        // Format the svg into an image file
        file_put_contents("/tmp/chart.svg", $svg);
        $cmd = "convert /tmp/chart.svg /tmp/chart.png";
        exec($cmd, $output, $rc);
        if ($rc) {
            returnError($output);
        }
        // Deliver the chart image file according to the url
        if ($protocol == 'mailto') {
            $cmd = "EMAIL='{$target}' mutt -s 'Here is the chart' -a /tmp/chart.
        png -- {$protocol}:{$target} <

網頁腳本運行在Linux平台上(Ubuntu 12.04)，至於email的部分我們使用兩個命令行工具來幫助我們。首先是一個快速的圖形轉換工具convert，它是ImageMagick包的一部分（關於這個包可以在[http://www.imagemagick.org/script/index.php](http://www.imagemagick.org/script/index.php)這裡可以看到更多的詳細資料）。在腳本中我們將SVG數據作為POST的參數提交到服務器端，並將其保存到一個文件中，然後執行轉換工具將其轉換為一個PNG圖片。這個轉換工具還支援其他的圖片格式和其他很多的功能。此外我們還可以使用以下指令來透過Batik來做一個簡單的轉換

    java -jar batik-rasterizer.jar /tmp/chart.svg

這個指令也可以將SVG文件自動輸出到/tmp/chart.png文件中。因為希望可以快速實現email的緣故，我們會啟動一個email工具mutt(可在這個網站查看到更多詳細資料http://www.mutt.org )來代替PHP mail擴展。一旦PNG圖片文件被建立完成，我們使用mutt來將其作為一個附件並使用定界符來定制郵件內容。（定界符是在Unix命令行可以快速輸入換行和空格的方式，可參見http://en.wikipedia.org/wiki/Here_document ）

下面是郵件被發出後的截圖

[![7-6](/images/learning_highcharts/7-6.png)](/images/learning_highcharts/7-6.png)

下面是我們收到的將圖表作為附件的郵件內容。

[![7-7](/images/learning_highcharts/7-7.png)](/images/learning_highcharts/7-7.png)

## 選擇數據點及增加測繪線(plot lines)

下面的部分來增加Show Range選框及Show Point Value按鈕。Show Range選項會在圖表的最高點和最低點畫兩條測繪線，Show Point Value會將當前選中的數據點中的數值顯示在左下角的一個方框區域中，下圖演示了這兩個選項都開啟時的效果

[![7-8](/images/learning_highcharts/7-8.png)](/images/learning_highcharts/7-8.png)

（雖然Show Point Value的範例在這裡雖然可以使用在每個數據節點的select事件上應用回調處理程序的方式來取得需要顯示的數值，但我們可以直接使用Chart.getSelectedPoints方法即可）

## 使用Axis.getExtremes和Axis.addPlotLine

Axis.getExtremes方法不僅返回軸線的最小值和最大值的範圍，並且也返回來最高和最低的數據節點，在這裡我們合併使用Axis.addPlotLine函數可以增加一對沿著Y軸的測繪線。addPointLine通常比用來配置一些測繪線，在這個例子中，我們特別定義一個數據標籤及一個id的名字，使得我們可以在取消Show Range勾選或需要重新顯示新值之後可以移除測繪線，下面是Show Range的代碼：

    // Show the highest and lowest range in the plotlines.
    var showRange = function(chart, checked) {
        if (!chart.series || !chart.series.length) {
            return;
        }
        // Checked or not checked, we still need to remove
        // any existing plot lines first
        chart.yAxis[0].removePlotLine('highest');
        chart.yAxis[0].removePlotLine('lowest');
        if (!checked) {
            return;
        }
        // Checked - get the highest & lowest points
        var extremes = chart.yAxis[0].getExtremes();
        // Create plot lines for the highest & lowest points
        chart.yAxis[0].addPlotLine({
            width: 2,
            label: {
                text: extremes.dataMax,
                enabled: true,
                y: -7
            },
            value: extremes.dataMax,
            id: 'highest',
            zIndex: 2,
            dashStyle: 'dashed',
            color: '#33D685'
        });
        chart.yAxis[0].addPlotLine({
            width: 2,
            label: {
                text: extremes.dataMin,
                enabled: true,
                y: 13
            },
            value: extremes.dataMin,
            zIndex: 2,
            id: 'lowest',
            dashStyle: 'dashed',
            color: '#FF7373'
        });
    };

## 使用Chart.getSelectedPoints和Chart.renderer方法

Show Point Value按鈕利用Chart.getSelectedPoints來出去當前所選中的數據點。注意這個方法需要序列選項中的allowPointSelect是開啟的狀態。一旦選中了一個數據點，並且Show Point Value按鈕被點擊後，我們使用Chart.renderer所提供的函數來畫出一個類似提示框的方框顯示所選中數據節點的值，我們可以使用方法Renderer.path或Renderer.rect來畫這個圓角框，再使用Renderer.text來顯示數據值。

（Highcharts也允許按住Ctrl鍵複選多個數據節點）

另外我們通常使用Renderer.g來將SVG框和值一起加入到圖表的一個組元素中，原因是這樣我們就可以在需要顯示新值的時候直接整體刪除而不是每個元素單獨操作。

    $('#showPoint').button().click(function(evt) {
        // Remove the point info box if exists
        chart.infoBox && (chart.infoBox =
            chart.infoBox.destroy());
        // Display the point value box if a data point
        // is selected
        var selectedPoint = chart.getSelectedPoints();
        var r = chart.renderer;
        if (selectedPoint.length) {
            chart.infoBox = r.g();
            r.rect(20, 255, 150, 30, 3).attr({
                stroke: chart.options.colors[0],
                'stroke-width': 2
            }).add(chart.infoBox);
            // Convert selected point UTC value to date string
            var tm = new Date(selectedPoint[0].x);
            tm = tm.getFullYear() + '-' +
                (tm.getMonth() + 1) + '-' + tm.getDate();
            r.text(tm + ': ' + selectedPoint[0].y,
                28, 275).add(chart.infoBox);
            chart.infoBox.add();
        }
    });

Highcharts的Renderer類還附帶了其他的方法在圖表上來畫一個簡單的SVG圖形，如arc, circle, image, rect, text, g和path。更多複雜的圖形，我們可以使用path方法採用SVG的path語法及有限度支援的VML path。Renderer類還可以獨立在圖表外中，我們可以在圖表建立之前事先增加一個SVG內容並顯示在一個HTML元素中。

    var renderer = new Highcharts.Renderer($('#container')[0],200, 100);

這樣會產生一個Renderer對象並允許我們創建一個200像素寬，100像素高的SVG元素塞入到container元素中。

## 探索Series更新

series更新是在報表執行過程中非常頻繁的任務之一，在本節中我們會重點探討它。在Highcharts中我們有幾種途徑來更新series，通常我們可以從一個序列或數據節點的層級來更新，然後更新方法會自己產生一個實際的變更或重新塞入新的數值，我們需要討論全部的方法並創建一個範例來嘗試這些技術。

為了比較這些方法，我們繼續使用股票數據，但是我們這次改變使用界面可以看到歷史的股票價格，下面是這個示例的截圖：

[![7-9](/images/learning_highcharts/7-9.png)](/images/learning_highcharts/7-9.png)

如我們所看到的，這裡有多個選單：要看到多少年的股票價格，每個迭代中要看到多少的數據節點，每次更新之間的時間間隔是多久，我們可以觀察到他們之間有趣的差異。這個範例也可以在這裡之間查看[http://joekuan.org/Learning_Highcharts/Chapter_7/Example_2.html](http://joekuan.org/Learning_Highcharts/Chapter_7/Example_2.html)，我強烈建議將這個例子的代碼看一遍。在我們開始看之前，讓我們先來了解如何對連續序列進行更新的過程。

## 連續序列的更新

當我們輸入了股票代碼及選擇了所希望查看的年數之後，我們可以點擊Load Data按鈕來獲取價格數據，當數據獲取完成後，將會彈出一個確認對話框，按下Start按鈕後即可啟動這個流程，下面是Start按鈕的代碼：

    // Create a named space to store the current user
    // input field values and the timeout id
    $.histStock = {};
    $('#Start').button().click(function() {
        chart.showLoading("Loading stock price ... ");
        // Remove old timeout if exists
        $.histStock.timeoutID &&
            clearTimeout($.histStock.timeoutID);
        var symbol =
            encodeURIComponent($('#symbol').val().toLowerCase());
        var years = encodeURIComponent($('#years').val());
        // Remember current user settings and initialize values
        // for the run
        $.histStock = {
            // First loop start at the beginning
            offset: 0,
            // Number of data pts to display in each iteration
            numPoints: 30,
            // How long to wait in each iteration
            wait: parseInt($('#updateMs').val(), 10),
            // Which Highcharts method to update the series
            method: $('#update').val(),
            // How many data points to update in each iteration
            'update:'
            parseInt($('#updatePoints').val(), 10)
        };
        // Clean up old data points from the last run
        chart.series.length && chart.series[0].setData([]);
        // Start Ajax query to get the stock history
        $.getJSON('./histStock.php?symbol=' + symbol +
            '&years=' + years,
            function(stockData) {
                // Got the whole period of historical stock data
                $.histStock.name = stockData.name;
                $.histStock.data = stockData.rows;
                chart.hideLoading();
                // Start the chart refresh
                refreshSeries();
            }
        );
    })

我們首先在jQuery命名空間下創建一個變量histStock用來訪問這個示例的各部分。histStock會將當前使用者的輸入保存下來並關聯到刷新任務(the reference to the refresh task)。任何從UI更新$.histStock都會相應對序列作出更新。

基本上，當點擊了Start按鈕後，我們會初始化$.histStock變量並將股票代碼和查詢年數摻入到Ajax的查詢中，返回股票價格數據會存在一個變量中，然後執行refreshSeries，它會通過定時執行設定來自己啟動，下面是這個方法的精簡版：

    var refreshSeries = function() {
        var i = 0,
            j;
        // Update the series data according to each approach
        switch ($.histStock.method) {
            case 'setData':
                ....
                break;
            case 'renewSeries':
                ....
                break;
            case 'update':
                ....
                break;
            case 'addPoint':
                ....
                break;
        }
        // Shift the offset for the next update
        $.histStock.offset += $.histStock.update;
        // Update the jQuery UI progress bar
        ....
        // Finished
        if (i == $.histStock.data.length) {
            return;
        }
        // Setup for the next loop
        $.histStock.timeoutID =
            setTimeout(refreshSeries, $.histStock.wait);
    };

在refreshSeries內部，會檢查$.histStock內的設定並根據使用者的選擇來更新序列，一旦更新完成我們會將offset的值自動累加，用來記錄有多少的股票數據被複製到了圖表中。如果計數變量i所表示的全部股票數據已經到了最後，則會跳出這個方法，否則會呼叫JavaScript的timer函數來啟動下一個循環。下面的目標是再來看每一個更新方法的執行效能。

## 執行效能實驗

更新series數據有四種技術來實現：Series.setData，Series.remove/Chart.addSeries，Point.update以及 Series.addPoint。我們會使用這四種技術會佔用的CPU時間來作為執行效能的衡量標準，一個CPU衡量工具typeperf會在背景窗口中執行。每一個方法都會使用過去一年的股票價格並設定0.5秒為每次更新的間隔時間。我們會將每次的實驗執行兩次並取其平均值，實驗會執行在下列選擇的瀏覽器上Firefox, Chrome, Internet Explorer 8和9以及Safari。雖然IE8不支持SVG，只支持VML，但因為Highcharts是支援IE8的，且IE8在目前的瀏覽器市場上仍然佔有相當的比例，因此它也是一個很重要的實驗項目。另一件事情是我們可以明顯的觀察到相同的圖表在IE8的表現明顯沒有在SVG的狀況下吸引人。

（所有的實驗都是執行在Windows 7系統下，硬體為2 GB RAM Core 2 Duo 2.66 GHz及 ATI
Radeon 4890 /1 GB，瀏覽器的版本為Firefox 16.0.1, Chrome 22.0.1129,IE9 9.0.8112, Safari 5.1.7, and IE8 8.0.760.）

下面的章節中，會解釋每一種序列的更新方式，並比較出在不同瀏覽器下的效能對比。讀者不能結論視為瀏覽器在各種情境下的綜合執行效能的標準，我們的測試只是Highcharts在各瀏覽器下執行SVG動畫的效能測試。

## 使用Series.setData來應用一個新的數據集

我們可以使用Series.setData方法來將一個新的數據集應用到一個現有的序列上。

    setData (Array data, [Boolean redraw])

數據可以是一個一維數組，一個x和y的值對（value pairs）數組，一個數據節點數組。這是所有方法中的最簡單的方式，所以它不提供任何的動畫效果，下面是我們使用setData函數的示例：

    case 'setData':
        var data = [];
        // Building up the data array in the series option
        for (i = $.histStock.offset, j = 0; i < $.histStock.data.length && j < $.histStock.numPoints; i++, j++) {
            data.push([
                $.histStock.data[i].date,
                $.histStock.data[i].price
            ]);
        }
        if (!chart.series.length) {
            // Insert the very first series
            chart.addSeries({
                name: $.histStock.name,
                data: data
            });
        } else {
            // Just update the series with
            // the new data array
            chart.series[0].setData(data, true);
        }
        break;

因為沒有動畫，所以整個重放的過程變得有些斷斷續續。下圖是setData方法在各瀏覽器中的效能表現。

[![7-10](/images/learning_highcharts/7-10.png)](/images/learning_highcharts/7-10.png)

因為setData沒有動畫效果，因此這個方法預期不會使用到很多的CPU，特別是IE8因為沒有支援SVG，VML技術執行起來比較慢也是可以解釋IE8為什麼具有較高的CPU使用率。在這些瀏覽器中，Chorme的CPU使用率最低(4.25%)，第二低是Safari(4.79%)，然後是Firefox。內存使用方面Firefox最高，而IE9則是最低，也許有點出人意料的是Safari的執行效能還好過了Firefox，也與Chrome的效能非常接近。

## 使用Series.remove和Chart.addSeries來使用新值重新插入序列中

另外我們可以使用Series.remove將全部的序列都移除，然後重建序列的選項和數據並通過Chart.addSeries來重新插入到序列中。這種方法的缺點是預設顏色和點符號的內部索引是自動增量的，就好像是我們遇到了前面的例子。我們通過指定color和marker選項來避免，下面是addSeries方法的代碼：

    case 'renewSeries':
        var data = [];
        for (i = $.histStock.offset, j = 0; i < $.histStock.data.length &&
            j < $.histStock.numPoints; i++, j++) {
            data.push([$.histStock.data[i].date,
                $.histStock.data[i].price
            ]);
        }
        // Remove all the existing series
        if (chart.series.length) {
            chart.series[0].remove();
        }
        // Re-insert a new series with new data
        chart.addSeries({
            name: $.histStock.name,
            data: data,
            color: chart.options.colors[0],
            marker: {
                symbol: chart.options.symbols[0]
            }
        });
        break;

在這個實驗中，我們每半秒刷新一次，這比預設的動畫時間還要短。因此更新序列會出現一些斷斷續續，有一些類似於setData，然而如果我們修改刷新率到3秒或更多之後，我們就會看到序列從左到右在每次跟新時依次重繪。

[![7-11](/images/learning_highcharts/7-11.png)](/images/learning_highcharts/7-11.png)

下面是使用addSeries方法在各個瀏覽器中的效能表現：

[![7-12](/images/learning_highcharts/7-12.png)](/images/learning_highcharts/7-12.png)

內存的消耗狀況與上一個測試大致相同，IE8和IE9具有最高的CPU使用率，比較不尋常的結果是Safari比Chrome和Firefox所需的CPU使用率都還要來得低。我們會在下面的章節中繼續探討。

## 使用Point.update更新數據節點

我們可以使用Point.update方法來更新指定的數據節點，update方法與setData很類似，允許使用一個單值，一個x和y的值對，或一個數據節點對象。每一個更新都可以在圖表中使用或不使用動畫來重繪。

    update ([Mixed options], [Boolean redraw], [Mixed animation])

下面是我們如何使用Point.update方法，我們遍歷所有的數據節點對象並呼叫他們的成員函數，為了節省CPU時間，我們在更新了數據節點之後設置redraw參數為false並執行Chart.redraw。

    case 'update':
        // Note: Series can be already existed
        // at start if we click 'Stop' and 'Start'
        // again
        if (!chart.series.length || !chart.series[0].points.length) {
            // Build up the first series
            var data = [];
            for (i = $.histStock.offset, j = 0; i < $.histStock.data.length &&
                j < $.histStock.numPoints; i++, j++) {
                data.push([
                    $.histStock.data[i].date,
                    $.histStock.data[i].price
                ]);
            }
            if (!chart.series.length) {
                chart.addSeries({
                    name: $.histStock.name,
                    data: data
                });
            } else {
                chart.series[0].setData(data);
            }
        } else {
            // Updating each point
            for (i = $.histStock.offset, j = 0; i < $.histStock.data.length &&
                j < $.histStock.numPoints; i++, j++) {
                chart.series[0].points[j].update([
                        $.histStock.data[i].date,
                        $.histStock.data[i].price
                    ],
                    false);
            }
            chart.redraw();
        }
        break;

Point.update會設定垂直的數據節點上的每個動畫，整體看來在更新時它類似一種波浪效果。另一個更新時的差異是x軸的標籤的動畫效果不同，在使用Chart.addSeries時軸線標籤是斜的，而使用Point.update時軸線標籤是水平的。

[![7-13](/images/learning_highcharts/7-13.png)](/images/learning_highcharts/7-13.png)

下圖是Point.update方法在不同瀏覽器中的效能表現：

[![7-14](/images/learning_highcharts/7-14.png)](/images/learning_highcharts/7-14.png)

內存使用在全部的瀏覽器中都基本保持靜態，CPU的使用率在每個瀏覽器中的分數也都低於上一個實驗，另外要特別指出的是這邊的動畫比較少。

## 使用Point.remove和Series.addPoint來移除與增加數據節點

代替為一個數據節點一個數據節點的更新，我們可以使用Point.remove移除series.data數組中的數據節點，及使用Series.addPoint為series增加回一個數據節點。

    remove ([Boolean redraw], [Mixed animation])
    addPoint (Object options, [Boolean redraw], [Boolean shift],[Mixed animation])

至於時間序列數據，我們可以單獨使用addPoint並設定shift參數為true，就可以自動移入在數據節點數組中：

    case 'addPoint':
        // Note: Series can be already existed at
        // start if we click 'Stop' and 'Start' again
        if (!chart.series.length || !chart.series[0].points.length) {
            // Build up the first series
            var data = [];
            for (i = $.histStock.offset, j = 0; i < $.histStock.data.length && j < $.histStock.numPoints; i++, j++) {
                data.push([
                    $.histStock.data[i].date,
                    $.histStock.data[i].price
                ]);
            }
            if (!chart.series.length) {
                chart.addSeries({
                    name: $.histStock.name,
                    data: data
                });
            } else {
                chart.series[0].setData(data);
            }
            // This is different, we don't redraw
            // any old points
            $.histStock.offset = i;
        } else {
            // Only updating the new data point
            for (i = $.histStock.offset, j = 0; i < $.histStock.data.length && j < $.histStock.update; i++, j++) {
                chart.series[0].addPoint([
                        $.histStock.data[i].date,
                        $.histStock.data[i].price
                    ],
                    false, true);
            }
            chart.redraw();
        }
        break;

addPoint的方式更適合與完整的顯示圖表，在這裡數據節點平滑的從右側移動到了左邊，數據節點也是同時保持水平的移動。

[![7-15](/images/learning_highcharts/7-15.png)](/images/learning_highcharts/7-15.png)

下圖是使用Point.update方法更新是的效能對比：

[![7-16](/images/learning_highcharts/7-16.png)](/images/learning_highcharts/7-16.png)

這個結果跟Point.update的結果很難比較出差異。

## 探索各瀏覽器的SVG動畫效能

目前為止我們看到了CPU隨著動畫的使用率增加，然而我們還有一個問題沒有回答就是為什麼Safari具有比Chrome和Firefox還要低的CPU使用率。一個瀏覽器基準分數測試的數字可確認一個通用的共識是Firefox和Chrome瀏覽器的效能要好過Safari。

（所有瀏覽器在SunSpider下的得分可參考http://www.webkit.org/perf/sunspider/sunspider.html ，Google V8得分可參考http://v8.googlecode.com/svn/data/benchmarks/v3/run.html ，Peacekeeper可參考http://peacekeeper.futuremark.com ）

儘管如此，在這一個使用SVG動畫特定的情況下卻是Safari比其他的瀏覽器具有更高的執行效能。在這裡我們使用Cameron Adams所寫的一個分數測試工具來呈現，它是專門使用彈跳球的方式來衡量SVG動畫效能的工具。測試工具原始是用來比較HTML5的動畫和Flash動畫直接效能所使用（HTML5 versus Flash: Animation Benchmarking http://www.themaninblue.com/writing/perspective/2010/03/22/ ）。我們使用Chrome和Safari瀏覽器，下圖為Fafari在500個點下的測試：

[![7-17](/images/learning_highcharts/7-17.png)](/images/learning_highcharts/7-17.png)

至於Chrome，這個測試的結果是102FPS。我們在這兩個瀏覽器中一直持續評估不同的數量，下圖為不同情況下綜合的效能評估：

[![7-18](/images/learning_highcharts/7-18.png)](/images/learning_highcharts/7-18.png)

我們可以看到在2000之前，Safari的幀速率都高過Chrome，超過2000之後，則Safari的效能就降到與Chrome相同了。

這導致了另一個不可避免的問題，為什麼都是基於webkit的瀏覽器允許相同的代碼卻有不同的效能表現？這很難找到造成差異的原因。然而其中的一些差異是JavaScript引擎的不同也許會有影響，webkit version版本的微小差異也許也是。另外一個SVG效能測試的結果（ http://jsperf.com ）也是顯示Safari的效能要高於Chrome。

在下一節中，我們會看到數據量對Highcharts效能的影響。

## 比較海量數據下的Highcharts效能

最後一個測試是觀察海量數據下的Highcharts執行狀況，在這個實驗中我們會根據不同的數據量下來產生離散圖來觀察顯示這些數據量所需要的時間。我們之所以選擇離散圖是因為使用他們來繪製一萬個樣本，對用戶來說就像是在圖表上繪製一個數據節點而已。

在這個分析中，我們會構建一個包含Stop timing按鈕的簡單HTML頁面，使用URL參數來設定所需要的數據量，我們使用上一個時間中的瀏覽器依次測試這些數據量。當頁面在瀏覽器中載入完成之後，數據集就被隨機創建，然後開始計時直到圖表被建立完成並顯示在熒幕上。Stop Timing按鈕被按下來衡量圖表創建和顯示的時間，如下圖為在Firefox瀏覽器顯示3000個數據點：

[![7-19](/images/learning_highcharts/7-19.png)](/images/learning_highcharts/7-19.png)

（我們不計算當圖表在Highcharts.Chart或events.load handler中通過回調處理程序的時間，因為使用海量數據時，在渲染圖表和顯示圖表之間會有一個比較大的時間差，而正常的數據大小這個時間差是微不足道的。因此依靠圖表的渲染屬性會導致比實際顯示的時間要快一些）
下面的圖表是Highcharts的不同大小的數據在各瀏覽器上的性能表現測試：

[![7-20](/images/learning_highcharts/7-20.png)](/images/learning_highcharts/7-20.png)

上圖主要需觀察的是IE8的執行效能，因為IE8使用老舊的VML，因此在1000個數據點是比IE9慢了2.5倍，15000個數據點時，慢了幾乎10倍。此外IE8因為一直彈出警告對話框，要完成這些數據節點的測試非常困難。

如我們所見，SVG的效能一般是表現在同一個方向，只有VML是幾乎快速飆升到頂。換一個說法，该曲线图表明VML和SVG技术的可扩展性之间的差异。

SVG瀏覽器中，在1000到10000個數據點之間他們都有很相近的效能範圍，大概從0.56秒到2.99秒之間。在可擴展性方面，Safari在Windows平台上沒有完成全部的實驗。超過了10000個數據點，瀏覽器的執行時間開始變的不穩定。

除了Safari，所有的瀏覽器都是隨著數據量的增加而呈現線性的效能開銷增加。Chrome開始在50000個數據點的時候出現分歧，Firefox在運行Highcharts方面更具有擴展性。三個瀏覽器都可以進一步的增加效能負載，但這也許已經超過了時間的使用需求，對一般的使用而言，不太可能有超過1000個數據節點的使用需求。然而重要的是Highcharts在所有的瀏覽器上都可以執行的很正常，並且有需要的時候也可以執行更多的資料量。

# 總結

在這一章中，我們學習了Highcharts APIs的類模型及在應用中使用他們。然後對使用不同技術更新圖表時，查看他們之間效能的差異。最終在章節的最後分析了不同瀏覽器及不同資料量的狀況下，Highcharts渲染數據節點的效能表現。

在下一節中，我們會來看Highcharts的事件處理程序，它與Highcharts APIs的相關性很高。