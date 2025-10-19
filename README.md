# 🎓 Gestor de Convalidaciones Académicas - UNAB# 🎓 Gestor de Convalidaciones Académicas - UNAB# HU-01: Recepción de Solicitud de Convalidación



Sistema automatizado para la recepción, validación y procesamiento de solicitudes de convalidación académica utilizando n8n.



## 📋 Historia de Usuario 01 (HU-01)Sistema automatizado para la recepción, validación y procesamiento de solicitudes de convalidación académica utilizando n8n.## 📋 Descripción



**Como** estudiante de la UNAB  Sistema automatizado para recibir, validar y procesar solicitudes de convalidación de asignaturas de estudiantes universitarios.

**Quiero** enviar mi solicitud de convalidación a través de un formulario web  

**Para que** sea recibida, validada y registrada automáticamente en el sistema## 📋 Historia de Usuario 01 (HU-01)



## 🚀 Inicio Rápido## 🎯 Historia de Usuario



### 1. Iniciar n8n**Como** estudiante de la UNAB  **Como** estudiante universitario,    



```bash**Quiero** enviar mi solicitud de convalidación a través de un formulario web  **Quiero** un formulario web para ingresar mis datos y adjuntar documentos de respaldo,    

docker-compose up -d

```**Para que** sea recibida, validada y registrada automáticamente en el sistema**Para** iniciar el proceso de convalidación de forma digital y centralizada.



n8n estará disponible en: http://localhost:5678



### 2. Importar el Workflow## 🚀 Inicio Rápido## ✅ Criterios de Aceptación



1. Accede a n8n (http://localhost:5678)

2. Ve a: Menú → Import from File

3. Selecciona: `flow_HU01.json`### 1. Iniciar n8n### Escenario 1 – Envío exitoso con todos los datos  

4. Configura las credenciales necesarias

5. Activa el workflow✅ **IMPLEMENTADO**



### 3. Iniciar el Servidor del Formulario```bash- Todos los campos completados correctamente



```bashdocker-compose up -d- Documento PDF adjunto

python3 -m http.server 8080

``````- Sistema registra en Google Sheets



Formulario disponible en: http://localhost:8080/formulario-convalidacion-unab.html- Envía email de confirmación



### 4. Ejecutar Pruebasn8n estará disponible en: http://localhost:5678



```bash### Escenario 2 – Intento de envío sin adjuntar documento  

./test_hu01.sh

```### 2. Importar el Workflow✅ **IMPLEMENTADO**



## 📁 Estructura del Proyecto- Formulario muestra mensaje de error



```1. Accede a n8n (http://localhost:5678)- No se procesa la solicitud

Proyecto-n8n/

├── docker-compose.yml                                # Configuración de n8n2. Ve a: Menú → Import from File

├── flow_HU01.json                                    # Workflow de n8n para HU-01

├── formulario-convalidacion-unab.html               # Formulario web UNAB3. Selecciona: `flow_HU01.json`### Escenario 3 – Intento de envío con formato incorrecto  

├── test_hu01.sh                                      # Script de pruebas automatizadas

├── README.md                                         # Este archivo4. Configura las credenciales necesarias✅ **IMPLEMENTADO**

│

├── Proyecto-Gestor-Convalidaciones-Academicas.txt   # 📄 Especificaciones del proyecto5. Activa el workflow- Sistema rechaza archivos no-PDF

├── sprint1.txt                                       # 📄 Contexto del Sprint 1

│- Notifica que solo se aceptan PDFs

├── HU-01_FICHA_TECNICA.md                           # Especificación técnica

├── HU-01_CASOS_PRUEBA.md                            # Casos de prueba### 3. Iniciar el Servidor del Formulario

├── HU-01_RESULTADOS_PRUEBAS.md                      # Resultados de pruebas

├── HU-01_RESUMEN_FINAL.md                           # Resumen ejecutivo## 🏗️ Arquitectura del Sistema

│

└── n8n/                                              # Datos persistentes de n8n```bash

    ├── database.sqlite                               # Base de datos (workflows, credenciales)

    ├── binaryData/                                   # Archivos binariospython3 -m http.server 8080### Flujo Completo:

    ├── nodes/                                        # Nodos personalizados

    ├── config                                        # Configuración```1. **Formulario Web** → Estudiante ingresa datos

    └── CONFIGURACION_APIS.md                        # Guía de configuración de APIs

```2. **Validación Cliente** → JavaScript valida PDF y campos



## 📄 Archivos de ContextoFormulario disponible en: http://localhost:8080/formulario-convalidacion-unab.html3. **Webhook n8n** → Recibe datos vía POST



### `Proyecto-Gestor-Convalidaciones-Academicas.txt`4. **Validación Servidor** → n8n valida campos obligatorios

Especificaciones generales del proyecto, incluyendo:

- Objetivo del sistema### 4. Ejecutar Pruebas5. **Google Sheets** → Guarda en hoja "Solicitudes"

- Alcance del proyecto

- Historias de usuario planificadas6. **Google Sheets** → Registra log en hoja "Logs"

- Requisitos funcionales

```bash7. **Email SMTP** → Envía confirmación al estudiante

### `sprint1.txt`

Documentación específica del Sprint 1:./test_hu01.sh8. **Respuesta JSON** → Retorna resultado al formulario

- HU-01: Recepción de Solicitud

- Criterios de aceptación```

- Definición de terminado

- Detalles de implementación## 📁 Archivos del Proyecto



## ⚙️ Configuración## 📁 Estructura del Proyecto



### Credenciales Necesarias### `/developer/lucas/`



#### 1. Google Sheets API (OAuth2)``````

- Ver guía completa en: `n8n/CONFIGURACION_APIS.md`

- Crear proyecto en Google Cloud ConsoleProyecto-n8n/├── flow_HU01.json                      # Flujo completo de n8n

- Habilitar APIs: Google Sheets + Google Drive

- Configurar OAuth2 y agregar usuario de prueba├── docker-compose.yml                    # Configuración de n8n├── formulario-convalidacion-unab.html  # Formulario web con colores UNAB



#### 2. SMTP para Emails├── flow_HU01.json                        # Workflow de n8n para HU-01├── test_hu01.sh                        # Script de pruebas automatizadas

- **Opción 1 (Producción):** Gmail con App Password

- **Opción 2 (Testing):** Ethereal Email├── formulario-convalidacion-unab.html   # Formulario web UNAB├── sprint1.txt                         # Documentación Sprint 1

- Configurar credenciales en n8n

├── test_hu01.sh                          # Script de pruebas automatizadas├── Proyecto-Gestor-Convalidaciones-Academicas.txt  # Especificaciones

### Webhook

├── README.md                             # Este archivo└── README.md                           # Este archivo

- **URL de Producción:** `http://localhost:5678/webhook/solicitud-convalidacion`

- **Método:** POST│```

- **Content-Type:** application/json

├── HU-01_FICHA_TECNICA.md               # Especificación técnica

## 🧪 Casos de Prueba

├── HU-01_CASOS_PRUEBA.md                # Casos de prueba## 🚀 Cómo Usar

El script `test_hu01.sh` ejecuta automáticamente:

├── HU-01_RESULTADOS_PRUEBAS.md          # Resultados de pruebas

- ✅ **TC1.1:** Solicitud válida completa

- ✅ **TC1.2:** Campo obligatorio faltante├── HU-01_RESUMEN_FINAL.md               # Resumen ejecutivo### 1. Importar el Flujo en n8n

- ✅ **TC1.3:** Múltiples campos faltantes

- ✅ **TC5.1:** Caracteres especiales en nombres│```bash

- ✅ **TC2.1:** Verificación de email de confirmación

└── n8n/                                  # Datos persistentes de n8n# En n8n, ir a: Import from File

Ver detalles en: `HU-01_CASOS_PRUEBA.md`

    ├── database.sqlite                   # Base de datos (workflows, credenciales)# Seleccionar: flow_HU01.json

## 📊 Funcionalidad Implementada

    ├── binaryData/                       # Archivos binarios```

### Flujo de Trabajo (Workflow)

    ├── nodes/                            # Nodos personalizados

1. **Recepción:** Webhook recibe solicitud POST

2. **Validación:** Verifica campos obligatorios y formato PDF    ├── config                            # Configuración### 2. Configurar Credenciales

3. **Registro:** Almacena en Google Sheets (hoja "Solicitudes")

4. **Logging:** Registra evento en Google Sheets (hoja "Logs")    └── CONFIGURACION_APIS.md            # Guía de configuración de APIs

5. **Notificación:** Envía email de confirmación al estudiante

6. **Respuesta:** Retorna JSON con resultado```#### Google Sheets OAuth2:



### Campos Validados1. Settings > Credentials > Google Sheets OAuth2 API



- Nombre completo del estudiante## ⚙️ Configuración2. Configurar Client ID y Client Secret

- RUT (formato chileno)

- Email institucional3. Autorizar acceso

- Carrera actual

- Asignatura a convalidar### Credenciales Necesarias

- Institución de origen

- Certificado de notas (PDF)#### SMTP (Ethereal Email para pruebas):



## 🔒 Persistencia de Datos#### 1. Google Sheets API (OAuth2)1. Ir a: https://ethereal.email/create



Los datos de n8n (workflows, credenciales, ejecuciones) se almacenan en:- Ver guía completa en: `n8n/CONFIGURACION_APIS.md`2. Copiar credenciales generadas

```

./n8n/database.sqlite- Crear proyecto en Google Cloud Console3. Configurar en n8n

```

- Habilitar APIs: Google Sheets + Google Drive

**Importante:** Esta carpeta garantiza que tu configuración persista entre reinicios.

- Configurar OAuth2 y agregar usuario de prueba### 3. Levantar Formulario Web

## 📝 Documentación Adicional

```bash

- **Ficha Técnica:** `HU-01_FICHA_TECNICA.md`

- **Casos de Prueba:** `HU-01_CASOS_PRUEBA.md`#### 2. SMTP para Emailscd /workspaces/Proyecto-n8n/developer/lucas

- **Resultados:** `HU-01_RESULTADOS_PRUEBAS.md`

- **Resumen Final:** `HU-01_RESUMEN_FINAL.md`- **Opción 1 (Producción):** Gmail con App Passwordpython3 -m http.server 8080

- **Configuración APIs:** `n8n/CONFIGURACION_APIS.md`

- **Opción 2 (Testing):** Ethereal Email# Acceder a: http://localhost:8080/formulario-convalidacion-unab.html

## 🛠️ Comandos Útiles

- Configurar credenciales en n8n```

```bash

# Iniciar n8n

docker-compose up -d

### Webhook### 4. Ejecutar Pruebas

# Detener n8n

docker-compose down```bash



# Ver logs de n8n- **URL de Producción:** `http://localhost:5678/webhook/solicitud-convalidacion`chmod +x test_hu01.sh

docker-compose logs -f

- **Método:** POST./test_hu01.sh

# Reiniciar n8n

docker-compose restart- **Content-Type:** application/json```



# Ejecutar pruebas

./test_hu01.sh

## 🧪 Casos de Prueba## 🧪 Casos de Prueba

# Servidor para formulario

python3 -m http.server 8080

```

El script `test_hu01.sh` ejecuta automáticamente:### TC1.1 - Caso Válido Completo

## 👥 Equipo

**Input:**

**Desarrollador:** Lucas Maulén Riquelme  

**Email:** l.maulnriquelme@uandresbello.edu  - ✅ **TC1.1:** Solicitud válida completa```json

**Institución:** Universidad Andrés Bello (UNAB)

- ✅ **TC1.2:** Campo obligatorio faltante{

## 📄 Licencia

- ✅ **TC1.3:** Múltiples campos faltantes  "nombre": "María Elena Rodríguez",

Proyecto académico - UNAB 2025

- ✅ **TC5.1:** Caracteres especiales en nombres  "rut": "20.111.222-3",

- ✅ **TC2.1:** Verificación de email de confirmación  "carrera": "Psicología",

  "asignatura": "Estadística Aplicada",

Ver detalles en: `HU-01_CASOS_PRUEBA.md`  "institucionOrigen": "Universidad de Santiago",

  "documentos": "certificado_maria.pdf",

## 📊 Funcionalidad Implementada  "email": "maria.elena@test.com"

}

### Flujo de Trabajo (Workflow)```

**Output:** HTTP 200 - Solicitud registrada

1. **Recepción:** Webhook recibe solicitud POST

2. **Validación:** Verifica campos obligatorios y formato PDF### TC1.2 - Campo Obligatorio Faltante

3. **Registro:** Almacena en Google Sheets (hoja "Solicitudes")**Input:** Sin campo `rut`

4. **Logging:** Registra evento en Google Sheets (hoja "Logs")**Output:** HTTP 200 - Error de validación

5. **Notificación:** Envía email de confirmación al estudiante

6. **Respuesta:** Retorna JSON con resultado### TC1.3 - Múltiples Campos Faltantes

**Input:** Solo nombre y carrera

### Campos Validados**Output:** HTTP 200 - Lista de campos faltantes



- Nombre completo del estudiante### TC2.1 - Verificación de Email

- RUT (formato chileno)**Input:** Solicitud válida con email

- Email institucional**Output:** Email de confirmación enviado

- Carrera actual

- Asignatura a convalidar### TC5.1 - Caracteres Especiales

- Institución de origen**Input:** Nombres con tildes, ñ, guiones

- Certificado de notas (PDF)**Output:** HTTP 200 - Procesa correctamente



## 🔒 Persistencia de Datos## 📊 Estructura Google Sheets



Los datos de n8n (workflows, credenciales, ejecuciones) se almacenan en:### Hoja "Solicitudes"

```| ID | Fecha | Estudiante | RUT | Carrera | Asignatura | InstitucionOrigen | Estado |

./n8n/database.sqlite|----|-------|------------|-----|---------|------------|-------------------|--------|

```

### Hoja "Logs"

**Importante:** Esta carpeta garantiza que tu configuración persista entre reinicios.| Timestamp | ID | Status | Details |

|-----------|----|---------| --------|

## 📝 Documentación Adicional

## 🎨 Colores UNAB

- **Ficha Técnica:** `HU-01_FICHA_TECNICA.md`- Rojo Principal: `#E30613`

- **Casos de Prueba:** `HU-01_CASOS_PRUEBA.md`- Rojo Oscuro: `#8B0000`

- **Resultados:** `HU-01_RESULTADOS_PRUEBAS.md`- Gris Oscuro: `#333333`

- **Resumen Final:** `HU-01_RESUMEN_FINAL.md`- Gris Claro: `#666666`

- **Configuración APIs:** `n8n/CONFIGURACION_APIS.md`

## 📧 Configuración Email

## 🛠️ Comandos Útiles

### Ethereal Email (Pruebas)

```bash- Host: smtp.ethereal.email

# Iniciar n8n- Port: 587

docker-compose up -d- SSL/TLS: Desactivado

- Ver emails: https://ethereal.email/messages

# Detener n8n

docker-compose down### Gmail (Producción)

- Host: smtp.gmail.com

# Ver logs de n8n- Port: 587

docker-compose logs -f- SSL/TLS: STARTTLS

- Requiere: App Password

# Reiniciar n8n

docker-compose restart## 🔧 Tecnologías Utilizadas

- **n8n**: Orquestación y automatización

# Ejecutar pruebas- **Google Sheets API**: Almacenamiento de datos

./test_hu01.sh- **SMTP/Ethereal**: Envío de emails

- **HTML/CSS/JavaScript**: Formulario web

# Servidor para formulario- **Python HTTP Server**: Servidor local

python3 -m http.server 8080

```## 📈 Métricas de Éxito

- ✅ 100% de solicitudes válidas procesadas

## 👥 Equipo- ✅ Tiempo de respuesta < 8 segundos

- ✅ 0% pérdida de datos

**Desarrollador:** Lucas Maulén Riquelme  - ✅ Email confirmación enviado en < 10 segundos

**Email:** l.maulnriquelme@uandresbello.edu  - ✅ Validación de PDF funcionando

**Institución:** Universidad Andrés Bello (UNAB)- ✅ Detección de campos faltantes



## 📄 Licencia## 🐛 Solución de Problemas



Proyecto académico - UNAB 2025### Error: "Unable to sign without access token"

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
