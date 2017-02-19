go langについてのメモ

# 環境変数について
GOPATH  外部ライブラリが格納されるパス

# ビルド方法と実行方法
go build [source-file]

# vim-go plugin

GOPATH環境変数を設定しておく必要がある




# 文法

## packageについて
記述するソースがどのパッケージに属しているかを明示する為に記述

```
package main
```

## import
使いたいパッケージを指定する

```
import "fmt"

func main() {
  fmt.Print("hello")
}

```


### importのエイリアスを指定する
```
import f "fmt"

func main() {
  f.Print("hello")
}
```

### ドット付き指定
ドット付きで宣言することで呼び出し時にパッケージ名を省略できる

```
import . "fmt"

func main() {
	Printf("Hello world")
}
```

### アンダースコア
通常、使用されていないパッケージを宣言するとエラーとなるが、先頭にアンダーバーを付けることでエラーを回避することができる

```
import (
  _ "fmt"
)
```


## 変数の宣言

var [変数名] [型]
```
var s string = ""
var s = ""  // 上の行と同じ(文字定数が初期値なのでstringを宣言する必要がない)
s := ""  // 上の行と同じ(:=演算子を使うとvarを指定する必要がない)
```


## if文

```
	x := 1

	// ok
	if x == 1 {
		fmt.Println("ok")
	}

	// ng
	if x == 2 {
		fmt.Println("ok")
	} else if x == 1 {
		fmt.Println("ng")
	}

	// ng
	if x == 2 {
		fmt.Println("ok")
	} else {
		fmt.Println("ng")
	}
```

## 繰り返し(for文のみ)

```
	// 0 1 2 3 4 5 6 7 8 9
	for i := 0; i < 10; i++ {
		fmt.Println(i)
	}
```


## 関数

```
func f(a int, b int) (int, int) {
	c := a + b
	d := a * b
	return c, d
}

func main() {
	c, d := f(100, 100)
	fmt.Println(c) // 200
	fmt.Println(d) // 10000
}
```



