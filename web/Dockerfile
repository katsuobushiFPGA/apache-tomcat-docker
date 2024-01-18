FROM tomcat:9.0.85-jdk11-corretto

RUN yum install -y httpd httpd-ajp

COPY httpd/httpd-ajp.conf /etc/httpd/conf.d/

COPY tomcat/server.xml /usr/local/tomcat/conf/

WORKDIR /usr/local/tomcat/webapps/
RUN curl -OL https://tomcat.apache.org/tomcat-9.0-doc/appdev/sample/sample.war
WORKDIR /root

# supervisorインストール
RUN yum -y install python3 && pip3 install supervisor

# supervisorの設定をコピー
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

CMD ["supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]