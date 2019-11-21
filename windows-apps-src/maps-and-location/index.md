---
title: Introducción a ubicación y mapas
description: En esta sección se explica cómo mostrar mapas, usar los servicios de mapa, buscar la ubicación y configurar una geovalla en la aplicación. En esta sección también se muestra cómo iniciar la aplicación Mapas de Windows con un mapa, una ruta o un conjunto de indicaciones paso a paso específicos.
ms.assetid: F4C1F094-CF46-4B15-9D80-C1A26A314521
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, map, location, map services
ms.localizationpriority: medium
ms.openlocfilehash: 6a27eeeb9aa7349e532dcd76e5b7a7176ac20c08
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259345"
---
# <a name="maps-and-location-overview"></a>Introducción a ubicación y mapas




En esta sección se explica cómo mostrar mapas, usar los servicios de mapa, buscar la ubicación y configurar una geovalla en la aplicación. En esta sección también se muestra cómo iniciar la aplicación Mapas de Windows con un mapa, una ruta o un conjunto de indicaciones paso a paso específicos.

> [!TIP]
> Para más información sobre el uso de mapas y la ubicación en tu aplicación, descarga los ejemplos siguientes del [repositorio Windows-universal-samples](https://github.com/Microsoft/Windows-universal-samples) en GitHub:
-   [Ejemplo de mapa de la Plataforma universal de Windows (UWP)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)
-   [Ejemplo de ubicación geográfica de UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Geolocation)

 

## <a name="display-maps"></a>Mostrar mapas


Muestra mapas con las vistas 2D, 3D o Streetside en tu aplicación mediante las API del espacio de nombres [**Windows.UI.Xaml.Controls.Maps**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps). Puedes marcar puntos de interés en el mapa con marcadores, imágenes, formas o elementos de interfaz de usuario XAML. También puedes superponer imágenes en ventana o reemplazar las imágenes de mapa de forma conjunta.

| Tema | Descripción |
|-------|-------------|
| [Solicitar una clave de autenticación de mapas](authentication-key.md) | La aplicación debe autenticarse para poder usar [**MapControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) y los servicios de mapa en el espacio de nombres [**Windows.Services.Maps**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps). Para autenticar la aplicación, debes especificar una clave de autenticación de mapas. En este artículo se describe cómo solicitar una clave de autenticación de mapas desde el [Centro para desarrolladores de Mapas de Bing](https://www.bingmapsportal.com/) y agregarla a la aplicación. |
| [Mostrar mapas con vistas 2D, 3D y Streetside](display-maps.md) | Muestra mapas personalizables en tu aplicación mediante la clase [**MapControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl). Este tema también presenta la vista 3D aérea y la vista de Streetside. |
| [Mostrar puntos de interés en un mapa](display-poi.md) | Agrega puntos de interés a un mapa mediante marcadores, imágenes, formas y elementos de interfaz de usuario XAML. |
| [Superponer imágenes en mosaico en un mapa](overlay-tiled-images.md) | Superpón imágenes en mosaico personalizadas o de terceros en un mapa mediante orígenes de icono. Usa orígenes de icono para superponer información especializada como, por ejemplo, datos meteorológicos, datos de población o datos sísmicos, o bien para reemplazar el mapa predeterminado por completo. |



## <a name="access-map-services"></a>Acceder a servicios de mapa

Agrega rutas, indicaciones y funcionalidades de geocodificación a tu aplicación mediante las API del espacio de nombres [**Windows.Services.Maps**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps).

| Tema | Descripción |
|-----------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Solicitar una clave de autenticación de mapas](authentication-key.md) | La aplicación debe autenticarse para poder usar [**MapControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) y los servicios de mapa en el espacio de nombres [**Windows.Services.Maps**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps). Para autenticar la aplicación, debes especificar una clave de autenticación de mapas. En este artículo se describe cómo solicitar una clave de autenticación de mapas desde el [Centro para desarrolladores de Mapas de Bing](https://www.bingmapsportal.com/) y agregarla a la aplicación. |
| [Mostrar puntos de interés en un mapa](display-poi.md) | Agrega puntos de interés a un mapa mediante marcadores, imágenes, formas y elementos de interfaz de usuario XAML. |
| [Mostrar rutas e indicaciones](routes-and-directions.md) | Solicita rutas e indicaciones y muéstralas en tu aplicación. |
| [Realizar geocodificación y geocodificación inversa](geocoding.md) | Puedes convertir direcciones en ubicaciones geográficas (geocodificación), y ubicaciones geográficas en direcciones (geocodificación inversa), si llamas a los métodos de la clase [**MapLocationFinder**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps.MapLocationFinder) del espacio de nombres [**Windows.Services.Maps**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps). |
| [Buscar y descargar paquetes de mapas para usarlos sin conexión](https://docs.microsoft.com/uwp/api/windows.services.maps.offlinemaps)| En el pasado, la aplicación debía dirigir a los usuarios a la aplicación Configuración para descargar mapas sin conexión. Ahora, puedes usar las clases del espacio de nombres [Windows.Services.Maps.OfflineMaps](https://docs.microsoft.com/en-us/uwp/api/windows.services.maps.offlinemaps) para buscar los paquetes descargados en un área determinada (según una clase [Geopoint](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geopoint), [GeoboundingBox](https://docs.microsoft.com/en-us/uwp/api/windows.devices.geolocation.geoboundingbox), etc.). <br> También puedes comprobar y escuchar el estado descargado de los paquetes de mapas, así como iniciar una descarga sin necesidad de que el usuario abandone la aplicación. <br> Encontrarás ejemplos de cómo hacer esto en el contenido de referencia y en el [ejemplo de mapa de la Plataforma universal de Windows (UWP)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl).

## <a name="get-the-users-location"></a>Obtener la ubicación del usuario

Obtén la ubicación actual del usuario y recibe notificaciones cuando la ubicación cambie en tu aplicación mediante las API del espacio de nombres [**Windows.Devices.Geolocation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation). Estos miembros de las API también se usan con frecuencia en los parámetros de las API de mapas. Las API del espacio de nombres [**Windows.Devices.Geolocation.Geofencing**](https://docs.microsoft.com/uwp/api/Windows.Devices.Geolocation.Geofencing) envían notificaciones a tu aplicación cuando el usuario accede a una geovalla o sale de esta (un área geográfica predefinida).

| Tema | Descripción |
|-------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Solicitar una clave de autenticación de mapas](authentication-key.md) | La aplicación debe autenticarse para poder usar [**MapControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) y los servicios de mapa en el espacio de nombres [**Windows.Services.Maps**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps). Para autenticar la aplicación, debes especificar una clave de autenticación de mapas. En este artículo se describe cómo solicitar una clave de autenticación de mapas desde el [Centro para desarrolladores de Mapas de Bing](https://www.bingmapsportal.com/) y agregarla a la aplicación. |
| [Directrices de diseño para las aplicaciones con reconocimiento de ubicación](guidelines-and-checklist-for-detecting-location.md) | Directrices de rendimiento para las aplicaciones que necesitan acceder a la ubicación del usuario. |
| [Obtener la ubicación del usuario](get-location.md) | Obtén acceso a la ubicación del usuario y luego recupérala. | 
| [Directrices para usar Visits tracking](guidelines-for-visits.md) | Aprende a usar la eficaz característica Visits Tracking para realizar cómodamente el seguimiento de la ubicación. |
| [Guía de diseño de geovallas](guidelines-for-geofencing.md) | Directrices de rendimiento para las aplicaciones que usan la característica de geovalla. |
| [Configurar una geovalla](set-up-a-geofence.md) | Configura una geovalla en tu aplicación y aprende a administrar las notificaciones en primer y segundo plano. |

## <a name="launch-the-windows-maps-app"></a>Iniciar la aplicación Mapas de Windows

La aplicación puede iniciar la aplicación Mapas de Windows como aquí se indica para mostrar mapas específicos e indicaciones paso a paso. En lugar de proporcionar la funcionalidad de mapa directamente en tu propia aplicación, considera la posibilidad de usar la aplicación Mapas de Windows para proporcionar esa funcionalidad. Para obtener más información, consulta [Iniciar la aplicación Mapas de Windows](https://docs.microsoft.com/windows/uwp/launch-resume/launch-maps-app).

![Ejemplo de la aplicación Mapas de Windows.](images/mapnyc.png)

## <a name="related-topics"></a>Temas relacionados

* [Ejemplo de mapa de UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)
* [Ejemplo de ubicación geográfica de UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Geolocation)
* [Bing Maps Developer Center](https://www.bingmapsportal.com/)
* [Obtener la ubicación actual](get-location.md)
* [Directrices de diseño para las aplicaciones con reconocimiento de ubicación](guidelines-and-checklist-for-detecting-location.md)
* [Directrices de diseño para mapas](controls-map.md)
* [Directrices de diseño para aplicaciones basadas en la privacidad](https://docs.microsoft.com/windows/uwp/security/index)
* Vídeo de [Build 2015: Leveraging Maps and Location Across Phone, Tablet, and PC in Your Windows Apps](https://channel9.msdn.com/Events/Build/2015/2-757) (Aprovechamiento de mapas y ubicación entre teléfonos, tabletas y equipos en las aplicaciones de Windows)
* [Ejemplo de aplicación de tráfico para UWP](https://github.com/Microsoft/Windows-appsample-trafficapp)
