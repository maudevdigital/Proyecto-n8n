# Documentación: Flujo n8n — Convalidaciones Académicas (Estructura Base)

## Resumen

Este repositorio contiene un flujo en n8n para gestionar solicitudes de convalidación académica. El objetivo del archivo `flow.json` es proporcionar un "esqueleto" completo y preparado para conectar servicios (almacenamiento, base de datos, correo, generación de documentos) cuando se dispongan las credenciales y endpoints.

Nombre del flujo: Convalidaciones Académicas - Estructura Base


## Objetivos del flujo

- Recepción de solicitudes mediante webhook
- Validación de datos mínimos
- Almacenamiento de documentos recibidos
- Registro inicial en hoja de cálculo (Google Sheets)
- Notificación al director (por email) para revisión
- Recepción de la decisión vía webhook
- Procesamiento de la decisión y generación de acta (PDF)
- Notificación al estudiante con el resultado
- Registro histórico de la resolución


## Archivo principal
- `flow.json`: definición exportable del flujo n8n con todos los nodos y conexiones.


## Estructura de nodos (descripción)

1. Webhook - Recepción
   - Tipo: Webhook (POST)
   - Path: `/solicitud-convalidacion`
   - Autenticación: basicAuth (ejemplo añadido)
   - Response Mode: onReceived
   - Output: JSON con los datos de la solicitud

2. Function - Validar
   - Tipo: Function
   - Función: valida campos obligatorios: `nombre`, `carrera`, `materias`, `documentos`.
   - Output: si falta algún campo devuelve `success: false` con `errors`. Si es correcto añade `documentosCompletos: true` y `fechaSolicitud`.

3. IF - Documentación
   - Tipo: If/Condicional
   - Condición: `documentosCompletos === true`
   - Ramas: true -> subir documentos y registrar; false -> (pendiente de implementación: respuesta de error al origen)

4. Storage - Documentos
   - Tipo: Google Drive (placeholder)
   - Operación: upload (subida de archivos)

5. DB - Registro
   - Tipo: Google Sheets (placeholder)
   - Operación: append
   - SheetName: `Solicitudes` (campo añadido al esqueleto)
   - Columnas: ID, Fecha, Estudiante, Estado

6. Email - Director
   - Tipo: Email Send (placeholder)
   - Para: `={{$json.directorEmail}}` (plantilla)
   - Asunto: Nueva Solicitud de Convalidación

7. Webhook - Decisión
   - Tipo: Webhook (GET)
   - Path: `/decision-director`
   - Uso: punto de entrada para la decisión del director (aceptar/rechazar)

8. Function - Procesar Decisión
   - Tipo: Function
   - Procesa la decisión recibida, actualiza estado, prepara datos para la generación del PDF

9. PDF - Generar Acta
   - Tipo: HTTP Request (placeholder)
   - Uso: Llamar a un servicio externo que genere el PDF (o usar un nodo de PDF nativo si se configura)

10. Email - Estudiante
    - Tipo: Email Send
    - Para: `={{$json.estudianteEmail}}`
    - Asunto: Resultado Solicitud de Convalidación

11. DB - Histórico
    - Tipo: Google Sheets
    - Operación: append
    - Uso: almacenar el resultado final y metadatos


## Contrato de datos (inputs/outputs)

- Entrada (Webhook - Recepción) - JSON esperado mínimo:
  {
    "nombre": "Nombre del estudiante",
    "carrera": "Ingeniería X",
    "materias": [ { "nombre": "Materia A", "creditos": 4, "nota": "A" } ],
    "documentos": [ { "fileName": "certificado.pdf", "url": "..." } ],
    "estudianteEmail": "est@ejemplo.com",
    "directorEmail": "director@universidad.edu"
  }

- Salidas intermedias:
  - `Function - Validar` devuelve `success: false` con `errors` si faltan campos.
  - Cuando es válido, añade `documentosCompletos: true` y `fechaSolicitud`.

- Output final:
  - Registro en `DB - Histórico` con campos de control: ID, fecha, estado, observaciones, enlace al acta (PDF)


## Casos borde y consideraciones

- Solicitudes sin documentos: ruta de rechazo o petición de reenvío (no implementado en skeleton, marcado como mejora).
- Documentos corruptos o no accesibles: retry o notificación al estudiante.
- Duplicados: comprobar por `nombre` + `fecha` o por un identificador único si está disponible.
- Seguridad: el webhook debería usar autenticación y validación de firma en entornos productivos.


## Diagrama (Mermaid)

A continuación se incluye un diagrama en formato Mermaid que representa la estructura lógica del flujo. Puedes pegar este bloque en un editor que soporte Mermaid (por ejemplo VS Code con la extensión 'Markdown Preview Mermaid Support') para visualizarlo.

```mermaid
flowchart LR
  A[Webhook - Recepción \n POST /solicitud-convalidacion] --> B[Function - Validar]
  B --> C{IF - Documentación}
  C -- Sí --> D[Storage - Documentos \n Google Drive]
  C -- Sí --> E[DB - Registro \n Google Sheets: Solicitudes]
  D --> F[Email - Director]
  E --> F
  F --> G[Webhook - Decisión \n GET /decision-director]
  G --> H[Function - Procesar Decisión]
  H --> I[PDF - Generar Acta]
  I --> J[Email - Estudiante]
  J --> K[DB - Histórico]

  C -- No --> L[Responder error al origen (pendiente)]
```


## Pasos siguientes sugeridos para completar el flujo

1. Configurar credenciales de Google Drive y Google Sheets en n8n.
2. Crear/usar plantillas de correo y configurar un servicio SMTP.
3. Implementar el nodo de generación de PDF (servicio externo o nodo interno).
4. Añadir manejo de errores y reintentos.
5. Añadir tests o un entorno de staging para validación.


## Archivo(s) añadidos/modificados

- `n8n/DOCUMENTACION.md` — Documentación generada (este archivo).
- `n8n/flow.json` — Se actualizaron algunos parámetros de ejemplo (validación, headers, sheet columns).


## Verificación rápida

- Este repositorio contiene la estructura; los nodos de terceros son placeholders y requieren credenciales.
- El flujo es exportable e importable en n8n como base para la práctica.


## Grafica/Diagrama

[ENTRADA] → [PROCESAMIENTO] → [ALMACENAMIENTO] → [NOTIFICACIONES] → [DECISIÓN] → 
→[RESULTADO]
   ↓              ↓                   ↓                  ↓              ↓            ↓
Webhook       Validación          Base Datos         Emails         Webhook      Actualización
Form Input    Documentos          Documentos         Director       Director     Final + Email
                  ↓                   ↓                                             ↓
              Verificación        Google Drive                                  Histórico

## 1.Entrada/Trigger
[Webhook Form]
├─ Nombre, RUT
├─ Carrera
├─ Asignatura
├─ Institución origen
├─ Documento PDF
└─ Email


## 2.Procesamiento
[Validador]
├─ Check campos requeridos
└─ IF (documentos completos)
    ├─ TRUE → Continuar flujo
    └─ FALSE → Email solicitud documentos

## 3.Almacenamiento
[Storage Handler]
├─ Google Drive / Storage
│  └─ Guardar PDF
└─ Database / Sheets
   └─ Registro inicial (estado: PENDIENTE)

## 4.Notificaciones
[Email Handler]
├─ Template Director
│  ├─ Datos solicitud
│  └─ Botones/Links decisión
└─ CC Comisión (opcional)

## 5.Decision
[Decision Webhook]
├─ Input: solicitudId + acción
└─ SWITCH (acción)
    ├─ ACEPTAR
    ├─ RECHAZAR
    └─ REVISIÓN

## 6.Resultado
[Result Handler]
├─ Actualizar estado DB/Sheets
├─ Generar PDF (acta)
├─ Email estudiante
│  ├─ Resultado
│  └─ Adjunto acta
└─ Registro histórico