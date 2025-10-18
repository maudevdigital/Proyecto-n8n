# 🔧 CORRECCIÓN: Problema de Hojas en Google Sheets

## ⚠️ Problema Detectado
Las hojas "Solicitudes" y "Logs" están guardando datos en la hoja incorrecta:
- **DB-Registro** guarda en "Logs" (debería guardar en "Solicitudes")
- **DB-Log** guarda en "Solicitudes" (debería guardar en "Logs")

## ✅ Solución

### En n8n:

#### Nodo DB-Registro:
1. Abrir el nodo "DB-Registro"
2. Verificar configuración:
   - **Document**: By ID → (ID del Google Sheets)
   - **Sheet**: Cambiar a `0` (primera hoja = Solicitudes)
   - **Operation**: Append Row
   - **Mapping Column Mode**: Map Automatically

#### Nodo DB-Log:
1. Abrir el nodo "DB-Log"
2. Verificar configuración:
   - **Document**: By ID → (mismo ID)
   - **Sheet**: Cambiar a `1` (segunda hoja = Logs)
   - **Operation**: Append Row
   - **Mapping Column Mode**: Map Automatically

## 📝 Alternativa: Usar nombres de hojas

Si prefieres usar nombres en lugar de IDs numéricos:

### Opción 1: Verificar dropdown
1. En el campo "Sheet", buscar el dropdown
2. Seleccionar "Solicitudes" o "Logs" de la lista

### Opción 2: Modo By Name
1. Buscar si hay opción "By Name"
2. Escribir exactamente: `Solicitudes` o `Logs`

## 🔍 Verificación

Después de corregir, probar con:
```bash
curl -X POST http://localhost:5678/webhook/solicitud-convalidacion \
  -H "Content-Type: application/json" \
  -d '{
    "nombre": "Test Verificación",
    "rut": "99.999.999-9",
    "carrera": "Test",
    "asignatura": "Test",
    "institucionOrigen": "Test",
    "documentos": "test.pdf"
  }'
```

Luego revisar:
- ✅ Hoja "Solicitudes" debe tener nueva fila con datos de estudiante
- ✅ Hoja "Logs" debe tener nueva fila con timestamp y status

## 📊 Estructura Correcta

### Google Sheets debe tener:
- **Hoja 0 (Solicitudes)**: Datos de solicitudes de estudiantes
- **Hoja 1 (Logs)**: Registros de eventos del sistema

### Nombres de pestañas:
- Primera pestaña: "Solicitudes"
- Segunda pestaña: "Logs"

## ⚡ Solución Rápida

Si el problema persiste:

1. **Eliminar ambos nodos** DB-Registro y DB-Log
2. **Crear nuevos nodos** Google Sheets
3. **Configurar desde cero**:
   - Primero DB-Registro → Hoja "Solicitudes" (o ID 0)
   - Luego DB-Log → Hoja "Logs" (o ID 1)

---
**Nota:** El ID numérico de las hojas es más confiable que el nombre.
- ID 0 = Primera hoja
- ID 1 = Segunda hoja
- ID 2 = Tercera hoja (si existe)
