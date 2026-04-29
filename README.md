# Proyecto Semestral: Sistema de Gestión de Librería
## Integrantes: Vicente Araya, Ítalo Carmona

### Estado del Sistema (Hito 1)
| Microservicio  	| Puerto | DB Name 	| Funcionalidad              |
| :--- 		 	| :---   | :---    	| :---                       |
| UserService    	| 8080   | db_user 	| CRUD de usuario            |
| CatalogService	| 8081   | db_catalog   | CRUD de Libro              |
| InventoryService 	| 8082   | db_inventory | CRUD de Inventario (stock) |
| [Nombre 3] 		| 8083   | [DB]    	| CRUD de [Entidad]          |
| [Nombre 3] 		| 8084   | [DB] 	| CRUD de [Entidad]          |
| [Nombre 3] 		| 8085   | [DB]	 	| CRUD de [Entidad]          |
| [Nombre 3] 		| 8086   | [DB] 	| CRUD de [Entidad]          |
| [Nombre 3] 		| 8087   | [DB] 	| CRUD de [Entidad]          |
| ReviewService 	| 8088   | [DB] 	| CRUD de Review             |
| AuthService 		| 8089   | db_auth 	| CRUD de Auntentificador         |

### Despliegue Técnico
- **Instancia:** AWS EC2 t3.large (Ubuntu 24.04)
- **Comando de inicio:** `docker compose up -d`