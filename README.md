Link de Video: https://utmachalaeduec-my.sharepoint.com/:v:/g/personal/razuero2_utmachala_edu_ec/EYFGbTmFb_tKgdkEwIu6cb8BstCNq1tzNAG5p8-1mkBHag?nav=eyJyZWZlcnJhbEluZm8iOnsicmVmZXJyYWxBcHAiOiJPbmVEcml2ZUZvckJ1c2luZXNzIiwicmVmZXJyYWxBcHBQbGF0Zm9ybSI6IldlYiIsInJlZmVycmFsTW9kZSI6InZpZXciLCJyZWZlcnJhbFZpZXciOiJNeUZpbGVzTGlua0NvcHkifX0&e=2EgXEp
-----

# 🌿 Huerto Urbano Inteligente

-----

## 💡 Descripción del Proyecto

Este proyecto busca transformar un huerto urbano tradicional en un sistema inteligente y automatizado, optimizando el crecimiento de las plantas y minimizando la intervención manual. Utilizando una combinación de sensores ambientales, actuadores controlados y una plataforma de visualización, el sistema monitorea y gestiona las condiciones críticas como la temperatura, humedad del aire, pH del suelo, y activa automáticamente el riego, la sombra y la ventilación según sea necesario.

El corazón del sistema es una implementación en **Node-RED** que integra datos de **OpenWeatherMap** para el clima externo, simulación de **pH del suelo**, y controla actuadores a través de **MQTT**. Todo esto se visualiza en un **dashboard interactivo** y envía **notificaciones en tiempo real vía Telegram**.

-----

## ✨ Características Principales

  * **Monitoreo Ambiental:**
      * **Temperatura y Humedad (OpenWeatherMap):** Consulta periódica de las condiciones climáticas externas de tu ubicación específica (Machala, Ecuador en este caso).
      * **pH del Suelo:** Simulación de datos de pH para monitorear la acidez/alcalinidad del suelo. (Puede ser adaptado a un sensor real).
  * **Automatización Inteligente:**
      * **Riego Automático:** Activación inteligente del sistema de riego basada en umbrales de temperatura y humedad ambiental (ej. alta temperatura o baja humedad).
      * **Control de Sombra:** Despliegue automático de sombra en respuesta a altas temperaturas para proteger los cultivos.
      * **Ventilación Automática:** Activación de ventiladores para disipar el calor cuando la temperatura excede ciertos límites.
  * **Notificaciones en Tiempo Real:**
      * **Telegram:** Recibe alertas instantáneas en tu dispositivo móvil cada vez que una acción de automatización (riego, sombra, ventilador) es activada.
  * **Dashboard Interactivo (Node-RED Dashboard):**
      * Visualización clara de todos los datos de los sensores en tiempo real.
      * Controles manuales intuitivos (toggle switches) para cada actuador (riego, sombra, ventilador).
      * Calendario de cultivos para planificación.
      * Visor de cámara IP integrado para monitoreo visual del huerto (con relación de aspecto 16:9).
      * Historial de datos de sensores y actuadores con función de exportación a CSV.
  * **Tecnologías Utilizadas:**
      * **Node-RED:** Plataforma de programación visual para el flujo de datos.
      * **MQTT (Broker: HiveMQ):** Protocolo de mensajería ligero para la comunicación con actuadores.
      * **OpenWeatherMap API:** Fuente de datos climáticos.
      * **Telegram Bot API:** Para notificaciones.
      * **HTML, CSS, JavaScript (UI Template):** Para un dashboard personalizado y estético.
      * **Font Awesome:** Iconografía.
      * **Google Fonts (Poppins, Inter):** Tipografías modernas.

-----

## ⚙️ Estructura del Flujo de Node-RED

El proyecto se compone de un único flujo principal en Node-RED que orquesta todas las funcionalidades:

  * **`Inject Nodes`:** Disparadores para consulta de clima, simulación de pH y generación de resumen diario.
  * **`HTTP Request Node`:** Para consultar la API de OpenWeatherMap.
  * **`Random Node`:** Simulación de datos de pH del suelo.
  * **`Function Nodes`:**
      * **`Procesar Datos del Clima y Guardar Historial`:** Parsear la respuesta de OpenWeatherMap y almacenar datos en el contexto de flujo.
      * **`Lógica de Automatización (5 Salidas)`:** Contiene la lógica de decisión para riego, sombra y ventilación, generando comandos y notificaciones.
      * **`Ensamblar Datos para Dashboard`:** Consolida todos los datos de sensores y estados de actuadores para ser mostrados en el UI.
      * **`Calcular Resumen Diario`:** Genera un resumen de las condiciones promedio y el estado de los actuadores del día.
  * **`MQTT Out Nodes`:** Para enviar comandos ON/OFF a los actuadores (ej. `huerto/actuador/riego/set`).
  * **`MQTT In Nodes`:** Para recibir el estado actual de los actuadores (ej. `huerto/actuador/riego/status`).
  * **`Change Nodes`:** Para manipular tópicos y guardar datos en el contexto del flujo.
  * **`Telegrambot-Notify Node`:** Para enviar mensajes de notificación.
  * **`UI Template Nodes`:**
      * **`Dashboard Principal (UI)`:** El dashboard principal personalizado que muestra todos los datos y la cámara IP.
      * **`Visualización Temperatura`**, **`Visualización Humedad`**, **`Visualización Clima`:** Tarjetas individuales para métricas climáticas.
      * **`Control Riego`**, **`Control Sombra`**, **`Control Ventilador`:** Componentes de `toggle switch` para control manual de actuadores.
      * **`Resumen Diario (Tabla UI)`:** Tabla que muestra el resumen diario de datos.
  * **`UI Group` & `UI Tab` Nodes:** Organizan el dashboard de Node-RED.

-----

## 🚀 Instalación y Configuración

Sigue estos pasos para poner en marcha el proyecto en tu instancia de Node-RED:

1.  **Instala Node-RED:** Si no lo tienes, sigue las instrucciones oficiales en [Node-RED.org](https://nodered.org/docs/getting-started/).
2.  **Instala Nodos Adicionales:**
    Abre la paleta de Node-RED (menú `≡` -\> `Manage palette` -\> `Install`) e instala los siguientes nodos:
      * `node-red-dashboard`
      * `node-red-contrib-telegrambot`
3.  **Configura tu Bot de Telegram:**
      * Crea un nuevo bot con `@BotFather` en Telegram y obtén tu **HTTP API Token**.
      * Inicia un chat con tu nuevo bot (envíale cualquier mensaje como "Hola bot").
      * En tu navegador, ve a `https://api.telegram.org/botYOUR_BOT_TOKEN/getUpdates` (reemplaza `YOUR_BOT_TOKEN`) para obtener tu **`chat ID`**.
4.  **Importa el Flujo de Node-RED:**
      * Copia el contenido del archivo `flow.json` (que sería el JSON completo de tu flujo) de este repositorio.
      * En Node-RED, ve al menú `≡` -\> `Import` -\> `Clipboard`.
      * Pega el JSON copiado y haz clic en `Import`.
5.  **Configura los Nodos Específicos del Flujo:**
      * **`Telegrambot-Notify` (`Notificación Telegram`):**
          * Haz doble clic en el nodo.
          * Junto a "Bot", haz clic en el lápiz para añadir/editar la configuración del bot.
          * Ingresa tu **HTTP API Token** y tu **`chat ID`** (el número que obtuviste, sin comillas).
          * Haz clic en `Update`.
      * **`Function Node: Ensamblar Datos para Dashboard`:**
          * Asegúrate de que este nodo (y otros `change` nodes relevantes) esté configurado para guardar los estados de los actuadores (riego, sombra, ventilador) en el **contexto de flujo** (`flow.set('latestRiegoStatus', ...)`), ya que el nodo de resumen diario los lee desde allí.
      * **Nodos `MQTT Out`:**
          * Verifica que los tópicos `utmach/fic/6to/Proyecto/Iot/HuertoUrbano/riego`, `sombra` y `ventilador` sean los correctos para tus dispositivos. Considera usar `/command` y `/status` como buena práctica.
          * Asegúrate de que la configuración de tus brokers MQTT (`HiveMQ Broker 1` y `HiveMQ Broker 2`) apunten a `broker.hivemq.com` y puerto 1883.
6.  **Configura tu Cámara IP (Opcional):**
      * En el dashboard, en la sección de "Visualización Cámara IP", introduce la URL o IP de tu streaming de video (ej. `http://tu_camara_ip:puerto/stream`).
7.  **Implementa los Dispositivos Físicos:**
      * Prepara tus microcontroladores (ESP32/ESP8266) para que se suscriban a los tópicos MQTT de comando (`huerto/actuador/.../set`) y publiquen sus estados (`huerto/actuador/.../status`).
      * Conecta tus sensores y actuadores (bomba de agua, servo para sombra, ventilador) a los pines correspondientes de tus microcontroladores.
8.  **Deploy el Flujo:**
      * Haz clic en el botón `Deploy` en la esquina superior derecha de Node-RED.

-----

## 📊 Visualización del Dashboard

Accede al dashboard de Node-RED en `http://tu_ip_nodered:1880/ui`.

Aquí verás:

  * Un panel de "Estado Actual del Huerto" con temperatura, humedad, descripción del clima, pH del suelo, y el estado de riego, sombra y ventilador.
  * Controles tipo `toggle switch` para accionar manualmente el riego, la sombra y el ventilador.
  * Un "Calendario de Cultivos" para referencia.
  * La "Visualización Cámara IP" para monitoreo visual en tiempo real.
  * Un "Historial de Datos" de los últimos 30 registros, con opción a exportar a CSV.
  * Un "Resumen Diario" con la temperatura promedio y el estado general de los actuadores del día.

-----

## ⚠️ Consideraciones Importantes

  * **Umbrales de Automatización:** Los umbrales de temperatura y humedad en el nodo "Lógica de Automatización" (`temperatura > 20 || humedad < 70`, `temperatura > 30` para sombra, `temperatura > 28` para ventilador) son ejemplos. **Debes ajustarlos según las necesidades específicas de tus plantas y las condiciones climáticas de tu región.**
  * **pH del Suelo:** Actualmente, el pH se simula con un nodo `random`. Para un sistema real, necesitarías un sensor de pH adecuado y la lógica para interpretarlo.
  * **Horas de Sol:** El valor de "Horas de Sol" en el resumen diario es un estimado (`6h (estimado)`). Para un dato real, se requeriría un sensor de luz o una integración con una API que proporcione esa información.
  * **Seguridad MQTT:** Para despliegues en entornos de producción o con datos sensibles, se recomienda encarecidamente utilizar MQTT con autenticación (usuario/contraseña) y cifrado (TLS/SSL), en lugar de conexiones anónimas.
  * **Persistencia de Datos:** El historial de datos se almacena en el contexto del navegador (`scope.historial`) y se limita a 30 entradas. Para una persistencia a largo plazo, considera integrar una base de datos (ej. InfluxDB, SQLite) o un servicio de logging.

-----

## 🤝 Contribución

¡Las contribuciones son bienvenidas\! Si tienes ideas para mejorar este proyecto, como integrar nuevos sensores, lógicas de automatización más avanzadas, o mejoras en el dashboard, no dudes en abrir un `issue` o enviar un `pull request`.

-----

## 📄 Licencia

Este proyecto está bajo la Licencia MIT. Consulta el archivo [LICENSE](https://www.google.com/search?q=LICENSE) para más detalles.

-----

## 📧 Contacto

Para cualquier consulta o sugerencia, puedes contactarme a través de mi perfil de GitHub: [TuUsuarioGitHub](https://www.google.com/search?q=https://github.com/TuUsuarioGitHub)

-----
