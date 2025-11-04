FROM debian:latest

RUN apt update  && apt install -y \
    procps \
    iproute2 \
    iputils-ping \
    bind9 \
    bind9utils \
    dnsutils \
    && rm -rf /var/lib/apt/lists/*

# COPY named.conf.options /etc/bind/named.conf.options

RUN mkdir -p /dns

COPY entrypoint.sh /dns/entrypoint.sh
RUN chmod a+x /dns/entrypoint.sh
RUN mkdir -p /var/log/bind
RUN touch /var/log/bind/queries.log
RUN touch /var/log/bind/xfer-in.log
RUN touch /var/log/bind/xfer-out.log
RUN chown bind:bind /var/log/bind -R
RUN chmod 777 /var/log/bind -R
RUN chmod 777 /var/log/bind/queries.log
RUN chmod 755 /var/log/bind/xfer-in.log
RUN chmod 755 /var/log/bind/xfer-out.log
RUN ufw allow from 192.168.20.10 to any port 953 proto tcp
RUN ufw allow from 192.168.20.11 to any port 953 proto tcp
RUN ufw allow from 192.168.20.12 to any port 953 proto tcp

EXPOSE 53
# USER bind
WORKDIR /var/cache/bind

CMD ["-g","-4"]

ENTRYPOINT ["/dns/entrypoint.sh"]