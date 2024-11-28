# Nodemation (n8n)

Nodemation (n8n) è una piattaforma open source 
per l'automazione dei processi e l'integrazione
di applicazioni, progettata per essere altamente
flessibile e personalizzabile. 

Permette agli utenti di creare flussi di lavoro
automatizzati tramite un'interfaccia visiva intuitiva, 
collegando tra loro diverse applicazioni e API senza 
bisogno di codice complesso. 

Con supporto per centinaia di integrazioni predefinite
e la possibilità di estendere le funzionalità tramite
codice personalizzato, n8n è ideale per gestire attività 
ripetitive, sincronizzare dati o costruire automazioni
complesse.

## Stack

```yaml
volumes:
  data:
    driver: local

services:
  nodemation:
    image: n8nio/n8n
    container_name: nodemation
    hostname: nodemation
    restart: unless-stopped
    environment:
      - N8N_BASIC_AUTH_ACTIVE=true
      - N8N_BASIC_AUTH_USER=admin
      - N8N_BASIC_AUTH_PASSWORD=password
      - N8N_HOST=localhost
      - N8N_PORT=5678
      - N8N_PROTOCOL=http
      - NODE_ENV=production
      - WEBHOOK_URL=https://localhost/
      - GENERIC_TIMEZONE=Europe/Rome
    volumes:
      - data:/home/node/.n8n
    ports:
      - 5678:5678
```
  