# Innovatech Chile - Frontend (Evaluación Parcial N°3 DevOps)

Este repositorio contiene el código fuente y la configuración de despliegue continuo (CI/CD) para el Frontend de la aplicación "Tienda de Alimentos para Perritos", desarrollada para el caso Innovatech Chile.

## 📁 Estructura del Proyecto

El repositorio está organizado de la siguiente manera:

*   **`frontend/`**: Contiene el código fuente de la interfaz de usuario.
    *   `index.html` / `app.js`: Lógica y estructura de la aplicación web que consume la API REST del backend.
    *   `default.conf`: Configuración del servidor web (Nginx) para enrutar el tráfico correctamente.
    *   `Dockerfile`: Instrucciones para la contenedorización de la aplicación frontend.
*   **`.github/workflows/`**: Contiene el archivo `cicd-tienda-frontend.yml` responsable de la automatización del despliegue.

## 🚀 Arquitectura y Despliegue

La aplicación está diseñada para ejecutarse en una arquitectura Cloud-Native utilizando contenedores.

1.  **Contenedorización**: El Frontend se empaqueta en una imagen Docker nativa.
2.  **Orquestación**: La imagen se ejecuta en un clúster de **AWS ECS utilizando AWS Fargate**, eliminando la necesidad de administrar servidores subyacentes.
3.  **Balanceo de Carga**: El tráfico de los usuarios ingresa a través de un **Application Load Balancer (ALB)** en el puerto 80, el cual redirige las peticiones a las tareas activas de ECS.
4.  **Comunicación con Backend**: El Frontend se comunica con el Backend a través de la variable de entorno `VITE_API_URL`, la cual apunta al DNS del ALB en el puerto 3000 (`/api`).

## ⚙️ Integración y Despliegue Continuo (CI/CD)

El ciclo de vida del software está completamente automatizado a través de **GitHub Actions**. Al realizar un `push` a la rama `main`, el workflow ejecuta:

1.  **Configuración de Credenciales**: Autenticación segura con AWS mediante GitHub Secrets (`AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, etc.).
2.  **Build & Push**: Construcción de la imagen Docker y subida al repositorio privado de **Amazon ECR**.
3.  **Deploy**: Renderización de la nueva imagen en la *Task Definition* actual y actualización sin tiempo de inactividad (Rolling Update) del servicio en AWS ECS.
