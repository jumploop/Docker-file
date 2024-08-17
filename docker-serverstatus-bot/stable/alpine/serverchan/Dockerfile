FROM python:alpine

LABEL maintainer="jumploop"
LABEL version="0.0.1"
LABEL description="ServerChan Bot for ServerStatus"

WORKDIR /app
RUN pip install requests
COPY bot-serverchan.py ./

CMD [ "python", "./bot-serverchan.py" ]
