---
title: Desarrollo de Android nativo en Windows
description: Introducción al desarrollo de aplicaciones nativas de Android en Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Android, Windows, Android Studio, Visual Studio, juego Android de c++, Windows Defender, emulador, dispositivo virtual, instalación, Java, Kotlin
ms.date: 04/28/2020
ms.openlocfilehash: c9c718d2cccc6a38ac75d3220a79c7b2ec757f54
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166849"
---
# <a name="get-started-with-native-android-development-on-windows"></a>Introducción al desarrollo de Android nativo en Windows

Esta guía le ayudará a empezar a usar Windows para crear aplicaciones nativas de Android. Si prefiere una solución multiplataforma, consulte [información general sobre el desarrollo de Android en Windows](./overview.md) para obtener un breve resumen de algunas opciones.

La forma más directa de crear una aplicación Android nativa es usar Android Studio con [Java o Kotlin](#java-or-kotlin), aunque también es posible [usar C o C++ para el desarrollo de Android](#use-c-or-c-for-android-game-development) si tiene un propósito específico. Las herramientas del SDK de Android Studio compilan los archivos de código, datos y recursos en un archivo. apk de archivo de paquete Android. Un archivo APK contiene todo el contenido de una aplicación Android y es el archivo que usan los dispositivos Android para instalar la aplicación.

## <a name="install-android-studio"></a>Instalar Android Studio

Android Studio es el entorno de desarrollo integrado oficial del sistema operativo Android de Google. [Descargue la versión más reciente de Android Studio para Windows](https://developer.android.com/studio).

- Si descargó un archivo. exe (recomendado), haga doble clic para iniciarlo.
- Si descargó un archivo. zip, Desempaquete el archivo ZIP, copie la carpeta Android-Studio en la carpeta archivos de programa y, a continuación, abra la carpeta Android-Studio > bin e inicie studio64.exe (para equipos de 64 bits) o studio.exe (para equipos de 32 bits).

Siga el Asistente para la instalación de Android Studio e instale los paquetes de SDK que recomiende. A medida que estén disponibles nuevas herramientas y otras API, Android Studio le notificará con un elemento emergente, o bien comprobará si hay actualizaciones seleccionando **ayuda**  >  **para buscar**actualizaciones.

## <a name="create-a-new-project"></a>Creación de un nuevo proyecto

Seleccione **archivo**  >  **nuevo**  >  **proyecto nuevo**.

En la ventana **elegir el proyecto** , podrá elegir entre estas plantillas:

- **Actividad básica**: crea una aplicación sencilla con una barra de la aplicación, un botón de acción flotante y dos archivos de diseño: uno para la actividad y otro para separar el contenido de texto.

- **Actividad vacía**: crea una actividad vacía y un archivo de diseño único con contenido de texto de ejemplo.

- **Actividad de navegación inferior**: crea una barra de navegación inferior estándar para una actividad. Vea el [componente de navegación inferior](https://material.io/guidelines/components/bottom-navigation.html) en las directrices de diseño de material de Google.

- Obtenga más información sobre cómo [seleccionar una plantilla de actividad](https://developer.android.com/studio/projects/templates#SelectTemplate) en el Android Studio docs.

Las plantillas se usan normalmente para agregar actividades a los módulos de aplicación nuevos y existentes. Por ejemplo, para crear una pantalla de inicio de sesión para los usuarios de la aplicación, agregue una actividad con la [plantilla de actividad de inicio de sesión](https://developer.android.com/studio/projects/templates#LoginActivity).

> [!NOTE]
> El sistema operativo Android se basa en la idea de los **componentes** y usa la **actividad** de los términos y la **intención** de definir las interacciones. Una **actividad** representa una única tarea centrada que puede realizar un usuario. Una **actividad** proporciona una ventana para compilar la interfaz de usuario mediante clases basadas en la clase de **vista** . Hay un ciclo de vida para **las actividades** en el sistema operativo Android, definido por un conjunto de seis devoluciones de llamada: `onCreate()` ,, `onStart()` `onResume()` , `onPause()` , `onStop()` y `onDestroy()` . Los componentes de actividad interactúan entre sí mediante objetos de **intención** . Intención: define la actividad para iniciar o describe el tipo de acción que se va a realizar (y el sistema selecciona la actividad adecuada para usted, que puede ser incluso de otra aplicación). Obtenga más información sobre [las actividades](https://developer.android.com/reference/android/app/Activity), el [ciclo de vida](https://developer.android.com/guide/components/activities/activity-lifecycle)de la actividad y los [intentos](https://developer.android.com/reference/android/content/Intent.html) en los documentos de Android.

### <a name="java-or-kotlin"></a>Java o Kotlin

**Java** se convirtió en un lenguaje de 1991, desarrollado por lo que era entonces Sun Microsystems, pero que ahora pertenece a Oracle. Se ha convertido en uno de los lenguajes de programación más populares y eficaces con una de las mayores comunidades de soporte técnico del mundo. Java está basado en clases y está orientado a objetos, diseñado para tener el menor número posible de dependencias de implementación. La sintaxis es similar a C y C++, pero tiene menos recursos de bajo nivel que cualquiera de ellos.

**Kotlin** se anunció primero como un nuevo lenguaje de código abierto de JetBrains en 2011 y se incluyó como alternativa a Java en Android Studio desde 2017. En el 2019 de mayo, Google anunció Kotlin como lenguaje preferido para los desarrolladores de aplicaciones Android, por lo que, a pesar de ser un lenguaje más reciente, también tiene una comunidad de soporte técnico sólida y se ha identificado como uno de los lenguajes de programación más rápidos. Kotlin es multiplataforma, tiene un tipo estático y está diseñado para interoperar completamente con Java.

Java se usa más ampliamente para una gama más amplia de aplicaciones y ofrece algunas características que Kotlin no, como excepciones comprobadas, tipos primitivos que no son clases, miembros estáticos, campos no privados, tipos comodín y operadores ternarios. Kotlin está diseñado específicamente para Android y lo recomienda. También ofrece algunas características que Java no admite, como las referencias nulas controladas por el sistema de tipos, sin tipos sin procesar, matrices invariables, tipos de función adecuados (en oposición a las conversiones de SAM de Java), la varianza de los sitios de uso sin comodines, las conversiones inteligentes, etc. La documentación de Kotlin ofrece [una visión más detallada de la comparación con Java](https://kotlinlang.org/docs/reference/comparison-to-java.html).

### <a name="minimum-api-level"></a>Nivel mínimo de API

Tendrá que decidir el nivel mínimo de API para la aplicación. Esto determina qué versión de Android admitirá la aplicación. Los niveles de API inferiores son más antiguos y, por lo general, admiten más dispositivos, pero los niveles de API más altos son más recientes y sus proporcionan más características.

![Android Studio pantalla de selección de API mínima](../images/android-minimum-api-selection.png)

Seleccione el vínculo **ayudarme a elegir** para abrir un gráfico de comparación que muestra la distribución de compatibilidad del dispositivo y las características clave asociadas a la versión de la plataforma.

![Android Studio pantalla de comparación mínima de API](../images/android-minimum-api-selection-2.png)

### <a name="instant-app-support-and-androidx-artifacts"></a>Compatibilidad con aplicaciones instantáneas y artefactos de Androidx

Es posible que observe una casilla para **admitir aplicaciones instantáneas** y otra para **usar los artefactos de androidx** en las opciones de creación del proyecto. No se comprueba la *compatibilidad de aplicaciones instantáneas* y se comprueba la *androidx* como el valor predeterminado recomendado.

Google Play **aplicaciones Instant** proporcionan a los usuarios una forma de probar una aplicación o un juego sin instalarlo primero. Estas aplicaciones instantáneas pueden aparecer en el Play Store, Google Search, redes sociales y cualquier lugar en el que comparta un vínculo. Al activar el cuadro **support Instant apps (compatibilidad con aplicaciones instantáneas** ), está preguntando Android Studio incluir el Google Play SDK de desarrollo instantáneo con su proyecto. Para obtener más información sobre [Google Play aplicaciones instantáneas](https://developer.android.com/topic/google-play-instant) y cómo [crear un paquete de aplicaciones habilitadas para el uso instantáneo](https://developer.android.com/topic/google-play-instant/getting-started/instant-enabled-app-bundle), consulte la documentación de Android.

**AndroidX artefactos** representa la nueva versión de la biblioteca de compatibilidad de Android y proporciona compatibilidad con versiones anteriores en las versiones de Android. AndroidX proporciona un espacio de nombres coherente que comienza con la cadena AndroidX para todos los paquetes disponibles.

> [!NOTE]
> AndroidX es ahora la biblioteca predeterminada. Para desactivar esta casilla y usar la biblioteca de compatibilidad anterior, es necesario quitar el SDK Q de Android más reciente. Consulte [desactive la casilla usar artefactos de Androidx](https://stackoverflow.com/questions/56580980/uncheck-use-androidx-artifacts) en stackoverflow para obtener instrucciones, pero primero tenga en cuenta que los paquetes de la biblioteca de compatibilidad anteriores se han asignado en los paquetes Androidx. * correspondientes. Para una asignación completa de todas las clases antiguas y los artefactos de compilación a los nuevos, vea [migrar a AndroidX](https://developer.android.com/jetpack/androidx/migrate).

## <a name="project-files"></a>Archivos de proyecto

La ventana Android Studio **proyecto** contiene los siguientes archivos (Asegúrese de que la vista Android está seleccionada en el menú desplegable):

**App > Java > com. example. MyFirstApp > MainActivity**

La actividad principal y el punto de entrada de la aplicación. Al compilar y ejecutar la aplicación, el sistema inicia una instancia de esta actividad y carga su diseño.

**> de diseño de > de App > res activity_main.xml**

El archivo XML que define el diseño de la interfaz de usuario (UI) de la actividad. Contiene un elemento TextView con el texto "Hola mundo"

**manifiestos de > de la aplicación > AndroidManifest.xml**

El archivo de manifiesto que describe las características fundamentales de la aplicación y cada uno de sus componentes.

**Scripts de Gradle > Build. Gradle**

Hay dos archivos con este nombre: "proyecto: mi primera aplicación", para todo el proyecto y "módulo: aplicación", para cada módulo de la aplicación. Un proyecto nuevo solo tendrá un módulo. Use el archivo build del módulo para controlar cómo el complemento de Gradle compila la aplicación. Obtenga más información en los documentos de Android, [Configure el](https://developer.android.com/studio/build/index) artículo de compilación.

## <a name="use-c-or-c-for-android-game-development"></a>Uso de C o C++ para el desarrollo de juegos de Android

El sistema operativo Android está diseñado para admitir aplicaciones escritas en Java o Kotlin, lo que se beneficia de las herramientas incrustadas en la arquitectura del sistema. Muchas características del sistema, como la interfaz de usuario de Android y el control de intención, solo se exponen a través de interfaces de Java. Hay algunos casos en los que puede que desee **usar código de C o C++ a través del kit de desarrollo nativo (NDK) de Android a** pesar de algunos de los desafíos asociados. El desarrollo de juegos es un ejemplo, ya que los juegos suelen usar la lógica de representación personalizada escrita en OpenGL o Vulkan y se benefician de una gran cantidad de bibliotecas de C centradas en el desarrollo de juegos. El uso de C o C++ también *puede* ayudarle a reducir el rendimiento extra de un dispositivo para lograr una latencia baja o ejecutar aplicaciones informáticas intensivas, como las simulaciones físicas. Sin embargo, el NDK **no es adecuado para la mayoría de los programadores de Android inexpertos** . A menos que tenga un propósito específico para usar el NDK, se recomienda el uso de Java, Kotlin o uno de los [marcos de trabajo multiplataforma](./overview.md).

Para crear un nuevo proyecto con compatibilidad con C/C++:

- En la sección **elegir el proyecto** del asistente para Android Studio, seleccione el tipo de proyecto C++ * *nativo*. Seleccione **siguiente**, complete los campos restantes y, después, vuelva a seleccionar **siguiente** .

- En la sección **personalizar la compatibilidad con c++** del asistente, puede personalizar el proyecto con el campo estándar de **c++** . Use la lista desplegable para seleccionar qué estandarización de C++ desea utilizar. Si selecciona **cadena** de herramientas, se usa el valor predeterminado CMake. Seleccione **Finalizar**.

- Una vez que Android Studio crea el nuevo proyecto, puede encontrar una carpeta **CPP** en el panel **proyecto** que contiene los archivos de código fuente nativos, los encabezados, los scripts de compilación para cmake o NDK-Build, y las bibliotecas creadas previamente que forman parte del proyecto. También puede encontrar un archivo de código fuente de C++ de ejemplo, `native-lib.cpp` , en la `src/main/cpp/` carpeta que proporciona una función simple que `stringFromJNI()` devuelve la cadena "Hello from C++". Además, encontrará un script de compilación de CMake, [`CMakeLists.txt`](https://developer.android.com/studio/projects/configure-cmake.html) , en el directorio raíz del módulo necesario para compilar la biblioteca nativa.

Para obtener más información, vea el tema sobre documentos de Android: [Agregar código de C y C++ al proyecto](https://developer.android.com/studio/projects/add-native-code). Para obtener ejemplos, consulte los [ejemplos de NDK de Android con el repositorio de integración de Android Studio C++](https://github.com/android/ndk-samples) en github. Para compilar y ejecutar un juego de C++ en Android, use la [API de Google Play Game Services](https://developers.google.com/games/services/cpp/gettingStartedAndroid).

## <a name="design-guidelines"></a>Guías de diseño

Los usuarios del dispositivo esperan que las aplicaciones tengan el aspecto y el comportamiento de una manera determinada... tanto si se desliza con el dedo o en el uso de los controles de voz, los usuarios tendrán expectativas específicas sobre el aspecto que debe tener su aplicación y cómo usarla. Estas expectativas deben ser coherentes para reducir la confusión y la frustración. Android ofrece una guía para estas expectativas de plataforma y dispositivo que combina la base de diseño de material de Google para visual y los patrones de navegación, junto con directrices de calidad para la compatibilidad, el rendimiento y la seguridad.

Obtenga más información en la documentación sobre el [diseño de Android](https://developer.android.com/design).

### <a name="fluent-design-system-for-android"></a>Sistema de diseño fluida para Android

Microsoft también ofrece instrucciones de diseño con el objetivo de proporcionar una experiencia sin problemas en toda la cartera de aplicaciones móviles de Microsoft.

[Sistema de diseño fluida para el diseño de Android](https://www.microsoft.com/design/fluent/#/android) y creación de aplicaciones personalizadas que son Android de forma nativa sin dejar de ser exclusivas.

- [Kit de herramientas de boceto](https://aka.ms/fluenttoolkits/android/sketch)
- [Kit de herramientas de Figma](https://aka.ms/fluenttoolkits/android/figma)
- [Fuente de Android](https://fonts.google.com/specimen/Roboto)
- [Directrices de la interfaz de usuario de Android](https://developer.android.com/design/)
- [Directrices para iconos de aplicaciones Android](https://developer.android.com/guide/practices/ui_guidelines/icon_design)

## <a name="additional-resources"></a>Recursos adicionales

- [Aspectos básicos de la aplicación Android](https://developer.android.com/guide/components/fundamentals)

- [Desarrollo de aplicaciones de pantalla dual para Android y obtención del SDK de dispositivo Surface Duo](/dual-screen/android/)

- [Agregar exclusiones de Windows Defender para mejorar el rendimiento](defender-settings.md)

- [Habilitar la compatibilidad con la virtualización para mejorar el rendimiento del emulador](emulator.md#enable-virtualization-support)