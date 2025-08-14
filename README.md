APLICACIONES DISTRIBUIDAS: NRC 23316

# 🧩 Kubernetes Publicaciones

Este proyecto implementa una arquitectura de microservicios utilizando **Spring Boot**, **Docker**, **Kubernetes (Minikube)** y **CockroachDB**, orquestados mediante servicios desplegados en un clúster local de Minikube. También se usa **RabbitMQ** como broker de mensajería y un **API Gateway** para enrutar las peticiones.

---

## 🚀 Tecnologías Utilizadas

- Java + Spring Boot
- Docker
- Kubernetes (Minikube)
- CockroachDB
- RabbitMQ
- Eureka Server (Service Discovery)
- Swagger UI (Documentación de API)

---

## ⚙️ Prerrequisitos

- Docker
- Minikube
- kubectl
- Java 17+
- DockerHub (para subir imágenes)
- CockroachDB CLI (dentro del contenedor)

---

## 🐳 Construcción y Push de Imágenes Docker

```bash
minikube start --driver=docker


# AuthService

```bash
cd authservice
docker build -t mil0st4r/authservice:latest .
docker push mil0st4r/authservice:latest
cd ..
'''bash

### ☸️ Despliegue en Kubernetes

A continuación, se detallan los pasos para desplegar todos los componentes en tu clúster de Minikube usando `kubectl`:

### 🐘 Base de Datos - CockroachDB

kubectl apply -f cockroachdb-deployment.yaml
kubectl apply -f cockroachdb-service.yaml

### Inicialización del cluster
kubectl exec -it <nombre-del-pod> -- cockroach init --insecure --host=cockroachdb-svc

cockroach init --insecure --host=cockroachdb-svc

exit

### Acceso a CockroachDB desde Shell
kubectl exec -it <nombre-del-pod> -- ./cockroach sql --insecure

### Creación de las bases de datos necesarias
CREATE DATABASE publicaciones_db;
CREATE DATABASE authdb;
CREATE DATABASE catalogo_db;
CREATE DATABASE notificaciones_db;

q\

### Instanciando el broker de mensajería
kubectl apply -f rabbitmq-deployment.yml
kubectl apply -f rabbitmq-service.yml

### Instanciando el servicio de descubrimiento
Servicio de Descubrimiento - Eureka Server

### Microservicios
# AuthService
kubectl apply -f authservice-deployment.yml
kubectl apply -f authservice-service.yml

# Catálogo
kubectl apply -f ms-catalogo-deployment.yml
kubectl apply -f ms-catalogo-service.yml

# Notificaciones
kubectl apply -f ms-notificaciones-deployment.yml
kubectl apply -f ms-notificaciones-service.yml

# Publicaciones
kubectl apply -f ms-publicaciones-deployment.yml
kubectl apply -f ms-publicaciones-service.yml

# Sincronización
kubectl apply -f ms-sincronizacion-deployment.yml
kubectl apply -f ms-sincronizacion-service.yml

### API Gateway
kubectl apply -f api-gateway-config.yaml
kubectl apply -f ms-api-gateway-deployment.yml
kubectl apply -f ms-api-gateway-service.yml

### Verificación del estado de los pods
kubectl get pods

### Comprobación de rutas con Swagger
kubectl port-forward svc/ms-api-gateway-svc 8000:8000

http://localhost:8000/swagger-ui/index.html

