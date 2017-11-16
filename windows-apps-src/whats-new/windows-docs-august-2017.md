---
author: QuinnRadich
title: 'Novedades en los documentos de Windows de agosto de 2017: desarrollar aplicaciones para UWP'
description: "Se han agregado nuevas características, vídeos y directrices para los desarrolladores a la documentación de desarrollador de Windows 10 de agosto de 2017."
keywords: "novedades, actualización, características, directrices para los desarrolladores, Windows 10, 1708"
ms.author: quradic
ms.date: 08/03/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
redirect_url: windows-docs-latest
ms.openlocfilehash: 43eebdc73f626034d9aaea12d18bdb6ce8657d90
ms.sourcegitcommit: 6c6f3c265498d7651fcc4081c04c41fafcbaa5e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2017
---
# <a name="whats-new-in-the-windows-developer-docs-in-august-2017"></a>Novedades de los documentos de Windows de agosto de 2017

La documentación del desarrollador de Windows se actualiza constantemente con información sobre las nuevas características disponibles para los desarrolladores a través de la plataforma de Windows. La siguiente información general sobre las características, directrices para los desarrolladores y los vídeos están a tu disposición desde hace poco y contienen información nueva y actualizada para los desarrolladores de Windows.

[Instala las herramientas y el SDK](http://go.microsoft.com/fwlink/?LinkId=821431) en Windows 10 y estarás listo para [crear una nueva aplicación universal de Windows](../get-started/your-first-app.md) o para aprender a usar el [código de aplicación existente en Windows](../porting/index.md).

## <a name="features"></a>Características

### <a name="windows-template-studio"></a>Windows Template Studio

Usa la nueva extensión de [Windows Template Studio](https://aka.ms/wtsinstall) para Visual Studio 2017 para crear rápidamente una aplicación para UWP con las páginas, el marco y las características que desees. Esta experiencia basada en un asistente implementa patrones demostrados y procedimientos recomendados para ahorrar tiempo y problemas al agregar características a tu aplicación.

![Windows Template Studio](images/template-studio.png)

### <a name="conditional-xaml"></a>XAML condicional

Ya tienes acceso a una vista previa del [XAML condicional](../debug-test-perf/conditional-xaml.md) para crear [aplicaciones adaptables para versiones](../debug-test-perf/version-adaptive-apps.md). El XAML condicional te permite usar el método ApiInformation.IsApiContractPresent en el marcado XAML para establecer propiedades y crear una instancia de los objetos con un marcado basado en la presencia de una API, sin necesidad de usar código subyacente.

### <a name="game-mode"></a>Modo de juego

La API [Modo de juego](https://msdn.microsoft.com/library/windows/desktop/mt808808) para la Plataforma universal de Windows (UWP) te permite crear la experiencia de juego más optimizada al aprovechar el modo de juego en Windows 10. Estas API se encuentran en el encabezado <expandedresources.h>.

![Modo de juego](images/game-mode.png)

### <a name="submission-api-supports-video-trailers-and-gaming-options"></a>La API de envío es compatible con los tráileres de los vídeos y las opciones de juego.

La [API de envío de la Tienda Windows](../monetize/create-and-manage-submissions-using-windows-store-services.md) ahora te permite incluir [tráileres de vídeo](../monetize/manage-app-submissions.md#trailer-object) y [opciones de juego](../monetize/manage-app-submissions.md#gaming-options-object) con los envíos de aplicaciones.


## <a name="developer-guidance"></a>Guía para desarrolladores

### <a name="data-schemas-for-store-products"></a>Esquemas de datos para productos de la Tienda

Hemos agregado el artículo [Esquemas de datos para productos de la Tienda](../monetize/data-schemas-for-store-products.md). Este artículo proporciona esquemas para los datos relacionados con la Tienda Windows disponibles para varios objetos en el espacio de nombres [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx), incluidos [StoreProduct](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct) y [StoreAppLicense](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense).

### <a name="desktop-bridge"></a>Puente de dispositivo de escritorio

Hemos agregado dos guías que te ayudarán a agregar experiencias modernas que destacan para los usuarios de Windows 10.

Consulta [Mejorar tu aplicación de escritorio para Windows 10](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-enhance) para encontrar y hacer referencia a los archivos correctos y, a continuación, escribe el código para mejorar las experiencias de UWP para los usuarios de Windows 10.  

Consulta [Ampliar tu aplicación de escritorio con componentes de UWP modernos](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-extend) para incorporar modernas interfaces de usuario basadas en XAML y otras experiencias de UWP que deben ejecutarse en un contenedor de aplicación para UWP.

### <a name="getting-started-with-point-of-service"></a>Introducción al punto de servicio

Hemos agregado una nueva guía para que te sirva de [introducción con los dispositivos con un punto de servicio](https://docs.microsoft.com/en-us/windows/uwp/devices-sensors/pos-get-started). Trata de temas como la enumeración de dispositivos, la comprobación de las funcionalidades del dispositivo, la reclamación de los dispositivos y el uso compartido. 


## <a name="videos"></a>Vídeos

### <a name="mixed-reality"></a>Realidad mixta

Se ha publicado un grupo de nuevos vídeos de tutoriales para [el curso 250 de Microsoft HoloLens](https://developer.microsoft.com/en-us/windows/mixed-reality/mixed_reality_250). Si ya has instalado las herramientas y te has familiarizado con los conceptos básicos del desarrollo de la realidad mixta, echa un vistazo a estos cursos en vídeo para obtener más información sobre cómo crear experiencias compartidas en los dispositivos de realidad mixta.

### <a name="narrator-and-dev-mode"></a>Narrador y el modo de desarrollador

Probablemente ya sepas que puedes usar el [Narrador](https://support.microsoft.com/help/22798/windows-10-narrator-get-started) para probar la experiencia de lectura en la pantalla de tu aplicación. El Narrador también te ofrece el modo de desarrollador que proporciona una buena representación visual de la información expuesta. [Ve el vídeo](https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-Narrator-and-Dev-Mode) y obtén más información sobre el [modo de desarrollador del Narrador](https://channel9.msdn.com/Blogs/One-Dev-Minute/Using-Narrator-and-Dev-Mode).

### <a name="windows-template-studio"></a>Windows Template Studio

Se proporciona una visión general más detallada de Windows Template Studio en [este vídeo](https://channel9.msdn.com/Blogs/One-Dev-Minute/Getting-Started-with-Windows-Template-Studio). Cuando estés listo, [instala la extensión](https://aka.ms/wtsinstall) o [echa un vistazo a la documentación y el código fuente](https://aka.ms/wtsinstall).