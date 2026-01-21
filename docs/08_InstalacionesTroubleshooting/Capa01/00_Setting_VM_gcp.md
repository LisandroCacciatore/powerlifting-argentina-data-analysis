# 📋 Configuración de la VM en Google Cloud Platform

## 🖥️ Información Básica de la Instancia

| Parámetro | Valor |
|-----------|-------|
| **Nombre** | n8n |
| **Instance ID** | 7742195453437040845 |
| **Tipo** | Instance |
| **Estado** | Running |
| **Fecha de creación** | 20 de enero de 2026, 17:31:31 UTC-03:00 |
| **Zona** | us-central1-c |
| **Sistema operativo** | Debian 12 Bookworm |
| **Imagen de boot disk** | debian-12-bookworm-v20260114 |
| **Arquitectura** | X86_64 |
| **Tipo de licencia** | Free |
| **Protección contra eliminación** | Deshabilitada |

---

## 💻 Configuración de Hardware

### Máquina Virtual

| Componente | Especificación |
|------------|----------------|
| **Machine type** | e2-micro |
| **vCPUs** | 2 |
| **Memoria RAM** | 1 GB |
| **CPU Platform** | Intel Broadwell |
| **Arquitectura** | x86/64 |
| **GPUs** | Ninguna |
| **Display device** | Deshabilitado |

> **⚠️ Nota Importante**: La configuración actual tiene **solo 1 GB de RAM**, lo cual es el mínimo para n8n. Para uso productivo se recomienda al menos **4 GB de RAM** (machine type: e2-medium).

### Almacenamiento

| Parámetro | Valor |
|-----------|-------|
| **Nombre del disco** | n8n (Boot Disk) |
| **Tamaño** | 28 GB |
| **Tipo** | Standard persistent disk |
| **Modo** | Read/write |
| **Política de eliminación** | Delete disk (al eliminar instancia) |
| **Backup schedule** | Diario, entre 6:00 AM y 7:00 AM |
| **Data protection** | `default-schedule-1` |

---

## 🌐 Configuración de Red

### Interfaces de Red

| Parámetro | Valor |
|-----------|-------|
| **Network** | default |
| **Subnetwork** | default |
| **NIC type** | VIRTIO_NET |
| **IP interna primaria** | 10.128.0.2 |
| **IP stack type** | IPv4 |
| **IP externa** | 34.9.235.127 (Ephemeral) |
| **Network tier** | Premium |
| **IP forwarding** | Off |

> **📝 Nota**: La IP externa es **efímera**, cambiará si se detiene y reinicia la VM. Para producción se recomienda una IP estática reservada.

### Configuración de Firewall

#### Reglas Habilitadas

| Regla | Estado |
|-------|--------|
| **HTTP traffic** | ✅ Habilitado |
| **HTTPS traffic** | ✅ Habilitado |
| **Allow Load Balancer Health checks** | ✅ Habilitado |

#### Network Tags

```
- http-server
- https-server
- lb-health-check
```

#### Reglas de Firewall Personalizadas (Configuradas manualmente)

| Nombre | Protocolo | Puerto | Source Range | Descripción |
|--------|-----------|--------|--------------|-------------|
| `allow-n8n-8080` | TCP | 8080 | 0.0.0.0/0 | Allow n8n web interface |

---

## 🔐 Seguridad y Acceso

### Shielded VM

| Característica | Estado |
|----------------|--------|
| **Secure Boot** | ❌ Deshabilitado |
| **vTPM** | ✅ Habilitado |
| **Integrity Monitoring** | ✅ Habilitado |

### SSH Keys

**Usuario configurado**: `lisandrocacciatore`

**Claves SSH registradas**:
- ECDSA-SHA2-NISTP256 (asociada a lisandrocacciatore@gmail.com)
- RSA (asociada a lisandrocacciatore@gmail.com)

**Block project-wide SSH keys**: ❌ Deshabilitado

### Service Account

| Parámetro | Valor |
|-----------|-------|
| **Service account** | 454219534157-compute@developer.gserviceaccount.com |
| **Cloud API access scopes** | Allow default access |

#### Permisos de API Habilitados

| API/Servicio | Acceso |
|--------------|--------|
| Service Control | ✅ Enabled |
| Service Management | 📖 Read Only |
| Stackdriver Logging API | ✏️ Write Only |
| Stackdriver Monitoring API | ✏️ Write Only |
| Stackdriver Trace | ✏️ Write Only |
| Storage | 📖 Read Only |

#### Servicios Deshabilitados

```
- BigQuery
- Bigtable Admin
- Bigtable Data
- Cloud Datastore
- Cloud Debugger
- Cloud Platform
- Cloud Pub/Sub
- Cloud Source Repositories
- Cloud SQL
- Compute Engine
- Task queue
- User info
```

---

## ⚙️ Políticas de Disponibilidad

### VM Provisioning

| Parámetro | Configuración |
|-----------|---------------|
| **Provisioning model** | Standard |
| **Max duration** | None |
| **Preemptibility** | ❌ Off (Recomendado) |
| **On VM termination** | — |
| **Automatic restart** | ✅ On (Recomendado) |
| **On host maintenance** | Migrate VM instance (Recomendado) |
| **Host error timeout** | — |
| **CMEK revocation policy** | Do nothing |

### Metadata Personalizada

| Key | Value |
|-----|-------|
| `enable-osconfig` | TRUE |

---

## 📊 Resumen de Costos Estimados

### Costos Mensuales Aproximados (us-central1)

| Recurso | Especificación | Costo Mensual (USD)* |
|---------|----------------|----------------------|
| **Compute** | e2-micro (2 vCPU, 1GB RAM) | ~$7.11 |
| **Storage** | 28 GB Standard persistent disk | ~$1.12 |
| **IP Externa** | Ephemeral IP | $0 (mientras esté en uso) |
| **Tráfico** | Egress internet (variable) | Variable según uso |
| **TOTAL ESTIMADO** | | **~$8.23/mes** |

> *Precios aproximados de GCP para enero 2026. Los costos reales pueden variar según uso, región y promociones disponibles.

---

## 🔧 Comandos Útiles para Gestión

### Conexión SSH

```bash
# Desde GCP Console (recomendado)
# Click en "SSH" junto a la instancia en Compute Engine → VM instances

# Desde terminal local con gcloud CLI
gcloud compute ssh n8n --zone=us-central1-c

# SSH directo (requiere configuración de claves)
ssh lisandrocacciatore@34.9.235.127
```

### Gestión de la VM desde gcloud CLI

```bash
# Ver información de la instancia
gcloud compute instances describe n8n --zone=us-central1-c

# Detener la instancia
gcloud compute instances stop n8n --zone=us-central1-c

# Iniciar la instancia
gcloud compute instances start n8n --zone=us-central1-c

# Reiniciar la instancia
gcloud compute instances reset n8n --zone=us-central1-c

# Ver logs de la instancia
gcloud compute instances get-serial-port-output n8n --zone=us-central1-c
```

### Gestión de Firewall

```bash
# Listar reglas de firewall
gcloud compute firewall-rules list

# Ver detalles de una regla específica
gcloud compute firewall-rules describe allow-n8n-8080

# Crear nueva regla (ejemplo para puerto 443)
gcloud compute firewall-rules create allow-https-443 \
  --allow tcp:443 \
  --source-ranges 0.0.0.0/0 \
  --description "Allow HTTPS traffic"

# Eliminar regla
gcloud compute firewall-rules delete RULE_NAME
```

---

## 🚀 Recomendaciones para Producción

### 1. Actualizar Machine Type

```bash
# Detener la VM
gcloud compute instances stop n8n --zone=us-central1-c

# Cambiar a e2-medium (2 vCPU, 4GB RAM)
gcloud compute instances set-machine-type n8n \
  --zone=us-central1-c \
  --machine-type=e2-medium

# Iniciar la VM
gcloud compute instances start n8n --zone=us-central1-c
```

**Costo adicional**: ~$17/mes (total ~$25/mes)

### 2. Reservar IP Estática

```bash
# Crear IP estática
gcloud compute addresses create n8n-static-ip \
  --region=us-central1

# Obtener la IP reservada
gcloud compute addresses describe n8n-static-ip \
  --region=us-central1

# Asignar IP estática a la VM
gcloud compute instances delete-access-config n8n \
  --zone=us-central1-c \
  --access-config-name="External NAT"

gcloud compute instances add-access-config n8n \
  --zone=us-central1-c \
  --access-config-name="External NAT" \
  --address=IP_ESTATICA_RESERVADA
```

**Costo adicional**: ~$3/mes si la VM está corriendo (gratis mientras esté en uso)

### 3. Habilitar Secure Boot

```bash
# Detener la VM
gcloud compute instances stop n8n --zone=us-central1-c

# Habilitar Secure Boot
gcloud compute instances update n8n \
  --zone=us-central1-c \
  --shielded-secure-boot

# Iniciar la VM
gcloud compute instances start n8n --zone=us-central1-c
```

### 4. Configurar Backups Automáticos

Los backups ya están configurados con el schedule `default-schedule-1`:
- Frecuencia: Diaria
- Hora: Entre 6:00 AM y 7:00 AM

Para personalizar:

```bash
# Crear snapshot manual
gcloud compute disks snapshot n8n \
  --zone=us-central1-c \
  --snapshot-names=n8n-backup-$(date +%Y%m%d)

# Listar snapshots
gcloud compute snapshots list
```

### 5. Monitoreo con Cloud Monitoring

```bash
# Instalar agente de monitoreo (dentro de la VM)
curl -sSO https://dl.google.com/cloudagents/add-google-cloud-ops-agent-repo.sh
sudo bash add-google-cloud-ops-agent-repo.sh --also-install
```

---

## 📈 Monitoreo de Recursos

### Desde GCP Console

1. Ve a **Compute Engine → VM instances**
2. Click en el nombre de la instancia **n8n**
3. Ve a la pestaña **Monitoring**

Métricas disponibles:
- CPU utilization
- Memory utilization
- Disk read/write
- Network sent/received

### Desde la VM (SSH)

```bash
# Uso de CPU y memoria
top

# Uso de disco
df -h

# Uso de memoria
free -h

# Procesos de Docker
docker stats
```

---

## 🔄 Mantenimiento y Actualizaciones

### Actualizaciones del Sistema

```bash
# Actualizar paquetes
sudo apt update
sudo apt upgrade -y

# Actualizar Debian (major version)
sudo apt full-upgrade -y
```

### Actualizaciones de n8n

```bash
cd ~/n8n
docker compose pull
docker compose up -d
```

### Limpieza de Recursos

```bash
# Limpiar imágenes Docker no utilizadas
docker image prune -a

# Limpiar volúmenes no utilizados
docker volume prune

# Limpiar todo (cuidado)
docker system prune -a --volumes
```

---

## 📞 Contacto y Soporte

### GCP Support
- **Console**: https://console.cloud.google.com/support
- **Documentación**: https://cloud.google.com/docs

### n8n Community
- **Forum**: https://community.n8n.io/
- **GitHub**: https://github.com/n8n-io/n8n

---

## 📝 Notas Adicionales

1. **IP Ephemeral**: La IP externa actual (34.9.235.127) puede cambiar si se detiene la VM. Para producción, considerar reservar una IP estática.

2. **RAM Limitada**: Con 1 GB de RAM, n8n puede experimentar lentitud con workflows complejos. Monitorear el uso y escalar si es necesario.

3. **Backups**: Los backups automáticos están habilitados, pero considerar también backups manuales del volumen Docker de n8n.

4. **Seguridad**: Para producción, habilitar Secure Boot y configurar reglas de firewall más restrictivas.

5. **Costos**: Monitorear el uso y costos en GCP Console → Billing para evitar sorpresas.

---

**Documento generado**: 21 de enero de 2026  
**Autor**: Lisandro Cacciatore  
**Última actualización**: 21 de enero de 2026
