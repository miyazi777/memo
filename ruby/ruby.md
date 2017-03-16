# コメント

# クラス
class Cell
    @x
    def initialize(x)
        @x = x
    end

    def getX()
        return @x
    end
end

c = Cell.new(10)
puts c.getX()   # 10


# 関数
def swap(x, y)
  return y, x   # 複数の値を返却
end

a = 10
b = 20
a, b = swap(a, b)
puts a, b   # 10, 20

# if文

## イコールのみ
a = 10
result = ""
if a == 10
  result = "good"
elsif a == 20
  result = "very good"
else
  result = "so so"
end

puts result # 10

## >= などを使う時
a = 10
if a > 5 then
 puts "ok"  # ok
end

# ３項演算子
x = 10 > 5 ? "ok" : "ng"
puts x  # "ok"


# for文
## 通常
for i in 1..3
    puts i  # 1, 2, 3
end

# timesによる繰り返し
10.times do |idx|
  puts idx
end

## eachによる繰り返し
ar = [1, 2, 3]
ar.each do |val|
    puts val    # 1, 2, 3
end

## eachで、index付き
ar = [3, 5, 7]
ar.each.with_index do |val, index|
    puts "#{index} : #{val}"  # 0:3, 1:5, 2:7
end

## eachで途中からループ
ar = [1,2,3]
ar.each.with_index(1) do |val, index|
  puts val
end

## eachで決まった範囲のループ
ar = [3, 5, 7, 9, 11]
(1..3).each do |idx|
   puts ar[idx] # 5, 7, 9
end

## 配列のfor文
ar = [3, 2, 1]
for i in ar
    puts i  # 1, 2, 3
end

# while文
while 条件式 do
end



# 配列
## 配列の初期化
ar = []
puts ar


### 特定の値での初期化
ar = Array.new(3, 0)
puts ar # 0, 0, 0

### ２次元配列
ar = Array.new(3){ Array.new(3) }
ar[1][1] = 1
puts ar[1][1]   # 1

## 配列の追加
ar = ['a', 'b']
ar.push('c')
puts ar     # a, b, c


## 配列の削除
ar = ['a', 'b', 'c']
ar.delete_at(1)
puts ar     # a, c

## 配列内に要素が存在しているかどうか



## 配列同士の連結
ar = ['a', 'b', 'c']
ar.concat(['d', 'e']
puts ar     # a, b, c, d, e

## 配列の抽出・検索
ar = ['a', 'b', 'c']
puts ar.include?("a") # true
puts ar.include?("x") # false

### select
{}ブロックがtrueになる要素だけを抽出

#### 2で割り切れるものだけを抽出
ar = [1,2,3,4,5]
ar2 = ar.select { |n| n % 2 == 0}
puts ar2    # 2, 4

### reject
{}ブロックがfalseになる要素だけを抽出

#### 2で割り切れないものだけを抽出
ar = [1,2,3,4,5]
ar2 = ar.reject { |n| n % 2 == 0}
puts ar2    # 1, 3, 5

### partition
{}ブロックがtrueの時とfalseの時の両方の要素を抽出した配列を作成

#### 2で割り切れる配列と割り切れない配列の２つを作る
ar = [1,2,3,4,5]
ar2 = ar.partition{ |n| n % 2 == 0}
puts ar2[0]     # 2, 4
puts "---"
puts ar2[1]     # 1, 3, 5


### find
条件に合致する最初の要素を取得する

idx = spots.find{ |e| e == no}

#### 6以上のデータを取得
ar = [4, 5, 6, 7, 8]
x = ar.find{ |e| e >= 6}
puts x  # 6

#### 6以上のデータのindexを取得
ar = [4, 5, 6, 7, 8]
x = ar.find_index{ |e| e >= 6}
puts x   # 2

### map
配列の各要素に対してブロック内の式を実行した結果を返す

#### 配列内の文字列を数値に変換
mx, my, count = lines[0].chomp.split(" ").map{|v| v.to_i}

### inject
各要素に対してブロック内の式を順次実行し、最後までのブロックの実行結果を返します。

#### 最大値を取得
x = [1, 2, 3, 4, 5].inject { |max, item| item > max ? item : max }
puts x  # 5



## 文字列操作

### 連結
a = "abc"
b = "def"
c = a + b
puts c  # abcdef

### 比較
a = "abc"
b = "abc"
if a == b
    puts "ok"   # ok
else
    puts "ng"
end


### 分割
str = "aaa,bbb,ccc"
ary = str.split(",")
puts ary    # aaa bbb ccc



# hash
hashes = { "aaa" => 1, "bbb"=> 2, "ccc"=> 3}

## 初期化
hs = {}
hs.default = 0  # ここでデフォルト値を設定 配列は使用できないことに注意！
hs["key1"] += 1
hs["key2"] += 2
puts hs     # {"key1"=>1, "key2"=>2}

## アクセス
puts hashes["bbb"]  # 2

## 繰り返しアクセス
hashes.each do |key, value|
    p "#{key}: #{value}"
end

## ハッシュキーを取得
hs = {"key1" => "value1", "key2" => "value2"}
hs.keys   # ["key1", "key2"]

## ハッシュの値からキーを取得
hs = {"key1" => "value1", "key2" => "value2"}
hs.key*("value1")   # key1

## ソート
hases.sort

### 降順ソート
???



# 文字列 - 数値の変換

## 文字列 -> 数値
str = "35"
x = str.to_i
puts x+10  # 45

## 数値 -> 文字列
a = 35
str = a.to_s
puts "[" + str + "]"   # [35]

## 正規表現で指定された文字列を抽出する
### 正規表現内で変数を使うには
scan内で正規表現内に変数を使う場合は
#{}内で変数を指定すること

### 例
stag = "<<<"
etag = ">>>"
str = "<<<abc>>>"
result = str.scan(/#{stag}(.*?)#{etag}/)
puts result


# 正規表現
## マッチ1
マッチする文字列がある時はMatchDataオブジェクトを返却
マッチする文字列がなければ、nilが返却される

str = "boxs"
if str.match(/.*(s|sh|ch|o|x)$/) != nil
    puts "ok"   # ok
end

### マッチした文字列を取得
マッチした文字列には順番に$1,$2などに格納されていく

#### 例
str = "go to 300 place"
str.match(/^go to (\d*) (\w*)/)
puts "#{$1} #{$2}" # 300 place

## マッチ2
/パターン/ =~ 文字列

マッチする文字列がある時はマッチした位置が返却される
マッチする文字列がなければ、nilが返却される

### 例
<pre>
result = /#{arg1}/ =~ str
</pre>

## 置換
マッチした最初の部分を置換文字列に置換
全部を置き換える時にはgsubを使う

### 例
str = "田中さん、こんにちは"
str.sub(/さん/, "君")
puts str #  "田中君、こんにちは"


# 日付・時刻

## 以下のライブラリ指定が必要
require("time")

## 文字列からTime型へ
t = Time.strptime("2012/5/3 09:13:23", "%Y/%m/%d %H:%M:%S")
p t # 2012-05-03 09:13:23

## Time型から文字列へ
t = Time.now
puts t.strftime('%Y/%m/%d %H:%M:%S')

## Time型の加算・減算
t = Time.strptime("2016/2/20 00:00:00", "%Y/%m/%d %H:%M:%S")
p t # 2012-05-03 09:13:23
p t + 1         # 1秒加算
p t + 3*60      # 3分加算
p t + 2*60*60   # 2時間加算


# 計算

## べき乗
puts 5 ** 2     # 25

## 四捨五入
x = 1.04
puts x.round(1)     # 1.0
x = 1.05
puts x.round(1)     # 1.1


## 絶対値
x = -100
puts x.abs()    # 100
x = 100
puts x.abs()    # 100


## 三角関数

### sin
deg = 90.0  # 90度
puts  Math.sin(deg / 180.0 * Math::PI)  # 1.0

### cos
deg = 180.0  # 180度
puts  Math.cos(deg / 180.0 * Math::PI)  # -1.0


# 半角・全角変換
"abc123".tr('0-9a-zA-Z', '０-９a-zA-Z')


Structクラスの使い方
---------------------------

Structクラスを使うことで新しいデータ構造を定義することが可能

```
Item = Struct.new(:item1, :item2)   # Itemサブクラスを作成
obj = Item.new("value1", "value2")  # Itemクラスをnewする

p obj.item1   # value1
p obj.item2   # value2
```

定数にアクセスする方法
---------------------------
以下のような定数がある時
```
class SignupToken < ActiveRecord::Base
  EXPIRE_TIME = 72
```

このようにアクセス可能
```
hours = SignupToken::EXPIRE_TIME
```



rubyの引数に関する個人的な基準
---------------------------
### 引数が3つ以下なら
通常の定義
```
def method(arg1, arg2, arg3)
```

### 引数が4つ以上なら
キーワード引数を使う
```
method(arg1: "aaa", arg2: "bbb", arg3: "ccc", arg4: "ddd")

def method(arg1*:, arg2:, arg3:, arg4:)
end
```


動的にプロパティを追加する
---------------------------
```
    def initialize(attributes = {})
      attributes.each { |name, value| instance_variable_set("@#{name}", value) }
    end
```

ブロックを渡して重複をなくす方法
---------------------------
以下に例を示す。
method_aとmethod_bがブロック使用前。
method_cがブロック使用後

```
def method_a
  puts "begin"
  puts "aaa"    # ここだけが違う
  puts "end"
  puts "---------"
end

def method_b
  puts "begin"
  puts "bbb"    # ここだけが違う
  puts "end"
  puts "---------"
end

method_a
method_b
# method_aとmethod_bは


def method_c(str)
  puts "begin"
  yield(str)    # 違うところをブロック化
  puts "end"
  puts "---------"
end

method_c("test") do |str| # このようにしてメソッドにブロックを渡せる
  puts str
end
```


### ブロックの有無を判定する
```
if block_given?
  puts "exec block"
  yield
else
  puts "not block"
end
```

動的メソッドの呼び出し
---------------------------

```
class Test
  def method_a(str)
    puts "aaa " + str
  end

  def method_b(str)
    puts "bbb " + str
  end
end

test = Test.new
method = :method_b   # ここで呼び出すメソッドを決定
test.send(method, "test") # sendメソッドで実行時に呼び出すメソッドを決定
```

慣習・お作法
---------------------------

## bool値を返却するメソッドの場合、メソッド名の後ろに?をつける


