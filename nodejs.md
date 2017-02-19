# node.jsのバージョンアップ
以下の手順では安定版にバージョンアップした。最新版にバージョンアップするにはstableの部分をlatestにすること

* sudo npm cache clean
* sudo npm install -g n
* sudo n --stable	バージョン表示
* sudo n stable
* node -v

# nodebrew
## 概要
Nodeのバージョン切り替えツール。rubyのrbenvと同じ。

## Install
以下のコマンドでインストール可能。nodeが既にインストールされている場合は削除する。
```
brew install nodebrew
mkdir -p ~/.nodebrew/src
echo 'export PATH="$HOME/.nodebrew/current/bin:$PATH"' >> .zshenv
```

## Commands
|機能|コマンド|
|:---|:---|
|使用可能なバージョン一覧表示|nodebrew ls-remote|
|nodeのインストール|nodebrew install-binary <version>|
|インストール済みのバージョン確認|nodebrew ls|
|使用するバージョンの指定|nodebrew use <version>|



# npmについて

## 概要
nodeのパッケージマネージャ。bendlerとかmavenとかのようなもの。

## npmでローカルモジュールをインストールした場合のインストール先パスを確認する方法
npm bin

## コマンド一覧
| コマンド                      | 内容                                                     |
|-------------------------------|----------------------------------------------------------|
| npm init                      | pacakge.jsonを生成する                                   |
| npm -v                        | バージョン表示                                           |
| npm ls --depth=0              | インストール済パッケージを表示（依存パッケージは非表示） |
| npm ls                        | インストール済パッケージを表示（依存パッケージを表示）   |
| npm install <pacakge-name>    | パッケージをインストールする  npm i <package-name>でも可 |
| npm install -g <package-name> | パッケージをグローバルにインストール                     |
| npm remove <package-name>     | パッケージを削除する                                     |
| npm update                    | パッケージをまとめてインストール・アップデート           |


## 類似ツール
yarn


# webpackについて

## 概要
依存関係などを解決して成果物を出力してくれるビルドツール。
js内でのrequireを解釈できるようにする為のツール。

### 一例
例えば、main.jsとprint.jsという２つのjsファイルが存在している時、main.jsのrequireを解釈してbundle.jsというファイルに統合してくれる。

main.js
```
var print = require("./print");   // この部分がprint.jsの中身に置き換わる
print("Hello webpack");
```

print.js
```
module.exports = function(msg) {
    document.write("[print]" + msg);
};
```

以下のコマンドでコンパイルする
```
webpack main.js bundle.js
```

bundle.js（中身の１部）
```
function(module, exports, __webpack_require__) {
  var print = __webpack_require__(1);
  print("Hello webpack");
},
function(module, exports) {
  module.exports = function(msg) {
      document.write("[print]" + msg);
  };
}
```


## 使い方
### コンパイルする
```
webpack <src> <dist>
webpack main.js bundle.js
```


### コンパイルする際に自動参照する設定ファイルについて
webpack.config.jsというファイルがあれば、それを参照し、自動的にその設定ファイルを適用する。

####

```
module.exports = {
    entry: "./entry.js",    // エントリーポイントのファイル名
    output: {               // コンパイル後の出力ファイル設定
        path: __dirname,
        filename: "bundle.js"   // 出力ファイル名をbundle.jsとしている
    },
    module: {               // 各種モジュールの設定
        loaders: [          // ローダーの設定
            { test: /\.css$/, loader: "style!css"}  // 拡張子がcssというファイルをstyle_loaderでロードしている
        ]
    }
};
```



### 開発用のサーバーを立ててファイルを監視し、自動リロードする

#### インストール
```
npm install webpack-dev-server
```

#### 使い方
以下のコマンドで開発用のサーバーを起動し、localhost:8080でアクセスすることで参照することができる。

```
webpack-dev-server --hot --inline
```

## 類似ツール
browserify



# bowerについて(2016/1/23)

## 概要

javascriptのパッケージマネージャ。


## インストール
npmがインストールされていることが前提。
npm install bower -g


# tscについて
typescriptのコンパイラ。
コンパイル後はjsファイルを出力する。

## インストール
npm install -g tsc

## コンパイル方法
tsc src/ts/app.ts --outDir dist/ts


# gulpについて
タスクランナー。gruntみたいなもの

## インストール(ローカルにインストールする方法)
npm install
質問はとりあえず、全部enter
npm install gulp --save-dev


## 使い方
gulpfile.jsにタスクを定義し、gulpコマンドを叩くと定義されたタスクが実行される




# nvmについて
## 概要
Node Version Manager
node.jsのバージョン切り替えツール。 rubyに対するrvmのようなもの
※nodebrewの方が早かったのでnodebrewに切り替えた

## コマンド
|機能|コマンド|
|:-----|:---|
|バージョン確認|nvm -v|
|インストールされているバージョンの確認|nvm ls|
|使用可能なバージョンの確認|nvm ls-remote|
|該当バージョンのインストール|nvm install <version>|
|デフォルトのバージョンを指定する|nvm alias default <version>|

## 現状の問題点
起動が遅い。遅いのでnodebrewに乗り換えてみる -> 切り替えた。nodebrewの方が起動が速い




