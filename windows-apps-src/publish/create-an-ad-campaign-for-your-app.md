---
author: jnHs
Description: "Puedes crear una campaña publicitaria con el panel del Centro de desarrollo para ayudar a promover tu aplicación y cultivar tu base de usuarios."
title: "Crear una campaña publicitaria para tu aplicación"
ms.assetid: 10D94929-92C4-4379-AA5F-6FEF879F2463
translationtype: Human Translation
ms.sourcegitcommit: 65b82f422e602515e9531664e35f1e1c1e9f5932
ms.openlocfilehash: 3ea67f9e4f0d834bd77ef116c5e0b16008f4ae5f

---

# <a name="create-an-ad-campaign-for-your-app"></a>Crear una campaña publicitaria para tu aplicación


Puedes crear una campaña publicitaria con el panel del Centro de desarrollo para ayudar a promover tu aplicación y cultivar tu base de usuarios. De manera predeterminada, elegimos el público de destino para tus anuncios en función de la configuración de tu aplicación en el panel del Centro de desarrollo, pero puedes definir tu propio público de manera opcional. También puedes usar un conjunto predeterminado de plantillas de anuncios o cargar tus propios diseños de anuncio. Para obtener más información sobre las campañas publicitarias, consulta [Preguntas comunes sobre las campañas publicitarias](common-questions.md).

> **Nota**  Puedes crear campañas de anuncios solo para las aplicaciones que hayan pasado a la fase final de publicación del [proceso de certificación de aplicaciones](the-app-certification-process.md).

Aquí te mostramos cómo crear una campaña publicitaria con el fin de promocionar tu aplicación.

1.  En el menú de navegación izquierdo de la página de la aplicación, en el panel del Centro de desarrollo, haz clic en **Monetización** &gt; **Promoción de la aplicación**.
2.  Realiza una de las acciones siguientes:

    -   Si aún no creaste ninguna campaña publicitaria para esta aplicación, la página **Promocionar la aplicación** mostrará información sobre las ventajas de las campañas publicitarias. Haz clic en **Introducción** o **Crear campañas publicitarias**.
    -   Si ya creaste una campaña publicitaria para esta aplicación, la página **Promocionar la aplicación** mostrará una lista de tus campañas publicitarias existentes. Haz clic en **Nueva campaña**.
3.  En la página **Nueva campaña**, en la sección **Campaign objective**, elige una de estas acciones:
    -   **Incrementar las instalaciones de la aplicación**. Selecciona esta opción si la campaña publicitaria está destinada a conseguir que haya personas que instalen tu aplicación.
    -   **Incrementar el compromiso con la aplicación**. Selecciona esta opción si la campaña publicitaria pretende conseguir que tus clientes aumenten el uso que hacen de la aplicación. Si seleccionas esta opción, puedes dirigir la campaña publicitaria a [segmentos de clientes](create-customer-segments.md) específicos que definas.

4.  En la sección **Detalles de campaña**, define la configuración global de tu campaña.
    -   Asigna un nombre a la campaña publicitaria en el campo **Nombre de la campaña**.
    -   En **Tipo de campaña**, elige una de estas opciones:
        -   **De pago**: estos anuncios se ejecutarán en cualquier aplicación que coincida con el dispositivo y la categoría de la aplicación.
        -   **De la comunidad (gratuita)**: esta publicidad se ejecutará en aplicaciones publicadas por otros desarrolladores que también crean campañas de anuncios de la comunidad. Para poder seleccionar esta opción, primero debes activar la casilla **Mostrar anuncios de la comunidad en mi aplicación** en la página del panel **Monetizar con anuncios**. Para obtener más información, consulta [Acerca de la publicidad de la comunidad](about-community-ads.md).
        -   **Interna (gratuita)**: esta publicidad solo se ejecutará en tus aplicaciones (que coincidan con el dispositivo de la aplicación anunciado). La publicidad interna es gratuita. Para obtener más información, consulta [Acerca de los anuncios internos](about-house-ads.md).
    -   En **Campaign duration**, elige una de estas opciones:
        - **Custom**. Si eliges esta opción, el presupuesto de tu campaña se gastará durante el intervalo de fecha y hora que especifiques. Esta opción solo está disponible para los desarrolladores que tengan una cuenta premium. Para obtener información sobre las cuentas premium, consulta [Preguntas comunes sobre las campañas publicitarias](common-questions.md#how-can-i-increase-the-maximum-monthly-budget-amount-allowed-for-my-ad-campaign).
        - **Mensualmente**. Si eliges esta opción, el presupuesto de tu campaña se gastará cada mes de forma periódica, hasta que detengas la campaña.

    > **Nota:** Si la aplicación no se ha publicado aún, recibirás un mensaje de error en la página **Nueva campaña**. Debes esperar a que tu aplicación se publique antes de poder crear una campaña publicitaria para ella.

5.  Si eliges el objetivo de campaña **Increase app installations**, definiremos los destinatarios de tus anuncios, en función de la configuración que hayas seleccionado al crear la aplicación en el panel del Centro de desarrollo. Si prefieres elegir tú mismo el público de tus anuncios, selecciona **Manual** para expandir la sección **Público**. Si quieres volver a los destinatarios predeterminados, selecciona **Automático**.

    Si seleccionas **Manual**, puedes editar la siguiente información de identificación:

    -   Elige los países o las regiones en que quieres que aparezcan estos anuncios. Puedes elegir hasta 5. Para obtener una lista de los países o regiones admitidos, consulta [Preguntas comunes sobre las campañas publicitarias](common-questions.md#where-will-my-ad-appear).
    -   Elige los tipos de dispositivos en los que quieres que aparezcan estos anuncios. Solo se muestran los tipos de dispositivo admitidos por la aplicación.
    -   Elige el sistema operativo. Solo se muestran los sistemas operativos admitidos por la aplicación.
    -   Selecciona el sexo y el intervalo de edad de la audiencia deseada.

    Esta sección también muestra un gráfico de **Alcance estimado**. Este gráfico muestra el público al que puedes llegar con tus selecciones actuales de destinatarios como un porcentaje de todos los usuarios de aplicaciones habilitadas para anuncios de Windows en los mercados seleccionados.

6.  Si has elegido el objetivo de campaña **Increase app engagement**, puedes seleccionar uno de los segmentos de clientes como destino.

    > **Nota:** Los anuncios creados con esta campaña solo se mostrarán a los clientes que estén incluidos en el segmento. Solo se puede seleccionar un segmento por campaña publicitaria. Para información sobre los segmentos, consulta [Crear segmentos de clientes](create-customer-segments.md).


7.  En la sección **Diseño de anuncios**, elige una de estas opciones:
    -   **Personalizado**. Elige esta opción para usar tus propios diseños de anuncio. Ten en cuenta que si has seleccionado un segmento de clientes en el paso 6, debes usar creativos personalizados. Puedes cargar distintos archivos para cada uno de los tamaños de anuncio disponibles. Los archivos deben cumplir los siguientes requisitos y directrices:
        -   Cada archivo debe ser un .png o .jpg de 40 KB o menos.
        -   Los diseños de tus anuncios deben cumplir los requisitos especificados en la [Microsoft Creative Acceptance Policy](http://go.microsoft.com/fwlink?LinkId=532595).
        -   El contenido de los diseños de anuncio debe ser pertinente para la aplicación que promueves. Los diseños de anuncios que no están relacionados con la aplicación no se distribuirán a los anuncios de otras aplicaciones.
        -   Todo el contenido de los diseños de anuncio debe ser legible. Por ejemplo, el contenido no debe aparecer borroso, pixelado ni estirado.
    -   **Anuncio generado automáticamente**. Elige esta opción para usar anuncios de una lista de plantillas predeterminadas. Tienes las siguientes opciones para personalizar el contenido de los anuncios. A medida que hagas selecciones, las vistas previas de los anuncios se actualizarán automáticamente.
        -   En el menú desplegable **Idioma**, selecciona el idioma de los anuncios. El texto del distintivo de la Tienda Windows y el texto de lema personalizado (si se especifica) se mostrarán en el idioma que selecciones.
        -   Para agregar una línea de texto adicional al anuncio, escribe el texto en el campo **Custom tag line**.
            > **Nota**  El texto que escribas debe estar localizado en el idioma seleccionado. El lema personalizado se rechazará si el texto no cumple las [Directivas de Bing Ads](http://go.microsoft.com/fwlink?LinkId=398341). Consulta esta página para obtener instrucciones sobre el estilo y sobre el contenido no permitido.

        -   Para personalizar aún más el anuncio, expande **Personalizar el diseño del anuncio/ver los tamaños de los anuncios** y elige una de estas opciones:
            - **Color de fondo**. Elige entre las opciones disponibles.
            - **Imágenes**. Las imágenes disponibles son las imágenes que has asociado a la aplicación en la Tienda.
            - **Mostrar clasificación de mi aplicación**. Selecciona esta casilla si quieres mostrar la clasificación de la aplicación.
            - **Mostrar que mi aplicación es gratuita**. Si la aplicación es gratuita en todos los mercados seleccionados, también tendrás la opción de seleccionar esta casilla.
            - **Llamada a la acción**. Si has elegido **Incrementar el compromiso con la aplicación** como objetivo de la campaña, puedes establecer el botón de llamad a la acción de tu anuncio en **Abrir**, **Reproducir**, **Leer**, **Escuchar** o **Comprar**.  

8.  Si tienes una [cuenta premium](common-questions.md#how-can-i-increase-the-maximum-monthly-budget-amount-allowed-for-my-ad-campaign), puedes usar la casilla **Dirección URL de destino** para controlar lo que sucede cuando un cliente hace clic en el anuncio.
    - Si dejas la casilla vacía, cuando un cliente haga clic en el anuncio, se mostrará la descripción de la Tienda de la aplicación.
    - Si usas Kochava o Tune para medir el análisis de instalación de la aplicación, escribe la dirección URL de seguimiento de instalación desde Kochava o Tune. Al guardar la campaña, se valida la dirección URL de seguimiento para garantizar que se resuelve en la página de descripción de la aplicación de la Tienda Windows. Para obtener más información sobre el seguimiento de la instalación con Kochava y Tune, consulta la documentación de [Kochava](http://support.kochava.com/) y [Tune](https://help.tune.com/).
    - Si has elegido **Incrementar el compromiso con la aplicación** como objetivo de la campaña, puedes especificar un [vínculo profundo de URI](../launch-resume/handle-uri-activation.md) para redirigir a los clientes del segmento seleccionado a una página específica dentro de la aplicación.
    - Si especificas cualquier destino que no sea la página de descripción de la aplicación o una página dentro de la aplicación, la campaña se pondrá en pausa automáticamente.

9.  Ahora elige la configuración financiera de la campaña publicitaria en la sección **Presupuesto y pago**.
   > **Nota**  Si vas a crear una campaña interna o de la comunidad, no aparecerá la sección **Presupuesto y pago**, ya que estas campañas son gratis.

    -   En **Presupuesto**, usa el control deslizante para establecer la cantidad de dinero que quieres gastar cada mes para mostrar el anuncio.

        El presupuesto mensual se prorratea el mes en el que se crea la campaña publicitaria. En otras palabras, si creas una campaña publicitaria a mitad de un mes natural, se te cobrará la mitad del presupuesto mensual para ese mes.

    -   Establece un instrumento de pago de la campaña publicitaria haciendo clic en **Agregar nuevo instrumento de pago** y rellena los detalles de la cuenta.
        > **Importante**  El país o la región de la dirección de facturación de tu instrumento de pago debe coincidir con el país o la región asociado con tu cuenta del Centro de desarrollo.
- Si has recibido un cupón de un representante de Microsoft para pagar una campaña publicitaria, haz clic en **Utilizar un cupón**, escribe el código del cupón y haz clic en **Aplicar** para aplicar el cupón a la campaña.

10.  Por último, haz clic en **Revisar** para confirmar la configuración de la campaña publicitaria y, si es una campaña publicitaria de pago, la información de presupuesto y pago. Haz clic en **Confirmar** y tus anuncios empezarán a aparecer en dispositivos en pocas horas.
   > **Sugerencia** Para ver cómo se están comportando las campañas, en el menú de navegación superior del panel, selecciona **Promociones**. Selecciona **Filtros de sección** para definir el ámbito de lo que se incluye en el informe por **Fecha**, **Objetivo de la campaña**, **Nombre de la aplicación**, **Tipo de campaña** o **Estado**. Además de ver información sobre las **Impresiones**, **Clics**, **Conversiones** y **Gasto** de la campaña, puedes usar el informe para **Pausar** o **Reanudar** una campaña. Para editar una campaña, selecciona su nombre en la lista.

## <a name="related-topics"></a>Temas relacionados

* [Administrar campañas publicitarias](managing-your-ad-campaign.md)
* [Acerca de los anuncios internos](about-house-ads.md)
* [Informe de anuncios de instalación de aplicaciones](app-install-ads-reports.md)
* [Preguntas comunes sobre las campañas publicitarias](common-questions.md)
 

 



<!--HONumber=Dec16_HO1-->


