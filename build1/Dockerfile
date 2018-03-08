FROM ubuntu

RUN apt-get update && apt-get install -y \
  curl \
  pv


RUN echo "Wingardium Leviosa"
CMD curl -s http://kevinhanson.org/shuttle.vt | pv -L4800 -q
