FROM alpine:3.6
MAINTAINER james collins
RUN apk update
RUN apk add python3 python3-dev postgresql postgresql-contrib libxml2-dev libxslt-dev  git g++ gcc postgresql-dev --no-cache
RUN pip3 install ez_setup --upgrade setuptools
RUN git clone https://github.com/andela/healthchecks-f-society.git
WORKDIR healthchecks-f-society
RUN pip3 install -r requirements.txt
RUN python3 manage.py migrate
RUN python3 manage.py ensuretriggers
RUN python3 manage.py createsuperuser
EXPOSE 8080
ENTRYPOINT [ "/usr/bin/python3", "/healthchecks-f-society/manage.py", "runserver" ]