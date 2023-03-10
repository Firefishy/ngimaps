FROM debian:unstable

RUN apt-get update -y; apt-get install python3-six mapproxy uwsgi uwsgi-plugin-python3 curl --no-install-recommends -y

WORKDIR /mapproxy

COPY mapproxy.yaml .
COPY mapproxy.ini .
COPY seed.yaml .
COPY benchmark.yaml .

RUN mapproxy-util create -t wsgi-app --mapproxy-conf=mapproxy.yaml wsgi; cat wsgi.py

CMD ["uwsgi", "--ini", "mapproxy.ini"]
