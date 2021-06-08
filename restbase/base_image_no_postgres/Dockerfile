FROM python:3.6-slim

COPY requirements.txt .

RUN python3.6 -m venv ./venv
RUN /bin/sh ./venv/bin/activate
RUN pip3 install -r requirements.txt
