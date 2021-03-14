FROM python:3.8
RUN apt-get update -y

RUN apt-get install -y software-properties-common 
RUN add-apt-repository ppa:deadsnakes/ppa
RUN apt-get install -y postgresql-client default-libmysqlclient-dev
COPY requirements.txt .
RUN pip3 install -r requirements.txt
COPY . /app
WORKDIR /app
