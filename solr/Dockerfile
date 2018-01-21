# escape=\
FROM openjdk:8u151-jdk-windowsservercore-1709 as builder

ARG SOLR_VERSION=6.6.2

RUN powershell -Command Invoke-WebRequest -Method Get -Uri ('http://archive.apache.org/dist/lucene/solr/{0}/solr-{0}.zip' -f $env:SOLR_VERSION) -OutFile /solr.zip; \
    Expand-Archive -Path /solr.zip -DestinationPath /temp; \
    Remove-Item /solr.zip -Force; \
    Move-Item -Path "C:/temp/solr-*" -Destination c:\solr; \
    Remove-Item -Path "C:/temp" -Recurse -Force

FROM openjdk:8u151-jdk-nanoserver

COPY --from=builder /solr /solr

ADD ./certs c:/solr/server/etc

ENV SOLR_SSL_KEY_STORE=c:/solr/server/etc/solr-ssl.keystore.pfx
ENV SOLR_SSL_KEY_STORE_PASSWORD=secret
ENV SOLR_SSL_TRUST_STORE=c:/solr/server/etc/solr-ssl.keystore.pfx
ENV SOLR_SSL_TRUST_STORE_PASSWORD=secret

VOLUME c:/data

RUN REG ADD 'HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\DOS Devices' /v 'G:' /d '\??\C:\data' /reg:64

EXPOSE 8983

ENTRYPOINT [ "c:/solr/bin/solr.cmd" ]

CMD [ "start", "-port 8983", "-f" ]