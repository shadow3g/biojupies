FROM ubuntu:18.04

ENV DEBIAN_FRONTEND=noninteractive

COPY pkglist.txt .
RUN apt-get update && apt-get install -y $(cat pkglist.txt)

RUN mkdir /notebook-generator-server; virtualenv -p /usr/bin/python3 /notebook-generator-server/venv/;

COPY requirements.R .
RUN Rscript requirements.R

COPY requirements.txt .
RUN . /notebook-generator-server/venv/bin/activate && pip3 install -r requirements.txt
RUN . /notebook-generator-server/venv/bin/activate && python -m ipykernel install --user --name venv --display-name "python3-venv"

ENV LIBRARY_VERSION=v1.1.4

COPY . /notebook-generator-server  
WORKDIR /notebook-generator-server
RUN chmod +x boot.sh download_library.sh; ./download_library.sh

RUN wget https://dl.google.com/cloudsql/cloud_sql_proxy.linux.amd64 -O cloud_sql_proxy; chmod +x cloud_sql_proxy; mkdir /cloudsql; chmod 777 /cloudsql; 

ENTRYPOINT mkdir -p .config/gcloud; echo $APPLICATION_DEFAULT_CREDENTIALS > $GOOGLE_APPLICATION_CREDENTIALS; ./cloud_sql_proxy -instances=$SQL_INSTANCE_CONNECTION_NAME=tcp:3306 & ./boot.sh