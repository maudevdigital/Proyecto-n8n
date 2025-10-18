# 🚨 SOLUCIÓN URGENTE: Corregir Hojas Invertidas

## ⚠️ Problema Identificado

**LO QUE ESTÁ PASANDO:**
- Ambas hojas (Solicitudes y Logs) tienen EXACTAMENTE los mismos datos
- Ambas tienen columnas: nombre, rut, carrera, asignatura, institucionOrigen, email, documentos, success, documentosCompletos, fechaSolicitud, id, estado, pdfsOk, badFiles, validacionPDF

**LO QUE DEBERÍA PASAR:**

**Hoja SOLICITUDES debe tener SOLO:**
```
ID | Fecha | Estudiante | RUT | Carrera | Asignatura | InstitucionOrigen | Estado
```

**Hoja LOGS debe tener SOLO:**
```
Timestamp | ID | Status | Details
```

**CAUSA:** Los nodos DB-Registro y DB-Log están usando "Map Automatically" y están mapeando TODAS las columnas del JSON, cuando solo deben mapear columnas específicas.

## ✅ Solución - Pasos Exactos en n8n

### PASO 1: Corregir nodo DB-Registro

1. **Abre n8n**: http://localhost:5678
2. **Ve al flujo HU-01**
3. **Haz clic en el nodo "DB-Registro"**
4. **IMPORTANTE - Cambiar a mapeo MANUAL:**
   - **Mapping Column Mode**: Cambia a **"Map Each Column Manually"**
   
5. **Configurar Sheet:**
   - **Sheet**: `Solicitudes` (o `0`)
   
6. **Configurar SOLO estas columnas (elimina las demás):**
   ```
   Column: ID          → Value: ={{$json.id}}
   Column: Fecha       → Value: ={{$json.fechaSolicitud}}
   Column: Estudiante  → Value: ={{$json.nombre}}
   Column: RUT         → Value: ={{$json.rut}}
   Column: Carrera     → Value: ={{$json.carrera}}
   Column: Asignatura  → Value: ={{$json.asignatura}}
   Column: InstitucionOrigen → Value: ={{$json.institucionOrigen}}
   Column: Estado      → Value: ={{$json.estado}}
   ```

7. **ELIMINAR columnas como:**
   - success
   - documentosCompletos
   - pdfsOk
   - badFiles
   - validacionPDF
   - email (opcional, puedes dejarlo si quieres)
   - documentos (opcional)

### PASO 2: Corregir nodo DB-Log

1. **Haz clic en el nodo "DB-Log"**
2. **IMPORTANTE - Cambiar a mapeo MANUAL:**
   - **Mapping Column Mode**: Cambia a **"Map Each Column Manually"**

3. **Configurar Sheet:**
   - **Sheet**: `Logs` (o `1`)

4. **Configurar SOLO estas columnas (elimina las demás):**
   ```
   Column: Timestamp   → Value: ={{$json.fechaSolicitud}}
   Column: ID          → Value: ={{$json.id}}
   Column: Status      → Value: Solicitud recibida y registrada
   Column: Details     → Value: HU-01: Recepción exitosa - Estudiante: {{$json.nombre}}
   ```

5. **ELIMINAR todas las otras columnas que aparezcan**

6. **Guardar el nodo**

### PASO 3: Verificar las Hojas

En tu Google Sheets, asegúrate de que:

**Hoja "Solicitudes" (primera pestaña):**
```
A1: ID | B1: Fecha | C1: Estudiante | D1: RUT | E1: Carrera | F1: Asignatura | G1: InstitucionOrigen | H1: Estado
```

**Hoja "Logs" (segunda pestaña):**
```
A1: Timestamp | B1: ID | C1: Status | D1: Details
```

### PASO 4: Probar

Ejecuta este comando para probar:
```bash
curl -X POST http://localhost:5678/webhook/solicitud-convalidacion \
  -H "Content-Type: application/json" \
  -d '{
    "nombre": "TEST CORRECCION",
    "rut": "11.111.111-1",
    "carrera": "TEST",
    "asignatura": "TEST",
    "institucionOrigen": "TEST",
    "documentos": "test.pdf",
    "email": "test@test.com"
  }'
```

**Verificar en Google Sheets:**
- ✅ Hoja "Solicitudes" debe tener nueva fila con: TEST CORRECCION, 11.111.111-1, TEST, etc.
- ✅ Hoja "Logs" debe tener nueva fila con: Timestamp, ID, "Solicitud recibida y registrada"

## 🎯 Configuración Correcta en n8n

### Nodo DB-Registro:
```yaml
Name: DB-Registro
Type: Google Sheets
Credential: Google Sheets account
Resource: Sheet Within Document
Operation: Append Row
Document: By ID → (tu ID de Sheets)
Sheet: Solicitudes (o 0)
Mapping Column Mode: Map Automatically
```

### Nodo DB-Log:
```yaml
Name: DB-Log
Type: Google Sheets
Credential: Google Sheets account
Resource: Sheet Within Document
Operation: Append Row
Document: By ID → (mismo ID de Sheets)
Sheet: Logs (o 1)
Mapping Column Mode: Map Automatically
```

## 📝 Truco Rápido

Si el dropdown de "Sheet" no muestra las hojas:

1. **Usa el ID numérico:**
   - Primera hoja (Solicitudes) = `0`
   - Segunda hoja (Logs) = `1`

2. **Escribe directamente:**
   - En DB-Registro: escribe `0`
   - En DB-Log: escribe `1`

## ⚡ Si Nada Funciona

**Solución drástica:**

1. Elimina ambos nodos (DB-Registro y DB-Log)
2. Crea nuevo nodo Google Sheets para Solicitudes
3. Crea nuevo nodo Google Sheets para Logs
4. Conecta correctamente en el flujo
5. Configura las columnas desde cero

---
**IMPORTANTE:** Después de cambiar, GUARDA el workflow y prueba inmediatamente.
