FROM tomcat:9.0.85-jdk11-corretto

RUN yum install openssl

RUN yum install -y httpd httpd-ajp mod_ssl

RUN yum install -y mod_http2

COPY cert/localhost.crt /etc/pki/tls/certs/
COPY cert/localhost.key /etc/pki/tls/private/

COPY httpd/*.conf /etc/httpd/conf.d/

COPY tomcat/server.xml /usr/local/tomcat/conf/

WORKDIR /usr/local/tomcat/webapps/
RUN curl -OL https://tomcat.apache.org/tomcat-9.0-doc/appdev/sample/sample.war
WORKDIR /root

# Apache設定
# server MPMがpreforkの場合http2通信が使えないため、eventに変更する
RUN sed -i.bak -e 's/^LoadModule mpm_prefork_module/#LoadModule mpm_prefork_module/' \
           -e 's/^#LoadModule mpm_event_module/LoadModule mpm_event_module/' \
           /etc/httpd/conf.modules.d/00-mpm.conf

# supervisorインストール
RUN yum -y install python3 && pip3 install supervisor

# supervisorの設定をコピー
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

CMD ["supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]