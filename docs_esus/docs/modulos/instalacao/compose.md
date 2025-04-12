# Instalação com Docker Compose

Crie o arquivo `docker-compose.yml`:

``` yml
version: '3.5'

services:
  web:
    container_name: web
    image: ghcr.io/projeto-af/e-susaf/e-susaf-web:latest
    ports:
      - "8000:80"
    environment:
      VAR__API_URL: "http://api:3001"

  api:
    container_name: api
    image: ghcr.io/projeto-af/e-susaf/e-susaf-api:latest
    ports:
      - "3001:3001"
    environment:
      DB_HOST: db
      PORT: "3001"

  db:
    container_name: db
    image: postgres:15
    environment:
      POSTGRES_DB: esus
      POSTGRES_USER: ${POSTGRES_USER}
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
      PGDATA: /data/postgres
    volumes:
      - ../database/postgres:/docker-entrypoint-initdb.d
      - postgres:/data/postgres
    ports:
      - "5432:5432"
    restart: unless-stopped

  redis:
    image: registry.redict.io/redict:latest
    container_name: redis
    restart: always
    ports:
      - "${REDIS_PORT}:6379"
    environment:
      REDICT_PASSWORD: ${REDIS_PASSWORD}
    volumes:
      - redis_data:/data

volumes:
  postgres:
  redis_data:

```


### Execução

Para executar, siga com o comando:
```bash
docker-compose up -d
```
