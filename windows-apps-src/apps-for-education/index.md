---
author: PatrickFarley
title: Desarrolla aplicaciones educativas.
description: En esta sección se describen los recursos para aplicaciones universales de Windows que están disponibles para crear aplicaciones educativas para la plataforma de Windows 10.
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, educación
ms.assetid: 2431f253-efe3-4895-b131-34653b61f13c
ms.localizationpriority: medium
ms.openlocfilehash: da03a3c478ca45cc2d2b518988738e510a6c5ea9
ms.sourcegitcommit: 9e2c34a5ed3134aeca7eb9490f05b20eb9a3e5df
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2018
ms.locfileid: "3988254"
---
# <a name="develop-universal-windows-apps-for-education"></a>Desarrollar aplicaciones universales de Windows para la educación
![captura de pantalla de aplicación de la aplicación hacer un examen](images/take-a-test-screen-small.png)

Los siguientes recursos te ayudarán a crear una aplicación universal de Windows para la educación.

### <a name="accessibility"></a>Accesibilidad
Las aplicaciones educativas deben ser accesibles. Para obtener más información, consulta [Desarrollo de aplicaciones accesibles](https://developer.microsoft.com/windows/accessible-apps).


### <a name="secure-assessments"></a>Evaluaciones seguras
A menudo, las aplicaciones de evaluación y exámenes deberán crear un entorno *sellado* para impedir que los alumnos usen otros equipos o recursos de Internet durante un examen. Esta funcionalidad está disponible a través de la [API Hacer un examen](take-a-test-api.md). Consulta la aplicación web [Hacer un examen](https://technet.microsoft.com/edu/windows/take-tests-in-windows-10) en el centro de TI de Windows para ver un ejemplo de un entorno de exámenes con acceso en línea bloqueado para realizar pruebas de alto riesgo.

### <a name="user-input"></a>Entrada de usuario
La entrada del usuario es una parte fundamental de las aplicaciones educativas; los controles de interfaz de usuario deben ser intuitivos y con capacidad de respuesta para desconcentrar al usuario. Para obtener una visión general de las opciones de entrada disponibles en una aplicación universal de Windows, consulta [Información básica de entradas](https://docs.microsoft.com/windows/uwp/design/input/input-primer) y los temas de la sección "Diseño e interfaz de usuario". Además, las aplicaciones de ejemplo siguientes muestran el control de la interfaz de usuario básico en la Plataforma universal de Windows.
- En [Muestra de entrada básica](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput) se muestra como controlar la entrada en las aplicaciones universales de Windows.
- En [User interaction mode sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode) (Muestra del modo de interacción del usuario) se muestra cómo detectar el modo de interacción del usuario y cómo responder a dicho modo.
- En [Muestra de elementos visuales de foco](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals) se muestra cómo aprovechar los nuevos elementos visuales de foco dibujados por el sistema, o cómo crear elementos visuales de foco personalizados si los dibujados por el sistema no se ajustan a tus necesidades.

La plataforma Windows Ink consigue que las aplicaciones educativas deslumbren adaptándolas a un modo de entrada al que están acostumbrados los alumnos. Consulta [Interacciones de lápiz y Windows Ink](https://docs.microsoft.com/windows/uwp/design/input/pen-and-stylus-interactions) y los temas siguientes para obtener una guía completa para la implementación de Windows Ink en tu aplicación. Las aplicaciones de ejemplo siguientes proporcionan ejemplos funcionales de esta API.
- En [Muestra de entrada de lápiz](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Ink) se muestra cómo usar la funcionalidad de entrada de lápiz (por ejemplo, capturar, manipular e interpretar trazos de lápiz) en aplicaciones universales de Windows con JavaScript.
- En [Inking sample](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleInk) (Muestra de entrada de lápiz) se muestra cómo usar la funcionalidad de entrada de lápiz (por ejemplo, capturar, manipular e interpretar trazos de lápiz) en aplicaciones universales de Windows con JavaScript.
- En [Muestra de entrada de lápiz compleja](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk) se muestra cómo usar la funcionalidad InkPresenter avanzada para intercalar tinta con otros objetos, seleccionar trazos de lápiz, copiar o pegar y controlar eventos. Se basa en la Plataforma universal de Windows en C++ y puede ejecutarse en ambas SKU de escritorio y móvil de Windows 10.


### <a name="microsoft-store"></a>Microsoft Store
A menudo, las aplicaciones educativas se publican en circunstancias especiales para una organización específica. Consulta [Distribuir aplicaciones de LOB en las empresas](https://msdn.microsoft.com/windows/uwp/publish/distribute-lob-apps-to-enterprises) para obtener información sobre este tema.

## <a name="related-topics"></a>Temas relacionados
- [Windows 10 for Education](https://technet.microsoft.com/edu/windows/index) en el centro de TI de Windows
