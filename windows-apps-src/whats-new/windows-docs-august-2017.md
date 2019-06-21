---
title: 'Novedades en los documentos de Windows de agosto de 2017: desarrollar aplicaciones para UWP'
description: Se han agregado nuevas características, vídeos y directrices para desarrolladores a la documentación para los desarrolladores de Windows 10 de agosto de 2017.
keywords: novedades, actualización, características, directrices para los desarrolladores, Windows 10, 1708
ms.date: 08/03/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 53bd11950d30e3924d6d196e911fa05208d0f5ec
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66371917"
---
# <a name="whats-new-in-the-windows-developer-docs-in-august-2017"></a>Novedades de los documentos de los desarrolladores de Windows de agosto de 2017

La documentación para desarrolladores de Windows se actualiza constantemente con información sobre las nuevas características disponibles para los desarrolladores a través de la plataforma de Windows. Las siguientes introducciones de características, directrices para los desarrolladores y vídeos están a tu disposición desde hace poco y contienen información nueva y actualizada para los desarrolladores de Windows.

[Instala las herramientas y el SDK](https://go.microsoft.com/fwlink/?LinkId=821431) en Windows 10 y estarás listo para [crear una nueva aplicación universal de Windows](../get-started/your-first-app.md) o para explorar cómo puedes usar tu [código de aplicación existente en Windows](../porting/index.md).

## <a name="features"></a>Características

### <a name="windows-template-studio"></a>Windows Template Studio

Usa la nueva extensión de [Windows Template Studio](https://aka.ms/wtsinstall) para Visual Studio 2017 para crear rápidamente una aplicación para UWP con las páginas, el marco y las características que desees. Esta experiencia basada en un asistente implementa patrones demostrados y procedimientos recomendados para ahorrar tiempo y problemas al agregar características a tu aplicación.

![Windows Template Studio](images/template-studio.png)

### <a name="conditional-xaml"></a>XAML condicional

Ya puedes obtener una vista previa del [XAML condicional](../debug-test-perf/conditional-xaml.md) para crear [aplicaciones adaptables para versiones](../debug-test-perf/version-adaptive-apps.md). El XAML condicional te permite usar el método ApiInformation.IsApiContractPresent en la revisión XAML para establecer propiedades y crear una instancia de los objetos en la revisión en función de la presencia de una API, sin necesidad de usar código subyacente.

### <a name="game-mode"></a>Modo de juego

Las API [Modo Juego](https://docs.microsoft.com/previous-versions/windows/desktop/gamemode/game-mode-portal) para la Plataforma universal de Windows (UWP) te permiten crear la experiencia de juego más optimizada al aprovechar Modo Juego en Windows 10. Estas API se encuentran en el encabezado **&lt;expandedresources.h&gt;** .

![Modo de juego](images/game-mode.png)

### <a name="submission-api-supports-video-trailers-and-gaming-options"></a>La API de envío es compatible con los tráileres de los vídeos y las opciones de juego.

La [API de envío de Microsoft Store](../monetize/create-and-manage-submissions-using-windows-store-services.md) ahora te permite incluir [tráileres de vídeo](../monetize/manage-app-submissions.md#trailer-object) y [opciones de juego](../monetize/manage-app-submissions.md#gaming-options-object) con los envíos de aplicaciones.


## <a name="developer-guidance"></a>Guía para desarrolladores

### <a name="data-schemas-for-store-products"></a>Esquemas de datos para productos de Store

Hemos agregado el artículo [Esquemas de datos para productos de la Store](../monetize/data-schemas-for-store-products.md). En este artículo se proporcionan esquemas para los datos relacionados con Microsoft Store disponibles para varios objetos en el espacio de nombres [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store), incluidos [StoreProduct](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct) y [StoreAppLicense](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense).

### <a name="desktop-bridge"></a>Puente de dispositivo de escritorio

Hemos agregado dos guías que te ayudarán a agregar experiencias modernas que se mejoran para los usuarios de Windows 10.

Consulta [Enhance your desktop application for Windows 10](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-enhance) (Mejorar tu aplicación de escritorio para Windows 10) para encontrar y hacer referencia a los archivos correctos y, a continuación, escribe el código para mejorar las experiencias de UWP para los usuarios de Windows 10.  

Consulta [Ampliar tu aplicación de escritorio con componentes de UWP modernos](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-extend) para incorporar modernas interfaces de usuario basadas en XAML y otras experiencias de UWP que deben ejecutarse en un contenedor de aplicación para UWP.

### <a name="getting-started-with-point-of-service"></a>Introducción al punto de servicio

Hemos agregado una nueva guía para que te sirva de [introducción con los dispositivos de punto de servicio](https://docs.microsoft.com/en-us/windows/uwp/devices-sensors/pos-get-started). Trata de temas como la enumeración de dispositivo, la comprobación de funcionalidades de dispositivo, la reclamación de dispositivos y el uso compartido de dispositivos. 

### <a name="xbox-live"></a>Xbox Live

Hemos agregado documentos para los desarrolladores de Xbox Live, para juegos de UWP y de Kit de desarrollo de Xbox (XDK).

Consulta [Xbox Live developer guide](https://docs.microsoft.com/en-us/windows/uwp/xbox-live/) (Guía para desarrolladores de Xbox Live) para obtener información sobre cómo usar las API de Xbox Live para conectar tu juego a la red social de juegos de Xbox Live.

Con el [Programa de creadores de Xbox Live](https://docs.microsoft.com/en-us/windows/uwp/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators), cualquier desarrollador de juegos de UWP puede desarrollar y publicar un juego habilitado para Xbox Live en PC y Xbox One.

Consulta la [información general del programa de desarrollador de Xbox Live](https://docs.microsoft.com/en-us/windows/uwp/xbox-live/developer-program-overview) para obtener información sobre los programas y características disponibles para los desarrolladores de Xbox Live.

## <a name="videos"></a>Vídeos

### <a name="mixed-reality"></a>Realidad mixta

Se ha publicado una serie de nuevos vídeos de tutoriales para [Microsoft HoloLens Course 250](https://developer.microsoft.com/en-us/windows/mixed-reality/mixed_reality_250). Si ya has instalado las herramientas y te has familiarizado con los conceptos básicos del desarrollo de Mixed Reality, echa un vistazo a estos cursos en vídeo para obtener más información sobre cómo crear experiencias compartidas en los dispositivos de Mixed Reality.

### <a name="narrator-and-dev-mode"></a>Narrador y el modo de desarrollador

Probablemente ya sepas que puedes usar el [Narrador](https://support.microsoft.com/help/22798/windows-10-narrator-get-started) para probar la experiencia de lectura de pantalla de tu aplicación. Pero el Narrador también te ofrece un modo de desarrollador, que proporciona una buena representación visual de la información expuesta. [Mira el vídeo](https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-Narrator-and-Dev-Mode) y obtén más información sobre el [modo de desarrollador del Narrador](https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-Narrator-and-Dev-Mode).

### <a name="windows-template-studio"></a>Windows Template Studio

Se proporciona una visión general más detallada de Windows Template Studio en [este vídeo](https://channel9.msdn.com/Blogs/One-Dev-Minute/Getting-Started-with-Windows-Template-Studio). Cuando estés listo, [instala la extensión](https://aka.ms/wtsinstall) o [echa un vistazo a la documentación y el código fuente](https://aka.ms/wtsinstall).