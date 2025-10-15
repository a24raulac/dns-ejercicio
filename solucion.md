# 1. Instalación de zonas mestras primarias
## 1.1 Instala o servidor BIND9 no equipo darthvader. Comproba que xa funciona coma servidor DNS caché pegando no documento de entrega a saída deste comando dig @localhost www.edu.xunta.es
### Salida comando: dig @localhost www.edu.xunta.es
<img width="944" height="567" alt="2025-10-07_09-19" src="https://github.com/user-attachments/assets/7036d814-b381-441e-aad1-6402226b7396" />

---
## 1.2. Configura o servidor BIND9 para que empregue como reenviador 8.8.8.8. pegando no documento de entrega contido do ficheiro /etc/bind/named.conf.options e a saída deste comando: dig @localhost www.mecd.gob.es
### Arquivo /etc/bind/named.conf.local
```
// Zona directa para starwars.lan
zone "starwars.lan" {
    type master;
    file "/etc/bind/db.starwars.lan";
    allow-transfer { key "transfer-key"; };
};

// Zona inversa para la red 192.168.20.0/24
zone "20.168.192.in-addr.arpa" {
    type master;
    file "/etc/bind/db.20.168.192.in-addr.arpa";
    allow-transfer { 192.168.20.11; };
};

```
### Salida comando: dig @localhost www.mecd.gob.es
![dig @localhost www.mecd.gob.es](Comando-1.2.png)
---
## 1.3. Instala unha zona primaria de resolución directa chamada "starwars.lan" e engade os seguintes rexistros de recursos (a maiores dos rexistros NS e SOA imprescindibles):
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
- Pega no documento de entrega o contido do arquivo de zona, e do arquivo /etc/bind/named.conf.local
### Arquivo db.starwars.lan
```
Archivo de zona para starwars.lan.
$TTL    86400   ; TTL por defecto para todos los registros (1 día)
$ORIGIN starwars.lan.

; Registro SOA (Start of Authority)
@       IN      SOA     darthvader.starwars.lan. admin.starwars.lan. (
                        3              ; Serial (AAAAMMDDNN)
                        3600           ; Refresh (1 hora)
                        1800           ; Retry (30 minutos)
                        1209600        ; Expire (2 semanas)
                        86400          ; Negative Cache TTL (1 día)
                        )

; Registro NS (Name Server)
@       IN      NS      darthvader.starwars.lan.
@       IN      NS      darthsidious.starwars.lan.

; Registros A
@           IN      A       192.168.20.10
darthvader  IN      A       192.168.20.10
skywalker   IN      A       192.168.20.101
skywalker   IN      A       192.168.20.111
luke        IN      A       192.168.20.22
darthsidious    IN      A       192.168.20.11
yoda        IN      A       192.168.20.24
yoda        IN      A       192.168.20.25
c3p0        IN      A       192.168.20.26
chewbacca        IN      A       192.168.20.28
rd2d2        IN      A       192.168.20.29


; Registro MX (Mail Exchange)
@       IN      MX      10 c3p0.starwars.lan.

; Registro TXT
lenda       IN      TXT     "Que a forza te acompanhe"

; Registro CNAME
palpatine   IN      CNAME   darthsidious.starwars.lan.

```
### Arquivo /etc/bind/named.conf.local
```
// Zona directa para starwars.lan
zone "starwars.lan" {
    type master;
    file "/etc/bind/db.starwars.lan";
    allow-update { none; };
};

// Zona inversa para la red 192.168.20.0/24
zone "20.168.192.in-addr.arpa" {
    type master;
    file "/etc/bind/db.20.168.192";
    allow-update { none; };
};
```
## 1.4. Instala unha zona de resolución inversa que teña que ver co enderezo do equipo darthvader, e engade rexistros PTR para os rexistros tipo A do exercicio anterior. Pega no documento de entrega o contido do arquivo de zona, e do arquivo /etc/bind/named.conf.local
### Arquivo db.20.168.192.in-addr.arpa
```
$TTL    86400
@       IN      SOA     ns1.starwars.lan. admin.starwars.lan. (
                        2025100103  ; Serial
                        3600        ; Refresh
                        1800        ; Retry
                        604800      ; Expire
                        86400       ; Minimum
                        )
        IN      NS      ns1.starwars.lan.
        IN      NS      ns2.starwars.lan.

10      IN      PTR     ns1.starwars.lan.
11      IN      PTR     ns2.starwars.lan.
20      IN      PTR     darthvader.starwars.lan.
22      IN      PTR     luke.starwars.lan.
22      IN      PTR     leia.starwars.lan.   ; Dos nombres, pero solo uno será válido (generalmente el primero)
23      IN      PTR     han.starwars.lan.
25      IN      PTR     yoda.starwars.lan.
26      IN      PTR     c3p0.starwars.lan.
28      IN      PTR     chewbacca.starwars.lan.
30      IN      PTR     mail.starwars.lan.
101     IN      PTR     skywalker.starwars.lan.
101     IN      PTR     darthsidious.starwars.lan. ; Igual que antes, solo uno será válido

```
### Arquivo /etc/bind/named.conf.local
```
// Zona directa para starwars.lan
zone "starwars.lan" {
    type master;
    file "/etc/bind/db.starwars.lan";
    allow-update { none; };
};

// Zona inversa para la red 192.168.20.0/24
zone "20.168.192.in-addr.arpa" {
    type master;
    file "/etc/bind/db.20.168.192";
    allow-update { none; };
};
```
## 1.5. Comproba que podes resolver os distintos rexistros de recursos. Pega no documento de entrega a saída dos comandos:
- nslookup darthvader.starwars.lan localhost
- nslookup skywalker.starwars.lan localhost
- nslookup starwars.lan localhost
- nslookup -q=mx starwars.lan localhost
- nslookup -q=ns starwars.lan localhost
- nslookup -q=soa starwars.lan localhost
- nslookup -q=txt lenda.starwars.lan localhost
- nslookup 192.168.20.11 localhost
### Salida comandos: 
<img width="941" height="513" alt="2025-10-07_09-30" src="https://github.com/user-attachments/assets/f661e4d1-2016-497b-9bc0-8c98f11ce1f3" />

<img width="945" height="570" alt="2025-10-07_09-36" src="https://github.com/user-attachments/assets/76c58553-7114-4d68-bcb0-3e097bf571be" />

<img width="978" height="622" alt="2025-10-07_09-37" src="https://github.com/user-attachments/assets/c613c096-0f97-464d-9eb6-7b721879c8ca" />

<img width="2211" height="1063" alt="2025-10-08_09-27" src="https://github.com/user-attachments/assets/965888ee-5dd0-4f9a-90c3-7dce7efb4aa7" />

# 2. Instalación de zonas secundarias
## 2.1. Tomaremos a máquina darthsidious, e configuraremola para ser servidor secundario, tanto da zona primaria de resolución directa como de resolución inversa. Captura os ficheiros de configuración en ambalas dúas máquinas. Fai unha captura onde se vexa o reinicio da máquina darthsidious, no que se vexa no log dos dous equipos e que se fixo a transferencia de zona.
### Ficheiro named.conf.local en darthsidious
```

```
### Ficheiro named.conf.local en darthvader
```
//
// Do any local configuration here
//

// Consider adding the 1918 zones here, if they are not used in your
// organization
//include "/etc/bind/zones.rfc1918";

key "transfer-key" {
	algorithm hmac-sha256;
	secret "rFqCzTlO3IcQiBCtoaRPeW6U0yuQptjUCAzLxAWWeX4=";
};

server 192.168.20.10{
    keys {"transfer-key";};
};

// Zona directa para starwars.lan
zone "starwars.lan" {
    type slave;
    file "db.starwars.lan";
    masters { 192.168.20.10; };
};

// Zona inversa para la red 192.168.20.0/24
zone "20.168.192.in-addr.arpa" {
    type slave;
    file "db.20.168.192.in-addr.arpa";
    masters { 192.168.20.10; };
};

```
### Ficheiro named.conf.option iguales
```
options {
	directory "/var/cache/bind";
	// forwarders {
	// 	8.8.8.8;
	// 	9.9.9.9;
	// 	// Renviador 9.9.9.9 para asegurarnos que si no funciona el 8.8.8.8 tengamos uno
	// };	
};
```
### Salida de logs
<img width="1777" height="1043" alt="2025-10-14_10-03" src="https://github.com/user-attachments/assets/9cc515c8-1256-4ca0-a586-1e3f5531a976" />

## 2.2. Engade un rexistro tipo A (Chewbacca 192.168.20.28) na zona de resolución directa e tamén na de resolución inversa.  Fai unha captura no momento do reinicio do equipo darthvader, no que se vexa o log dos dous equipos e que se amose que se fixo a transferencia de zona. Adxunta tamén unha captura do ficheiro de zona no servidor secundario.
### Salida de logs
<img width="1776" height="1040" alt="2025-10-14_10-19" src="https://github.com/user-attachments/assets/ed26fc96-17d6-49bf-b842-923f0491beb6" />

### Ficheiro db.20.168.192 en darthsidious
<img width="1172" height="677" alt="2025-10-15_09-33" src="https://github.com/user-attachments/assets/79add79d-a0a3-4dec-b5c5-86d2821f4ec8" />

## 2.3. Comproba que o servidor secundario pode resolver ese nome.
<img width="1175" height="678" alt="2025-10-15_09-17" src="https://github.com/user-attachments/assets/ca1604da-93ed-4f74-a958-d9f0f3be47f2" />

## 2.4. Fai os cambios necesarios para que as trasferencias se fagan de forma segura empregando chaves.  Repite as capturas e vídeos do punto 2, engadindo o rexistro r2d2 (192.168.20.29)
### Salida de logs
<img width="1777" height="1043" alt="2025-10-14_10-03" src="https://github.com/user-attachments/assets/fccb9276-ed4f-4aeb-bcea-caafd83dd777" />

### Ficheiro db.20.168.192 en darthsidious
<img width="1172" height="677" alt="2025-10-15_09-33" src="https://github.com/user-attachments/assets/658530e1-b92c-4e4a-8a8b-44537632fb49" />
