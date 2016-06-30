---
author: jnHs
Description: "Puedes administrar y ver los detalles relacionados con cada una de tus aplicaciones en el panel del Centro de desarrollo de Windows y configurar servicios como notificaciones de inserción y Mapas."
title: "Administración y servicios de aplicaciones"
ms.assetid: 99DA2BC1-9B5D-4746-8BC0-EC723D516EEF
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 71077ac3f64e10734021e5fb655168f7273e3cb6

---

# Administración y servicios de aplicaciones

Puedes administrar y ver los detalles relacionados con cada una de tus aplicaciones en el panel del Centro de desarrollo de Windows y configurar servicios como notificaciones de inserción y Mapas.

Al trabajar con una aplicación en el panel, verás secciones en el menú de navegación izquierdo para administrar servicios y aplicaciones. Puedes expandir estas secciones para tener acceso a la funcionalidad que se describe a continuación.

## Servicios

La sección **Servicios** te permite administrar varios servicios diferentes para tus aplicaciones.

### Notificaciones de inserción

Según el tipo de paquete de la aplicación y sus requisitos específicos, puedes usar una de las siguientes opciones para las notificaciones de inserción:

-   **Servicios de notificaciones de inserción de Windows (WNS)** te permite enviar notificaciones del sistema, iconos, distintivos y actualizaciones sin procesar desde tu propio servicio en la nube. Para obtener más información, consulta [Introducción a los Servicios de notificaciones de inserción de Windows (WNS)](https://msdn.microsoft.com/library/windows/apps/mt187203).

-   **Aplicaciones móviles de Microsoft Azure** te permite enviar notificaciones push, autenticar y administrar los usuarios de la aplicación, y almacenar datos de la aplicación en la nube. Para obtener más información, consulta la [documentación de Aplicaciones móviles](http://go.microsoft.com/fwlink/p/?LinkId=221116).

-   **Servicio de notificaciones push de Microsoft (MPNS)** puede usarse con los paquetes .xap para Windows Phone. Puedes enviar un número limitado de notificaciones no autenticadas sin realizar ninguna configuración, aunque recomendamos usar notificaciones autenticadas para evitar limitaciones. Si estás usando MPNS, tendrás que cargar un certificado en el campo proporcionado en la página **Notificaciones de inserción**. Para obtener más información, consulta [Configurar un servicio web autenticado para enviar notificaciones de inserción para Windows Phone 8](http://go.microsoft.com/fwlink/p/?LinkId=528736).

### Experimentación

Usa la página **Experimentación** para crear y ejecutar experimentos para las aplicaciones para la Plataforma universal de Windows (UWP) con pruebas A/B. En una prueba A/B, se mide la eficacia en tu aplicación de los cambios de características (o variaciones) en algunos clientes antes de habilitar los cambios para todos.

Para obtener más información, consulta [Ejecutar experimentos en la aplicación con pruebas A/B](../monetize/run-app-experiments-with-a-b-testing.md).

### Mapas

Para usar servicios de mapa en aplicaciones para Windows Phone 8.1 y versiones anteriores, necesitas un id. de aplicación del servicio de mapas y un token para incluirlo en el código de la aplicación. Puedes obtener este token en la página **Mapas** de la sección **Servicios**.

> **Nota**  Para usar servicios de mapa en aplicaciones destinadas a otros sistemas operativos, visita el [Centro de desarrollo de Mapas de Bing](http://go.microsoft.com/fwlink/p/?LinkId=614880). Consulta [Solicitar una clave de autenticación de mapas](https://msdn.microsoft.com/library/windows/apps/mt219694) para obtener más información.

Para más información, consulta [Usar servicios de mapa](use-map-services.md).

### Colecciones y compras de producto

Para usar la API de colección de la Tienda Windows y la API de compra de la Tienda Windows para acceder a la información sobre la propiedad de las aplicaciones y los IAP, tienes que especificar aquí los identificadores de cliente Azure AD asociados. Ten en cuenta que la aplicación de los cambios puede tardar hasta 16 horas.

Para más información, consulta [Ver y conceder productos desde un servicio](https://msdn.microsoft.com/library/windows/apps/mt609002).

## Administración de aplicaciones

La sección **Administración de aplicaciones** te permite ver los detalles de identidad y del paquete, además de administrar los nombres de la aplicación.

### Identidad de la aplicación

Esta página muestra detalles relacionados con la identidad única de tu aplicación dentro de la Tienda, incluidas las direcciones URL para vincular a la descripción de la aplicación.

Para más información, consulta [Ver detalles de identidad de la aplicación](view-app-identity-details.md).

### Administrar nombres de aplicación

Aquí es donde puedes ver todos los nombres que has reservado para tu aplicación. Puedes reservar más nombres aquí o eliminar los que ya no usas.

Para más información, consulta [Administrar nombres de aplicación](manage-app-names.md).

### Paquetes actuales

Esta página permite ver los detalles relacionados con todos los paquetes publicados.

> **Nota**  No verás información aquí hasta que no se haya publicado la aplicación.

Se muestra el nombre, la versión y la arquitectura de cada paquete. Haz clic en **Detalles** para mostrar información adicional como el idioma admitido, las capacidades de la aplicación y los tamaños de archivo.

La información exacta que ves para cada paquete puede variar en función de su sistema operativo de destino y otros factores. Por ejemplo, si has agregado [mediación de anuncios de Windows](https://msdn.microsoft.com/library/windows/apps/mt219691) al paquete, encontrarás un vínculo para configurar la mediación para ese paquete aquí.

Los desarrolladores con permisos de OEM también pueden [generar paquetes de preinstalación](generate-preinstall-packages-for-oems.md) desde la página **Paquetes actuales**.

 

 



<!--HONumber=Jun16_HO4-->


