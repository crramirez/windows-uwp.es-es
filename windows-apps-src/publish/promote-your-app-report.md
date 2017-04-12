---
author: shawjohn
title: "Informe Promocionar la aplicación: desarrollar aplicaciones para UWP"
ms.assetid: 8C5907A6-8059-4CAF-951F-C97301A5EEDF
description: "El informe &quot;Promocionar la aplicación&quot; en el panel del Centro de desarrollo de Windows te permite ver el rendimiento de las campañas de anuncios de promoción de tu aplicación."
ms.author: johnshaw
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, UWP, promote, promoción, app, aplicación, campaign, campaña, report, informe, installs, instalaciones"
ms.openlocfilehash: 25127b8f7214a712b10a1beda1e9b277370e6df2
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="promote-your-app-report"></a>Informe Promocionar la aplicación

El informe **Promocionar la aplicación** en el panel del Centro de desarrollo de Windows te permite ver el rendimiento de las campañas de anuncios de promoción de tu aplicación.

Para ver el informe, selecciona **Promociones** en el menú de navegación superior. En la parte superior de la página **Promocionar la aplicación**, verás una lista de tus campañas publicitarias y métricas de rendimiento en forma de tabla.

Al final de la página, verás las métricas de rendimiento como líneas trazadas en un gráfico con la métrica seleccionada en el eje Y y el tiempo en el eje X. Selecciona los encabezados de pestaña para ver datos de estas métricas de rendimiento:

-   **Impresiones**: número de veces que se ha mostrado tu anuncio a los clientes.
-   **Clics**: número de veces que un cliente ha hecho clic en el anuncio.
-   **Conversiones**: si el objetivo de la campaña es aumentar las instalaciones de la aplicación, una conversión es el número de veces que alguien ha visto el anuncio y ha instalado la aplicación durante las 24 horas siguientes. Si el objetivo de la campaña es aumentar la actividad en la aplicación, una conversión es el número de veces que alguien ha visto el anuncio y ha abierto la aplicación durante las 24 horas siguientes. Consulta a continuación la información sobre el seguimiento de instalaciones y la medición de conversiones.
-   **Gasto**: cantidad de dinero que has gastado en cada campaña.

Puedes mostrar datos de rendimiento de hasta seis campañas publicitarias a la vez. Selecciona **Más campañas** para elegir las campañas que se mostrarán. También puedes seleccionar el signo menos en una campaña mostrada para quitarla.

### <a name="what-is-install-tracking"></a>¿Qué es el seguimiento de instalaciones?

Ejecutar una campaña publicitaria de instalaciones a través de **Promocionar la aplicación** en el Centro de desarrollo proporciona una exposición muy necesaria para anunciar las aplicaciones. Las impresiones de anuncios se muestran a los clientes que probablemente están más interesados en la aplicación, y los clientes hacen clic en el anuncio e instalan la aplicación desde la Tienda. Antes, era difícil distinguir entre instalaciones que procedían de una campaña publicitaria frente a las instalaciones procedentes de otras fuentes.

El informe **Promocionar la aplicación** muestra cuántas instalaciones has ganado mediante la ejecución de la campaña publicitaria. Esto solo representa las descargas que son un resultado directo de la campaña publicitaria y no incluye las descargas de otras fuentes.

Mediante la supervisión de las instalaciones de tus campañas publicitarias, puedes medir el retorno de la inversión del dinero gastado en promocionar las aplicaciones. También te ayuda a comparar el coste de captar a un nuevo cliente con el valor del ciclo de vida de los clientes.

### <a name="how-are-conversions-measured"></a>¿Cómo se miden las conversiones?

Las campañas publicitarias **Promocionar tu aplicación** realizan impresiones de anuncios dentro de otras aplicaciones. Los clientes que se exponen al anuncio probablemente instalarán la aplicación de una de estas dos maneras: haciendo clic en el anuncio o en función de la visualización de la impresión del anuncio.

Si se muestra un anuncio a un cliente y este instala la aplicación en las 24 horas siguientes, ya sea haciendo clic en el anuncio o yendo directamente a la página de la aplicación en la Tienda, la instalación se atribuye a la campaña que entregó la impresión.

Una instalación se sigue en la Tienda (por teléfono, tableta, PC y otros dispositivos Windows 10) según el cliente que ha instalado la aplicación. El motor de publicidad realiza un seguimiento de los clientes que ven el anuncio y nosotros usamos esta información para correlacionar los clientes que vieron el anuncio con los que instalaron la aplicación. Para contar la instalación de la aplicación, se deben cumplir unos cuantos requisitos:

1.  El cliente no ha rechazado la identificación.
2.  El cliente ha iniciado sesión con una cuenta Microsoft.
3.  El cliente cumple los requisitos [COPPA](http://go.microsoft.com/fwlink?LinkId=536558) (no se puede hacer un seguimiento de los clientes que no los cumplen).

En consecuencia, es posible que el seguimiento de instalaciones de la aplicación *informe* de una cantidad inferior al número real de instalaciones generadas por la campaña **Promocionar tu aplicación**. Ten en cuenta que el número total de descargas de una aplicación se puede ver en el informe **Adquisiciones** en el Centro de desarrollo.

## <a name="account-billing-history"></a>Historial de facturación de la cuenta

Para ver todas las transacciones asociadas con tu cuenta, selecciona **Historial de facturación** en el menú de navegación izquierdo.

Para cada transacción, mostramos los datos de **Fecha de la transacción**, **Nombre de la campaña** correspondiente, **Método de pago** cargado, **Id. de pago**, **Fecha de inicio de la facturación**, **Fecha de finalización de la facturación**, **Importe total** del cargo y **Estado del pago**.

Si prefieres descargar el historial de facturación de la cuenta como un documento de Microsoft Word, puedes hacer clic en el vínculo **Descargar**.

## <a name="related-topics"></a>Temas relacionados

* [Crear una campaña publicitaria para tu aplicación](create-an-ad-campaign-for-your-app.md)
* [Administrar campañas publicitarias](managing-your-ad-campaign.md)
* [Acerca de los anuncios internos](about-house-ads.md)
* [Preguntas comunes](common-questions.md)
 

 
