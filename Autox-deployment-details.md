# **Autox Deployments Services Details**

## **Autox Services**

| Service Name                                     | Kind       | Storage | NodePort | NodeSelector |
| ------------------------------------------------ | ---------- | ------- | -------- | ------------ |
| 1.Nginx pod ("Autox-frontend", "Agent-builder-frontend") | Deployment | None    | NodePort | None         |
| 2.Authentication                                   | Deployment | None    | None     | None         |
| 3.Camel                                            | Deployment | None    | None     | None         |
| 4.Communication                                    | Deployment | None    | None     | None         |
| 5.Customevaluator                                  | Deployment | None    | None     | None         |
| 6.Gateway                                          | Deployment | None    | None     | None         |
| 7.Resource                                         | Deployment | None    | None     | None         |
| 8.Statemachine-src                                 | Deployment | None    | None     | None         |
| 9.Statemahcine-celery                              | Deployment | None    | None     | None         |
| 10.orchestration-node                               | Deployment | None    | None     | None         |
| 11.Agent-builder-backend                            | Deployment | pv/pvc  | None     | NodeSelector |
| 12.Agent-observer-frontend                          | Deployment | None    | None     | None         |
| 13.Agent-observer-backend                           | Deployment | None    | None     | None         |

---

## **Autox Tools**

| Tool Name  | Kind        | Storage | NodePort | NodeSelector |
| ---------- | ----------- | ------- | -------- | ------------ |
| 14.Mongodb    | StatefulSet | pv/pvc  | None     | NodeSelector |
| 15.RabbitMQ   | StatefulSet | pv/pvc  | None     | NodeSelector |
| 16.Clickhouse | Deployment  | pv/pvc  | None     | NodeSelector |
| 17.OPA        | Deployment  | pv/pvc  | None     | NodeSelector |
| 18.Keycloak   | Deployment  | pv/pvc  | None     | NodeSelector |
| 19.Minio      | Deployment  | pv/pvc  | None     | NodeSelector |
| 20.MySQL      | Deployment  | pv/pvc  | None     | NodeSelector |
| 21.Postgres   | Deployment  | pv/pvc  | None     | NodeSelector |
| 22.Redis      | Deployment  | pv/pvc  | None     | NodeSelector |

---


