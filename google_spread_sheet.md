## 商品ごとの集計
=QUERY(A:C, "SELECT B,COUNT(C) GROUP BY B")

## 日毎・商品ごとの集計
=QUERY(A:C, "SELECT A,B,COUNT(C) GROUP BY A,B")

## 日付をyyyy-mm形式に変換
=QUERY(A:C, "SELECT A FORMAT A 'YYYY-MM'")

## 日付形式のセルからyyy/MM/ww(月の第何週目)を取得する
=TEXT(A2,"YYYY/MM") & "-" & TEXT(WEEKNUM(A2) - WEEKNUM(DATE(YEAR(A2), MONTH(A2), 1)) + 1, "#")



