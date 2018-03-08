FROM ubuntu

RUN apt-get update && apt-get install -y \
  curl \
  pv \
  iputils-ping \
  net-tools

CMD curl -s http://webserver01/wineglas.vt | pv -L3000 -q
