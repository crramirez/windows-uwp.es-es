---
Description: Administre y vea los detalles relacionados con cada una de las aplicaciones en el centro de Partners y configure servicios como pruebas y asignaciones A/B.
title: Administración y servicios de aplicaciones
ms.assetid: 99DA2BC1-9B5D-4746-8BC0-EC723D516EEF
ms.date: 03/21/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 30610cdacbd9d2be10205958688376371f0387f6
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260037"
---
# <a name="app-management-and-services"></a>Administración y servicios de aplicaciones

Puede administrar y ver los detalles relacionados con cada una de las aplicaciones en el [centro de Partners](https://partner.microsoft.com/dashboard)y configurar servicios como notificaciones, pruebas a/B y asignaciones.

Al trabajar con una aplicación en el centro de Partners, verá secciones en el menú de navegación izquierdo para la **Administración**de **servicios** y aplicaciones. Puedes expandir estas secciones para tener acceso a la funcionalidad que se describe a continuación.

## <a name="services"></a>Servicios

La sección **Servicios** te permite administrar varios servicios diferentes para tus aplicaciones.

## <a name="xbox-live"></a>Xbox Live

Si va a publicar un juego, puede habilitar el [programa de creadores de Xbox Live](https://www.xbox.com/developers/creators-program) en esta página. Esto le permite empezar a configurar y probar las características de Xbox Live y, finalmente, publicar el juego de programas de Xbox Live Creators.

Para obtener más información, consulte Introducción al [programa de creadores de Xbox Live](https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators) y [creación de un nuevo título de programa de Xbox Live Creators y publicación en el entorno de prueba](https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/create-and-test-a-new-creators-title).

## <a name="experimentation"></a>Experimentación

Usa la página **Experimentación** para crear y ejecutar experimentos para las aplicaciones para la Plataforma universal de Windows (UWP) con pruebas A/B. En una prueba A/B, se mide la eficacia en tu aplicación de los cambios de características (o variaciones) en algunos clientes antes de habilitar los cambios para todos.

Para obtener más información, consulta [Ejecutar experimentos en la aplicación con pruebas A/B](../monetize/run-app-experiments-with-a-b-testing.md).

## <a name="maps"></a>Maps

Para usar los servicios de mapa en aplicaciones destinadas a Windows 10 o Windows 8.x, visita el [Centro de desarrollo de Mapas de Bing](https://www.bingmapsportal.com/). Para obtener información sobre cómo solicitar una clave de autenticación de Maps en el centro de desarrollo de mapas de Bing y agregarla a la aplicación, consulte [solicitar una clave de autenticación de Maps](../maps-and-location/authentication-key.md) para obtener más información. 

Use la página **asignaciones** solo para aplicaciones publicadas anteriormente para Windows Phone 8,1 y versiones anteriores. Para usar los servicios de mapa en estas aplicaciones, deberá solicitar un identificador de aplicación de servicio de mapa y un token para incluirlos en el código de la aplicación. Al hacer clic en **obtener token**, se generará un identificador de aplicación de servicio de mapa (**ApplicationID**) y un token de autenticación de servicio de mapa (**AuthenticationToken**) para la aplicación. Asegúrese de agregar estos valores al código antes de empaquetar y enviar la aplicación. Para obtener más información, consulta [Cómo agregar un control de mapa a una página (Windows Phone 8.1)](https://docs.microsoft.com/previous-versions/windows/apps/jj207033(v=vs.105)?redirectedfrom=MSDN).

## <a name="product-collections-and-purchases"></a>Colecciones y compras de producto

Para usar la API de colección de Microsoft Store y la API de compra de Microsoft Store para tener acceso a la información de propiedad de aplicaciones y complementos, debe especificar aquí los identificadores de cliente de Azure AD asociados. Ten en cuenta que la aplicación de los cambios puede tardar hasta 16 horas.

Para obtener más información, consulta [Administrar los derechos de producto de un servicio](../monetize/view-and-grant-products-from-a-service.md).

## <a name="administrator-consent"></a>Consentimiento del administrador

Si el producto se integra con Azure AD y llama a las API que solicitan [permisos de aplicación o permisos delegados](https://developer.microsoft.com/graph/docs/concepts/permissions_reference) que requieren el consentimiento del administrador, escriba su ID. de cliente de Azure ad aquí. Esto permite a los administradores que adquieren la aplicación para su organización dar su consentimiento para que el producto actúe en nombre de todos los usuarios del inquilino.

Para obtener más información, consulte [solicitar consentimiento para todo un inquilino](https://docs.microsoft.com/azure/active-directory/develop/v2-permissions-and-consent#requesting-consent-for-an-entire-tenant).

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

La sección **WNS/MPNS** proporciona opciones para ayudarle a crear y enviar notificaciones a los clientes de su aplicación. 

> [!TIP]
> En el caso de las aplicaciones UWP, se recomienda usar la característica de **notificaciones** del centro de Partners. Esta característica le permite enviar notificaciones a todos los clientes de la aplicación o a un subconjunto dirigido de los clientes de Windows 10 que cumplan los criterios que haya definido en un [segmento de cliente](create-customer-segments.md). Para obtener más información, consulta [Enviar notificaciones a los clientes de la aplicación](send-push-notifications-to-your-apps-customers.md).

En función del tipo de paquete de la aplicación y de sus requisitos específicos, también puede usar una de las siguientes opciones: 

-   **Servicios de notificaciones de inserción de Windows (WNS)** te permite enviar notificaciones del sistema, iconos, distintivos y actualizaciones sin procesar desde tu propio servicio en la nube. Para más información, consulta [Introducción a los Servicios de notificaciones de inserción de Windows (WNS)](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md).

-   **Aplicaciones móviles de Microsoft Azure** te permite enviar notificaciones push, autenticar y administrar los usuarios de la aplicación y almacenar datos de la aplicación en la nube. Para obtener más información, consulta la [documentación de Aplicaciones móviles](https://docs.microsoft.com/azure/app-service-mobile/).

-   El **servicio de notificaciones de envío de Microsoft (MPNS)** se puede usar con paquetes. xap publicados anteriormente para Windows Phone. Puedes enviar un número limitado de notificaciones no autenticadas sin realizar ninguna configuración, aunque recomendamos usar notificaciones autenticadas para evitar limitaciones. Si está usando MPNS, deberá cargar un certificado en el campo proporcionado en la página **WNS/MPNS** . Para obtener más información, consulta [Configurar un servicio web autenticado para enviar notificaciones de inserción para Windows Phone 8](https://docs.microsoft.com/previous-versions/windows/apps/ff941099(v=vs.105)?redirectedfrom=MSDN).
 

 
