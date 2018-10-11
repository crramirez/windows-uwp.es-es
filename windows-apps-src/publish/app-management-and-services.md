---
author: jnHs
Description: Manage and view details related to each of your apps in the Windows Dev Center dashboard, and configure services such as A/B testing and maps.
title: Administración y servicios de aplicaciones
ms.assetid: 99DA2BC1-9B5D-4746-8BC0-EC723D516EEF
ms.author: wdg-dev-content
ms.date: 07/18/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: d0e4be450aa972ad8561f27a8d4749050458520a
ms.sourcegitcommit: 933e71a31989f8063b020746fdd16e9da94a44c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2018
ms.locfileid: "4532583"
---
# <a name="app-management-and-services"></a>Administración y servicios de aplicaciones

Puedes administrar y visualizar los detalles relacionados con cada una de tus aplicaciones en el panel de información del Centro de desarrollo de Windows y configurar servicios, tales como notificaciones, pruebas A/B y mapas.

Al trabajar con una aplicación en el panel, verás secciones en el menú de navegación izquierdo para administrar **servicios** y **aplicaciones**. Puedes expandir estas secciones para tener acceso a la funcionalidad que se describe a continuación.

## <a name="services"></a>Servicios

La sección **Servicios** te permite administrar varios servicios diferentes para tus aplicaciones.

## <a name="xbox-live"></a>XboxLive

Si va a publicar un juego, puedes habilitar el [Programa de creadores de Xbox Live](http://xbox.com/developers/creators-program) en esta página. Esto te permite empezar a configurar y probar características de Xbox Live y finalmente publicar tu juego de programa de creadores de Xbox Live.

Para obtener más información, consulta [empezar a trabajar con el programa de creadores de Xbox Live](../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md) y [crear un nuevo título de Xbox Live Creators Program y publicar en el entorno de prueba](../xbox-live/get-started-with-creators/create-and-test-a-new-creators-title.md).

## <a name="experimentation"></a>Experimentación

Usa la página **Experimentación** para crear y ejecutar experimentos para las aplicaciones para la Plataforma universal de Windows (UWP) con pruebas A/B. En una prueba A/B, se mide la eficacia en tu aplicación de los cambios de características (o variaciones) en algunos clientes antes de habilitar los cambios para todos.

Para obtener más información, consulta [Ejecutar experimentos en la aplicación con pruebas A/B](../monetize/run-app-experiments-with-a-b-testing.md).

## <a name="maps"></a>Mapas

Para usar servicios de mapa en aplicaciones para Windows Phone 8.1 y versiones anteriores, necesitas un id. de aplicación del servicio de mapas y un token para incluirlo en el código de la aplicación. Puedes obtener este token en la página **Mapas** de la sección **Servicios**.

> [!NOTE]
> Para usar los servicios de mapa en aplicaciones destinadas a Windows10 o Windows8.x, visita el [Centro de desarrollo de Mapas de Bing](http://go.microsoft.com/fwlink/p/?LinkId=614880). Consulta [Solicitar una clave de autenticación de mapas](https://docs.microsoft.com/windows/uwp/maps-and-location/authentication-key) para obtener más información.

Para más información, consulta [Usar servicios de mapa](use-map-services.md).

## <a name="product-collections-and-purchases"></a>Colecciones y compras de producto

Para usar Microsoft Store API de colecciones y la API de compras de Microsoft Store para obtener acceso a la información de propiedad para aplicaciones y complementos, debes hacer el cliente de Azure AD identificadores aquí. Ten en cuenta que la aplicación de los cambios puede tardar hasta 16 horas.

Para obtener más información, consulta [Administrar los derechos de producto de un servicio](../monetize/view-and-grant-products-from-a-service.md).

## <a name="administrator-consent"></a>Consentimiento del administrador

f tu producto se integra con Azure AD y llama a las API que solicitan [permisos de la aplicación o los permisos delegados](https://developer.microsoft.com/graph/docs/concepts/permissions_reference) que requieren el consentimiento del administrador, escribe tu Id. de cliente de Azure AD. Esto permite a los administradores que adquieran la aplicación para su consentimiento de concesión de organización para tu producto actuar en nombre de todos los usuarios del inquilino.

Para obtener más información, consulta la [solicitud de consentimiento para un inquilino de todo](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-v2-scopes#requesting-consent-for-an-entire-tenant).

## <a name="app-management"></a>Administración de aplicaciones

La sección **Administración de aplicaciones** te permite ver los detalles de identidad y del paquete, además de administrar los nombres de la aplicación.

## <a name="app-identity"></a>Identidad de la aplicación

Esta página muestra detalles relacionados con la identidad única de tu aplicación dentro de la Tienda, incluidas las direcciones URL para vincular a la descripción de la aplicación.

Para más información, consulta [Ver detalles de identidad de la aplicación](view-app-identity-details.md).

## <a name="manage-app-names"></a>Administrar nombres de aplicación

Aquí es donde puedes ver todos los nombres que has reservado para tu aplicación. Puedes reservar más nombres aquí o eliminar los que ya no usas.

Para más información, consulta [Administrar nombres de aplicación](manage-app-names.md).

## <a name="current-packages"></a>Paquetes actuales

Esta página permite ver los detalles relacionados con todos los paquetes publicados.

> [!NOTE]
> No verás información aquí hasta que no se haya publicado la aplicación.

Se muestra el nombre, la versión y la arquitectura de cada paquete. Haz clic en **Detalles** para mostrar información adicional como el idioma admitido, las capacidades de la aplicación y los tamaños de archivo. La información que ves para cada paquete puede variar en función de su sistema operativo de destino y otros factores. 

Los desarrolladores con permisos de OEM también pueden [generar paquetes de preinstalación](generate-preinstall-packages-for-oems.md) desde la página **Paquetes actuales**.

## <a name="wnsmpns"></a>WNS/MPNS

La sección **WNS/MPNS** proporciona opciones que te ayudarán a crear y enviar notificaciones a clientes de la aplicación. 

> [!TIP]
> Las aplicaciones para UWP, se recomienda con la opción de **notificaciones** en el panel. Esta característica te permite enviar notificaciones a todos los clientes de la aplicación o a un subconjunto destino de los clientes de Windows 10 que cumplan los criterios que hayas definido en un [segmento de clientes](create-customer-segments.md). Para obtener más información, consulta [Enviar notificaciones a los clientes de la aplicación](send-push-notifications-to-your-apps-customers.md).

Según el tipo de paquete de la aplicación y sus requisitos específicos, también puedes usar una de las siguientes opciones: 

-   **Servicios de notificaciones de inserción de Windows (WNS)** te permite enviar notificaciones del sistema, iconos, distintivos y actualizaciones sin procesar desde tu propio servicio en la nube. Para obtener más información, consulta [Introducción a los Servicios de notificaciones de inserción de Windows (WNS)](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md).

-   **Aplicaciones móviles de Microsoft Azure** te permite enviar notificaciones push, autenticar y administrar los usuarios de la aplicación y almacenar datos de la aplicación en la nube. Para obtener más información, consulta la [documentación de Aplicaciones móviles](http://go.microsoft.com/fwlink/p/?LinkId=221116).

-   **Servicio de notificaciones push de Microsoft (MPNS)** puede usarse con los paquetes .xap para Windows Phone. Puedes enviar un número limitado de notificaciones no autenticadas sin realizar ninguna configuración, aunque recomendamos usar notificaciones autenticadas para evitar limitaciones. Si estás usando MPNS, tendrás que cargar un certificado en el campo proporcionado en la página **WNS/MPNS** . Para obtener más información, consulta [Configurar un servicio web autenticado para enviar notificaciones de inserción para Windows Phone 8](http://go.microsoft.com/fwlink/p/?LinkId=528736).
 

 
