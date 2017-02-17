
# リモートリポジトリの確認
git remote -v

# 各種設定の確認
git config --list

# githubに新しいリポジトリを作成し、ファイルをコミットする手順

* github上で新しいリポジトリを作成
* 管理したいリソースのあるディレクトリへ移動
* git init
* git add .
* git commit -m "first commit"
* git remote add <branch> <url>
* git push -u origin master

# ブランチの確認
git branch

# ログの確認
git log
git log --oneline   各コミットを１行ずつで表示する
git log --oneline --graph --all ローカルの全ブランチのログをグラフ表示

# gitで編集したファイルの一覧を表示する
git status

# ステージングにあげる
git add ファイル名

# コミット
git commit -m "コメント"

# リモートブランチへ反映
git push origin xxx:zzz
originはリポジトリ名
xxxはローカルブランチ名
zzzはリモートブランチ名

# githubでコミットするたびに毎回アカウントとパスワードを聞かれる時の対応

## 原因
httpsで通信していたことが原因

## 対応
現状はgitプロトコルでの通信する必要があるらしい。
以下の手順でgitプロトコルに変更にし、アカウントを聞かれないようにすることができる。

### 現在のリモートの接続先を確認
git config remote.origin.url

https://github.com/[username]/[resository]と表示されたのでhttpsでの通信だった

### gitプロトコルに変更
git remote set-url origin git@github.com:[user-name]/[resository-name]

### 通信先を再確認
git config remote.origin.url

git@github.com:miyazi777/dotfiles.git




# リポジトリ

## リポジトリの確認
git remote -v

# ブランチを作成してマージ
## ブランチの作成
git branch <brandh-name>

## ブランチの切り替え
git checkout <branch-name>


## ブランチをリモートに反映
git commit -m ""
git push origin <branch-name>

## マスターに切り替えてマージ
git checkout master
git marge <branch-name>

## マスターをリモートに反映
git push origin master

## ローカルブランチの削除
git branch -d <branch-name>

## リモートブランチの削除
git push origin :<branch-name>

example
```
git push origin :bugfix/20170216_order_bug
```


## ブランチ名の変更
git branch -m <old-name> <new-name>
### 名称変更したいブランチにいる時
git branch -m <new-name>



# その他how to
## 指定文字列を含んだファイルを検索する
git grep -e "検索したい文字列"

## ファーストコミットをrebaseの対象にしたい
git rebase -i --root

## 特定のファイルの履歴を表示
git log -p <file-name>

## gitでリモートとローカルが一致していない時に強制的に一致させる方法
下はmasterブランチの場合の例であることに注意
```
git fetch origin
git reset --hard origin/master
```
## ファイルの変更履歴を表示
g diff --stat HEAD~1 HEAD


## 直前のaddを取り消し
g reset HEAD

## 直前のコミットを取り消し
g reset --soft HEAD~1

## リモートオートブランチを取得
git fetch
git checkout -b feature/issue_xxx origin/feature/issue_xxx

## ローカルブランチをリモートブランチで上書きする
git fetch origin/feature/issue_915
git reset --hard origin/feature/issue_915

## pullとrebaseを同時に行う

hoge branchにいて、developmentブランチの先頭からrebaseしたい時は以下のコマンドでdevelopmentのpullとrebaseを同時に行う
```
git pull --rebase origin development
```

## cherry-pickの実例

* チェリーピックを適用したいブランチに移動する
* git log --onelineでログを表示
* git cherry-pick <commit-hash>でコミットを適用

### 実例
feature/re_prepressの３つのコミットをhotfix/prepressに適用

feature/re_prepressのログ
```
a6a57e7 ステータス更新時の不要なコードを削除
6f8aa40 ステータスチェックのリファクタリング
330839e 再プリプレス対応
c27549a Fix payment record counter
4104abb Merge branch 'master' into development
```

```
g checkout hotfix/prepress  # hotfixブランチへ移動
g cherry-pick 330839e
g cherry-pick 6f8aa40
g cherry-pick a6a57e7
```

## pecoとの連携
例えば、下のようなコマンドでブランチを切り替えることができる。
```
git checkout B
```

.zshrc
```
alias -g B='`git branch | peco --prompt "GIT BRANCH>" | head -n 1 | sed -e "s/^\*\s*//g"`'
```

### 参考サイト
http://qiita.com/Kuniwak/items/b711d6c3e402dfd9356b


## サブコマンドのalias
以下のようにサブコマンドのエイリアスを設定することが可能

```
g co hoge
```

.zshrc
```
alias g='git'
git config --global alias.co 'checkout'
```



