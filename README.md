# Propuesta Técnica: Sistema de Localización para Personas Dependientes

## 1. Introducción para el Cliente: Conceptos Clave
Para que entienda cómo funcionará la solución que vamos a desarrollar, es importante aclarar primero tres conceptos fundamentales que utilizaremos constantemente en la aplicación:

* **Localización:** Es la tecnología base que nos permite saber dónde está un objeto o persona en el mundo real. Imagínelo como el sistema que nos da las coordenadas matemáticas de dónde se encuentra el reloj o teléfono de la persona que cuidamos.

* **Mapas:** Son la representación visual y gráfica de la zona calles, edificios, montañas. Es el "dibujo" sobre el cual pintaremos la posición de la persona para que usted pueda ver visualmente dónde está.

* **Geocodificación:** Este es el proceso de "traducción". Convierte las coordenadas numéricas que nos da el GPS en una dirección comprensible (por ejemplo, "Plaza Mayor, nº 5"). Esto es vital para que, si recibe una alerta, sepa la dirección exacta y no solo un punto en el mapa.

## 2. Estudio de Tecnologías de Localización Disponibles
Para el objetivo de localizar a personas dependientes usando móviles o relojes inteligentes, hemos analizado las siguientes tecnologías disponibles en el mercado actual:

### GPS (Sistema de Posicionamiento Global)
Es el sistema más conocido.Utiliza una red de satélites para determinar la posición con muchísima precisión.
* **Ventaja:** Es muy preciso en exteriores.
* **Inconveniente:** La señal puede bloquearse si la persona entra en edificios, túneles o zonas con muchas obstrucciones.

### A-GPS (GPS Asistido) - **Recomendado**
Es una evolución del GPS que utiliza las antenas de telefonía terrestre para ayudar a los satélites.
* **Por qué nos interesa:** Permite un inicio de navegación más rápido y, muy importante para este proyecto, mejora el consumo de batería. Dado que vamos a usar dispositivos móviles o relojes, ahorrar batería es crítico.

### Wi-Fi y Redes Móviles
Estas tecnologías permiten calcular la posición usando la intensidad de la señal de las antenas de teléfono o los puntos Wi-Fi cercanos.
* **Por qué nos interesa:** Aunque son menos precisas que el GPS, son más rápidas y funcionan mejor en interiores (dentro de casa o centros comerciales), donde el GPS suele fallar.

### Tecnologías de apoyo (Bluetooth / AirTag)
Para distancias cortas o localización muy precisa, existen tecnologías como los AirTags de Apple, que envían señales cifrada y anónimas a otros dispositivos cercanos para ubicar objetos o personas. Esto podría ser un complemento útil si la persona se pierde en una multitud.

## 3. Conclusión del Estudio
Para garantizar la seguridad de las personas dependientes, **recomendamos un sistema híbrido**. Utilizaremos principalmente **A-GPS** para exteriores por su equilibrio entre precisión y batería y nos apoyaremos en la localización por **Wi-Fi/Redes Móviles** para cuando la persona se encuentre en interiores, asegurando así cobertura en todo momento.

## 4. Hoja de Ruta del Desarrollo Técnico

Para construir esta aplicación de localización para personas dependientes, seguiremos un proceso estructurado en cuatro fases clave. Este enfoque nos asegura cumplir con los requisitos técnicos de las plataformas móviles (Android e iOS) y garantizar la privacidad del usuario.

### Paso 1: Configuración del Entorno y Dependencias
Lo primero es preparar nuestro entorno de trabajo. Si optamos por un desarrollo multiplataforma con **Flutter** para tener una sola base de código para Android e iOS, debemos incorporar herramientas externas que nos faciliten el acceso al GPS. Para ello, añadiremos la librería `geolocator` en nuestro archivo de configuración `pubspec.yaml`.Si necesitásemos mostrar mapas visuales, también incluiríamos la dependencia `Maps_flutter`, la cual requiere obtener una **API Key** (una clave de seguridad) de los servicios de Google para funcionar correctamente.

### Paso 2: Gestión de Permisos (Privacidad)
Este es el paso más delicado y legalmente importante. Los sistemas operativos móviles protegen la ubicación del usuario, por lo que la aplicación no funcionará si no pedimos permiso explícito.
* **En Android:** Debemos editar el archivo `manifest.xml` para declarar que nuestra app usará el GPS.
* **En iOS:** Debemos editar el archivo `info.plist` añadiendo una descripción de por qué necesitamos saber dónde está la persona (por ejemplo: "Para localizar al familiar dependiente").

### Paso 3: Implementación de la Lógica de Ubicación
Una vez tenemos los permisos, programamos la lógica interna.Utilizando la librería `geolocator` (o `FusedLocationProviderClient` si hiciéramos Android nativo con Kotlin), escribiremos el código para obtener la posición.Aquí podemos configurar si queremos la ubicación actual puntual o recibir actualizaciones en tiempo real mientras la persona se mueve.

### Paso 4: Integración Visual de Mapas
Finalmente, conectamos los datos de ubicación con la interfaz gráfica.Es importante distinguir que obtener la coordenada (latitud/longitud) es una cosa, y representarla en un mapa es otra.En esta fase, usamos la API de Google Maps para pintar el punto exacto de la persona dependiente sobre el callejero, permitiendo al familiar ver visualmente dónde se encuentra o incluso trazar la ruta para llegar hasta ella.

## 5. Funcionamiento de los Mapas y Estructura de la App

Para que la aplicación sea útil, no basta con tener una coordenada numérica; necesitamos representarla gráficamente.Los mapas actúan como una representación visual de la zona geográfica, permitiéndonos ver en perspectiva la distribución de calles, edificios y otros elementos del entorno. En nuestra aplicación para personas dependientes, el mapa será la pantalla principal donde visualizaremos el "pin" o marcador de la persona ubicada.

### Secciones Principales de la Aplicación
Basándonos en la funcionalidad de localización, la aplicación contará con las siguientes secciones clave:
* **Vista de Mapa en Tiempo Real:** Será el núcleo de la app. Aquí se cargará el mapa (usando servicios como Google Maps) y se actualizará la posición del usuario.
* **Gestión de Rutas e Historial:** Como hemos visto, no es lo mismo obtener una ubicación puntual que trazar una línea recorrida. Esta sección permitirá ver el historial de movimientos o calcular la distancia y el trayecto necesario para llegar hasta la persona dependiente.
* **Panel de Alertas:** Una sección para configurar notificaciones si la persona sale de una zona segura.

## 6. Uso de Emuladores vs. Dispositivos Reales

Durante la fase de desarrollo, no siempre podemos salir a la calle para probar si el GPS funciona. Para ello utilizamos **emuladores**, herramientas que imitan el funcionamiento de un móvil real dentro de nuestro ordenador.

### Diferencias Clave
La principal diferencia es el origen de los datos. Un **dispositivo físico** utiliza sensores reales (módulos GPS, antenas Wi-Fi) para calcular la posición basándose en satélites y redes. En cambio, el **emulador** nos permite "fingir" o simular ubicaciones dentro de un entorno controlado, lo cual es ventajoso para probar cómo responde la app si la persona se mueve rápido o cambia de ciudad, sin movernos nosotros del sitio.

### Simulación de Ubicaciones (Testing)
Para probar nuestra app en el laboratorio, inyectaremos datos de localización falsos en los emuladores:

* **En Android Studio:** Abrimos las opciones del emulador y buscamos la pestaña "Location". Aquí podemos cargar archivos **GPX** o **KML**. El formato GPX es un estándar XML para intercambio de datos de mapas, mientras que KML es similar pero enfocado a la representación en 3D (usado originalmente por Google Earth).
* **En Xcode:** Vamos a la configuración ("Settings" o "Preferences"), buscamos "Locations" y, dentro de las opciones avanzadas de "Derived Data", modificamos la ubicación simulada para que el iPhone virtual crea que está en otro lugar.

## 7. Boceto Conceptual de la Aplicación (Diseño)

Dado que nuestro objetivo es localizar a personas dependientes, la interfaz debe ser extremadamente sencilla y visual. A continuación, describo las pantallas principales que compondrán la aplicación:

**A. Pantalla Principal: "El Radar"**
* **Elemento central:** Un mapa a pantalla completa (integrando Google Maps).
* **Marcadores:** Un icono con la foto del familiar que se mueve sobre el mapa en tiempo real.
* **Panel inferior:** Una tarjeta deslizable que muestra la dirección escrita (gracias a la geocodificación) de donde está la persona (ej: "Calle de las Angustias, 12").
* **Botón de acción rápida:** "Cómo llegar", que traza automáticamente la ruta desde mi posición hasta la del familiar.

**B. Pantalla de Gestión: "Zonas Seguras"**
* Esta pantalla permite configurar áreas en el mapa (como la casa o el parque habitual).
* **Funcionalidad:** Si el GPS detecta que la persona sale de estas coordenadas, la app envía una alerta.

## 8. Resumen de Funcionalidades Técnicas

Para cerrar el estudio técnico, estas son las características que implementaremos en el código:
* **Rastreo Híbrido:** Uso de A-GPS en exteriores y Wi-Fi en interiores para no perder la señal.
* **Geocodificación Inversa:** Para traducir las coordenadas (latitud/longitud) a direcciones de calles comprensibles para el usuario.
* **Modo de bajo consumo:** La app ajustará la frecuencia de actualización del GPS según la batería restante del dispositivo.
* **Privacidad:** Los datos de ubicación estarán cifrados y solo accesibles por el familiar autorizado.

## 9. Conclusión del Proceso de Desarrollo

El desarrollo de este proyecto seguirá un ciclo iterativo. Comenzaremos configurando el entorno en **Flutter** para asegurar compatibilidad con Android e iOS. Durante la fase de codificación, utilizaremos **emuladores** cargando archivos **GPX** para simular rutas de caminata y verificar que las alertas de "zona segura" saltan correctamente sin necesidad de realizar pruebas de campo físicas inicialmente. Finalmente, realizaremos pruebas en dispositivos reales para calibrar el consumo de batería del A-GPS.
