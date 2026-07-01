# Backend 1 - User Service

API REST en Node.js para gestión de usuarios, desplegada en AWS ECS Fargate con base de datos MySQL en Amazon RDS.

## Arquitectura

- **Tecnología**: Node.js 18 + Express + MySQL2
- **Contenedor**: Docker
- **Orquestación**: AWS ECS Fargate
- **Base de datos**: Amazon RDS MySQL 8.0 (`users_db`)
- **Secrets**: Credenciales de BD almacenadas en AWS SSM Parameter Store
- **Logs**: CloudWatch Logs (`/ecs/user-service`)

## Endpoints

| Método | Ruta | Descripción |
|--------|------|-------------|
| POST | `/api/users/register` | Registrar nuevo usuario |
| GET | `/api/users` | Obtener todos los usuarios |
| GET | `/api/users/:id` | Obtener usuario por ID |
| GET | `/api/users/username/:username` | Obtener usuario por username |
| DELETE | `/api/users/:id` | Eliminar usuario |

## Pipeline CI/CD

Cada commit a `main` dispara automáticamente el workflow `.github/workflows/deploy.yml`:
1. **Build**: construye la imagen Docker
2. **Push**: sube la imagen a Amazon ECR
3. **Deploy**: fuerza un nuevo despliegue en ECS

## Variables de entorno

| Variable | Descripción | Fuente |
|----------|-------------|--------|
| `DB_HOST` | Host de RDS MySQL | SSM Parameter Store |
| `DB_PASSWORD` | Contraseña de la BD | SSM Parameter Store (SecureString) |
| `DB_PORT` | Puerto MySQL (3306) | Task Definition |
| `DB_USER` | Usuario MySQL (admin) | Task Definition |
| `DB_NAME` | Nombre de la BD (users_db) | Task Definition |
| `PORT` | Puerto del servidor (8081) | Task Definition |

## Autoscaling

Configurado con Target Tracking al 50% de CPU:
- Mínimo: 1 task
- Máximo: 3 tasks

## Cómo ejecutar localmente

```bash
cp .env.example .env
# Editar .env con credenciales locales
npm install
npm start
```

## Estructura del proyecto
