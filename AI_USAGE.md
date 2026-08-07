# Bitácora de Co-Pilotaje e IA (AI_USAGE)

## 1. Herramientas Utilizadas

* **Asistente Principal:** Claude
* **Rol en el flujo de trabajo:** Co-piloto conversacional durante todo el desarrollo — desde la configuración del entorno hasta la generación de scripts, hasta la toma de decisiones de alcance (scope).

## 2. Casos de Uso Específicos

Mi enfoque de trabajo con la IA fue conversacional e iterativo: la usé como acelerador de tareas puntuales, no como reemplazo del trabajo de ingeniería, exploración y verificación, que hice yo directamente sobre mi máquina y el emulador.

**Lo que hice yo directamente:**
* Instalé y configuré todo el entorno (Java, Android Studio, emulador, variables de entorno, Maestro CLI, Postman, Newman).
* Exploré la app manualmente con Maestro Studio e identifiqué yo mismo cada selector (`id`, `text`) usado en los flujos.
* Detecté por mi cuenta que la app no valida credenciales incorrectas a nivel de negocio, probando manualmente distintas combinaciones antes de decidir cómo ajustar el caso de prueba.
* Ejecuté cada flujo de Maestro y cada request de Postman en mi entorno, y reporté los errores/comportamientos reales encontrados.
* Tomé la decisión final de no implementar el bonus de Kafka, priorizando la calidad de los entregables obligatorios dado mi plazo de 4 días.

**Lo que delegué a la IA como acelerador:**
* Guía paso a paso para configurar el entorno de automatización móvil en Windows, ya que a la fecha he realizado pruebas de automatización en paginas web y aplicaciones, pero no en el apartado mobile.
* Generación del boilerplate de los scripts de test en Postman (JavaScript) para status code.

## 3. Ejemplos de Prompts Clave

**Prompt 1: Destrabando la configuración del entorno móvil**
> *"Actúa como un Arquitecto de Pruebas. Tengo un background automatizando flujos funcionales en web, pero necesito tu asistencia para configurar desde cero mi entorno local de pruebas enfocado en un sistema operativo móvil (Android). ¿Podrías darme una guía secuencial para instalar las dependencias necesarias, configurar las variables de entorno para el SDK y levantar correctamente el emulador, considerando que el objetivo final es ejecutar scripts de automatización?"*

Este prompt surgió en medio de un bloqueo real de configuración (no lo planeé de antemano). Al intentar ejecutar los primeros comandos, la terminal no lograba detectar el dispositivo. El problema no radicaba en el entorno de mi computadora, sino que necesitaba asistencia técnica específica para que las herramientas interactuaran correctamente con el sistema operativo móvil emulado. La respuesta de la IA me permitió identificar que el cuello de botella estaba en el mapeo de la ruta del SDK. Con la guía generada, logré que la consola reconociera el sistema operativo del emulador en cuestión de minutos, permitiéndome avanzar al código.

**Prompt 2: Acelerando las aserciones de la API en Postman**
> *"Para la capa de API de mi prueba técnica estoy estructurando mis peticiones en Postman. Necesito acelerar la creación de aserciones repetitivas para los contratos. Genera un boilerplate en JavaScript utilizando el objeto pm.test que valide de forma dinámica si el código de estado HTTP de la respuesta es exitoso (como un 200 OK o 201 Created). La idea es que este snippet sea modular y fácil de reutilizar en toda mi colección."*

Este prompt nació de la necesidad de optimizar el tiempo de entrega. Dado que la prueba técnica exige validar varios flujos de la API con peticiones dinámicas, escribir las aserciones de estado HTTP desde cero para cada endpoint resultaba ineficiente. La respuesta me entregó un bloque de código JavaScript limpio y modular que pude insertar directamente en la pestaña 'Tests' de la colección, permitiéndome enfocar mi energía en pensar analíticamente los escenarios de prueba y no en tipear sintaxis repetitiva.

## 4. Reflexión Técnica

**Impacto en velocidad y calidad:**
El uso del asistente redujo drásticamente el tiempo de configuración inicial. Vengo de un background de automatización funcional (con herramientas como Selenium y PyCharm), pero nunca había configurado un entorno de automatización móvil desde cero (Android SDK, emulador, variables de entorno, Maestro). Sin la guía paso a paso, hubiera invertido mucho más tiempo buscando en foros dispersos. En cuanto a calidad, delegar la generación inicial de los flujos YAML y los scripts de prueba me dio una base sólida y bien estructurada desde el primer intento, sobre la cual solo tuve que iterar para corregir detalles puntuales del comportamiento real de la aplicación.

**Manejo de errores y ajustes (Juicio de Ingeniería):**
El principal caso de corrección no fue una "alucinación" de código roto, sino una suposición incorrecta sobre el flujo de la aplicación. El primer script de inicio de sesión generado asumía que la app abría directamente en la pantalla de Login, cuando en realidad es necesario navegar primero por el menú y seleccionar la opción. Reporté el comportamiento real observado en el emulador y la IA ajustó los flujos afectados. Esto me reforzó una lección importante: la IA puede generar código sintácticamente correcto, pero el juicio de ingeniería y la validación manual contra la app real siguen siendo indispensables para confirmar la lógica del negocio.

**Decisiones de alcance y priorización:**
También utilicé el asistente para tomar una decisión de alcance con criterio. Antes de invertir esfuerzo en el reto opcional de Kafka, pedí un desglose de su complejidad técnica para evaluar hasta qué punto era viable implementarlo dado mi plazo de 4 días. Con esa información, tomé la decisión de priorizar la estabilidad y calidad de los entregables obligatorios (Móvil y API), en lugar de arriesgar la entrega completa por perseguir el bonus.
