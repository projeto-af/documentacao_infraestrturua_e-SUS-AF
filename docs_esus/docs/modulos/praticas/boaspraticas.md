# Boas Práticas

- Agende backups diários com `cron`.
- Armazene em local externo (cloud ou storage dedicado).
- Mantenha ao menos 7 dias de retenção e uma cópia off-site.

## Segurança e Boas Práticas

- Nunca armazene segredos no `docker-compose.yml` ou manifests do Kubernetes. Utilize:
    - Docker Secrets
    - Kubernetes Secrets + Vault (Hashicorp) ou SOPS

- Acesso SSH restrito e auditado
- Backup criptografado
- Use TLS para todas as comunicações (Ingress, API, DB)
- Configurar políticas de rede (NetworkPolicy no Kubernetes ou firewalls em Docker)
- Configurar autenticação e RBAC nos clusters

## Monitoramento e Logging

Para garantir a estabilidade, performance e segurança do sistema, o monitoramento contínuo e o logging centralizado são indispensáveis.

### Requisitos de Monitoramento

- **Métricas essenciais**:
    - CPU, memória, disco e rede de todos os serviços
    - Latência e disponibilidade da API
    - Status do banco de dados (replicação, consumo de espaço, performance de queries)
    - Load Balancer (tempo de resposta, erros 4xx/5xx)


### Stack Recomendada

- **Prometheus + Grafana**: coleta e visualização de métricas.
- **Loki ou ELK Stack (Elasticsearch, Logstash, Kibana)**: logging centralizado.
- **Alertmanager**: envio de alertas por e-mail, Slack ou outros canais.


### Boas Práticas

- Retenção de logs adequada (mínimo 30 dias em produção).
- Logs armazenados fora da máquina principal (volumes ou serviços externos).
- Alertas de thresholds críticos bem definidos.

### Testes de funcionalidade

- Consumo de API via Postman ou Insomnia
- Fluxo de autenticação e funcionalidades principais


### Testes de carga (opcional)

- `k6`, `wrk`, `locust` para simular usuários simultâneos


### Verificação de monitoramento e alertas

- Simular falha de serviço para validar recebimento de alertas
- Verifique status:
