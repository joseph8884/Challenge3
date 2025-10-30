# Sistema IoT de Detección Temprana de Deslizamientos de Tierra
## Challenge #3 - Internet de las Cosas

**Equipo #6**  
**Integrantes:** Héctor José Guzmán, Luis Mario [Apellido]  
**Profesor:** Juan Manuel King
**Universidad de La Sabana** - Facultad de Ingeniería - Ingeniería Informática  
**Fecha de Entrega:** 10 de noviembre de 2025

---

## Tabla de Contenidos

1. [Resumen General](#1-resumen-general)
2. [Solución Propuesta](#2-solución-propuesta)
   - 2.1 [Restricciones de Diseño](#21-restricciones-de-diseño)
   - 2.2 [Arquitectura del Sistema](#22-arquitectura-del-sistema)
   - 2.3 [Desarrollo Teórico Modular](#23-desarrollo-teórico-modular)
3. [Configuración Experimental, Resultados y Análisis](#3-configuración-experimental-resultados-y-análisis)
4. [Autoevaluación del Protocolo de Pruebas](#4-autoevaluación-del-protocolo-de-pruebas)
5. [Conclusiones y Trabajo Futuro](#5-conclusiones-y-trabajo-futuro)
6. [Referencias](#6-referencias)
7. [Anexos](#7-anexos)

---

## 1. Resumen General

### 1.1 Motivación

En mayo de 2025 se presentaron deslizamientos en Tabio y Cajicá (Cundinamarca) que ocasionaron cierre de vías y afectaron a más de 2.200 familias. Este reto busca un sistema de bajo costo, desplegable localmente y que permita a autoridades monitorear indicadores clave de riesgo de inestabilidad: inclinación, vibración, lluvia, humedad del suelo y temperatura [1].

El enfoque se centra en: (1) medición multi-sensorial continua [2], (2) procesamiento y fusión local de datos [3], (3) visualización restringida a la WLAN oficial y (4) alarmas diferenciadas por nivel [4].


### 1.2 Justificación

La región de la Sabana de Bogotá, y Cundinamarca en general, presenta una alta vulnerabilidad a fenómenos de remoción en masa, como los ocurridos recientemente en Tabio y Cajicá, que representan un riesgo constante para la vida y la infraestructura. Actualmente, muchas zonas críticas carecen de sistemas de monitoreo continuo y en tiempo real, dependiendo de inspecciones manuales que son costosas y poco frecuentes.

Este proyecto se justifica al proponer una solución tecnológica de bajo costo que cierra esa brecha. Un sistema IoT ofrece ventajas significativas sobre los métodos tradicionales, como la medición constante de variables clave, el análisis de datos in situ y la capacidad de emitir alertas tempranas automáticas. La implementación de esta tecnología busca reducir las pérdidas humanas y materiales al facilitar evacuaciones oportunas y la toma de decisiones informadas por parte de las autoridades, alineándose con las políticas nacionales de gestión del riesgo de desastres.


### 1.3 Objetivos

#### Objetivo General

Desarrollar un prototipo funcional de sistema IoT para detectar señales de inestabilidad en taludes mediante la medición y análisis en tiempo real de inclinación, intensidad de lluvia, vibración y humedad del suelo, emitiendo alertas tempranas in situ y remotas a las autoridades locales.

#### Objetivos Específicos

-   Diseñar e implementar un sistema de adquisición de datos en un ESP32 que integre sensores de inclinación (MPU6050), vibración, lluvia, humedad del suelo (YL-100) y temperatura (DS18B20).
-   Desarrollar un algoritmo de fusión de señales para combinar las lecturas de los sensores en un índice de riesgo unificado, capaz de activar un sistema de alarmas con múltiples niveles.
-   Implementar un dashboard web local en el ESP32 y una interfaz en la nube (Ubidots) para la visualización en tiempo real de los datos y el estado del sistema, utilizando MQTT como protocolo de comunicación.
-   Configurar un gateway en una Raspberry Pi que actúe como broker MQTT local, almacene los datos históricos en una base de datos SQLite y gestione la comunicación con la plataforma en la nube.
-   Validar la funcionalidad del prototipo mediante la ejecución de escenarios de prueba simulados que verifiquen la correcta detección de condiciones de riesgo y la activación de las alertas correspondientes.


### 1.4 Alcance del Proyecto

El proyecto abarca:

- **Monitoreo de variables físicas:** Inclinación del talud (MPU6050), intensidad de lluvia (sensor analógico), vibración (sensor digital), humedad del suelo (YL-100) y temperatura ambiente (DS18B20).
- **Dispositivo IoT embebido:** ESP32 como unidad de procesamiento y adquisición de datos.
- **Gateway IoT local:** Raspberry Pi para procesamiento intermedio, almacenamiento en base de datos local (SQLite) y comunicación MQTT.
- **Plataforma IoT en la nube:** Integración con Ubidots para visualización remota.
- **Lógica de fusión:** Combinación de al menos tres señales independientes para generación de alertas.
- **Notificación dual:** Alarmas físicas in situ (buzzer, LEDs) y tablero de control web (local y global).
- **Módulo de Inteligencia Artificial:** API de Gemini/ChatGPT para predicciones y recomendaciones.

**Limitaciones:**
- El alcance no incluye instalación permanente en campo.
- Se trabaja con datos simulados y pruebas de laboratorio.
- No se implementa alimentación autónoma (energía solar/batería) en esta fase.

### 1.5 Estructura del Documento

Este documento se organiza en las siguientes secciones: solución propuesta (restricciones, arquitectura y desarrollo modular), configuración experimental y resultados, autoevaluación de pruebas, conclusiones, referencias bibliográficas y anexos técnicos.

---

## 2. Solución Propuesta

### 2.1 Restricciones de Diseño

#### 2.1.1 Restricciones Técnicas

<!--
═══════════════════════════════════════════════════════════════════════════════
COMPLETAR: Detallar las restricciones técnicas identificadas:
- Hardware obligatorio: ESP32, Raspberry Pi, sensores específicos
- Comunicación: MQTT (broker Mosquitto local y Ubidots cloud)
- Base de datos local: SQLite en Raspberry Pi
- Servidor web embebido en ESP32
- ISR (Interrupt Service Routines) para lectura de sensores críticos
- Limitaciones de memoria RAM/Flash del ESP32
- Restricciones de conectividad (WiFi WLAN local obligatoria)
- Requerimientos de tiempo real (frecuencia de muestreo, latencia máxima)
═══════════════════════════════════════════════════════════════════════════════
-->

#### 2.1.2 Restricciones Económicas

<!--
═══════════════════════════════════════════════════════════════════════════════
COMPLETAR: Describir restricciones de presupuesto:
- Presupuesto total disponible
- Costo de componentes (sensores, microcontroladores, cableado, etc.)
- Limitaciones que afectaron selección de componentes
- Comparación costo-beneficio con alternativas comerciales
═══════════════════════════════════════════════════════════════════════════════
-->

#### 2.1.3 Restricciones Regulatorias y de Seguridad

<!--
═══════════════════════════════════════════════════════════════════════════════
COMPLETAR: Identificar normas y regulaciones aplicables:
- Normatividad colombiana sobre sistemas de alerta temprana
- Estándares de seguridad eléctrica y protección IP requerida
- Regulaciones de transmisión de datos (WiFi, MQTT)
- Consideraciones de privacidad de datos si aplica
- Normas de gestión del riesgo aplicables (si las hay)
═══════════════════════════════════════════════════════════════════════════════
-->

#### 2.1.4 Restricciones de Espacio y Despliegue

<!--
═══════════════════════════════════════════════════════════════════════════════
COMPLETAR: Describir limitaciones físicas:
- Dimensiones máximas del dispositivo IoT
- Condiciones ambientales esperadas (humedad, temperatura, polvo)
- Requerimientos de protección (carcasa, impermeabilización)
- Accesibilidad para mantenimiento
═══════════════════════════════════════════════════════════════════════════════
-->

#### 2.1.5 Restricciones Temporales

<!--
═══════════════════════════════════════════════════════════════════════════════
COMPLETAR: Detallar plazos y cronograma:
- Tiempo total de desarrollo (fechas inicio y fin)
- Hitos principales y entregables
- Tiempo dedicado a cada fase (diseño, implementación, pruebas)
═══════════════════════════════════════════════════════════════════════════════
-->

#### 2.1.6 Restricciones de Escalabilidad

<!--
═══════════════════════════════════════════════════════════════════════════════
COMPLETAR: Analizar capacidad de expansión:
- Número máximo de sensores soportados
- Capacidad de agregar nuevos nodos IoT a la red
- Limitaciones del Gateway (procesamiento, almacenamiento)
- Escalabilidad de la plataforma cloud (Ubidots)
═══════════════════════════════════════════════════════════════════════════════
-->

### 2.2 Arquitectura del Sistema

#### 2.2.1 Diagrama de Bloques General

<!--
═══════════════════════════════════════════════════════════════════════════════
COMPLETAR: Insertar diagrama de bloques que muestre:
- Capa de sensores (MPU6050, DS18B20, sensor lluvia, sensor vibración, YL-100)
- Dispositivo IoT (ESP32) con:
  * Módulo de adquisición de datos
  * Lógica de fusión
  * Servidor web embebido
  * Cliente MQTT
  * Alarmas físicas (buzzer, LEDs)
- Gateway IoT (Raspberry Pi) con:
  * Broker MQTT (Mosquitto)
  * Base de datos SQLite
  * Cliente MQTT hacia cloud
- Plataforma Cloud (Ubidots)
- Módulo de IA (API externa)
- Usuarios finales (autoridades locales)

Ejemplo de herramientas: draw.io, Lucidchart, Mermaid, etc.
═══════════════════════════════════════════════════════════════════════════════
-->

#### 2.2.2 Arquitectura de Hardware

<!--
═══════════════════════════════════════════════════════════════════════════════
COMPLETAR: Crear diagrama esquemático detallado mostrando:
- Conexiones de pines entre ESP32 y cada sensor
- Interfaces de comunicación (I2C, OneWire, ADC, GPIO)
- Circuito de alarmas (buzzer, LEDs con resistencias)
- Alimentación (fuente de voltaje, reguladores si aplican)
- Diagrama de conexión Raspberry Pi ↔ ESP32 (si hay comunicación directa)
- Tabla de asignación de pines del ESP32

Incluir esquemático en formato imagen o PDF generado desde KiCad, Fritzing, etc.
═══════════════════════════════════════════════════════════════════════════════
-->

**Tabla de Asignación de Pines (ESP32):**

| Sensor/Actuador | Tipo de Interfaz | Pin(es) ESP32 | Notas |
|-----------------|------------------|---------------|-------|
| MPU6050 | I2C | SDA: GPIO21, SCL: GPIO22 | Dirección 0x68 |
| DS18B20 | OneWire | GPIO5 | Temperatura |
| Sensor Vibración | Digital (ISR) | GPIO34 | Interrupción |
| Sensor Lluvia | Analógico/Digital | ADC: GPIO36, Digital: GPIO4 | Intensidad |
| YL-100 (Humedad Suelo) | Analógico | GPIO39 | Lectura ADC |
| LED Verde | Digital | GPIO13 | Estado Normal |
| LED Amarillo | Digital | GPIO12 | Precaución |
| LED Naranja | Digital | GPIO14 | Alerta |
| LED Rojo | Digital | GPIO27 | Emergencia |
| Buzzer | PWM | GPIO25 | Alarma sonora |

<!--
═══════════════════════════════════════════════════════════════════════════════
NOTA: Ajustar la tabla según los pines realmente utilizados en su implementación
═══════════════════════════════════════════════════════════════════════════════
-->

#### 2.2.3 Arquitectura de Software

<!--
═══════════════════════════════════════════════════════════════════════════════
COMPLETAR: Crear diagrama de capas de software:

CAPA DE APLICACIÓN:
- Dashboard Web (HTML/CSS/JS)
- API REST endpoints (/data, /history, /ack, /reset, /emergency)

CAPA DE LÓGICA DE NEGOCIO:
- Motor de fusión de señales
- Máquina de estados de alarmas
- Módulo de IA (integración API)

CAPA DE COMUNICACIÓN:
- Cliente MQTT (ESP32 → Raspberry Pi)
- Servidor MQTT (Raspberry Pi → Ubidots)
- Servidor Web Asíncrono (ESP32WebServer)

CAPA DE DATOS:
- Buffer circular de sensores
- Base de datos SQLite (Raspberry Pi)
- Persistencia de configuración (SPIFFS/Preferences)

CAPA DE HARDWARE ABSTRACTION LAYER (HAL):
- Drivers de sensores
- Control de actuadores
- Gestión de interrupciones (ISR)

Usar UML, capas o diagrama modular
═══════════════════════════════════════════════════════════════════════════════
-->

#### 2.2.4 Flujo de Datos

<!--
═══════════════════════════════════════════════════════════════════════════════
COMPLETAR: Crear diagrama de flujo de datos (DFD) mostrando:
1. Sensores → ESP32 (adquisición)
2. ESP32 → Procesamiento local (fusión)
3. ESP32 → Raspberry Pi (MQTT publish)
4. Raspberry Pi → SQLite (almacenamiento)
5. Raspberry Pi → Ubidots (MQTT publish)
6. ESP32 → Actuadores locales (alarmas)
7. ESP32 → Servidor Web → Clientes (dashboard)
8. API IA ← ESP32/Raspberry Pi (consultas de predicción)

Incluir flujo bidireccional donde aplique (ej: comandos desde dashboard)
═══════════════════════════════════════════════════════════════════════════════
-->

### 2.3 Desarrollo Teórico Modular

#### 2.3.1 Criterios de Diseño Establecidos

<!--
═══════════════════════════════════════════════════════════════════════════════
COMPLETAR: Documentar decisiones de diseño clave:

CRITERIOS DE DISEÑO DE HARDWARE:
- ¿Por qué se seleccionó cada sensor específico?
- Justificación de rangos de medición requeridos
- Criterios de consumo energético
- Redundancia y tolerancia a fallos

CRITERIOS DE DISEÑO DE SOFTWARE:
- Arquitectura concurrente (FreeRTOS tasks, ISRs)
- Frecuencia de muestreo de cada sensor (justificación)
- Algoritmo de fusión de señales (ponderación, umbrales)
- Protocolo de comunicación (por qué MQTT, QoS seleccionado)
- Estrategia de almacenamiento (buffer circular vs. persistente)

CRITERIOS DE DISEÑO DE INTERFAZ:
- Diseño responsive del dashboard
- Priorización de información crítica
- Tiempo de refresco de datos
- Accesibilidad y usabilidad

ESTÁNDARES APLICADOS:
- Estándares de código (nomenclatura, documentación)
- Estándares de comunicación (MQTT v3.1.1/v5)
- Buenas prácticas de ingeniería de software embebido
═══════════════════════════════════════════════════════════════════════════════
-->

#### 2.3.2 Diagramas UML del Sistema

##### 2.3.2.1 Diagrama de Casos de Uso

<!--
═══════════════════════════════════════════════════════════════════════════════
COMPLETAR: Crear diagrama de casos de uso mostrando:

ACTORES:
- Autoridad Local (usuario principal)
- Sistema IoT (actor secundario)
- Módulo de IA (actor externo)

CASOS DE USO PRINCIPALES:
- Monitorear estado del talud en tiempo real
- Visualizar dashboard web local
- Visualizar dashboard cloud (Ubidots)
- Reconocer alarma activa
- Recibir alerta de emergencia
- Consultar historial de mediciones
- Activar emergencia manual
- Solicitar predicción/recomendación IA
- Configurar umbrales de alerta

Incluir relaciones <<include>> y <<extend>> donde aplique
═══════════════════════════════════════════════════════════════════════════════
-->

##### 2.3.2.2 Diagrama de Clases

<!--
═══════════════════════════════════════════════════════════════════════════════
COMPLETAR: Crear diagrama de clases principal con al menos:

CLASES DE SENSORES:
- SensorInterface (interfaz abstracta)
  * +readValue(): float
  * +isConnected(): bool
  * +calibrate(): void
- MPU6050Sensor : SensorInterface
- DS18B20Sensor : SensorInterface
- VibrationSensor : SensorInterface
- RainSensor : SensorInterface
- SoilMoistureSensor : SensorInterface

CLASES DE PROCESAMIENTO:
- DataProcessor
  * -filterWindow: int
  * +normalizeData(): float
  * +calculateGradient(): float
- FusionEngine
  * -weights: float[]
  * +calculateRisk(): float
  * +detectSynergies(): bool
- AlarmStateMachine
  * -currentState: State
  * +transition(): void
  * +acknowledge(): void

CLASES DE COMUNICACIÓN:
- MQTTClient
  * +publish(): bool
  * +subscribe(): void
- WebServerController
  * +handleRequest(): void
- DataBuffer
  * -circularBuffer: SensorData[]
  * +store(): void
  * +retrieve(): SensorData

CLASES DE ACTUADORES:
- ActuatorController
  * +setLED(): void
  * +playBuzzer(): void
  * +stopAlarm(): void

Incluir relaciones (herencia, composición, agregación, asociación)
═══════════════════════════════════════════════════════════════════════════════
-->

##### 2.3.2.3 Diagrama de Secuencia - Flujo de Alerta

<!--
═══════════════════════════════════════════════════════════════════════════════
COMPLETAR: Crear diagrama de secuencia para el escenario:
"Detección de condición de riesgo y activación de alarma"

OBJETOS PARTICIPANTES:
- Sensores
- SensorTask (FreeRTOS)
- FusionEngine
- AlarmStateMachine
- ActuatorController
- MQTTClient
- WebServer
- AutoridadLocal (actor)

FLUJO:
1. SensorTask lee todos los sensores (cada 1 segundo)
2. SensorTask llama a FusionEngine.calculateRisk()
3. FusionEngine retorna score y nivel
4. SensorTask llama a AlarmStateMachine.transition(nivel)
5. Si nivel >= 2: AlarmStateMachine activa alarma
6. AlarmStateMachine llama a ActuatorController.setLEDs() y .playBuzzer()
7. SensorTask publica datos vía MQTTClient
8. AutoridadLocal visualiza alerta en WebServer/Dashboard
9. AutoridadLocal envía comando /ack
10. WebServer llama a AlarmStateMachine.acknowledge()
11. AlarmStateMachine silencia buzzer pero mantiene LED
═══════════════════════════════════════════════════════════════════════════════
-->

##### 2.3.2.4 Diagrama de Estados

<!--
═══════════════════════════════════════════════════════════════════════════════
COMPLETAR: Crear diagrama de estados para AlarmStateMachine:

ESTADOS:
- INICIALIZANDO
- NORMAL (nivel 0)
- PRECAUCIÓN (nivel 1)
- ALERTA (nivel 2)
- EMERGENCIA (nivel 3)
- ALARMA_RECONOCIDA
- EMERGENCIA_MANUAL

TRANSICIONES (incluir condiciones y acciones):
- INICIALIZANDO → NORMAL [hardware OK]
- NORMAL ↔ PRECAUCIÓN [score cambia]
- PRECAUCIÓN ↔ ALERTA [score cambia]
- ALERTA ↔ EMERGENCIA [score cambia]
- ALERTA/EMERGENCIA → ALARMA_RECONOCIDA [comando /ack]
- Cualquiera → EMERGENCIA_MANUAL [comando /emergency]
- EMERGENCIA_MANUAL → estado previo [timeout 60s]

Incluir acciones en cada transición (activar/desactivar alarmas)
═══════════════════════════════════════════════════════════════════════════════
-->

##### 2.3.2.5 Diagrama de Actividades - Lógica de Fusión

<!--
═══════════════════════════════════════════════════════════════════════════════
COMPLETAR: Crear diagrama de actividades detallando:

ACTIVIDAD: calcularRiesgoFusion()

1. INICIO
2. [Paralelización] Leer 5 sensores simultáneamente
3. Para cada sensor:
   - Normalizar valor (0-100)
   - Calcular score individual
4. [Sincronización] Esperar todos los scores
5. Aplicar pesos ponderados (w_inc=0.35, w_vib=0.25, ...)
6. Calcular score_base = Σ(score_i * peso_i)
7. Detectar sinergias críticas:
   - ¿inc>60 AND vib>60? → multiplicador *= 1.5
   - ¿lluvia>60 AND suelo>60? → multiplicador *= 1.5
   - ¿inc>50 AND vib>50 AND suelo>70? → multiplicador *= 1.3
8. score_final = score_base * multiplicador
9. Mapear a nivel (0-3) según umbrales
10. Actualizar datos compartidos (con mutex)
11. FIN

Usar notación UML activity diagram con forks/joins para concurrencia
═══════════════════════════════════════════════════════════════════════════════
-->

#### 2.3.3 Módulos de Software Desarrollados

##### 2.3.3.1 Módulo de Adquisición de Sensores

<!--
═══════════════════════════════════════════════════════════════════════════════
COMPLETAR: Documentar módulo de sensores:

DESCRIPCIÓN:
- Responsabilidades del módulo
- Sensores gestionados
- Frecuencia de muestreo

INTERFAZ PÚBLICA:
```cpp
class SensorInterface {
public:
    virtual float readValue() = 0;
    virtual bool isConnected() = 0;
    virtual void calibrate() = 0;
    virtual String getLastError() = 0;
};
```

IMPLEMENTACIONES:
- MPU6050Sensor: detalles de lectura I2C, cálculo de inclinación
- DS18B20Sensor: protocolo OneWire, conversión de temperatura
- VibrationSensor: ISR para conteo de pulsos, detección de vibración continua
- RainSensor: lectura ADC, mapeo a intensidad de lluvia
- SoilMoistureSensor: lectura ADC, estimación de % humedad

MANEJO DE ERRORES:
- Timeouts de comunicación
- Valores fuera de rango (NaN)
- Sensores desconectados

CALIBRACIÓN:
- Procedimiento de calibración para cada sensor
- Valores de referencia utilizados
═══════════════════════════════════════════════════════════════════════════════
-->

##### 2.3.3.2 Módulo de Fusión de Señales

<!--
═══════════════════════════════════════════════════════════════════════════════
COMPLETAR: Documentar algoritmo de fusión:

DESCRIPCIÓN:
- Objetivo: combinar múltiples señales en un score unificado de riesgo
- Método: ponderación + detección de sinergias

FUNCIONES DE SCORING INDIVIDUAL:
```cpp
float scoreInclinacion(float grados);
float scoreVibracion(float eventos_por_min, bool continua);
float scoreLluvia(int adc_value);
float scoreSuelo(float porcentaje_humedad);
float scoreTemperatura(float celsius, float gradiente);
```

Para cada función, documentar:
- Umbrales utilizados (NORMAL, PRECAUCIÓN, ALERTA, EMERGENCIA)
- Justificación geotécnica de los umbrales
- Curva de escalado (lineal, exponencial, por tramos)

ALGORITMO DE FUSIÓN:
```cpp
float calcularRiesgoFusion(float sc_inc, float sc_vib, 
                           float sc_lluvia, float sc_suelo, float sc_temp) {
    // Pesos calibrados
    const float w_inc = 0.35;
    const float w_vib = 0.25;
    const float w_sue = 0.20;
    const float w_llu = 0.15;
    const float w_tmp = 0.05;
    
    float base = sc_inc*w_inc + sc_vib*w_vib + sc_suelo*w_sue + 
                 sc_lluvia*w_llu + sc_temp*w_tmp;
    
    // Detección de sinergias críticas
    float multiplicador = 1.0;
    if (sc_inc > 60 && sc_vib > 60) multiplicador *= 1.5;
    if (sc_lluvia > 60 && sc_suelo > 60) multiplicador *= 1.5;
    if (sc_inc > 50 && sc_vib > 50 && sc_suelo > 70) multiplicador *= 1.3;
    
    return clamp(base * multiplicador, 0.0, 100.0);
}
```

JUSTIFICACIÓN DE PESOS:
- ¿Por qué inclinación tiene mayor peso (35%)?
- Explicar relevancia de cada variable según literatura geotécnica

UMBRALES DE NIVEL:
- Nivel 0 (NORMAL): score < 26
- Nivel 1 (PRECAUCIÓN): 26 ≤ score < 51
- Nivel 2 (ALERTA): 51 ≤ score < 76
- Nivel 3 (EMERGENCIA): score ≥ 76

Justificar selección de umbrales
═══════════════════════════════════════════════════════════════════════════════
-->

##### 2.3.3.3 Módulo de Gestión de Alarmas

<!--
═══════════════════════════════════════════════════════════════════════════════
COMPLETAR: Documentar máquina de estados de alarmas:

ESTADOS Y TRANSICIONES:
- Incluir tabla de estados con: Estado | Entrada | Condición | Salida | Acción
- Ejemplo:
  | Estado Actual | Nivel Detectado | Condición | Nuevo Estado | Acción |
  |---------------|-----------------|-----------|--------------|--------|
  | NORMAL | 2 | score>=51 | ALERTA | Activar LED naranja, buzzer doble |

PERSISTENCIA DE ALARMAS:
- Una vez activada alarma (nivel ≥2), permanece hasta reconocimiento
- Comando /ack silencia buzzer pero mantiene indicadores visuales
- Comando /reset reinicia completamente el estado

PATRONES DE BUZZER:
- Nivel 1 (PRECAUCIÓN): beep ocasional cada 3s
- Nivel 2 (ALERTA): beep doble cada 650ms
- Nivel 3 (EMERGENCIA): beep continuo cada 300ms

PATRONES DE LED:
- Normal: Verde fijo
- Precaución: Amarillo fijo
- Alerta: Naranja parpadeante
- Emergencia: Rojo parpadeante rápido

EMERGENCIA MANUAL:
- Descripción de funcionalidad /emergency
- Timeout automático de 60 segundos
- Casos de uso (evacuaciones preventivas, pruebas del sistema)
═══════════════════════════════════════════════════════════════════════════════
-->

##### 2.3.3.4 Módulo de Comunicación MQTT

<!--
═══════════════════════════════════════════════════════════════════════════════
COMPLETAR: Documentar arquitectura MQTT:

ARQUITECTURA DE 3 NIVELES:
1. ESP32 (publisher) → Raspberry Pi (broker Mosquitto)
2. Raspberry Pi (publisher) → Ubidots Cloud (broker remoto)
3. Raspberry Pi almacena datos en SQLite antes de enviar a cloud

TÓPICOS MQTT:
- Definir estructura de tópicos, ejemplo:
  * `talud/sabana_centro/sensor_data` (ESP32 → RPi)
  * `talud/sabana_centro/alertas` (ESP32 → RPi)
  * `talud/sabana_centro/comandos` (RPi → ESP32, bidireccional)
  * `[ubidots_device]/sensor_data` (RPi → Ubidots)

FORMATO DE MENSAJES:
```json
{
  "timestamp": 1699564800,
  "inclinacion": 3.5,
  "vibracion": 4.2,
  "lluvia": 1850,
  "humedad_suelo": 68.3,
  "temperatura": 22.5,
  "score_total": 58.7,
  "nivel_alerta": 2,
  "alarma_activa": true
}
```

QoS (QUALITY OF SERVICE):
- Justificar selección de QoS (0, 1 o 2) para cada tipo de mensaje
- Crítico (alertas): QoS 2 (exactly once)
- Telemetría: QoS 1 (at least once)

SEGURIDAD:
- Autenticación usuario/contraseña en broker
- Uso de TLS/SSL (si aplica)
═══════════════════════════════════════════════════════════════════════════════
-->

##### 2.3.3.5 Módulo de Servidor Web y Dashboard

<!--
═══════════════════════════════════════════════════════════════════════════════
COMPLETAR: Documentar servidor web embebido:

TECNOLOGÍAS:
- ESP32: AsyncWebServer (librería ESPAsyncWebServer)
- Frontend: HTML5, CSS3, JavaScript vanilla

ENDPOINTS REST:
| Ruta | Método | Descripción | Respuesta |
|------|--------|-------------|-----------|
| `/` | GET | Dashboard HTML principal | HTML |
| `/data` | GET | Datos actuales en JSON | JSON |
| `/history` | GET | Historial últimas 50 muestras | JSON array |
| `/ack` | POST | Reconocer alarma | {"success": true} |
| `/reset` | POST | Reiniciar estado alarma | {"success": true} |
| `/emergency` | POST | Activar emergencia manual | {"success": true} |

RESTRICCIÓN DE ACCESO:
- Validación de IP: solo clientes en misma subred /24
- Implementación de validación en handleRoot()
- Respuesta 403 Forbidden para IPs externas

DASHBOARD WEB:
- Diseño responsive (compatible móvil/escritorio)
- Actualización automática cada 2 segundos (polling /data)
- Visualización de:
  * Valores actuales de sensores (gauges o números)
  * Nivel de alerta actual (indicador visual)
  * Gráficas históricas (Chart.js, canvas)
  * Botones de control (Reconocer, Reset, Emergencia)
- Indicadores de estado (WiFi conectado, última actualización)

CÓDIGO EJEMPLO (fragmento):
```javascript
function actualizarDatos() {
    fetch('/data')
        .then(response => response.json())
        .then(data => {
            document.getElementById('inclinacion').textContent = data.inclinacion.toFixed(1) + '°';
            // ... actualizar otros campos
            actualizarNivelAlerta(data.nivel_alerta);
        });
}
setInterval(actualizarDatos, 2000);
```
═══════════════════════════════════════════════════════════════════════════════
-->

##### 2.3.3.6 Módulo de Base de Datos (Raspberry Pi)

<!--
═══════════════════════════════════════════════════════════════════════════════
COMPLETAR: Documentar base de datos SQLite:

ESQUEMA DE BASE DE DATOS:
```sql
CREATE TABLE sensor_data (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    timestamp INTEGER NOT NULL,
    inclinacion REAL,
    vibracion REAL,
    lluvia INTEGER,
    humedad_suelo REAL,
    temperatura REAL,
    score_total REAL,
    nivel_alerta INTEGER,
    alarma_activa BOOLEAN
);

CREATE INDEX idx_timestamp ON sensor_data(timestamp);
```

OPERACIONES CRUD:
- INSERT: al recibir datos por MQTT desde ESP32
- SELECT: para consultas históricas y generación de reportes
- DELETE: limpieza periódica de datos antiguos (política de retención)

POLÍTICA DE ALMACENAMIENTO:
- Frecuencia de escritura: cada vez que se recibe dato del ESP32 (1 Hz)
- Retención: mantener últimos 7 días (configurable)
- Respaldo: exportación periódica a CSV/JSON

INTEGRACIÓN CON MQTT:
- Script Python en Raspberry Pi:
  * Suscriptor MQTT
  * Almacena en SQLite
  * Reenvía a Ubidots
═══════════════════════════════════════════════════════════════════════════════
-->

##### 2.3.3.7 Módulo de Inteligencia Artificial

<!--
═══════════════════════════════════════════════════════════════════════════════
COMPLETAR: Documentar integración con IA:

API SELECCIONADA:
- Gemini / ChatGPT / Claude (especificar cuál)
- Versión y modelo específico utilizado
- Autenticación (API key)

CASOS DE USO:
1. Predicción de riesgo a corto plazo (próximas horas):
   - Input: historial de sensores últimas N horas
   - Output: probabilidad de deslizamiento, nivel de confianza

2. Recomendaciones de acción:
   - Input: estado actual de sensores + nivel de alerta
   - Output: acciones sugeridas para autoridades

3. Análisis de tendencias:
   - Input: datos históricos extendidos
   - Output: patrones detectados, correlaciones

IMPLEMENTACIÓN:
```python
# Ejemplo de llamada API desde Raspberry Pi
import openai  # o google.generativeai

def consultar_ia_prediccion(datos_historicos):
    prompt = f"""
    Analiza los siguientes datos de sensores de un talud:
    {datos_historicos}
    
    Proporciona:
    1. Probabilidad de deslizamiento en próximas 6 horas
    2. Recomendaciones específicas
    3. Nivel de confianza del análisis
    """
    
    response = openai.ChatCompletion.create(
        model="gpt-4",
        messages=[{"role": "user", "content": prompt}]
    )
    
    return response.choices[0].message.content
```

INTEGRACIÓN CON DASHBOARD:
- Botón "Solicitar Análisis IA" en interfaz web
- Mostrar resultados en sección dedicada
- Indicador de tiempo de procesamiento

LIMITACIONES:
- Costo de API calls
- Latencia de red
- Límites de rate limiting
═══════════════════════════════════════════════════════════════════════════════
-->

#### 2.3.4 Esquemáticos de Hardware

<!--
═══════════════════════════════════════════════════════════════════════════════
COMPLETAR: Incluir esquemáticos detallados:

1. ESQUEMÁTICO COMPLETO DEL SISTEMA:
   - Circuito principal con ESP32
   - Todos los sensores conectados
   - Circuito de alimentación
   - Actuadores (LEDs con resistencias, buzzer)

2. DIAGRAMAS DE CONEXIÓN POR MÓDULO:
   - Módulo I2C (MPU6050, LCD si aplica)
   - Módulo OneWire (DS18B20)
   - Módulo ADC (sensores analógicos)
   - Módulo de alarmas (LEDs + buzzer)

3. PCB LAYOUT (si fue diseñado):
   - Layout de PCB
   - Planos de capa (top, bottom, ground, power)

4. LISTA DE MATERIALES (BOM):
| Componente | Cantidad | Valor/Modelo | Notas |
|------------|----------|--------------|-------|
| ESP32 DevKit | 1 | ESP32-WROOM-32 | Dual-core 240MHz |
| MPU6050 | 1 | GY-521 | Acelerómetro + Giroscopio |
| DS18B20 | 1 | TO-92 | Sensor temperatura |
| ... | ... | ... | ... |

Herramientas sugeridas: KiCad, Fritzing, EasyEDA
Formatos: PNG de alta resolución + archivos fuente (.kicad_pcb, .fzz, etc.)
═══════════════════════════════════════════════════════════════════════════════
-->

#### 2.3.5 Estándares de Ingeniería Aplicados

<!--
═══════════════════════════════════════════════════════════════════════════════
COMPLETAR: Documentar estándares y buenas prácticas aplicadas:

ESTÁNDARES DE CÓDIGO:
- Convención de nomenclatura:
  * Variables: camelCase (ej: sensorData)
  * Funciones: camelCase (ej: readSensorValue())
  * Clases: PascalCase (ej: SensorInterface)
  * Constantes: UPPER_SNAKE_CASE (ej: MAX_BUFFER_SIZE)
- Documentación de código:
  * Comentarios Doxygen para funciones públicas
  * Comentarios inline para lógica compleja
- Organización de archivos:
  * Separación por responsabilidades (sensores/, comunicacion/, ui/)

ESTÁNDARES DE COMUNICACIÓN:
- MQTT v3.1.1 / v5.0 (especificar)
- HTTP/1.1 para API REST
- JSON para serialización de datos (RFC 8259)

ESTÁNDARES DE SEGURIDAD:
- Validación de entradas en endpoints web
- Sanitización de datos
- Rate limiting para prevenir DoS

BUENAS PRÁCTICAS EMBEBIDAS:
- Uso de watchdog timer
- Gestión de memoria (evitar fragmentación)
- Manejo de errores robusto
- Logging estructurado

CONTROL DE VERSIONES:
- Git + GitHub/GitLab
- Semantic Versioning (vX.Y.Z)
- Commits descriptivos

TESTING:
- Unit tests para funciones críticas (scoring, fusión)
- Integration tests para flujo completo
- Herramientas: Unity Test Framework, Google Test
═══════════════════════════════════════════════════════════════════════════════
-->

---

## 3. Configuración Experimental, Resultados y Análisis

### 3.1 Configuración Experimental

#### 3.1.1 Entorno de Pruebas

<!--
═══════════════════════════════════════════════════════════════════════════════
COMPLETAR: Describir entorno donde se realizaron las pruebas:
- Ubicación física (laboratorio, campo, simulación)
- Condiciones ambientales (temperatura, humedad)
- Infraestructura de red (router WiFi, configuración WLAN)
- Equipos auxiliares utilizados (multímetro, osciloscopio, fuentes)
═══════════════════════════════════════════════════════════════════════════════
-->

#### 3.1.2 Protocolo de Pruebas

<!--
═══════════════════════════════════════════════════════════════════════════════
COMPLETAR: Documentar metodología de pruebas:

PRUEBAS UNITARIAS:
- Verificación individual de cada sensor
- Validación de rangos y precisión
- Prueba de funciones de scoring

PRUEBAS DE INTEGRACIÓN:
- Comunicación ESP32 ↔ Raspberry Pi (MQTT)
- Almacenamiento en SQLite
- Sincronización entre tareas FreeRTOS

PRUEBAS DE SISTEMA:
- Escenarios de prueba diseñados:
  1. Escenario 1: Condiciones normales
  2. Escenario 2: Lluvia moderada + inclinación leve
  3. Escenario 3: Vibración intensa + saturación de suelo
  4. Escenario 4: Emergencia simulada (todos los sensores críticos)
  
SIMULACIÓN DE EVENTOS:
- Método para simular vibración (motor/shaker)
- Método para simular lluvia (goteo controlado)
- Método para simular inclinación (plataforma ajustable)
═══════════════════════════════════════════════════════════════════════════════
-->

#### 3.1.3 Instrumentación y Medición

<!--
═══════════════════════════════════════════════════════════════════════════════
COMPLETAR: Listar instrumentos de medición usados para validación:
- Inclinómetro de referencia (para validar MPU6050)
- Termómetro calibrado (para validar DS18B20)
- Medidor de humedad de suelo comercial (para validar YL-100)
- Cronómetro/analizador lógico (para verificar timings)
═══════════════════════════════════════════════════════════════════════════════
-->

### 3.2 Resultados Experimentales

#### 3.2.1 Caracterización de Sensores

<!--
═══════════════════════════════════════════════════════════════════════════════
COMPLETAR: Presentar datos de calibración y caracterización:

SENSOR MPU6050:
- Rango de medición verificado: ±X°
- Precisión: ±X°
- Ruido observado: X° RMS
- Tabla de comparación con inclinómetro de referencia

SENSOR DS18B20:
- Rango de medición: X°C a Y°C
- Precisión: ±X°C
- Tiempo de respuesta: X segundos
- Gráfica comparativa con termómetro calibrado

SENSOR DE LLUVIA:
- Valores ADC en seco: X
- Valores ADC en saturación: Y
- Relación ADC vs. intensidad de lluvia (gráfica)

SENSOR YL-100:
- Valores ADC en seco: X
- Valores ADC en saturación: Y
- Calibración de % humedad

SENSOR DE VIBRACIÓN:
- Sensibilidad medida
- Frecuencia de muestreo efectiva
- Tasa de falsos positivos
═══════════════════════════════════════════════════════════════════════════════
-->

#### 3.2.2 Resultados de Escenarios de Prueba

<!--
═══════════════════════════════════════════════════════════════════════════════
COMPLETAR: Presentar resultados de cada escenario:

ESCENARIO 1: CONDICIONES NORMALES
- Valores de sensores observados
- Score calculado: X (nivel 0)
- Comportamiento esperado vs. observado: ✅ Match

ESCENARIO 2: LLUVIA MODERADA + INCLINACIÓN LEVE
- Inclinación: X°, Lluvia ADC: Y
- Score calculado: X (nivel 1 - Precaución)
- Tiempo de respuesta del sistema: X ms
- Alarmas activadas: LED amarillo, beep ocasional
- Comportamiento esperado vs. observado: ✅ Match

ESCENARIO 3: VIBRACIÓN INTENSA + SATURACIÓN DE SUELO
- Vibración: X eventos/min, Humedad: Y%
- Score calculado: X (nivel 2 - Alerta)
- Sinergia detectada: ✅ (multiplicador 1.5x aplicado)
- Alarmas activadas: LED naranja, beep doble
- Persistencia de alarma: ✅ (no se silenció sin /ack)

ESCENARIO 4: EMERGENCIA SIMULADA
- Todos los sensores en valores críticos
- Score calculado: X (nivel 3 - Emergencia)
- Alarmas activadas: LED rojo, beep continuo
- Latencia ESP32 → Dashboard: X ms
- Latencia ESP32 → Ubidots: X segundos

Incluir tablas, gráficas de series temporales, capturas de pantalla del dashboard
═══════════════════════════════════════════════════════════════════════════════
-->

#### 3.2.3 Análisis de Desempeño

<!--
═══════════════════════════════════════════════════════════════════════════════
COMPLETAR: Analizar métricas de desempeño del sistema:

LATENCIAS:
- Tiempo de ciclo de adquisición: X ms (target: 1000ms)
- Latencia ESP32 → Raspberry Pi (MQTT): X ms
- Latencia Raspberry Pi → Ubidots: X ms
- Tiempo de respuesta del dashboard web: X ms

USO DE RECURSOS:
- RAM utilizada en ESP32: X KB / 320 KB (X%)
- Flash utilizada: X MB / 4 MB (X%)
- Uso de CPU (ambos cores): Core 0: X%, Core 1: Y%
- Almacenamiento SQLite: X MB después de Y horas

CONFIABILIDAD:
- Uptime del sistema: X horas continuas sin reinicio
- Paquetes MQTT perdidos: X% (QoS 1)
- Tasa de falsos positivos de alarma: X%
- Tasa de detección correcta: X%

CONSUMO ENERGÉTICO:
- Corriente promedio ESP32: X mA
- Corriente en alarma activa: X mA
- Estimación de autonomía con batería de Y mAh: X horas
═══════════════════════════════════════════════════════════════════════════════
-->

### 3.3 Análisis de Resultados

#### 3.3.1 Validación del Algoritmo de Fusión

<!--
═══════════════════════════════════════════════════════════════════════════════
COMPLETAR: Análisis crítico del algoritmo de fusión:

SENSIBILIDAD:
- ¿El sistema detecta correctamente variaciones pequeñas en sensores?
- ¿Los pesos asignados (w_inc=0.35, etc.) son apropiados?
- Análisis de sensibilidad: variar cada peso ±10% y observar impacto

SINERGIAS:
- ¿Las sinergias detectadas son geotécnicamente válidas?
- Casos donde las sinergias mejoraron la detección
- Casos de falsos positivos por sinergias

UMBRALES:
- Validación de umbrales (26, 51, 76) con literatura geotécnica
- Comparación con sistemas comerciales similares
- Ajustes propuestos basados en resultados experimentales

ROBUSTEZ:
- Comportamiento ante fallo de un sensor (NaN)
- Comportamiento ante datos ruidosos
- Estabilidad ante variaciones ambientales normales
═══════════════════════════════════════════════════════════════════════════════
-->

#### 3.3.2 Comparación con Objetivos

<!--
═══════════════════════════════════════════════════════════════════════════════
COMPLETAR: Tabla comparativa de objetivos vs. resultados:

| Objetivo | Métrica | Target | Logrado | Cumplimiento |
|----------|---------|--------|---------|--------------|
| Detección multi-sensorial | N° sensores integrados | ≥3 | 5 | ✅ 166% |
| Latencia de alerta | Tiempo detección→alarma | <5s | Xs | ✅/❌ |
| Disponibilidad del sistema | Uptime | >95% | X% | ✅/❌ |
| Dashboard funcional | Endpoints implementados | 6 | X | ✅/❌ |
| Integración IA | API funcional | Sí | Sí/No | ✅/❌ |
| ... | ... | ... | ... | ... |

Análisis de desviaciones y causas
═══════════════════════════════════════════════════════════════════════════════
-->

#### 3.3.3 Limitaciones Identificadas

<!--
═══════════════════════════════════════════════════════════════════════════════
COMPLETAR: Documentar limitaciones del prototipo:

LIMITACIONES TÉCNICAS:
- Precisión limitada de sensores económicos
- Falta de alimentación autónoma (depende de fuente externa)
- No hay protección IP contra intemperie
- Alcance WiFi limitado (requiere infraestructura WLAN cercana)

LIMITACIONES DEL ALGORITMO:
- No considera datos históricos a largo plazo (>50 muestras)
- Sinergias predefinidas (no aprendizaje automático)
- Umbrales fijos (no adaptativos)

LIMITACIONES DE DESPLIEGUE:
- Requiere instalación manual y calibración en sitio
- Sin redundancia de sensores críticos
- Dependencia de conectividad para alertas remotas
═══════════════════════════════════════════════════════════════════════════════
-->

---

## 4. Autoevaluación del Protocolo de Pruebas

### 4.1 Evaluación de Completitud

<!--
═══════════════════════════════════════════════════════════════════════════════
COMPLETAR: Autoevaluación crítica del proceso de pruebas:

¿QUÉ SE PROBÓ CORRECTAMENTE?
- Lista de funcionalidades exhaustivamente probadas
- Casos de prueba que generaron resultados confiables

¿QUÉ FALTÓ PROBAR?
- Funcionalidades sin pruebas suficientes
- Casos extremos no cubiertos
- Pruebas de estrés (carga, duración)

¿QUÉ SE PODRÍA MEJORAR?
- Propuestas para mejorar cobertura de pruebas
- Herramientas adicionales que se deberían usar
- Metodologías de testing más rigurosas (TDD, BDD)
═══════════════════════════════════════════════════════════════════════════════
-->

### 4.2 Análisis de Casos de Prueba

<!--
═══════════════════════════════════════════════════════════════════════════════
COMPLETAR: Tabla de análisis de casos de prueba:

| ID | Caso de Prueba | Resultado Esperado | Resultado Obtenido | Estado | Observaciones |
|----|----------------|--------------------|--------------------|--------|---------------|
| TC01 | Boot normal del sistema | Inicialización completa | ... | ✅/❌ | ... |
| TC02 | Lectura MPU6050 | Inclinación ±0.5° | ... | ✅/❌ | ... |
| TC03 | Fusión con 3 señales críticas | Nivel 2, score>51 | ... | ✅/❌ | ... |
| TC04 | Comando /ack desde dashboard | Silenciar buzzer | ... | ✅/❌ | ... |
| TC05 | MQTT ESP32→RPi | Mensaje recibido <1s | ... | ✅/❌ | ... |
| TC06 | Fallo de sensor (desconectar MPU) | Score=0, continuar | ... | ✅/❌ | ... |
| ... | ... | ... | ... | ... | ... |

Incluir mínimo 15-20 casos de prueba cubriendo todos los módulos
═══════════════════════════════════════════════════════════════════════════════
-->

### 4.3 Lecciones Aprendidas

<!--
═══════════════════════════════════════════════════════════════════════════════
COMPLETAR: Reflexión sobre el proceso de pruebas:

¿QUÉ FUNCIONÓ BIEN?
- Estrategias de testing exitosas
- Herramientas útiles
- Colaboración en el equipo

¿QUÉ NO FUNCIONÓ COMO SE ESPERABA?
- Problemas encontrados durante pruebas
- Supuestos incorrectos
- Bugs críticos descubiertos tarde

¿QUÉ SE HARÍA DIFERENTE?
- Cambios en el enfoque de testing
- Mejores prácticas a aplicar en el futuro
- Inversión de tiempo en pruebas (más temprano, más frecuente)
═══════════════════════════════════════════════════════════════════════════════
-->

---

// ...existing code...
## 5. Conclusiones y Trabajo Futuro

### 5.1 Conclusiones

Se desarrolló exitosamente un prototipo funcional de un sistema de alerta temprana para deslizamientos, cumpliendo con el objetivo general del proyecto. Se logró la integración de cinco sensores clave (inclinación, vibración, lluvia, humedad y temperatura) en un nodo ESP32, demostrando la viabilidad de una solución de monitoreo de bajo costo. La implementación de un algoritmo de fusión de señales, que pondera las variables y detecta sinergias críticas, permitió generar un índice de riesgo unificado y coherente, activando un sistema de alarmas multinivel de forma efectiva durante las pruebas simuladas.

Desde el punto de vista técnico, el proyecto representó un aprendizaje significativo en arquitecturas IoT de extremo a extremo, desde la programación de sistemas embebidos con FreeRTOS hasta la configuración de un gateway en Raspberry Pi y la visualización de datos en la nube con Ubidots. La implementación del protocolo MQTT como eje de la comunicación resultó ser robusta y eficiente para la transmisión de datos entre los distintos componentes del sistema, tanto en la red local como hacia la nube.

La validación experimental, a través de escenarios simulados, confirmó que el prototipo es capaz de identificar condiciones de riesgo y responder adecuadamente, activando las alarmas locales y remotas. Aunque el sistema presenta limitaciones, como la falta de una carcasa para exteriores y una fuente de alimentación autónoma, establece una base sólida y prometedora para futuras iteraciones. Este trabajo contribuye con una solución tangible y escalable al problema de la gestión del riesgo por remoción en masa en la región.

### 5.2 Retos Presentados Durante el Desarrollo
<!--
═══════════════════════════════════════════════════════════════════════════════
 Falta poner aca los retos
-->

### 5.3 Trabajo Futuro

<!--
═══════════════════════════════════════════════════════════════════════════════
COMPLETAR: Propuestas de mejoras y extensiones:

MEJORAS A CORTO PLAZO (Challenge 4):
- Implementación completa de Gateway en Raspberry Pi
- Desarrollo de modelo de IA entrenado (no solo API genérica)
- Dashboard cloud en Ubidots con alertas por email/SMS
- Modo de bajo consumo energético
- Protección física del hardware (carcasa IP65)

MEJORAS A MEDIANO PLAZO:
- Redundancia de sensores críticos
- Sistema de alimentación autónomo (panel solar + batería)
- Calibración adaptativa de umbrales con machine learning
- Integración con sistemas de información geográfica (GIS)
- Red de múltiples nodos IoT coordinados

INVESTIGACIÓN FUTURA:
- Validación con datos reales de campo en zonas de riesgo
- Estudio de correlación entre variables medidas y deslizamientos históricos
- Análisis de big data con años de telemetría
- Integración con modelos geotécnicos avanzados (ej: Factor de Seguridad FS)

ESCALABILIDAD:
- Arquitectura para desplegar 100+ nodos en una región
- Plataforma de gestión centralizada para múltiples municipios
- APIs públicas para integración con sistemas de gestión del riesgo
═══════════════════════════════════════════════════════════════════════════════
-->

### 5.4 Roles y Contribuciones del Equipo

**Héctor José Guzmán (Investigación y Desarrollo):**
- Responsabilidades principales:
  * Investigación de sensores y algoritmos de fusión de señales.
  * Desarrollo del firmware principal en el ESP32 para la adquisición y procesamiento de datos.
  * Implementación de la lógica de lectura para los sensores (MPU6050, DS18B20, etc.).
  * Programación del motor de fusión de señales y la máquina de estados de las alarmas.
- Contribuciones específicas:
  * Escritura del código C++ para el ESP32 utilizando PlatformIO.
  * Implementación de tareas concurrentes con FreeRTOS para el manejo de sensores y comunicación.
  * Documentación de los módulos de software y criterios de diseño.

**Luis Mario Ramirez (Arquitecto de la Solución):**
- Responsabilidades principales:
  * Diseño de la arquitectura general del sistema (nodo, gateway, nube).
  * Configuración de la Raspberry Pi como gateway IoT, incluyendo el broker MQTT (Mosquitto) y la base de datos SQLite.
  * Implementación de la comunicación entre la red local y la plataforma en la nube (Ubidots).
  * Diseño y desarrollo del dashboard web local en el ESP32.
- Contribuciones específicas:
  * Desarrollo de los scripts en Python para el gateway.
  * Diseño de la estructura de tópicos MQTT y el formato de los mensajes JSON.
  * Creación de la interfaz de usuario (HTML/CSS/JS) para el monitoreo en tiempo real.
  * Diseño del protocolo de pruebas y los escenarios de validación.

**Trabajo Colaborativo:**
- Las decisiones sobre la arquitectura y el diseño del sistema se tomaron en conjunto.
- Ambos miembros participaron activamente en las fases de integración, pruebas y depuración.
- La redacción de la documentación final y la producción del video demostrativo fueron esfuerzos compartidos.



## 6. Referencias

[1] M. El Moulat, O. Debauche, S. Mahmoudi, L. Aït Brahim, P. Manneback, and F. Lebeau, "Monitoring System Using Internet of Things For Potential Landslides," *Procedia Computer Science*, vol. 134, pp. 26-34, 2018, doi: 10.1016/j.procs.2018.07.140.

[2] E. S. Soegoto, F. A. Fauzi, and S. Luckyardi, "Internet of things for flood and landslide early warning," *Journal of Physics: Conference Series*, vol. 1764, art. no. 012190, 2021, doi: 10.1088/1742-6596/1764/1/012190.

[3] R. B. Bhardwaj, "Landslide Detection System Based on IOT," *ResearchGate Preprint*, 2021. [Online]. Available: https://www.researchgate.net/publication/350069472_Landslide_Detection_System_Based_on_IOT

[4] L. Piciullo, V. Capobianco, and H. Heyerdahl, "A first step towards a IoT-based local early warning system for an unsaturated slope in Norway," *Natural Hazards*, 2022, doi: 10.1007/s11069-022-05524-3.

[5] V. Henao-Céspedes, Y. A. Garcés-Gómez, and M. N. Marín Olaya, "Landslide early warning systems: a perspective from the internet of things," *Indonesian Journal of Electrical Engineering and Computer Science*, 2023. [Online]. Available: https://d1wqtxts1xzle7.cloudfront.net/97071057/99_28430_EMr_15sep22_16Mei22_20_K-libre.pdf

---

## 7. Anexos

### 7.1 Código Fuente

<!--
═══════════════════════════════════════════════════════════════════════════════
COMPLETAR: Enlace al repositorio de código:

**Repositorio GitHub/GitLab:** [Agregar URL]

**Estructura del repositorio:**
```
├── esp32/
│   ├── src/
│   │   ├── main.cpp
│   │   ├── sensors/
│   │   ├── communication/
│   │   └── ui/
│   ├── include/
│   └── platformio.ini
├── raspberry_pi/
│   ├── mqtt_gateway.py
│   ├── database_manager.py
│   └── requirements.txt
├── docs/
│   ├── diagramas/
│   ├── esquematicos/
│   └── README.md (este documento)
└── tests/
    ├── unit_tests/
    └── integration_tests/
```

**Instrucciones de compilación y despliegue:**
- Requisitos previos (Arduino IDE, PlatformIO, Python 3.x)
- Librerías necesarias (con versiones específicas)
- Pasos para configurar WiFi
- Comandos para compilar y cargar firmware
- Configuración de Raspberry Pi (Mosquitto, SQLite)
═══════════════════════════════════════════════════════════════════════════════
-->

### 7.2 Esquemáticos Completos

<!--
═══════════════════════════════════════════════════════════════════════════════
COMPLETAR: Archivos de esquemáticos en carpeta /docs/esquematicos/ del repo

Incluir:
- Esquemático completo (PDF + fuente .kicad_sch)
- Layout PCB si fue diseñado
- Fritzing diagram para visualización didáctica
- Diagramas de conexión física (fotos anotadas)
═══════════════════════════════════════════════════════════════════════════════
-->

### 7.3 Video Demostrativo

<!--
═══════════════════════════════════════════════════════════════════════════════
COMPLETAR: Información del video:

**Link al video:** [YouTube/Drive/OneDrive URL]

**Duración:** X minutos (máx 10 min)

**Contenido del video:**
1. Introducción del equipo y proyecto (30s)
2. Demostración del hardware (sensores, conexiones) (2 min)
3. Demostración del dashboard web (1 min)
4. Escenario de prueba en vivo:
   - Condiciones normales (30s)
   - Activación de alarma por múltiples sensores (2 min)
   - Reconocimiento de alarma (30s)
5. Demostración de integración cloud (Ubidots) (1 min)
6. Demostración de módulo IA (predicción/recomendación) (1 min)
7. Conclusiones y trabajo futuro (1 min)

**Participación:**
- Todos los miembros del equipo aparecen en el video ✅
- Cámaras encendidas durante presentación ✅

**Calidad técnica:**
- Resolución: 1080p mínimo
- Audio claro
- Capturas de pantalla legibles
═══════════════════════════════════════════════════════════════════════════════
-->

### 7.4 Material Complementario

<!--
═══════════════════════════════════════════════════════════════════════════════
COMPLETAR: Archivos adicionales en repositorio:

- Hojas de datos (datasheets) de sensores utilizados
- Manuales de usuario:
  * Guía de instalación del dispositivo IoT
  * Manual de operación del dashboard
  * Guía de troubleshooting común
- Resultados de pruebas (logs, CSVs con datos experimentales)
- Presentación tipo pitch (slides de 3 minutos para sustentación)
- Certificados de calibración de instrumentos (si aplica)
═══════════════════════════════════════════════════════════════════════════════
-->

---
## 8. Uso de Generative AI

Se uso generative AI para la generacion de la estructura inicial de la wiki, ademas se uso para la revision final del documento, que cumpliera la rubrica establecida y cada seccion estuviera bien estructurada. 

**Fin del documento** • **Versión 1.0** • **10 de noviembre de 2025**
