title: "在Yii中使用Oracle時，簡化大小寫問題"
date: 2013-10-12 00:18:32
tags:
id: 442
categories:
  - Yii
---

修改Yii的CDbConnection.php這個文件：

public function quoteTableName($name)
{
return $this-&gt;getSchema()-&gt;quoteTableName(<span style="color: #ff0000;">strtoupper(</span>$name<span style="color: #ff0000;">)</span>);
}

public function quoteColumnName($name)
{
return $this-&gt;getSchema()-&gt;quoteColumnName(<span style="color: #ff0000;">strtoupper(</span>$name<span style="color: #ff0000;">)</span>);
}

紅色部分為新增內容，這樣Yii通過AR自動產生的SQL就會表名與欄位名都是大寫了，需要注意的是不要用引號去限制表名或欄位名變小寫哦。