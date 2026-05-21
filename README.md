# robobash
código de robot autónomo 
¡Manos a la obra! Como tu ingeniera "Robotica", aquí tienes la estructura base del código en Python para el "cerebro" de tu Raspberry Pi.
Este pseudocódigo traduce toda la lógica táctica y matemática que definimos previamente a una estructura real que puedes ejecutar en tu placa.
### 1. Definición de Estados y Roles
Para definir los estados de la FSM de forma estructurada y evitar errores usando "strings" sueltos, usaremos la clase Enum nativa de Python.
```python
from enum import Enum
import math
import socket
import threading

class Estado(Enum):
    BUSCAR_BALON = 1
    IR_A_BALON = 2
    CONTROL_BALON = 3
    ATACAR = 4
    RECUPERAR = 5
    DEFENDER = 6

class Rol(Enum):
    ATACANTE = 1
    DEFENSOR = 2

```
### 2. Módulo de Lógica Dinámica (Matemática)
Aquí aplicamos la fórmula de la distancia euclidiana para evaluar las coordenadas en tiempo real. La regla de software exige que el robot más cercano asuma el ataque y el más lejano la defensa.
```python
def calcular_distancia(x_robot, y_robot, x_balon, y_balon):
    # Retorna la distancia euclidiana exacta
    return math.sqrt((x_balon - x_robot)**2 + (y_balon - y_robot)**2)

def evaluar_rol(distancia_propia, distancia_companero):
    # Comparación en tiempo real para asignar el comportamiento
    if distancia_propia < distancia_companero:
        return Rol.ATACANTE
    else:
        return Rol.DEFENSOR

```
### 3. Módulo de Sincronización (UDP)
Debes implementar comunicación UDP local para que la Raspberry envíe y reciba constantemente la posición del balón. Esto idealmente corre en un hilo secundario (Thread) para no pausar las decisiones del robot.
```python
def hilo_comunicacion_udp():
    # Inicializar socket UDP
    # udp_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    
    while True:
        # 1. Enviar 'distancia_propia' y 'rol_actual' a la IP del robot compañero
        # 2. Escuchar el puerto y recibir 'distancia_companero'
        pass

```
### 4. Bucle Principal de la FSM (Toma de Decisiones)
Este es el ciclo maestro que evalúa el estado actual, lee los sensores y decide si debe transitar a otro comportamiento. Se estructura mediante un bloque condicional if/elif.
```python
def tick_fsm():
    estado_actual = Estado.BUSCAR_BALON
    rol_actual = Rol.DEFENSOR
    
    while True:
        # 1. Módulo de Percepción (Lectura de sensores)
        # x_balon, y_balon = leer_esp32_cam()
        # distancia_propia = calcular_distancia(x_robot, y_robot, x_balon, y_balon)
        # rol_actual = evaluar_rol(distancia_propia, distancia_companero)
        
        # 2. Transiciones de la Máquina de Estados
        if estado_actual == Estado.BUSCAR_BALON:
            # Salida: Girar sobre su eje
            # Transición: Si la cámara detecta el centroide naranja -> IR_A_BALON
            pass
            
        elif estado_actual == Estado.IR_A_BALON:
            # Salida: Avanzar hacia x_balon, y_balon
            # Transición: Si los ultrasónicos frontales leen < 5 cm -> CONTROL_BALON
            pass
            
        elif estado_actual == Estado.CONTROL_BALON:
            # Salida: Activar rodillo de captura
            # Transición: Si está alineado con la portería rival -> ATACAR
            pass
            
        elif estado_actual == Estado.ATACAR:
            # Salida: Avanzar a máxima velocidad esquivando obstáculos
            # Transición: Si se pierde el balón repentinamente -> RECUPERAR
            pass
            
        elif estado_actual == Estado.DEFENDER:
            # Salida exclusiva para Rol.DEFENSOR
            # Transición: Posicionarse en la coordenada entre el balón y portería propia
            pass
            
        # 3. Salida Cinematográfica
        # enviar_velocidades_ESP32(Vx, Vy, W)

```

