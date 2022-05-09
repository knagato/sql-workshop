# SQL Workshop

パブリックデータとして公開されているデータセットを題材として、データ分析領域で用いられているSQLについて学ぶワークショップのためのレポジトリです。

サンプルのデータセットには、kaggle.comの [Anime Recommendation Database 2020](https://www.kaggle.com/datasets/hernan4444/anime-recommendation-database-2020) を使用します。
このデータセットには、世界最大級のアニメ・マンガコミュニティである [MyAnimeList](https://myanimelist.net/) から取得された、17,562件のアニメデータと、325,770人のユーザによるアニメの評価データが含まれています。

このデータセットを用いて、SQLによるデータ加工やデータ分析手法を学び、基本的なレコメンドシステムの構築を実践します。

## Setup

本ワークショップでは、Dockerを用いてローカル環境にPostgreSQL, Jupyter Notebook, Metabaseを構築し、データの格納や分析、可視化をおこないます。

[PostgreSQL](https://www.postgresql.org/) は、代表的なオープンソースのリレーショナルデータベース管理システム (RDBMS) のひとつです。標準的なSQLに準拠し、分析のための構文も豊富に実装されているため、データ分析のためのSQL基盤としてもよく利用されています。

[Jupyter Notebook](https://jupyter.org/) は、ブラウザ上で利用可能なデータ分析のためのプログラミング実行環境です。PostgreSQLの拡張機能を導入することで、直接SQLやPostgreSQLコマンドの実行をインタラクティブにおこない、結果を保存したり共有したりすることができます。

[Metabase](https://www.metabase.com/) は、オープンソースのデータ可視化・BIツールのひとつです。PostgreSQLを対象データベースとして、SQLの補完機能や、出力データの可視化機能を備えています。簡単なデータ集計や加工であれば、SQLを使わずGUI上の操作でも実現可能です。

### Prepare

まず、本レポジトリを作業用環境にダウンロードします。

```bash
git clone git@github.com:knagato/sql-workshop.git
cd sql-workshop
```

Jupyter Notebook と PostgreSQL にアクセスするためのパスワードを設定します。
以下のコマンドで、`.env.sample`ファイルをコピーして`.env`ファイルを作成します。

```bash
cp work/.env.sample work/.env
```

`.env`ファイルをエディタで開き、`NOTEBOOK_PASSWORD`と`POSTGRES_PASSWORD`に任意のパスワードを記入します。

```bash
vi work/.env
```

work/.env
```
NOTEBOOK_PASSWORD=<Your Password for Jupyter Notebook>
POSTGRES_PASSWORD=<Your Password for Postgres User>
```

kaggle.comの [Anime Recommendation Database 2020](https://www.kaggle.com/datasets/hernan4444/anime-recommendation-database-2020) ページにブラウザでアクセスし、アカウント登録またはログインをおこない「Download」ボタンから`archive.zip`ファイルを作業ディレクトリにダウンロードします。

下記コマンドで、`work/input_data`ディレクトリにデータセットのcsvファイル群を解凍します。

```bash
unzip archive.zip -d work/input_data
```

## Start

下記コマンドで、Dockerコンテナを起動します。

```bash
docker-compose up -d
```

### Jupyter Notebook

起動が完了したら、ブラウザで [http://localhost:8888/](http://localhost:8888/) にアクセスし、Jupyter Notebookを開きます。初回アクセス時は、上記手順で設定した`NOTEBOOK_PASSWORD`の入力を求められます。

ログインが完了したら、[00_Setup](/work/00_Setup)フォルダ内のノートを開き、手順に従ってデータベースのセットアップをおこなってください。

[01_Tutorial](/work/01_Tutorial)フォルダには、セットアップしたデータベースに対してSQLで基本的な操作をおこなうためのサンプルが含まれています。

[02_Exercise](/work/02_Exercise)フォルダには、3種類のレコメンドを実装する課題と、課題提出用のテンプレートが含まれています。

### Metabase

Jupyter Notebookでデータベースのセットアップを終えたあと、[http://localhost:3000/](http://localhost:3000/) にアクセスすると、Metabaseの機能を用いて、より簡単にSQLの実行が可能です。

初回起動時には、Metabaseのローカルアカウント設定と接続先データベース設定が求められるので、画面の指示に従って設定を完了してください。

データベースの設定に必要な項目は下記のとおりです。

| 項目 | 値 |
|------|----|
| データベースのタイプ | PostgreSQL |
| 名前 | （任意の名前） |
| ホスト | postgres |
| ポート | 5432 |
| データベース名 | postgres |
| ユーザー名 | postgres |
| パスワード | （上記手順で設定したパスワード） |

## Stop

作業を終了するときは、下記コマンドでDockerコンテナを停止してください。

```bash
docker-compose down
```
