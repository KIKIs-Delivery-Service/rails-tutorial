<!-- omit in toc -->

# Ruby on Rails Tutorial

目次

- [Ruby on Rails Tutorial](#ruby-on-rails-tutorial)
  - [概要](#概要)
  - [前提](#前提)
  - [プロジェクト一覧](#プロジェクト一覧)
    - [hello_app (第 1 章 ゼロからデプロイまで)](#hello_app-第-1-章-ゼロからデプロイまで)

## 概要

Ruby on Rails Tutorial で扱うプロジェクトをコンテナ単位で分けている。  
記載した手順を実行することでコンテナ経由で各環境にログインまたはアプリを起動することができる。  
各環境で作った成果物については別リポジトリで管理する。

## 前提
- Docker / docker-compose が使える環境であること
- VS Code を使用していること
- VS Code の拡張機能 Remote - Containers を入れていること

## プロジェクト一覧

### hello_app (第 1 章 ゼロからデプロイまで)

開発環境確認
```shell
cd hello_app
code .
# 上記実行後， [Reopen in Container] が VS Code 右下に表示されるのでクリック
# ビルドされるまで数分〜10分ほど待つ
```

サービス起動・確認
```shell
docker-compose up -d --build &&\
docker-compose exec web rails server -b 0.0.0.0
```

後片付け
```shell
docker-compose down --rmi all --volumes --remove-orphans
```
