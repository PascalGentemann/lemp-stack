FROM ubuntu:xenial

RUN apt-get update && \
    apt-get install -y supervisor postfix libsasl2-modules bsd-mailx rsyslog && \
    apt-get install nano && \
    apt-get clean

COPY main.cf /etc/postfix/

EXPOSE 25

ADD install.sh /opt/install.sh

CMD /opt/install.sh;/usr/bin/supervisord -c /etc/supervisor/supervisord.conf
