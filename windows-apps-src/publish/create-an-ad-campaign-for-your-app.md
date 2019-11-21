---
Description: You can create ad campaigns in Partner Center to help promote your app and grow your app's user base.
title: Crear una campaña publicitaria para la aplicación
ms.assetid: 10D94929-92C4-4379-AA5F-6FEF879F2463
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, anuncio, campaña, promover
ms.localizationpriority: medium
ms.openlocfilehash: ec5d23a502c5517193956da6c57efe1eb668e0f0
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260019"
---
# <a name="create-an-ad-campaign-for-your-app"></a>Crear una campaña publicitaria para la aplicación

You can create ad campaigns in [Partner Center](https://partner.microsoft.com/dashboard) to help promote your app and grow its user base. By default, we will choose the target audience for your ads based on the settings for your app in Partner Center, but you can optionally define your own audience. También puedes usar un conjunto predeterminado de plantillas de anuncios o cargar tus propios diseños de anuncio. Para obtener más información sobre las campañas publicitarias, consulta [Preguntas comunes sobre las campañas de anuncios](common-questions.md).

Puedes crear campañas de anuncios solo para las aplicaciones que hayan pasado a la fase final de publicación del [proceso de certificación de aplicaciones](the-app-certification-process.md).

> [!NOTE]
> This section of the documentation describes how to create an ad campaign in Partner Center. Other campaign options to create and manage ad campaigns programmatically include [Vungle](https://vungle.com/) and the [Microsoft Store promotions API](../monetize/run-ad-campaigns-using-windows-store-services.md).

## <a name="instructions"></a>Instrucciones

Aquí te mostramos cómo crear una campaña publicitaria con el fin de promocionar una aplicación.

1.  From the left navigation menu of [Partner Center](https://partner.microsoft.com/dashboard), expand **Attract** and then select **Ad campaigns**.
2.  Selecciona **Crear campaña** (o si has creado campañas anteriormente, selecciona **Nueva campaña**).
3.  En la página siguiente, en la sección **Tipo de objetivo**, elige una de las siguientes opciones:
    * **Incrementar las instalaciones de la aplicación**. Selecciona esta opción si la campaña publicitaria está destinada a conseguir que haya personas que instalen tu aplicación.
    * **Incrementar el compromiso con la aplicación**. Selecciona esta opción si la campaña publicitaria pretende conseguir que tus clientes aumenten el uso que hacen de la aplicación. Si seleccionas esta opción, puedes dirigir la campaña publicitaria a [segmentos de clientes](create-customer-segments.md) específicos que definas.

4.  Selecciona la aplicación que quieras promocionar con esta campaña. Ten en cuenta que la aplicación ya debe estar disponible en la Tienda.
5.  Revisa el nombre proporcionado para la campaña en el campo **Nombre de la campaña** y realiza cambios, si lo deseas.
6.  En **Tipo de campaña**, elige una de estas opciones:
    * **Anuncio pagado**: estos anuncios se ejecutarán en cualquier aplicación que coincida con el dispositivo y la categoría de la aplicación. Para las campañas nuevas creadas a partir del 9 de enero de 2017, estos anuncios también aparecerán en MSN.com, Outlook.com, Skype y otras propiedades premium de Microsoft. Las campañas de promoción de aplicaciones dirigidas a aplicaciones y propiedades premium de Microsoft se conocen como campañas *universales*.
    * **Anuncio de la comunidad (gratuito)** : estos anuncios se ejecutarán en aplicaciones publicadas por otros desarrolladores que también crean campañas publicitarias de la comunidad. Para poder seleccionar esta opción, debes haber optado por mostrar los anuncios de la comunidad en la página **Monetizar** -> **Anuncios en la aplicación**. Para obtener más información, consulta [Acerca de la publicidad de la comunidad](about-community-ads.md).
    * **Anuncio interno (gratuito)** : estos anuncios solo se ejecutarán en tus aplicaciones que coincidan con el tipo de dispositivo de la aplicación anunciada. La publicidad interna es gratuita. Para obtener más información, consulta [Acerca de los anuncios internos](about-house-ads.md).

7.  En el caso de campañas publicitarias de pago, confirma la opción **Campaign duration** (el período de tiempo en el que se gastará el presupuesto asignado a la campaña). La opción predeterminada es **Mensualmente**, lo que significa que el presupuesto asignado a la campaña se gastará cada mes de forma recurrente hasta que finalices la campaña. Si tienes una cuenta premium, puedes elegir la opción **Personalizado** para especificar un intervalo de fecha y hora durante el que se gastará el presupuesto asignado a la campaña. Para obtener información sobre las cuentas premium, consulta [Preguntas comunes sobre las campañas publicitarias](common-questions.md#how-can-i-increase-the-maximum-monthly-budget-amount-allowed-for-my-ad-campaign).

8.  Confirma la información de presupuesto y pago. (Si vas a crear una campaña interna o de la comunidad, estas opciones no aparecerán, ya que estas campañas son gratuitas).
    * En **Presupuesto**, usa el control deslizante para establecer la cantidad de dinero que quieres gastar cada mes para mostrar el anuncio (o el presupuesto total, si has seleccionado una duración de campaña personalizada).

        El presupuesto mensual se prorratea el mes en el que se crea la campaña publicitaria. En otras palabras, si creas una campaña publicitaria a mitad de un mes natural, se te cobrará la mitad del presupuesto mensual para ese mes.

    * Especifica un método de pago para la campaña publicitaria haciendo clic en **Agregar nuevo método de pago** y rellena los detalles de la cuenta. Si ya dispones de un instrumento de pago, puedes seleccionar **Elige otro método de pago** si necesitas actualizarlo. The country/region of your payment method's billing address must match the country/region associated with your developer account.

    * Si has recibido un cupón de un representante de Microsoft para pagar una campaña publicitaria, haz clic en **Utilizar un cupón**, escribe el código del cupón y haz clic en **Aplicar** para aplicar el cupón a la campaña.

    Cuando hayas terminado, haz clic en **Guardar y siguiente** para continuar al paso **Audiencia**. This step is not available for House ad campaigns, since they run only in your own apps.

9.  En la página **Audiencia**, mostraremos la configuración de audiencia que recomendamos para la campaña. También puedes ajustar esta información:
    * **Países/regiones**: elige hasta 5 países o regiones en los que quieras que aparezca tu anuncio. Para obtener una lista de los países o regiones admitidos, consulta [Preguntas comunes sobre las campañas publicitarias](common-questions.md#where-will-my-ad-appear).

    * **Dispositivos**: elige los tipos de dispositivos en los que quieres que aparezcan estos anuncios. Solo se muestran los tipos de dispositivo admitidos por la aplicación.

    * **Surface**: elige **Universal** para que tus anuncios aparezcan en aplicaciones, además de en MSN.com, Outlook.com, Skype y otras propiedades premium de Microsoft. Elige **Aplicación** si solo quieres que tu anuncio aparezca en las aplicaciones.

    * **Sistema operativo**: elige los sistemas operativos en los que debe aparecer el anuncio. Solo se muestran los sistemas operativos admitidos por la aplicación.

    * **Sexo**: elige esta opción si deseas restringir la audiencia de tu anuncio por sexo.

    * **Intervalo de edad**: selecciona el intervalo de edad de la audiencia que desees.

    Esta sección también muestra un gráfico de **Alcance estimado**. Este gráfico muestra la audiencia a la que puedes esperar llegar con tus selecciones actuales de destinatarios como un porcentaje de todos los usuarios de aplicaciones habilitadas para anuncios de Windows en los mercados seleccionados.

10.  Si elegiste el objetivo de campaña **Incrementar el compromiso con la aplicación**, puedes seleccionar uno de los segmentos de clientes como destino. Los anuncios creados con esta campaña solo se mostrarán a los clientes que estén incluidos en el segmento. Solo se puede seleccionar un segmento por campaña publicitaria. Para información sobre los segmentos, consulta [Crear segmentos de clientes](create-customer-segments.md). Cuando hayas terminado, haz clic en **Guardar y siguiente** para continuar al paso **Diseño de anuncios**. This step is not available for House ad campaigns, because they run only in your own apps.

11.  En la página **Diseño de anuncios**, elige una de estas opciones:
    * **Anuncio generado automáticamente**. This is the default option, and it allows you to create an ad from our default templates. Puedes realizar selecciones para personalizar el contenido de tu anuncio y obtendremos una vista previa del aspecto de tu anuncio según tus elecciones (se actualiza automáticamente a medida que realizas las selecciones).
        * En el menú desplegable **Idioma**, selecciona el idioma para el anuncio. El texto del distintivo de Microsoft Store aparecerá en el idioma que selecciones.
        * Para agregar una línea de texto adicional al anuncio, escribe el texto en el campo **Lema personalizado**.
            > [!NOTE]
            > El texto que escribas aquí debe estar localizado en el idioma seleccionado. El lema personalizado se rechazará si el texto no cumple las [Directivas de Bing Ads](https://advertise.bingads.microsoft.com/bing-ads-policies). Consulta esta página para obtener instrucciones sobre el estilo y sobre el contenido no permitido.
        * Para personalizar aún más el anuncio, expande **Personalizar el diseño del anuncio/ver los tamaños de los anuncios** y elige una de estas opciones:
            * **Color de fondo**. Elige entre las opciones disponibles.
            * **Imágenes**. Elige una de las imágenes disponibles (tomadas de la descripción de la Tienda de la aplicación).
            * **Mostrar clasificación de mi aplicación**. Selecciona esta casilla si quieres mostrar la clasificación de la aplicación.
            * **Mostrar que mi aplicación es gratuita**. Si la aplicación es gratuita en todos los mercados seleccionados, tienes la opción de seleccionar esta casilla.
            * **Llamada a la acción**. Si elegiste **Incrementar el compromiso con la aplicación** como objetivo de la campaña, puedes establecer el botón de llamad a la acción de tu anuncio en **Abrir**, **Reproducir**, **Leer**, **Escuchar** o **Comprar**.  

    * **Custom**. Elige esta opción para usar tus propios diseños de anuncio. Note that if you selected a customer segment earlier, you must use custom creatives. Puedes cargar distintos archivos para cada uno de los tamaños de anuncio disponibles. Los archivos deben cumplir los siguientes requisitos y directrices:
        * Cada archivo debe ser un .png o .jpg de 40 KB o menos.
        * Los diseños de tus anuncios deben cumplir los requisitos especificados en la [Microsoft Creative Acceptance Policy](https://about.ads.microsoft.com/solutions/ad-products/display-advertising/creative-acceptance-policies).
        * El contenido de los diseños de anuncio debe ser pertinente para la aplicación que promueves. Los diseños de anuncios que no están relacionados con la aplicación no se distribuirán a los anuncios de otras aplicaciones.
        * Todo el contenido de los diseños de anuncio debe ser legible. Por ejemplo, el contenido no debe aparecer borroso, pixelado ni estirado.

12.  Si tienes una [cuenta premium](common-questions.md#how-can-i-increase-the-maximum-monthly-budget-amount-allowed-for-my-ad-campaign), puedes usar la casilla **Dirección URL de destino** para controlar lo que sucede cuando un cliente hace clic en el anuncio.
    * Si dejas la casilla vacía, cuando un cliente haga clic en el anuncio, se mostrará la descripción de la Tienda de la aplicación.
    * If you are using Adjust, Kochava, Tune, or Vungle to measure install analytics for your app, enter your install tracking URL. Al guardar la campaña, se valida la dirección URL de seguimiento para garantizar que se resuelve en la página de descripción de la aplicación en Microsoft Store. For more information about install tracking with these services, see the [Adjust](https://docs.adjust.com/en/), [Kochava](https://support.kochava.com/), [Tune](https://help.tune.com/hasoffers/), and [Vungle](https://support.vungle.com/hc/en-us) documentation.
    * Si elegiste **Incrementar el compromiso con la aplicación** como objetivo de la campaña, puedes especificar un [vínculo profundo de URI](../launch-resume/handle-uri-activation.md) para redirigir a los clientes del segmento seleccionado a una página específica dentro de la aplicación.
    * Si especificas cualquier destino que no sea la página de descripción de la aplicación o una página dentro de la aplicación, la campaña se pondrá en pausa automáticamente.

13.  Por último, haz clic en **Revisar** para confirmar la configuración de la campaña publicitaria y, si es una campaña publicitaria de pago, la información de presupuesto y pago. Haz clic en **Confirmar** y tus anuncios empezarán a aparecer en dispositivos en pocas horas.

## <a name="review-ad-campaign-performance"></a>Revisar el rendimiento de una campaña publicitaria

Para ver el rendimiento de tus campañas, vuelve a la página **Campañas de anuncios**. Selecciona **Filtros de sección** para definir el ámbito de lo que se incluye en el informe por **Fecha**, **Objetivo de la campaña**, **Nombre de la aplicación**, **Tipo de campaña** o **Estado**. Además de ver información sobre las **Impresiones**, **Clics**, **Conversiones** y **Gasto** de la campaña, puedes usar el informe para **Pausar** o **Reanudar** una campaña. Para obtener más información, consulta el [Informe Campaña publicitaria](promote-your-app-report.md).

Para editar una campaña, selecciona su nombre en la lista.
