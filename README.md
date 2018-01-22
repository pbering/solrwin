# Docker image for Solr on nano server with SSL

Small (1.5 GB) Windows container based on OpenJDK 8 nanoserver with Solr 6.6.2 and SSL configured.

## First time setup

1. As an admin run "/solr/Generate-Certificate.ps2". This generates a self-signed certificate and adds it to your machine "Trusted Root Certification Authorities". It will also be used when building the Docker image.
1. Run: `docker-compose build`
1. Place the contents of "/solr-6.6.2/server/solr/" (from a normal "solr-6.6.2.zip") to initialize it.

## Usage

1. Run: `docker-compose up`
1. Browse [https://solr:8983](https://solr:8983)