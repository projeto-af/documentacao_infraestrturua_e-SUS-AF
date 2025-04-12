# Troubleshooting

| Problema                 | Causa Comum                      | Solução                                            |
| ------------------------ | -------------------------------- | -------------------------------------------------- |
| API não conecta no Banco | Variáveis de ambiente incorretas | Verifique a definição de env vars                  |
| Porta não acessível      | Firewall bloqueando              | Libere as portas 8000 e 3001                       |
| Load Balancer não sobe   | Cluster mal configurado          | Verifique logs e provisionamento do cloud provider |

## Comandos úteis

| Função                  | Comando                                     |
| ----------------------- | ------------------------------------------- |
| Verificar logs (Docker) | `docker-compose logs -f`                    |
| Verificar containers    | `docker ps`                                 |
| Verificar pods (K8s)    | `kubectl get pods`                          |
| Logs de um pod          | `kubectl logs <pod>`                        |
| Acesso ao banco         | `psql -h <db_host> -U <user> -d <database>` |

## Diagnóstico de problemas comuns

- Variáveis de ambiente ausentes ou mal configuradas

- Consumo elevado de recursos

- Falha de conectividade entre serviços

- Volume não persistente no banco de dados
