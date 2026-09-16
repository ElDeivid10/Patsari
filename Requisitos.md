Claro. El `.md` funciona como **documento de requisitos técnicos para posteriormente desarrollar el código**, dejando clara la estructura del paquete, pines, sensores, estados de misión y almacenamiento.

También hice una precisión importante: en el BME688, el sensor entrega **resistencia de gas**, que puede utilizarse como indicador de VOC/calidad del aire; para un índice IAQ formal se necesitaría un algoritmo adicional.

# Requisitos técnicos del sistema de telemetría

## 1. Descripción general

El sistema será desarrollado para un **Arduino Nano ESP32** y tendrá como función principal adquirir datos de sensores, obtener la posición mediante GPS, transmitir la información mediante un módulo LoRa y almacenar exactamente los mismos datos en una tarjeta microSD.

El sistema contará con:

* Arduino Nano ESP32.
* Sensor BNO085.
* Sensor BME688.
* Módulo LoRa SX1278 / RA-02.
* GPS NEO-6M V2.
* Módulo lector microSD.
* LED indicador.

El sistema deberá realizar una lectura periódica de todos los sensores y generar y transmitir un paquete de telemetría como mínimo cada **1 segundo**, cumpliendo una frecuencia mínima de **1 Hz**.

Cada paquete deberá:

1. Tener un número de paquete consecutivo.
2. Contener los campos obligatorios de telemetría en el orden definido por el reglamento.
3. Contener los datos del BNO085.
4. Contener los datos del BME688.
5. Contener el voltaje de la batería.
6. Indicar el estado de la misión.
7. Ser transmitido mediante LoRa.
8. Ser almacenado en la tarjeta microSD con exactamente la misma estructura.

---

# 2. Hardware y conexiones

## 2.1 Arduino Nano ESP32

El controlador principal será un:

**Arduino Nano ESP32**

Será responsable de:

* Leer los sensores.
* Procesar los datos.
* Obtener información GPS.
* Calcular el estado de la misión.
* Crear los paquetes de telemetría.
* Transmitir los paquetes mediante LoRa.
* Guardar los paquetes en la microSD.
* Controlar el LED de estado.

---

# 3. Sensor BNO085

El BNO085 estará conectado mediante el bus **I2C**.

### Conexiones

| BNO085 | Arduino Nano ESP32 |
| ------ | ------------------ |
| SDA    | A4                 |
| SCL    | A5                 |
| VCC    | 3.3 V              |
| GND    | GND                |

## 3.1 Datos requeridos

El programa deberá obtener del BNO085:

### Acelerómetro

* Aceleración X.
* Aceleración Y.
* Aceleración Z.

Unidades requeridas para la trama de telemetría:

`g`

La aceleración deberá enviarse con una precisión de **2 decimales**. El sensor deberá configurarse o verificarse para contar con un rango mínimo de **±16 g**.

Unidades internas recomendadas:

`m/s²`

### Giroscopio

* Velocidad angular X.
* Velocidad angular Y.
* Velocidad angular Z.

Unidades recomendadas:

°/s

Se deberá seleccionar una sola unidad para toda la aplicación.

### Magnetómetro

* Campo magnético X.
* Campo magnético Y.
* Campo magnético Z.

Unidades recomendadas:

`µT`

Los datos del magnetómetro deberán utilizarse para obtener una referencia de orientación tipo **brújula** cuando sea posible.

---

# 4. Sensor BME688

El BME688 también estará conectado mediante I2C.

### Conexiones

| BME688 | Arduino Nano ESP32 |
| ------ | ------------------ |
| SDA    | A4                 |
| SCL    | A5                 |
| VCC    | 3.3 V              |
| GND    | GND                |

El BNO085 y el BME688 compartirán el mismo bus I2C.

## 4.1 Datos requeridos

El programa deberá obtener:

### Temperatura

Unidad:

`°C`

### Humedad relativa

Unidad:

`%`

### Presión atmosférica

Unidad:

`hPa`

### Altitud calculada mediante presión

Unidad:

`m`

La altitud deberá calcularse utilizando la presión atmosférica y una presión de referencia.

El sistema deberá permitir establecer una **presión de referencia al inicio del vuelo** para mejorar el cálculo de altitud relativa.

### Gas / VOC

El BME688 deberá utilizar su lectura de gas como indicador relacionado con:

* VOC.
* Calidad del aire.

El valor deberá almacenarse en el paquete de telemetría.

> Nota: el valor de resistencia de gas del BME688 no deberá presentarse directamente como un índice IAQ certificado. Para este proyecto puede utilizarse como indicador de VOC/calidad del aire.

---

# 5. Módulo LoRa SX1278

El módulo LoRa será utilizado para transmitir los paquetes de telemetría.

### Conexiones

| SX1278   | Arduino Nano ESP32 |
| -------- | ------------------ |
| RST      | D8                 |
| DIO0     | D2                 |
| NSS / CS | D9                 |
| MOSI     | D11                |
| MISO     | D12                |
| SCK      | D13                |
| VCC      | 3.3 V              |
| GND      | GND                |

El bus SPI utilizado por LoRa será compartido con el lector microSD.

Por lo tanto, cada dispositivo deberá utilizar su propio pin **CS/NSS**:

* LoRa: `D9`
* microSD: `D10`

Nunca deberán permanecer seleccionados simultáneamente ambos dispositivos SPI.

---

# 6. GPS NEO-6M V2

El GPS será utilizado para obtener la posición geográfica del sistema.

### Conexiones

| GPS | Arduino Nano ESP32                          |
| --- | ------------------------------------------- |
| TXD | D4                                          |
| RXD | D5                                          |
| VCC | 5 V o alimentación compatible con el módulo |
| GND | GND                                         |

El programa deberá intentar obtener:

* Latitud.
* Longitud.
* Estado de fijación GPS.
* Número de satélites, si está disponible.

La latitud y longitud deberán incluirse en cada paquete.

Si el GPS todavía no tiene una posición válida, el programa deberá utilizar un valor claramente identificable, por ejemplo:

`0.000000,0.000000`

y deberá indicar internamente que el GPS no tiene fix.

---

# 7. Lector microSD

El lector microSD utilizará el bus SPI.

### Conexiones

| microSD   | Arduino Nano ESP32                    |
| --------- | ------------------------------------- |
| CS        | D10                                   |
| MOSI      | D11                                   |
| CLK / SCK | D13                                   |
| MISO      | D12                                   |
| VCC       | Alimentación compatible con el módulo |
| GND       | GND                                   |

El módulo microSD compartirá:

* MOSI.
* MISO.
* SCK.

con el módulo LoRa.

El pin CS deberá utilizarse exclusivamente para seleccionar la tarjeta:

`D10`

---

# 8. LED indicador

Se utilizará un LED conectado a:

`A7`

El LED se utilizará para indicar que el sistema está transmitiendo y enviando datos a la estación de tierra.

El LED deberá parpadear aproximadamente cada 1 segundo, coincidiendo con la transmisión de cada paquete de telemetría mediante LoRa.

---

# 9. Lectura de voltaje de la batería

Se agregará un divisor de voltaje conectado a la entrada analógica:

`A1`

El divisor deberá adaptar el voltaje máximo de la batería al rango seguro de lectura del Arduino Nano ESP32.

El programa deberá:

* Leer el voltaje mediante A1.
* Convertir la lectura ADC al voltaje real de la batería utilizando la relación del divisor.
* Incluir el valor en el campo `VOLTAGE` de la telemetría.
* Enviar y almacenar el voltaje con precisión de **2 decimales**.
* Detectar lecturas fuera de rango o desconexión del divisor cuando sea posible.

---

# 10. Estados de la misión

El programa deberá calcular el estado de la misión y colocar uno de los siguientes códigos de cuatro caracteres en el campo `STATE`:

* `WAIT`: sistema encendido, inicializando o esperando el inicio del descenso.
* `DESC`: descenso detectado.
* `LAND`: aterrizaje detectado.

Los estados podrán calcularse mediante altitud, variación de altitud, tiempo y estabilidad de las lecturas.

## 10.1 Detección sugerida de estados

### Estado `WAIT`

El sistema permanecerá en `WAIT` mientras se inicializan los sensores, se obtiene la presión de referencia o no se confirma una disminución sostenida de altitud.

### Estado `DESC`

El sistema cambiará a `DESC` cuando se cumplan varias comprobaciones consecutivas, por ejemplo:

* La altitud disminuye más que un umbral configurable.
* La tendencia de altitud es negativa durante al menos 2 o 3 lecturas consecutivas.
* El tiempo de misión indica que el sistema ya fue liberado o inició la prueba.
* Opcionalmente, el acelerómetro confirma movimiento compatible con el descenso.

No se deberá cambiar a `DESC` por una sola lectura anormal.

### Estado `LAND`

El sistema cambiará a `LAND` cuando, después de haber estado en `DESC`, la variación de altitud sea menor que un umbral durante varias lecturas, la velocidad vertical estimada sea cercana a cero y el sistema permanezca estable durante un tiempo configurable.

Una vez confirmado `LAND`, el estado no deberá regresar a `WAIT` o `DESC` durante la misma misión.

### Parámetros configurables sugeridos

```cpp
ALTITUDE_CHANGE_THRESHOLD
DESCENT_CONFIRMATION_COUNT
LANDING_STABILITY_TIME
LANDING_CONFIRMATION_COUNT
```

## 10.2 Protección contra falsos positivos

El cálculo de estados deberá considerar:

* Variaciones normales de presión.
* Ruido del sensor.
* Cambios bruscos temporales.
* Lecturas incorrectas.
* Falta de datos del sensor.

El sistema deberá evitar cambiar de estado debido a una única lectura anormal.

---

# 11. Frecuencia de adquisición y transmisión

El sistema deberá generar y transmitir un paquete como mínimo cada:

**1 segundo (1 Hz)**

La secuencia general deberá ser:

```text
Leer sensores
      ↓
Leer GPS
      ↓
Calcular altitud
      ↓
Actualizar estado de la misión
      ↓
Crear paquete
      ↓
Transmitir mediante LoRa
      ↓
Guardar paquete en microSD
      ↓
Esperar siguiente ciclo
```

El tiempo de procesamiento no deberá provocar retrasos innecesarios.

Se deberá utilizar una lógica basada en `millis()` en lugar de utilizar largos `delay()` para permitir que GPS, sensores, Mission Clock y comunicación continúen funcionando.

---

# 12. Contador de paquetes

Cada paquete deberá tener un número único y consecutivo.

El primer paquete deberá comenzar en:

`1`

Después:

```text
1
2
3
4
5
6
...
```

El contador deberá representar el total de paquetes transmitidos desde el inicio de la misión. Cada paquete utilizará el valor consecutivo actual y el contador se incrementará después de preparar y transmitir ese paquete.

Ejemplo:

```text
Paquete 1
Paquete 2
Paquete 3
Paquete 4
```

El número de paquete deberá ser el tercer campo de la estructura, después de `TEAM_ID` y `MISSION_TIME`.

---

# 13. Estructura del paquete de telemetría

La trama deberá cumplir el formato indicado por el reglamento:

* Texto plano ASCII.
* Valores separados mediante comas.
* Un paquete por línea.
* Cada línea deberá finalizar con salto de línea (`\n`).
* Los campos obligatorios deberán conservar exactamente el orden indicado.

Los diez campos obligatorios serán:

```text
TEAM_ID,MISSION_TIME,PACKET_COUNT,ALTITUDE,TEMPERATURE,VOLTAGE,ACCEL_X,ACCEL_Y,ACCEL_Z,STATE
```

Valores iniciales y precisión:

* `TEAM_ID`: `0001` mientras el equipo no reciba su identificador definitivo.
* `MISSION_TIME`: `HH:MM:SS`.
* `PACKET_COUNT`: entero consecutivo.
* `ALTITUDE`: una decimal.
* `TEMPERATURE`: una decimal.
* `VOLTAGE`: dos decimales.
* `ACCEL_X`, `ACCEL_Y`, `ACCEL_Z`: dos decimales en `g`.
* `STATE`: código de cuatro caracteres: `WAIT`, `DESC` o `LAND`.

Las variables adicionales de la misión secundaria deberán anexarse únicamente después de los campos obligatorios y en un orden fijo. Se podrán incluir, por ejemplo:

```text
TEAM_ID,MISSION_TIME,PACKET_COUNT,ALTITUDE,TEMPERATURE,VOLTAGE,ACCEL_X,ACCEL_Y,ACCEL_Z,STATE,LATITUDE,LONGITUDE,PRESSURE,HUMIDITY,VOC,GYRO_X,GYRO_Y,GYRO_Z,MAG_X,MAG_Y,MAG_Z
```

## 13.1 Ejemplo

```text
0001,00:00:01,1,0.0,25.4,8.12,0.02,-0.01,0.99,WAIT,19.406521,-102.058721,998.21,62.5,15432,0.01,-0.02,0.00,32.4,-18.2,41.7
```

Otro paquete:

```text
0001,00:00:02,2,0.8,25.5,8.10,0.10,-0.05,1.00,DESC,19.406530,-102.058740,998.05,62.2,15120,0.02,-0.01,0.01,31.9,-18.5,42.0
```

Después del aterrizaje:

```text
0001,00:01:25,85,42.6,26.8,7.92,0.03,-0.01,1.00,LAND,19.407100,-102.060100,1003.21,64.1,14820,0.15,-0.08,0.02,35.1,-16.4,43.2
```

---

# 14. Orden obligatorio de los datos

El programa deberá mantener siempre el mismo orden:

| # | Campo | Descripción |
| -: | --- | --- |
| 1 | `TEAM_ID` | Identificador del equipo, inicialmente `0001` |
| 2 | `MISSION_TIME` | Tiempo desde el encendido, `HH:MM:SS` |
| 3 | `PACKET_COUNT` | Conteo consecutivo de paquetes transmitidos |
| 4 | `ALTITUDE` | Altitud relativa, una decimal |
| 5 | `TEMPERATURE` | Temperatura interna, una decimal |
| 6 | `VOLTAGE` | Voltaje de batería, dos decimales |
| 7 | `ACCEL_X` | Aceleración X en `g`, dos decimales |
| 8 | `ACCEL_Y` | Aceleración Y en `g`, dos decimales |
| 9 | `ACCEL_Z` | Aceleración Z en `g`, dos decimales |
| 10 | `STATE` | Estado de cuatro caracteres: `WAIT`, `DESC` o `LAND` |
El orden **no deberá cambiar entre paquetes**.

Después del campo `STATE` podrán agregarse, en orden fijo, los campos de misión secundaria:

| Campo | Descripción |
| --- | --- |
| `LATITUDE` | Latitud GPS |
| `LONGITUDE` | Longitud GPS |
| `PRESSURE` | Presión atmosférica |
| `HUMIDITY` | Humedad relativa |
| `VOC` | Indicador de gas/VOC |
| `GYRO_X`, `GYRO_Y`, `GYRO_Z` | Velocidad angular |
| `MAG_X`, `MAG_Y`, `MAG_Z` | Campo magnético |

---

# 15. Almacenamiento en tarjeta microSD

Todos los paquetes transmitidos deberán almacenarse también en la tarjeta microSD.

El archivo podrá llamarse:

```text
TELEMETRY.CSV
```

El formato deberá ser compatible con CSV.

## 15.1 Encabezado

Al iniciar una nueva misión, el archivo deberá contener:

```text
TEAM_ID,MISSION_TIME,PACKET_COUNT,ALTITUDE,TEMPERATURE,VOLTAGE,ACCEL_X,ACCEL_Y,ACCEL_Z,STATE,LATITUDE,LONGITUDE,PRESSURE,HUMIDITY,VOC,GYRO_X,GYRO_Y,GYRO_Z,MAG_X,MAG_Y,MAG_Z
```

Posteriormente se agregará una línea por cada paquete.

Ejemplo:

```text
TEAM_ID,MISSION_TIME,PACKET_COUNT,ALTITUDE,TEMPERATURE,VOLTAGE,ACCEL_X,ACCEL_Y,ACCEL_Z,STATE,LATITUDE,LONGITUDE,PRESSURE,HUMIDITY,VOC,GYRO_X,GYRO_Y,GYRO_Z,MAG_X,MAG_Y,MAG_Z
0001,00:00:01,1,0.0,25.4,8.12,0.02,-0.01,0.99,WAIT,19.406521,-102.058721,998.21,62.5,15432,0.01,-0.02,0.00,32.4,-18.2,41.7
0001,00:00:02,2,0.8,25.5,8.10,0.10,-0.05,1.00,DESC,19.406530,-102.058740,998.05,62.2,15120,0.02,-0.01,0.01,31.9,-18.5,42.0
```

---

# 16. Igualdad entre paquete LoRa y registro SD

La información almacenada en la microSD deberá corresponder exactamente con la información enviada mediante LoRa.

La variable o cadena utilizada para construir el paquete deberá reutilizarse para:

1. Transmitir mediante LoRa.
2. Guardar en microSD.

Conceptualmente:

```text
crear paquete
     ↓
packet = datos
     ↓
LoRa.send(packet)
     ↓
SD.println(packet)
```

No deberán existir dos formatos diferentes para los mismos datos.

---

# 17. Manejo de errores

El programa deberá contemplar errores de inicialización.

Al iniciar deberá comprobar:

* BNO085 detectado.
* BME688 detectado.
* LoRa inicializado.
* GPS funcionando.
* microSD detectada.
* Divisor de voltaje disponible en A1.

Si un componente no responde, el programa deberá indicar el error mediante el LED y evitar que un fallo de un componente provoque un bloqueo completo del sistema cuando sea posible.

## 17.1 Errores recomendados

Ejemplos:

```text
ERROR_BNO085
ERROR_BME688
ERROR_LORA
ERROR_GPS
ERROR_SD
```

Los errores deberán poder identificarse fácilmente durante las pruebas.

---

# 18. Inicialización de la misión

Al encender el sistema deberá realizarse aproximadamente la siguiente secuencia:

```text
INICIO
  ↓
Inicializar comunicación serial
      ↓
Inicializar Mission Clock en 00:00:00
      ↓
Inicializar I2C
  ↓
Inicializar BNO085
  ↓
Inicializar BME688
  ↓
Inicializar GPS
  ↓
Inicializar LoRa
      ↓
Inicializar microSD
      ↓
Obtener presión inicial de referencia
      ↓
Inicializar contador de paquetes
  ↓
Comenzar misión
```

---

# 19. Presión de referencia y altitud

Para mejorar el cálculo de altitud y de los estados de misión, el sistema deberá establecer una presión de referencia al inicio.

La altitud podrá calcularse utilizando la presión actual respecto a la presión de referencia.

Conceptualmente:

```text
Presión inicial
      ↓
Presión actual
      ↓
Cálculo de diferencia
      ↓
Altitud relativa
```

La presión inicial deberá obtenerse después de que el sensor BME688 esté correctamente inicializado.

Se recomienda realizar varias lecturas iniciales y utilizar un promedio para reducir el ruido.

---

# 20. Mission Clock

El sistema deberá implementar un contador de tiempo relativo de la misión.

El Mission Clock deberá:

* Iniciar en `00:00:00` inmediatamente después de encender el sistema.
* Continuar contando aunque el GPS no tenga fix.
* Utilizarse para generar el campo `MISSION_TIME`.
* Mantener una precisión mínima de ±1 segundo por cada hora de operación.
* Implementarse preferentemente con `millis()` y sin utilizar `delay()` prolongados.

---

# 21. Requisitos de robustez

El código deberá cumplir los siguientes requisitos:

* No utilizar `delay()` prolongados durante la operación normal.
* Utilizar `millis()` para controlar el intervalo de transmisión de aproximadamente 1 segundo.
* Evitar bloqueos cuando el GPS no tenga señal.
* Evitar bloqueos si una lectura del sensor falla.
* Verificar que la microSD esté disponible antes de escribir.
* Verificar la inicialización de LoRa.
* Utilizar diferentes pines CS para LoRa y microSD.
* Mantener un único formato de paquete.
* Mantener el contador de paquetes consecutivo.
* No cambiar de estado por una sola lectura anormal.
* Mantener el estado de misión coherente durante cada ejecución.
* Leer el voltaje de batería mediante el divisor conectado a A1.
* Formatear el voltaje con dos decimales.
* Mantener `MISSION_TIME` y `PACKET_COUNT` consecutivos y válidos.
* Mantener el paquete suficientemente pequeño para ser transmitido correctamente mediante LoRa.

---

# 22. Resumen de pines

| Componente  | Señal   | Pin Arduino Nano ESP32 |
| ----------- | ------- | ---------------------- |
| BNO085      | SDA     | A4                     |
| BNO085      | SCL     | A5                     |
| BME688      | SDA     | A4                     |
| BME688      | SCL     | A5                     |
| LoRa SX1278 | RST     | D8                     |
| LoRa SX1278 | DIO0    | D2                     |
| LoRa SX1278 | NSS/CS  | D9                     |
| LoRa SX1278 | MOSI    | D11                    |
| LoRa SX1278 | MISO    | D12                    |
| LoRa SX1278 | SCK     | D13                    |
| GPS NEO-6M  | TXD     | D4                     |
| GPS NEO-6M  | RXD     | D5                     |
| microSD     | CS      | D10                    |
| microSD     | MOSI    | D11                    |
| microSD     | MISO    | D12                    |
| microSD     | CLK/SCK | D13                    |
| Batería     | Divisor de voltaje | A1             |
| LED         | Señal   | A7                     |

---

# 23. Flujo general del programa

```text
┌──────────────────────────────┐
│        INICIAR SISTEMA       │
└──────────────┬───────────────┘
               ↓
┌──────────────────────────────┐
│     Inicializar sensores     │
│    GPS / LoRa / SD / A1      │
└──────────────┬───────────────┘
               ↓
┌──────────────────────────────┐
│ Obtener presión de referencia│
└──────────────┬───────────────┘
               ↓
       ┌───────▼────────┐
       │ Cada 1 segundo │
       └───────┬────────┘
               ↓
┌──────────────────────────────┐
│       Leer BNO085            │
│ Acelerómetro/Gyro/Magnetómetro│
└──────────────┬───────────────┘
               ↓
┌──────────────────────────────┐
│        Leer BME688           │
│ P/T/H/VOC/Altitud            │
└──────────────┬───────────────┘
               ↓
┌──────────────────────────────┐
│          Leer GPS            │
│     Latitud / Longitud       │
└──────────────┬───────────────┘
               ↓
┌──────────────────────────────┐
│    Actualizar estado         │
│     WAIT/DESC/LAND           │
└──────────────┬───────────────┘
               ↓
        ¿Estado LAND confirmado?
          /           \
        NO             SI
        ↓               ↓
      Continuar    Mantener LAND
                       ↓
                 Crear paquete
                       ↓
            ┌──────────┴──────────┐
            ↓                     ↓
       Transmitir LoRa        Guardar SD
            └──────────┬──────────┘
                       ↓
                Siguiente ciclo
```

---

# 24. Criterios de aceptación

El código será considerado funcional cuando:

* [ ] El Arduino Nano ESP32 inicie correctamente.
* [ ] El BNO085 proporcione acelerómetro, giroscopio y magnetómetro.
* [ ] El BME688 proporcione temperatura, humedad, presión y lectura de gas/VOC.
* [ ] El divisor de voltaje conectado a A1 permita leer el voltaje de la batería.
* [ ] El voltaje se transmita con precisión de dos decimales.
* [ ] La altitud pueda calcularse mediante presión.
* [ ] El GPS proporcione latitud y longitud cuando exista fix.
* [ ] El LoRa SX1278 transmita correctamente los paquetes.
* [ ] Los paquetes sean enviados como mínimo a 1 Hz.
* [ ] El campo `TEAM_ID` sea inicialmente `0001`.
* [ ] El campo `MISSION_TIME` inicie en `00:00:00` al encender el sistema.
* [ ] El primer paquete tenga `PACKET_COUNT` igual a `1`.
* [ ] El contador aumente consecutivamente.
* [ ] La trama sea ASCII, esté separada por comas y termine en `\n`.
* [ ] Los diez campos obligatorios aparezcan primero y en el orden reglamentario.
* [ ] Los datos adicionales aparezcan después del campo `STATE`.
* [ ] El campo `STATE` use códigos de cuatro caracteres (`WAIT`, `DESC`, `LAND`).
* [ ] Los paquetes transmitidos y almacenados tengan exactamente la misma estructura.
* [ ] La tarjeta microSD almacene los datos correctamente.
* [ ] El sistema utilice D9 para LoRa CS y D10 para microSD CS.
* [ ] El cambio a `DESC` requiera varias comprobaciones consecutivas.
* [ ] El cambio a `LAND` requiera estabilidad durante varias lecturas.
* [ ] El sistema continúe recopilando y transmitiendo datos después del aterrizaje.
* [ ] Los errores de sensores o periféricos puedan identificarse durante las pruebas.

---

# 25. Consideración para la implementación

El código deberá desarrollarse de forma modular. Se recomienda separar las funciones principales en módulos o funciones independientes:

```text
initSensors()
readBNO085()
readBME688()
readGPS()
readBatteryVoltage()
updateMissionClock()
updateMissionState()
calculateAltitude()
createTelemetryPacket()
sendLoRa()
saveToSD()
updateLED()
```

Esto permitirá realizar pruebas independientes de cada componente y facilitará posteriormente el mantenimiento del código.

El objetivo final es disponer de un sistema de telemetría capaz de **medir, procesar, transmitir y almacenar los datos del vuelo**, con una trama compatible con el reglamento, un Mission Clock, lectura del voltaje de batería y una máquina de estados de misión basada en altitud, tiempo y estabilidad de las mediciones.
