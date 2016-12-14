---
author: mijacobs
Description: "Notifications Visualizer es una nueva aplicación para la Plataforma universal de Windows (UWP) en la Tienda que ayuda a los desarrolladores a diseñar iconos dinámicos adaptables para Windows 10."
title: Notifications Visualizer
ms.assetid: FCBB7BB1-2C79-484B-8FFC-26FE1934EC1C
label: TBD
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: d51aacb31f41cbd9c065b013ffb95b83a6edaaf4
ms.openlocfilehash: 20e73c2de82bcc76c94fff38163dddac7ef9d8a9

---
# <a name="notifications-visualizer"></a>Notifications Visualizer

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 


Notifications Visualizer es una nueva aplicación para la Plataforma universal de Windows (UWP) en [la Tienda](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1) que ayuda a los desarrolladores a diseñar iconos dinámicos adaptables para Windows 10.

## <a name="overview"></a>Información general


La aplicación Notifications Visualizer proporciona vistas previas instantáneas de tu icono a medida que lo editas, de forma similar a la vista de editor/diseño de XAML de Visual Studio. La aplicación también comprueba si hay errores, lo cual garantiza que crees una carga de icono válida.

Esta captura de pantalla de la aplicación muestra la carga de XML y cómo aparecen los tamaños de icono en un dispositivo determinado:

![captura de pantalla del editor de la aplicación Notifications Visualizer con código e iconos](images/notif-visualizer-001.png)

 

Con Notifications Visualizer, puedes crear y probar cargas de iconos adaptables sin tener que editar e implementar la aplicación en sí. Una vez creada una carga con resultados visuales ideales puedes integrarla en tu aplicación. Consulta [Enviar una notificación de icono local](tiles-and-notifications-sending-a-local-tile-notification.md) para obtener más información.

**Nota** La simulación de Notifications Visualizer del menú Inicio de Windows no siempre es totalmente precisa y no es compatible con algunas propiedades de carga como [baseUri](https://msdn.microsoft.com/library/windows/apps/br208712). Cuando tengas el diseño de icono que desees, pruébalo anclando el icono al menú Inicio real para comprobar que aparece como deseas.

 

## <a name="features"></a>Funciones


Notifications Visualizer viene con varias cargas de muestra para mostrar lo que es posible con los iconos dinámicos adaptables y para ayudarte a empezar. Puedes experimentar con todas las opciones de texto, grupos/subgrupos, imágenes de fondo diferentes y ver cómo el icono se adapta a distintas pantallas y dispositivos. Una vez que hayas realizado cambios, puedes guardar la carga actualizada en un archivo para el uso futuro.

El editor proporciona advertencias y errores en tiempo real. Por ejemplo, si la carga de la aplicación se limita a menos de 5 KB (una limitación de plataforma), Notifications Visualizer te avisa si la carga supera ese límite. Te proporciona advertencias para los nombres de atributos o valores incorrectos, lo cual te ayuda a depurar problemas visuales.

Puedes controlar las propiedades de los iconos como el nombre para mostrar, el color, los logotipos, ShowName, el valor del distintivo. Estas opciones te ayudan a comprender al instante cómo interactúan tus cargas de notificación de iconos y propiedades de los iconos, así como los resultados que producen.

Esta captura de pantalla de la aplicación muestra el editor de iconos:

![captura de pantalla del editor de Notifications Visualizer con iconos](images/notif-visualizer-004.png)

 

## <a name="related-topics"></a>Temas relacionados


* [Obtener Notifications Visualizer en la Tienda](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1)
* [Crear iconos adaptables](tiles-and-notifications-create-adaptive-tiles.md)
* [Plantillas de iconos adaptables: esquema y documentación](tiles-and-notifications-adaptive-tiles-schema.md)
* [Iconos y notificaciones del sistema (blog de MSDN)](http://blogs.msdn.com/b/tiles_and_toasts/)


<!--HONumber=Dec16_HO1-->


