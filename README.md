# Práctica 01: Introducción a ArgoCD

Este proyecto documenta los primeros pasos para aprender y desplegar **ArgoCD** en un clúster de Kubernetes local (Docker Desktop/Minikube).

## 🧐 ¿Qué es ArgoCD?

**ArgoCD** es una herramienta de **Continuous Delivery (Entrega Continua)** declarativa para Kubernetes que sigue la metodología **GitOps**.

### ¿Para qué sirve?

Sirve para automatizar el despliegue de aplicaciones en Kubernetes. En lugar de ejecutar comandos manuales (`kubectl apply`) o scripts complejos, ArgoCD sincroniza automáticamente el estado de tu clúster con lo que tienes definido en un repositorio de Git.

### ¿Cómo funciona?

1.  **Git como Fuente de Verdad:** Toda la configuración de tu infraestructura y aplicaciones (archivos YAML) vive en Git.
2.  **Monitoreo:** ArgoCD observa tu repositorio Git y lo compara constantemente con lo que está corriendo en tu clúster.
3.  **Sincronización:** Si detecta diferencias (por ejemplo, cambiaste una imagen Docker en Git), ArgoCD aplica esos cambios en el clúster para que ambos estados coincidan.

---

## 🚀 Guía de Instalación y Configuración

A continuación, se detallan los pasos que realizamos para levantar ArgoCD desde cero.

### 1. Prerrequisitos

- Tener un clúster de Kubernetes corriendo (Docker Desktop, Minikube, etc.).
- Tener `kubectl` instalado y configurado.

### 2. Instalación

Creamos el namespace y aplicamos el manifiesto oficial de instalación:

```bash
# Crear el namespace para ArgoCD
kubectl create namespace argocd

# Aplicar el manifiesto de instalación oficial
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

### 3. Verificación

Esperamos a que el servidor de ArgoCD estuviera listo:

```bash
kubectl wait --for=condition=available --timeout=300s deployment/argocd-server -n argocd
```

### 4. Acceso a la Interfaz (UI)

#### Obtener la contraseña inicial

La contraseña se genera automáticamente en un Secret. Usamos este comando para obtenerla y decodificarla (en PowerShell):

```powershell
# Obtener el secret codificado en base64
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}"

# Decodificar el resultado (ejemplo manual)
[System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String("YTlXNmZaRUdieDFscEYtLQ=="))
```

#### Exponer el servicio (Port Forward)

Para acceder desde nuestro navegador local (`localhost`), hicimos un port-forward:

```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

### 5. Ingreso

- **URL:** [https://localhost:8080](https://localhost:8080)
- **Usuario:** `admin`
- **Contraseña:** (La obtenida en el paso 4)

---
