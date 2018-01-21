# escape=\
FROM openjdk:8u151-jdk-windowsservercore-1709

SHELL ["powershell", "-NoProfile", "-Command", "$ErrorActionPreference = 'Stop';"]

ARG SERVER_VERSION=6.6.2
ARG SERVER_ROOT=C:/solr/solr-${SERVER_VERSION}

ENV SOLR_SSL_KEY_STORE=${SERVER_ROOT}/server/etc/solr-ssl.keystore.pfx
ENV SOLR_SSL_KEY_STORE_PASSWORD=secret
ENV SOLR_SSL_TRUST_STORE=${SERVER_ROOT}/server/etc/solr-ssl.keystore.pfx
ENV SOLR_SSL_TRUST_STORE_PASSWORD=secret

RUN Invoke-WebRequest -Method Get -Uri ('http://archive.apache.org/dist/lucene/solr/{0}/solr-{0}.zip' -f $env:SERVER_VERSION) -OutFile /solr.zip; \
    Expand-Archive -Path /solr.zip -DestinationPath /solr; \
    Remove-Item /solr.zip -Force;

VOLUME c:/data

RUN Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\DOS Devices' -Name 'G:' -Value '\??\C:\data' -Type String

WORKDIR ${SERVER_ROOT}

ADD ./certs ./server/etc

EXPOSE 8983

# TODO: multistage build, måske nemmere hvis ikke vi sætter SHELL?
# TODO: Split ENTRYPOINT into CMD
ENTRYPOINT bin/solr start -port 8983 -f -noprompt