


install(mac)
--------------------
```
brew update
brew install yarn
```

### コマンド一覧
| コマンド                     | 説明                                                                |
|------------------------------|---------------------------------------------------------------------|
| yarn init                    | package.jsonを作成                                                  |
| yarn add <package-name>      | package.jsonのdependenciesに追記しながらパッケージをインストール    |
| yarn add <package-name> -D   | package.jsonのdevDependenciesに追記しながらパッケージをインストール |
| yarn add <package-name> -dev | 上と同じ                                                            |
| yarn run <script-name>       | pacakge.jsonで定義したスクリプトを実行する                          |
| yarn remove <package-name>   | インストールしたパッケージを削除                                    |
| yarn install                 | pacakge.jsonを元にパッケージのインストール                          |

### パッケージのバージョンを指定する時

<package-name>@<version>で指定

webpackの1系のインストール
```
yarn add webpack@1.x
```

