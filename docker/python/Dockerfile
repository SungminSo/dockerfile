FROM python:3.8-slim-buster

VOLUME /dev/shm

RUN apt-get update && apt-get install -y \
	curl \
	gnupg \
	wget

WORKDIR /builder

ADD . /builder
RUN pip install --upgrade pip ; \
	pip install -r requirements.txt

EXPOSE 5000

CMD ["python", "./service.py"]
