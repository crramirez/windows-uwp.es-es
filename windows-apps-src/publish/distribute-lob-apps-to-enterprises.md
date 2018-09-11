---
author: jnHs
Description: You can publish line-of-business (LOB) apps directly to enterprises for volume acquisition via the Microsoft Store for Business or Microsoft Store for Education, without making the apps broadly available in the Store.
title: Distribuir aplicaciones de LOB a empresas
ms.assetid: 2050126E-CE49-4DE3-AC2B-A572AC895158
ms.author: wdg-dev-content
ms.date: 03/28/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, lob, línea de negocio, aplicaciones para empresas, store para empresas, store para educación, empresa
ms.localizationpriority: medium
ms.openlocfilehash: 9149533a12263e105356a1683257c4d9172eefb5
ms.sourcegitcommit: 72710baeee8c898b5ab77ceb66d884eaa9db4cb8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2018
ms.locfileid: "3846449"
---
# <a name="distribute-lob-apps-to-enterprises"></a>Distribuir aplicaciones de LOB a empresas


Puedes publicar aplicaciones de línea de negocio (LOB) directamente para que las empresas las compren por volumen a través de la Tienda Microsoft para Empresas o la Tienda Microsoft para Educación, sin necesidad de que las aplicaciones estén disponibles en la Tienda de forma general.

> [!NOTE]
> Por ahora, solo las aplicaciones gratuitas pueden distribuirse de forma exclusiva a las empresas a través de la Tienda Microsoft para Empresas o la Tienda Microsoft para Educación. Si envías una aplicación de pago como LOB, esta no estará disponible para la empresa. 

> [!IMPORTANT]
> No puedes usar la [API de envío de Microsoft Store](../monetize/create-and-manage-submissions-using-windows-store-services.md) para publicar aplicaciones de LOB directamente en las empresas. Todos los envíos de aplicaciones de LOB deben realizarse mediante el Panel del Centro de desarrollo de Windows.


## <a name="set-up-the-enterprise-association"></a>Configurar la asociación de empresa

El primer paso para publicar aplicaciones LOB exclusivamente para una empresa es establecer la asociación entre tu cuenta y la tienda privada de la empresa.

> [!IMPORTANT]
> Este proceso de asociación debe comenzarlo la empresa y debe usarse la dirección de correo electrónico asociada a la cuenta de Microsoft que se usó para crear la cuenta de desarrollador. Para obtener más información, consulta el artículo [Trabajar con aplicaciones de línea de negocio](http://go.microsoft.com/fwlink/p/?LinkId=698846).

Si una empresa decide invitarte para que publiques aplicaciones para su uso exclusivo, recibirás un correo electrónico con un vínculo para confirmar la asociación. También puedes confirmar estas asociaciones si te diriges a la sección **Asociaciones de empresa** de la **Configuración de la cuenta** (siempre que hayas iniciado sesión con la cuenta de Microsoft que usaste para abrir la cuenta de desarrollador).

Para confirmar la asociación, haz clic en **Aceptar**. De este modo, podrás publicar aplicaciones desde tu cuenta para uso exclusivo de la empresa.


## <a name="submit-lob-apps"></a>Enviar aplicaciones de LOB

Cuando estés listo para publicar una aplicación para uso exclusivo de la empresa, el proceso es similar al proceso de envío de aplicaciones. La aplicación pasa por el mismo [proceso de certificación](the-app-certification-process.md), y debe cumplir con todas las [directivas de Microsoft Store](https://docs.microsoft.com/legal/windows/agreements/store-policies). Solo hay algunas etapas del proceso que son diferentes.


### <a name="visibility"></a>Visibilidad

Una vez hayas configurado una asociación de empresa, cada vez que envíes una aplicación, verás un cuadro desplegable en la sección **Visibilidad** de la página **Precios y disponibilidad** del envío. Esta opción está establecida en **Retail distribution** de forma predeterminada. Para que la aplicación sea exclusiva de una empresa, tienes que elegir **Line-of-business (LOB) distribution**.

Cuando se selecciona la opción **Line-of-business (LOB) distribution**, las opciones habituales de **Visibilidad** se reemplazarán por una lista de las empresas para las que puedes publicar aplicaciones exclusivas. Nadie fuera de las empresas que selecciones podrá ver o descargar la aplicación.

Debes seleccionar una empresa como mínimo para publicar una aplicación como línea de negocio.

<span id="organizational" />

### <a name="organizational-licensing"></a>Licencias organizativas

De forma predeterminada, la casilla **licencias por volumen (en línea) administradas por la Tienda** se activa al enviar una aplicación. Al publicar aplicaciones de LOB, esta casilla debe permanecer activada para que la empresa pueda comprar tu aplicación por volumen. Esto no significa que la aplicación estará a disposición del público, sino para las empresas específicas que seleccionaste en la sección **Distribución y visibilidad**.

Si quieres que la aplicación esté disponible para la empresa a través de licencias sin conexión, puedes activar la casilla **Licencias desconectadas (sin conexión)** también.

Para obtener más información, consulta [Opciones de licencias organizativas](organizational-licensing.md).


### <a name="age-ratings"></a>Clasificaciones por edades

Para las aplicaciones de LOB, el paso [clasificación por edades](age-ratings.md) del proceso de envío funciona igual que para aplicaciones de comercios minoristas, pero también tiene una opción adicional que permite indicar la clasificación por edades de la Tienda de la aplicación manualmente, en lugar de completar el cuestionario o importando un id. de clasificación de IARC existente. Esta clasificación manual solo puede usarse con la distribución de LOB; por lo tanto, si cambias el valor **Visibilidad** de la aplicación a **Retail distribution**, tendrás que completar el cuestionario de clasificación por edades para poder publicar el envío.


## <a name="enterprise-deployment-of-lob-apps"></a>Implementación de aplicaciones de LOB para empresas

Después de hacer clic **Enviar a la Tienda**, la aplicación pasará por el proceso de certificación. Cuando esté lista, un administrador de la empresa debe agregarla a su tienda privada en el portal de la Tienda Microsoft para Empresas o la Tienda Microsoft para Educación. Luego la empresa puede implementar la aplicación para sus usuarios.

> [!NOTE]
> Para obtener la aplicación de línea de negocio, la organización debe encontrarse en un [mercado admitido](https://technet.microsoft.com/itpro/windows/whats-new/windows-store-for-business-overview#supported-markets), y no debes haber [excluido ese mercado](define-pricing-and-market-selection.md) al enviar la aplicación. 

Para obtener más información, consulta [Trabajar con aplicaciones de línea de negocio](http://go.microsoft.com/fwlink/p/?LinkId=698846) y [Distribuir aplicaciones a través de la tienda privada](http://go.microsoft.com/fwlink/p/?LinkId=698847).


## <a name="update-lob-apps"></a>Actualizar aplicaciones de LOB

Para publicar actualizaciones de una aplicación que ya publicaste como LOB, simplemente crea un nuevo envío. Puedes cargar nuevos paquetes o hacer cambios y luego solo tienes que hacer clic en **Enviar a la Tienda** para que la versión actualizada esté disponible. Asegúrate de que las selecciones de empresas en **Visibilidad** sean las mismas, a menos que intencionadamente quieras cambiarlas; por ejemplo, seleccionar una empresa adicional para que compre la aplicación o eliminar una de las empresas a la que habías distribuido la aplicación anteriormente.

Si quieres dejar de ofrecer una aplicación que ya publicaste como línea de negocio y evitar nuevas compras, tienes que crear un nuevo envío. En primer lugar, cambia la selección en **Visibilidad** de **Line-of-business (LOB) distribution** a **Retail distribution**. Luego, en la sección [Visibilidad](choose-visibility-options.md#discoverability), elige **Hacer este producto disponible, pero no descubrible, en Store** con la opción **Detener la compra**.

Después de que el envío pase por el proceso de certificación, la aplicación ya no estará disponible para nuevas adquisiciones (aunque quienes ya la tienen podrán seguir usándola).

> [!NOTE]
> Si cambias una aplicación a **Retail distribution**, tienes que completar el [cuestionario de clasificación por edad](age-ratings.md) si aún no lo has hecho, aunque la aplicación no estará disponible para nuevas adquisiciones.


## <a name="distribute-lob-apps-through-sideloading"></a>Distribuir aplicaciones de LOB a través de la instalación de prueba

La creación de aplicaciones para una empresa a través de la Tienda Microsoft para Empresas o la Tienda Microsoft para Educación garantiza que la aplicación esté firmada por la Tienda y que cumpla con las directivas estándar de esta.

En algunos casos, puede que las empresas no quieran que sus aplicaciones de LOB se envíen a través del Centro de desarrollo de Windows (por ejemplo, por cuestiones de cumplimiento o debido a aplicaciones que necesitan funciones adicionales). En este caso, la empresa puede implementar aplicaciones directamente en las máquinas a través de la instalación de prueba, sin necesidad de usar la Tienda Microsoft para Empresas o la Tienda Microsoft para Educación.

Para obtener más información, consulta [Transferir localmente aplicaciones de LOB en Windows10](http://go.microsoft.com/fwlink/p/?LinkId=623433).

 

 




