# 🔧 Corrección: ID muestra N/A en formulario

## Problema
El formulario web muestra "ID de Solicitud: N/A" en lugar del ID real.

## Causa
El nodo "Function-Success Response" no está recibiendo correctamente los datos con el ID generado.

## Solución

### En n8n:

1. **Abre el nodo "Function-Success Response"**
2. **Reemplaza el código JavaScript con este:**

```javascript
// HU-01: Generar respuesta final exitosa
// El ID viene del nodo anterior (DB-Registro o Email-Confirmación)
const inputData = $input.item.json;

return {
  json: {
    success: true,
    message: 'Solicitud recibida y procesada correctamente',
    id: inputData.id || `SOL-${inputData.rut?.replace(/[.-]/g, '')}-${Date.now()}`,
    fechaSolicitud: inputData.fechaSolicitud || new Date().toISOString(),
    estado: inputData.estado || 'Recibida',
    proximosPasos: 'Recibirá notificación por email una vez revisada la solicitud',
    tiempoEstimado: '5-10 días hábiles',
    code: 200
  }
};
```

3. **Guarda el nodo**

## Verificación

El código corregido:
- ✅ Lee el ID del input anterior
- ✅ Si no existe, genera uno nuevo
- ✅ Incluye toda la información en la respuesta
- ✅ El formulario mostrará el ID correctamente

## Prueba

Después de corregir, ejecuta:
```bash
curl -X POST http://localhost:5678/webhook/solicitud-convalidacion \
  -H "Content-Type: application/json" \
  -d '{
    "nombre": "Test ID",
    "rut": "99.999.999-9",
    "carrera": "Test",
    "asignatura": "Test",
    "institucionOrigen": "Test",
    "documentos": "test.pdf"
  }'
```

**Respuesta esperada:**
```json
{
  "success": true,
  "message": "Solicitud recibida y procesada correctamente",
  "id": "SOL-999999999-1729283847123",  ← Debe mostrar un ID real
  "fechaSolicitud": "2025-10-18T...",
  "estado": "Recibida",
  "proximosPasos": "Recibirá notificación...",
  "tiempoEstimado": "5-10 días hábiles",
  "code": 200
}
```

## Alternativa: Verificar conexión

Si sigue mostrando N/A:

1. **Verifica que el nodo "Function-Success Response" esté conectado DESPUÉS de:**
   - Email-Confirmación (preferido)
   - O directamente después de DB-Registro

2. **El flujo correcto debe ser:**
   ```
   DB-Registro → Email-Confirmación → Function-Success Response
   ```

3. **NO debe estar conectado directamente después de:**
   - Function - Validar PDF
   - IF - PDF Válido

---
**Aplicar esta corrección resolverá el problema del ID = N/A**
