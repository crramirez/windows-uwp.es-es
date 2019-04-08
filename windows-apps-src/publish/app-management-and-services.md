---
Description: Administrar y ver los detalles relacionados con cada una de las aplicaciones en el centro de partners y configurar servicios como A / B, las pruebas y se asigna.
title: Administración y servicios de aplicaciones
ms.assetid: 99DA2BC1-9B5D-4746-8BC0-EC723D516EEF
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d6261a7cce86c82b4865d7ca1d68c082cba9ccca
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57610010"
---
# <a name="app-management-and-services"></a>Administración y servicios de aplicaciones

Puede administrar y ver los detalles relacionados con cada una de las aplicaciones en [centro de partners](https://partner.microsoft.com/dashboard/)y configurar servicios como las notificaciones, A / B las pruebas y se asigna.

Cuando se trabaja con una aplicación en el centro de partners, podrá ver secciones en el menú de navegación izquierdo para **servicios** y **administración de aplicaciones**. Puedes expandir estas secciones para tener acceso a la funcionalidad que se describe a continuación.

## <a name="services"></a>Servicios

La sección **Servicios** te permite administrar varios servicios diferentes para tus aplicaciones.

## <a name="xbox-live"></a>Xbox Live

Si está publicando un juego, puede habilitar la [programa de creadores de Xbox Live](https://xbox.com/developers/creators-program) en esta página. Esto le permite empezar a configurar y probar características de Xbox Live y, finalmente, publique su juego de programa de creadores de Xbox Live.

Para obtener más información, consulte [empezar a trabajar con el programa de creadores de Xbox Live](../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md) y [crear un nuevo título de programa de creadores de Xbox Live y publicar en el entorno de prueba](../xbox-live/get-started-with-creators/create-and-test-a-new-creators-title.md).

## <a name="experimentation"></a>Experimentación

Usa la página **Experimentación** para crear y ejecutar experimentos para las aplicaciones para la Plataforma universal de Windows (UWP) con pruebas A/B. En una prueba A/B, se mide la eficacia en tu aplicación de los cambios de características (o variaciones) en algunos clientes antes de habilitar los cambios para todos.

Para obtener más información, consulta [Ejecutar experimentos en la aplicación con pruebas A/B](../monetize/run-app-experiments-with-a-b-testing.md).

## <a name="maps"></a>Maps

Para usar los servicios de mapa en aplicaciones destinadas a Windows 10 o Windows 8.x, visita el [Centro de desarrollo de Mapas de Bing](https://go.microsoft.com/fwlink/p/?LinkId=614880). Para obtener información sobre cómo solicitar una clave de autenticación de asignaciones desde el Centro para desarrolladores de Bing Maps y agregarlo a la aplicación, consulte [solicitar una clave de autenticación de asignaciones](../maps-and-location/authentication-key.md) para obtener más información. 

Use la **mapas** página únicamente para las aplicaciones publicadas anteriormente para Windows Phone 8.1 y versiones anteriores. Para usar servicios de mapa en estas aplicaciones, deberá solicitar un identificador de aplicación de servicio de mapa y un token para incluir en el código de la aplicación. Al hacer clic en **obtener token**, vamos a generar un Id. de aplicación de servicio de mapa (**ApplicationID**) y asignar el servicio de Token de autenticación (**AuthenticationToken**) para la aplicación. No olvide agregar estos valores en el código antes de los paquetes y enviar la aplicación. Para obtener más información, consulta [Cómo agregar un control de mapa a una página (Windows Phone 8.1)](https://go.microsoft.com/fwlink/p/?LinkId=614882).

## <a name="product-collections-and-purchases"></a>Colecciones y compras de producto

Para usar la Microsoft Store API de recopilación y la API de compra de Microsoft Store para tener acceso a la información de propiedad para las aplicaciones y complementos, deberá indicar asociado AD Azure Id. de cliente aquí. Ten en cuenta que la aplicación de los cambios puede tardar hasta 16 horas.

Para obtener más información, consulta [Administrar los derechos de producto de un servicio](../monetize/view-and-grant-products-from-a-service.md).

## <a name="administrator-consent"></a>Consentimiento del administrador

Si el producto se integra con Azure AD y llama a las API que solicitan cualquiera [permisos de la aplicación o permisos delegados](https://developer.microsoft.com/graph/docs/concepts/permissions_reference) que requieren el consentimiento del administrador, escriba su Id. de cliente de Azure AD. Esto permite a los administradores que adquieren la aplicación para su consentimiento de concesión de organización para su producto actuar en nombre de todos los usuarios en el inquilino.

Para obtener más información, consulte [solicitud de consentimiento para un inquilino al completo](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-v2-scopes#requesting-consent-for-an-entire-tenant).

## <a name="app-management"></a>Administración de aplicaciones

La sección **Administración de aplicaciones** te permite ver los detalles de identidad y del paquete, además de administrar los nombres de la aplicación.

## <a name="app-identity"></a>Identidad de la aplicación

Esta página muestra detalles relacionados con la identidad única de tu aplicación dentro de la Tienda, incluidas las direcciones URL para vincular a la descripción de la aplicación.

Para más información, consulta [Ver detalles de identidad de la aplicación](view-app-identity-details.md).

## <a name="manage-app-names"></a>Administrar nombres de aplicación

Aquí es donde puedes ver todos los nombres que has reservado para tu aplicación. Puedes reservar más nombres aquí o eliminar los que ya no usas.

Para obtener más información, consulta [Administrar nombres de aplicación](manage-app-names.md).

## <a name="current-packages"></a>Paquetes actuales

Esta página permite ver los detalles relacionados con todos los paquetes publicados.

> [!NOTE]
> No verás información aquí hasta que no se haya publicado la aplicación.

Se muestra el nombre, la versión y la arquitectura de cada paquete. Haz clic en **Detalles** para mostrar información adicional como el idioma admitido, las capacidades de la aplicación y los tamaños de archivo. La información que ves para cada paquete puede variar en función de su sistema operativo de destino y otros factores. 

Los desarrolladores con permisos de OEM también pueden [generar paquetes de preinstalación](generate-preinstall-packages-for-oems.md) desde la página **Paquetes actuales**.

## <a name="wnsmpns"></a>WNS/MPNS

El **WNS/MPNS** sección ofrece opciones para ayudarle a crear y enviar notificaciones a los clientes de la aplicación. 

> [!TIP]
> Para aplicaciones UWP, se recomienda usar la **notificaciones** característica Centro de partners. Esta característica le permite enviar notificaciones a todos los clientes de la aplicación, o a un subconjunto de destino de los clientes de Windows 10 que cumplan los criterios que haya definido en un [segmento de clientes](create-customer-segments.md). Para obtener más información, consulta [Enviar notificaciones a los clientes de la aplicación](send-push-notifications-to-your-apps-customers.md).

Según el tipo de paquete de aplicación y sus requisitos específicos, también puede usar una de las opciones siguientes: 

-   **Servicios de notificaciones de inserción de Windows (WNS)** te permite enviar notificaciones del sistema, iconos, distintivos y actualizaciones sin procesar desde tu propio servicio en la nube. Para más información, consulta [Introducción a los Servicios de notificaciones de inserción de Windows (WNS)](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md).

-   **Aplicaciones móviles de Microsoft Azure** te permite enviar notificaciones push, autenticar y administrar los usuarios de la aplicación y almacenar datos de la aplicación en la nube. Para obtener más información, consulta la [documentación de Aplicaciones móviles](https://go.microsoft.com/fwlink/p/?LinkId=221116).

-   **Servicio de notificaciones Push de Microsoft (MPNS)** se puede usar con los paquetes publicados anteriormente .xap para Windows Phone. Puedes enviar un número limitado de notificaciones no autenticadas sin realizar ninguna configuración, aunque recomendamos usar notificaciones autenticadas para evitar limitaciones. Si usa MPNS, deberá cargar un certificado en el campo proporcionado en el **WNS/MPNS** página. Para obtener más información, consulta [Configurar un servicio web autenticado para enviar notificaciones de inserción para Windows Phone 8](https://go.microsoft.com/fwlink/p/?LinkId=528736).
 

 
