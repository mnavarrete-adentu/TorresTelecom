# Procesamiento de Antenas de Telecomunicaciones

Este programa permite procesar imágenes de antenas de telecomunicaciones para calcular parámetros como azimuth, ancho, alto, altura en torre y caras de torre. Incluye funcionalidades para descargar datos desde CVAT, procesar imágenes y subir reportes a AWS S3.

## 📋 Requisitos Previos

- Python 3.8 o superior
- Git
- Acceso a CVAT (para descarga de datos)
- Credenciales de AWS (para subida de reportes)

## 🚀 Instalación

### 1. Clonar el Repositorio

```bash
git clone https://github.com/mnavarrete-adentu/TorresTelecom.git
cd TorresTelecom
```

### 2. Crear Entorno Virtual

#### En Windows:

```bash
# Crear el entorno virtual
python -m venv venv

# Activar el entorno virtual
venv\Scripts\activate
```

#### En Linux/macOS:

```bash
# Crear el entorno virtual
python3 -m venv venv

# Activar el entorno virtual
source venv/bin/activate
```

### 3. Instalar Dependencias

```bash
pip install -r requirements.txt
```

**Nota:** Si hay problemas con algunas dependencias, instálalas manualmente:

```bash
pip install opencv-python
pip install matplotlib
pip install tqdm
pip install boto3
pip install python-dotenv
pip install cvat-sdk
pip install Pillow
pip install lensfunpy
pip install xlsxwriter
```

## ⚙️ Configuración

### 1. Crear Archivo de Variables de Entorno

Crea un archivo `.env` en la raíz del proyecto con las siguientes variables:

```env
# Configuración AWS
AWS_ACCESS_KEY_ID=tu_access_key_id
AWS_SECRET_ACCESS_KEY=tu_secret_access_key
AWS_DEFAULT_REGION=tu_region
AWS_BUCKET=tu_bucket_name

# Configuración CVAT
CVAT_HOST = 'http://3.225.205.173:8080'
CVAT_USERNAME = 'usuario_cvat'
CVAT_PASSWORD = 'contraseña_cvat'
```

Pedir credenciales de AWS y de CVAT a developer de Adentu.

### 2. Configurar Permisos de Ejecución (Linux/macOS)

```bash
chmod +x utils/exiftool
```

## 🎯 Uso del Programa

### Ejecutar el Programa Principal

```bash
python main.py
```

### Menú de Opciones

El programa presenta un menú interactivo con las siguientes opciones:

- **00**: Descargar imágenes de CVAT
- **0**: Pre-Proceso
- **1**: Calcular Azimuth antenas
- **2**: Calcular Ancho antenas
- **3**: Calcular Alto antenas
- **4**: Calcular Altura en Torre
- **5**: Actualizar reporte desde excel
- **6**: Subir reporte a S3
- **7**: Subir Imágenes de baja calidad
- **8**: Calcular Caras Torre
- **9**: Borrar archivos locales
- **x**: Salir del programa

### Flujo de Trabajo Típico

1. **Descargar datos de CVAT** (opción 00)
2. **Pre-procesar imágenes** (opción 0)
3. **Calcular parámetros** (opciones 1-4, 8)
4. **Actualizar reporte** (opción 5)
5. **Subir resultados** (opciones 6-7)

## 📁 Estructura del Proyecto

```
TorresTelecom/
├── main.py                 # Programa principal
├── requirements.txt        # Dependencias de Python
├── .env                   # Variables de entorno (crear)
├── .gitignore            # Archivos ignorados por Git
├── README.md             # Este archivo
├── utils/                # Utilidades del programa
│   ├── functions.py      # Funciones principales
│   ├── downloadCvat.py  # Descarga desde CVAT
│   ├── preProceso.py    # Pre-procesamiento
│   ├── metadata.py      # Manejo de metadatos
│   ├── saveGeoMatriz.py # Guardado de datos geoespaciales
│   ├── rename.py        # Renombrado de archivos
│   └── exiftool         # Herramienta para metadatos
├── torres/              # Directorio de trabajo (se crea automáticamente)
└── Guia de Procesamiento/ # Documentación adicional
```

## 🔧 Solución de Problemas

### Error de Dependencias

Si hay problemas con las dependencias, intenta:

```bash
pip install --upgrade pip
pip install -r requirements.txt --force-reinstall
```

### Error de Permisos en Linux/macOS

```bash
chmod +x utils/exiftool
```

### Error de Configuración AWS

Verifica que las credenciales en el archivo `.env` sean correctas y tengan los permisos necesarios.

### Error de Conexión CVAT

Verifica que la URL y credenciales de CVAT sean correctas.

## 📚 Documentación Adicional

- `Guia de Procesamiento 2025-04.pdf`: Guía detallada del proceso
- `Guia de Procesamiento.docx`: Documentación en formato Word
