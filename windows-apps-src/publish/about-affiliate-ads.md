---
author: jnHs
Description: "Si tu aplicación usa un objeto AdMediatorControl o AdControl para mostrar anuncios en banners, puedes aumentar la velocidad de relleno de anuncios y los ingresos mostrando anuncios de filiales de Microsoft en la aplicación."
title: Acerca de los anuncios de filiales
ms.assetid: 7B5478FB-7E68-4956-82EF-B43C2873E3EF
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: de99f73022987410ee95973542d9a80e9e130e39
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="about-affiliate-ads"></a>Acerca de los anuncios de filiales

Si tu aplicación usa un objeto **AdMediatorControl** o **AdControl** para mostrar anuncios en banners, puedes aumentar los ingresos y la velocidad de relleno de anuncios mostrando anuncios de filiales de Microsoft en tu aplicación para los productos de la Tienda. Cuando los usuarios hacen clic en los anuncios de filiales y compran productos dentro de una ventana de atribución determinada, obtienes ingresos sobre las compras aprobadas.

Así es cómo funciona este programa:

* Después de [participar en el programa de anuncios de filiales de Microsoft](#opt-in) en el Centro de desarrollo, Microsoft elige los anuncios de productos principales en la Tienda para proporcionar a tu aplicación. Estos productos pueden incluir aplicaciones, juegos, música, películas, hardware y software.

 > **Nota** Microsoft solo muestra anuncios de filial en los siguientes tamaños unitarios de anuncio: 300 x 50, 480 x 80, 160 x 600, 300 x 250 o 728 x 90.

* Microsoft proporcionará anuncios de filiales en la aplicación solo cuando no haya anuncios de otras redes de anuncios disponibles. El objetivo es ayudar a rentabilizar tus impresiones en blanco y maximizar la velocidad de relleno de anuncios.
* Si un usuario hace clic en un anuncio de filial y compra cualquier producto de la Tienda dentro de una ventana de atribución determinada, se te pagará un porcentaje de ingresos o una comisión fija por la compra (de hasta el 10%).

  Para poder recibir una comisión, el anuncio de filial debe generar una venta válida en la Tienda en dispositivos Windows 10 o en la Tienda web; las ventas de la Tienda realizadas en dispositivos Windows 8.x no tienen derecho a comisión. Los anuncios de filial de productos de la Tienda (como aplicaciones, juegos, música y películas) solo se muestran en dispositivos Windows 10. Los anuncios de filial de productos de la Tienda web (como dispositivos y software) se muestran en dispositivos Windows 10 y Windows 8.x.

    > **Nota**: Puedes cobrar por *cualquier* producto que el usuario compre desde la ventana de atribución, no solo por el producto que se promocionaba en la aplicación. En el caso de las aplicaciones gratuitas que se promocionan en tu aplicación, puedes obtener un porcentaje de ingresos por las compras desde la aplicación que hubiera realizado el usuario dentro de la ventana de atribución.

* Las ganancias que acumules en relación con el programa de anuncios de filiales de Microsoft se distribuirán en la [cuenta de pago configurada en el Centro de desarrollo](setting-up-your-payout-account-and-tax-forms.md), junto con las ganancias de Microsoft Advertising.
* Para hacer un seguimiento del rendimiento de los anuncios de filiales en la aplicación, consulta el [informe de rendimiento de filiales](affiliates-performance-report.md). Puedes hacer un seguimiento de las compras diarias realizadas a través de los anuncios de filiales de la aplicación y del porcentaje de ingresos recibido.  

  > **Nota** Cuando un usuario compra un producto en la Tienda, se produce un período de espera de 45 días hasta que la compra puede aprobarse para el programa de anuncios de filial. Debido a este período de espera, los datos de **ingresos estimados**, **compras (aprobadas)** y **compras (pendiente de aprobación)** del [informe de rendimiento de afiliados](affiliates-performance-report.md) de un día determinado pueden cambiar cuando se aprueben o rechacen las compras.

<span id="opt-in" />
## <a name="how-to-opt-in-to-the-affiliate-ads-program"></a>Cómo participar en el programa de anuncios de filiales

Para participar en el programa de anuncios de filiales de Microsoft:

1. Ve a la página **Monetización** &gt; **Monetizar con anuncios** en el panel del Centro de desarrollo de Windows.
2. En la sección **Microsoft affiliate ads**, activa la casilla **Show Microsoft affiliate ads in my app**.

Después de activar o desactivar esta casilla, no es necesario volver a publicar la aplicación para que los cambios surtan efecto.


## <a name="related-topics"></a>Temas relacionados


* [Monetizar con anuncios](monetize-with-ads.md)
* [Informe de rendimiento de filiales](affiliates-performance-report.md)
