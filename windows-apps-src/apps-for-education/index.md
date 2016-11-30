---
author: TylerMSFT
title: Desarrolla aplicaciones educativas.
description: "En esta sección se describen los recursos para aplicaciones universales de Windows que están disponibles para crear aplicaciones educativas para la plataforma de Windows 10."
translationtype: Human Translation
ms.sourcegitcommit: 48fcfe2b033614b445a1be6d757a8d208c7b1292
ms.openlocfilehash: bb401b73432c072d551814dec9504a7d1742b7d4

---
# Desarrollar aplicaciones universales de Windows para la educación
Los siguientes recursos te ayudarán a crear una aplicación universal de Windows para la educación.

### Accesibilidad
Las aplicaciones educativas deben ser accesibles. Para obtener más información, consulta [Desarrollo de aplicaciones accesibles](https://developer.microsoft.com/windows/accessible-apps).


### Evaluaciones seguras
A menudo, las aplicaciones de evaluación y exámenes deberán crear un entorno *sellado* para impedir que los alumnos usen otros equipos o recursos de Internet durante un examen. Esta funcionalidad está disponible a través de la [API Hacer un examen](take-a-test-api.md). Consulta la aplicación web [Hacer un examen](https://technet.microsoft.com/edu/windows/take-tests-in-windows-10) en el centro de TI de Windows para ver un ejemplo de un entorno de exámenes con acceso en línea bloqueado para realizar pruebas de alto riesgo.

### Entrada de usuario
La entrada del usuario es una parte fundamental de las aplicaciones educativas; los controles de interfaz de usuario deben ser intuitivos y con capacidad de respuesta para desconcentrar al usuario. Para obtener una visión general de las opciones de entrada disponibles en una aplicación universal de Windows, consulta [Información básica de entradas](https://msdn.microsoft.com/windows/uwp/input-and-devices/input-primer) y los temas de la sección "Diseño e interfaz de usuario". Además, las aplicaciones de ejemplo siguientes muestran el control de la interfaz de usuario básico en la Plataforma universal de Windows.
- En [Muestra de entrada básica](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput) se muestra como controlar la entrada en las aplicaciones universales de Windows.
- En [User interaction mode sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode) (Muestra del modo de interacción del usuario) se muestra cómo detectar el modo de interacción del usuario y cómo responder a dicho modo.
- En [Muestra de elementos visuales de foco](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals) se muestra cómo aprovechar los nuevos elementos visuales de foco dibujados por el sistema, o cómo crear elementos visuales de foco personalizados si los dibujados por el sistema no se ajustan a tus necesidades.

La plataforma Windows Ink consigue que las aplicaciones educativas deslumbren adaptándolas a un modo de entrada al que están acostumbrados los alumnos. Consulta [Interacciones de lápiz y Windows Ink](https://msdn.microsoft.com/windows/uwp/input-and-devices/pen-and-stylus-interactions) y los temas siguientes para obtener una guía completa para la implementación de Windows Ink en tu aplicación. Las aplicaciones de ejemplo siguientes proporcionan ejemplos funcionales de esta API.
- En [Muestra de entrada de lápiz](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Ink) se muestra cómo usar la funcionalidad de entrada de lápiz (por ejemplo, capturar, manipular e interpretar trazos de lápiz) en aplicaciones universales de Windows con JavaScript.
- En [Inking sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleInk) (Muestra de entrada de lápiz) se muestra cómo usar la funcionalidad de entrada de lápiz (por ejemplo, capturar, manipular e interpretar trazos de lápiz) en aplicaciones universales de Windows con JavaScript.
- En [Muestra de entrada de lápiz compleja](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk) se muestra cómo usar la funcionalidad InkPresenter avanzada para intercalar tinta con otros objetos, seleccionar trazos de lápiz, copiar o pegar y controlar eventos. Se basa en la Plataforma universal de Windows en C++ y puede ejecutarse en ambas SKU de escritorio y móvil de Windows 10.


### Tienda Windows
A menudo, las aplicaciones educativas se publican en circunstancias especiales para una organización específica. Consulta [Distribuir aplicaciones de LOB en las empresas](https://msdn.microsoft.com/windows/uwp/publish/distribute-lob-apps-to-enterprises) para obtener información sobre este tema.

## Temas relacionados
- [Windows 10 for Education](https://technet.microsoft.com/edu/windows/index) en el centro de TI de Windows



<!--HONumber=Nov16_HO1-->


