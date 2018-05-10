# we use vanilla Ubuntu 16.04 as the base layer
FROM ubuntu:16.04

MAINTAINER Friedrich Kauder <fkauder@gmail.com>

# setup variables
ENV PROJECT_NAME=antrags_analyse_tool
ENV PROJECT_HOME=/opt/$PROJECT_NAME
ENV PROJECT_URL=localhost
ENV PROJECT_PORT=80
ENV PROJECT_PASSWORD=thispasswordisverylongandthereforeverysafe

# setup application
RUN apt-get update
RUN apt-get install -y git less vim wget software-properties-common

# setup PostgreSQL/PostGIS libraries
RUN add-apt-repository "deb http://apt.postgresql.org/pub/repos/apt/ xenial-pgdg main"
RUN wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add -
RUN apt-get update
RUN apt-get install -y postgresql-9.6 postgresql-contrib-9.6
RUN add-apt-repository ppa:ubuntugis/ubuntugis-unstable
RUN apt-get update
RUN apt-get install -y postgis postgresql-9.6-postgis-2.3

WORKDIR $PROJECT_HOME
RUN mkdir -p $PROJECT_HOME/logs
RUN git init
RUN git remote add origin https://github.com/spd-digital/antrags-analyse-tool.git
RUN git pull origin AAT-8_upload_files_to_s3
RUN apt-get install -y python2.7 python-pip
RUN apt-get install -y virtualenv
RUN virtualenv --python=python2.7 venv
RUN venv/bin/pip install -r requirements.txt
RUN venv/bin/pip install -r requirements.deploy.txt
COPY ./att.env $PROJECT_HOME/.env
RUN sed -i "s/\%\%PROJECT_NAME/$PROJECT_NAME/g" $PROJECT_HOME/.env
RUN sed -i "s#\%\%PROJECT_HOME#$PROJECT_HOME#g" $PROJECT_HOME/.env
RUN sed -i "s/\%\%PROJECT_URL/$PROJECT_URL/g" $PROJECT_HOME/.env
RUN sed -i "s/\%\%PROJECT_PORT/$PROJECT_PORT/g" $PROJECT_HOME/.env
RUN sed -i "s/\%\%PROJECT_PASSWORD/$PROJECT_PASSWORD/g" $PROJECT_HOME/.env

# execute the container
ENTRYPOINT $PROJECT_HOME/venv/bin/gunicorn --access-logfile "-" --error-logfile "-" --enable-stdio-inheritance --workers 3 --bind unix:$PROJECT_HOME/$PROJECT_NAME.sock propositionguide.wsgi:application

