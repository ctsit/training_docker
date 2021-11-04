FROM tomcat:9-jdk11-openjdk

RUN mkdir /usr/local/tomcat/webapps/ROOT/

COPY /app /usr/local/tomcat/webapps/ROOT/

CMD ["catalina.sh", "run"]
# The commands to run this simple tomcat server are as follows.
# docker build -t tomcat-test .
# docker run tomcat-test -p 8080:8080 