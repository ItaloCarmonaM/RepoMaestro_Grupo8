# Proyecto Semestral: Sistema de Gestión de Librería
## Integrantes: Vicente Araya, Ítalo Carmona

### Estado del Sistema (Hito 1)
| Microservicio  | Puerto | DB Name | Funcionalidad     |
| :--- 		 | :---   | :---    | :---              |
| CatalogService | 8080   | auth_db | CRUD de Libro     |
| ReviewService	 | 8081   | [DB]    | CRUD de Review    |
| [Nombre 3] 	 | 8082   | [DB]    | CRUD de [Entidad] |
| [Nombre 3] 	 | 8083   | [DB]    | CRUD de [Entidad] |
| [Nombre 3] | 8084 | [DB] | CRUD de [Entidad] |
| [Nombre 3] | 8085 | [DB] | CRUD de [Entidad] |
| [Nombre 3] | 8086 | [DB] | CRUD de [Entidad] |
| [Nombre 3] | 8087 | [DB] | CRUD de [Entidad] |
| [Nombre 3] | 8088 | [DB] | CRUD de [Entidad] |
| [Nombre 3] | 8089 | [DB] | CRUD de [Entidad] |

### Despliegue Técnico
- **Instancia:** AWS EC2 t3.large (Ubuntu 24.04)
- **Comando de inicio:** `docker compose up -d`