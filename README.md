# BTShutter

BTShutter es una solución de hardware/software que convierte cualquier ordenador Linux en un disparador remoto de latencia cero para cámaras Android vía Bluetooth. Diseñado para automatizar capturas, grabaciones sincronizadas o controlar el teléfono sin separarse del teclado.

## Características
*   **Latencia Cero:** Conexión persistente de lectura de un solo byte. Evita retardos por apertura/cierre de canales.
*   **Integración Transparente:** Utiliza los protocolos Bluetooth nativos del sistema. No requiere conexión a internet.
*   **Canal Dinámico:** El script descubre automáticamente el puerto RFCOMM asignado por Android mediante SDP.
*   **Calibración Visual:** Widget flotante para ajustar el punto exacto de disparo sobre la interfaz de cualquier aplicación de cámara.

## Requisitos Previos
*   **Android:** Dispositivo con Android 7.0 (API 24) o superior.
*   **Escritorio:** Entorno Linux (X11) con adaptador Bluetooth, paquete `bluez` y Python 3 del sistema (los entornos virtuales bloquean el acceso a sockets Bluetooth).

## Instalación

### 1. Configuración en Android
1. Descarga el archivo `BTShutter.apk` desde [Releases](../../releases) e instálalo.
2. Abre la aplicación y concede los permisos de conexión Bluetooth si el sistema lo solicita.
3. Verifica el botón **Accesibilidad**. Si indica "Revisar", púlsalo para abrir los ajustes de accesibilidad de tu dispositivo y activa el servicio **BTShutter**.
   * *Nota para Android 13+:* Si el interruptor de accesibilidad aparece bloqueado en gris, ve a los Ajustes de tu teléfono > Aplicaciones > BTShutter > pulsa el menú de tres puntos (o baja hasta el final) y selecciona **Permitir ajustes restringidos**.
4. Para garantizar su funcionamiento en segundo plano, desactiva la optimización de batería para BTShutter en los ajustes del sistema.

### 2. Configuración en Linux
Asegúrate de que el PC y el móvil están emparejados y marcados como de confianza (Trusted) en BlueZ.

Instala la librería de captura de teclado a nivel de sistema operativo:
```bash
sudo apt install python3-pynput
```
*(Alternativa si no usas APT: `pip install pynput --break-system-packages`)*

Descarga el ejecutable `bt_shutter.py` y edita la línea de configuración para introducir la dirección MAC de tu dispositivo Android:
```python
MAC_ANDROID = "XX:XX:XX:XX:XX:XX"
```

## Uso

### Calibración Inicial
*Solo es necesario realizar este paso una vez, a menos que cambies de aplicación de cámara.*
1. Abre BTShutter y pulsa **Calibrar**. (Concede el permiso de superposición de pantalla si el sistema lo solicita).
2. La aplicación se minimizará y aparecerá un retículo rojo flotante.
3. Abre tu aplicación de cámara.
4. Arrastra el retículo y sitúalo exactamente sobre el botón del obturador de la cámara.
5. Pulsa el botón **Guardar** situado en la esquina superior derecha de la pantalla.

### Disparo Remoto
1. Abre BTShutter y pulsa **Iniciar**. El botón cambiará a color azul indicando que el demonio está activo.
2. Ejecuta el demonio en Linux:
```bash
python3 bt_shutter.py
```
3. El script localizará el servicio en el dispositivo y establecerá un túnel continuo.
4. Con tu aplicación de cámara abierta en Android, presiona la **barra espaciadora** en el PC para capturar fotografías de forma inmediata.

## Detención
Para finalizar el proceso, abre BTShutter y pulsa **Detener**, lo que cerrará los sockets y liberará los recursos.
