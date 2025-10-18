# HU-01: Recepción de Solicitud de Convalidación

## 📋 Descripción
Sistema automatizado para recibir, validar y procesar solicitudes de convalidación de asignaturas de estudiantes universitarios.

## 🎯 Historia de Usuario
**Como** estudiante universitario,    
**Quiero** un formulario web para ingresar mis datos y adjuntar documentos de respaldo,    
**Para** iniciar el proceso de convalidación de forma digital y centralizada.

## ✅ Criterios de Aceptación

### Escenario 1 – Envío exitoso con todos los datos  
✅ **IMPLEMENTADO**
- Todos los campos completados correctamente
- Documento PDF adjunto
- Sistema registra en Google Sheets
- Envía email de confirmación

### Escenario 2 – Intento de envío sin adjuntar documento  
✅ **IMPLEMENTADO**
- Formulario muestra mensaje de error
- No se procesa la solicitud

### Escenario 3 – Intento de envío con formato incorrecto  
✅ **IMPLEMENTADO**
- Sistema rechaza archivos no-PDF
- Notifica que solo se aceptan PDFs

## 🏗️ Arquitectura del Sistema

### Flujo Completo:
1. **Formulario Web** → Estudiante ingresa datos
2. **Validación Cliente** → JavaScript valida PDF y campos
3. **Webhook n8n** → Recibe datos vía POST
4. **Validación Servidor** → n8n valida campos obligatorios
5. **Google Sheets** → Guarda en hoja "Solicitudes"
6. **Google Sheets** → Registra log en hoja "Logs"
7. **Email SMTP** → Envía confirmación al estudiante
8. **Respuesta JSON** → Retorna resultado al formulario

## 📁 Archivos del Proyecto

### `/developer/lucas/`
```
├── flow_HU01.json                      # Flujo completo de n8n
├── formulario-convalidacion-unab.html  # Formulario web con colores UNAB
├── test_hu01.sh                        # Script de pruebas automatizadas
├── sprint1.txt                         # Documentación Sprint 1
├── Proyecto-Gestor-Convalidaciones-Academicas.txt  # Especificaciones
└── README.md                           # Este archivo
```

## 🚀 Cómo Usar

### 1. Importar el Flujo en n8n
```bash
# En n8n, ir a: Import from File
# Seleccionar: flow_HU01.json
```

### 2. Configurar Credenciales

#### Google Sheets OAuth2:
1. Settings > Credentials > Google Sheets OAuth2 API
2. Configurar Client ID y Client Secret
3. Autorizar acceso

#### SMTP (Ethereal Email para pruebas):
1. Ir a: https://ethereal.email/create
2. Copiar credenciales generadas
3. Configurar en n8n

### 3. Levantar Formulario Web
```bash
cd /workspaces/Proyecto-n8n/developer/lucas
python3 -m http.server 8080
# Acceder a: http://localhost:8080/formulario-convalidacion-unab.html
```

### 4. Ejecutar Pruebas
```bash
chmod +x test_hu01.sh
./test_hu01.sh
```

## 🧪 Casos de Prueba

### TC1.1 - Caso Válido Completo
**Input:**
```json
{
  "nombre": "María Elena Rodríguez",
  "rut": "20.111.222-3",
  "carrera": "Psicología",
  "asignatura": "Estadística Aplicada",
  "institucionOrigen": "Universidad de Santiago",
  "documentos": "certificado_maria.pdf",
  "email": "maria.elena@test.com"
}
```
**Output:** HTTP 200 - Solicitud registrada

### TC1.2 - Campo Obligatorio Faltante
**Input:** Sin campo `rut`
**Output:** HTTP 200 - Error de validación

### TC1.3 - Múltiples Campos Faltantes
**Input:** Solo nombre y carrera
**Output:** HTTP 200 - Lista de campos faltantes

### TC2.1 - Verificación de Email
**Input:** Solicitud válida con email
**Output:** Email de confirmación enviado

### TC5.1 - Caracteres Especiales
**Input:** Nombres con tildes, ñ, guiones
**Output:** HTTP 200 - Procesa correctamente

## 📊 Estructura Google Sheets

### Hoja "Solicitudes"
| ID | Fecha | Estudiante | RUT | Carrera | Asignatura | InstitucionOrigen | Estado |
|----|-------|------------|-----|---------|------------|-------------------|--------|

### Hoja "Logs"
| Timestamp | ID | Status | Details |
|-----------|----|---------| --------|

## 🎨 Colores UNAB
- Rojo Principal: `#E30613`
- Rojo Oscuro: `#8B0000`
- Gris Oscuro: `#333333`
- Gris Claro: `#666666`

## 📧 Configuración Email

### Ethereal Email (Pruebas)
- Host: smtp.ethereal.email
- Port: 587
- SSL/TLS: Desactivado
- Ver emails: https://ethereal.email/messages

### Gmail (Producción)
- Host: smtp.gmail.com
- Port: 587
- SSL/TLS: STARTTLS
- Requiere: App Password

## 🔧 Tecnologías Utilizadas
- **n8n**: Orquestación y automatización
- **Google Sheets API**: Almacenamiento de datos
- **SMTP/Ethereal**: Envío de emails
- **HTML/CSS/JavaScript**: Formulario web
- **Python HTTP Server**: Servidor local

## 📈 Métricas de Éxito
- ✅ 100% de solicitudes válidas procesadas
- ✅ Tiempo de respuesta < 8 segundos
- ✅ 0% pérdida de datos
- ✅ Email confirmación enviado en < 10 segundos
- ✅ Validación de PDF funcionando
- ✅ Detección de campos faltantes

## 🐛 Solución de Problemas

### Error: "Unable to sign without access token"
**Solución:** Reconectar credencial OAuth2 de Google Sheets

### Error: "Sheet with ID not found"
**Solución:** Usar Sheet ID numérico (0 para primera hoja)

### Email no se ve bien
**Solución:** Activar modo "Expression" en campo Text del nodo Email

## 👨‍💻 Desarrollador
- **Nombre:** Lucas Maulen
- **Email:** lucasmaulenr@gmail.com
- **Proyecto:** Sistema de Convalidaciones Académicas UNAB
- **Sprint:** 1
- **Fecha:** Octubre 2025

## 📝 Notas Adicionales
- El formulario valida PDF del lado del cliente y del servidor
- Los datos se guardan en Google Sheets en tiempo real
- El sistema envía confirmación por email automáticamente
- Todos los criterios de aceptación están cumplidos
- El flujo está completamente funcional y probado

---
**Versión:** 1.0  
**Última actualización:** 18 de octubre de 2025
