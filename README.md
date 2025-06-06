# sample-mysql-rdb

[![Build and Deploy MkDocs per Branch](https://github.com/semba-yui/sample-mysql-rdb/actions/workflows/build-and-deploy.yml/badge.svg)](https://github.com/semba-yui/sample-mysql-rdb/actions/workflows/build-and-deploy.yml)
[![pages-build-deployment](https://github.com/semba-yui/sample-mysql-rdb/actions/workflows/pages/pages-build-deployment/badge.svg?branch=gh-pages)](https://github.com/semba-yui/sample-mysql-rdb/actions/workflows/pages/pages-build-deployment)
[![Ask DeepWiki](https://deepwiki.com/badge.svg)](https://deepwiki.com/semba-yui/sample-mysql-rdb)

## 目次

- [概要](#概要)
- [開発者向けガイドライン](#開発者向けガイドライン)
- [必須環境](#必須環境)
- [セットアップ](#セットアップ)
  - [0. mise / docker, docker-compose のインストール](#0-mise--docker-docker-compose-のインストール)
  - [1. 任意の箇所に workspace を作成する](#1-任意の箇所に-workspace-を作成する)
  - [2. リポジトリをクローンする](#2-リポジトリをクローンする)
  - [3. mise を用いて開発環境をセットアップする](#3-mise-を用いて開発環境をセットアップする)
  - [4. poetry を用いて依存関係をインストールする](#4-poetry-を用いて依存関係をインストールする)
  - [5. npm を用いて依存関係をインストールする](#5-npm-を用いて依存関係をインストールする)
- [使用方法](#使用方法)
  - [1. データベース起動 & テーブル定義書自動生成](#1-データベース起動--テーブル定義書自動生成)
  - [2. テーブル定義書の再生成](#2-テーブル定義書の再生成)
  - [3. データベースアクセス](#3-データベースアクセス)
  - [4. ER図生成](#4-er図生成)
  - [5. ER図確認](#5-er図確認)
  - [6. ドキュメント生成](#6-ドキュメント生成)
  - [7. ドキュメント確認](#7-ドキュメント確認)
  - [8. データベースの停止](#8-データベースの停止)
- [CI で生成される Excel ファイル](#ci-で生成される-excel-ファイル)
- [参考](#参考)
- [ライセンス](#ライセンス)

## 概要

このプロジェクトは、MySQL を使用したデータベース管理と開発のワークフローの例を提供します。

- Flyway を用いたマイグレーション管理
- tbls を用いたテーブル定義書自動生成
- LiamERD を用いたER図自動生成
- SQLフォーマッターによるコード整形
- Dockerを用いた開発環境
- mkdocs を用いた markdown → HTML 変換（GitHub Pages 用）
- husky を用いたコミット時の自動フォーマット

## 開発者向けガイドライン

開発者向けガイドラインは[CONTRIBUTING.md](CONTRIBUTING.md)をご参照ください。

## 必須環境

mise を用いて開発環境をセットアップします。\
以下に各種バージョンを記載しますが、最新の情報は [mise.toml](./mise.toml) を参照してください。

- [mise](https://mise.jdx.dev/)
  - Node.js: v23.11.0
  - Python: v3.13.3
  - Poetry: Latest
- Docker および Docker Compose

## セットアップ

### 0. mise / docker, docker-compose のインストール

#### mise のインストール

以下のサイトを参照してください。

- [mise Getting Started](https://mise.jdx.dev/getting-started.html)

homebrew を用いてインストールする場合は以下のコマンドを実行してください。

```bash
brew install mise
```

#### docker, docker-compose のインストール

docker desktop は商用利用できません。\
利用している OS に応じて適宜インストールしてください。

homebrew を用いてインストールする場合は以下のコマンドを実行してください。

```bash
brew install docker
brew install docker-compose
```

### 1. 任意の箇所に workspace を作成する

```bash
mkdir workspace
cd workspace
```

### 2. リポジトリをクローンする

```bash
git clone https://github.com/semba-yui/sample-mysql-rdb.git
cd sample-mysql-rdb
```

### 3. mise を用いて開発環境をセットアップする

```bash
mise install
```

### 4. poetry を用いて依存関係をインストールする

```bash
poetry install
```

### 5. npm を用いて依存関係をインストールする

```bash
npm install
```

## 使用方法

### 1. データベース起動 & テーブル定義書自動生成

データベース起動時、Flyway によるマイグレーションと、tbls によるテーブル定義書の自動生成が行われます。生成されたファイルは `docs/schema/` 配下に出力されます。

```bash
npm run db:serve
```

### 2. テーブル定義書の再生成

すでにデータベースが起動している場合、テーブル定義書のみを更新したいときは次のコマンドを実行します。

```bash
npm run db:schema
```

### 3. データベースアクセス

お使いのデータベースツールから以下の接続情報でアクセスしてください。

- DB: MySQL 8.0.41
- ユーザー名: user
- パスワード: Password
- ポート: 3306
- データベース名: sample\_rdb

### 4. ER図生成

ER 図を生成すると `docs/out/` 以下に HTML ファイルが出力されます。

```bash
npm run er:build
```

生成された `docs/schema` と `docs/out` ディレクトリは `.gitignore` で管理対象から除外されています。

### 5. ER図確認

```bash
npm run er:serve
```

### 6. ドキュメント生成

```bash
npm run docs:build
```

### 7. ドキュメント確認

```bash
npm run docs:serve
```

### 8. データベースの停止

利用が完了したら、以下のコマンドで docker コンテナを停止してください。

```bash
npm run db:down
```

## CI で生成される Excel ファイル

GitHub Actions の `Build and Deploy MkDocs per Branch` ワークフローでは、
テーブル定義書 (`excel/schema.xlsx`) を自動生成し、Artifacts にアップロードしています。
ワークフロー実行結果ページの **Artifacts** から `excel-<branch_name>` という名前でダウンロードできます。

## 参考

- [Flyway](https://flywaydb.org/)
- [tbls](https://github.com/k1LoW/tbls)
- [The future of tbls and "Documentation as Code" / phpconfuk 2023](https://speakerdeck.com/k1low/phpconfuk-2023)
- [Liam ERD](https://liambx.com/)

## ライセンス

本プロジェクトは [MIT License](LICENSE) の下で公開されています。
