# Instalação com Kubernetes

## Deployment WEB

`web-deployment.yaml`

```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web
spec:
  replicas: 2
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
    spec:
      containers:
      - name: web
        image: ghcr.io/projeto-af/e-susaf/e-susaf-web:latest
        env:
        - name: VAR__API_URL
          value: "http://api:3001"
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: web
spec:
  type: LoadBalancer
  selector:
    app: web
  ports:
  - port: 80
    targetPort: 80

```

---

## Deployment API

`api-deployment.yaml`
```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: api
spec:
  replicas: 3
  selector:
    matchLabels:
      app: api
  template:
    metadata:
      labels:
        app: api
    spec:
      containers:
      - name: api
        image: ghcr.io/projeto-af/e-susaf/e-susaf-api:latest
        env:
        - name: DB_HOST
          value: "db"
        - name: PORT
          value: "3001"
        ports:
        - containerPort: 3001
---
apiVersion: v1
kind: Service
metadata:
  name: api
spec:
  selector:
    app: api
  ports:
  - port: 3001
    targetPort: 3001

```

---

## StatefulSet Banco de Dados

Para o banco de dados, a versão preliminar seria um PostgreSQL.  Recomendamos um serviço gerenciado de banco de dados.  Se necessário por economia ou para disponibilizar um ambiente menor para treinamento ou homologação do sistema, pode-se instalar um banco de dados diretamente no cluster.  Segue um exemplo do yaml para isso.


`db-statefulset.yaml`

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: db
spec:
  serviceName: "db"
  replicas: 1
  selector:
    matchLabels:
      app: db
  template:
    metadata:
      labels:
        app: db
    spec:
      containers:
      - name: postgres
        image: postgres:15
        env:
        - name: POSTGRES_DB
          value: esus
        - name: POSTGRES_USER
          valueFrom:
            secretKeyRef:
              name: postgres-secret
              key: user
        - name: POSTGRES_PASSWORD
          valueFrom:
            secretKeyRef:
              name: postgres-secret
              key: password
        ports:
        - containerPort: 5432
        volumeMounts:
        - name: postgres-pv
          mountPath: /data/postgres
  volumeClaimTemplates:
  - metadata:
      name: postgres-pv
    spec:
      accessModes: ["ReadWriteOnce"]
      resources:
        requests:
          storage: 100Gi

```
