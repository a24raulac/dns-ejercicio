FROM debian:13

RUN apt-get update && apt-get install -y bind9 procps iproute2 dnsutils iputils-ping && \
    rm -rf /var/lib/apt/lists/*

COPY named.conf.local /etc/bind/named.conf.local
COPY db.starwars.lan /etc/bind/db.starwars.lan
COPY db.20.168.192.in-addr.arpa /etc/bind/db.20.168.192.in-addr.arpa


RUN chmod 644 /etc/bind/named.conf.local
CMD ["named", "-g", "-c", "/etc/bind/named.conf"]