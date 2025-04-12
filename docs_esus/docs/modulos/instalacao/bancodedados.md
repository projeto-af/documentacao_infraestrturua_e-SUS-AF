# Banco de Dados

- Sempre utilizar armazenamento persistente.
- Implementar **backup automatizado** com `pg_dump`, `wal-g` ou serviço gerenciado.
- Se possível, utilizar PostgreSQL como serviço gerenciado (AWS RDS, GCP CloudSQL, etc).

## Backup PostgreSQL

```bash
pg_dump -U postgres -h <db_host> -F c -b -v -f /backups/backup-$(date +%F).sqlc esus
```
### Restore PostgreSQL

```bash
pg_restore -U postgres -h <db_host> -d esus -v /backups/backup-2025-03-27.sqlc
```
