---
title: 'Novedades en los documentos de Windows de agosto de 2018: desarrollar aplicaciones para UWP'
description: Se han agregado nuevas características, vídeos, ejemplos e instrucciones para desarrolladores en la documentación de agosto de 2018 para desarrolladores de Windows 10.
keywords: novedades, actualización, actualizaciones, características, instrucciones para desarrolladores, Windows 10, agosto
ms.date: 08/14/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 723cae783ba16fe5be9bb2076f96d4a5f823d733
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258839"
---
# <a name="whats-new-in-the-windows-developer-docs-in-august-2018"></a>Novedades en los documentos para desarrolladores de Windows de agosto de 2018

La documentación para desarrolladores de Windows se actualiza constantemente con información sobre las nuevas características disponibles para los desarrolladores a través de la plataforma de Windows. Las siguientes descripciones de características, instrucciones para desarrolladores y vídeos se han puesto a disposición de los desarrolladores en agosto.

[Instala las herramientas y el SDK](https://developer.microsoft.com/windows/downloads#_blank) en Windows 10 y estarás listo para [crear una nueva aplicación universal de Windows](../get-started/create-uwp-apps.md) o para explorar cómo puedes usar tu [código de aplicación existente en Windows](../porting/index.md).

## <a name="features"></a>Características

### <a name="design"></a>Diseño

Se agregaron las siguientes características a las compilaciones de Windows Insider Preview, disponibles a través del Programa [Windows Insider](https://insider.windows.com/).

* La [Biblioteca de interfaz de usuario de Windows](https://docs.microsoft.com/uwp/toolkits/winui/) es un conjunto de paquetes NuGet que proporcionan controles y otros elementos de interfaz de usuario para aplicaciones para UWP. Estos paquetes también son compatibles con las versiones anteriores de Windows 10, por lo que la aplicación funciona incluso si los usuarios no tienen la versión más reciente del sistema operativo.

* [DropDownButton](../design/controls-and-patterns/buttons.md#create-a-drop-down-button), [SplitButton](../design/controls-and-patterns/buttons.md#create-a-split-button), [ToggleSplitButton](../design/controls-and-patterns/buttons.md#create-a-toggle-split-button) proporcionan controles de botón con características especializadas para mejorar la interfaz de usuario de la aplicación.

![Botón de expansión para seleccionar el color de primer plano](../design/controls-and-patterns/images/split-button-rtb.png)

* El control NavigationView ahora admite la [navegación superior](../design/controls-and-patterns/navigationview.md), para aquellos casos en que la aplicación tiene un número menor de opciones de navegación y requiere más espacio para el contenido.

* El control TreeView se ha mejorado para admitir [el enlace de datos, las plantillas de elementos y la acción de arrastrar y colocar](../design/controls-and-patterns/tree-view.md).

### <a name="package-support-framework"></a>Marco de compatibilidad de paquete

El marco de compatibilidad de paquete es un kit de código abierto que te ayuda a aplicar correcciones a tu aplicación Win32 cuando no tienes acceso al código fuente, para que pueda ejecutarse en un contenedor de MSIX.

Para conocer más detalles, consulta [Apply runtime fixes to an MSIX package by using the Package Support Framework](../porting/package-support-framework.md) (Aplicar correcciones en tiempo de ejecución a un paquete MSIX mediante el Marco de compatibilidad de paquete).

## <a name="developer-guidance"></a>Guía para desarrolladores

### <a name="web-api-extensions"></a>Extensiones de API web

Se ha agregado una lista de [extensiones heredadas de las API de Microsoft](https://developer.mozilla.org/docs/Web/API/Microsoft_API_extensions) a la documentación de Mozilla Developer Network para el desarrollo web entre exploradores. Estas extensiones de API son exclusivas de Internet Explorer o Microsoft Edge, y complementan la información existente sobre compatibilidad del explorador en la documentación web de MDN. Las [extensiones CSS](https://developer.mozilla.org/docs/Web/CSS/Microsoft_Extensions) y las [extensiones de JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Microsoft_JavaScript_extensions) heredadas de Microsoft también están disponibles, y se puede encontrar información valiosa sobre la API web de MDN expuesta directamente en [Visual Studio Code](https://code.visualstudio.com/updates/v1_25#_new-css-pseudo-selectors-and-pseudo-elements-from-mdn).

### <a name="cwinrt-code-examples"></a>Ejemplos de código de C++/WinRT

Hemos agregado 250 listados de código de [C++/WinRT](../cpp-and-winrt-apis/index.md) en los temas de nuestros documentos, que se suman a los ejemplos de código de C++/CX existentes.

### <a name="project-rome"></a>Proyecto Roma

El sitio de [documentos de Proyecto Roma](https://docs.microsoft.com/windows/project-rome/) se ha reorganizado con un enfoque centrado en las características. Gracias a este cambio, los desarrolladores ahora deberían encontrar lo que buscan e implementar las características que elijan en varias plataformas con mayor facilidad.

## <a name="videos"></a>Vídeos

### <a name="xbox-live-unity-plugin"></a>Complemento de Xbox Live para Unity

El complemento de Xbox Live para Unity incluye compatibilidad para agregar firma, estadísticas, listas de amigos, almacenamiento en la nube y marcadores de Xbox Live en tu título. [Mira el vídeo](https://youtu.be/fVQZ-YgwNpY) para obtener más información y, luego, [descarga el paquete de GitHub](https://aka.ms/UnityPlugin) para empezar a trabajar.

### <a name="one-dev-question"></a>Una pregunta de desarrollador

En la serie de vídeos de una pregunta de desarrollador, los desarrolladores de Microsoft cubren una serie de preguntas acerca del desarrollo de Windows, la cultura del equipo y el historial. A continuación encontrarás las preguntas más recientes que hemos respondido.

Raymond Chen:

* [¿Cómo sabe el kernel cuándo se debe reiniciar un controlador de vídeo?](https://youtu.be/3SNAdyO1l5c)

Larry Osterman:

* [¿Cuál es la historia detrás del objeto Burgermaster de Windows?](https://youtu.be/0TDSbyAIvX0)