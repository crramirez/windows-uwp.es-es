---
author: mcleanbyron
ms.assetid: 772DEBF2-1578-4330-9C14-70BCC6F55005
description: "Microsoft proporciona compatibilidad con la mediación de anuncios, lo que permite optimizar los ingresos generados por la publicidad desde la aplicación al arbitrar solicitudes de anuncios de banner de varias redes de anuncios."
title: "Usar la mediación de anuncios para maximizar los ingresos"
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: c0669e35b285ee7dfeda0c039d8455a4237960f5

---

#  Usar la mediación de anuncios para maximizar los ingresos


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Microsoft proporciona compatibilidad con la mediación de anuncios, lo que permite optimizar los ingresos generados por la publicidad desde la aplicación al arbitrar solicitudes de anuncios de banner de varias redes de anuncios. Es posible que las distintas redes de anuncios puedan tener sus propias ventajas, aunque con un mayor coste por mil vistas (eCPM) o mayor velocidad de relleno (porcentaje de anuncios ofrecidos cuando la aplicación realiza una solicitud) según el mercado. El uso de una única red de anuncios puede derivar en solicitudes no cubiertas, con la consecuente pérdida de posibles ingresos. La mediación de anuncios te permite maximizar la rentabilidad de los anuncios asegurándose de que se muestra siempre un anuncio dinámico.

Compatibilidad con mediación de anuncios está disponible a través de la [participación de la tienda de Microsoft y el SDK de rentabilidad](http://aka.ms/store-em-sdk). Después de instalar el SDK y de agregar el control de Ad Mediator a la aplicación, podrás especificar la frecuencia de uso de cada red actualizando la configuración de mediación en el Centro de desarrollo. Puedes optimizar esto por mercado, para usar las redes de anuncios más eficaces en las regiones adecuadas. Además, puedes realizar cambios sobre el uso de cada red de anuncios sin necesidad de volver a publicar la aplicación.

## Introducción a la mediación de anuncios


Sigue estos pasos para instalar y configurar la mediación de anuncios en tu aplicación:

1.  Revisa la lista de tipos de redes de anuncios y proyectos compatibles con la mediación de anuncios, configurar cuentas con las redes de anuncios que deseas usar y sigue las instrucciones de cada red de anuncios para incorporar una aplicación. Para obtener más información, consulta [Seleccionar y administrar redes de anuncios](select-and-manage-your-ad-networks.md).

2.  Instala el [SDK de Microsoft Store Engagement and Monetization](http://aka.ms/store-em-sdk) con Visual Studio 2015 o Visual Studio 2013.

3.  En Visual Studio, abre el proyecto o crea uno nuevo. Abre la página donde deseas hospedar anuncios y arrastra un **AdMediatorControl** a la página. Si usas Microsoft Advertising, ajusta el alto y el ancho del control para ajustarlo a los tamaños de anuncios admitidos. Para más información, consulta [Agregar y usar el control de Ad Mediator](add-and-use-the-ad-mediator-control.md).

4.  Ejecuta **Servicios conectados** para elegir las redes de anuncios que quieras como destino y configura los parámetros necesarios para cada red de anuncios. Para más información, consulta [Agregar y usar el control de Ad Mediator](add-and-use-the-ad-mediator-control.md).

5.  Prueba la implementación de mediación de anuncios en tu aplicación. Para más información, consulta [Probar la implementación de mediación de anuncios](test-your-ad-mediation-implementation.md).

6.  Envía la aplicación al panel del Centro de desarrollo de Windows y configura la mediación de anuncios en el panel. Para más información, consulta [Enviar la aplicación y configurar la mediación de anuncios](submit-your-app-and-configure-ad-mediation.md).

7.  Revisa los informes de mediación de anuncios en el panel del Centro de desarrollo. Para obtener más información, consulta [Informe de mediación de anuncios](https://msdn.microsoft.com/library/windows/apps/mt148521).

## Uso de Microsoft Advertising sin mediación de anuncios


Si no deseas usar la mediación de anuncios o si el tipo de proyecto no es compatible actualmente con la mediación de anuncios, puedes seguir proporcionando banners desde Microsoft sin usar la mediación de anuncios. Para obtener más información, consulta [AdControl en XAML y .NET](https://msdn.microsoft.com/library/mt313186.aspx) y [AdControl en HTML 5 y JavaScript](https://msdn.microsoft.com/library/mt313130.aspx).

## Temas relacionados

* [Seleccionar y administrar redes de anuncios](select-and-manage-your-ad-networks.md)
* [Agregar y usar el control de mediación de anuncios](add-and-use-the-ad-mediator-control.md)
* [Probar la implementación de mediación de anuncios](test-your-ad-mediation-implementation.md)
* [Enviar la aplicación y configurar la mediación de anuncios](submit-your-app-and-configure-ad-mediation.md)
* [Solucionar problemas de mediación de anuncios](troubleshoot-ad-mediation.md)
 

 



<!--HONumber=Jun16_HO4-->


