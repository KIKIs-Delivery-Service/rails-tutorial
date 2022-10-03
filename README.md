<!-- omit in toc -->
# Ruby on Rails Tutorial

目次

- [概要](#概要)
- [前提](#前提)
- [プロジェクト一覧](#プロジェクト一覧)
- [コンテナ実行手順](#コンテナ実行手順)
  - [開発環境立ち上げ](#開発環境立ち上げ)
  - [サービス起動・確認](#サービス起動確認)
  - [Git 環境整備](#git-環境整備)
  - [Heroku本番環境デプロイ](#heroku本番環境デプロイ)
  - [Docker image 後片付け](#docker-image-後片付け)

## 概要

Ruby on Rails Tutorial で扱うプロジェクトをコンテナ単位で分けている。  
記載した手順を実行することでコンテナ経由で各環境にログインまたはアプリを起動することができる。  
各環境で作った成果物については別リポジトリで管理する。  
（Dockerfile の内容は章が進むごとに進化していく）

## 前提

- Docker / docker-compose が使える環境であること
- VS Code を使用していること
- VS Code の拡張機能 Remote - Containers を入れていること

## プロジェクト一覧

- hello_app（第 1 章 ゼロからデプロイまで）
- toy_app（第 2 章 Toy アプリケーション）

## コンテナ実行手順

※ \<project\> と書かれた部分は各プロジェクト名に置き換えること  
例）hello_app プロジェクトの場合  
   code ./\<project\>/ ⇒ code ./hello_app/

### 開発環境立ち上げ
```shell
# ディレクトリを指定してローカルの VS Code を開く
code ./<project>/

# 上記実行後， [Reopen in Container] がローカルの VS Code の右下に表示されるのでクリック
# ビルドされるまで数分〜10分ほど待つ
# 成功するとリモートの VS Code が起動する
```

### サービス起動・確認
```shell
cd hello_app
docker-compose up -d --build &&\
docker-compose exec web rails server -b 0.0.0.0
```

### Git 環境整備
```shell
#########################
# 新しく資源を作る場合
#########################
# 先に GitHub 等のブラウザ上でリポジトリを作っておく
git add -A
git commit -m "first commit"
git branch -M main
git remote add origin <作成したリポジトリへのリンク>
git push --set-upstream origin main

#########################
# 既存の資源を持ってくる場合
#########################
# ※下記記コマンド実行中、一時的にリモートの VS Code 内の Explorer が表示できなくなるが、
#  一通りのコマンド実行完了後に再読み込みアイコンをクリックすると治る
cd /rails-tutorial/environment/
rm -rf <project>
git clone https://github.com/KIKIs-Delivery-Service/rails-tutorial_<project>.git
mv {rails-tutorial_,}<project>
# Git で追跡されていることを確認
cd <project>
git status
# Gem を更新しておく
bundle _2.2.17_ install --path vendor/bundle
```

### Heroku本番環境デプロイ
```shell
# Heroku ログイン
heroku -v && heroku login --interactive
# Heroku に本番環境作成
heroku create
# Heroku にデプロイ
heroku buildpacks:set heroku/ruby
heroku buildpacks:add --index 1 heroku/nodejs
git push heroku main
# デプロイの反映をプラウザから確認
heroku open

# heroku 環境削除 (アプリ名は heroku apps コマンドで確認)
heroku apps:destroy --app アプリ名 --confirm アプリ名
git remote rm heroku
```

### Docker image 後片付け
```shell
# 無名 image をすべて削除
docker rmi $(docker images -f "dangling=true" -q)
# 前回作成した image 削除
cd <project>
docker-compose down --rmi all --volumes --remove-orphans
```
