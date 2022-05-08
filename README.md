# sql-workshop

## Setup

prepare env files

```bash
cp work/.env.sample work/.env
cp docker/postgres/.env.sample docker/postgres/.env
```

download archive.zip

```bash
unzip archive.zip -d work/input_data
```

## Start

```bash
docker-compose up -d
```

Jupyter
http://localhost:8888/

Metabase
http://localhost:3000/


## Stop

```bash
docker-compose down
```
