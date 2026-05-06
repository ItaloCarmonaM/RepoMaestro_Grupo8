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
| AuthService 		  | 8089   | db_auth     	| CRUD de Auntentificador         |

### Despliegue Técnico
- **Instancia:** AWS EC2 t3.large (Ubuntu 24.04)
- **Comando de inicio:** `docker compose up -d`

### Dependencias
| Servicio Origen | Servicio Destino | Método | Endpoint                          | Datos Requeridos (DTO)            |
| :-------------- | :--------------- | :----- | :-------------------------------- | :-------------------------------- |
| User            | Auth              | GET    |                                   | id, mail                          |
| Inventory       | Catalog          | GET    |                                   | id, name                          |
| Review          | Catalog          | GET    |                                   | id, name, author                  |
| Review          | User             | GET    |                                   | id, name, author                  |
| Cart            | Inventory        | GET    |                                   | productId, stockAvailable         |
| Order           | Cart             | GET    |                                   | userId, itemsList[]               |
| Order           | Inventory        | PUT    |                                   | productId, quantity               |
| Payment         | Order            | GET    |                                   | id, totalAmount, userId           |
| Logistics       | Order            | GET    |                                   | id, items[], totalWeight          |
| Logistics       | Payment          | GET    |                                   | orderId, paymentStatus            |
| Notification    | User             | GET    |                                   | userId, email, firstName          |
