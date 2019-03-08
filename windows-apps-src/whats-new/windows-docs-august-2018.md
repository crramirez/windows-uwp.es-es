---
title: 'Novedades de Windows Docs en agosto de 2018: desarrollar aplicaciones para UWP'
description: Instrucciones para desarrolladores, vídeos, ejemplos y las nuevas características se agregaron a la documentación para desarrolladores de Windows 10 de agosto de 2018.
keywords: Novedades, update, características, instrucciones para desarrolladores, Windows 10, agosto
ms.date: 08/14/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9922aa1ad2442153dcc2c13d05520c05c3b56d31
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57616490"
---
# <a name="whats-new-in-the-windows-developer-docs-in-august-2018"></a>Novedades en la documentación para desarrolladores de Windows de agosto de 2018

La documentación del desarrollador de Windows se actualiza constantemente con información sobre las nuevas características disponibles para los desarrolladores a través de la plataforma de Windows. La introducción a las características, instrucciones para desarrolladores y vídeos siguientes se realizaron disponibles en el mes de agosto.

[Instala las herramientas y el SDK](https://go.microsoft.com/fwlink/?LinkId=821431) en Windows 10 y estarás listo para [crear una nueva aplicación universal de Windows](../get-started/create-uwp-apps.md) o para explorar cómo puedes usar tu [código de aplicación existente en Windows](../porting/index.md).

## <a name="features"></a>Características

### <a name="design"></a>Diseño

Las siguientes características se agregaron a la Windows compilaciones de Insider Preview, disponibles a través de la [Windows Insider](https://insider.windows.com/) programa.

* El [biblioteca de interfaz de usuario de Windows](https://aka.ms/winui-docs) es un conjunto de paquetes de NuGet que proporcionan los elementos del otro usuario y controles para aplicaciones UWP. Estos paquetes también son compatible con versiones anteriores de Windows 10, por lo que la aplicación funciona incluso si los usuarios no tienen la versión más reciente del sistema operativo.

* [DropDownButton](../design/controls-and-patterns/buttons.md#create-a-drop-down-button), [SplitButton](../design/controls-and-patterns/buttons.md#create-a-split-button), y [ToggleSplitButton](../design/controls-and-patterns/buttons.md#create-a-toggle-split-button) proporcionar controles de botón con características especializadas para mejorar la interfaz de usuario de la aplicación.

![Un botón de expansión para seleccionar el color de primer plano](../design/controls-and-patterns/images/split-button-rtb.png)

* Control NavigationView ahora admite [principales navegación](../design/controls-and-patterns/navigationview.md), para los casos en que la aplicación tiene un número menor de las opciones de navegación y requieren más espacio para el contenido de la aplicación.

* Vista de árbol se ha mejorado para admitir [plantillas, de elemento de enlace de datos y arrastrar y colocar.](../design/controls-and-patterns/tree-view.md)

### <a name="package-support-framework"></a>Marco de compatibilidad de paquete

El marco de soporte técnico de paquete es un kit de código abierto que le permite aplicar correcciones a la aplicación de win32 cuando no tienes acceso al código fuente, para que se pueda ejecutar en un contenedor MSIX.

Para obtener más información, consulte [en tiempo de ejecución de aplicar correcciones para un paquete MSIX utilizando el marco de trabajo de paquete de soporte técnico](../porting/package-support-framework.md).

## <a name="developer-guidance"></a>Guía para desarrolladores

### <a name="web-api-extensions"></a>Extensiones de API Web

Una lista de [extensiones heredadas de API de Microsoft](https://developer.mozilla.org/docs/Web/API/Microsoft_API_extensions) se ha agregado a la documentación de Mozilla Developer Network para el desarrollo web entre exploradores. Estas extensiones de API son únicas en Internet Explorer o Microsoft Edge y complementan la información sobre la compatibilidad con explorador y compatibilidad en la documentación de web MDN existente. Microsoft heredada [extensiones CSS](https://developer.mozilla.org/docs/Web/CSS/Microsoft_Extensions) y [extensiones de JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Microsoft_JavaScript_extensions) también están disponibles y se pueden encontrar web enriquecidas de información de la API de MDN se expone directamente en [Visual Studio Code.](https://code.visualstudio.com/updates/v1_25#_new-css-pseudo-selectors-and-pseudo-elements-from-mdn)

### <a name="cwinrt-code-examples"></a>C++ / c++ / ejemplos de código de WinRT

Hemos agregado 250 [C++ / c++ / WinRT](../cpp-and-winrt-apis/index.md) anuncios a temas en los documentos, que acompaña a C++ / de código c++ / ejemplos de código CX.

### <a name="project-rome"></a>Proyecto Roma

El [docs proyecto Roma](https://docs.microsoft.com/windows/project-rome/) se ha reorganizado el sitio en un enfoque centrado en la característica. Esto debería facilitar a los desarrolladores para encontrar lo busca y para implementar características de su elección en varias plataformas.

## <a name="videos"></a>Vídeos

### <a name="xbox-live-unity-plugin"></a>Complemento de Unity de Xbox Live

El complemento de Xbox Live para Unity contiene compatibilidad para agregar la firma de Xbox Live, estadísticas, listas de amigos, almacenamiento en la nube y marcadores en su título. [Vea el vídeo](https://youtu.be/fVQZ-YgwNpY) para obtener más información, a continuación, [descargar el paquete de GitHub](https://aka.ms/UnityPlugin) para empezar a trabajar.

### <a name="one-dev-question"></a>Una pregunta de desarrollo

En la serie de vídeos de una pregunta de desarrollo, los desarrolladores de Microsoft ha cubren una serie de preguntas acerca del desarrollo de Windows, referencia cultural del equipo y el historial. Aquí es que hemos respondido a las preguntas más recientes.

Raymond Chen:

* [¿Cómo sabe el kernel de cuándo se debe reiniciar un controlador de vídeo?](https://youtu.be/3SNAdyO1l5c)

Larry Osterman:

* [¿Qué es la historia detrás del objeto Burgermaster en Windows?](https://youtu.be/0TDSbyAIvX0)