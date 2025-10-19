# Guía para Desarrolladores

## 🎯 Estructura del Proyecto

Este proyecto utiliza una estructura modular para facilitar el trabajo en equipo:

```
Proyecto-n8n/
├── README.md                          # Documentación principal
├── docker-compose.yml                 # Configuración de n8n
├── .gitignore                         # Archivos a ignorar
│
├── developers/                        # 👥 Carpeta por desarrollador
│   └── lucas/                         # Carpeta del desarrollador Lucas
│       └── hu001/                     # Historia de Usuario 001
│           ├── README.md              # Documentación de la HU
│           ├── CONFIG.md              # Configuración local
│           ├── formulario-convalidacion-unab.html
│           ├── workflows/             # Workflows de n8n
│           │   └── flow_HU01.json
│           ├── docs/                  # Documentación
│           │   ├── HU-01_FICHA_TECNICA.md
│           │   ├── HU-01_CASOS_PRUEBA.md
│           │   ├── HU-01_RESULTADOS_PRUEBAS.md
│           │   └── HU-01_RESUMEN_FINAL.md
│           └── tests/                 # Scripts de prueba
│               └── test_hu01.sh
│
├── shared/                            # 📚 Recursos compartidos
│   ├── docs/                          # Documentación general
│   │   └── CONFIGURACION_APIS.md
│   ├── specs/                         # Especificaciones del proyecto
│   │   ├── Proyecto-Gestor-Convalidaciones-Academicas.txt
│   │   └── sprint1.txt
│   └── scripts/                       # Scripts utilitarios
│
└── n8n/                               # 💾 Datos persistentes de n8n
    ├── database.sqlite                # NO versionar
    ├── config                         # NO versionar
    ├── binaryData/                    # NO versionar
    └── nodes/                         # Nodos personalizados
```

## 👨‍💻 Flujo de Trabajo para Nuevos Desarrolladores

### 1. Setup Inicial

```bash
# Clonar el repositorio
git clone https://github.com/maudevdigital/Proyecto-n8n.git
cd Proyecto-n8n

# Iniciar n8n
docker-compose up -d

# Verificar que n8n está corriendo
curl http://localhost:5678/healthz
```

### 2. Crear Tu Espacio de Trabajo

```bash
# Crear carpeta para tu trabajo
mkdir -p developers/TU_NOMBRE/huXXX/{workflows,docs,tests}

# Copiar plantillas si es necesario
cp developers/lucas/hu001/README.md developers/TU_NOMBRE/huXXX/
cp developers/lucas/hu001/CONFIG.md developers/TU_NOMBRE/huXXX/
```

### 3. Importar Workflows

```bash
# Tus workflows van en:
developers/TU_NOMBRE/huXXX/workflows/

# Para importar en n8n:
# 1. Abrir http://localhost:5678
# 2. Menú → Import from File
# 3. Seleccionar tu archivo .json
```

### 4. Documentar Tu Trabajo

Cada historia de usuario debe tener:

- ✅ `README.md` - Descripción y guía de uso
- ✅ `CONFIG.md` - Configuración y variables
- ✅ `workflows/` - Archivos .json de n8n
- ✅ `docs/` - Documentación técnica
- ✅ `tests/` - Scripts de prueba

## 🔀 Convenciones de Git

### Branches

```bash
# Crear branch para tu HU
git checkout -b feature/huXXX-nombre-descriptivo

# Ejemplo:
git checkout -b feature/hu002-validacion-documentos
```

### Commits

Usar convenciones de [Conventional Commits](https://www.conventionalcommits.org/):

```bash
# Tipos de commit
feat:     # Nueva funcionalidad
fix:      # Corrección de bug
docs:     # Cambios en documentación
style:    # Formato, sin cambios de código
refactor: # Refactorización de código
test:     # Agregar o modificar tests
chore:    # Tareas de mantenimiento

# Ejemplos:
git commit -m "feat(hu001): agregar validación de RUT"
git commit -m "fix(hu001): corregir formato de email"
git commit -m "docs(hu001): actualizar README con nuevos casos"
```

### Pull Requests

1. Crear PR desde tu branch a `main`
2. Título descriptivo: `[HU-XXX] Descripción breve`
3. Incluir en la descripción:
   - ✅ Qué se implementó
   - ✅ Cómo probarlo
   - ✅ Screenshots si aplica
   - ✅ Checklist de criterios de aceptación

## 📦 Estructura de una Historia de Usuario

### Template de Carpeta

```
developers/TU_NOMBRE/huXXX/
├── README.md                    # Descripción general y guía
├── CONFIG.md                    # Configuración específica
├── formulario-XXX.html          # Frontend (si aplica)
│
├── workflows/                   # Workflows de n8n
│   ├── flow_main.json          # Workflow principal
│   └── flow_helper.json        # Workflows auxiliares
│
├── docs/                        # Documentación técnica
│   ├── HU-XXX_FICHA_TECNICA.md
│   ├── HU-XXX_CASOS_PRUEBA.md
│   ├── HU-XXX_RESULTADOS_PRUEBAS.md
│   └── HU-XXX_RESUMEN_FINAL.md
│
└── tests/                       # Scripts de prueba
    ├── test_huXXX.sh           # Tests automatizados
    └── test_data/              # Datos de prueba
```

## 🔧 Comandos Útiles

```bash
# Iniciar n8n
docker-compose up -d

# Detener n8n
docker-compose down

# Ver logs de n8n
docker-compose logs -f n8n

# Reiniciar n8n (aplicar cambios)
docker-compose restart n8n

# Exportar workflow desde n8n
# En n8n UI: Menú → Download (guarda como .json)

# Ejecutar tests
cd developers/TU_NOMBRE/huXXX/tests
./test_huXXX.sh

# Ver estructura del proyecto
tree -L 3 -I 'node_modules|.git'
```

## 📚 Recursos Compartidos

### Especificaciones (`shared/specs/`)

- `Proyecto-Gestor-Convalidaciones-Academicas.txt` - Visión general
- `sprint1.txt` - Detalles del Sprint 1
- Agregar nuevos documentos de specs aquí

### Documentación (`shared/docs/`)

- `CONFIGURACION_APIS.md` - Guía de configuración de APIs
- Documentación técnica compartida por todos

### Scripts (`shared/scripts/`)

- Scripts utilitarios compartidos
- Helpers de desarrollo
- Scripts de deployment

## 🤝 Mejores Prácticas

### 1. Mantén Tu Espacio Organizado

- ✅ Un workflow por archivo .json
- ✅ Nombres descriptivos para archivos
- ✅ README actualizado con cada cambio
- ✅ Tests funcionando antes de hacer commit

### 2. Documenta Todo

- ✅ Comentarios en workflows complejos
- ✅ README con ejemplos de uso
- ✅ CONFIG.md con todas las variables
- ✅ Casos de prueba documentados

### 3. No Versiones Datos Sensibles

**NUNCA incluir en git:**
- ❌ `n8n/database.sqlite`
- ❌ `n8n/config` con credenciales
- ❌ Archivos con API keys
- ❌ Contraseñas en código

Usar `.gitignore` y variables de entorno.

### 4. Prueba Antes de Hacer Push

```bash
# Antes de commit:
1. Ejecuta tus tests
2. Verifica que el workflow funciona en n8n
3. Revisa que no rompiste nada de otros devs
4. Actualiza documentación si es necesario
```

## 🐛 Debugging

### n8n no inicia

```bash
# Ver logs
docker-compose logs n8n

# Verificar puertos
netstat -tlnp | grep 5678

# Reiniciar limpio
docker-compose down
docker-compose up -d
```

### Workflow no ejecuta

1. ✅ Verificar que está activo (toggle en n8n)
2. ✅ Revisar credenciales configuradas
3. ✅ Ver logs de ejecución en n8n
4. ✅ Probar manualmente cada nodo

### Tests fallan

1. ✅ Verificar que n8n está corriendo
2. ✅ Workflow está activo
3. ✅ URL del webhook es correcta
4. ✅ Datos de prueba son válidos

## 📞 Contacto y Soporte

- **Slack/Teams:** Canal #proyecto-convalidaciones
- **Email:** soporte-dev@unab.cl
- **Wiki:** [Link a wiki interna]
- **Issues:** GitHub Issues del repositorio

## 📝 Checklist para Nueva HU

Antes de considerar una HU como "terminada":

- [ ] Workflow implementado y probado
- [ ] Tests automatizados funcionando
- [ ] Documentación completa (README, CONFIG, docs/)
- [ ] Casos de prueba documentados y pasando
- [ ] Código revisado por al menos 1 peer
- [ ] No hay credenciales hardcodeadas
- [ ] .gitignore actualizado si es necesario
- [ ] Pull Request creado y revisado
- [ ] Demo exitosa con el equipo

---

**¿Preguntas?** Revisa la documentación en `shared/docs/` o contacta al equipo.
