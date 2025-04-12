# Infraestrutura de Referência

## Requisitos Mínimos

| Serviço        | CPU     | Memória RAM | Disco      | Observação                                              |
| -------------- | ------- | ----------- | ---------- |---------------------------------------------------------|
| API            | 2 vCPUs | 4 GB        | 20 GB      | Ideal em VM ou container isolado                        |
| WEB            | 1 vCPU  | 2 GB        | 10 GB      | Pode ser compartilhado com a API em ambientes pequenos  |
| Banco de Dados | 4 vCPUs | 8 GB        | SSD 100 GB | Volume persistente recomendado                          |
| Redis          | 1 vCPU  | 1 GB        | 1 GB       | Utilizado para cache                                    |
  
### Requisitos de Infraestrutura (Cloud e On-Premises)

#### Requisitos mínimos (por componente)

| Componente     | CPU     | Memória | Armazenamento | Observação                   |
| -------------- | ------- | ------- | ------------- | ---------------------------- |
| API            | 2 vCPUs | 4 GB    | 20 GB SSD     | Banco externo recomendado    |
| WEB            | 2 vCPUs | 2 GB    | 10 GB SSD     | Load Balancer recomendado    |
| Banco de Dados | 4 vCPUs | 8 GB    | 100 GB SSD    | Discos provisionados ou NVMe |
| Redis          | 1 vCPU  | 1 GB    | 2 GB          | Para cache auxiliar          |

#### Requisitos recomendados (Produção - Alta disponibilidade)

| Cloud | API           | WEB      | Banco de Dados     | Observação                |
| ----- | ------------- | -------- | ------------------ | ------------------------- |
| AWS   | t3.medium     | t3.small | r5.large + EBS gp3 | Multi-AZ e snapshots      |
| Azure | B2ms          | B1ms     | E4s v5             | Storage Premium           |
| GCP   | e2-standard-2 | e2-small | n2-standard-4      | Persistent Disks Regional |

#### Para On-Premises:

- Virtualização recomendada: Proxmox, VMware, KVM
- Load Balancer: HAProxy ou NGINX
- Storage: RAID 10 com discos SSD ou NVMe
- Backup: sistema de snapshots + offsite
