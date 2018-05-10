# we use vanilla Ubuntu 16.04 as the base layer
FROM ubuntu:16.04

MAINTAINER Friedrich Kauder <fkauder@gmail.com>

# setup variables
ENV PROJECT_NAME=antrags_analyse_tool
ENV PROJECT_HOME=/opt/$PROJECT_NAME
ENV PROJECT_URL=localhost
ENV PROJECT_PORT=80

# update system package sources
RUN apt-get update

# setup web server
RUN apt-get install -y nginx
RUN unlink /etc/nginx/sites-enabled/default
COPY ./att.nginx.conf /etc/nginx/sites-available/$PROJECT_NAME.nginx.conf
RUN sed -i "s/\%\%PROJECT_NAME/$PROJECT_NAME/g" /etc/nginx/sites-available/$PROJECT_NAME.nginx.conf
RUN sed -i "s#\%\%PROJECT_HOME#$PROJECT_HOME#g" /etc/nginx/sites-available/$PROJECT_NAME.nginx.conf
RUN sed -i "s/\%\%PROJECT_URL/$PROJECT_URL/g" /etc/nginx/sites-available/$PROJECT_NAME.nginx.conf
RUN sed -i "s/\%\%PROJECT_PORT/$PROJECT_PORT/g" /etc/nginx/sites-available/$PROJECT_NAME.nginx.conf
RUN ln -s /etc/nginx/sites-available/$PROJECT_NAME.nginx.conf /etc/nginx/sites-enabled/$PROJECT_NAME.nginx.conf
#RUN service nginx restart

# execute the container
CMD ["nginx", "-g", "daemon off;"]
EXPOSE $PROJECT_PORT

