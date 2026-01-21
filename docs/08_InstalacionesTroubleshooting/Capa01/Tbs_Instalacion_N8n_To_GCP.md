🚨 Troubleshooting
Problema 1: Instance owner already setup

cd ~/n8n
docker compose down -v
docker volume rm n8n_n8n_data
docker compose up -d
Problema 2: 401 Unauthorized → limpiar cache o usar incógnito

Problema 3: Secure cookie warning → verificar N8N_SECURE_COOKIE=false

Problema 4: Cannot GET / → revisar logs, reiniciar contenedor

Problema 5: Contenedor name already in use → eliminar contenedor zombie

Problema 6: No puedo acceder desde navegador → revisar contenedor, logs, firewall, IP externa

📊 Comandos de verificación útiles
docker ps
docker ps -a
docker logs -f n8n
docker logs n8n --tail 50
docker exec n8n env | grep N8N
docker info
docker stats n8n
docker inspect n8n
docker volume ls
docker volume inspect n8n_n8n_data
🔄 Comandos de mantenimiento
docker compose restart
docker compose stop
docker compose start
docker compose pull
docker compose up -d
# Backup
docker run --rm -v n8n_n8n_data:/data -v $(pwd):/backup alpine tar czf /backup/n8n-backup-$(date +%Y%m%d).tar.gz -C /data .
📝 Próximos Pasos Recomendados
Configurar HTTPS con Caddy y dominio propio

Backups automáticos diarios

Monitoreo de logs y recursos con Docker Stats / UptimeRobot

📚 Integraciones Comunes
Google Sheets
Credentials → Add Credential → Google Sheets API → OAuth2 wizard

Webhooks
https://your-domain.com/webhook/your-webhook-id
🔐 Notas de Seguridad
Desarrollo: HTTP sin TLS, puerto expuesto, solo login n8n

Producción: HTTPS, firewall restrictivo, 2FA, backups diarios, monitoreo, contraseñas fuertes

🎯 Checklist de instalación completa
VM creada en GCP

Docker instalado y funcionando

Usuario agregado al grupo docker

Directorio ~/n8n creado

docker-compose.yml configurado con IP correcta

n8n levantado y corriendo

Firewall configurado (8080)

Acceso web funcionando

Cuenta owner creada

Onboarding completado

Opcional: HTTPS configurado

Opcional: Backups configurados

Opcional: Monitoreo configurado
