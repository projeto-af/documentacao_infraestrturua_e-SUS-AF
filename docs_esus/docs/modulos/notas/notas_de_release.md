# Notas de Release

Este documento contém o histórico de lançamentos do e-SUS AF, detalhando novas funcionalidades, correções de bugs e instruções necessárias para atualização e migração.

---

## [v1.1.1] - 2026-03-02

### 🚀 Novidades
- **Informação de Óbito do Usuário SUS**: Adicionada funcionalidade para registrar informações de óbito do usuário SUS.
- **Cadastro e Gestão de Profissionais de Saúde (Prescritores)**: Adiciona recurso para gerenciar informações dos profissionais prescritores no sistema.
- **Exportação em Excel do Relatório de Histórico de Dispensação do Usuário SUS**: Permite exportar dados do histórico de dispensação em formato Excel para facilitar análises e relatórios.

### 🛠 Correções
- Correção de falhas que impactavam fluxos críticos como óbito, dispensação e gestão de perfis.  
- Melhorias nos relatórios gerenciais, com adequações de layout e geração em formato Excel.

### ⚠️ Mudanças Críticas (Breaking Changes)
- Adição da variável de ambiente `MAIL_SECURE` para habilitar ou desabilitar o TLS para e-mail (implementação de compatibilidade com STARTTLS).
- Adição de novas entidades de banco de dados.

---

## 📋 Instruções de Atualização

Para atualizar para esta versão, siga os passos abaixo de acordo com sua infraestrutura.

### 1. Atualização de Imagens (Registry)
As novas imagens estão disponíveis no Gitlab Container Registry. Atualize seu arquivo de implantação com as tags específicas da versão.

**Docker Compose:**
No arquivo `docker-compose.yml`, altere as tags das imagens:
```yaml
services:
  web:
    image: gitlab.inova-af.dev.br:5050/inova-af/esus-af/e-susaf-web:v1.1.1
  api:
    image: gitlab.inova-af.dev.br:5050/inova-af/esus-af/e-susaf-api:v1.1.1
```

**Kubernetes:**
Atualize os deployments via kubectl:
```bash
kubectl set image deployment/esus-web web=gitlab.inova-af.dev.br:5050/inova-af/esus-af/e-susaf-web:v1.1.1
kubectl set image deployment/esus-api api=gitlab.inova-af.dev.br:5050/inova-af/esus-af/e-susaf-api:v1.1.1
```

### 2. Migração de Banco de Dados
Esta versão contém migrações de esquema que devem ser aplicadas **antes** de subir a nova versão da aplicação.

**Passo 1: Backup de Segurança**
Sempre realize um backup antes de iniciar a migração:
```bash
pg_dump -U postgres -h <db_host> -F c -b -v -f backup_antes_v1.0.5.sqlc esus
```

**Passo 2: Execução das Migrações**
As migrações deverão ser executadas manualmente, respeitando a ordem de execução definida no repositório database/postgres.

!!! danger "ATENÇÃO"
    Os arquivos DDL do e-SUS AF estão no repositório em `/database/postgres` e os arquivos de DML estão no repositório `database/data`.  
    Os arquivos estão organizados em ordem cronológica. Para o sucesso da migração, siga a ordem cronológica dos arquivos partindo do arquivo subsequente ao último executado.

### 3. Verificação
Após verificar a última atualização em banco de dados, execute em ordem os arquivos DDL e DML. Por exemplo, se a versão implantada em sua infraestrutura é a `v1.0.5` e todos os arquivos foram executados com sucesso para a versão em questão (deverão ter sido executados os arquivos e anteriores aos citados a seguir: DML - 20251027211540-Ajustes-dominios-bug.sql e DDL - 20250815104637-configuracao_si_bnafar.sql), execute os arquivos DDL e DML listados abaixo na ordem em que estão apresentados.

#### DDL
- 20251117205241-ajuste_prescritor_conselho.sql
- 20260115174000-add_sigla_tipo_movimentacao.sql
- 20260227133926-ajuste-numero-conselho-crf.sql

#### DML

- 20251201210012-permissoes-prescritor.sql
- 20260115174001-update_siglas_tipo_movimentacao.sql
- 20260122104835-resources-config-sibnafar.sql
- 20260122163945-Insert-Config-SIBNAFAR.sql
- 20260227174856-Insert_entrada_por_doacao.sql

---

## 🔄 Plano de Rollback
Caso ocorra algum erro crítico após a atualização:
1. Reverta as imagens para a versão anterior (sugerida a `v1.0.5`).
2. Se houve alteração destrutiva no banco, restaure o backup realizado no Passo 1:
   ```bash
   pg_restore -U postgres -h <db_host> -d esus -v backup_antes_v1.0.5.sqlc
   ```
3. Entre em contato com a equipe do Inova-AF para apoio e suporte.

---

