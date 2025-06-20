# Arquitectura Honeycomb

## 1. Introducción

Este documento describe una **arquitectura semiestructurada** que combina los beneficios de los **microservicios**, la **arquitectura hexagonal**, el patrón **honeycomb** para backend, pensando en un ecosistema completamente desarrollado en **Go**. Su objetivo es proporcionar una guía clara y aplicable para todo el equipo de desarrollo de la empresa.

## 2. Objetivos

* Presentar fundamentos y ventajas de cada patrón arquitectónico.
* Mostrar cómo aplicarlos en proyectos Go para lograr sistemas escalables, mantenibles y comprensibles.
* Ofrecer ejemplos prácticos y recomendaciones de implementación.

## 3. Visión General de la Arquitectura

La solución propuesta consta de múltiples microservicios independientes (cada uno en Go), diseñados internamente bajo los **principios hexagonales** (Ports & Adapters) y organizados en un **panel honeycomb** según dominios.

```
┌───────────┐   ┌───────────┐   ┌───────────┐
│ Servicio 1│   │ Servicio 2│ … │ Servicio N│
└───────────┘   └───────────┘   └───────────┘
     │               │               │
     └─── Honeycomb Cluster (Dominio) ───┘
```

## 4. Principios de Microservicios en Go

1. **Descomposición por Dominio**: Cada servicio refleja un contexto delimitado (DDD).
2. **Despliegue Independiente**: Binarios Go desplegados por servicio.
3. **Escalabilidad Horizontal**: Imágenes Docker por servicio y réplicas en Kubernetes.
4. **Comunicación Ligera**: HTTP/REST con **Fiber** (`gofiber/fiber/v2`) o gRPC con `google.golang.org/grpc`.

## 5. Principios de la Arquitectura Hexagonal

* **Ports (Puertos)**: Interfaces Go (`interface{}`) que definen la lógica de negocio.
* **Adapters (Adaptadores)**: Implementaciones de puertos para infraestructuras como bases de datos, colas de mensajes o HTTP.
* **Core**: Paquetes de dominio aislados de detalles técnicos.

### 5.1. Componentes Hexagonales en Go

* **`domain/`**: Entidades y lógica de negocio pura.
* **`usecase/`**: Casos de uso que orquestan entidades y puertos.
* **`port/`**: Definiciones de interfaces para entrada (HTTP/gRPC) y salida (DB/EventBus).
* **`adapter/`**: Paquetes con implementaciones:

  * **`inbound/http/`**: Handlers HTTP construidos con **Fiber** (`github.com/gofiber/fiber/v2`) para rutas y middleware.
  * **`inbound/grpc/`**: Servidor gRPC.
  * **`outbound/db/`**: Repositorios con `database/sql` o `gorm`.
  * **`outbound/event/`**: Productores/consumidores en Kafka/RabbitMQ.

## 6. Patrón Honeycomb

Organiza microservicios en "celdas" hexagonales por dominio, facilitando aislamiento y despliegue:

* Visualización clara de interdependencias.
* Aislamiento de fallos y despliegues independientes.

## 7. Estructura de Carpetas para cada Servicio en Go

```bash
go-service-name/
├─ cmd/                  # Ejecutable principal
│   └─ service/          # main.go
├─ internal/             # Código privado de la aplicación
│   ├─ domain/           # Entidades y contratos
│   ├─ usecase/          # Casos de uso
│   ├─ port/             # Interfaces
│   └─ adapter/          # Implementaciones de puertos
│       ├─ inbound/      # HTTP (Fiber), gRPC
│       └─ outbound/     # DB, mensajería
│           └─ db/       # Conexión y migración de base de datos
├─ pkg/                  # Código público (si aplica)
├─ config/               # Archivos YAML/TOML
├─ test/                 # Pruebas unitarias e integración
│   ├─ unit/             # Table-driven tests, mocks
│   └─ integration/      # Tests con httptest, DB en memoria
├─ docker/               # Archivos y configuraciones Docker-compose
│   └─ docker-compose.yml
├─ Dockerfile            # Multi-stage build para binario Go
├─ go.mod
└─ README.md
```

### 7.1 Organización de la Base de Datos

La gestión de la base de datos sigue una estructura organizada dentro del adaptador de base de datos (`adapter/outbound/db/`):

1. **Configuración de Base de Datos** (`database.go`):
   - Estructura de configuración con parámetros de conexión
   - Función de inicialización de la base de datos
   - Gestión de migraciones automáticas

2. **Repositorios** (`*_repository.go`):
   - Implementaciones de los puertos de repositorio
   - Lógica de acceso a datos específica del dominio

Esta organización asegura:
- Separación clara de responsabilidades
- Configuración centralizada de la base de datos
- Gestión de migraciones fuera del `main.go`
- Fácil mantenimiento y pruebas

## 8. Comunicación Entre Servicios

* **Síncrona**: APIs REST (`net/http`) o gRPC (`grpc-go`).
* **Asíncrona**: Mensajería con librerías como `segmentio/kafka-go` o `github.com/streadway/amqp`.

## 9. Despliegue e Infraestructura

* **Contenerización**: Docker + multi-stage builds en Go para binarios ligeros.
* **Orquestación**: Kubernetes con Helm Charts.
* **Monitorización**: `Prometheus` + `Grafana` + exportadores Go.
* **Logs**: `logrus` o `zap` con envío a ELK/EFK.

## 10. Buenas Prácticas en Go

* Mantener dominios libres de dependencias externas.
* Documentar APIs con Swagger/OpenAPI (usando `swaggo/swag`).
* **Pruebas unitarias**:

  * Usar el paquete `testing` y estructuras *table-driven* para cubrir múltiples casos.
  * Emplear *mocks* con `stretchr/testify/mock` o `gomock` para aislar dependencias externas.
  * Nombrar funciones de prueba con el patrón `TestNombreFunción_CasoDeUso`.
* **Pruebas de integración**:

  * Utilizar `net/http/httptest` para levantar servidores en memoria y probar handlers Fiber/gRPC.
  * Verificar la interacción con bases de datos usando contenedores efímeros o SQLite en memoria.
* **Cobertura y calidad**:

  * Ejecutar `go test ./... -coverprofile=coverage.out` y analizar con `go tool cover`.
  * Configurar `golangci-lint` para chequeos estáticos (gocyclo, errcheck, govet).
* **CI/CD**:

  * Incluir pasos de test y lint en pipelines (GitHub Actions, GitLab CI).
  * Configurar gates de cobertura mínima (por ejemplo, 80%) antes de mergear.

## 11. Ejemplo Práctico: Servicio de Órdenes Ejemplo Práctico: Servicio de Órdenes

1. **Definir interfaz** `port/OrderRepository.go`:

   ```go
   package port

   import "context"
   type Order struct { ID string; Amount float64 }
   type OrderRepository interface { Save(ctx context.Context, o Order) error; Find(ctx context.Context, id string) (Order, error) }
   ```
2. **Implementación** `adapter/outbound/db/postgres/OrderRepo.go` con `database/sql`.
3. **HTTP Handler** `adapter/inbound/http/OrderHandler.go` usando `chi` y `encoding/json`.
4. **Caso de uso** `usecase/CreateOrder.go` que llama a `OrderRepository` y publica evento con `kafka-go`.

## 12. Conclusión

Al usar Go en toda la plataforma, junto con microservicios, arquitectura hexagonal y honeycomb, obtenemos un sistema sólido, fácil de entender y con despliegues independientes. Sigue esta guía para iniciar tu primer servicio Go y expande el panel honeycomb.
