# TALLER2
Desarrollado por Edward Sosa y Juan Zapata 
Este README incluye:

✅ **Todos los pasos que seguimos** desde la instalación hasta la ejecución exitosa  
✅ **Comandos listos para copiar y pegar**  
✅ **Solución de errores comunes** que enfrentamos  
✅ **Explicaciones claras** de cada componente  
✅ **Formato profesional** con emojis y secciones organizadas  
✅ **Instrucciones para contribuciones y próximos pasos**

# 🚀 Laboratorio - Despliegue de Drones con PyBullet en WSL

## 📋 Descripción
Este proyecto implementa el despliegue del entorno de simulación de drones [gym-pybullet-drones](https://github.com/utiasDSL/gym-pybullet-drones) en WSL2 con Windows 11, utilizando PyBullet para la simulación física y renderizado 3D.

## 🛠️ Configuración del Entorno

### Prerrequisitos
- Windows 11 con WSL2 instalado y actualizado
- Python 3.10+ en WSL
- Entorno gráfico WSLg activado

### Paso 1: Verificación del Entorno Gráfico
```bash
# Actualizar WSL desde PowerShell (como administrador)
wsl --update
wsl --shutdown

# Reiniciar el sistema y verificar WSLg
echo $DISPLAY
echo $WAYLAND_DISPLAY
```

# Paso 2: Prueba Básica de PyBullet
``` bash
# Crear entorno de prueba
mkdir ~/pybullet-test && cd ~/pybullet-test
python3 -m venv venv
source venv/bin/activate
pip install pybullet

# Crear script de prueba
cat > test_pybullet.py << 'EOF'
import pybullet as p
import pybullet_data
import time

p.connect(p.GUI)
p.setAdditionalSearchPath(pybullet_data.getDataPath())
p.setGravity(0, 0, -9.8)
p.loadURDF("plane.urdf")
cube = p.loadURDF("r2d2.urdf", [0, 0, 1])

while True:
    p.stepSimulation()
    time.sleep(1.0 / 240.0)
EOF

# Ejecutar prueba
python3 test_pybullet.py
```

# Paso 3: Configuración del Proyecto de Drones
```bash
# Clonar repositorio
cd ~
git clone https://github.com/utiasDSL/gym-pybullet-drones.git
cd gym-pybullet-drones


# Crear entorno virtual
python3 -m venv venv
source venv/bin/activate

# Instalar dependencias
pip install --upgrade pip
pip install gymnasium pybullet stable-baselines3 matplotlib opencv-python pandas numpy

# Instalar el paquete en modo editable
pip install -e .
```
# Paso 4: Ejecución de Simulaciones de Drones
```bash
# Navegar a ejemplos
cd gym_pybullet_drones/examples

# Ejecutar controlador PID con GUI
python3 pid.py --gui True

# Otros ejemplos disponibles
python3 learn.py --gui True     
python3 downwash.py --gui True   
python3 play.py --gui True
```
   
# 🎮 Scripts Disponibles
Control Básico (pid.py)
Controlador PID para estabilización de drones

Simulación de vuelo autónomo

Parámetros ajustables de control

Entrenamiento RL (learn.py)
Algoritmos de aprendizaje por refuerzo

Entrenamiento de políticas de vuelo

Logs de progreso y rewards

Demostraciones (downwash.py, play.py)
Efectos aerodinámicos entre drones

Control manual interactivo

Visualización de trayectorias

# 🔧 Solución de Problemas Comunes
Error: "Cannot connect to X server"
bash
# Verificar display
```bash
echo $DISPLAY
export DISPLAY=:0
```
# Si usa VcXsrv externo
export DISPLAY=$(grep nameserver /etc/resolv.conf | awk '{print $2}'):0
Error: Módulos no encontrados
```bash
# Reinstalar en modo editable desde la raíz
cd ~/gym-pybullet-drones
pip install -e .
```


# 📊 Resultados Esperados
Simulación Exitosa
Ventana de PyBullet con entorno 3D

Dron(s) renderizados correctamente

Física realista de vuelo

Controles responsivos

![Imagen de WhatsApp 2025-10-20 a las 18 25 19_157a41bb](https://github.com/user-attachments/assets/84a98d00-cec9-460c-8b70-8267e9c57312)

# 🧩 Paso a paso de la instalación y despliegue

# 🧱 1. Creación del entorno virtual e instalación de dependencias

Primero se creó y activó un entorno virtual en Python para aislar las dependencias del proyecto. Luego se instalaron los paquetes necesarios para la simulación:

pip install gymnasium pybullet stable-baselines3 matplotlib opencv-python pandas numpy



![Imagen de WhatsApp 2025-10-20 a las 18 41 15_302b9f28](https://github.com/user-attachments/assets/2513e6c4-4288-456a-9ba4-1254556a6697)


⚙️ 2. Creación del Dockerfile

Se elaboró un archivo Dockerfile que contiene las instrucciones para construir la imagen de Docker con todas las dependencias necesarias.
Este archivo realiza las siguientes acciones:

Instala dependencias del sistema.

Clona el repositorio de pybullet_robots.

Instala PyBullet.

Define el comando por defecto para ejecutar la demo del robot Baxter.

FROM python:3.10-slim

# Instalar dependencias del sistema
RUN apt-get update && apt-get install -y \
    git xvfb python3-opengl \
    && rm -rf /var/lib/apt/lists/*

# Clonar el repositorio de PyBullet Robots
RUN git clone https://github.com/erwincoumans/pybullet_robots.git

WORKDIR /pybullet_robots

# Instalar PyBullet
RUN pip install pybullet

# Comando por defecto: ejecutar la demo de Baxter
CMD ["python3", "baxter_ik_demo.py"]


![Imagen de WhatsApp 2025-10-20 a las 18 43 52_02452e04](https://github.com/user-attachments/assets/a21bd882-4350-4d4a-bf9d-c3f592bab225)


🧰 3. Construcción de la imagen de Docker

Con el Dockerfile listo, se construyó la imagen de Docker ejecutando:

docker build -t baxter_pybullet .


Este comando descargó las dependencias, instaló los paquetes y preparó la imagen llamada baxter_pybullet.

![Imagen de WhatsApp 2025-10-20 a las 18 45 49_02f4eb35](https://github.com/user-attachments/assets/95675fa2-be47-468d-9d85-e62769ec32c8)


🧪 4. Verificación del entorno Docker

Para comprobar que el contenedor funcionaba correctamente sin entorno gráfico, se probó la conexión directa con PyBullet:

docker run -it baxter_pybullet python3 -c "import pybullet as p; cid = p.connect(p.DIRECT); print('🔗 Conexión establecida con ID:', cid)"


El resultado fue exitoso, mostrando la conexión directa establecida con el entorno PyBullet.

![Imagen de WhatsApp 2025-10-20 a las 18 53 03_d3f5c7f1](https://github.com/user-attachments/assets/8aeca4c4-6d5f-4e46-8072-3bdbc9e89ce4)


🤖 5. Ejecución del simulador 3D

Finalmente, se ejecutó el script principal para visualizar el robot Baxter en el entorno 3D:

python3 baxter_ik_demo.py --nogui


Esto permitió abrir la interfaz gráfica donde se muestra el modelo del robot, sus cámaras virtuales y parámetros de control.

![Imagen de WhatsApp 2025-10-20 a las 19 20 40_05dd5563](https://github.com/user-attachments/assets/0d3467b3-f6f8-492e-8173-ea0a6e6be80c)

# 🤖 Despliegue del Robot ATLAS en PyBullet usando Docker

🚀 Proceso de Despliegue

# 1️⃣ Configuración del Entorno Virtual e Instalación de Dependencias

![Imagen de WhatsApp 2025-10-20 a las 19 28 27_2745c532](https://github.com/user-attachments/assets/d08ad7ca-8e7b-4c27-80e1-259997bb28d8)


´´´bash
# Crear entorno virtual e instalar dependencias
python3 -m venv venv
source venv/bin/activate
pip install gymnasium pybullet stable-baselines3 matplotlib opencv-python pandas numpy
´´´

# 2️⃣ Creación del Dockerfile y Construcción de la Imagen
´´´bash
![Imagen de WhatsApp 2025-10-20 a las 19 36 24_89d3a297](https://github.com/user-attachments/assets/ae35f5d3-172c-4457-8ead-5aee9246267b)


Dockerfile:
´´´bash
dockerfile
FROM python:3.10-slim
RUN apt-get update && apt-get install -y git xvfb python3-opengl && rm -rf /var/lib/apt/lists/*
RUN git clone https://github.com/erwincoumans/pybullet_robots.git
WORKDIR /pybullet_robots
RUN pip install pybullet numpy
CMD ["python3", "atlas.py"]
´´´
Construcción de la imagen:
´´´bash
docker build -t atlas_pybullet .
´´´
# 3️⃣ Ejecución del Contenedor y Prueba de Simulación

![Imagen de WhatsApp 2025-10-20 a las 19 36 35_5bbf0a11](https://github.com/user-attachments/assets/fb70cb3a-ad06-4ea4-9f4e-80ba5afc0e95)

Ejecutar en modo headless:

´´´bash
docker run -it atlas_pybullet python3 atlas.py --nogui
´´´
# 4️⃣ Confirmación del Despliegue Exitoso

![Imagen de WhatsApp 2025-10-20 a las 19 38 22_638b00e1](https://github.com/user-attachments/assets/d293edc1-0dc8-4ebe-810e-c59386e3838e)


Verificar conexión con PyBullet:

´´´bash
docker run -it atlas_pybullet python3 -c "import pybullet as p; cid=p.connect(p.DIRECT); print('✅ PyBullet dentro del contenedor, conexión ID:', cid)"
´´´
✅ Confirmación del Despliegue Exitoso

# Salida esperada en terminal:
pybullet build time: Jan 29 2025 23:16:28
✅ PyBullet dentro del contenedor, conexión ID: 0
🎉 Estado Final del Despliegue
El robot ATLAS ha sido desplegado exitosamente en un contenedor Docker con PyBullet. La configuración incluye:


# 🛠️ Comandos Adicionales Útiles
´´´bash
# Ejecutar con interfaz gráfica (requiere X11 forwarding)
docker run -it -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix atlas_pybullet

# Inspeccionar la imagen creada
docker images | grep atlas_pybullet

# Ejecutar shell interactiva en el contenedor
docker run -it atlas_pybullet /bin/bash
´´´
