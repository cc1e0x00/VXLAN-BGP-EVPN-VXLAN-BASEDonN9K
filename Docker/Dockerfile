FROM alpine:3.24

LABEL org.opencontainers.image.authors="rafaesil" \ 
      org.opencontainers.image.version="1.0" \ 
      org.opencontainers.image.description="Custom image from alpine with network tools to be used in CCIE DC Lab"

RUN set -ex && \
    echo "http://dl-cdn.alpinelinux.org/alpine/edge/main" >> /etc/apk/repositories && \
    echo "http://dl-cdn.alpinelinux.org/alpine/edge/testing" >> /etc/apk/repositories && \
    echo "http://dl-cdn.alpinelinux.org/alpine/edge/community" >> /etc/apk/repositories && \
    apk update && \
    apk upgrade && \
    apk add --no-cache \
    curl \
    bind-tools \
    iproute2 \
    iputils \
    iperf3 \
    tcpdump \
    mtr \
    nmap \
    openssh-client \
    libcap

# By kernel capabilities setcap: Allow sockets RAW and  Admin role to manager NIC   
RUN setcap cap_net_raw,cap_net_admin=eip /usr/bin/tcpdump && \
    setcap cap_net_raw,cap_net_admin=eip /usr/bin/nmap && \
    setcap cap_net_raw,cap_net_admin=eip /usr/bin/iperf3 && \
    setcap cap_net_raw,cap_net_admin=eip /bin/ping 

RUN adduser -D -u 10001 -G root neteng && \
    chmod -R g=u /home/neteng

WORKDIR /home/neteng

# Trust on ID not in username
USER 10001

CMD ["sleep", "infinity"]
