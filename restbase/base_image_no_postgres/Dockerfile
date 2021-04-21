FROM python:3.6-slim

RUN apt-get update

RUN apt-get install -y libmysqlclient-dev

COPY requirements.txt .

RUN python3.6 -m venv ./venv
RUN /bin/sh ./venv/bin/activate
RUN pip3 install -r requirements.txt
