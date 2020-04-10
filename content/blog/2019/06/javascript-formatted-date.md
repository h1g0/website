---
title: "JavascriptでYYYY/MM/DDみたいな形式で出力＋日本語の曜日を手軽に出力"
date: 2019-06-20T00:00:00+09:00
draft: false
categories: [ "Javascript" ]
tags: [ "Javascript" ]
---

月や日付を2ケタに揃えて出したり、曜日を漢字1字で出したりする。
割とよく使うコードなのでsnippet的に。

```javascript
var today = new Date();

var year = (today.getFullYear()).toString();
var month = ("0" + (today.getMonth()+1).toString()).slice(-2);
var date = ("0" + today.getDate().toString()).slice(-2);

var dayOfWeek = "日月火水木金土".slice(today.getDay(), today.getDay() + 1);

console.log( year + "年" + month + "月" + date + "日（" + dayOfWeek + "）" );
```

のようにしてやると

```raw
2019年06月20日（木）
```

のように出力される。


~~ドメイン期限切れのアラートメールが届くまで、このブログの存在を完全に忘れていた。完全な三日坊主である。~~