# HU-001: Recepción de Solicitud de Convalidación

**Desarrollador:** Lucas Maulén Riquelme  
**Estado:** ✅ Completado  
**Sprint:** 1  
**Fecha:** Octubre 2025

## 📋 Descripción

Sistema automatizado para recibir, validar y procesar solicitudes de convalidación de asignaturas mediante formulario web, validación de campos, almacenamiento en Google Sheets y notificación por email.

## 🎯 Objetivos

- ✅ Crear formulario web con validación client-side
- ✅ Implementar webhook en n8n para recepción de datos
- ✅ Validar campos obligatorios y formato PDF
- ✅ Almacenar solicitudes en Google Sheets
- ✅ Registrar logs de operaciones
- ✅ Enviar email de confirmación automático
- ✅ Retornar respuesta JSON al cliente

## 📁 Estructura de Archivos

```
hu001/
├── README.md                               # Este archivo
├── formulario-convalidacion-unab.html     # Formulario web
│
├── workflows/                              # Workflows de n8n
│   └── flow_HU01.json                     # Workflow principal
│
├── docs/                                   # Documentación
│   ├── HU-01_FICHA_TECNICA.md
│   ├── HU-01_CASOS_PRUEBA.md
│   ├── HU-01_RESULTADOS_PRUEBAS.md
│   └── HU-01_RESUMEN_FINAL.md
│
└── tests/                                  # Pruebas
    └── test_hu01.sh                       # Script de tests automatizados
```

## 🚀 Guía de Uso Rápida

### 1. Importar Workflow en n8n

```bash
# El workflow está en:
workflows/flow_HU01.json

# En n8n:
# 1. Abrir http://localhost:5678
# 2. Menú → Import from File
# 3. Seleccionar: workflows/flow_HU01.json
# 4. Configurar credenciales (ver sección Configuración)
# 5. Activar workflow
```

### 2. Configurar Credenciales

#### Google Sheets OAuth2
Ver guía completa: `/n8n/CONFIGURACION_APIS.md`

**Pasos rápidos:**
1. Google Cloud Console → Nuevo Proyecto
2. Habilitar APIs: Google Sheets + Google Drive
3. Crear OAuth2 Client ID
4. Agregar usuario de prueba en OAuth Consent Screen
5. Configurar en n8n con Client ID y Secret
6. Autorizar acceso

#### SMTP Email
**Opción 1 - Ethereal (Testing):**
- URL: https://ethereal.email/create
- Copiar credenciales generadas
- Host: smtp.ethereal.email, Port: 587

**Opción 2 - Gmail (Producción):**
- Habilitar verificación en 2 pasos
- Generar App Password
- Host: smtp.gmail.com, Port: 587

### 3. Levantar Formulario

```bash
# Desde la carpeta hu001
python3 -m http.server 8080

# Acceder a:
# http://localhost:8080/formulario-convalidacion-unab.html
```

### 4. Ejecutar Tests

```bash
# Desde la carpeta hu001
cd tests
chmod +x test_hu01.sh
./test_hu01.sh
```

## 🧪 Casos de Prueba

El script de tests ejecuta:

- **TC1.1:** Solicitud válida completa → HTTP 200
- **TC1.2:** Campo obligatorio faltante → Error validación
- **TC1.3:** Múltiples campos faltantes → Lista de errores
- **TC5.1:** Caracteres especiales → Procesamiento correcto
- **TC2.1:** Email de confirmación → Email enviado

## ⚙️ Configuración del Webhook

**URL:** `http://localhost:5678/webhook/solicitud-convalidacion`  
**Método:** POST  
**Content-Type:** application/json

**Campos esperados:**
```json
{
  "nombre": "string (requerido)",
  "rut": "string (requerido)",
  "email": "string (requerido)",
  "carrera": "string (requerido)",
  "asignatura": "string (requerido)",
  "institucionOrigen": "string (requerido)",
  "documentos": "string (requerido, .pdf)"
}
```

## 📊 Google Sheets - Estructura

### Hoja "Solicitudes" (Sheet ID: 0)
| Columna | Tipo | Descripción |
|---------|------|-------------|
| ID | String | Identificador único (SOL-YYYYMMDD-XXX) |
| Fecha | DateTime | Fecha y hora de solicitud |
| Estudiante | String | Nombre completo |
| RUT | String | RUT del estudiante |
| Carrera | String | Carrera actual |
| Asignatura | String | Asignatura a convalidar |
| InstitucionOrigen | String | Institución de origen |
| Estado | String | Estado inicial: "Pendiente" |

### Hoja "Logs" (Sheet ID: 1)
| Columna | Tipo | Descripción |
|---------|------|-------------|
| Timestamp | DateTime | Fecha/hora del evento |
| ID | String | ID de la solicitud |
| Status | String | success / error |
| Details | String | Detalles del evento |

## 🔧 Flujo del Workflow

```
1. [Webhook] Recibe POST con datos
        ↓
2. [Function] Valida campos obligatorios y PDF
        ↓
3. [IF] ¿Validación OK?
        ↓ SÍ                    ↓ NO
4. [Function] Genera ID    →  [Respond] Error 400
        ↓
5. [Google Sheets] → Solicitudes
        ↓
6. [Google Sheets] → Logs
        ↓
7. [Email] Confirmación
        ↓
8. [Respond] Success 200
```

## 🐛 Problemas Comunes y Soluciones

### Error: "Webhook not registered"
**Causa:** Workflow no está activo  
**Solución:** Activar workflow con el toggle en n8n

### Error: "Unable to sign without access token"
**Causa:** Credencial OAuth2 no autorizada  
**Solución:** Reconectar credencial en n8n

### Error: "Sheet with ID not found"
**Causa:** ID de hoja incorrecto  
**Solución:** Usar ID numérico (0 para primera hoja, 1 para segunda)

### Emails no se ven con formato
**Causa:** Modo texto plano en lugar de Expression  
**Solución:** Activar modo "Expression" en nodo Email

## 📈 Métricas de Éxito

- ✅ 100% solicitudes válidas procesadas
- ✅ Tiempo de respuesta < 8 segundos
- ✅ 0% pérdida de datos
- ✅ Email enviado en < 10 segundos
- ✅ Validación de PDF funcional
- ✅ Todos los criterios de aceptación cumplidos

## 🔄 Próximos Pasos / Mejoras Futuras

- [ ] Implementar upload de archivos PDF binarios a Google Drive
- [ ] Agregar autenticación de estudiantes
- [ ] Dashboard de visualización de solicitudes
- [ ] Notificaciones push en tiempo real
- [ ] Sistema de seguimiento de estados

## 📞 Contacto

**Desarrollador:** Lucas Maulén Riquelme  
**Email:** l.maulnriquelme@uandresbello.edu  
**Proyecto:** Sistema de Convalidaciones UNAB

## 📝 Notas de Desarrollo

- Formulario validado con JavaScript antes de envío
- Backend valida nuevamente por seguridad
- Sistema resiliente a errores (manejo de excepciones)
- Logs detallados para debugging
- Respuestas JSON estandarizadas

---
**Última actualización:** 19 de octubre de 2025
