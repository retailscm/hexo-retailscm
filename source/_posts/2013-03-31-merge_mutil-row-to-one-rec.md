title: "在Oracle中將查詢結果中的多行合併在一列中顯示"
date: 2013-03-31 20:49:57
tags:
categories:
  - Oracle
---

前一段時間在維運專案中幫同事看一個Case:

同一張訂單對應多張不同的發票號碼，為了跨通路的業務支持，又會在不同通路來查詢比對開發票的結果，所以需要在查詢的時候將多行合併到一個欄位中用逗號來分隔不同的發票號碼

去Google找了一下行列轉換的辦法，最終使用下面的SQL來成功達到預期目標。

關鍵的SQL是RTRIM (XMLAGG (XMLELEMENT ("User", INVOICE_ID || ',')).EXTRACT ('//text()'),',') as d1

User是固定的，不用修改，INVOICE_ID是發票的欄位，具體各函數的說明已經有人蠻具體的表述了，可以參考Danny's Blog [將多筆資料合併成一個欄位](http://jmh273.blog.ithome.com.tw/post/3268/121476 "將多筆資料合併成一個欄位")

需要注意的是，因為只能做同一個欄位的多行合併，所以需要將這部分的group by之後查出的結果之後再做sub query來Join產生所需要的結果。