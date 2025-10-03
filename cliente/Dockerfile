FROM debian:13

RUN apt update && \
    apt upgrade -y && \
    apt install -y procps iproute2 dnsutils iputils-ping && \
    rm -rf /var/lib/apt/lists/*

RUN mkdir -p /kk
CMD [ "tail", "-f", "/dev/null" ]