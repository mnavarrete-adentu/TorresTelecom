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

## 📖 Explicación Detallada de Cada Paso

### **00. Descargar imágenes de CVAT**

- **Propósito**: Descarga las imágenes y datos de etiquetado desde CVAT
- **Solicita**:
  - ID de Levantamiento (levID)
  - ID de Medición (medID)
  - Nombre de la tarea (task_name)
- **Proceso**: Descarga automáticamente las imágenes, etiquetas y metadatos desde CVAT
- **Resultado**: Crea la estructura de carpetas en `torres/{task_name}/`

### **0. Pre-Proceso**

- **Propósito**: Realiza el pre-procesamiento inicial de las imágenes
- **Solicita**:
  - ID de Levantamiento (levID)
  - ID de Medición (medID)
  - Nombre de la tarea (task_name)
- **Proceso**:
  - Corrige distorsiones de las imágenes
  - Genera imágenes cenitales
  - Prepara archivos de reporte iniciales
- **Resultado**: Imágenes procesadas y archivos de reporte base

### **1. Calcular Azimuth antenas**

- **Propósito**: Calcula el ángulo de azimuth de las antenas
- **Solicita**:
  - ID de Levantamiento (levID)
  - ID de Medición (medID)
  - Nombre de la tarea (task_name)
  - **Opcional**: ¿Calcular una antena específica? (y/N)
    - Si "y": Ingresar ID de la antena específica
- **Proceso**:
  - Utiliza la imagen cenital como referencia
  - Calcula el ángulo basado en la posición de la antena
  - Guarda coordenadas (Pointx, Pointy) y ángulo (Azimuth)
- **Resultado**: Actualiza el reporte con los valores de azimuth calculados

### **2. Calcular Ancho antenas**

- **Propósito**: Calcula el ancho de las antenas
- **Solicita**:
  - ID de Levantamiento (levID)
  - ID de Medición (medID)
  - Nombre de la tarea (task_name)
  - **Opcional**: ¿Calcular una antena específica? (y/N)
    - Si "y":
      - ID de la antena específica
      - **Opcional**: ¿Usar medida de referencia específica? (y/N)
        - Si "y": Ingresar medida de referencia en cm
- **Proceso**:
  - Utiliza la imagen cenital para calcular pixeles por cm
  - Mide el ancho de la antena en la imagen
  - Convierte a centímetros
- **Resultado**: Actualiza el reporte con el ancho calculado (en metros)

### **3. Calcular Alto antenas**

- **Propósito**: Calcula la altura de las antenas
- **Solicita**:
  - ID de Levantamiento (levID)
  - ID de Medición (medID)
  - Nombre de la tarea (task_name)
  - **Opcional**: ¿Calcular una antena específica? (y/N)
    - Si "y": ID de la antena específica
- **Proceso**:
  - Utiliza el ancho previamente calculado como referencia
  - Mide la altura de la antena en la imagen
  - Convierte a centímetros
- **Resultado**: Actualiza el reporte con la altura calculada (en metros)

### **4. Calcular Altura en Torre**

- **Propósito**: Calcula la altura de las antenas respecto a la torre
- **Solicita**:
  - ID de Levantamiento (levID)
  - ID de Medición (medID)
  - Nombre de la tarea (task_name)
  - **Altura de referencia en cm**: Valor numérico
  - **Opcional**: ¿Calcular una antena específica? (y/N)
    - Si "y":
      - ID de la antena específica
      - **Método de cálculo**:
        1. Alto de la antena (usa altura previamente calculada)
        2. Imagen General de la torre (permite seleccionar imagen)
- **Proceso**:
  - Calcula la posición vertical de la antena en la torre
  - Determina altura inicial, centro y final de la antena
- **Resultado**: Actualiza el reporte con H_inicial, H_centro, H_final (en metros)

### **5. Actualizar reporte desde excel**

- **Propósito**: Actualiza el reporte JSON desde un archivo Excel
- **Solicita**:
  - ID de Levantamiento (levID)
  - ID de Medición (medID)
  - Nombre de la tarea (task_name)
- **Proceso**:
  - Lee datos desde archivo Excel
  - Actualiza el archivo `reporte.json`
- **Resultado**: Reporte actualizado con datos del Excel

### **6. Subir reporte a S3**

- **Propósito**: Sube el reporte final a AWS S3
- **Solicita**:
  - ID de Levantamiento (levID)
  - ID de Medición (medID)
  - Nombre de la tarea (task_name)
  - **Opcional**: ¿Subir imágenes de baja calidad? (y/N)
- **Proceso**:
  - Sube el archivo de reporte a S3
  - Opcionalmente sube imágenes de baja calidad
  - Crea archivo de control `uploaded.txt`
- **Resultado**: Reporte disponible en S3

### **7. Subir Imágenes de baja calidad**

- **Propósito**: Sube imágenes de baja calidad a S3
- **Solicita**:
  - ID de Levantamiento (levID)
  - ID de Medición (medID)
  - Nombre de la tarea (task_name)
- **Proceso**:
  - Detecta automáticamente si hay archivo `train.txt`
  - Sube imágenes desde CVAT o S3 según corresponda
- **Resultado**: Imágenes de baja calidad disponibles en S3

### **8. Calcular Caras Torre**

- **Propósito**: Calcula y asigna las caras de la torre a cada antena
- **Solicita**:
  - ID de Levantamiento (levID)
  - ID de Medición (medID)
  - Nombre de la tarea (task_name)
  - **Cantidad de caras**: Número de caras a calcular (típicamente 4)
- **Proceso**:
  - Divide la torre en caras (A, B, C, D)
  - Calcula los ángulos de cada cara
  - Asigna cada antena a su cara correspondiente
  - Genera imagen `cenital_view_caras.jpg`
  - Crea archivo `caras.json`
- **Resultado**: Cada antena tiene asignada su cara de torre

### **9. Borrar archivos locales**

- **Propósito**: Elimina archivos locales para liberar espacio
- **Solicita**:
  - ID de Levantamiento (levID)
  - ID de Medición (medID)
  - Nombre de la tarea (task_name)
  - **Confirmación**: ¿Estás seguro? (y/N)
  - **Si no hay uploaded.txt**: Confirmación adicional de seguridad
- **Proceso**:
  - Verifica si el reporte fue subido a S3
  - Elimina carpeta `torres/{task_name}/`
  - Elimina archivo `{task_name}.zip`
- **Resultado**: Archivos locales eliminados

### **g. Generar imágenes por cara** (opción oculta)

- **Propósito**: Organiza las imágenes por cara de torre
- **Solicita**:
  - ID de Levantamiento (levID)
  - ID de Medición (medID)
  - Nombre de la tarea (task_name)
- **Proceso**:
  - Crea carpetas A, B, C, D
  - Copia imágenes con etiquetas según su cara asignada
- **Resultado**: Imágenes organizadas por cara de torre

### **x. Salir del programa**

- **Propósito**: Termina la ejecución del programa
- **Proceso**: Sale del bucle principal
- **Resultado**: Programa terminado

## 🔄 Flujo de Trabajo Típico

1. **Descargar datos de CVAT** (opción 00)
2. **Pre-procesar imágenes** (opción 0)
3. **Calcular parámetros** (opciones 1-4, 8)
4. **Actualizar reporte** (opción 5)
5. **Subir resultados** (opciones 6-7)
6. **Limpiar archivos** (opción 9)

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

- `Guia de Procesamiento 2025-04.pdf`: Guía detallada del proceso. (Actualizada el 04/2025)
- `Guia de Procesamiento.docx`: Documentación en formato Word
