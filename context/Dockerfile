FROM ubuntu:20.04

RUN apt-get update && \
    DEBIAN_FRONTEND="noninteractive" apt-get -y upgrade && \
    DEBIAN_FRONTEND="noninteractive" apt-get -y install \
        nodejs npm git ca-certificates gnupg curl wget \
        unzip gdal-bin tar bzip2 clang python pkg-config \
        zlib1g-dev make libnode64
    
WORKDIR /root/

RUN npm cache clean -f && \
    npm install -g n && \
    n "6.11"

RUN cd /root/ && \
    git clone https://github.com/tilemill-project/tilemill && \
    cd tilemill && \
    sed -i -e 's/127.0.0.1/0.0.0.0/g' -e 's/open/#open/g' utils/runclient.sh && \
    mkdir -p /root/.tilemill && \
    npm install

RUN echo "{\"listenHost\":\"0.0.0.0\",\"server\":true}" > /root/.tilemill/config.json && \
    cat /root/.tilemill/config.json && \
    mkdir -p /root/Documents/MapBox
    
    # add this to the config manually if running remotely: ,\"coreUrl\":\"x.x.x.x:20009\",\"tileUrl\":\"x.x.x.x:20008\"

WORKDIR /root/tilemill

ENTRYPOINT ["npm", "start"]

EXPOSE 20008 20009

