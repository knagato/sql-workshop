FROM jupyter/scipy-notebook

USER root

RUN apt-get update
RUN apt-get install -y python3-dev libpq-dev

RUN pip install --upgrade pip \
 && pip install ipython-sql \
 && pip install psycopg2 \
 && pip install sqlalchemy \
 && pip install pgspecial \
 && pip install python-dotenv

CMD jupyter notebook --allow-root --NotebookApp.token="$NOTEBOOK_PASSWORD" /opt/work