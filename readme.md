# **ELUNM Networking**

Es un patch desarrollado en el lenguaje de programación Pure Data ([http://msp.ucsd.edu/software.html](http://msp.ucsd.edu/software.html)) para el ELUNM (Ensamble de Laptops de la Universidad Nacional de Música) en Lima, Perú.

Programado para el envío de mensajes a través de una red local y/o Internet desde múltiples usuarios (clientes) hacia un usuario principal (servidor) mediante la interfaz *netreceive/netsend* propia en Pure Data. Los mensajes son recibidos y descifrados por el patch servidor para activar funciones preprogramadas relacionadas a la apertura de carpetas y archivos (samples de audio) para su reproducción y procesamiento en tiempo real.

---

Descargar la última versión en [https://github.com/jlquispec/elunm-networking/releases](https://github.com/jlquispec/elunm-networking/releases).

## Versión 1.0

Necesita Pd **0.51** o superior.

- Disposición de 5 canales/instancias para el procesamiento en tiempo real (lectura de samples mono y stereo, reproducción, espacialización stereo y aplicación de efectos).
- Soporte para 10 usuarios conectados hacia el patch servidor con las denominaciones user0, user1, user2,... y user9.
- Se permite la recepción de mensajes por usuario en un intervalo mínimo de 100ms para asegurar el correcto funcionamiento.
- Soporte para renderización en formato WAV estéreo de la salida principal.

## Preparación

Se necesitarán las siguientes librerías externas, las cuales deberán ser instaladas en su última versión a través del complemento Deken integrado dentro de Pure Data.

- else (objetos: fbdelay~, pitch.shift~, rm~, dir, pic).
- zexy (objetos: z~, limiter~).
- freeverb (objeto: freeverb~).

Nota: Mapear la librería zexy en el startup de Pure Data.

## Manual

Para uso local (en la misma computadora):

- Abrir el patch servidor: **main_elunm-networking-server.pd** .
- Configurar el puerto a través el cual se recepcionarán los mensajes (por ejemplo: 13000) y permitir la escucha de mensajes a través del **toggle Listen**. Ya se encuentra listo la operación.
- Abrir el patch cliente:  **main_elunm-networking-client.pd** en una instancia nueva a través del toggle **Load Client** del patch servidor.
- Configurar la dirección de IP de destino en 127.0.0.1 (localhost) y el puerto a través del cual se realizará la comunicación (debe ser el mismo usado en el patch servidor).
- Configurar el usuario para identificar a través del cual se realizará la comunicación (user0, user1, user2,... o user9).
- Activar el envío de mensajes usando el **toggle Connect**, esperar 1 segundo aproximadamente para asegurar que la conexión se realizó satisfactoriamente. Ya se encuentra listo para la operación.

Para uso a través de LAN (red local) e Internet:

- Servidor:
    - Abrir el patch servidor: **main_elunm-networking-server.pd**.
    - Configurar el puerto a través el cual se recepcionarán los mensajes; por ejemplo: 13000.
    - Debe proporcionar a los usuarios cliente su dirección IP, en caso que la comunicación se realice a través de una red local (LAN) obtenerlo dentro de las configuraciones de red de la computadora que se usa (Para Windows usar el comando ***ipconfig***  a través del Símbolo del Sistema)
- En caso la comunicación se realice vía internet, se debe proporcionar a los usuarios cliente la dirección IP pública, se puede obtener a través de  [https://www.monip.org/](https://www.monip.org/) o páginas similares. Considerar que previamente el usuario servidor debe haber preparado la configuración de su router para el redireccionamiento de mensajes (Port Forwarding) a su dirección IP privada para el puerto determinado (el mismo que se configura en los patches).
	- Activar la escucha de mensajes usando el **toggle Listen**.
- Cliente:
    - Abrir el patch cliente: **main_elunm-networking-client.pd**.
    - Configurar la dirección de IP de destino (proporcionado por el usuario que opera el patch servidor) y el puerto por el cual se realizará la comunicación.
    - Configurar el usuario para identificar a través de quién se realizará la comunicación (user0, user1, user2,... o user9).
    - Activar el envío de mensajes usando el **toggle Connect**, esperar para asegurar que la conexión se realizó satisfactoriamente. Ya se encuentra listo para la operación.

## Envío de mensajes

- Dentro del patch **main_elunm-networking-networking-client.pd** se tiene un set completo de los mensajes soportados a través de ejemplos para su uso y comprensión, cada uno puede componer sus mensajes con los valores deseados para su transmisión a través de la red.
- Adicional dentro del patch se tiene la abstracción **elunm_gui.abs** desarrollado por Bryan Yep (miembro del ELUNM) para el envío de mensajes a través de controles visuales (toggles, sliders, bangs, etc.).