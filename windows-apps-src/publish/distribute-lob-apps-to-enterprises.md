---
Description: Puedes publicar aplicaciones de línea de negocio (LOB) directamente para que las empresas las compren por volumen a través de la Tienda Microsoft para Empresas o la Tienda Microsoft para Educación, sin necesidad de que las aplicaciones estén disponibles en la Tienda de forma general.
title: Distribuir aplicaciones de LOB a empresas
ms.assetid: 2050126E-CE49-4DE3-AC2B-A572AC895158
ms.date: 01/16/2020
ms.topic: article
keywords: windows 10, uwp, lob, línea de negocio, aplicaciones para empresas, store para empresas, store para educación, empresa
ms.localizationpriority: medium
ms.openlocfilehash: faf750ece274776a147dff9e825f32534eb13014
ms.sourcegitcommit: 7a8aea567b26283c71420e0d305d78f675e1fba7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2020
ms.locfileid: "76125672"
---
# <a name="distribute-lob-apps-to-enterprises"></a>Distribuir aplicaciones de LOB a empresas

You have several options for distributing line of business (LOB) apps to your organization’s users using [MSIX packages](https://docs.microsoft.com/windows/msix/) without making the apps broadly available to the public. You can use device management tools, configure an App Installer-based deployment, sideload the apps directly, or publish the apps to the Microsoft Store for Business or Microsoft Store for Education.

## <a name="microsoft-endpoint-configuration-manager-and-microsoft-intune"></a>Microsoft Endpoint Configuration Manager and Microsoft Intune

If your organization uses Microsoft Endpoint Configuration Manager or Microsoft Intune to manage devices, you can deploy LOB apps using these tools. Para más información, consulte estos artículos:

* [Introduction to application management in Configuration Manager](https://docs.microsoft.com/configmgr/apps/understand/introduction-to-application-management)
* [Overview of the app lifecycle in Microsoft Intune](https://docs.microsoft.com/intune/apps/app-lifecycle)

## <a name="app-installer"></a>Instalador de aplicación

App Installer enables Windows 10 apps to be installed by double-clicking an MSIX app package directly, or by double-clicking an .appinstaller file that installs the app package from a web server. This means that users don't need to use PowerShell or other developer tools to install LOB apps. App Installer can also install app packages that include optional packages and related sets.

El Instalador de aplicación se puede descargar para su uso sin conexión en la empresa desde el [portal web](https://businessstore.microsoft.com/store/details/app-installer/9NBLGGH4NNS1) de Microsoft Store para Empresas. For more information about App Installer, see [Install Windows 10 apps with App Installer](https://docs.microsoft.com/windows/msix/app-installer/app-installer-root).

## <a name="sideloading"></a>Instalación de prueba

Another option for distributing LOB apps directly to users in your organization is sideloading. This option is similar to App Install-based deployment in that it enables users to install MSIX app packages directly. Starting in Windows 10 version 2004, sideloading is enabled by default and users can install apps by double-clicking signed MSIX app packages. On Windows 10 version 1909 and earlier, sideloading requires some additional configuration and the use of a PowerShell script. Para obtener más información, consulta [Transferir localmente aplicaciones de LOB en Windows 10](https://docs.microsoft.com/windows/application-management/sideload-apps-in-windows-10).

## <a name="microsoft-store-for-business-or-microsoft-store-for-education"></a>Microsoft Store for Business or Microsoft Store for Education

Puedes publicar aplicaciones de línea de negocio (LOB) directamente para que las empresas las compren por volumen a través de la Tienda Microsoft para Empresas o la Tienda Microsoft para Educación, sin necesidad de que las aplicaciones estén disponibles en la Tienda de forma general. When using this option, the apps are signed by the Store and must comply with the standard Store Policies.

> [!NOTE]
> Por ahora, solo las aplicaciones gratuitas pueden distribuirse de forma exclusiva a las empresas a través de la Tienda Microsoft para Empresas o la Tienda Microsoft para Educación. Si envías una aplicación de pago como LOB, esta no estará disponible para la empresa. 

> [!IMPORTANT]
> No puedes usar la [API de envío de Microsoft Store](../monetize/create-and-manage-submissions-using-windows-store-services.md) para publicar aplicaciones de LOB directamente en las empresas. All submissions for LOB apps must be published through Partner Center.

### <a name="set-up-the-enterprise-association"></a>Configurar la asociación de empresa

El primer paso para publicar aplicaciones LOB exclusivamente para una empresa es establecer la asociación entre tu cuenta y la tienda privada de la empresa.

> [!IMPORTANT]
> Este proceso de asociación debe comenzarlo la empresa y debe usarse la dirección de correo electrónico asociada a la cuenta de Microsoft que se usó para crear la cuenta de desarrollador. Para obtener más información, consulta el artículo [Trabajar con aplicaciones de línea de negocio](https://docs.microsoft.com/microsoft-store/working-with-line-of-business-apps).

Si una empresa decide invitarte para que publiques aplicaciones para su uso exclusivo, recibirás un correo electrónico con un vínculo para confirmar la asociación. También puedes confirmar estas asociaciones si te diriges a la sección **Asociaciones de empresa** de la **Configuración de la cuenta** (siempre que hayas iniciado sesión con la cuenta de Microsoft que usaste para abrir la cuenta de desarrollador).

Para confirmar la asociación, haz clic en **Aceptar**. De este modo, podrás publicar aplicaciones desde tu cuenta para uso exclusivo de la empresa.

### <a name="submit-lob-apps"></a>Enviar aplicaciones de LOB

Cuando estés listo para publicar una aplicación para uso exclusivo de la empresa, el proceso es similar al proceso de envío de aplicaciones. La aplicación pasa por el mismo [proceso de certificación](the-app-certification-process.md), y debe cumplir con todas las [directivas de Microsoft Store](store-policies.md). Solo hay algunas etapas del proceso que son diferentes.

#### <a name="visibility"></a>Visibilidad

Una vez hayas configurado una asociación de empresa, cada vez que envíes una aplicación, verás un cuadro desplegable en la sección **Visibilidad** de la página **Precios y disponibilidad** del envío. Esta opción está establecida en **Retail distribution** de forma predeterminada. Para que la aplicación sea exclusiva de una empresa, tienes que elegir **Line-of-business (LOB) distribution**.

Cuando se selecciona la opción **Line-of-business (LOB) distribution**, las opciones habituales de **Visibilidad** se reemplazarán por una lista de las empresas para las que puedes publicar aplicaciones exclusivas. Nadie fuera de las empresas que selecciones podrá ver o descargar la aplicación.

Debes seleccionar una empresa como mínimo para publicar una aplicación como línea de negocio.

<span id="organizational" />

#### <a name="organizational-licensing"></a>Licencias organizativas

De forma predeterminada, la casilla **licencias por volumen (en línea) administradas por la Tienda** se activa al enviar una aplicación. Al publicar aplicaciones de LOB, esta casilla debe permanecer activada para que la empresa pueda comprar tu aplicación por volumen. Esto no significa que la aplicación estará a disposición del público, sino para las empresas específicas que seleccionaste en la sección **Distribución y visibilidad**.

Si quieres que la aplicación esté disponible para la empresa a través de licencias sin conexión, puedes activar la casilla **Licencias desconectadas (sin conexión)** también.

Para obtener más información, consulta [Opciones de licencias organizativas](organizational-licensing.md).

#### <a name="age-ratings"></a>Clasificaciones por edades

Para las aplicaciones de LOB, el paso [clasificación por edades](age-ratings.md) del proceso de envío funciona igual que para aplicaciones de comercios minoristas, pero también tiene una opción adicional que permite indicar la clasificación por edades de la Tienda de la aplicación manualmente, en lugar de completar el cuestionario o importando un id. de clasificación de IARC existente. Esta clasificación manual solo puede usarse con la distribución de LOB; por lo tanto, si cambias el valor **Visibilidad** de la aplicación a **Retail distribution**, tendrás que completar el cuestionario de clasificación por edades para poder publicar el envío.

### <a name="enterprise-deployment-of-lob-apps"></a>Implementación de aplicaciones de LOB para empresas

Después de hacer clic **Enviar a la Tienda**, la aplicación pasará por el proceso de certificación. Cuando esté lista, un administrador de la empresa debe agregarla a su tienda privada en el portal de la Tienda Microsoft para Empresas o la Tienda Microsoft para Educación. Luego la empresa puede implementar la aplicación para sus usuarios.

> [!NOTE]
> Para obtener la aplicación de línea de negocio, la organización debe encontrarse en un [mercado admitido](https://docs.microsoft.com/windows/whats-new/windows-store-for-business-overview#supported-markets), y no debes haber [excluido ese mercado](define-pricing-and-market-selection.md) al enviar la aplicación. 

Para obtener más información, consulta [Trabajar con aplicaciones de línea de negocio](https://docs.microsoft.com/microsoft-store/working-with-line-of-business-apps) y [Distribuir aplicaciones a través de la tienda privada](https://docs.microsoft.com/microsoft-store/distribute-apps-from-your-private-store).

### <a name="update-lob-apps"></a>Actualizar aplicaciones de LOB

Para publicar actualizaciones de una aplicación que ya publicaste como LOB, simplemente crea un nuevo envío. Puedes cargar nuevos paquetes o hacer cambios y luego solo tienes que hacer clic en **Enviar a la Tienda** para que la versión actualizada esté disponible. Asegúrate de que las selecciones de empresas en **Visibilidad** sean las mismas, a menos que intencionadamente quieras cambiarlas; por ejemplo, seleccionar una empresa adicional para que compre la aplicación o eliminar una de las empresas a la que habías distribuido la aplicación anteriormente.

Si quieres dejar de ofrecer una aplicación que ya publicaste como línea de negocio y evitar nuevas compras, tienes que crear un nuevo envío. En primer lugar, cambia la selección en **Visibilidad** de **Line-of-business (LOB) distribution** a **Retail distribution**. Luego, en la sección [Visibilidad](choose-visibility-options.md#discoverability), elige **Hacer este producto disponible, pero no descubrible, en Store** con la opción **Detener la compra**.

Después de que el envío pase por el proceso de certificación, la aplicación ya no estará disponible para nuevas adquisiciones (aunque quienes ya la tienen podrán seguir usándola).

> [!NOTE]
> Si cambias una aplicación a **Retail distribution**, tienes que completar el [cuestionario de clasificación por edad](age-ratings.md) si aún no lo has hecho, aunque la aplicación no estará disponible para nuevas adquisiciones.