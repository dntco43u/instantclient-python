FROM ubuntu:latest
ARG DEBIAN_FRONTEND=noninteractive
ENV TZ Asia/Seoul
ENV ORACLE_HOME /opt/oracle/instantclient
ENV PATH $PATH:$ORACLE_HOME
ENV LD_LIBRARY_PATH $LD_LIBRARY_PATH:$ORACLE_HOME
ENV SQLPATH ${INSTANT_CLIENT_PATH}:${SQLPATH}
ENV TNS_ADMIN /opt/oracle/instantclient/network/admin
RUN apt-get update && \
    apt-get install -y tzdata \
    libaio1 \
    python3 \
    python3-pip \
    curl \
    unzip && \
    ln -sf /usr/share/zoneinfo/$TZ /etc/localtime && \
    echo $TZ > /etc/timezone && \
    echo /opt/oracle/instantclient > /etc/ld.so.conf.d/oic.conf && \
    ldconfig && \
    curl https://download.oracle.com/otn_software/linux/instantclient/218000/instantclient-basiclite-linux.x64-21.8.0.0.0dbru.zip -so /tmp/instantclient.zip && \
    unzip -o /tmp/instantclient.zip -d /opt/oracle && \
    rm /tmp/instantclient.zip && \
    curl https://download.oracle.com/otn_software/linux/instantclient/218000/instantclient-sqlplus-linux.x64-21.8.0.0.0dbru.zip -so /tmp/instantclient.zip && \
    unzip -o /tmp/instantclient.zip -d /opt/oracle && \
    rm /tmp/instantclient.zip && \    
    curl https://download.oracle.com/otn_software/linux/instantclient/218000/instantclient-tools-linux.x64-21.8.0.0.0dbru.zip -so /tmp/instantclient.zip && \
    unzip -o /tmp/instantclient.zip -d /opt/oracle && \
    rm /tmp/instantclient.zip && \
    cd /opt/oracle && \
    mv instantclient_21_8 instantclient && \
    python3 -m pip install --no-cache-dir pip && \
    python3 -m pip install --no-cache-dir cx_Oracle && \
    apt-get purge -y python3-pip \
    curl \
    unzip && \
    apt-get autoremove -y && \
    apt-get clean autoclean && \
    rm -rf /var/lib/apt/lists/*
