haml
=================

## install
gem install haml

## command

| command                       | 意味           |
|-------------------------------|----------------|
| haml (template) (output-file) | コンパイル     |
| haml -v                       | バージョン確認 |

## template reference
| tag   | 意味        |
|-------|-------------|
| !!!   | DOCTYPE生成 |
| %html | HTMLタグ    |
| %doby | BODYタグ    |

インデントで親子関係を表現することに注意

### 属性の表現方法

#### rubyっぽい記法
```
%html{:lang => "ja"}
%div{:id => "main", :class => "myClass"}
```

#### HTMLっぽい記法
```
%html(lang="ja")
%div(id="main" class="myClass")
```

#### idとclassのショートカット
```
%div#main3.myClass
```

```
<div class='myClass' id='main3'></div>
```

### コメント
#### HTMLのコメント(HTMLにも反映される）
```
/ <comment>
```

#### HTMLのコメント(HTMLに反映されない）
```
-# <comment>
```

### 改行
#### 改行をしない指定
```
%p hello
```

```
  <p>hello</p>
```

#### 改行をしない指定(haml上では構造的に記述)
```
%ul
  %li<
    item
```

```
  <ul>
    <li>item</li>
  </ul>
```

#### 親要素も含めて改行をしない指定(haml上では構造的に記述)
```
%ul
  %li<>
    item
```

```
  <ul><li>item</li></ul>
```

### フィルター
<style>タグや<script>タグなどと同等の機能を提供
以下の鱺ではcss、javascript、escapedの３種類のフィルターを使用している

haml
```
    :css
       .myStyle {
         color: red;
       }
    :javascript
      console.log("test");
      if (1) {
        console.log("test2");
      }
    :escaped
      <html>
      </html>
```

html
``` 
   <style>
       .myStyle {
         color: red;
       }
    </style>
    <script>
      console.log("test");
      if (1) {
        console.log("test2");
      }
    </script>
    &lt;html&gt;
    &lt;/html&gt;
```

### rubyの式を評価
以下のようにテンプレート内にrubyコードを埋め込むことが可能

haml
```
    %p total is #{5 * 3}
    %p= Time.now
    - x = 5
    %p= x

    / ループ
    - (1..3).each do |i|
      %p{:id => "item_#{i}"} #{i}
```

html
```
    <p>total is 15</p>
    <p>2016-04-28 10:51:54 +0900</p>
    <p>5</p>
    <!-- ループ -->
    <p id='item_1'>1</p>
    <p id='item_2'>2</p>
    <p id='item_3'>3</p>
```


### その他
#### divは省略可能
以下のように省略可能

haml
```
    #main5
      .class1
```

html
```
<div id='main5'>
  <div class='class1'></div>
</div>
```

