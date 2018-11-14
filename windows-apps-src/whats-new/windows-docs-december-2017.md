---
author: QuinnRadich
title: 'Novedades en los documentos de Windows de diciembre de 2017: desarrollar aplicaciones para UWP'
description: Se han agregado nuevas características, vídeos y directrices para los desarrolladores a la documentación de diciembre de 2017 para los desarrolladores de Windows 10.
keywords: novedades, actualización, características, directrices para los desarrolladores, Windows 10, diciembre
ms.author: quradic
ms.date: 12/14/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 394dbdb5ae9065aa9424a470ad2e9295ddd63c8d
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2018
ms.locfileid: "6671971"
---
# <a name="whats-new-in-the-windows-developer-docs-in-december-2017"></a>Novedades de la documentación de los desarrolladores de Windows de diciembre de 2017

La documentación del desarrollador de Windows se actualiza constantemente con información sobre las nuevas características disponibles para los desarrolladores a través de la plataforma de Windows. La siguiente información general sobre directrices para los desarrolladores y muestras que están a tu disposición después del lanzamiento de Fall Creators Update y contienen información nueva y actualizada para los desarrolladores de Windows.

[Instala las herramientas y el SDK](http://go.microsoft.com/fwlink/?LinkId=821431) en Windows 10 y estarás listo para [crear una nueva aplicación universal de Windows](../get-started/create-uwp-apps.md) o para aprender a usar el [código de aplicación existente en Windows](../porting/index.md).

## <a name="features"></a>Características

### <a name="windows-mixed-reality-enthusiasts-guide"></a>Windows Mixed Reality: Guía para entusiastas

Con los entusiastas de la tecnología como objetivo para animarles a que se adentren en el mundo de la realidad mixta, la [Guía para entusiastas](https://docs.microsoft.com/en-us/windows/mixed-reality/enthusiast-guide/) responde a las principales preguntas que la gente tiene sobre Windows Mixed Reality. 

En la guía encontrarás: 
- Preguntas frecuentes que se realizan antes de la compra, 
- Cómo comprobar la compatibilidad de tu equipo, 
- Instrucciones de configuración, 
- Cómo usar el casco y los controladores, 
- Cómo descargar y jugar a juegos envolventes, vídeos en 360º, aplicaciones 2D, WebVR y SteamVR, 
- Cómo solucionar problemas y mucho más.

![Controladores de movimiento y usuario de casco de Windows Mixed Reality](images/BeforeYouBegin-tile.jpg)

### <a name="keyboard-interactions"></a>Interacciones de teclado

Diseña y optimiza tus aplicaciones para UWP para ofrecer una experiencia y características accesibles para usuarios avanzados con [interacciones de teclado](../design/input/keyboard-interactions.md) actualizadas. Hemos actualizado nuestras recomendaciones e instrucciones para reflejar las nuevas mejoras de estas interacciones que se agregaron en la actualización Fall Creators Update.

Consulta [Aceleradores de teclado](../design/input/keyboard-accelerators.md) y [Personalizar interacciones de teclado](../design/input/custom-keyboard-interactions.md) para ampliar la funcionalidad de teclado de tus aplicaciones.

En dispositivos que admiten interacciones táctiles, agrega la funcionalidad de teclado consultando los artículos [Responder a la presencia del teclado táctil](../design/input/respond-to-the-presence-of-the-touch-keyboard.md) y [Usar el ámbito de entrada para cambiar el teclado táctil](../design/input/use-input-scope-to-change-the-touch-keyboard.md).

### <a name="microsoft-collaborate"></a>Microsoft Collaborate

El portal Microsoft Collaborate ofrece herramientas y servicios para optimizar la colaboración de ingeniería en el ecosistema de Microsoft al habilitar el uso compartido de elementos de trabajo de sistemas de ingeniería (errores, solicitudes de funciones, etc.) y la distribución de contenido (compilación, documentos, especificaciones). [Más información](https://docs.microsoft.com/en-us/collaborate).

![Microsoft Collaborate en el centro de partners](images/microsoft_collaborate_screenshot.PNG)

### <a name="package-desktop-applications-with-uwp-projects"></a>Empaquetar aplicaciones de escritorio con proyectos UWP

Visual Studio 2017 versión 15.5 ha actualizado la plantilla **Proyecto de paquete de aplicación de Windows** de modo que es mucho más fácil incluir un proyecto de UWP. Ya no es necesario usar un proyecto de empaquetado basado en JavaScript y después ajustar manualmente el manifiesto de paquete.  

Consulta [Empaquetar una aplicación mediante Visual Studio](https://docs.microsoft.com/en-us/windows/uwp/porting/desktop-to-uwp-packaging-dot-net) para obtener instrucciones sobre cómo usar esta nueva plantilla para empaquetar la aplicación de escritorio.

Consulta [Ampliar tu aplicación de escritorio con componentes de UWP modernos](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-extend) para obtener instrucciones sobre cómo agregar un proyecto de UWP a tu paquete.

### <a name="subscription-add-ons-are-now-available-to-developers-in-the-windows-dev-center-insider-program"></a>Los complementos de suscripción ahora están disponibles para los desarrolladores en el Programa Insider del Centro de desarrollo de Windows

Todos los desarrolladores que han participado en el [Programa Insider del Centro de desarrollo](../publish/dev-center-insider-program.md) ahora pueden usar complementos de suscripción para vender productos digitales en sus aplicaciones (por ejemplo, características de la aplicación o contenido digital) con períodos de facturación periódicos. Para obtener más información, consulta [Habilitar complementos de una suscripción para tu aplicación](../monetize/enable-subscription-add-ons-for-your-app.md).

## <a name="developer-guidance"></a>Guía para desarrolladores

### <a name="color"></a>Color

Hemos agregado algunas nuevas instrucciones sobre cómo usar el color en tus aplicaciones para la mejor experiencia del usuario posible. Esto incluye los escenarios de uso de API, así como instrucciones generales sobre la accesibilidad y el diseño de IU. También hemos actualizado la lista de colores de énfasis del usuario disponibles en Xbox. [Consulta el artículo de Color actualizado aquí.](../design/style/color.md)

![paleta de colores de windows universales](../design/basics/images/colors.png)

### <a name="data-access-guides"></a>Guías de acceso a datos

Hemos agregado una [Guía de SQL Server](../data-access/sql-server-databases.md) para mostrar cómo la aplicación puede acceder directamente a una base de datos de SQL Server. Sin capa de servicio necesaria.

Además, hemos renovado completamente nuestra [Guía de SQLite](../data-access/sqlite-databases.md) con un aspecto más fresco y hemos incluido nuestros últimos procedimientos recomendados para almacenar y recuperar datos en una base de datos ligera en el dispositivos del usuario.

### <a name="forms"></a>Formularios

Hemos agregado un nuevo artículo en [cómo construir formularios en tus aplicaciones](../design/controls-and-patterns/forms.md) para recopilar y enviar datos de los usuarios. Esto incluye información específica sobre la implementación de formularios e instrucciones generales sobre cuándo y dónde usarlos.

### <a name="intro-to-app-design"></a>Introducción al diseño de aplicaciones

Las instrucciones sobre diseño de la Plataforma universal de Windows (UWP) son un recurso que te ayudan a diseñar y crear fantásticas aplicaciones. [Nuestra nueva introducción](../design/basics/design-and-ui-intro.md) proporciona una descripción general de las características de diseño universal que se incluyen en cada aplicación para UWP y cómo puedes usar los documentos para crear interfaces de usuario (IU) que se distribuyen con elegancia en una amplia gama de dispositivos.


### <a name="request-ratings-and-reviews"></a>Solicitar valoraciones y opiniones

Hemos agregado un nuevo artículo en el que se muestra cómo se puede [solicitar valoraciones y opiniones de tu aplicación](../monetize/request-ratings-and-reviews.md). Puedes mostrar una clasificación y revisar el cuadro de diálogo en el contexto de la aplicación, o puedes abrir la clasificación y revisar la página de la aplicación en Store.

## <a name="samples"></a>Muestras

### <a name="customer-orders"></a>Pedidos de clientes

La muestra [Base de datos de pedidos se clientes](https://github.com/Microsoft/Windows-appsample-customers-orders-database) se ha actualizado para mostrar los procedimientos recomendados para el acceso de datos, como el uso de un patrón de repositorio y cómo conectar varios orígenes de datos (incluidos Sqlite, SQL Azure y un servicio REST).

## <a name="videos"></a>Vídeos

### <a name="package-a-net-app-in-visual-studio"></a>Empaquetar una aplicación .NET en Visual Studio

Es más fácil que nunca llevar tu aplicación de escritorio a la Plataforma universal de Windows. [Ve el vídeo](https://www.youtube.com/watch?v=fJkbYPyd08w) para descubrir cómo empaquetar tu aplicación .NET para su distribución y luego [echa un vistazo a esta página](../porting/desktop-to-uwp-packaging-dot-net.md) para obtener más información.