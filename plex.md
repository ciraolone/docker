# Plex

Plex è una piattaforma multimediale che permette
di organizzare, gestire e riprodurre contenuti 
digitali come film, serie TV, musica, foto e 
podcast. 

Funziona tramite un server centrale che cataloga
i file multimediali, rendendoli accessibili da 
diversi dispositivi, inclusi smartphone, smart 
TV, computer e console. Oltre alla gestione di 
contenuti personali, Plex offre un servizio di
streaming con film gratuiti, canali TV in diretta
e integrazione con altri servizi di abbonamento. 

Grazie alla sua interfaccia intuitiva e alle 
funzionalità avanzate come il transcodificatore 
integrato, Plex garantisce un’esperienza 
multimediale personalizzata e di alta qualità 
ovunque ci si trovi.

## Stack

```yaml
volumes:
  config:
    driver: local

services:
  plex:
    image: plexinc/pms-docker
    container_name: plex
    hostname: plex
    restart: unless-stopped
    environment:
      - TZ=Europe/Rome
      - HOSTNAME=plexserver
      - PLEX_CLAIM=XXX
    volumes:
      - config:/config
      - XXX:/data
    ports:
      - 8324:8324/tcp
      - 32400:32400/tcp
      - 32469:32469/tcp
      - 1900:1900/udp
      - 32410:32410/udp
      - 32412:32412/udp
      - 32413:32413/udp
      - 32414:32414/udp
```

Per ottenere il valore da inserire in claim
accedere dal sito
`https://www.plex.tv/claim`

Per quanto riguarda il volume data va inserita
la cartella in cui salveremo i file multimediali
ad esempio sotto windows potremo avere
`C:\\plex_data` o su mac/linux `/plex_data`
cosi da poter poi facilmente caricare i files.

Per accedere all'interfaccia web aprire il browser su
`http://localhost:32400/web`
