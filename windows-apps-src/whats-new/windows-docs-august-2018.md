---
author: QuinnRadich
title: 'Novedades de Windows documentos en agosto de 2018: desarrollar aplicaciones UWP'
description: Se han agregado nuevas características, vídeos, ejemplos y orientación para el programador la documentación para desarrolladores de Windows 10 de 2018 de agosto.
keywords: ¿Qué es nuevo, actualización, funciones, instrucciones de desarrollo, 10 de Windows, agosto
ms.author: quradic
ms.date: 08/14/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: c294dedc8e19605bc2cee0308022bed8624df57e
ms.sourcegitcommit: f2f4820dd2026f1b47a2b1bf2bc89d7220a79c1a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2018
ms.locfileid: "2787790"
---
# <a name="whats-new-in-the-windows-developer-docs-in-august-2018"></a>Novedades en los documentos para desarrolladores de Windows en agosto de 2018

La documentación del desarrollador de Windows se actualiza constantemente con información sobre las nuevas características disponibles para los desarrolladores a través de la plataforma de Windows. La siguiente información general de características, instrucciones de desarrollo y vídeos esté a disposición en el mes de agosto.

[Instala las herramientas y el SDK](http://go.microsoft.com/fwlink/?LinkId=821431) en Windows 10 y estarás listo para [crear una nueva aplicación universal de Windows](../get-started/create-uwp-apps.md) o para aprender a usar el [código de aplicación existente en Windows](../porting/index.md).

## <a name="features"></a>Características

### <a name="design"></a>Diseñar

Se han agregado las siguientes características a las ventanas compilaciones de vista previa de la información confidencial, disponibles a través del programa de [Información confidencial de Windows](https://insider.windows.com/) .

* La [Biblioteca de la interfaz de usuario de Windows](https://aka.ms/winui-docs) es un conjunto de paquetes de NuGet que proporcionan controles y otros elementos del usuario para las aplicaciones UWP. Estos paquetes también son compatibles con versiones anteriores de Windows 10, por lo que la aplicación funciona incluso si los usuarios no tienen la versión más reciente del sistema operativo.

* [DropDownButton](../design/controls-and-patterns/buttons.md#create-a-drop-down-button), [botón de división](../design/controls-and-patterns/buttons.md#create-a-split-button)y [ToggleSplitButton](../design/controls-and-patterns/buttons.md#create-a-toggle-split-button) proporcionan controles de botón con características especializadas para mejorar la interfaz de usuario de su aplicación.

![Un botón de expansión para seleccionar el color de primer plano](../design/controls-and-patterns/images/split-button-rtb.png)

* NavigationView ahora admite la [navegación superior](../design/controls-and-patterns/navigationview.md), para los casos en que la aplicación tiene un número menor de opciones de navegación y requieren más espacio para el contenido de su aplicación.

* TreeView se ha mejorado para admitir [enlace de datos, plantillas, de elemento y arrastrar y colocar.](../design/controls-and-patterns/tree-view.md)

### <a name="package-support-framework"></a>Marco de apoyo de paquete

El marco de soporte técnico de paquete es un kit de código abierto que le ayudará a aplicar revisiones a la aplicación de win32 cuando no tiene acceso al código fuente, por lo que se puede ejecutar en un contenedor MSIX.

Para obtener más información, vea [en tiempo de ejecución de aplicar revisiones a un paquete de MSIX mediante el marco de soporte técnico de paquete](../porting/package-support-framework.md).

## <a name="developer-guidance"></a>Guía para desarrolladores

### <a name="web-api-extensions"></a>Extensiones Web API

Una lista de [extensiones de API de Microsoft heredadas](https://developer.mozilla.org/docs/Web/API/Microsoft_API_extensions) se ha agregado a la documentación de Mozilla Developer Network para el desarrollo de entre exploradores web. Estas extensiones de API son únicas en Internet Explorer o Microsoft Edge y complementan existente información sobre la compatibilidad y Examinador de compatibilidad en los documentos de web MDN. Heredado de Microsoft [extensiones de CSS](https://developer.mozilla.org/docs/Web/CSS/Microsoft_Extensions) y [JavaScript extensiones](https://developer.mozilla.org/docs/Web/JavaScript/Microsoft_JavaScript_extensions) también están disponibles, y puede encontrar información sobre las API de MDN contenidos web avanzados de exponer directamente en [código de Visual Studio.](https://code.visualstudio.com/updates/v1_25#_new-css-pseudo-selectors-and-pseudo-elements-from-mdn)

### <a name="cwinrt-code-examples"></a>C + + / ejemplos de código WinRT

Hemos agregado 250 [C + + / WinRT](../cpp-and-winrt-apis/index.md) código listados temas en nuestros documentos, que acompaña a existente C + + / ejemplos de código de CX.

### <a name="project-rome"></a>Proyecto Roma

Se ha reorganizado el sitio de [proyecto Roma documentos](https://docs.microsoft.com/windows/project-rome/) en un enfoque de característica en primer lugar. Esto debe facilitar a los desarrolladores para encontrar lo que están buscando y para implementar las características de su elección en varias plataformas.

## <a name="videos"></a>Vídeos

### <a name="xbox-live-unity-plugin"></a>Complemento de unidad de Xbox Live

El complemento de Xbox Live para unidad contiene soporte para agregar firma Xbox Live, estadísticas, listas de amigos, almacenamiento en la nube y tablas de clasificación a su título. [Vea el vídeo](https://youtu.be/fVQZ-YgwNpY) para obtener más información, a continuación, [Descargue el paquete de depósito](https://aka.ms/UnityPlugin) para empezar a trabajar.

### <a name="one-dev-question"></a>Una pregunta de desarrollo

En la serie de vídeos de una pregunta de desarrollo, los desarrolladores de Microsoft siempre describe una serie de preguntas acerca del desarrollo de Windows, referencia cultural de equipo e historial. Aquí es las preguntas más recientes que respondemos!

Raymond Chen:

* [¿Cómo sabe cuándo se debe reiniciar un controlador de vídeo el núcleo?](https://youtu.be/3SNAdyO1l5c)

Larry Osterman:

* [¿Qué es la historia detrás del objeto Burgermaster en Windows?](https://youtu.be/0TDSbyAIvX0)