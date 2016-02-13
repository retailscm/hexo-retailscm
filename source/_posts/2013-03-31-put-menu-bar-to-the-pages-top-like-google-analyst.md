title: "使用Bootstrap來實現Google Analytics中的菜單(Menu bar)隨頁面上移時置頂的功能"
date: 2013-03-31 00:50:01
tags:
categories:
  - WEB2.0
  - Bootstrap
---

在設計目前手上的專案prototype時，為了符合集團UI標準的同時還可以提供User比較fancy的功能，用bootstrap來實現了將菜單(Menu bar)可以在頁面上翻時置頂到頁面頂部的功能。

在最近給Blog增加Google Analytics的時候，沒想到發現Google也使用相同的概念，暗爽一下。

下面貼出專案SOM的畫面（上面綠色的區域是集團的UI標準，因為目前展示無關，就先就先將其中的內容拿掉了）（下面兩個畫面可點開看GIF動態示意）

[![som_reservation](/images/post/som_reservation.gif)](/images/post/som_reservation.gif)

對比一下Google Analytics，有沒有發現在菜單(Menu bar)的置頂部分很相似呢？

[![google analytics](/images/post/google-analytics.gif)](/images/post/google-analytics.gif)

在SOM這個專案中利用Bootstrap來實現這個功能很簡單

    //banner
    $("body").css('padding-top', 0);
    $('#oms_menu').removeClass('navbar-fixed-top');
    $('#oms_menu').affix({offset: {top: 80}});

    $('#oms_menu').on('affixed', function () {
    $('#oms_menu').addClass('navbar-fixed-top');
    $("body").css('padding-top', 40);
    });

    $('#oms_menu').on('unaffixed', function () {
    $('#oms_menu').removeClass('navbar-fixed-top');
    $("body").css('padding-top', 0);
    });

在Bootstrap的[affix](http://twitter.github.com/bootstrap/javascript.html#affix "affix")中，提供了類似的功能，但是目前的版本中還需要對affix的js做一些修改，將affixed和unaffixed這兩個狀態開放出來。之後就可以按照上面的這段代碼在網頁下拉的過程中將菜單修改為置頂狀態了。

查找bootstrap-affix.js中的

    if (this.affixed === affix) return

在後面增加這一段：

    if (affix) {
      this.$element.trigger('unaffixed')
    } else {
      this.$element.trigger('affixed')
    }