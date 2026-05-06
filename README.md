# Proyecto Semestral: Sistema de Gestión de Librería
## Integrantes: Vicente Araya, Ítalo Carmona

### Estado del Sistema (Hito 1)
| Microservicio  	  | Puerto | DB Name     	| Funcionalidad              |
| :--- 		 	        | :---   | :---        	| :---                       |
| UserService    	  | 8080   | db_user 	    | CRUD de usuario            |
| CatalogService	  | 8081   | db_catalog   | CRUD de Libro              |
| InventoryService 	| 8082   | db_inventory | CRUD de Inventario (stock) |
| CartService 		  | 8083   | db_    	    | CRUD de [Entidad]          |
| OrderService		  | 8084   | db_ 	        | CRUD de [Entidad]          |
| PaymentService 		| 8085   | db_	 	      | CRUD de [Entidad]          |
| LogisticService 	| 8086   | db_ 	        | CRUD de [Entidad]          |
| CommService		    | 8087   | db_ 	        | CRUD de [Entidad]          |
| ReviewService 	  | 8088   | db_ 	        | CRUD de Review             |
| AuthService 		  | 8089   | db_auth     	| CRUD de Auntentificador    |

### Despliegue Técnico
- **Instancia:** AWS EC2 t3.large (Ubuntu 24.04)
- **Comando de inicio:** `docker compose up -d`

### Dependencias
| Servicio Origen                      | Servicio Destino                       | Método | Endpoint (GET)               | Datos esperados (DTO)                           |
| :----------------------------------- | :------------------------------------- | :----- | :--------------------------- | :---------------------------------------------- |
| **User**                             | **Auth**                               | GET    | `/api/v1/auth/{id}`          | `id, email`                                     |
| **Inventory**                        | **Catalog**                            | GET    | `/api/v1/catalog/{id}`       | `id, titulo`                                    |
| **Review**                           | **Catalog**                            | GET    | `/api/v1/catalog/{id}`       | `id, titulo, autor`                             |
| **Review**                           | **User**                               | GET    | `/api/v1/user/{id}`          | `id, firstName, lastName`                       |
| **Cart**                             | **Catalog**                            | GET    | `/api/v1/catalog/{id}`       | `id, titulo, precio`                            |
| **Cart**                             | **Inventory**                          | GET    | `/api/v1/inventory/{prodId}` | `productId, stockQuantity`                      |
| **Order**                            | **User**                               | GET    | `/api/v1/user/{userId}`      | `id, firstName, lastName, address`              |
| **Order**                            | **Cart**                               | GET    | `/api/v1/cart/{userId}`      | `userId, items[{id, titulo, precio, cantidad}]` |
| **Payment**                          | **Order**                              | GET    | `/api/v1/orders/{id}`        | `id, totalAmount, userId`                       |
| **Logistics**                        | **Order**                              | GET    | `/api/v1/orders/{id}`        | `id, items[{titulo, editorial}]`                |
| **Logistics**                        | **User**                               | GET    | `/api/v1/user/{userId}`      | `id, address, phoneNumber`                      |
| **Notification**                     | **User**                               | GET    | `/api/v1/user/{userId}`      | `id, email, firstName`                          |
| **Notification**                     | **Order**                              | GET    | `/api/v1/orders/{id}`        | `id, status, orderDate`                         |
