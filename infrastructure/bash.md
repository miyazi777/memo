

# シェルスクリプト(bash)に関するメモ

## １行目に必ず記述もの（実行シェルの指定）
<pre>
#!/bin/bash
</pre>

# exit 0
実行終了。引数は終了コード

# 変数
## 変数の定義
hoge = "HOGE"

## 文字列中に変数を参照 
hoge="HOGE"
fuga="FUGA"

hogefuga="${hoge}${suga}"   
echo $hogefuga  // HOGEFUGA


# 条件分岐



