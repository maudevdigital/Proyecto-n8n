# Configuración Local - HU-001

## Variables de Entorno

```bash
# n8n
N8N_HOST=localhost
N8N_PORT=5678
N8N_PROTOCOL=http
WEBHOOK_URL=http://localhost:5678/

# Google Sheets
GOOGLE_SHEET_ID=<TU_SPREADSHEET_ID>
SHEET_SOLICITUDES_ID=0
SHEET_LOGS_ID=1

# SMTP (Ethereal - Testing)
SMTP_HOST=smtp.ethereal.email
SMTP_PORT=587
SMTP_USER=<usuario_ethereal>
SMTP_PASS=<password_ethereal>
SMTP_FROM=noreply@convalidaciones.unab.cl

# Servidor Formulario
FORM_PORT=8080
FORM_URL=http://localhost:8080/formulario-convalidacion-unab.html
```

## Credenciales Google Cloud

```
Project ID: proyecto-convalidaciones-unab
OAuth2 Client ID: <tu_client_id>.apps.googleusercontent.com
OAuth2 Client Secret: <tu_client_secret>
Redirect URI: http://localhost:5678/rest/oauth2-credential/callback
```

## Endpoints

- **n8n UI:** http://localhost:5678
- **Webhook:** http://localhost:5678/webhook/solicitud-convalidacion
- **Formulario:** http://localhost:8080/formulario-convalidacion-unab.html
- **Ethereal Inbox:** https://ethereal.email/messages

## Notas

- No versionar este archivo con datos reales
- Usar `.env` para producción
- Rotar credenciales periódicamente
