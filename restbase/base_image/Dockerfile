FROM python:3.8

RUN apt-get update \
    && apt-get -y install libpq-dev gcc

COPY requirements.txt .

RUN pip3 install -r requirements.txt
