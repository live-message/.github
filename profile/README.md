### Быстрый запуск проекта

```yml
services:
  chat:
    image: ghcr.io/live-message/chat:latest
    container_name: lime
    restart: unless-stopped
    ports:
      - "2000:80"
  api:
    image: ghcr.io/live-message/api-app:latest
    env_file: .env
    container_name: api
    restart: unless-stopped
    networks:
      - lime
    ports:
      - "2001:8000"
  db:
    image: postgres:latest
    restart: unless-stopped
    environment:
      POSTGRES_USER: app
      POSTGRES_PASSWORD: app
      POSTGRES_DB: live-message
    ports:
      - "2002:5432"
    volumes:
      - /data/databases/falbue/test/postgres:/var/lib/postgres
    networks:
      - lime
  signaling:
    restart: unless-stopped
    
    image: ghcr.io/live-message/api-signaling:latest
    ports:
      - "2003:8080"

  coturn:
    image: coturn/coturn:latest
    container_name: coturn
    restart: unless-stopped
    network_mode: host
    volumes:
      - ./turnserver.conf:/etc/turnserver.conf:ro
      - /var/lib/caddy/.local/share/caddy/certificates:/etc/caddy-certs:ro

networks:
  lime:

```
