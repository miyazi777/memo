docker でrails(bundleを別コンテナに切り出したバージョン)
-----------------------------------------------------------

## 前準備
以下の３つのファイルを同じディレクトリに用意

### Dockerfile
```
FROM ruby:2.2.2
RUN apt-get update -qq && apt-get install -y build-essential libpq-dev

RUN mkdir /myapp
WORKDIR /myapp
ADD Gemfile /myapp/Gemfile

ENV BUNDLE_GEMFILE=/myapp/Gemfile \
    BUNDLE_JOBS=2 \
    BUNDLE_PATH=/bundle

ADD . /myapp
```

### docker-compose.yml
```
db:
    image: mysql
    environment:
        MYSQL_ROOT_PASSWORD: root
    ports:
        - "3306:3306"
web:
    image: rails
    command: bundle exec rails s -p 3000 -b '0.0.0.0'
    volumes:
        - .:/myapp
    ports:
        - "3000:3000"
    links:
        - db
    volumes_from:
        - bundle
bundle:
    image: rails
    volumes:
        - /bundle
```

### Gemfile
```
source 'https://rubygems.org'
gem 'rails', '4.2.5'
```



## 手順1
docker build -t rails .

## 手順２
docker-compose run web bundle install

### nokogiriがインストールエラーとなった時の対処方法
nokogiriがインストールエラーとなることがある。そういう時はコンテナを全て削除すると大丈夫になる

## 手順３
docker-compose run web bundle exec rails new . --force --database=mysql --skip-bundle

## 手順４
作成されたプロジェクトのGemfileが上書き生成されている。
この中でtherubyracerのコメントアウトを外す

```
gem 'therubyracer', platforms: :ruby
```

## 手順５
docker-compose run web bundle install

## 手順６
config/database.ymlの設定をdockerに書き換える

```
default: &default
  adapter: mysql2
  encoding: utf8
  pool: 5
  username: root    <- rootに変更
  password: root    <- rootに変更
  host: db          <- dbに変更
```

## 手順７
docker-compose up -d

## 手順８
docker-compose run web bundle exec rake db:create

