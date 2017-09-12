---
author: jnHs
Description: "Administra y visualiza los detalles relacionados con cada una de tus aplicaciones en el panel de información del Centro de desarrollo de Windows y configura servicios, tales como notificaciones, pruebas A/B y mapas."
title: "Administración y servicios de aplicaciones"
ms.assetid: 99DA2BC1-9B5D-4746-8BC0-EC723D516EEF
ms.author: wdg-dev-content
ms.date: 06/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: 2ff75a5e85a37f4f47b6b32f3e8338524752bb2f
ms.sourcegitcommit: fadde8afee46238443ec1cb71846d36c91db9fb9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/21/2017
---
# <a name="app-management-and-services"></a>Administración y servicios de aplicaciones

Puedes administrar y visualizar los detalles relacionados con cada una de tus aplicaciones en el panel de información del Centro de desarrollo de Windows y configurar servicios, tales como notificaciones, pruebas A/B y mapas.

Al trabajar con una aplicación en el panel, verás secciones en el menú de navegación izquierdo para administrar **servicios** y **aplicaciones**. Puedes expandir estas secciones para tener acceso a la funcionalidad que se describe a continuación.

## <a name="services"></a>Servicios

La sección **Servicios** te permite administrar varios servicios diferentes para tus aplicaciones.

## <a name="push-notifications"></a>Notificaciones de inserción

La sección **Notificaciones de inserción** te permite crear y enviar notificaciones a clientes de la aplicación. Puedes enviarlas a todos los clientes de la aplicación o puedes elegir un subconjunto de los clientes de Windows10 que cumplan los criterios que has definido en un [segmento de clientes](create-customer-segments.md). Para obtener más información, consulta [Enviar notificaciones a los clientes de la aplicación](send-push-notifications-to-your-apps-customers.md).

Según el tipo de paquete de la aplicación y sus requisitos específicos, también puedes usar una de las siguientes opciones para notificaciones de inserción haciendo clic en **WNS/MPNS** en el menú de navegación izquierdo: 

-   **Servicios de notificaciones de inserción de Windows (WNS)** te permite enviar notificaciones del sistema, iconos, distintivos y actualizaciones sin procesar desde tu propio servicio en la nube. Para obtener más información, consulta [Introducción a los Servicios de notificaciones de inserción de Windows (WNS)](https://msdn.microsoft.com/library/windows/apps/mt187203).

-   **Aplicaciones móviles de Microsoft Azure** te permite enviar notificaciones push, autenticar y administrar los usuarios de la aplicación y almacenar datos de la aplicación en la nube. Para obtener más información, consulta la [documentación de Aplicaciones móviles](http://go.microsoft.com/fwlink/p/?LinkId=221116).

-   **Servicio de notificaciones push de Microsoft (MPNS)** puede usarse con los paquetes .xap para Windows Phone. Puedes enviar un número limitado de notificaciones no autenticadas sin realizar ninguna configuración, aunque recomendamos usar notificaciones autenticadas para evitar limitaciones. Si estás usando MPNS, tendrás que cargar un certificado en el campo proporcionado en la página **Notificaciones de inserción**. Para obtener más información, consulta [Configurar un servicio web autenticado para enviar notificaciones de inserción para Windows Phone 8](http://go.microsoft.com/fwlink/p/?LinkId=528736).

## <a name="experimentation"></a>Experimentación

Usa la página **Experimentación** para crear y ejecutar experimentos para las aplicaciones para la Plataforma universal de Windows (UWP) con pruebas A/B. En una prueba A/B, se mide la eficacia en tu aplicación de los cambios de características (o variaciones) en algunos clientes antes de habilitar los cambios para todos.

Para obtener más información, consulta [Ejecutar experimentos en la aplicación con pruebas A/B](../monetize/run-app-experiments-with-a-b-testing.md).

## <a name="maps"></a>Mapas

Para usar servicios de mapa en aplicaciones para Windows Phone 8.1 y versiones anteriores, necesitas un id. de aplicación del servicio de mapas y un token para incluirlo en el código de la aplicación. Puedes obtener este token en la página **Mapas** de la sección **Servicios**.

> [!NOTE]
> Para usar los servicios de mapa en aplicaciones destinadas a Windows10 o Windows8.x, visita el [Centro de desarrollo de Mapas de Bing](http://go.microsoft.com/fwlink/p/?LinkId=614880). Consulta [Solicitar una clave de autenticación de mapas](https://msdn.microsoft.com/library/windows/apps/mt219694) para obtener más información.

Para más información, consulta [Usar servicios de mapa](use-map-services.md).

## <a name="product-collections-and-purchases"></a>Colecciones y compras de producto

Para usar la API de colección de la TiendaWindows y la API de compra de la TiendaWindows para acceder a la información sobre la propiedad de las aplicaciones y los complementos, tienes que especificar aquí los identificadores de cliente de Azure AD asociados. Ten en cuenta que la aplicación de los cambios puede tardar hasta 16 horas.

Para obtener más información, consulta [Administrar los derechos de producto de un servicio](../monetize/view-and-grant-products-from-a-service.md).

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

 

 
