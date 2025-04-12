# Backup Redict

O Redict salva automaticamente no arquivo `dump.rdb` configurado em `redis.conf`.

Para garantir um backup diário, armazene o arquivo rdb:

```bash
BGSAVE cp /data/dump.rdb /backup/redis-dump-$(date +%F).rdb
```
