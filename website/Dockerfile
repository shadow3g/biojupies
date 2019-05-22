FROM ubuntu:18.04

MAINTAINER Denis Torre <denis.torre@mssm.com>

COPY pkglist.txt .
RUN apt-get update && apt-get install -y $(cat pkglist.txt)

COPY requirements.txt .
RUN pip3 install -r requirements.txt

RUN mkdir biojupies
COPY . /biojupies
WORKDIR /biojupies
RUN chmod +x boot.sh; chmod -R 777 /biojupies/app/static;

RUN wget https://dl.google.com/cloudsql/cloud_sql_proxy.linux.amd64 -O cloud_sql_proxy; chmod +x cloud_sql_proxy; mkdir /cloudsql; chmod 777 /cloudsql; 

ENTRYPOINT mkdir -p .config/gcloud; echo $APPLICATION_DEFAULT_CREDENTIALS > $GOOGLE_APPLICATION_CREDENTIALS; ./cloud_sql_proxy -instances=$SQL_INSTANCE_CONNECTION_NAME=tcp:3306 & ./boot.sh