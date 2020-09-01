---
Description: Puede crear campañas de AD en el centro de partners para ayudar a promocionar su aplicación y a ampliar la base de usuarios de la aplicación.
title: Crear una campaña publicitaria para tu aplicación
ms.assetid: 10D94929-92C4-4379-AA5F-6FEF879F2463
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, ad, campaña, promover
ms.localizationpriority: medium
ms.openlocfilehash: f0294ff668ea31abb32e7d646be85a89770bfd02
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172869"
---
# <a name="create-an-ad-campaign-for-your-app"></a>Crear una campaña publicitaria para tu aplicación

>[!WARNING]
> A partir del 1 de junio de 2020, se cerrará la plataforma de monetización de Microsoft ad para aplicaciones UWP de Windows. [Más información](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Puede crear campañas de AD en el [centro de Partners](https://partner.microsoft.com/dashboard) para ayudar a promocionar su aplicación y a ampliar su base de usuarios. De forma predeterminada, elegiremos el público de destino de los anuncios en función de la configuración de la aplicación en el centro de Partners, pero también puede definir su propia audiencia. También puedes usar un conjunto predeterminado de plantillas de anuncios o cargar tus propios diseños de anuncio. Para obtener más información sobre las campañas publicitarias, consulta [Preguntas comunes sobre las campañas publicitarias](common-questions.md).

Puede crear campañas de ad solo para aplicaciones que hayan superado la fase de publicación final del [proceso de certificación](the-app-certification-process.md)de la aplicación.

> [!NOTE]
> En esta sección de la documentación se describe cómo crear una campaña de AD en el centro de Partners. Otras opciones de la campaña para crear y administrar campañas de ad incluyen mediante programación [Vungle](https://vungle.com/) y la [API de promociones Microsoft Store](../monetize/run-ad-campaigns-using-windows-store-services.md).

## <a name="instructions"></a>Instructions

Aquí se muestra cómo crear una campaña de AD para promocionar una aplicación.

1.  En el menú de navegación izquierdo del [centro de Partners](https://partner.microsoft.com/dashboard), expanda **atraer** y seleccione campañas de **anuncios**.
2.  Seleccione **crear campaña** (o, si ha creado campañas antes, seleccione **nueva campaña**).
3.  En la página siguiente, en la sección **tipo de objetivo** , elija una de las siguientes opciones:
    * **Incrementar las instalaciones de la aplicación**. Selecciona esta opción si la campaña publicitaria está destinada a conseguir que haya personas que instalen tu aplicación.
    * **Incrementar el compromiso con la aplicación**. Seleccione esta opción si la campaña de ad está pensada para que los clientes aumenten el uso de la aplicación. Si seleccionas esta opción, puedes dirigir la campaña publicitaria a [segmentos de clientes](create-customer-segments.md) específicos que definas.

4.  Seleccione la aplicación que desea promocionar con esta campaña. Tenga en cuenta que la aplicación ya debe estar disponible en el almacén.
5.  Revise el nombre proporcionado para la campaña en el campo Nombre de la **campaña** y realice los cambios que desee.
6.  En **Tipo de campaña**, elige una de estas opciones:
    * **Ad de pago**: estos anuncios se ejecutarán en cualquier aplicación que coincida con el dispositivo y la categoría de la aplicación. En el caso de las nuevas campañas creadas después del 9 de enero de 2017, estos anuncios también aparecerán en MSN.com, Outlook.com, Skype y otras propiedades de Microsoft Premium. Las campañas de promoción de aplicaciones destinadas a aplicaciones y propiedades de Microsoft Premium se conocen como campañas *universales* .
    * **Ad de la comunidad (gratis)**: estos anuncios se ejecutarán en aplicaciones publicadas por otros desarrolladores que también creen campañas de ad de la comunidad. Para poder seleccionar esta opción, debe haber optado por mostrar anuncios de la comunidad en la página **monetizar**  ->  **en la aplicación ADS** . Para obtener más información, consulta [Acerca de la publicidad de la comunidad](about-community-ads.md).
    * **House ad (gratis)**: estos anuncios solo se ejecutarán en las aplicaciones que coincidan con el tipo de dispositivo de la aplicación anunciada. La publicidad interna es gratuita. Para obtener más información, consulte [About House ADS](about-house-ads.md).

7.  En el caso de las campañas publicitarias de pago, confirme la duración de la **campaña** (el período de tiempo en el que se va a gastar el presupuesto de la campaña). La opción predeterminada es **mensual**, lo que significa que el presupuesto de la campaña se gastará cada mes de forma periódica hasta que se detenga la campaña. Si tiene una cuenta premium, puede elegir opcionalmente **personalizar** para especificar un intervalo de fecha y hora personalizado durante el que se va a gastar el presupuesto de la campaña. Para obtener información sobre las cuentas premium, consulta [Preguntas comunes sobre las campañas publicitarias](common-questions.md#how-can-i-increase-the-maximum-monthly-budget-amount-allowed-for-my-ad-campaign).

8.  Confirme su presupuesto y la información de pago. (Si va a crear una campaña de la empresa o una campaña de la comunidad, estas opciones no aparecerán, ya que estas campañas son gratuitas).
    * En **presupuesto**, use el control deslizante para establecer la cantidad de dinero que desea gastar cada mes para ejecutar el anuncio (o el presupuesto total, si ha seleccionado una duración de campaña personalizada).

        El presupuesto mensual se prorratea el mes en el que se crea la campaña publicitaria. En otras palabras, si creas una campaña publicitaria a mitad de un mes natural, se te cobrará la mitad del presupuesto mensual para ese mes.

    * Especifique un método de pago para la campaña de AD; para ello, haga clic en **Agregar nuevo método de pago** y rellene los detalles de la cuenta. Si ya ha proporcionado un instrumento de pago, puede seleccionar **elegir otro método de pago** si necesita actualizarlo. El país o región de la dirección de facturación de su método de pago debe coincidir con el país o región asociados a su cuenta de desarrollador.

    * Si ha recibido un cupón de un representante de Microsoft para pagar una campaña de anuncios, haga clic en **usar un cupón**, escriba el código del cupón y haga clic en **aplicar** para aplicar el cupón a la campaña.

    Cuando haya terminado, haga clic en **Guardar y en siguiente** para continuar con el paso **audiencia** . Este paso no está disponible para las campañas de ad de la vivienda, ya que solo se ejecutan en sus propias aplicaciones.

9.  En la página **audiencia** , se mostrará la configuración de la audiencia que recomendamos para la campaña. Opcionalmente, puede ajustar esta información:
    * **Países o regiones**: elija hasta 5 países o regiones en los que desea que aparezca el anuncio. Para obtener una lista de países o regiones admitidos, consulte [preguntas comunes sobre las campañas de anuncios](common-questions.md#where-will-my-ad-appear).

    * **Dispositivos**: elija los tipos de dispositivo en los que desea que aparezcan estos anuncios. Solo se muestran los tipos de dispositivo admitidos por la aplicación.

    * **Surface**: elija **universal** para permitir que el anuncio aparezca dentro de las aplicaciones, además de MSN.com, Outlook.com, Skype y otras propiedades de Microsoft Premium. Elija **aplicación** si solo desea que el anuncio aparezca dentro de las aplicaciones.

    * **Sistema operativo**: elija los sistemas operativos en los que debe aparecer el anuncio. Solo se muestran los sistemas operativos admitidos por la aplicación.

    * **Sexo**: elija si quiere restringir la audiencia de su anuncio por sexo.

    * **Rango de edad**: seleccione los intervalos de edad para la audiencia deseada.

    Esta sección también muestra un gráfico de **Alcance estimado**. En este gráfico se muestra la audiencia a la que puede esperar tener acceso con las selecciones de destinatarios actuales como porcentaje de todos los usuarios de la aplicación habilitada para AD en los mercados seleccionados.

10.  Si ha elegido **aumentar el compromiso en la aplicación** como objetivo de la campaña, puede seleccionar uno de los segmentos de cliente como destino. Los anuncios creados con esta campaña solo se mostrarán a los clientes que se incluyen en el segmento. Solo se puede seleccionar un segmento por campaña publicitaria. Para información sobre los segmentos, consulta [Crear segmentos de clientes](create-customer-segments.md). Cuando haya terminado, haga clic en **Guardar y en siguiente** para continuar con el paso de **diseño de ad** . Este paso no está disponible para las campañas publicitarias de la vivienda, ya que solo se ejecutan en sus propias aplicaciones.

11.  En la página de **diseño de ad** , elija una de estas opciones:
    * **Generado automáticamente**. Esta es la opción predeterminada y permite crear un anuncio a partir de nuestras plantillas predeterminadas. Puede realizar selecciones para personalizar el contenido de AD y obtener una vista previa del aspecto que tendrá su anuncio en función de las opciones (se actualiza automáticamente a medida que realiza selecciones).
        * En el menú desplegable **idioma** , seleccione el idioma para el anuncio. El texto de la notificación de Microsoft Store aparecerá en el idioma que seleccione.
        * Para agregar una línea de texto adicional al anuncio, escriba el texto en el campo **subtítulo personalizado** .
            > [!NOTE]
            > El texto que escriba aquí se debe localizar en el idioma seleccionado. El lema personalizado se rechazará si el texto no cumple las [Directivas de Bing Ads](https://advertise.bingads.microsoft.com/bing-ads-policies). Consulta esta página para obtener instrucciones sobre el estilo y sobre el contenido no permitido.
        * Para personalizar aún más el anuncio, expande **Personalizar el diseño del anuncio/ver los tamaños de los anuncios** y elige una de estas opciones:
            * **Color de fondo**. Elige entre las opciones disponibles.
            * **Imágenes**. Elija una de las imágenes disponibles (tomadas de la descripción de la tienda de la aplicación).
            * **Mostrar clasificación de mi aplicación**. Selecciona esta casilla si quieres mostrar la clasificación de la aplicación.
            * **Mostrar que mi aplicación es gratuita**. Si la aplicación está disponible en todos los mercados seleccionados, tiene la opción de seleccionar esta casilla.
            * **Llamada a la acción**. Si seleccionó **aumentar interacción en la aplicación** como objetivo de la campaña, puede establecer el botón llamar a acción de AD para **abrir**, **reproducir**, **leer**, **escuchar**o **comprar**.  

    * **Personalizado**. Elige esta opción para usar tus propios diseños de anuncio. Tenga en cuenta que si ha seleccionado anteriormente un segmento de cliente, debe usar creatividad personalizadas. Puedes cargar distintos archivos para cada uno de los tamaños de anuncio disponibles. Los archivos deben cumplir los siguientes requisitos y directrices:
        * Cada archivo debe ser un .png o .jpg de 40 KB o menos.
        * Los diseños de tus anuncios deben cumplir los requisitos especificados en la [Microsoft Creative Acceptance Policy](https://about.ads.microsoft.com/solutions/ad-products/display-advertising/creative-acceptance-policies).
        * El contenido de los diseños de anuncio debe ser pertinente para la aplicación que promueves. Los diseños de anuncios que no están relacionados con la aplicación no se distribuirán a los anuncios de otras aplicaciones.
        * Todo el contenido de los diseños de anuncio debe ser legible. Por ejemplo, el contenido no debe aparecer borroso, pixelado ni estirado.

12.  Si tienes una [cuenta premium](common-questions.md#how-can-i-increase-the-maximum-monthly-budget-amount-allowed-for-my-ad-campaign), puedes usar la casilla **Dirección URL de destino** para controlar lo que sucede cuando un cliente hace clic en el anuncio.
    * Si dejas la casilla vacía, cuando un cliente haga clic en el anuncio, se mostrará la descripción de la Tienda de la aplicación.
    * Si usa ADJUST, Kochava, TUNE o Vungle para medir el análisis de instalación de la aplicación, escriba la dirección URL de seguimiento de instalación. Al guardar la campaña, la dirección URL de seguimiento se valida para asegurarse de que se resuelve en la página de lista de la aplicación en el Microsoft Store. Para obtener más información sobre la instalación del seguimiento con estos servicios, consulte la documentación sobre el [ajuste](https://docs.adjust.com/en/), la [Kochava](https://support.kochava.com/), la [optimización](https://help.tune.com/hasoffers/)y la [Vungle](https://support.vungle.com/hc/en-us) .
    * Si seleccionó **aumentar interacción en la aplicación** como objetivo de la campaña, puede especificar un [URI de vínculo profundo](../launch-resume/handle-uri-activation.md) para redirigir a los clientes del segmento seleccionado a una página específica de la aplicación.
    * Si especificas cualquier destino que no sea la página de descripción de la aplicación o una página dentro de la aplicación, la campaña se pondrá en pausa automáticamente.

13.  Por último, haga clic en **revisar** para confirmar la configuración de la campaña de AD y, si se trata de una campaña de anuncios de pago, su presupuesto y su información de pago. Haz clic en **Confirmar** y tus anuncios empezarán a aparecer en dispositivos en pocas horas.

## <a name="review-ad-campaign-performance"></a>Revisar el rendimiento de la campaña de ad

Para ver el rendimiento de las campañas, vuelva a la página de **campañas de ad** . Selecciona **Filtros de sección** para definir el ámbito de lo que se incluye en el informe por **Fecha**, **Objetivo de la campaña**, **Nombre de la aplicación**, **Tipo de campaña** o **Estado**. Además de ver información sobre las **Impresiones**, **Clics**, **Conversiones** y **Gasto** de la campaña, puedes usar el informe para **Pausar** o **Reanudar** una campaña. Para obtener más información, consulte [Informe de campaña de ad](/windows/uwp/publish/ad-campaign-report).

Para editar una campaña, selecciona su nombre en la lista.