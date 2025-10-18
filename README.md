# 🎓 Sistema de Gestión de Convalidaciones Académicas - UNAB

## 📋 Descripción
Sistema automatizado para gestionar solicitudes de convalidación de asignaturas en la Universidad Andrés Bello, implementado con n8n + Google Workspace.

## 👥 Equipo
- **PO:** Felipe A.
- **SM:** Franco L.
- **Developers:**
  - Lucas Maulen (HU-01) ⇄ Felipe Vergara R.
  - maticata0111-bit ⇄ lucasmaulenr

## 📁 Estructura del Proyecto

```
├── developer/lucas/     # HU-01: Recepción de Solicitud ✅ COMPLETADO
│   ├── README.md       # 📖 Documentación completa
│   ├── flow_HU01.json  # Flujo n8n
│   ├── formulario-convalidacion-unab.html  # Formulario web
│   └── test_hu01.sh    # Pruebas automatizadas
├── n8n/                # Configuración n8n
└── docker-compose.yml  # Docker setup
```

## 🚀 Quick Start

### Iniciar n8n:
```bash
docker-compose up -d
# Acceder: http://localhost:5678

