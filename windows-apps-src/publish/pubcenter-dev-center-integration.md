---
author: jnHs
Description: Se ha integrado Microsoft Advertising con pubCenter en el Centro de desarrollo de Windows.
title: "Integración del Centro de desarrollo y pubCenter"
ms.assetid: C1EB51DF-7850-45F4-B565-FF5A690EBD8D
translationtype: Human Translation
ms.sourcegitcommit: b2aaf232567ea0910d83208ef77df127592afb0c
ms.openlocfilehash: 7f841b27e736206c89bb3ad035f497991a01fc74

---

# Integración del Centro de desarrollo y pubCenter


>**Importante**
-   **A partir del 1 de octubre de 2016, ya no tendrás acceso al panel de pubCenter.** Visita tu [panel del Centro de desarrollo de Windows](https://developer.microsoft.com/dashboard/apps/overview) para la administración de todos los anuncios e informes. Si no has vinculado cualquiera de tus cuentas de pubCenter a una cuenta de Centro de desarrollo, hazlo de inmediato mediante los pasos que se describen en este tema.
-   A partir del 1 de abril de 2016, las ganancias de Microsoft Advertising se abonarán en la cuenta de pago que configuraste en el Centro de desarrollo. Ahora debes administrar las actualizaciones o los cambios de tu cuenta de pago y tu perfil fiscal solo en el Centro de desarrollo. Para obtener más información, consulta [Consolidación del perfil fiscal y la cuenta de pago del Centro de desarrollo y pubCenter](pubcenter-devcenter-payout-account-and-tax-profile-consolidation.md).
-   Para seguir recibiendo los pagos después del 1 de diciembre de 2015, [tu cuenta de pubCenter debe estar vinculada a tu cuenta del Centro de desarrollo](#scenario-3-you-have-dev-center-and-pubcenter-accounts-registered-with-different-microsoft-accounts).
-   El historial de pagos ya no se actualizará en pubCenter. Para revisar el historial de pagos de ahora en adelante, [vincula tu cuenta de pubCenter a tu cuenta del Centro de desarrollo](#scenario-3-you-have-dev-center-and-pubcenter-accounts-registered-with-different-microsoft-accounts) y visita la página [Resumen de pago](payout-summary.md) del panel del Centro de desarrollo.

Se ha integrado Microsoft Advertising con pubCenter en el Centro de desarrollo de Windows.

Puede que tengas varias cuentas en el Centro de desarrollo y pubCenter y tu experiencia de migración dependerá de cómo se hayan configurado estas cuentas. Puede que te encuentres en uno o varios de los siguientes escenarios. Lee los detalles cuidadosamente para aquellos escenarios que te conciernen.

## Escenario n.º 1: Tienes una cuenta del Centro de desarrollo y una cuenta de pubCenter con la misma cuenta de Microsoft, registrada en el mismo país o región


**Ejemplo**: Tienes una cuenta del Centro de desarrollo registrada con \[nombre\]@outlook.com en los Estados Unidos. También tienes una cuenta de pubCenter registrada con \[nombre\]@outlook.com en los Estados Unidos.

**Resultado**: La cuenta de pubCenter se ha vinculado automáticamente a tu cuenta del Centro de desarrollo. Se requiere ninguna acción.

-   Los informes de todas las unidades de anuncios existentes en pubCenter están disponibles en el [Informe de rendimiento de publicidad](advertising-performance-report.md) del Centro de desarrollo, en la aplicación a la que pertenecen.
-   Si no encuentras una unidad de anuncio de pubCenter concreta en tu [Informe de rendimiento de publicidad](advertising-performance-report.md), puede que no hayamos podido vincular esa unidad de anuncio correctamente a una aplicación en el Centro de desarrollo. Esto puede ocurrir si el nombre de la aplicación no es coherente en el Centro de desarrollo y pubCenter. Para ver las unidades de anuncios que no hemos podido vincular correctamente a tus aplicaciones en el Centro de desarrollo, visita el informe **Rendimiento de la publicidad** de nivel de cuenta y selecciona el nombre de la aplicación de pubCenter. Para obtener acceso al informe **Rendimiento de la publicidad** de nivel de cuenta, ve a la página de información general del panel y haz clic en **Rendimiento de la publicidad** en el panel de navegación.
-   Para administrar el método de pago y los detalles de impuestos para tu cuenta de pubCenter, debes usar pubCenter.
-   Los detalles de impuestos y el método de pago usados en el Centro de desarrollo se usarán también para los pagos de pubCenter. Los datos de impuestos y el método de pago solo se pueden editar desde el Centro de desarrollo. Esto significa que:
    -   Si los métodos de pago en el Centro de desarrollo y pubCenter son diferentes, puede ser que empieces a recibir los pagos de pubCenter en una cuenta bancaria diferente.
    -   Si no has configurado el método de pago o los detalles de impuestos en el Centro de desarrollo o pubCenter, configúralos en el Centro de desarrollo y así se usará el mismo método de pago o detalles de impuestos para los pagos de pubCenter. Para obtener más información acerca de cómo administrar los métodos de pago y los detalles de impuestos en el Centro de desarrollo, consulta el tema [Configurar los formularios fiscales y la cuenta de pago](setting-up-your-payout-account-and-tax-forms.md).
    -   Los pagos de pubCenter se realizarán con el método de pago y detalles de impuestos configurados en pubCenter hasta que la información fiscal y el método de pago se configure en el Centro de desarrollo. Si ya están configurados en el Centro de desarrollo, los detalles de impuestos y el método del pago de pubCenter serán reemplazados por los configurados en el Centro de desarrollo. Para más información, consulta [Consolidación del perfil fiscal y la cuenta de pago del Centro de desarrollo y pubCenter](pubcenter-devcenter-payout-account-and-tax-profile-consolidation.md).
-   Los informes de pago ahora están disponibles en tu cuenta del Centro de desarrollo. Para obtener más información sobre los informes de pago en el Centro de desarrollo, consulta el tema [Resumen de pagos](payout-summary.md).
-   Todas las campañas de promoción de la aplicación ahora están asociadas con la cuenta del Centro de desarrollo y puedes administrarlas directamente desde el Centro de desarrollo.

> **Nota**  Ya no podrás crear nuevas unidades de anuncios ni campañas de promoción de la aplicación en pubCenter. Para obtener información sobre cómo crear unidades de anuncios en el Centro de desarrollo, consulta [Rentabilizar con anuncios](monetize-with-ads.md). Para obtener información sobre cómo crear campañas de promoción de la aplicación en el Centro de desarrollo, consulta el tema [Crear una campaña publicitaria para tu aplicación](create-an-ad-campaign-for-your-app.md).

## Escenario n.º2: Tienes una cuenta del Centro de desarrollo y una cuenta de pubCenter con la misma cuenta de Microsoft pero registradas en distintos países o regiones


**Ejemplo**: Tienes una cuenta del Centro de desarrollo registrada con \[nombre\]@outlook.com en los Estados Unidos. También tienes una cuenta de pubCenter registrada con \[nombre\]@outlook.com en la India.

**Resultado**: La cuenta de pubCenter se ha vinculado automáticamente a tu cuenta del Centro de desarrollo. Se requiere ninguna acción.

-   Los informes de todas las unidades de anuncios existentes en pubCenter están disponibles en el [Informe de rendimiento de publicidad](advertising-performance-report.md) del Centro de desarrollo, en la aplicación a la que pertenecen.
-   Si no encuentras una unidad de anuncio de pubCenter concreta en tu [Informe de rendimiento de publicidad](advertising-performance-report.md), puede que no hayamos podido vincular esa unidad de anuncio correctamente a una aplicación en el Centro de desarrollo. Esto puede ocurrir si el nombre de la aplicación no es coherente en el Centro de desarrollo y pubCenter. Para ver las unidades de anuncios que no hemos podido vincular correctamente a tus aplicaciones en el Centro de desarrollo, visita el informe **Rendimiento de la publicidad** de nivel de cuenta y selecciona el nombre de la aplicación de pubCenter. Para obtener acceso al informe **Rendimiento de la publicidad** de nivel de cuenta, ve a la página de información general del panel y haz clic en **Rendimiento de la publicidad** en el panel de navegación.
-   Los datos de ganancias de pubCenter se convertirán a la moneda del Centro de desarrollo y se notificarán al Centro de desarrollo. Sin embargo, los pagos de pubCenter seguirán realizándose en la moneda de la cuenta de pubCenter.
-   Para administrar el método de pago y los detalles de impuestos para tu cuenta de pubCenter, debes usar pubCenter.
-   Para las nuevas unidades de anuncios que crees en el Centro de desarrollo, las ganancias se acumularán, notificarán y abonarán en la moneda de la cuenta del Centro de desarrollo y las ganancias para estas unidades de anuncios nuevas usarán los detalles de impuestos y el método de pago del Centro de desarrollo.
-   Tendrás que administrar el método de pago y los detalles de impuestos para las ganancias de las nuevas unidades de anuncio en el Centro de desarrollo. Para más información acerca de cómo administrar los métodos de pago y los detalles de impuestos en el Centro de desarrollo, consulta [Configurar formularios fiscales y cuenta de pago](setting-up-your-payout-account-and-tax-forms.md) y [Consolidación del perfil fiscal y la cuenta de pago del Centro de desarrollo y pubCenter](pubcenter-devcenter-payout-account-and-tax-profile-consolidation.md).
-   Los informes de pago ahora están disponibles en tu cuenta del Centro de desarrollo. Para obtener más información sobre los informes de pago en el Centro de desarrollo, consulta el tema [Resumen de pagos](payout-summary.md).
-   Todas las campañas de promoción de la aplicación ahora están asociadas con la cuenta del Centro de desarrollo y puedes administrarlas directamente desde el Centro de desarrollo.

> **Nota**  Ya no podrás crear nuevas unidades de anuncios ni campañas de promoción de la aplicación en pubCenter. Para obtener información sobre cómo crear unidades de anuncios en el Centro de desarrollo, consulta [Rentabilizar con anuncios](monetize-with-ads.md). Para obtener información sobre cómo crear campañas de promoción de la aplicación en el Centro de desarrollo, consulta el tema [Crear una campaña publicitaria para tu aplicación](create-an-ad-campaign-for-your-app.md).

## Escenario n.º3: Tienes cuentas de pubCenter y el Centro de desarrollo registradas con diferentes cuentas de Microsoft


**Ejemplo**: Tienes una cuenta del Centro de desarrollo registrada con \[nombre\]@outlook.com. También tienes una cuenta de pubCenter registrada con \[otro\_nombre\]@outlook.com.

**Resultado**: Debes vincular manualmente tus cuentas de pubCenter a tu cuenta del Centro de desarrollo. Para ello:

1.  Inicia sesión en la cuenta de pubCenter que quieres vincular.
2.  En la página **Mi información** de esta cuenta, encontrarás un **código de vinculación de cuenta de pubCenter**. Ten este código disponible.

    > **Nota**  Se genera un nuevo código de vinculación de cuenta cada 60 minutos.

3.  Cierra la sesión en tu cuenta de pubCenter.
4.  Inicia sesión en tu [cuenta del Centro de desarrollo](https://dev.windows.com/).
5.  En el panel del Centro de desarrollo, haz clic en **Rendimiento de publicidad** en el panel izquierdo y después en **Vincular una cuenta de pubCenter**.
6.  Escribe la dirección de correo electrónico asociada a la cuenta de pubCenter y el código de vinculación de cuenta.
7.  Haz clic en **Vincular cuenta** y la cuenta de pubCenter se vinculará a tu cuenta del Centro de desarrollo.

    > **Nota**  Puedes vincular una o varias cuentas de pubCenter a tu cuenta del Centro de desarrollo. Sin embargo, cada una de tus cuentas de pubCenter pueden vincularse solo a una cuenta del Centro de desarrollo.

Después de vincular la cuenta de pubCenter a una cuenta del Centro de desarrollo:

-   Los datos de tu cuenta de pubCenter aparecerán en la cuenta del Centro de desarrollo vinculada en un plazo de 24 horas.
-   Si no encuentras una unidad de anuncio de pubCenter concreta en tu [Informe de rendimiento de publicidad](advertising-performance-report.md), puede que no hayamos podido vincular esa unidad de anuncio correctamente a una aplicación en el Centro de desarrollo. Esto puede ocurrir si el nombre de la aplicación no es coherente en el Centro de desarrollo y pubCenter. Para ver las unidades de anuncios que no hemos podido vincular correctamente a tus aplicaciones en el Centro de desarrollo, visita el informe **Rendimiento de la publicidad** de nivel de cuenta y selecciona el nombre de la aplicación de pubCenter. Para obtener acceso al informe **Rendimiento de la publicidad** de nivel de cuenta, ve a la página de información general del panel y haz clic en **Rendimiento de la publicidad** en el panel de navegación.
-   Si la cuenta de pubCenter se registró en un país o región diferente de la cuenta del Centro de desarrollo, los datos sobre las ganancias se convertirán a la moneda del Centro de desarrollo en los informes.
-   Las ganancias de la cuenta de pubCenter ya no se pagarán mediante el método de pago y los detalles de impuestos configurados en pubCenter. Si necesitas realizar cambios en los detalles de impuestos o el método de pago, tendrás que hacerlo en el Centro de desarrollo. Consulta [Consolidación del perfil fiscal y la cuenta de pago del Centro de desarrollo y pubCenter](pubcenter-devcenter-payout-account-and-tax-profile-consolidation.md) para más información.
-   Para recibir pagos de las ganancias acumuladas de las nuevas unidades de anuncios creadas en el Centro de desarrollo, debes configurar tus instrumentos de pago e impuestos en el Centro de desarrollo. Consulta [Consolidación del perfil fiscal y la cuenta de pago del Centro de desarrollo y pubCenter](pubcenter-devcenter-payout-account-and-tax-profile-consolidation.md) para más información.
-   Las ganancias de cualquier unidad de anuncio nueva que crees en el Centro de desarrollo se pagará mediante el método de pago y detalles de impuestos del Centro de desarrollo. Para obtener más información acerca de cómo administrar los métodos de pago y los detalles de impuestos en el Centro de desarrollo, consulta el tema [Configurar los formularios fiscales y la cuenta de pago](setting-up-your-payout-account-and-tax-forms.md).
-   Los informes de pago de pubCenter ahora están disponibles en tu cuenta del Centro de desarrollo. Para obtener más información sobre los informes de pago en el Centro de desarrollo, consulta el tema [Resumen de pagos](payout-summary.md).
-   Todas las campañas de promoción de la aplicación definidas en pubCenter se pausarán automáticamente. De ahora en adelante, crea las campañas nuevas en el Centro de desarrollo.

> **Nota**  Ya no podrás crear nuevas unidades de anuncios ni campañas de promoción de la aplicación en pubCenter. Para obtener información sobre cómo crear unidades de anuncios en el Centro de desarrollo, consulta [Rentabilizar con anuncios](monetize-with-ads.md). Para obtener información sobre cómo crear campañas de promoción de la aplicación en el Centro de desarrollo, consulta el tema [Crear una campaña publicitaria para tu aplicación](create-an-ad-campaign-for-your-app.md).

## Notas y recomendaciones adicionales


-   Asegúrate de vincular todas las cuentas de pubCenter (con cuentas de Microsoft no coincidentes) a la cuenta del Centro de desarrollo en la que quieres administrarlas.
-   No te registres una nueva cuenta en pubCenter, a menos que quieras monetizar aplicaciones de Xbox. Todas tus necesidades de rentabilidad y promoción de la aplicación pueden administrarse ahora en tu cuenta del Centro de desarrollo.
-   Ya no podrás crear nuevas unidades de anuncios en pubCenter. De ahora en adelante, crea las nuevas unidades de anuncios en el Centro de desarrollo. Si deseas editar las unidades de anuncios existentes, ponte en contacto con el soporte técnico.
-   Puede que se haya creado automáticamente una nueva cuenta de pubCenter, con la misma cuenta de Microsoft, para vincular a tu cuenta del Centro de desarrollo con la misma cuenta de Microsoft. Usa la ventana desplegable de nuevas cuentas en la esquina superior derecha de las páginas de pubCenter para ver y administrar la información de tus diferentes cuentas de pubCenter.
-   Para recibir pagos de las ganancias acumuladas de las nuevas unidades de anuncios creadas en el Centro de desarrollo, debes configurar tus instrumentos de pago e impuestos en el Centro de desarrollo. Para obtener más información, consulta [Consolidación del perfil fiscal y la cuenta de pago del Centro de desarrollo y pubCenter](pubcenter-devcenter-payout-account-and-tax-profile-consolidation.md).

## Preguntas más frecuentes


**¿Por qué no he recibido el pago de Microsoft Advertising este mes?**

Esto suele deberse a uno o varios de los siguientes motivos:

-   Tus [detalles de perfil fiscal y cuenta de pago](setting-up-your-payout-account-and-tax-forms.md) no están configurados correctamente en el Centro de desarrollo.
-   Tu cuenta de pubCenter no está vinculada a tu cuenta del Centro de desarrollo.

Para obtener más información, consulta [Consolidación del perfil fiscal y la cuenta de pago del Centro de desarrollo y pubCenter](pubcenter-devcenter-payout-account-and-tax-profile-consolidation.md)
**No puedo crear nuevas unidades de anuncios en pubCenter. ¿Qué sucede?**

Ya no se admite la creación de nuevas unidades de anuncios en pubCenter. Si has integrado el control de Microsoft Ad Mediator en la aplicación, se creará automáticamente una unidad de anuncio en el back-end al cargar la aplicación en la Tienda Windows. Como alternativa, también puedes crear manualmente una nueva unidad de anuncio independiente en el Centro de desarrollo de la página **Monetizar con anuncios** de tu aplicación.

**¿Cómo puedo crear unidades de anuncios en el Centro de desarrollo?**

En la página de la aplicación en el Centro de desarrollo, haz clic en **Monetización** &gt; **Monetizar con anuncios**. Para obtener más información sobre la creación de unidades de anuncios en el Centro de desarrollo, consulta [Rentabilizar con anuncios](monetize-with-ads.md).

**He instalado la versión más reciente de la extensión del mediador de anuncios y he agregado el control de mediador de anuncios con mi aplicación. ¿Necesito ir a pubCenter también y crear una nueva unidad de anuncio?**

Si estás usando la versión más reciente de la [extensión de mediador de anuncios](http://go.microsoft.com/fwlink/p/?LinkId=518026), se crea automáticamente una nueva unidad de anuncio al cargar la aplicación en la Tienda Windows. Ya no es necesario que vayas a pubCenter y crees manualmente una nueva unidad de anuncio. Si se produce un error en la creación de la unidad de anuncio por algún motivo, recibirás un correo electrónico con los detalles de los pasos recomendados a continuación.

**Sigo usando una versión anterior del control de Ad Mediator en mi aplicación y necesito obtener un nuevo identificador de la unidad de anuncio. ¿Cómo puedo crear unidades de anuncios?**

Puedes crear una nueva unidad de anuncio para la aplicación en el panel del Centro de desarrollo. En la página de la aplicación en el Centro de desarrollo, haz clic en **Monetización** &gt; **Monetizar con anuncios**. Para obtener más información sobre la creación de unidades de anuncios en el Centro de desarrollo, consulta [Rentabilizar con anuncios](monetize-with-ads.md).

**Mis unidades de anuncios se han creado automáticamente porque he usado el control de Ad Mediator y he confirmado que se sirvan anuncios. También he creado manualmente algunas otras unidades de anuncios desde el Centro de desarrollo. ¿Tengo que hacer algo más para recibir el pago de las ganancias por estas unidades de anuncios?**

Iniciar sesión en pubCenter con la misma cuenta Microsoft que usas para el Centro de desarrollo y garantizar que los instrumentos de pago e impuestos están configurados correctamente. Recibirás un pago por estas nuevas unidades de anuncios durante la primera semana del mes si las ganancias acumuladas cruzan el límite de umbral.

**No puedo crear nuevas campañas de promoción de aplicaciones en pubCenter. Algunas de mis campañas de promoción de aplicaciones también se han pausado automáticamente. ¿Se espera esto?**

Solo podrás crear campañas de promoción de aplicaciones en el Centro de desarrollo de ahora en adelante. Puede que algunas de tus campañas de promoción de aplicaciones en pubCenter se hayan pausado como parte de esta integración. De ahora en adelante, crea las nuevas campañas de promoción de aplicaciones en el Centro de desarrollo.

**¿Cómo creo campañas de promoción de aplicaciones en el Centro de desarrollo?**

En la página de la aplicación del Centro de desarrollo, haz clic en **Monetización** &gt; **Promocionar la aplicación**. Para obtener más información sobre la creación de campañas de anuncios para tus aplicaciones en el Centro de desarrollo, consulta el tema [Crear una campaña publicitaria para tu aplicación](create-an-ad-campaign-for-your-app.md).

**¿Dónde puedo ver los datos de rendimiento en el Centro de desarrollo?**

En la página de la aplicación del Centro de desarrollo, haz clic en **Análisis** &gt; **Rendimiento de la publicidad de Microsoft**. Para obtener más información, consulta [Informe de rendimiento de la publicidad](advertising-performance-report.md).

**¿Cómo administro mi método de pago y los detalles de impuestos en el Centro de desarrollo?**

Consulta [Configurar formularios fiscales y cuenta de pago](setting-up-your-payout-account-and-tax-forms.md).

**¿Dónde puedo encontrar informes de pago en el Centro de desarrollo?**

Consulta [Resumen de pago](payout-summary.md).

**He vinculado mi cuenta de pubCenter a mi cuenta del Centro de desarrollo, pero no he podido encontrar unidades de anuncios en la aplicación en la que las usé. ¿Dónde puedo encontrarlas?**

Si no encuentras una unidad de anuncio de pubCenter concreta en tu [Informe de rendimiento de publicidad](advertising-performance-report.md), puede que no hayamos podido vincular esa unidad de anuncio correctamente a una aplicación en el Centro de desarrollo. Esto puede suceder si has usado esa unidad de anuncio en varias aplicaciones, si has usado esa unidad de anuncio en aplicaciones que no pertenecen a la misma cuenta del Centro de desarrollo o si la unidad de anuncio no ha servido anuncios en los últimos días. Para ver las unidades de anuncios que no hemos podido vincular correctamente a tus aplicaciones en el Centro de desarrollo, visita el informe **Rendimiento de la publicidad** de nivel de cuenta y selecciona el nombre de la aplicación de pubCenter. Para obtener acceso al informe **Rendimiento de la publicidad** de nivel de cuenta, ve a la página de información general del panel y haz clic en **Rendimiento de la publicidad** en el panel de navegación.

**Solía ver mis unidades de anuncios en una aplicación de mi cuenta del Centro de desarrollo, pero ahora no las puedo encontrar. ¿Qué sucede?**

Esto puede suceder si no se sirven anuncios en tus unidades de anuncios durante unos días. Ve al informe **Rendimiento de la publicidad** de nivel de cuenta en el Centro de desarrollo y busca los datos para estas unidades de anuncios.

**¿Cómo puedo saber si mi cuenta de pubCenter se ha vinculado correctamente a la cuenta del Centro de desarrollo?**

Si tienes la misma cuenta Microsoft asociada a tus cuentas de Centro de desarrollo y pubCenter, las cuentas se vincularán automáticamente y empezarás a ver los informes de ganancias de pubCenter en tu cuenta del Centro de desarrollo. Si sus cuentas del Centro de desarrollo y pubCenter estaban asociadas a diferentes cuentas Microsoft, empezarás a ver los informes de ganancias de la cuenta de pubCenter en el Centro de desarrollo 24 horas después de vincular tu cuenta de pubCenter a tu cuenta del Centro de desarrollo.

**¿Cuándo debo completar la vinculación de mis cuentas de pubCenter a mi cuenta del Centro de desarrollo?**

Recomendamos que te asegures de que todas tus cuentas de pubCenter están vinculadas a tu cuenta del Centro de desarrollo lo antes posible.

**Veo un menú desplegable de nuevas cuentas en mi panel de pubCenter. ¿Qué es?**

Como parte de la integración del Centro de desarrollo y pubCenter, puede que se haya creado automáticamente una nueva cuenta de pubCenter en el back-end para vincularla a tu cuenta del Centro de desarrollo. En este menú desplegable de las cuentas de pubCenter, puedes elegir la cuenta para la que quieres revisar los datos.

**Veo una ligera diferencia entre los datos que se notifican en el Centro de desarrollo y en pubCenter. ¿Se espera esto?**

Sí, podría haber una ligera diferencia en los datos mostrados en los informes de pubCenter y el Centro de desarrollo. Esto puede deberse a que en pubCenter se informa de los datos en la zona horaria de la cuenta de pubCenter y los datos del Centro de desarrollo son en hora UTC.

 

 



<!--HONumber=Sep16_HO3-->


