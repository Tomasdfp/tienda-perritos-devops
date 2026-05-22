# 🐶 Tienda de Perritos - DevOps Project

**Autores:** Tomás Del Fierro & Karen Fuentealba  
**Asignatura:** Introducción a Herramientas DevOps (EPISY1101)  
**Evaluación:** Parcial N°2  
**Año:** 2025  

---

## 📋 Descripción

Aplicación web para gestión de productos de una tienda de alimentos para perritos.  
Incluye CRUD completo (Crear, Leer, Actualizar, Eliminar) con contenedores Docker desplegados en AWS.

---

## 🛠 Tecnologías utilizadas

| Componente | Tecnología |
|------------|------------|
| Frontend | Nginx + HTML/CSS/JS |
| Backend | Node.js + Express |
| Base de Datos | MySQL 8.0 |
| Contenedores | Docker |
| Orquestación | Docker Compose |
| Registro de imágenes | Amazon ECR |
| CI/CD | GitHub Actions |
| Infraestructura | AWS EC2 |

---

## 📁 Estructura del proyecto
tienda-perritos-devops/
├── .github/workflows/
│ ├── deploy-frontend.yml
│ ├── deploy-backend.yml
│ └── deploy-db.yml
├── frontend/
│ ├── Dockerfile
│ ├── index.html
│ ├── app.js
│ └── default.conf
├── backend/
│ ├── Dockerfile
│ ├── server.js
│ └── package.json
├── db/
│ ├── Dockerfile
│ └── init.sql
├── docker-compose.yml
└── README.md


## 🚀 Ejecución local

### Requisitos previos
- Docker Desktop instalado

### Pasos

```bash
# Clonar repositorio
git clone https://github.com/tomasdfp/tienda-perritos-devops.git
cd tienda-perritos-devops

# Levantar todos los servicios
docker-compose up -d --build

# Verificar estado
docker-compose ps

☁️ Despliegue en AWS
Infraestructura
Instancia	Función	IP Pública	IP Privada
Frontend	Web server	100.50.181.145	10.0.5.80
Backend	API REST	18.233.1.142	10.0.132.203
DB	MySQL	44.196.53.154	10.0.159.117
Acceso a la aplicación

----

http://100.50.181.145
🔄 Pipeline CI/CD
Disparador
Push a la rama deploy

----

Flujo
Construcción de imagen Docker

Publicación en Amazon ECR

Despliegue automático en EC2 vía AWS Systems Manager

Workflows
deploy-frontend.yml - Frontend Nginx

deploy-backend.yml - Backend Node.js

deploy-db.yml - Base de datos MySQL

🔐 Secrets de GitHub configurados
Secret	Descripción
AWS_ACCESS_KEY_ID	Credencial AWS
AWS_SECRET_ACCESS_KEY	Credencial AWS
AWS_SESSION_TOKEN	Token de sesión AWS
AWS_REGION	us-east-1
ECR_REGISTRY	URI del registro ECR
ECR_REPO_URL_FRONTEND	URL del repo frontend
ECR_REPO_URL_BACKEND	URL del repo backend
ECR_REPO_URL_DB	URL del repo db
EC2_FRONTEND_ID	ID instancia frontend
EC2_BACKEND_ID	ID instancia backend
EC2_DB_ID	ID instancia db
DB_HOST	IP privada de la DB

🐳 Dockerfiles
Frontend (Nginx - usuario no root)
dockerfile
FROM nginx:alpine
RUN chown -R nginx:nginx /usr/share/nginx/html && \
    chown -R nginx:nginx /var/cache/nginx && \
    chown -R nginx:nginx /var/log/nginx && \
    chown -R nginx:nginx /etc/nginx/conf.d
RUN rm -rf /usr/share/nginx/html/*
COPY index.html app.js /usr/share/nginx/html/
COPY default.conf /etc/nginx/conf.d/default.conf
USER nginx
EXPOSE 80
Backend (Node.js - multi-stage)
dockerfile
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm install --only=production

FROM node:18-alpine
RUN addgroup -g 1001 -S nodejs && adduser -S nodejs -u 1001
WORKDIR /app
COPY --from=builder --chown=nodejs:nodejs /app/node_modules ./node_modules
COPY --chown=nodejs:nodejs server.js ./
USER nodejs
EXPOSE 3001
CMD ["node", "server.js"]
Base de Datos (MySQL)
dockerfile
FROM mysql:8.0
COPY init.sql /docker-entrypoint-initdb.d/
EXPOSE 3306

# Acceder en navegador
http://localhost
