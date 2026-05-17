## Integrantes: Vicente Araya, Ítalo Carmona

### Estado del Sistema (Hito 1)
| Microservicio  	  | Puerto | DB Name     	| Funcionalidad              |
| :--- 		 	        | :---   | :---        	| :---                       |
| UserService    	  | 8080   | db_user 	    | CRUD de usuario            |
| CatalogService	  | 8081   | db_catalog   | CRUD de Libro              |
| InventoryService 	| 8082   | db_inventory | CRUD de Inventario (stock) |
| CartService 		  | 8083   | db_cart      | CRUD de Carrito            |
| OrderService		  | 8084   | db_order     | CRUD de Orden de compra    |
| PaymentService 		| 8085   | db_payment   | CRUD de Pago               |
| LogisticService 	| 8086   | db_logistics | CRUD de Envío              |
| CommService		    | 8087   | db_comms 	  | CRUD de Comms              |
| ReviewService 	  | 8088   | db_review 	  | CRUD de Review             |
| AuthService 		  | 8089   | db_auth     	| CRUD de Auntentificador    |

### Despliegue Técnico
- **Instancia:** AWS EC2 t3.large (Ubuntu 24.04)
- **Comando de inicio:** `docker compose up -d`

### Diagrama de dependencias
<img width="669" height="321" alt="Diagrama sin título drawio (5)" src="https://github.com/user-attachments/assets/251635cf-2733-4345-b56c-bfe32618ef0a" />

### Tabla de contratos
| Servicio Origen                      | Servicio Destino                       | Método | Endpoint (GET)               | Datos esperados (DTO)                           |
| :----------------------------------- | :------------------------------------- | :----- | :--------------------------- | :---------------------------------------------- |
| **User**                             | **Auth**                               | GET    | `/api/v1/auth/{id}`          | `id, correo`                                    |
| **Inventory**                        | **Catalog**                            | GET    | `/api/v1/catalog/{id}`       | `id, titulo`                                    |
| **Review**                           | **Catalog**                            | GET    | `/api/v1/catalog/{id}`       | `id, titulo, autor`                             |
| **Review**                           | **User**                               | GET    | `/api/v1/user/{id}`          | `id, nombreCompleto`                            |
| **Cart**                             | **Catalog**                            | GET    | `/api/v1/catalog/{id}`       | `id, titulo, precio`                            |
| **Cart**                             | **Inventory**                          | GET    | `/api/v1/inventory/{prodId}` | `id, stockQuantity`                             |
| **Cart**                             | **User**                               | GET    | `/api/v1/user/{id}`          | `id, nombreCompleto`                            |
| **Order**                            | **User**                               | GET    | `/api/v1/user/{userId}`      | `id, nombreCompleto, direccion`                 |
| **Order**                            | **Cart**                               | GET    | `/api/v1/cart/{userId}`      | `userId, items[{id, titulo, precio, cantidad}]` |
| **Payment**                          | **Order**                              | GET    | `/api/v1/orders/{id}`        | `id, totalAmount, userId`                       |
| **Logistics**                        | **Order**                              | GET    | `/api/v1/orders/{id}`        | `id, items[{titulo, editorial}]`                |
| **Logistics**                        | **User**                               | GET    | `/api/v1/user/{userId}`      | `id, direccion, numeroTelefono`                 |
| **Notification**                     | **User**                               | GET    | `/api/v1/user/{userId}`      | `id, correo, nombreCompleto`                    |
| **Notification**                     | **Order**                              | GET    | `/api/v1/orders/{id}`        | `id, status, orderDate`                         |

### Tecnología utilizada

* **Cliente REST: OpenFeign (Feign Client)**:
Se seleccionó OpenFeign debido a su enfoque declarativo, el cual permite modelar las comunicaciones HTTP entre los microservicios implementados mediante interfaces simples anotadas. Esto separa la lógica de negocio de los detalles de infraestructura de red, reduce el código repetitivo, facilita la inyección de dependencias mediante `@Autowired` y se integra de forma nativa con Spring Cloud.

* **Manejo de errores centralizado:** Implementación de un componente global `@ControllerAdvice` encargado de interceptar de manera síncrona las excepciones en toda la capa Web de los servicios. Se configuraron excepciones personalizadas (tales como `RecursoNoEncontradoException` y `ServicioNoDisponibleException`) combinadas con la captura de `FeignException`. Esto garantiza que los fallos de comunicación inter-servicio (como códigos 404 o 500 en las instancias EC2 externas) se transformen en respuestas JSON homogéneas y con estados HTTP semánticos (`404 Not Found`, `503 Service Unavailable`), evitando exponer las trazas internas del servidor al cliente.

* **Trazabilidad y Logs:** Utilización de la fachada **SLF4J** combinada con la configuración interna de Spring Boot para el registro de eventos en tiempo real. Cada flujo y petición distribuida cuenta con un sistema de logs estructurado que registra de manera explícita el inicio de las validaciones, los datos enviados, la confirmación exitosa de los microservicios remotos y alertas detalladas ante inconsistencias en el inventario o caídas de red. Esto optimiza los tiempos de auditoría y auditorías de rendimiento en ambientes AWS.

* **Pruebas de integración:** El flujo completo de integración y comunicación entre los diferentes endpoints del ecosistema se encuentra automatizado y documentado en la colección de Postman ubicada en la ruta local: `/postman/hito2-integracion.json`. Esta suite valida secuencialmente los métodos `POST`, `GET`, `PATCH` y `DELETE`, certificando la consistencia de los datos distribuidos.

### Escenario de despliegue
- [ ] Escenario A — Todos los servicios en una sola instancia EC2
- [x] Escenario B — Servicios distribuidos en múltiples instancias EC2: 
El sistema se encuentra desplegado de forma Mixta, ya que en cada instancia hay 5 microservicios a los cuales los levanta el mismo docker compose, teniendo el proyecto un total de dos docker compose.

#### 1. Distribución de IPs y Puertos por Servicio

| Instancia AWS EC2 | Microservicio | Puerto Base | Rol / Responsabilidad |
| :--- | :--- | :--- | :--- |
| **EC2 Nodo A**<br>IP Pública: `3.231.18.119` | `catalog_service`<br>`inventory_service`<br>`cart_service`<br>`order_service`<br>`review_service` | `8081`<br>`8082`<br>`8083`<br>`8084`<br>`8088` | Gestión de productos, control de existencias en tiempo real, persistencia de carritos activos, procesamiento de órdenes de compra y reseñas de usuarios. |
| **EC2 Nodo B**<br>IP Pública: `44.196.219.155` | `user_service`<br>`payment_service`<br>`logistic_service`<br>`comm_service`<br>`auth_service` | `8080`<br>`8085`<br>`8086`<br>`8087`<br>`8089` | Maestro de usuarios, pasarela de pagos simulada, despacho/logística, módulo de notificaciones y servidor de autenticación y seguridad. |

#### 2. Security Groups Configurados
- [x] **SÍ**

**Detalle de reglas de seguridad aplicadas:**
* **Reglas de Entrada (Inbound):** Se habilitaron los puertos de manera selectiva (`8080` al `8089`) mediante protocolo TCP para permitir la comunicación bidireccional por HTTP entre ambas IPs públicas (comunicaciones Feign Client). Adicionalmente, se configuró acceso restringido al puerto `22` (SSH) solo para IPs autorizadas de desarrollo y administración del servidor.
* **Reglas de Salida (Outbound):** Configuración *Full Access* (`0.0.0.0/0`) en ambas instancias para permitir la descarga de dependencias Maven, conexión con repositorios remotos de Docker y salida estándar de logs.
