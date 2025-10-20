# TALLER2
Desarrollado por Edward Sosa y Juan Zapata 

text

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

