# Prueba Técnica QA Automation — AVL Mobility Solutions

Suite de pruebas automatizadas para la capa móvil (Maestro) y la capa de API (Postman/Newman), desarrollada como parte del reto técnico de Ingeniero de Automatización QA.

## Estrategia y priorización de casos

Se priorizaron los flujos de mayor riesgo e impacto para el usuario final:

- **Login**: es el punto de entrada obligatorio a cualquier funcionalidad de la app; un fallo aquí bloquea el 100% del flujo de negocio.
- **Navegación y carrito**: valida que el usuario pueda completar el flujo core de la app (explorar catálogo → agregar producto), que es el objetivo principal de negocio de una app de e-commerce.
- **API (POST/PUT)**: se priorizaron operaciones mutables porque representan el mayor riesgo de corrupción de datos si fallan silenciosamente, a diferencia de un GET que solo lee información.

Durante la exploración manual se identificó que la app **no valida credenciales incorrectas** a nivel de negocio (permite el ingreso con cualquier combinación no vacía). Se ajustó el caso de "login inválido" al comportamiento real verificado: validación de campo username vacío, y se documentó el hallazgo como riesgo de seguridad a reportar.

## Requisitos previos

- [Node.js](https://nodejs.org/) (LTS)
- [Java JDK 17+](https://adoptium.net/)
- [Android Studio](https://developer.android.com/studio) con un emulador (AVD) configurado
- Variable de entorno `ANDROID_HOME` apuntando al SDK de Android
- [Maestro CLI](https://docs.maestro.dev/)
- [Postman](https://www.postman.com/downloads/) (para editar la colección) y [Newman](https://www.npmjs.com/package/newman) (para correrla por CLI)

## Instalación

```bash
# Instalar Newman globalmente
npm install -g newman

# (Opcional) Instalar el reporter HTML para Newman
npm install -g newman-reporter-htmlextra
```

Instala el APK de [Sauce Labs My Demo App](https://github.com/saucelabs/my-demo-app-android/releases) en tu emulador antes de correr las pruebas móviles.

## Cómo ejecutar las pruebas

### Pruebas móviles (Maestro)

Con el emulador encendido y la app instalada:

```bash
maestro test mobile-tests/specs/login_valido.yaml
maestro test mobile-tests/specs/login_invalido.yaml
maestro test mobile-tests/specs/navegacion_y_carrito.yaml
```

O todos los flujos de una vez:

```bash
maestro test mobile-tests/specs/
```

### Pruebas de API (Postman/Newman)

```bash
newman run api-tests/avl-api-tests.json
```

Para generar un reporte HTML:

```bash
newman run api-tests/avl-api-tests.json -r htmlextra --reporter-htmlextra-export api-tests/reporte.html
```

## Estructura del proyecto

```
/Avl-qa-automation-test
├── .gitignore
├── README.md
├── AI_USAGE.md
├── /mobile-tests
│   └── /specs
│       ├── login_valido.yaml
│       ├── login_invalido.yaml
│       └── navegacion_y_carrito.yaml
└── /api-tests
    ├── avl-api-tests.json
    └── reporte.html
```

## Herramientas utilizadas

- **Maestro CLI** para automatización móvil — elegido sobre Appium por su curva de aprendizaje más rápida, sintaxis declarativa en YAML y estabilidad frente a cambios en la UI mediante selectores como `id` y `text`.
- **Postman + Newman** para pruebas de API — permite diseñar, documentar y ejecutar por CLI las pruebas contra [DummyJSON](https://dummyjson.com/), incluyendo validación de status code, SLA de tiempo de respuesta y JSON Schema.
- **App bajo prueba (SUT)**: [Sauce Labs My Demo App (Android)](https://github.com/saucelabs/my-demo-app-android)
- **API bajo prueba**: [DummyJSON](https://dummyjson.com/)

## Hallazgos técnicos

- La app no implementa validación de credenciales inválidas a nivel de negocio (login exitoso con cualquier combinación no vacía). Se documenta como riesgo a reportar al equipo de producto.
- Se evitaron rutas XPath absolutas; todos los selectores usan `id` o `text` visible para reducir la fragilidad (flakiness) ante cambios de UI.
