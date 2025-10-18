# 🎯 GUÍA PASO A PASO - Configuración Correcta de Nodos Google Sheets

## 📋 Problema Actual

Las hojas Solicitudes y Logs tienen las mismas columnas:
```
nombre, rut, carrera, asignatura, institucionOrigen, email, documentos, 
success, documentosCompletos, fechaSolicitud, id, estado, pdfsOk, 
badFiles, validacionPDF
```

**Esto está MAL ❌**

## ✅ Configuración Correcta

### 🟢 NODO DB-REGISTRO

#### Paso 1: Abrir configuración
1. Haz clic en el nodo "DB-Registro"
2. Ve a la pestaña "Parameters"

#### Paso 2: Configuración básica
```yaml
Credential to connect with: Google Sheets account
Resource: Sheet Within Document
Operation: Append Row
Document: By ID → [tu ID de Google Sheets]
Sheet: Solicitudes (o escribe: 0)
```

#### Paso 3: IMPORTANTE - Modo de mapeo
```yaml
Mapping Column Mode: Map Each Column Manually
```
**⚠️ NO usar "Map Automatically"**

#### Paso 4: Agregar SOLO estas 8 columnas

**Haz clic en "Add Column" 8 veces y configura:**

1. **Column Name**: `ID`
   **Value**: `={{$json.id}}`

2. **Column Name**: `Fecha`
   **Value**: `={{$json.fechaSolicitud}}`

3. **Column Name**: `Estudiante`
   **Value**: `={{$json.nombre}}`

4. **Column Name**: `RUT`
   **Value**: `={{$json.rut}}`

5. **Column Name**: `Carrera`
   **Value**: `={{$json.carrera}}`

6. **Column Name**: `Asignatura`
   **Value**: `={{$json.asignatura}}`

7. **Column Name**: `InstitucionOrigen`
   **Value**: `={{$json.institucionOrigen}}`

8. **Column Name**: `Estado`
   **Value**: `={{$json.estado}}`

#### Paso 5: Guardar
- Haz clic en el botón "Save" o fuera del nodo

---

### 🔵 NODO DB-LOG

#### Paso 1: Abrir configuración
1. Haz clic en el nodo "DB-Log"
2. Ve a la pestaña "Parameters"

#### Paso 2: Configuración básica
```yaml
Credential to connect with: Google Sheets account
Resource: Sheet Within Document
Operation: Append Row
Document: By ID → [mismo ID que DB-Registro]
Sheet: Logs (o escribe: 1)
```

#### Paso 3: IMPORTANTE - Modo de mapeo
```yaml
Mapping Column Mode: Map Each Column Manually
```
**⚠️ NO usar "Map Automatically"**

#### Paso 4: Agregar SOLO estas 4 columnas

**Haz clic en "Add Column" 4 veces y configura:**

1. **Column Name**: `Timestamp`
   **Value**: `={{$json.fechaSolicitud}}`

2. **Column Name**: `ID`
   **Value**: `={{$json.id}}`

3. **Column Name**: `Status`
   **Value**: `Solicitud recibida y registrada`
   (⚠️ Este es texto fijo, sin {{}} )

4. **Column Name**: `Details`
   **Value**: `HU-01: Recepción exitosa - Estudiante: {{$json.nombre}}`

#### Paso 5: Guardar
- Haz clic en el botón "Save" o fuera del nodo

---

## 🧹 LIMPIAR Google Sheets

### Antes de probar:

1. **Abre tu Google Sheets**
2. **Hoja "Solicitudes":**
   - Elimina todas las filas de datos (mantén solo los headers)
   - Asegúrate de que los headers sean exactamente:
   ```
   A1: ID
   B1: Fecha
   C1: Estudiante
   D1: RUT
   E1: Carrera
   F1: Asignatura
   G1: InstitucionOrigen
   H1: Estado
   ```

3. **Hoja "Logs":**
   - Elimina todas las filas de datos (mantén solo los headers)
   - Asegúrate de que los headers sean exactamente:
   ```
   A1: Timestamp
   B1: ID
   C1: Status
   D1: Details
   ```

---

## 🧪 Probar

```bash
curl -X POST http://localhost:5678/webhook/solicitud-convalidacion \
  -H "Content-Type: application/json" \
  -d '{
    "nombre": "Prueba Final",
    "rut": "88.888.888-8",
    "carrera": "Test Carrera",
    "asignatura": "Test Asignatura",
    "institucionOrigen": "Test Universidad",
    "documentos": "prueba.pdf",
    "email": "prueba@test.com"
  }'
```

### ✅ Resultado Esperado:

**Hoja SOLICITUDES tendrá:**
```
ID                    | Fecha                  | Estudiante    | RUT           | Carrera      | Asignatura       | InstitucionOrigen | Estado   |
SOL-888888888-1760... | 2025-10-18T21:xx:xx.Z | Prueba Final  | 88.888.888-8  | Test Carrera | Test Asignatura  | Test Universidad  | Recibida |
```

**Hoja LOGS tendrá:**
```
Timestamp              | ID                    | Status                           | Details                                      |
2025-10-18T21:xx:xx.Z | SOL-888888888-1760... | Solicitud recibida y registrada | HU-01: Recepción exitosa - Estudiante: ... |
```

---

## ⚠️ Errores Comunes

### ❌ Error: "No columns found"
**Solución:** Verifica que los headers en Google Sheets estén en la fila 1

### ❌ Error: Aparecen columnas extras
**Solución:** Asegúrate de usar "Map Each Column Manually", NO "Map Automatically"

### ❌ Error: Los datos no aparecen
**Solución:** 
1. Verifica que el Sheet sea "Solicitudes" (o 0) para DB-Registro
2. Verifica que el Sheet sea "Logs" (o 1) para DB-Log

---

## 📝 Checklist Final

Antes de dar por terminado:

- [ ] DB-Registro tiene "Map Each Column Manually"
- [ ] DB-Registro tiene SOLO 8 columnas configuradas
- [ ] DB-Registro apunta a Sheet "Solicitudes" (o 0)
- [ ] DB-Log tiene "Map Each Column Manually"
- [ ] DB-Log tiene SOLO 4 columnas configuradas
- [ ] DB-Log apunta a Sheet "Logs" (o 1)
- [ ] Headers de Google Sheets están correctos
- [ ] Prueba exitosa muestra datos en hojas correctas

---

**Si sigues estos pasos EXACTAMENTE, el problema se resolverá.**
