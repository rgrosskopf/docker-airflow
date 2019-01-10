# docker-airflow w/ Oracle
A few modifications needed to Puckel's creation for my environment. Uploaded here in case others are trying to get airflow working in with Oracle.

## tl;dr
```
RUN apt-get install -yqq --no-install-recommends \
    alien \
    libaio1

#add oracle instant client from local directory
COPY /oracle/ /tmp/
WORKDIR /tmp
RUN alien -i oracle-instantclient12.2-basiclite-12.2.0.1.0-1.x86_64.rpm \
    && alien -i oracle-instantclient12.2-sqlplus-12.2.0.1.0-1.x86_64.rpm \
    && alien -i oracle-instantclient12.2-tools-12.2.0.1.0-1.x86_64.rpm \
    && echo /usr/lib/oracle/12.2/client64/lib > /etc/ld.so.conf.d/oracle-instantclient12.2.conf \
    && ldconfig \
    && apt-get purge --auto-remove -yqq alien \
    && apt-get autoremove -yqq --purge \
    && apt-get clean \
    && rm -rf /tmp/* \
#--
```
## Oracle Connection Format
This took some experimenting as well. Thanks to [stackoverflow](https://stackoverflow.com/questions/36945811/how-to-connect-airflow-to-oracle-database/43552918#43552918). \
\
Conn ID: connName \
Conn Type: Oracle \
Host: (DESCRIPTION =(ADDRESS = (PROTOCOL = TCP)(HOST = localhost)(PORT = 1521)) (CONNECT_DATA = (SERVER = DEDICATED) (SERVICE_NAME = mysidname))) \
Schema: -blank- \
Login: UserID \
Password: PW \
Port: -blank- \
Extra: -blank-
