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
    patchelf \
    unzip && \
    ln -sf /usr/share/zoneinfo/$TZ /etc/localtime && \
    echo $TZ > /etc/timezone && \
    echo /opt/oracle/instantclient > /etc/ld.so.conf.d/oic.conf && \
    ldconfig && \
    curl https://download.oracle.com/otn_software/linux/instantclient/191000/instantclient-basiclite-linux.arm64-19.10.0.0.0dbru.zip -so /tmp/instantclient.zip && \
    unzip -o /tmp/instantclient.zip -d /opt/oracle && \
    rm /tmp/instantclient.zip && \
    curl https://download.oracle.com/otn_software/linux/instantclient/191000/instantclient-sqlplus-linux.arm64-19.10.0.0.0dbru.zip -so /tmp/instantclient.zip && \
    unzip -o /tmp/instantclient.zip -d /opt/oracle && \
    rm /tmp/instantclient.zip && \    
    curl https://download.oracle.com/otn_software/linux/instantclient/191000/instantclient-tools-linux.arm64-19.10.0.0.0dbru.zip -so /tmp/instantclient.zip && \
    unzip -o /tmp/instantclient.zip -d /opt/oracle && \
    rm /tmp/instantclient.zip && \    
    cd /opt/oracle && \
    mv instantclient_19_10 instantclient && \    
    sh -c "echo /opt/oracle/instantclient > /etc/ld.so.conf.d/oracle-instantclient.conf" && \
    ldconfig && \
    patchelf --set-interpreter /lib/ld-linux-aarch64.so.1 instantclient/sqlplus && \
    patchelf --set-interpreter /lib/ld-linux-aarch64.so.1 instantclient/exp && \
    patchelf --set-interpreter /lib/ld-linux-aarch64.so.1 instantclient/expdp && \
    patchelf --set-interpreter /lib/ld-linux-aarch64.so.1 instantclient/imp && \
    patchelf --set-interpreter /lib/ld-linux-aarch64.so.1 instantclient/impdp && \
    patchelf --set-interpreter /lib/ld-linux-aarch64.so.1 instantclient/sqlldr && \
    patchelf --set-interpreter /lib/ld-linux-aarch64.so.1 instantclient/sqlplus && \
    patchelf --set-interpreter /lib/ld-linux-aarch64.so.1 instantclient/wrc && \
    python3 -m pip install --no-cache-dir pip && \
    python3 -m pip install --no-cache-dir cx_Oracle && \
    apt-get purge -y python3-pip \
    curl \
    unzip && \
    apt-get autoremove -y && \
    apt-get clean autoclean && \
    rm -rf /var/lib/apt/lists/*
