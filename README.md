# Estadísticas Service

Microservicio de Estadísticas (FastAPI) de la plataforma VidalCasino.

## Arquitectura y Despliegue en Amazon EKS (EP3)

Este servicio ha sido migrado a una arquitectura nativa de la nube en Kubernetes.

### Workflow de CI/CD Paso a Paso
El despliegue de esta aplicación está automatizado mediante GitHub Actions (`.github/workflows/deploy.yaml`).

1. **Commit y Push**: Al realizar un push a la rama `deploy`, se activa el pipeline.
2. **Build Docker**: GitHub Actions lee el `Dockerfile` optimizado (multi-stage, con ejecución de usuario no root y uvicorn) y construye la imagen.
3. **Push a Amazon ECR**: La imagen se publica de forma privada en ECR etiquetada con el SHA del commit para un versionado preciso.
4. **Deploy a Amazon EKS**: El pipeline se conecta de forma segura al clúster y actualiza el Deployment en Kubernetes usando la nueva imagen garantizando Zero Downtime.

### Componentes de Kubernetes (Carpeta `k8s/`)
- **Deployment**: Configura el contenedor, define las inyecciones de los secretos de base de datos (`casino-secrets`) y establece sondas de salud (`livenessProbe` y `readinessProbe`).
- **Service**: Permite el descubrimiento interno entre microservicios (`ClusterIP`).

### Secretos de GitHub Requeridos
`AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `AWS_SESSION_TOKEN`, `AWS_REGION`, `EKS_CLUSTER`, `ECR_REPOSITORY`, `DEPLOYMENT_NAME`, `CONTAINER_NAME`.
