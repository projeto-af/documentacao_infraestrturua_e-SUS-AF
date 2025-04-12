# Arquitetura do sistema

A arquitetura do sistema é composta de 2 partes para o sistema, que consiste na camada de apresentação denominada de  Serviço Web e o Serviço de API. O sistema é composto dos seguintes componentes:

* *Load Balance* - Componente responsável pela distribuição da carga entre máquinas
* *Serviço Web* - Componente responsável pela disponibilização da camada de apresentação da aplicação
* *Serviço de API* - Componente responsável pela disponibilização dos dados do sistema.
* *Redis* - Componente responsável pelo armazenamento dos tokens de autenticação e cache distribuído.
* *Banco de dados* - Componente de persistência de dados.  Nessa versão inicial a compatibilidade testada é com o Banco PostgreSQL.

``` text
+--------------------+        +--------------------+
|    Usuário Final   |<------>|   Load Balancer    |
+--------------------+        +--------------------+
                                     |
               +---------------------+--------------------+
               |                                          |
      +----------------+                          +--------------------+
      | Serviço WEB     |                         | Serviço API        |
      | (Nginx ou App)  |<----------------------->| REST API           |
      +----------------+                          +--------------------+
                                                     |               |
                                           +--------------+     +----------------+
                                           |  Redis       |     | Banco de Dados |
                                           | (Cache|      |     | PostgreSQL     |
									       +--------------+     +----------------+
													      
```
