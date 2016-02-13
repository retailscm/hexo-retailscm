title: "Yii+Oracle時使用Yii原生RBAC"
date: 2013-10-12 00:27:56
tags:
id: 444
categories:
  - Yii
---

1\. 使用下面的SQL建立Table
<pre>create table AuthItem
(
name varchar(64) not null,
type integer not null,
description varchar(4000),
bizrule varchar(4000),
data varchar(4000),
primary key (name)
);</pre>
<pre>create table AuthItemChild
(
parent varchar(64) not null,
child varchar(64) not null,
primary key (parent,child),
foreign key (parent) references AuthItem (name) on delete cascade,
foreign key (child) references AuthItem (name) on delete cascade
);</pre>
<pre>create table AuthAssignment
(
itemname varchar(64) not null,
userid varchar(64) not null,
bizrule varchar(4000),
data varchar(4000),
primary key (itemname,userid),
foreign key (itemname) references AuthItem (name) on delete cascade
);</pre>
2\. 參考另一篇貼文修改CDbConnection中的Method. &lt;&lt;[在Yii中使用Oracle時，簡化大小寫問題](http://www.retailscm.com/yii_oracle_upper_case/ "Yii中使用Oracle的代碼修改")&gt;&gt;

3.CDbAuthManager.php中$row的一些使用的變量都要修改為大寫（不改的話checkAccess的時候會出錯）

4\. 直接按照手冊使用Yii建立權限系統即可