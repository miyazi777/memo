# swift
## for 

### インデックス付きループ
<pre>
for i in 0...2 {
  print(i) // 0, 1, 2
}
</pre>

### 変数なしループ
<pre>
var x = 0
for _ in 0...2 {
  x += 1
  print(x)  // 1, 2, 3
}
</pre>

### 最終地を含まないループ
<pre>
for i in 0..<2 {
  print(i) // 0, 1
}
</pre>

### その他
continue    // 後続を処理しない
break       // ループを抜ける


## NSMutableDictionary

### 値の設定
let dic:NSMutableDictionary = ["life":"5"]


## Optional

nilが代入される可能性がある変数を定義する場合は以下のように変数名の後ろに?をつける
<pre>
let name: String?
</pre>

?が付けられた変数を参照する場合には!をつける必要がある
<pre>
let mas = "hello " + name!
</pre>

?をつけることで変数はOptional型となることに注意。
下の例では変数aはOptional型でString型がラップされている状態。
!をつけることでラップされた元の方で扱うことができる
<pre>
let a: String? = "test"
print(a)    // Optional("test")
print(a!)   // test
</pre>

## 関数
### 引数なし
<pre>
hello() {
    print("hello")
}
hello()
</pre>

### 引数付き
第二引数のアンダースコアは外部引数名を省略する為のもの
<pre>
func hello(last: String, _ first: String) {
    print("hello " + last + " " + first)
}
hello("takeshi", "miyajima")
</pre>

### 外部引数名指定付き
<pre>
func hello2(last last: String, first: String) {
    print("hello " + last + " " + first)
}
hello2(last:"takeshi", first:"miyazi")
</pre>

### デフォルト値
<pre>
func hello3(name: String = "xxx") {
    print("hello " + name)
}
hello3()        // hello xxx
hello3("miya")  // hello miya
</pre>

### 返り値の基本
func sum(a:Int, _ b:Int) -> Int {
    return a + b
}
print(sum(5, 12))   // 17

### タプルを使って２つの返り値
func swap(a:Int, _ b:Int) -> (Int, Int) {
    return (b, a)
}
print(swap(10, 20)) // (20, 10)

### 値渡し
func f(var a:Int) {
    a = 20
    print(a)    // 20
}
var a = 5
f(a)
print(a)    // 5

### 参照渡し(基本的には使わないと思われる）
メソッド側の引数の前にinoutを指定し、呼び出し側の変数に&をつける
func f2(inout a:Int) {
    a = 20
    print(a)    // 20
}
var a2 = 5
f2(&a2)
print(a2)    // 20


# 列挙型

## 列挙側の基本
<pre>
enum Result {
    case Success
    case Error
}

var r: Result

r = Result.Success
r = .Success
</pre>

## 列挙側に型も指定
<pre>
enum Result2: Int {
    case Success = 0
    case Error = 1
}

var r2: Result2
r2 = .Success
print(r2)           // Success
print(r2.rawValue)  // 0
</pre>


## 列挙型にメソッド定義
<pre>
enum Result3: Int {
    case Success = 0
    case Error = 1
    func getMessage() -> String {
        switch self {
        case .Success:
            return "OK"
        case .Error:
            return "NG"
        }
    }
}

var r3: Result3
r3 = .Error
print(r3)   // Error
print(r3.getMessage()) //  NG
</pre>


# クラスの定義
<pre>
class User {
    var name: String
    var score: Int = 0
    init(name: String) {
        self.name = name
    }
    func upgrade() {
        score++
    }
}

let tom = User(name: "Tom")
tom.name    // Tom
tom.score   // 0
tom.upgrade()
tom.score   // 1
</pre>

## クラスの継承
<pre>
class User {
    var name: String
    var score: Int = 0
    init(name: String) {
        self.name = name
    }
    func upgrade() {
        score++
    }
    func upgrade(add: Int) {
        score += add
    }
}

class AdminUser: User { // Userを継承しているメソッド
    func reset() {
       score = 0
    }
    override func upgrade() {   // メソッドのオーバーライド
        super.upgrade(3)        // 親クラスのメソッドの呼び出し
    }
}

var bob = AdminUser(name: "Bob")
bob.upgrade()
bob.score   // 3
bob.reset()
bob.score   // 0
</pre>

# プロトコル
## 概要
Javaでいう、Interface的なもので
プロトコルで定義された変数とメソッドの実装を
継承先に強制させることができる

## example
<pre>
protocol Student {
    var studentId: String {get set}
    func study()
}

class User: Student {
    var name: String
    var score: Int = 0
    var studentId: String = "hoge"
    init(name: String) {
        self.name = name
    }
    func study() {
        print("studying...")
    }
    func upgrade() {
        score++
    }
}
</pre>


# プロパティ

## 動的なプロパティ(get, set)
プロパティにget、set時の動作を指定することで
動的なプロパティを定義することが可能。

<pre>
class User {
    var score: Int = 0
    var level: Int {
        get {
            return Int(score / 10)  // 参照される時の処理
        }
        set {
            score = Int(newValue * 10)  // 値がセットされる時の処理 newValueで新しい値を参照可能
        }
    }
}

var tom  = User()
tom.level   //
tom.score = 52
tom.level   // 5
tom.level = 8
tom.score   // 80
</pre>


## プロパティの監視(willSet, didSet)
プロパティの変化した時に処理を行うことも可能
willSet 値が変化する前に処理可能。newValueで新しい値を参照できる
didSet 値が変化した後に処理可能。oldValueで元の値を参照できる

<pre>
class User {
    var score: Int = 0 {
        // 値が変更される前の処理
        willSet {
            print("willSet \(score) -> \(newValue)") // willSet 0 -> 32
        }
        // 値が変更された後の処理
        didSet {
            print("didSet \(oldValue) -> \(score)") // didSet 0 -> 32
        }
    }
}

var tom  = User()
tom.score = 32
</pre>


# optinal chaining
連続的に参照する際に途中にOptinal型のデータが挟まった時に例外が発生する可能性があるが、それを安全に対応する方法
nilの可能性があるプロパティなどの後ろに?を入れることで以降の評価を行わない

<pre>
class User {
    var blog: Blog?
}

class Blog {
    var title = "title"
}

var tom = User()
tom.blog = Blog()
tom.blog?.title     // title    

// nil場合
var bob = User()
bob.blog?.title     // blogのところで評価が止まる為、titleは評価されない

// よくある書き方
if let t = tom.blog?.title {
    print("title : " + t)   // titleがあるので実行される
}
if let b = bob.blog?.title {
    print("title = " + b)   // titleがないので実行されない
}
</pre>





# type casting

## ダウンキャストとは
継承元のクラスを継承先のクラス（派生クラス）に変換すること

<pre>
class Animal{}
class Cat: Animal{}

let tama:Animal = Cat()
let cat = tama as! Cat
</pre>

## サンプル
<pre>
class User {
    var userType: String
    init() {
        self.userType = "User"
    }
    
    func getUserType() -> String {
        return self.userType
    }
}

class AdminUser: User {
    override init() {
        super.init()
        self.userType = "Admin"
    }
    func getAdminUserType() -> String {
        return "* " + self.userType
    }
}

let user1 = User()
let user2 = AdminUser()

let users:[User] = [user1, user2]
//let users:[AnyObject] = [user1, user2]  // AnyObjectでも可

for user in users {
    if user is AdminUser {  // 型判定
        // AdminUser型ならこちらの処理を実施
        let u = user as! AdminUser     // User型をAdminUser型へ変換（ダウンキャスト）
        print(u.getAdminUserType())    // * Admin
    } else {
        // AdminUser型でないならそのままUserTypeを表示
        print(user.getUserType())      // User
    }
}
</pre>


# 構造体
<pre>
struct UserStruct {
    var name: String
    init(name:String) {
        self.name = name
    }
    func getName()-> String {
        return self.name
    }
}

let user1 = UserStruct(name:"User1")
print(user1.getName())  // User1
</pre>
print(user1.name)       // User1
</pre>


# 拡張

## 概要
既存のデータ型をさらに拡張することが可能

## サンプル
<pre>
extension String {
    var size: Int {
        return self.characters.count
    }
    func dummy() -> String {
        return "dummy"
    }
}

var s: String = "hoge"
s.size      // 7
s.dummy()   // dummy
</pre>

# ジェネリクス
型を抽象化する。

## サンプル

このサンプルでは配列を作成するメソッドを定義しており、
このメソッドはどのような型でも対応できるようにしている。
Tが抽象化された型情報。

<pre>
func getArray<T>(item: T, _ count: Int) -> [T] {
    var result = [T]()
    for _ in 0..<count {
        result.append(item)
    }
    return result
}

getArray(5, 3)       // [5, 5, 5]
getArray("hoge", 2)  // ["hoge", "hoge"]
</pre>


# タプル
２つの値をまとめて扱うことのできる型

## サンプル
<pre>
// 基本的な形
let error = (404, "not found")
print(error.0)     // 404
print(error.1)     // not found

// keyをつけたタプル
let error2 = (code:404, msg:"not found")
print(error2.code)  // 404
print(error2.msg)   // not found

// タプルの代入
let error3 = (404, "not found")
let (code, msg) = error3
print(code)     // 404
print(msg)      // not found

// 値の破棄
let error4 = (404, "not found")
let (code2, _ ) = error4
print(code2)     // 404
print(msg2)      // error
</pre>

# 配列
// 宣言
var colors: [String] = ["blue", "pink"]

// 参照
print(colors[0])   // blue
print(colors[1])   // pink

print(colors.first) // blue
print(colors.last)  // pink


// 要素の数を取得
colors.count    // 2

// 空かどうか判定
colors.isEmpty  // false

// 要素追加(最後尾)
colors.append("green")  // "blue", "pink", "green"
colors.insert("gray", atIndex:1)    // "blue", "gray", "pink", "green"

// 要素の削除
colors.removeLast() // "blue", "gray", "pink"
colors.removeAtIndex(1) // "blue", "pink"
colors.removeAll()  // delete all

// 空の配列宣言
var emptyArray = [String]()

// 配列のソート
var other = [1, 2, 3, 4, 5, 6, 7]
let reverse = other.reverse()   // 逆順の配列を返却　7, 6, 5, 4, 3, 2, 1
other.sort({$0 > $1})   // 自分自信をソート
print(other)    // 7, 6, 5, 4, 3, 2, 1

var other2 = [3, 2, 1]
let ary = other2.sort({$0 < $1}) // ソートした配列を返却（自分自信はソートされない)
print(ary)      // 1, 2, 3
print(other2)   // 3, 2, 1


# 辞書
// 辞書宣言
users: [String: Int] = [
    "user1": 500,
    "user2": 800
]

// 空の辞書宣言
var emptyDictionary = [String: Int]()

// 参照
users["user1"]    // 500

// 追加
users["user3"] = 900
print(users)    // "user1":500, "user2":800, "user3":900

// 更新
users["user2"] = 300
print(users)    // "user1":500, "user2":300, "user3":900

// 要素の削除
users.removeValueForKey("user1")
print(users)    // "user2":300, "user3":900

// 要素数
print(users.count)  // 2

// 空かどうか
print(users.isEmpty)    // false

// keyの配列を取得
let keys = Array(users.keys)
print(keys) // user2, user3

// valueの配列
let values = Array(users.values)
print(values)   // 300, 900



// if制御構造
let score = 82

if score > 80 {
    print("Great")      // "Great"
} else if score > 60 {
    print("Good")
} else {
    print("so so ...")
}

// NOT演算子
// !=

// 論理演算子
// &&   AND
// ||   OR

// 三項演算子
var result = score > 80 ? "Great" : "so so..."
print(result)   // "Great"

## NSString -> Stringへの変換方法
<pre>
let text = NSString()
let x = text as String
</pre>

## String -> NSStringへの変換
<pre>
let str = "hello"
let str2 = str as NSString
</pre>

