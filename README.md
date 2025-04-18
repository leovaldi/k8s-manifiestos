# Despliegue local de sitio web estático con Minikube y Kubernetes

Este repositorio contiene los manifiestos necesarios para desplegar localmente un sitio web estático utilizando Minikube, montado desde un volumen persistente.

## 🌐 Requisitos previos

Antes de comenzar, asegúrate de tener instalados los siguientes programas y herramientas:

- **Minikube**: Para crear un clúster de Kubernetes localmente.
- **Docker**: Necesario para ejecutar los contenedores de Kubernetes.
- **Kubectl**: Herramienta de línea de comandos para interactuar con Kubernetes.
- **Git**: Para clonar los repositorios.

**Versiones recomendadas**:
- Minikube: 1.24.x o superior.
- Docker: 20.x o superior.
- Kubectl: 1.21.x o superior.
  
Si tienes alguna versión diferente, podrían ocurrir errores durante el proceso. Si ves que algo no funciona correctamente, verifica las versiones.

**Importante**:
Si estás utilizando Minikube con el driver de Docker (--driver=docker), asegúrate de que Docker esté en ejecución en tu sistema. Minikube necesita Docker activo para crear los contenedores de Kubernetes. Si Docker no está corriendo, Minikube no podrá iniciar el clúster.

## 🚀 Pasos para ejecutar el entorno

### 1. Clonar los repositorios

Primero, clona los dos repositorios necesarios para este despliegue. Asegúrate de que ambos repositorios estén dentro de un directorio común:

```bash
git clone https://github.com/Leovaldi/k8s-manifiestos.git
git clone https://github.com/Leovaldi/static-website.git
```

### 2. Estructura de carpetas

Tu estructura de carpetas debe verse así, dentro de un directorio común:

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

### 3. Iniciar Minikube con volumen montado

Para iniciar Minikube y montar el volumen que contiene el sitio web estático, usa el siguiente comando. **Asegúrate de cambiar la ruta `D:\ruta\completa\a\static-website`** por la ubicación correcta de tu carpeta `static-website` en tu sistema.

#### En Windows:

```bash
minikube start --driver=docker --mount --mount-string="D:\ruta\completa\a\static-website:/mnt/web"
```

#### En Linux/macOS:

```bash
minikube start --driver=docker --mount --mount-string="/ruta/a/static-website:/mnt/web"
```

Este comando iniciará Minikube con el volumen montado correctamente.

### 4. Aplicar los manifiestos

Ahora, aplica los manifiestos de Kubernetes para crear los recursos necesarios (volúmenes, servicios, despliegues, etc.):

```bash
cd k8s-manifiestos

kubectl apply -f pv/
kubectl apply -f pvc/
kubectl apply -f deployments/
kubectl apply -f services/
```

### 5. Verificar estado del pod

Verifica que los pods estén corriendo correctamente:

```bash
kubectl get pods
```

Si el pod está en estado **Running**, continúa con el siguiente paso. Si no es así, revisa los logs con:

```bash
kubectl logs <nombre-del-pod>
```

Si necesitas acceder al contenedor para verificar los archivos, puedes usar:

```bash
kubectl exec -it <nombre-del-pod> -- /bin/bash
```

Dentro del contenedor, verifica que los archivos se hayan montado correctamente en `/usr/share/nginx/html`:

```bash
ls /usr/share/nginx/html
```

Deberías ver algo como:

```bash
index.html  java.js  static-website  style.css
```

### 6. Acceder al sitio

Para acceder al sitio web desde tu navegador, usa el siguiente comando:

```bash
minikube service static-site-service
```

Esto abrirá automáticamente tu navegador en la URL correcta. Si no se abre automáticamente, usa este comando para obtener la URL:

```bash
minikube service static-site-service --url
```

Visita la URL proporcionada y deberías ver el sitio web estático desplegado correctamente.

## ✅ Resultado esperado

Al acceder al servicio, podrás ver el contenido de tu sitio web personalizado, que debe mostrar el archivo `index.html` con sus respectivos estilos y scripts.

## 🧾 Licencia

Este repositorio puede ser usado con fines educativos y para prácticas en entornos locales.

--- 

📌 **Importante**: Este repositorio forma parte de un proyecto académico para la materia *Computación en la Nube*, cuyo entorno de ejecución completo está documentado en [este repositorio complementario](https://github.com/Leovaldi/static-website).

---
