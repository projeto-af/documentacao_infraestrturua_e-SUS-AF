# Load Balancer e Redundância

- O serviço `web` deve expor `type: LoadBalancer` ou ser gerenciado via `Ingress Controller`.

- Replique a `API` e o `WEB` para obter alta disponibilidade.

- Utilize Redis e PostgreSQL com estratégias de failover (Redis Sentinel e Patroni são recomendados).
