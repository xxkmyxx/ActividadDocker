# Tienda de Alimentos para Perritos - DevOps

Aplicación web de gestión de productos para una tienda de alimentos para mascotas, desplegada en AWS con contenedores Docker y pipeline CI/CD automatizado con GitHub Actions.
```
## Estructura del proyecto
ActividadDocker/
├── frontend/          # Interfaz web (Nginx)
│   ├── Dockerfile     # Multi-stage build
│   ├── index.html
│   ├── app.js
│   └── default.conf
├── backend/           # API REST (Node.js)
│   ├── Dockerfile     # Multi-stage build
│   ├── server.js
│   └── package.json
├── db/                # Base de datos (MySQL 8)
│   ├── Dockerfile     # Multi-stage build
│   └── init.sql
├── .github/workflows/ # Pipelines CI/CD
│   ├── cicd-tienda-frontend.yml
│   ├── cicd-tienda-backend.yml
│   └── cicd-tienda-db.yml
└── docker-compose.yml # Stack completo
```
## Contenedorización

Cada servicio tiene su propio Dockerfile con **multi-stage build**, usuario no root y buenas prácticas de seguridad.

### Levantar el stack completo localmente

```bash
docker-compose up --build
```

### Levantar un servicio individual

```bash
docker-compose up --build frontend
docker-compose up --build backend
docker-compose up --build db
```

## Persistencia de datos

Se utiliza un **named volume** (`dbdata`) para la base de datos MySQL, lo que garantiza que los datos no se pierdan al reiniciar los contenedores.

Se eligió **named volume** por sobre bind mount porque es más portable, no depende de la estructura de directorios del host y es la práctica recomendada para bases de datos en producción.

## Pipeline CI/CD

Cada servicio tiene su propio workflow en GitHub Actions que se activa con un **push a la rama `deploy`** en su carpeta correspondiente.

### Flujo del pipeline
Push a rama deploy
↓
Checkout del código
↓
Configurar credenciales AWS
↓
Login en Amazon ECR
↓
Build imagen Docker
↓
Push imagen a ECR
↓
Deploy en EC2 vía SSM

### Secrets requeridos en GitHub

| Secret | Descripción |
|--------|-------------|
| `AWS_ACCESS_KEY_ID` | Credencial AWS |
| `AWS_SECRET_ACCESS_KEY` | Credencial AWS |
| `AWS_SESSION_TOKEN` | Token de sesión AWS |
| `AWS_REGION` | Región AWS (us-east-1) |
| `ECR_REGISTRY` | URL base del registro ECR |
| `ECR_REPO_URL_FRONTEND` | URL repo ECR frontend |
| `ECR_REPO_URL_BACKEND` | URL repo ECR backend |
| `ECR_REPO_URL_DB` | URL repo ECR db |
| `EC2_FRONTEND_INSTANCE_ID` | ID instancia EC2 frontend |
| `EC2_BACKEND_INSTANCE_ID` | ID instancia EC2 backend |
| `EC2_DB_INSTANCE_ID` | ID instancia EC2 db |

## Infraestructura AWS

- **3 instancias EC2** (frontend, backend, db)
- **Amazon ECR** como registro de imágenes Docker
- **AWS Systems Manager (SSM)** para el despliegue automático

## Cómo desplegar

1. Configura los secrets en GitHub
2. Haz un cambio en el código
3. Haz push a la rama `deploy`
4. El pipeline se activa automáticamente
