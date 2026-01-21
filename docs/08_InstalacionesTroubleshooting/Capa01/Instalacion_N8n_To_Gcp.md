# 🚀 Instalación de n8n en Google Cloud Platform

## 📋 Tabla de Contenidos
- [Prerrequisitos](#prerrequisitos)
- [Preparación de la VM en GCP](#preparación-de-la-vm-en-gcp)
- [Instalación de Docker](#instalación-de-docker)
- [Instalación de n8n](#instalación-de-n8n)
- [Configuración de Firewall](#configuración-de-firewall)
- [Acceso y Configuración Inicial](#acceso-y-configuración-inicial)

---

## 🎯 Prerrequisitos

### Requisitos Técnicos
- Cuenta de Google Cloud Platform activa
- Conocimientos básicos de terminal Linux
- Navegador web moderno (Chrome, Firefox, Edge)

### Recursos Mínimos Recomendados
- CPU: 2 vCPUs
- RAM: 4 GB
- Disco: 20 GB
- SO: Debian 11 o Ubuntu 20.04/22.04

### Free Tier de Google Cloud
Esta guía usa la Free Tier de GCP. Las limitaciones son:

- **Compute Engine**:  
  - 1 VM **e2-micro no preemptible** por mes en las siguientes regiones de EE.UU.:  
    - Oregon: `us-west1`  
    - Iowa: `us-central1`  
    - South Carolina: `us-east1`  
  - 30 GB-mes de disco persistente estándar  
  - 1 GB de transferencia de datos saliente desde Norteamérica a cualquier región (excluyendo China y Australia) por mes  
- La cuota de e2-micro es **por tiempo**, no por instancia: cada mes, el uso de todas las instancias elegibles es gratuito hasta alcanzar el total de horas del mes.  
- **GPUs y TPUs no están incluidas** en la Free Tier. Se cobran siempre que las agregues a tus VM.

---

## 🖥️ Preparación de la VM en GCP

### 1. Crear una VM en Google Cloud
1. Ve a GCP Console → Compute Engine → VM instances  
2. Click en **CREATE INSTANCE**  
3. Configura la instancia:
   - **Name:** n8n (o el nombre que prefieras)  
   - **Region:** Elige la más cercana a tu ubicación  
   - **Machine type:** e2-medium (2 vCPU, 4 GB RAM)  
   - **Boot disk:** OS: Debian 11 o Ubuntu 22.04 LTS, Size: 20 GB  
   - **Firewall:** Marca "Allow HTTP traffic"  
4. Click en **CREATE**

### 2. Conectarse a la VM
```bash
# Desde GCP Console
SSH junto a tu instancia

# O usando gcloud desde tu terminal local
gcloud compute ssh n8n --zone=YOUR_ZONE
3. Actualizar el Sistema
sudo apt update
sudo apt upgrade -y
🐳 Instalación de Docker
1. Limpiar instalaciones previas (si existen)
sudo apt remove docker docker-engine docker.io containerd runc 2>/dev/null || true
sudo systemctl stop docker 2>/dev/null || true
sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd
2. Instalar Docker desde el repositorio oficial
sudo apt update
sudo apt install -y ca-certificates curl gnupg

sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
3. Configurar permisos de usuario
sudo usermod -aG docker $USER
newgrp docker
docker run hello-world
Salida esperada:

Hello from Docker!
This message shows that your installation appears to be working correctly.
4. Verificar instalación
docker --version
docker compose version
sudo systemctl status docker
📦 Instalación de n8n
1. Crear estructura de directorios
mkdir -p ~/n8n
cd ~/n8n
2. Crear archivo docker-compose.yml
nano docker-compose.yml
Pegar el contenido (reemplaza YOUR_VM_IP con la IP externa de tu VM):

version: "3.8"

services:
  n8n:
    image: n8nio/n8n:latest
    container_name: n8n
    ports:
      - "8080:5678"
    environment:
      - N8N_PORT=5678
      - N8N_PROTOCOL=http
      - N8N_SECURE_COOKIE=false
      - WEBHOOK_URL=http://YOUR_VM_IP:8080/
      - GENERIC_TIMEZONE=America/Argentina/Buenos_Aires
    volumes:
      - n8n_data:/home/node/.n8n
    restart: unless-stopped

volumes:
  n8n_data:
3. Obtener la IP externa de tu VM
curl -s http://checkip.amazonaws.com
4. Actualizar docker-compose.yml con la IP
nano docker-compose.yml
# Reemplazar YOUR_VM_IP con tu IP externa
5. Levantar n8n
docker compose up -d
6. Verificar que está corriendo
docker ps
docker logs -f n8n
Logs esperados:

Initializing n8n process
n8n ready on 0.0.0.0, port 5678
Migrations in progress, please do NOT stop the process.
...
Editor is now accessible via:
http://YOUR_IP:8080/
🔥 Configuración de Firewall
Método 1: Consola Web GCP
Ve a Firewall rules

Click CREATE FIREWALL RULE

Configura:

Name: allow-n8n-8080

Targets: All instances

Source IPv4 ranges: 0.0.0.0/0

Protocols and ports: tcp:8080

Click CREATE

Método 2: gcloud CLI
gcloud compute firewall-rules create allow-n8n-8080 \
  --allow tcp:8080 \
  --source-ranges 0.0.0.0/0 \
  --description "Allow n8n web interface on port 8080"
🌐 Acceso y Configuración Inicial
Acceder desde navegador (modo incógnito): http://YOUR_VM_IP:8080

Crear cuenta Owner (email, nombre, contraseña segura)

Completar onboarding wizard
