# Instalación DNS

## Instala o servidor BIND9 no equipo darthvader.Comproba que xa funciona coma servidor DNS caché pegando no documento de entrega a saída deste comando dig @localhost www.edu.xunta.es

### Salida de dig @localhost www.edu.xunta.es
<img width="944" height="567" alt="2025-10-07_09-19" src="https://github.com/user-attachments/assets/e65a73a1-7ef4-4b0f-b794-84b02c327b66" />


## Configura o servidor BIND9 para que empregue como reenviador 8.8.8.8. pegando no documento de entrega contido do ficheiro /etc/bind/named.conf.options e a saída deste comando: dig @localhost www.mecd.gob.es

> /etc/bind/named.conf.local

``` zone "starwars.lan" {
    type master;
    file "/etc/bind/db.starwars.lan";
};
zone "20.168.192.in-addr.arpa" {
    type master;
    file "/etc/bind/db.20.168.192.in-addr.arpa";
};
```
## Salida de dig @localhost www.mecd.gob.es

<img width="941" height="513" alt="2025-10-07_09-30" src="https://github.com/user-attachments/assets/d3f19ead-af75-41a1-b09c-77409f1ce68c" />

## Instala unha zona primaria de resolución directa chamada "starwars.lan" e engade os seguintes rexistros de recursos (a maiores dos rexistros NS e SOA imprescindibles):
- Tipo A: darthvader con IP 192.168.20.10
- Tipo A: skywalker con IP 192.168.20.101
- Tipo A: skywalker con IP 192.168.20.111
- Tipo A: luke con IP 192.168.20.22
- Tipo A: darthsidious con IP 192.168.20.11
- Tipo A: yoda con IP 192.168.20.24 e 192.168.20.25
- Tipo A: c3p0 con IP 192.168.20.26
- Tipo CNAME palpatine a darthsidious
- TIPO MX con prioridade 10 sobre o equipo c3po
- TIPO TXT "lenda" con "Que a forza te acompanhe"
- TIPO NS con darthsidious

> db.starwars.lan

```
$TTL    86400
@       IN      SOA     ns1.starwars.lan. admin.starwars.lan. (
                        2025100101  ; Serial
                        3600        ; Refresh
                        1800        ; Retry
                        604800      ; Expire
                        86400       ; Minimum
                        )
@        IN      NS      ns1.starwars.lan.
@        IN      NS      ns2.starwars.lan.
@        IN      A       127.0.0.1

ns1     IN      A       192.168.20.10
ns2     IN      A       192.168.20.11

darthvader   IN      A       192.168.20.20
skywalker   IN      A       192.168.20.101
luke   IN      A       192.168.20.22
darthsidious   IN      A       192.168.20.101
yoda    IN      A       192.168.20.25
leia    IN      A       192.168.20.22
han     IN      A       192.168.20.23
c3p0     IN      A       192.168.20.26

jedi    IN      CNAME   yoda.starwars.lan.
palpatine    IN      CNAME   darthsidious.starwars.lan.
sith    IN      CNAME   darthvader.starwars.lan.

@        IN      MX      10 mail.starwars.lan.
mail    IN      A       192.168.20.30

lenda   IN      TXT     "Que a forza te acompanhe"

```

> /etc/bind/named.conf.local

``` zone "starwars.lan" {
    type master;
    file "/etc/bind/db.starwars.lan";
};
zone "20.168.192.in-addr.arpa" {
    type master;
    file "/etc/bind/db.20.168.192.in-addr.arpa";
};
```

## Salida comandos

- nslookup darthvader.starwars.lan localhost
- nslookup skywalker.starwars.lan localhost
- nslookup starwars.lan localhost
- nslookup -q=mx starwars.lan localhost
- nslookup -q=ns starwars.lan localhost
- nslookup -q=soa starwars.lan localhost
- nslookup -q=txt lenda.starwars.lan localhost
- nslookup 192.168.20.11 localhost

<img width="945" height="570" alt="2025-10-07_09-36" src="https://github.com/user-attachments/assets/a3450f8f-b7ba-4a89-abb9-4fc10ab3c48f" />

<img width="978" height="622" alt="2025-10-07_09-37" src="https://github.com/user-attachments/assets/9278d3c3-e455-4d07-94fa-4bfa104b81d4" />
