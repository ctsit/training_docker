FROM ubuntu
VOLUME /opt/sample
COPY /src /opt/sample
RUN /bin/echo 'testfail' >> /opt/sample/textfile.txt
RUN /bin/echo 'testfail' >> /textfile.txt
