---
author: msatranjr
title: "Introducción a ubicación y mapas"
description: "En esta sección se explica cómo mostrar mapas, usar los servicios de mapa, buscar la ubicación y configurar una geovalla en la aplicación. En esta sección también se muestra cómo iniciar la aplicación Mapas de Windows con un mapa, una ruta o un conjunto de indicaciones paso a paso específicos."
ms.assetid: F4C1F094-CF46-4B15-9D80-C1A26A314521
ms.sourcegitcommit: a3240047ec77ada0c5f6b5586eee2404353889f6
ms.openlocfilehash: 829a8d7eb4da810e2353593c03cd5aa4e047173a

---

# Introducción a ubicación y mapas


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


En esta sección se explica cómo mostrar mapas, usar los servicios de mapa, buscar la ubicación y configurar una geovalla en la aplicación. En esta sección también se muestra cómo iniciar la aplicación Mapas de Windows con un mapa, una ruta o un conjunto de indicaciones paso a paso específicos.

> 
            **Sugerencia**  Para obtener más información sobre el uso de los mapas y la ubicación en tu aplicación, descarga las muestras siguientes de [Windows-universal-samples repo](http://go.microsoft.com/fwlink/p/?LinkId=619979) (Repositorio de muestras universales de Windows) en GitHub:
-   [Muestra de mapa en la Plataforma universal de Windows (UWP)](http://go.microsoft.com/fwlink/p/?LinkId=619977)
-   [Muestra de ubicación geográfica deUWP](http://go.microsoft.com/fwlink/p/?linkid=533278)

 

## Mostrar mapas


Muestra mapas con las vistas 2D, 3D o Streetside en tu aplicación mediante las API del espacio de nombres [**Windows.UI.Xaml.Controls.Maps**](https://msdn.microsoft.com/library/windows/apps/dn610751). Puedes marcar puntos de interés en el mapa con marcadores, imágenes, formas o elementos de interfaz de usuario XAML. También puedes superponer imágenes en ventana o reemplazar las imágenes de mapa de forma conjunta.

| Tema | Descripción |
|-------|-------------|
| [Solicitar una clave de autenticación de mapas](authentication-key.md) | La aplicación debe autenticarse para poder usar [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) y los servicios de mapa en el espacio de nombres [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979). Para autenticar la aplicación, debes especificar una clave de autenticación de mapas. En este artículo se describe cómo solicitar una clave de autenticación de mapas desde el [Centro para desarrolladores de Mapas de Bing](https://www.bingmapsportal.com/) y agregarla a la aplicación. |
| [Control de mapa](controls-map.md) | En el control de mapa se pueden mostrar mapas y vistas aéreas, indicaciones, resultados de búsqueda y el estado del tráfico. |
| [Mostrar mapas con vistas 2D, 3D y Streetside](display-maps.md) | Muestra mapas personalizables en tu aplicación mediante la clase [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004). Este tema también presenta la vista 3D aérea y la vista de Streetside. |
| [Mostrar puntos de interés en un mapa](display-poi.md) | Agrega puntos de interés a un mapa mediante marcadores, imágenes, formas y elementos de interfaz de usuario XAML. |
| [Superponer imágenes en mosaico en un mapa](overlay-tiled-images.md) | Superpón imágenes en mosaico personalizadas o de terceros en un mapa mediante orígenes de icono. Usa orígenes de icono para superponer información especializada como, por ejemplo, datos meteorológicos, datos de población o datos sísmicos, o bien para reemplazar el mapa predeterminado por completo. |



## Acceder a servicios de mapa

Agrega rutas, indicaciones y funcionalidades de geocodificación a tu aplicación mediante las API del espacio de nombres [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979). También puedes ayudar al usuario a administrar mapas sin conexión iniciando la aplicación Configuración directamente en la página correspondiente.

| Tema | Descripción |
|-----------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Solicitar una clave de autenticación de mapas](authentication-key.md) | La aplicación debe autenticarse para poder usar [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) y los servicios de mapa en el espacio de nombres [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979). Para autenticar la aplicación, debes especificar una clave de autenticación de mapas. En este artículo se describe cómo solicitar una clave de autenticación de mapas desde el [Centro para desarrolladores de Mapas de Bing](https://www.bingmapsportal.com/) y agregarla a la aplicación. |
| [Mostrar puntos de interés en un mapa](display-poi.md) | Agrega puntos de interés a un mapa mediante marcadores, imágenes, formas y elementos de interfaz de usuario XAML. |
| [Mostrar rutas e indicaciones](routes-and-directions.md) | Solicita rutas e indicaciones y muéstralas en tu aplicación. |
| [Realizar geocodificación y geocodificación inversa](geocoding.md) | Puedes convertir direcciones en ubicaciones geográficas (geocodificación), y ubicaciones geográficas en direcciones (geocodificación inversa), si llamas a los métodos de la clase [**MapLocationFinder**](https://msdn.microsoft.com/library/windows/apps/dn627550) del espacio de nombres [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979). |


## Obtener la ubicación del usuario

Obtén la ubicación actual del usuario y recibe notificaciones cuando la ubicación cambie en tu aplicación mediante las API del espacio de nombres [**Windows.Devices.Geolocation**](https://msdn.microsoft.com/library/windows/apps/br225603). Estos miembros de las API también se usan con frecuencia en los parámetros de las API de mapas. Las API del espacio de nombres [**Windows.Devices.Geolocation.Geofencing**](https://msdn.microsoft.com/library/windows/apps/dn263744) envían notificaciones a tu aplicación cuando el usuario accede a una geovalla o sale de esta (un área geográfica predefinida).

| Tema | Descripción |
|-------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Solicitar una clave de autenticación de mapas](authentication-key.md) | La aplicación debe autenticarse para poder usar [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) y los servicios de mapa en el espacio de nombres [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979). Para autenticar la aplicación, debes especificar una clave de autenticación de mapas. En este artículo se describe cómo solicitar una clave de autenticación de mapas desde el [Centro para desarrolladores de Mapas de Bing](https://www.bingmapsportal.com/) y agregarla a la aplicación. |
| [Directrices para aplicaciones con reconocimiento de ubicación](guidelines-and-checklist-for-detecting-location.md) | Directrices de rendimiento para las aplicaciones que necesitan acceder a la ubicación del usuario. |
| [Obtener la ubicación del usuario](get-location.md) | Obtén acceso a la ubicación del usuario y luego recupérala. |
| [Guía de diseño de geovallas](guidelines-for-geofencing.md) | Directrices de rendimiento para las aplicaciones que usan la característica de geovalla. |
| [Configurar una geovalla](set-up-a-geofence.md) | Configura una geovalla en tu aplicación y aprende a administrar las notificaciones en primer y segundo plano. |

## Iniciar la aplicación Mapas de Windows

La aplicación puede iniciar la aplicación Mapas de Windows como aquí se indica para mostrar mapas específicos e indicaciones paso a paso. En lugar de proporcionar la funcionalidad de mapa directamente en tu propia aplicación, considera la posibilidad de usar la aplicación Mapas de Windows para proporcionar esa funcionalidad. Para obtener más información, consulta [Iniciar la aplicación Mapas de Windows](https://msdn.microsoft.com/library/windows/apps/mt228341).

![Ejemplo de la aplicación Mapas de Windows.](images/mapnyc.png)

## Temas relacionados

* [Muestra de mapa de UWP](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [Muestra de ubicación geográfica de UWP](http://go.microsoft.com/fwlink/p/?linkid=533278)
* [Centro para desarrolladores de Mapas de Bing](https://www.bingmapsportal.com/)
* [Obtener la ubicación actual](get-location.md)
* [Directrices para aplicaciones con reconocimiento de ubicación](guidelines-and-checklist-for-detecting-location.md)
* [Directrices de diseño para mapas](controls-map.md)
* [Directrices para aplicaciones compatibles con la privacidad](https://msdn.microsoft.com/library/windows/apps/hh768223)
* [Vídeo de compilación de 2015: Leveraging Maps and Location Across Phone, Tablet, and PC in Your Windows Apps (Aprovechar los mapas y la ubicación entre teléfonos, tabletas y equipos en tus aplicaciones Windows)](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Ejemplo de aplicación de tráfico de UWP](http://go.microsoft.com/fwlink/p/?LinkId=619982)






<!--HONumber=Jun16_HO5-->


