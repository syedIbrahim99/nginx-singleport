# **Autox Deployments Services Details**

## **Autox Services**

| Service Name                                     | Kind       | Storage | NodePort | NodeSelector |
| ------------------------------------------------ | ---------- | ------- | -------- | ------------ |
| Nginx pod ("Autox-frontend", "Agent-builder-frontend") | Deployment | None    | NodePort | None         |
| Authentication                                   | Deployment | None    | None     | None         |
| Camel                                            | Deployment | None    | None     | None         |
| Communication                                    | Deployment | None    | None     | None         |
| Customevaluator                                  | Deployment | None    | None     | None         |
| Gateway                                          | Deployment | None    | None     | None         |
| Resource                                         | Deployment | None    | None     | None         |
| Statemachine-src                                 | Deployment | None    | None     | None         |
| Statemahcine-celery                              | Deployment | None    | None     | None         |
| orchestration-node                               | Deployment | None    | None     | None         |
| Agent-builder-backend                            | Deployment | pv/pvc  | None     | NodeSelector |
| Agent-observer-frontend                          | Deployment | None    | None     | None         |
| Agent-observer-backend                           | Deployment | None    | None     | None         |

---

## **Autox Tools**

| Tool Name  | Kind        | Storage | NodePort | NodeSelector |
| ---------- | ----------- | ------- | -------- | ------------ |
| Mongodb    | StatefulSet | pv/pvc  | None     | NodeSelector |
| RabbitMQ   | StatefulSet | pv/pvc  | None     | NodeSelector |
| Clickhouse | Deployment  | pv/pvc  | None     | NodeSelector |
| OPA        | Deployment  | pv/pvc  | None     | NodeSelector |
| Keycloak   | Deployment  | pv/pvc  | None     | NodeSelector |
| Minio      | Deployment  | pv/pvc  | None     | NodeSelector |
| MySQL      | Deployment  | pv/pvc  | None     | NodeSelector |
| Postgres   | Deployment  | pv/pvc  | None     | NodeSelector |
| Redis      | Deployment  | pv/pvc  | None     | NodeSelector |

---



