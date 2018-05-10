FROM mdillon/postgis

MAINTAINER Friedrich Kauder <fkauder@gmail.com>

# setup variables
ENV PROJECT_NAME=antrags_analyse_tool
ENV PROJECT_PASSWORD=thispasswordisverylongandthereforeverysafe

COPY init.sql /docker-entrypoint-initdb.d/

RUN sed -i "s/\%\%PROJECT_NAME/$PROJECT_NAME/g" /docker-entrypoint-initdb.d/init.sql
RUN sed -i "s/\%\%PROJECT_PASSWORD/$PROJECT_PASSWORD/g" /docker-entrypoint-initdb.d/init.sql
