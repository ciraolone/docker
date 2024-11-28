# Reverse Proxy

Un reverse proxy è un server che si posiziona 
tra i client e uno o più server backend, fungendo
da intermediario per inoltrare le richieste dei 
client verso i server appropriati e restituire le 
risposte ai client. 

È utilizzato per bilanciare il carico, migliorare
le prestazioni, aggiungere un livello di sicurezza
e semplificare la gestione delle applicazioni. 

Nginx Proxy Manager è un'interfaccia grafica 
facile da usare che semplifica la configurazione
e la gestione di Nginx come reverse proxy con cui
è possibile configurare facilmente reindirizzamenti
HTTPS, regole di accesso, certificati SSL (anche
con Let's Encrypt) e routing delle richieste verso
i server backend, il tutto con pochi clic, 
rendendolo ideale sia per i principianti che per
gli amministratori esperti.

## Network

Creiamo una network dedicata con il comando:

`docker network create reverse-proxy`

## Stack

```yaml
volumes:
  certs:
    driver: local
  data:
    driver: local

networks:
  reverse-proxy:
    external: true

services:
  nginxpm:
    image: jc21/nginx-proxy-manager:2
    container_name: nginxpm
    hostname: nginxpm
    restart: unless-stopped
    volumes:
      - certs:/etc/letsencrypt
      - data:/data
    networks:
      - reverse-proxy
    ports:
      - 80:80
      - 81:81
      - 443:443
```

Per accedere all'interfaccia web aprire il browser su
`http://localhost:81`

Per avere un dominio personale registrarsi su un
qualsiasi DDNS presente nella lista di quelli
supportati da NGINXPM, nel nostro caso useremo:
`https://www.duckdns.org`

Una volta entrati registrare un nuovo dominio.

Se vogliamo che si accessibile anche dall'esterno
lasciamo il nostro indirizzo IP pubblico e poi
scarichiamo e configuriamo il client per mantenere
aggiornato l'indirizzo a cadenza regolare.

Altrimenti se vogliamo solo accedere dalla nostra
rete locale, impostiamo l'indirizzo IP del nostro
server dove abbiamo installato Docker.

Copiamoci il `token` in alto a destra e torniamo in
NGINXPM e nella sezione `SSL Certificates` premiamo
`Add SSL Certificate` e scegliamo `Let's Encrypt`.

Inseriamo `*.domain.duckdns.org` e flagghiamo
`Use DNS Challenge`, nel menu a tendina scegliamo
`DuckDNS` e inseriamo il `token` che abbiamo copiato.
Se tutto è andato a buon fine dopo qualche secondo
avremo il nostro `Certificato SSL Wildcard`.

Andiamo poi su `Hosts` e scegliamo `Proxy Hosts`
e premiamo `Add Proxy Host`.

Nel domain inseriamo per esempio 
`nodemation.domain.duckdns.org` scegliamo 
`http`, inseriamo `nodemation` come Hostname 
e `5678` come Port e abilitiamo i Webstocks.

Nella tab SSL scegliamo il certificato che
abbiamo generato precedentemente, flagghiamo
`Force SSL` e `HTTP/2 Support`.

Torniamo poi in portainer e andiamo a modificare
lo `Stack` di Plex, aggiungendo all'inizio:

```yaml
networks:
  reverse-proxy:
    external: true
```

e al fondo del servizio che vogliamo esporre:

```yaml
    networks:
      - reverse-proxy
```

Queste operazioni vanno fatte per tutti gli stack
che si vogliono esporre tramite il reverse-proxy.

Mentre solamente per Plex la situazione è un po'
più complicata, quindi magari la tratteremo vi
rimandiamo al qualche tutorial dedicato..

Se volessimo esporre anche portainer, dobbiamo
rilanciarlo con questi comandi:

`docker stop portainer`

`docker remove portainer`

`docker run --name portainer --restart unless-stopped --net reverse-proxy -v /etc/localtime:/etc/localtime:ro -v /var/run/docker.sock:/var/run/docker.sock:ro -v portainer_data:/data -p 9000:9000 -d portainer/portainer-ce:latest`
