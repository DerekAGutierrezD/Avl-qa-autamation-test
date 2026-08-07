# Bitácora de Co-Pilotaje e IA (AI_USAGE)

## 1. Herramientas Utilizadas

* **Asistente Principal:** Claude (Anthropic)
* **Rol en el flujo de trabajo:** Co-piloto conversacional durante todo el desarrollo — desde la configuración del entorno hasta la generación de scripts, explicación de código y toma de decisiones de alcance (scope).

## 2. Casos de Uso Específicos

Mi enfoque de trabajo con la IA fue conversacional e iterativo: la usé como acelerador de tareas puntuales, no como reemplazo del trabajo de ingeniería, exploración y verificación, que hice yo directamente sobre mi máquina y el emulador.

**Lo que hice yo directamente:**
* Instalé y configuré todo el entorno (Java, Android Studio, emulador, variables de entorno, Maestro CLI, Postman, Newman).
* Exploré la app manualmente con Maestro Studio e identifiqué yo mismo cada selector (`id`, `text`) usado en los flujos.
* Detecté por mi cuenta que la app no valida credenciales incorrectas a nivel de negocio, probando manualmente distintas combinaciones antes de decidir cómo ajustar el caso de prueba.
* Ejecuté cada flujo de Maestro y cada request de Postman en mi entorno, y reporté los errores/comportamientos reales encontrados.
* Tomé la decisión final de no implementar el bonus de Kafka, priorizando la calidad de los entregables obligatorios dado mi plazo de 4 días.

**Lo que delegué a la IA como acelerador:**
* Guía paso a paso para configurar el entorno de automatización móvil en Windows (algo que no había hecho antes; mi experiencia previa es con Selenium/PyCharm en web, no en móvil).
* Generación del boilerplate inicial de los flujos YAML de Maestro, a partir de los selectores que yo ya había identificado.
* Corrección de esos flujos cuando le reporté que el comportamiento real de la app no coincidía con lo asumido (ej. la app no abre directo en login, requiere pasar por el menú primero).
* Generación del boilerplate de los scripts de test en Postman (JavaScript) para status code, SLA y JSON Schema.
* Explicación línea por línea del código generado, para poder entenderlo a fondo y sustentarlo en el video con criterio propio, no solo repetirlo de memoria.

## 3. Ejemplos de Prompts Clave

**Prompt 1: Troubleshooting real de variables de entorno**
> *"acá tengo otra duda, yo creé la variable ANDROID_HOME en variables del sistema y no en variables de usuario, ¿hay algún problema?"*

Este prompt surgió en medio de un bloqueo real de configuración (no lo planeé de antemano). La respuesta me permitió confirmar que no había conflicto y entender cómo Windows combina las variables de usuario y de sistema, evitando que perdiera tiempo deshaciendo una configuración que en realidad estaba bien.

**Prompt 2: Pidiendo entender el código, no solo recibirlo**
> *"tanto para el post como para el put, qué función tiene este código de [JavaScript]? entiendo lo del JSON porque son como los parámetros de la función en general, pero no entiendo la parte del código ya que me toca explicar todo esto y me siento algo perdido..."*

Este fue el prompt de mayor valor en todo el proceso: en vez de simplemente usar el código que la IA generó, le pedí que me lo explicara línea por línea (qué es `pm`, qué hace `pm.expect`, la diferencia entre `pm.test` con `function()` y con arrow function `() => {}`). Esto fue clave para poder sustentar el video con seguridad, en vez de repetir código que no domino.

## 4. Reflexión Técnica

**Impacto en velocidad y calidad:**
El uso del asistente redujo drásticamente el tiempo de configuración inicial — vengo de un background de automatización con Selenium/PyCharm, pero nunca había configurado un entorno de automatización móvil desde cero (Android SDK, emulador, variables de entorno, Maestro). Sin la guía paso a paso hubiera invertido mucho más tiempo buscando en foros dispersos. En cuanto a calidad, delegar la generación inicial de los flujos YAML y los scripts de test me dio una base sólida y bien estructurada desde el primer intento, sobre la cual solo tuve que corregir detalles puntuales de comportamiento real de la app.

**Manejo de errores y ajustes:**
El principal caso de corrección no fue una "alucinación" de código roto, sino una **suposición incorrecta sobre el flujo de la app**: el primer script de login asumía que la app abría directamente en la pantalla de login, cuando en realidad hay que pasar primero por el menú y tocar "Log In". Reporté el comportamiento real observado en el emulador y la IA ajustó los tres flujos afectados. Esto reforzó algo importante: la IA puede generar código sintácticamente correcto, pero solo yo, verificando contra la app real, puedo confirmar que la lógica del flujo es la correcta — el juicio de ingeniería y la validación manual siguen siendo indispensables.

También usé la IA para tomar una decisión de alcance con criterio: antes de invertir tiempo en el bonus de Kafka, pedí que me explicaran su complejidad real y until qué punto era razonable dado mi plazo de 4 días. Con esa información prioricé terminar bien lo obligatorio en vez de arriesgar la entrega completa por perseguir el bonus.
