FROM debian:stretch

RUN apt-get update && apt-get install -y \
  curl \
  pv


RUN echo "Wingardium Leviosa"
CMD curl -s http://artscene.textfiles.com/vt100/shuttle.vt | pv -L4800 -q
