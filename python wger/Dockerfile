from alpine:3.6
MAINTAINER cjmash "james.macharia@andela.com"
RUN apk update
ENV PYTHONUNBUFFERED 1
RUN apk add curl python3 python3-dev jpeg-dev bash 	nodejs-npm  make g++ zlib-dev git gcc postgresql-dev --no-cache  --repository http://dl-cdn.alpinelinux.org/alpine/edge/main/
RUN curl https://bootstrap.pypa.io/get-pip.py | python3
RUN mkdir /code
WORKDIR /code
COPY wger-f-society /code
RUN pip3 install -r requirements.txt
RUN invoke create-settings \
         --settings-path /home/wger/wger/settings.py \
         --database-path /home/wger/wger/database.sqlite
RUN   invoke bootstrap-wger
