FROM python:3.8
RUN apt-get update -y
RUN apt-get install -y postgresql-client
COPY requirements.txt .
RUN pip3 install -r requirements.txt
COPY . /app
WORKDIR /app