---
title: Información general sobre el desarrollo de Android en Windows
description: Una guía para ayudarle a empezar a desarrollar para Android en Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Android en Windows, Xamarin. Android, reAct Native, Cordova, iónico, PhoneGap, juego Android de c++, Windows Defender, emulador
ms.date: 04/28/2020
ms.openlocfilehash: d43420f442fd5dfcb2b885fb0369964a113e9bac
ms.sourcegitcommit: 24b19e7ee06e5bb11a0dae334806741212490ee9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "82255249"
---
# <a name="overview-of-android-development-on-windows"></a>Información general sobre el desarrollo de Android en Windows

Hay varias rutas para desarrollar una aplicación para dispositivos Android mediante el sistema operativo Windows. Estas rutas de acceso se dividen en tres tipos principales: **[desarrollo de Android nativo](#native-android)**, desarrollo **[multiplataforma](#cross-platform)** y **[desarrollo de juegos Android](#game-development)**. Esta información general le ayudará a decidir qué ruta de desarrollo debe seguir para desarrollar una aplicación Android y, a continuación, proporcionar [los siguientes pasos](#next-steps) para ayudarle a empezar a usar Windows para desarrollar con:

- [Android nativo](native-android.md)
- [Xamarin.Android](xamarin-android.md)
- [Xamarin.Forms](xamarin-forms.md)
- [React Native](react-native.md)
- [Cordova, iónico o PhoneGap](pwa.md)
- [C/C++ para el desarrollo de juegos](native-android.md#use-c-or-c-for-android-game-development)

Además, en esta guía se proporcionarán sugerencias sobre el uso de Windows para:

- [Prueba en un emulador o dispositivo Android](emulator.md)
- [Actualización de la configuración de Windows Defender para mejorar el rendimiento](defender-settings.md)
- [Desarrollo de aplicaciones de pantalla dual para Android y obtención del SDK de dispositivo Surface Duo](https://docs.microsoft.com/dual-screen/android/)

## <a name="native-android"></a>Android nativo

El [desarrollo de Android nativo en Windows](./native-android.md) significa que la aplicación tiene como destino solo Android (no dispositivos iOS o Windows). Puede usar [Android Studio](https://developer.android.com/studio/install#windows) o [Visual Studio](https://visualstudio.microsoft.com/vs/android/) para desarrollar en el ecosistema diseñado específicamente para el sistema operativo Android. El rendimiento se optimizará para dispositivos Android, la apariencia de la interfaz de usuario será coherente con otras aplicaciones nativas del dispositivo, y las características o funcionalidades del dispositivo del usuario serán de avance directo para el acceso y el uso. Desarrollar la aplicación en un formato nativo le ayudará a "sentirse bien", ya que sigue todos los patrones de interacción y estándares de experiencia del usuario establecidos específicamente para dispositivos Android.

## <a name="cross-platform"></a>Multiplataforma

Los marcos de trabajo multiplataforma proporcionan un único código base que se puede compartir (principalmente) entre dispositivos Android, iOS y Windows. El uso de un marco multiplataforma puede ayudar a su aplicación a mantener la misma apariencia, sensación y experiencia en las distintas plataformas de dispositivos, así como a beneficiarse de la implementación automática de actualizaciones y correcciones. En lugar de tener que comprender una variedad de lenguajes de código específicos del dispositivo, la aplicación se desarrolla en un código base compartido, normalmente en un lenguaje.

Aunque los marcos de trabajo multiplataforma tienen como objetivo tener una apariencia similar a la de las aplicaciones nativas posible, nunca se integrarán perfectamente como una aplicación desarrollada de forma nativa y pueden verse afectados por una velocidad reducida y un rendimiento degradado. Además, las herramientas que se usan para compilar aplicaciones multiplataforma podrían no tener todas las características que ofrece cada plataforma de dispositivo, lo que podría requerir soluciones alternativas.

Un código base se compone normalmente de **código de interfaz**de usuario para crear la interfaz de usuario como páginas, controles de botones, etiquetas, listas, etc., y **código lógico**, para llamar a servicios Web, obtener acceso a una base de datos, invocar capacidades de hardware y administrar el estado. En promedio, el 90% de esto se puede volver a usar, aunque normalmente es necesario personalizar el código para cada plataforma de dispositivo. Esta generalización depende en gran medida del tipo de aplicación que se va a compilar, pero proporciona un poco de contexto que, con suerte, le ayudará a tomar decisiones.  

## <a name="choosing-a-cross-platform-framework"></a>Elección de un marco multiplataforma

[Xamarin Native (Xamarin. Android)](xamarin-android.md)

- Código de la interfaz de usuario: XML con Android Designer y tema de material
- Código lógico: C# o F #
- Todavía puede acceder a algunos elementos de Android nativos, pero bien a la reutilización de la base de código para otras plataformas (iOS y Windows).
- Solo el código lógico se comparte entre plataformas, no por código de interfaz de usuario.
- Excelente para aplicaciones más complejas con una interfaz de usuario específica del dispositivo.

[Xamarin Forms (Xamarin. Forms)](xamarin-forms.md)

- Código de interfaz de usuario: XAML y .NET (con Visual Studio)
- Código lógico: C #
- Recursos compartidos en torno al 60:90% de la lógica y el código de la interfaz de usuario en aplicaciones de dispositivos Android, iOS y Windows. 
- Usa controles de usuario comunes como Button, Label, Entry, ListView, StackLayout, Calendar, TabbedPage, etc. Crear un botón y Xamarin Forms determinarán cómo llamar al botón nativo de cada plataforma mediante la biblioteca de enlace para llamar a código de Java o SWIFT desde C#.
- Excelente para aplicaciones sencillas, como aplicaciones internas o de línea de negocio (LOB), prototipos o MVP. Cualquier aplicación que pueda parecer algo estándar o genérica, con una interfaz de usuario sencilla.

[React Native](react-native.md)

- Código de interfaz de usuario: JavaScript
- Código lógico: JavaScript
- El objetivo de reAct Native no es escribir el código una vez y ejecutarlo en cualquier plataforma, en lugar de aprender-una vez (la forma reAct) y de escribir en cualquier lugar.
- La comunidad ha agregado herramientas como exponer y crear una aplicación nativa de reAct para ayudar a los usuarios que desean compilar aplicaciones sin usar Xcode ni Android Studio.
- De forma similar a Xamarin (C#), reAct Native (JavaScript) llama a los elementos de la interfaz de usuario nativa (sin necesidad de escribir Java/Kotlin o SWIFT).

[Aplicaciones web progresivas (PWA)](pwa.md)

- Código de interfaz de usuario: HTML, CSS, JavaScript
- Código lógico: JavaScript
- PWA son aplicaciones web compiladas con patrones estándar para que puedan aprovechar las ventajas de las características de aplicaciones web y nativas. Se pueden compilar sin un marco, pero hay un par de Marcos populares que se deben considerar: [iónico](https://ionicframework.com/docs/intro) y [PhoneGap](https://phonegap.com/about/).
- PWA se puede instalar en un dispositivo (Android, iOS o Windows) y puede funcionar sin conexión gracias a la incorporación de un trabajador de servicio.
- PWA se puede distribuir e instalar sin una tienda de aplicaciones usando solo una dirección URL Web. Los Microsoft Store y Google Play Store permiten que PWA se muestre en la lista, ya que actualmente no se puede instalar en un dispositivo iOS que ejecute 12,2 o posterior.
- Para obtener más información, consulte esta [Introducción a PWA](https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Introduction) en MDN.

## <a name="game-development"></a>Desarrollo de juegos

El desarrollo de juegos para Android suele ser único en el desarrollo de una aplicación Android estándar, ya que los juegos suelen usar la lógica de representación personalizada, que a menudo se escribe en OpenGL o Vulkan. Por esta razón, y debido a las muchas bibliotecas de C disponibles que admiten el desarrollo de juegos, es habitual que los desarrolladores usen [C/C++ con Visual Studio](https://docs.microsoft.com/cpp/cross-platform/?view=vs-2019), junto con el [Kit de desarrollo nativo (NDK)](https://docs.microsoft.com/cpp/cross-platform/create-an-android-native-activity-app?view=vs-2019)de Android, para crear juegos para Android. [Introducción a C/C++ para el desarrollo de juegos](native-android.md#use-c-or-c-for-android-game-development).

Otra ruta de acceso común para desarrollar juegos para Android es usar un motor de juegos. Hay muchos motores gratuitos y de código abierto, como [Unity con Visual Studio](https://docs.microsoft.com/visualstudio/cross-platform/visual-studio-tools-for-unity?view=vs-2019), no [real Engine](https://docs.unrealengine.com/en-US/Platforms/Mobile/Android/GettingStarted/index.html), [monogame con Xamarin](https://docs.microsoft.com/xamarin/graphics-games/monogame/introduction/), [UrhoSharp con](https://docs.microsoft.com/xamarin/graphics-games/urhosharp/introduction)Xamarin, [SkiaSharp con Xamarin. Forms](https://docs.microsoft.com/xamarin/xamarin-forms/user-interface/graphics/skiasharp/) CocoonJS, App Game kit, Fusion, Corona SDK, cocos 2D, etc.

## <a name="next-steps"></a>Pasos siguientes

- [Introducción al desarrollo de Android nativo en Windows](native-android.md)
- [Introducción al desarrollo para Android con Xamarin. Android](xamarin-android.md)
- [Introducción al desarrollo para Android con Xamarin. Forms](xamarin-forms.md)
- [Introducción al desarrollo para Android con reAct Native](react-native.md)
- [Introducción al desarrollo de un PWA para Android](pwa.md)
- [Desarrollo de aplicaciones de pantalla dual para Android y obtención del SDK de dispositivo Surface Duo](https://docs.microsoft.com/dual-screen/android/)
- [Agregar exclusiones de Windows Defender para mejorar el rendimiento](defender-settings.md)
- [Habilitar la compatibilidad con la virtualización para mejorar el rendimiento del emulador](emulator.md#enable-virtualization-support)
