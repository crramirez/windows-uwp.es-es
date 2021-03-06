---
title: Introducción a ubicación y mapas
description: En esta sección se explica cómo mostrar mapas, usar los servicios de mapa, buscar la ubicación y configurar una geovalla en la aplicación. En esta sección también se muestra cómo iniciar la aplicación Mapas de Windows con un mapa, una ruta o un conjunto de indicaciones paso a paso específicos.
ms.assetid: F4C1F094-CF46-4B15-9D80-C1A26A314521
ms.date: 10/20/2020
ms.topic: article
keywords: windows 10, uwp, map, location, map services
ms.localizationpriority: medium
ms.openlocfilehash: 61b36aa8299d98544c44039abb138f4422e0a164
ms.sourcegitcommit: 7aaf0740a5d3a17ebf9214aa5e5d056924317673
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2020
ms.locfileid: "92297663"
---
# <a name="maps-and-location-overview"></a>Introducción a ubicación y mapas

En esta sección se explica cómo mostrar mapas, usar los servicios de mapa, buscar la ubicación y configurar una geovalla en la aplicación. En esta sección también se muestra cómo iniciar la aplicación Mapas de Windows con un mapa, una ruta o un conjunto de indicaciones paso a paso específicos.

[**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) y servicios de mapas requieren una clave de autenticación de mapas denominada [**MapServiceToken**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.mapservicetoken). Para obtener más información sobre cómo obtener y establecer una clave de autenticación de mapas, consulta [Solicitar una clave de autenticación de mapas](authentication-key.md).

> [!TIP]
> Para más información sobre el uso de mapas y la ubicación en tu aplicación, descarga los ejemplos siguientes del [repositorio Windows-universal-samples](https://github.com/Microsoft/Windows-universal-samples) en GitHub:
-   [Ejemplo de mapa de la Plataforma universal de Windows (UWP)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)
-   [Ejemplo de ubicación geográfica de UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Geolocation)

 

## <a name="display-maps"></a>Mostrar mapas


Muestra mapas con las vistas 2D, 3D o Streetside en tu aplicación mediante las API del espacio de nombres [**Windows.UI.Xaml.Controls.Maps**](/uwp/api/Windows.UI.Xaml.Controls.Maps). Puedes marcar puntos de interés (POI) en el mapa con marcadores, imágenes, formas o elementos de interfaz de usuario XAML. También puedes superponer imágenes en mosaico o reemplazar las imágenes de mapa de forma conjunta.

| Tema | Descripción |
|-------|-------------|
| [Solicitar una clave de autenticación de mapas](authentication-key.md) | La aplicación debe autenticarse para poder usar la clase [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) y los servicios de mapa en el espacio de nombres [**Windows.Services.Maps**](/uwp/api/Windows.Services.Maps). Para autenticar la aplicación, debes especificar una clave de autenticación de mapas. En este artículo se describe cómo solicitar una clave de autenticación de mapas desde el [Centro de desarrollo de Mapas de Bing](https://www.bingmapsportal.com/) y cómo agregarla a la aplicación. |
| [Mostrar mapas con vistas 2D, 3D y Streetside](display-maps.md) | Muestra mapas personalizables en tu aplicación mediante la clase [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl). En este tema también se presentan las vistas 3D aérea y Streetside. |
| [Mostrar puntos de interés (POI) en un mapa](display-poi.md) | Agrega puntos de interés (POI) a un mapa mediante marcadores, imágenes, formas y elementos de interfaz de usuario XAML. |
| [Superponer imágenes en mosaico en un mapa](overlay-tiled-images.md) | Superpón imágenes en mosaico personalizadas o de terceros en un mapa mediante orígenes de mosaico. Usa orígenes de mosaico para superponer información especializada como, por ejemplo, datos meteorológicos, datos de población o datos sísmicos, o bien para reemplazar el mapa predeterminado por completo. |



## <a name="access-map-services"></a>Acceder a servicios de mapa

Agrega rutas, indicaciones y funcionalidades de geocodificación a tu aplicación mediante las API del espacio de nombres [**Windows.Services.Maps**](/uwp/api/Windows.Services.Maps).

| Tema | Descripción |
|-----------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Solicitar una clave de autenticación de mapas](authentication-key.md) | La aplicación debe autenticarse para poder usar la clase [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) y los servicios de mapa en el espacio de nombres [**Windows.Services.Maps**](/uwp/api/Windows.Services.Maps). Para autenticar la aplicación, debes especificar una clave de autenticación de mapas. En este artículo se describe cómo solicitar una clave de autenticación de mapas desde el [Centro de desarrollo de Mapas de Bing](https://www.bingmapsportal.com/) y cómo agregarla a la aplicación. |
| [Mostrar puntos de interés (POI) en un mapa](display-poi.md) | Agrega puntos de interés (POI) a un mapa mediante marcadores, imágenes, formas y elementos de interfaz de usuario XAML. |
| [Mostrar rutas e indicaciones](routes-and-directions.md) | Solicita rutas e indicaciones y muéstralas en tu aplicación. |
| [Realizar geocodificación y geocodificación inversa](geocoding.md) | Convierte direcciones en ubicaciones geográficas (geocodificación), y ubicaciones geográficas en direcciones (geocodificación inversa), mediante una llamada a los métodos de la clase [**MapLocationFinder**](/uwp/api/Windows.Services.Maps.MapLocationFinder) del espacio de nombres [**Windows.Services.Maps**](/uwp/api/Windows.Services.Maps). |
| [Buscar y descargar paquetes de mapas para usarlos sin conexión](/uwp/api/windows.services.maps.offlinemaps)| En el pasado, la aplicación debía dirigir a los usuarios a la aplicación Configuración para descargar mapas sin conexión. Ahora, puedes usar las clases del espacio de nombres [Windows.Services.Maps.OfflineMaps](/uwp/api/windows.services.maps.offlinemaps) para buscar los paquetes descargados en un área determinada (según una clase [Geopoint](/uwp/api/Windows.Devices.Geolocation.Geopoint), [GeoboundingBox](/uwp/api/windows.devices.geolocation.geoboundingbox), etc.). <br> También puedes comprobar y escuchar el estado descargado de los paquetes de mapas, así como iniciar una descarga sin necesidad de que el usuario abandone la aplicación. <br> Encontrarás ejemplos de cómo hacer esto en el contenido de referencia y en el [ejemplo de mapa de la Plataforma universal de Windows (UWP)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl).

## <a name="get-the-users-location"></a>Obtener la ubicación del usuario

Obtén la ubicación actual del usuario y recibe notificaciones cuando la ubicación cambie en tu aplicación mediante las API del espacio de nombres [**Windows.Devices.Geolocation**](/uwp/api/Windows.Devices.Geolocation). Estos miembros de las API también se usan con frecuencia en los parámetros de las API de mapas. Las API del espacio de nombres [**Windows.Devices.Geolocation.Geofencing**](/uwp/api/Windows.Devices.Geolocation.Geofencing) envían notificaciones a tu aplicación cuando el usuario accede a una geovalla o sale de esta (un área geográfica predefinida).

| Tema | Descripción |
|-------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Solicitar una clave de autenticación de mapas](authentication-key.md) | La aplicación debe autenticarse para poder usar la clase [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) y los servicios de mapa en el espacio de nombres [**Windows.Services.Maps**](/uwp/api/Windows.Services.Maps). Para autenticar la aplicación, debes especificar una clave de autenticación de mapas. En este artículo se describe cómo solicitar una clave de autenticación de mapas desde el [Centro de desarrollo de Mapas de Bing](https://www.bingmapsportal.com/) y cómo agregarla a la aplicación. |
| [Directrices de diseño para las aplicaciones con reconocimiento de ubicación](guidelines-and-checklist-for-detecting-location.md) | Directrices de rendimiento para las aplicaciones que necesitan acceder a la ubicación de un usuario. |
| [Obtener la ubicación del usuario](get-location.md) | Obtén acceso a la ubicación del usuario y luego recupérala. | 
| [Directrices para usar Visits tracking](guidelines-for-visits.md) | Aprende a usar la eficaz característica Visits Tracking para realizar cómodamente el seguimiento de la ubicación. |
| [Guía de diseño de geovallas](guidelines-for-geofencing.md) | Directrices de rendimiento para las aplicaciones que usan la característica de geovalla. |
| [Configurar una geovalla](set-up-a-geofence.md) | Configura una geovalla en tu aplicación, y aprende a administrar las notificaciones en primer y segundo plano. |

## <a name="launch-the-windows-maps-app"></a>Iniciar la aplicación Mapas de Windows

La aplicación puede iniciar la aplicación Mapas de Windows como aquí se indica para mostrar mapas específicos e indicaciones paso a paso. En lugar de proporcionar la funcionalidad de mapa directamente en tu propia aplicación, considera la posibilidad de usar la aplicación Mapas de Windows para proporcionar esa funcionalidad. Para obtener más información, consulta [Iniciar la aplicación Mapas de Windows](../launch-resume/launch-maps-app.md).

![Ejemplo de la aplicación Mapas de Windows.](images/mapnyc.png)

## <a name="related-topics"></a>Temas relacionados

* [Ejemplo de mapa de UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)
* [Ejemplo de ubicación geográfica de UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Geolocation)
* [Bing Maps Developer Center](https://www.bingmapsportal.com/)
* [Obtener la ubicación actual](get-location.md)
* [Directrices de diseño para las aplicaciones con reconocimiento de ubicación](guidelines-and-checklist-for-detecting-location.md)
* [Directrices de diseño para mapas](./display-maps.md)
* [Directrices de diseño para aplicaciones basadas en la privacidad](../security/index.md)
* [Vídeo de compilación de 2015: Leveraging Maps and Location Across Phone, Tablet, and PC in Your Windows Apps (Aprovechar los mapas y la ubicación entre teléfonos, tabletas y equipos en tus aplicaciones Windows)](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Ejemplo de aplicación de tráfico para UWP](https://github.com/Microsoft/Windows-appsample-trafficapp)
