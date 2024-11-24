# Portainer

Portainer è una piattaforma di gestione container basata
su un'interfaccia utente grafica progettata per semplificare
l'amministrazione di ambienti containerizzati.

Consente agli utenti di gestire facilmente container, 
immagini, reti e volumi attraverso un'interfaccia intuitiva,
senza dover ricorrere esclusivamente alla riga di comando. 

## Creazione volume dati

Portainer ha bisogno di un volume per gestire i propri dati:

`docker volume create portainer_data`

## Esecuzione di Portainer

Dopodiche eseguiamo Portainer con il seguente comando:

`docker run --name portainer --restart unless-stopped -v /etc/localtime:/etc/localtime:ro -v /var/run/docker.sock:/var/run/docker.sock:ro -v portainer_data:/data -p 9000:9000 portainer/portainer-ce:latest`

## Accesso

A questo punto su Docker desktop vedremo il container in
esecuzione con le relative porte aperte.

Cliccando sulla porta o puntando il browser su 
`http://localhost:9000`
potremo accedere alla prima configurazione.

Scegliamo una password e proseguiamo.

Premiamo poi sul logo in alto a sinistra.

E premiamo Live Connect sull'ambiente `local`
