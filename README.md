<!-- omit in toc -->

# Ruby on Rails Tutorial

目次

- [Ruby on Rails Tutorial](#ruby-on-rails-tutorial)
  - [概要](#概要)
  - [第 1 章 ゼロからデプロイまで](#第-1-章-ゼロからデプロイまで)
    - [環境構築](#環境構築)
    - [最初のアプリを動かしてみる](#最初のアプリを動かしてみる)

## 概要

Ruby on Rails Tutorial に沿って行った作業のログや自分用メモを残していく

## 第 1 章 ゼロからデプロイまで

### 環境構築

1. 自分用 VPC 等の作成
   - VPC 作成ワークフローを活用すればサブネットや igw などの諸々の設定を一気に自動構築してくれる
2. Cloud9 構築
   - Cloud9 コンソールの Create environment から環境作成
   - Ubuntu を選択
   - Preferences
     - Soft Tabs: 4 → 2
     - On Save, Strip Whitespace: enable
     - Mark Unused Function Arguments: enable
     - Format Code on Save: enable
     - Keep Array Indentation: enable
     - JSLint Strict Whitespace: enable
3. Rails をインストールする
   ```Shell
   # インストールを短縮するため、Ruby ドキュメントは一緒にインストールしない設定にする
   echo "gem: --no-document" >> ~/.gemrc
   # RubyGems が提供する gem コマンドを使って Rails をインストール
   gem install rails -v 6.0.4
   # Rails のバージョンを確認
   rails -v
   # bundlergem のバージョンを指定してインストール
   gem install bundler -v 2.2.17
   # Cloud9 環境でディスク容量が不足しないよう、ワークスペース環境に容量を追加
   source <(curl -sL https://cdn.learnenough.com/resize)
   # JavaScript ソフトウェアの依存関係を管理する Yarn のインストール
   source <(curl -sL https://cdn.learnenough.com/yarn_install)
   ```

### 最初のアプリを動かしてみる

1. Rails プロジェクト用の environment ディレクトリを作る
   ```Shell
   cd                    # プロジェクトのホームディレクトリに移動
   mkdir -p environment  # environmentディレクトリを作成
   cd environment/       # 作成したenvironmentディレクトリに移動
   ```
2. rails newを実行する（バージョン番号を指定）
   
3. 
