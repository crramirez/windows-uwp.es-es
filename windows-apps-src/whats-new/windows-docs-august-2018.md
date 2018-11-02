---
author: QuinnRadich
title: 'Novedades en los documentos de Windows de agosto de 2018: desarrollar aplicaciones para UWP'
description: Se agregaron nuevas características, vídeos, muestras y directrices para los desarrolladores a la documentación del desarrollador de Windows 10 de agosto de 2018.
keywords: Novedades, actualización, características, directrices para los desarrolladores, Windows 10, agosto
ms.author: quradic
ms.date: 08/14/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 76de6c5c1e8925dd8b166a8d99c39116bf66141d
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2018
ms.locfileid: "5977922"
---
# <a name="whats-new-in-the-windows-developer-docs-in-august-2018"></a>Novedades en los documentos de Windows de agosto de 2018

La documentación del desarrollador de Windows se actualiza constantemente con información sobre las nuevas características disponibles para los desarrolladores a través de la plataforma de Windows. La siguiente información general de características, directrices para los desarrolladores y vídeos que se han puesto a disposición en el mes de agosto.

[Instala las herramientas y el SDK](http://go.microsoft.com/fwlink/?LinkId=821431) en Windows 10 y estarás listo para [crear una nueva aplicación universal de Windows](../get-started/create-uwp-apps.md) o para aprender a usar el [código de aplicación existente en Windows](../porting/index.md).

## <a name="features"></a>Características

### <a name="design"></a>Diseñar

Se han agregado las siguientes características de Windows compilaciones de Insider Preview, disponibles a través del programa [Windows Insider](https://insider.windows.com/) .

* La [Biblioteca de la interfaz de usuario de Windows](https://aka.ms/winui-docs) es un conjunto de paquetes de NuGet que proporcionan controles y otros elementos del usuario para aplicaciones para UWP. Estos paquetes también son compatibles con versiones anteriores de Windows 10, por lo que la aplicación funciona incluso si los usuarios no tengan la versión del sistema operativo más reciente.

* [DropDownButton](../design/controls-and-patterns/buttons.md#create-a-drop-down-button), [botón de división](../design/controls-and-patterns/buttons.md#create-a-split-button)y [ToggleSplitButton](../design/controls-and-patterns/buttons.md#create-a-toggle-split-button) proporcionan los controles de botón con características especializadas para mejorar la interfaz de usuario de la aplicación.

![Un botón en dos paneles para seleccionar el color de primer plano](../design/controls-and-patterns/images/split-button-rtb.png)

* NavigationView ahora admite la [navegación superior](../design/controls-and-patterns/navigationview.md), para los casos en los que la aplicación tiene un menor número de opciones de exploración y requieren más espacio para el contenido de la aplicación.

* Vista de árbol se ha mejorado para admitir [enlace de datos, plantillas, de elementos y arrastrar y colocar.](../design/controls-and-patterns/tree-view.md)

### <a name="package-support-framework"></a>Marco de soporte técnico de paquete

El marco de soporte técnico de paquete es un kit de código abierto que ayuda a aplicar correcciones a la aplicación de win32 cuando no tienes acceso al código fuente, para que se puede ejecutar en un contenedor de MSIX.

Para obtener más información, consulta [en tiempo de ejecución de aplicar correcciones a un paquete de MSIX con el marco de soporte técnico del paquete](../porting/package-support-framework.md).

## <a name="developer-guidance"></a>Guía para desarrolladores

### <a name="web-api-extensions"></a>Extensiones de API Web

Se ha agregado una lista de [extensiones de API de Microsoft heredadas](https://developer.mozilla.org/docs/Web/API/Microsoft_API_extensions) a la documentación de Mozilla Developer Network de desarrollo y exploradores web. Estas extensiones de API son exclusivas de Internet Explorer o Microsoft Edge y complementan existente información sobre la compatibilidad de explorador y compatibilidad de los documentos de web MDN. Microsoft heredadas [extensiones CSS](https://developer.mozilla.org/docs/Web/CSS/Microsoft_Extensions) y [JavaScript extensiones](https://developer.mozilla.org/docs/Web/JavaScript/Microsoft_JavaScript_extensions) también están disponibles y puedes encontrar web enriquecidas información sobre la API de MDN expone directamente en [Visual Studio Code.](https://code.visualstudio.com/updates/v1_25#_new-css-pseudo-selectors-and-pseudo-elements-from-mdn)

### <a name="cwinrt-code-examples"></a>C++ / ejemplos de código de WinRT

Hemos agregado 250 [C++ / WinRT](../cpp-and-winrt-apis/index.md) código descripciones de los temas de nuestros documentos, elementos complementarios existentes C++ / ejemplos de código CX.

### <a name="project-rome"></a>Proyecto Roma

El sitio de [documentos de Project Rome](https://docs.microsoft.com/windows/project-rome/) ha se han reorganizado en un enfoque de la característica en primer lugar. Esto hará que sea más fácil para los desarrolladores para encontrar lo que buscan y la implementación de las características de su elección en varias plataformas.

## <a name="videos"></a>Vídeos

### <a name="xbox-live-unity-plugin"></a>Complemento de Xbox Live Unity

El complemento de Xbox Live para Unity contiene compatibilidad para agregar la firma de Xbox Live, estadísticas, listas de amigos, almacenamiento en la nube y marcadores a tu título. [Ve el vídeo](https://youtu.be/fVQZ-YgwNpY) para obtener más información y [descargar el paquete de GitHub](https://aka.ms/UnityPlugin) para empezar a trabajar.

### <a name="one-dev-question"></a>Una pregunta de desarrollo

En la serie de vídeos de una pregunta de desarrollo, los desarrolladores de Microsoft siempre cubren una serie de preguntas frecuentes sobre el desarrollo de Windows, referencia cultural de equipo e historial. Aquí es que hemos respondido a las preguntas más recientes.

Raymond Chen:

* [¿Cómo sabe el kernel de cuándo se debe reiniciar un controlador de vídeo?](https://youtu.be/3SNAdyO1l5c)

Larry Osterman:

* [¿Qué es la historia detrás del objeto Burgermaster en Windows?](https://youtu.be/0TDSbyAIvX0)