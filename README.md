# Despliegue local de sitio web estático con Minikube y Kubernetes

Este repositorio contiene los manifiestos necesarios para desplegar localmente un sitio web estático utilizando Minikube, montado desde un volumen persistente.

## 🌐 Requisitos previos

- Minikube
- Docker
- Kubectl
- Git

## 🚀 Pasos para ejecutar el entorno

### 1. Clonar los repositorios

El sitio web estático se encuentra en un repositorio separado y debe clonarse localmente:

```bash
git clone https://github.com/Leovaldi/k8s-manifiestos.git
git clone https://github.com/Leovaldi/static-website.git
```

## 📁 La estructura de carpetas debe verse así

```
(nombre de tu carpeta)/
├── k8s-manifiestos/
│   ├── deployments/
│   │   └── static-site-deployment.yaml
│   ├── services/
│   │   └── static-site-service.yaml
│   ├── pv/
│   │   └── static-site-pv.yaml
│   ├── pvc/
│   │   └── static-site-pvc.yaml
│   ├── ingress/
│   └── README.md  
├── static-website/
│   ├── index.html
│   ├── style.css
│   └── java.js
```

### 2. Iniciar Minikube con volumen montado

```bash
minikube start --driver=docker --mount --mount-string="D:\ruta\completa\a\static-website:/mnt/web"
```

### 3. Aplicar los manifiestos

```bash
cd k8s-manifiestos

kubectl apply -f pv/
kubectl apply -f pvc/
kubectl apply -f deployments/
kubectl apply -f services/
```

### 4. Verificar estado del pod

```bash
kubectl get pods
kubectl exec -it <nombre-del-pod> -- /bin/bash
ls /usr/share/nginx/html
```

---Nos debe figurar algo así---

```bash
index.html  java.js  static-website  style.css
```

### 5. Acceder al sitio

```bash
minikube service static-site-service
```

Esto abrirá tu navegador en la dirección local del servicio.

## ✅ Resultado esperado

Al acceder al servicio, se visualizará el contenido de tu sitio web personalizado.

## 🧾 Licencia

Este repositorio puede ser usado con fines educativos y para prácticas en entornos locales.

---

📌 **Importante**: Este repositorio forma parte de un proyecto académico para la materia *Computación en la Nube*, cuyo entorno de ejecución completo está documentado en [este repositorio complementario](https://github.com/Leovaldi/static-website).

```