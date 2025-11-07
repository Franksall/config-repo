# Repositorio de Configuraciones (config-repo)

Este repositorio actúa como la **fuente única de verdad** para la configuración de todos los microservicios del sistema.

El `ms-config-server` (que corre en Docker) está configurado para clonar este repositorio de GitHub. Cuando cualquier otro microservicio (como `ms-pedidos`) arranca, le pregunta al `ms-config-server` por su configuración, y éste le entrega el archivo `.yml` correspondiente (ej. `ms-pedidos-dev.yml`).

---

## 1. Archivos de Configuración de Servicios

Estos archivos contienen las propiedades que cada microservicio necesita para funcionar *dentro* del entorno Docker.

* `registry-service.yml`: Configuración de Eureka (puerto 8099, etc.).
* `gateway-service.yml`: Configuración del API Gateway (puerto 8080, reglas de enrutamiento).
* `ms-pedidos-dev.yml`: Configuración de desarrollo para Pedidos (conexión a BD, Eureka).
* `ms-productos-dev.yml`: Configuración de desarrollo para Productos (conexión a BD, Eureka).
* *(...y los archivos de `qa` y `prd`)* Lo cual soloe esta configura el profiel dev

---

## 2. Archivos de Despliegue (Importante)

Para mantener todo el proyecto en un solo lugar, este repositorio también almacena los archivos maestros para levantar todo el sistema con Docker.

* `docker-compose.yml`
* `database.sql`: El script de inicialización que crea las tablas y datos de prueba.

> **¡Instrucciones de Despliegue!**
> Para ejecutar el proyecto, debes **copiar** `docker-compose.yml` y `database.sql` desde esta carpeta (`config-repo`) y **pegarlos en la carpeta raíz** (`SistemaDePedidos`), que es donde se encuentran todas las carpetas de los microservicios, en mi monito le puse asi.
>
> La estructura de carpetas final debe verse así:
> ```
> SistemaDePedidos/
> ├── 📄 docker-compose.yml  <-- (Copiado de config-repo)
> ├── 📄 database.sql        <-- (Copiado de config-repo)
> ├── 📁 config-repo/
> ├── 📁 gateway-service/
> ├── 📁 ms-config-server/
> ├── 📁 ms-pedidos/
> ├── 📁 ms-productos/
> └── 📁 registry-service/
> ```

---

## 3. Detalles de Conexión (Dentro de Docker)

Los archivos `.yml` de este repositorio están diseñados para funcionar dentro de la red privada de Docker (`pedidos-net`) creada por `docker-compose.yml`.

En esta red, los contenedores **no usan `localhost`** para encontrarse. En su lugar, usan los **nombres de los servicios** definidos en `docker-compose.yml`.

### Conexión a la Base de Datos
Así es como `ms-pedidos` y `ms-productos` encuentran la base de datos:

**Archivo:** `ms-pedidos-dev.yml` / `ms-productos-dev.yml`
```yaml
spring:
  r2dbc:
    # "postgres-db" es el nombre del servicio en docker-compose.yml
    url: r2dbc:postgresql://postgres-db:5432/sistema_pedidos_db
    username: postgres
    password: postgres