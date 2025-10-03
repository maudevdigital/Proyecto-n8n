# 📊 Especificaciones Google Sheets - Sistema Convalidaciones

## 🎯 Hojas requeridas según documento TXT

### **Hoja 1: "Solicitudes"**
```
Columnas requeridas:
A1: ID
B1: Fecha  
C1: Estudiante
D1: RUT
E1: Carrera
F1: Asignatura
G1: InstitucionOrigen
H1: Estado
```

**Ejemplo de datos:**
```
A2: EST-001
B2: 2025-10-01T10:30:00Z
C2: Juan Pérez González
D2: 12.345.678-9
E2: Ingeniería Informática
F2: Cálculo Diferencial
G2: Universidad de Chile
H2: Pendiente
```

### **Hoja 2: "Logs"**
```
Columnas:
A1: Timestamp
B1: ID
C1: Status
D1: Details
```

### **Hoja 3: "Historico"**
```
Columnas según TXT:
A1: Estudiante
B1: AsignaturaConvalidada
C1: InstitucionOrigen
D1: EstadoFinal
E1: FechaResolucion
F1: RUT
```

**Ejemplo de datos finales:**
```
A2: Juan Pérez González
B2: Cálculo Diferencial
C2: Universidad de Chile
D2: Aceptada
E2: 2025-10-02T14:30:00Z
F2: 12.345.678-9
```

## 📝 Estructura de datos de entrada (JSON)

El webhook ahora espera este formato exacto:

```json
{
  "nombre": "Juan Pérez González",
  "rut": "12.345.678-9",
  "carrera": "Ingeniería Informática", 
  "asignatura": "Cálculo Diferencial",
  "institucionOrigen": "Universidad de Chile",
  "documentos": [
    {
      "fileName": "certificado-notas.pdf",
      "url": "https://ejemplo.com/documento.pdf"
    }
  ],
  "estudianteEmail": "juan.perez@email.com",
  "directorEmail": "director@universidad.edu"
}
```

## 🔄 URLs del sistema

- **Recepción de solicitudes:** `POST http://localhost:5678/webhook/solicitud-convalidacion`
- **Decisión del director:** `GET http://localhost:5678/webhook/decision-director?id=ID&decision=DECISION`

## 📋 Estados posibles

- **Solicitudes:** Pendiente → Convalidada/No Convalidada/En revisión
- **Decisiones:** Aceptada / Rechazada / Revisión Adicional