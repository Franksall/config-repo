# Repositorio de Configuraciones (config-repo)

Este repositorio actúa como el **almacén centralizado de configuración** del sistema de pedidos reactivo.  
Es utilizado por el `ms-config-server` para proveer configuraciones externas a cada microservicio.

---

##  Estructura

```
config-repo/
├── ms-productos-dev.yml
├── ms-productos-qa.yml
├── ms-productos-prd.yml
├── ms-pedidos-dev.yml
├── ms-pedidos-qa.yml
└── ms-pedidos-prd.yml
```

---

##  Ejemplo de Archivo (`ms-productos-dev.yml`)
```yaml
server:
  port: 8081
spring:
  r2dbc:
    url: r2dbc:postgresql://localhost:5432/db_productos_dev
    username: postgres
    password: [TU_PASSWORD_POSTGRES]
```

