# SQL Workshop

パブリックデータとして公開されているデータセットを題材として、データ分析領域で用いられているSQLについて学ぶワークショップのためのレポジトリです。

## Setup

本ワークショップでは、Dockerを用いてローカル環境にPostgreSQL, Jupyter Notebook, Metabaseを構築し、データの格納や分析、可視化をおこないます。

[PostgreSQL](https://www.postgresql.org/) は、代表的なオープンソースのリレーショナルデータベース管理システム (RDBMS) のひとつです。標準的なSQLに準拠し、分析のための構文も豊富に実装されているため、データ分析のためのSQL基盤としてもよく利用されています。

[Jupyter Notebook](https://jupyter.org/) は、ブラウザ上で利用可能なデータ分析のためのプログラミング実行環境です。PostgreSQLの拡張機能を導入することで、直接SQLやPostgreSQLコマンドの実行をインタラクティブにおこない、結果を保存したり共有したりすることができます。

[Metabase](https://www.metabase.com/) は、オープンソースのデータ可視化・BIツールのひとつです。PostgreSQLを対象データベースとして、SQLの補完機能や、出力データの可視化機能を備えています。簡単なデータ集計や加工であれば、SQLを使わずGUI上の操作でも実現可能です。

### Requirements

事前にDockerおよびDocker Composeをインストールしてください。Docker Desktopをインストールすると、Docker Composeも同時にインストールされます。

- [Docker](https://docs.docker.com/get-docker/)
- [Docker Desktop](https://www.docker.com/products/docker-desktop)

#### Windows

- [Docker Desktop for Windows](https://docs.docker.com/docker-for-windows/install/)

#### Mac

- [Docker Desktop for Mac](https://docs.docker.com/docker-for-mac/install/)

#### Linux

Docker Desktopを用いない場合、Docker EngineおよびDocker Composeをそれぞれインストールしてください。

- [Docker Engine](https://docs.docker.com/engine/install/)
- [Docker Compose](https://docs.docker.com/compose/install/)

Linuxの場合、Docker公式のConvenience Scriptを用いてインストールすることもできます。

```bash
$ curl -fsSL https://get.docker.com -o get-docker.sh
$ sh get-docker.sh
```

### Prepare

まず、本レポジトリを作業用環境にダウンロードします。

```bash
$ git clone git@github.com:knagato/sql-workshop.git
# or
# git clone https://github.com/knagato/sql-workshop.git

$ cd sql-workshop
```

Jupyter Notebook と PostgreSQL にアクセスするためのパスワードを設定します。
以下のコマンドで、`.env.sample`ファイルをコピーして`.env`ファイルを作成します。

```bash
$ cp work/.env.sample work/.env
```

`.env`ファイルをエディタで開き、`NOTEBOOK_PASSWORD`と`POSTGRES_PASSWORD`に任意のパスワードを記入します。

```bash
$ vi work/.env
```

work/.env
```
NOTEBOOK_PASSWORD=<Your Password for Jupyter Notebook>
POSTGRES_PASSWORD=<Your Password for Postgres User>
```

### Data Download

[kaggle.conの公式サイト](https://www.kaggle.com/)にアクセスし、アカウント登録またはログインをおこなっておきます。

次に、下記リストのデータセットのページにアクセスし、`Download`ボタンをクリックしてデータセットをダウンロードします。ダウンロードしたデータセット(`archive.zip`)は、各データセットに対応するディレクトリに配置してください。

| データセット | 保存ディレクトリ |
| --- | --- |
| [Anime Recommendation Database 2020](https://www.kaggle.com/datasets/hernan4444/anime-recommendation-database-2020) | `work/input/anime` |
| [eCommerce behavior data from multi category store](https://www.kaggle.com/datasets/mkechinov/ecommerce-behavior-data-from-multi-category-store) | `work/input/ecommerce` |


下記コマンドで、`work/input`ディレクトリ配下に保存したデータセットを解凍します。

```bash
$ unzip work/input/anime/archive.zip -d work/input/anime/
$ unzip work/input/ecommerce/archive.zip -d work/input/ecommerce/

$ ls work/input/anime
 README.md   anime.csv   anime_with_synopsis.csv   animelist.csv   archive.zip  'html folder'   rating_complete.csv   watching_status.csv

$ ls work/input/ecommerce
 2019-Nov.csv  2019-Oct.csv  README.md  archive.zip
```

---

※ 解凍したCSVファイルのうち、anime/animelist.csv(1億行, 1.9GB), anime/rating_complete.csv(5700万行, 781MB), ecommerce/2019-Nov.csv(6.7億行, 8.4GB), ecommerce/2019-Oct.csv(4.2億行, 5.3GB) はサイズが大きいため、環境によっては後のデータベースへのインポートができなかったり、SQLの実行に時間がかかったりする可能性があります。そのような場合は、下記のようなコマンドを用いてデータを削減して利用しても構いません。

```bash
$ cd work/input/anime

# animelist.csvをバックアップ
$ mv animelist.csv animelist.csv.bk

# animelist.csv.bkの行数を確認（約1億行）
$ wc -l animelist.csv.bk
109224748 animelist.csv.bk

# animelist.csv.bkの先頭1000万行だけをanimelist.csvとして切り出し
$ head -n 10000000 animelist.csv.bk > animelist.csv

# animelist.csvの行数を確認
$ wc -l animelist.csv
10000000 animelist.csv
```

## Start

docker-compose.ymlファイルのあるルートディレクトリで、下記コマンドを実行しDockerコンテナを起動します。

```bash
$ docker-compose up -d
```

### Jupyter Notebook

起動が完了したら、ブラウザで [http://localhost:8888/](http://localhost:8888/) にアクセスし、Jupyter Notebookを開きます。初回アクセス時は、上記手順で設定した`NOTEBOOK_PASSWORD`の入力を求められます。

ログインが完了したら、[00_Setup](/work/00_Setup)フォルダ内のノートを開き、手順に従ってデータベースのセットアップをおこなってください。

[01_Tutorial](/work/01_Tutorial)フォルダには、セットアップしたデータベースに対してSQLで基本的な操作をおこなうためのサンプルが含まれています。

[02_Exercise](/work/02_Exercise)フォルダには、各ワークショップの日付ごとの課題ノートを用意しています。

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
$ docker-compose down
```
### Remove

すべてのデータセットをインポートすると、PostgreSQLが使用するデータサイズは28GB超になります。

ディスクサイズが逼迫した場合は、下記の手順でデータの一括削除が可能です。

使用中ディスクサイズの確認：

```bash
$ docker system df -v

Images space usage:

REPOSITORY                                                       TAG                      IMAGE ID       CREATED         SIZE      SHARED SIZE   UNIQUE SIZE   CONTAINERS
sql-workshop_jupyter                                             latest                   5285597c4b09   3 days ago      3.081GB   3.08GB        1.416MB       1
...

Local Volumes space usage:
VOLUME NAME                                                        LINKS     SIZE
sql-workshop_postgres-data                                         1         28.66GB
sql-workshop_metabase-data                                         1         536.8kB
...
```

コンテナの削除：

```bash
$ docker-compose rm
```

ボリュームの削除：

```bash
$ docker volume rm sql-workshop_postgres-data
$ docker volume rm sql-workshop_metabase-data
```

イメージの削除：

```bash
$ docker image rm sql-workshop_jupyter
```
