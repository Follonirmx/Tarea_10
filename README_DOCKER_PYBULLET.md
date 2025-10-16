
# 🐍 Simulaciones en PyBullet con Docker

Este proyecto contiene tres simulaciones desarrolladas con **PyBullet**, organizadas en carpetas separadas:

```
pybullet/
├── bipedo/
│   ├── main.py
│   ├── Dockerfile
│   └── biped/
│
├── brazo/
│   ├── main.py
│   ├── Dockerfile
│   └── models/
│
└── carro/
    ├── main.py
    ├── Dockerfile
    └── assets/
```

Cada simulación tiene su propio contenedor Docker independiente.

---

## 🚀 1. Objetivo de la Dockerización

El objetivo es **aislar cada simulación** dentro de un contenedor, de forma que:
- No dependas de versiones de Python o librerías instaladas en tu sistema.
- Puedas ejecutar la simulación en cualquier PC con Docker.
- Se mantenga la compatibilidad con **entornos gráficos (GUI)** de PyBullet.

---

## 🧩 2. Estructura de archivos

Cada carpeta (`bipedo`, `brazo`, `carro`) contiene:
- `main.py`: el script principal de la simulación.  
- `Dockerfile`: define cómo construir el entorno de ejecución.  
- `README.md`: instrucciones específicas del modelo.

---

## ⚙️ 3. Contenido del Dockerfile

Cada `Dockerfile` tiene esta estructura básica:

```dockerfile
# Imagen base de Python
FROM python:3.10-slim

# Instalar dependencias del sistema necesarias para PyBullet con GUI
RUN apt-get update && apt-get install -y     xvfb     python3-tk     libglu1-mesa     libgl1-mesa-glx     && rm -rf /var/lib/apt/lists/*

# Crear directorio de trabajo
WORKDIR /app

# Copiar los archivos del proyecto
COPY . /app

# Instalar PyBullet
RUN pip install pybullet

# Comando por defecto
CMD ["python", "main.py"]
```

---

## 🧱 4. Construcción de la imagen

Desde el directorio de cada proyecto, ejecuta:

```bash
docker build -t pybullet-bipedo .
```

o para los otros proyectos:

```bash
docker build -t pybullet-brazo .
docker build -t pybullet-carro .
```

---

## ▶️ 5. Ejecución del contenedor (con GUI)

Para permitir que PyBullet muestre su ventana gráfica (interfaz 3D), habilita el acceso a X11:

```bash
xhost +local:docker
```

Luego ejecuta el contenedor:

```bash
docker run -it --rm   -e DISPLAY=$DISPLAY   -v /tmp/.X11-unix:/tmp/.X11-unix   pybullet-bipedo
```

Reemplaza `pybullet-bipedo` por `pybullet-brazo` o `pybullet-carro` según el caso.

---

## 🧹 6. Limpieza

Para borrar las imágenes cuando ya no las necesites:

```bash
docker rmi pybullet-bipedo pybullet-brazo pybullet-carro
```

---

## 🧠 7. Conceptos clave

| Concepto | Explicación |
|-----------|-------------|
| **Dockerfile** | Archivo que define cómo construir el entorno de ejecución del proyecto. |
| **Imagen** | Plantilla que incluye todo lo necesario (Python, PyBullet, dependencias). |
| **Contenedor** | Instancia en ejecución de una imagen (como si fuera un mini sistema operativo). |
| **Volumen X11** | Permite mostrar gráficos desde el contenedor hacia tu pantalla local. |
| **`--rm`** | Elimina automáticamente el contenedor al salir. |

---

## ✅ 8. Ejemplo completo (Bípedo)

### Construir imagen:
```bash
cd bipedo
docker build -t pybullet-bipedo .
```

### Ejecutar simulación:
```bash
xhost +local:docker
docker run -it --rm   -e DISPLAY=$DISPLAY   -v /tmp/.X11-unix:/tmp/.X11-unix   pybullet-bipedo
```

### Resultado:
Aparecerá la interfaz gráfica de PyBullet mostrando el modelo bípedo.

---

## 📦 9. Ventajas de usar Docker

- No requiere instalar Python ni dependencias en el sistema.
- Aislamiento total de librerías.
- Portabilidad (funciona en Linux, Windows o macOS).
- Reproducibilidad: todos ejecutan la misma versión del entorno.

---

---

**Autor:** _Jonathan David Díaz Vargas_  
 
**Año:** 2025
