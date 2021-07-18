FROM python:3.8
ENV PYTHONUNBUFFERED 1
WORKDIR /django
ADD requirements.txt /django/
RUN pip install -r requirements.txt
COPY scripts/start_prod.sh /
ADD . /django/
