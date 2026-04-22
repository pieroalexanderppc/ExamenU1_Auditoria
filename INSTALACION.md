# Guía de Instalación y Configuración

## Requisitos Previos

- **Windows 10/11** (o sistema equivalente)
- **Python 3.8+** (recomendado 3.10 o superior)
- **Node.js 18+** (recomendado LTS 24.x)
- **npm 9+** (incluido con Node.js)
- **Ollama** (para el motor de IA local)

---

## 1. Instalación de Node.js

### En Windows (recomendado: usar winget o descargar directo)

**Opción A: Usar winget (si tienes activado)**
```powershell
winget install OpenJS.NodeJS.LTS
```

**Opción B: Descargar manualmente**
- Visita https://nodejs.org/ 
- Descarga la versión LTS
- Ejecuta el instalador y sigue los pasos

### Verificar instalación
```powershell
node --version
npm --version
```

---

## 2. Configuración de Variable de Entorno (PATH)

Si `npm` no se reconoce en PowerShell, añade Node.js al PATH:

```powershell
$env:Path = "C:\Program Files\nodejs;" + $env:Path
npm --version  # Verifica que funciona
```

**Para hacerlo permanente:**
1. Abre `Editar las variables de entorno del sistema`
2. Click en "Variables de entorno..."
3. En "Variables de usuario", click "Nueva..."
4. Variable: `Path`
5. Valor: `C:\Program Files\nodejs`
6. Reinicia PowerShell

---

## 3. Instalación de Dependencias del Frontend

En la carpeta `AuditoriaRiesgos/`:

```powershell
cd AuditoriaRiesgos
npm install
```

**Qué instala:**
- React 18.2.0
- Vite 5.2.12 (bundler)
- Ant Design 5.17.4 (UI components)
- Axios 1.7.2 (HTTP client)
- Y 297 dependencias más

**Salida esperada:**
```
added 300 packages, audited 302 packages in 16s
```

---

## 4. Configuración del Backend (Flask)

### 4.1 Crear entorno virtual Python

```powershell
cd AuditoriaRiesgos
python -m venv venv
```

### 4.2 Activar entorno virtual

**En PowerShell:**
```powershell
.\venv\Scripts\Activate.ps1
```

**En CMD:**
```cmd
venv\Scripts\activate.bat
```

### 4.3 Instalar dependencias Python

```powershell
pip install flask openai
```

---

## 5. Configuración de Ollama (Motor de IA)

### 5.1 Instalar Ollama

- Descarga desde https://ollama.ai/
- Sigue el instalador para tu sistema operativo
- Ollama se ejecutará en `http://localhost:11434/v1`

### 5.2 Descargar modelo (terminal nueva)

```powershell
ollama pull mistral  # O el modelo que prefieras
```

**Modelos recomendados:**
- `mistral` - Rápido y eficiente
- `neural-chat` - Conversacional
- `openhermes` - Instrucciones generales

### 5.3 Verificar que Ollama está corriendo

```powershell
curl http://localhost:11434/api/tags
```

Si funciona, verás una lista de modelos disponibles.

---

## 6. Compilación del Frontend

Compilar React para producción (necesario para que Flask sirva el frontend):

```powershell
npm run build
```

**Qué genera:**
- `dist/index.html` - Entrada principal
- `dist/assets/` - JavaScript y CSS comprimidos
- Tamaño típico: ~964 KB sin comprimir, ~304 KB gzipped

---

## 7. Ejecución del Proyecto

### 7.1 Backend (Flask)

Terminal 1:
```powershell
cd AuditoriaRiesgos
.\venv\Scripts\Activate.ps1  # Activar venv
python app.py
```

**Salida esperada:**
```
 * Serving Flask app 'app'
 * Debug mode: on
 * Running on http://127.0.0.1:5500
```

### 7.2 Frontend (Vite - Desarrollo)

Terminal 2:
```powershell
cd AuditoriaRiesgos
npm run dev
```

**Salida esperada:**
```
VITE v5.2.12 ready in 237 ms

➜  Local:   http://localhost:5173/
```

### 7.3 Acceder a la aplicación

- **Desarrollo:** http://localhost:5173/
- **Producción:** http://127.0.0.1:5500/

**Credenciales de prueba:**
- Usuario: `admin`
- Contraseña: `123456`

---

## 8. Estructura de Carpetas

```
ExamenU1_Auditoria/
├── README.md                 # Informe académico de auditoría
├── INSTALACION.md           # Esta guía
├── AuditoriaRiesgos/
│   ├── app.py                 # Backend Flask
│   ├── package.json          # Dependencias npm
│   ├── package-lock.json
│   ├── vite.config.js        # Configuración Vite
│   ├── dist/                 # Frontend compilado (generado por npm build)
│   ├── public/               # Assets estáticos
│   ├── src/
│   │   ├── main.jsx         # Entrada React
│   │   ├── App.jsx          # Componente principal
│   │   ├── App.css
│   │   ├── components/
│   │   │   └── Login.jsx    # Pantalla de login
│   │   └── services/
│   │       └── LoginService.js  # Lógica de autenticación
│   ├── node_modules/        # Dependencias JavaScript (generado por npm install)
│   └── venv/               # Entorno virtual Python (generado por python -m venv)
└── img/                     # Imágenes de evidencia
    ├── login.png
    ├── home.png
    ├── app1.png
    ├── app2.png
    └── ...
```

---

## 9. Endpoints Backend

### Análisis de Riesgos
```
POST /analizar-riesgos
Body: { "activo": "Servidor de base de datos" }
Response: { "riesgos": [...], "impactos": [...] }
```

### Sugerir Tratamiento
```
POST /sugerir-tratamiento
Body: { "activo": "...", "riesgo": "...", "impacto": "..." }
Response: { "recomendacion": "..." }
```

### Servir Frontend
```
GET /
GET /<path>
```
Retorna `dist/index.html` y assets compilados.

---

## 10. Troubleshooting

### Error: "npm: no se reconoce"
```powershell
$env:Path = "C:\Program Files\nodejs;" + $env:Path
```

### Error: "@swc/core: npm ERR! node: not found"
→ Repite el paso anterior y ejecuta `npm install` nuevamente.

### Error: "Module not found: LoginService"
→ Ejecuta `npm install` dentro de la carpeta `AuditoriaRiesgos/`.

### Error: "Flask: Address already in use"
→ Cambia el puerto en `app.py`:
```python
app.run(debug=True, port=5501)  # O cualquier puerto disponible
```

### Error: "Ollama: Connection refused"
→ Asegúrate de que Ollama está corriendo:
```powershell
ollama serve
```

### Error: "ModuleNotFoundError: No module named 'openai'"
→ Dentro del venv, ejecuta:
```powershell
pip install openai flask
```

---

## 11. Comandos Útiles

```powershell
# Desarrollo frontend
npm run dev          # Inicia servidor dev en localhost:5173

# Compilación
npm run build        # Compila para producción → dist/

# Linting (si lo necesitas)
npm run lint         # Verifica código

# Backend
python app.py        # Inicia Flask en localhost:5500

# Ollama
ollama serve         # Inicia servidor Ollama
ollama list          # Lista modelos disponibles
```

---

## 12. Verificación Final

1. ✅ Ollama corriendo en `http://localhost:11434/v1`
2. ✅ Backend Flask corriendo en `http://127.0.0.1:5500`
3. ✅ Frontend Vite corriendo en `http://localhost:5173` (desarrollo)
4. ✅ `npm run build` ejecutado (para producción)
5. ✅ Credenciales de login: `admin` / `123456`
6. ✅ Modelos Ollama descargados: `ollama list`

Si todo está verde, ¡tu proyecto está listo! 🚀

---

**Creado:** 22/04/2026  
**Última actualización:** Configuración completa para Windows + portabilidad a otra PC
