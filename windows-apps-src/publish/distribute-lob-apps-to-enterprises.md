---
description: Puede publicar aplicaciones de línea de negocio (LOB) directamente en empresas para la adquisición de volúmenes a través del Microsoft Store para empresas o Microsoft Store para educación, sin que las aplicaciones estén disponibles en la tienda.
title: Distribuir aplicaciones de línea de negocio a empresas
ms.assetid: 2050126E-CE49-4DE3-AC2B-A572AC895158
ms.date: 01/16/2020
ms.topic: article
keywords: Windows 10, UWP, LOB, línea de negocio, aplicaciones empresariales, tienda para empresas, tienda para educación, Enterprise
ms.localizationpriority: medium
ms.openlocfilehash: b9bb384ac8514431899fd6ac37cf56303a80d214
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033928"
---
# <a name="distribute-lob-apps-to-enterprises"></a>Distribuir aplicaciones de línea de negocio a empresas

Tiene varias opciones para distribuir aplicaciones de línea de negocio (LOB) a los usuarios de su organización mediante [paquetes de MSIX](/windows/msix/) sin que las aplicaciones estén disponibles en general para el público. Puede usar las herramientas de administración de dispositivos, configurar una implementación basada en el instalador de la aplicación, transferir localmente las aplicaciones o publicar las aplicaciones en el Microsoft Store para empresas o Microsoft Store para educación.

## <a name="microsoft-endpoint-configuration-manager-and-microsoft-intune"></a>Configuration Manager de punto de conexión de Microsoft y Microsoft Intune

Si su organización usa el punto de conexión de Microsoft Configuration Manager o Microsoft Intune para administrar los dispositivos, puede implementar aplicaciones de LOB con estas herramientas. Para obtener más información, consulte estos artículos:

* [Introducción a la administración de aplicaciones en Configuration Manager](/configmgr/apps/understand/introduction-to-application-management)
* [Información general sobre el ciclo de vida de la aplicación en Microsoft Intune](/intune/apps/app-lifecycle)

## <a name="app-installer"></a>Instalador de aplicación

El instalador de aplicaciones permite instalar aplicaciones de Windows 10 haciendo doble clic en un paquete de aplicación de MSIX directamente o haciendo doble clic en un archivo. AppInstaller que instala el paquete de la aplicación desde un servidor Web. Esto significa que los usuarios no necesitan usar PowerShell u otras herramientas de desarrollo para instalar aplicaciones de LOB. El instalador de la aplicación también puede instalar paquetes de aplicaciones que incluyen paquetes opcionales y conjuntos relacionados.

El Instalador de aplicación se puede descargar para su uso sin conexión en la empresa desde el [portal web](https://businessstore.microsoft.com/store/details/app-installer/9NBLGGH4NNS1) de Microsoft Store para Empresas. Para obtener más información sobre el instalador de aplicaciones, consulte [instalación de aplicaciones de Windows 10 con el instalador de aplicaciones](/windows/msix/app-installer/app-installer-root).

## <a name="sideloading"></a>Instalación de prueba

Otra opción para la distribución de aplicaciones de LOB directamente a los usuarios de su organización es la instalación de prueba. Esta opción es similar a la implementación basada en instalación de aplicaciones, ya que permite a los usuarios instalar paquetes de aplicaciones de MSIX directamente. A partir de la versión 2004 de Windows 10, la instalación de prueba está habilitada de forma predeterminada y los usuarios pueden instalar aplicaciones haciendo doble clic en paquetes de aplicación MSIX firmados. En Windows 10 versión 1909 y versiones anteriores, la instalación de prueba requiere cierta configuración adicional y el uso de un script de PowerShell. Para obtener más información, consulta [Transferir localmente aplicaciones de LOB en Windows 10](/windows/application-management/sideload-apps-in-windows-10).

## <a name="microsoft-store-for-business-or-microsoft-store-for-education"></a>Microsoft Store para empresas o Microsoft Store para educación

Puede publicar aplicaciones de línea de negocio (LOB) directamente en empresas para la adquisición de volúmenes a través de Microsoft Store para empresas o Microsoft Store para educación, sin que las aplicaciones estén disponibles en la tienda. Cuando se usa esta opción, las aplicaciones están firmadas por la tienda y deben cumplir las directivas de almacenamiento estándar.

> [!NOTE]
> En este momento, solo las aplicaciones gratuitas se pueden distribuir exclusivamente a las empresas a través de Microsoft Store para empresas o Microsoft Store para educación. Si envía una aplicación de pago como LOB, no estará disponible para la empresa. 

> [!IMPORTANT]
> No puede usar la [API de envío de Microsoft Store](../monetize/create-and-manage-submissions-using-windows-store-services.md) para publicar aplicaciones de LOB directamente en las empresas. Todos los envíos de aplicaciones de LOB deben publicarse a través del centro de Partners.

### <a name="set-up-the-enterprise-association"></a>Configurar la asociación empresarial

El primer paso para publicar aplicaciones LOB exclusivamente para una empresa es establecer la asociación entre tu cuenta y la tienda privada de la empresa.

> [!IMPORTANT]
> Este proceso de asociación debe ser iniciado por la empresa y debe usar la dirección de correo electrónico asociada con la cuenta de Microsoft que se usó para crear la cuenta de desarrollador. Para obtener más información, consulta el artículo [Working with line-of-business apps (Trabajar con aplicaciones de línea de negocio)](/microsoft-store/working-with-line-of-business-apps).

Si una empresa decide invitarte para que publiques aplicaciones para su uso exclusivo, recibirás un correo electrónico con un vínculo para confirmar la asociación. También puede confirmar estas asociaciones; para ello, vaya a la sección **asociaciones empresariales** de la configuración de la **cuenta** (siempre que haya iniciado sesión con el cuenta de Microsoft que se usó para abrir la cuenta de desarrollador).

Para confirmar la asociación, haz clic en **Aceptar** . De este modo, podrás publicar aplicaciones desde tu cuenta para uso exclusivo de la empresa.

### <a name="submit-lob-apps"></a>Envío de aplicaciones de LOB

Cuando estés listo para publicar una aplicación para uso exclusivo de la empresa, el proceso es similar al proceso de envío de aplicaciones. La aplicación pasa por el mismo [proceso de certificación](the-app-certification-process.md)y debe cumplir todas [las directivas de Microsoft Store](store-policies.md). Solo hay algunas etapas del proceso que son diferentes.

#### <a name="visibility"></a>Visibilidad

Después de configurar una asociación empresarial, cada vez que envíe una aplicación verá un cuadro desplegable en la sección **visibilidad** de la página de **precios y disponibilidad** del envío. Esta opción está establecida en **Retail distribution** de forma predeterminada. Para que la aplicación sea exclusiva de una empresa, tienes que elegir **Line-of-business (LOB) distribution** .

Una vez seleccionada la **distribución de línea de negocio (LOB)** , las opciones de **visibilidad** habituales se reemplazarán por una lista de las empresas en las que puede publicar aplicaciones exclusivas. Nadie fuera de las empresas que selecciones podrá ver o descargar la aplicación de la.

Debe seleccionar al menos una empresa para publicar una aplicación como línea de negocio.

<span id="organizational" />

#### <a name="organizational-licensing"></a>Licencias organizativas

De forma predeterminada, la casilla **licencias por volumen (en línea) administradas por la Tienda** se activa al enviar una aplicación. Al publicar aplicaciones LOB, este cuadro debe permanecer activado para que la empresa pueda adquirir la aplicación en el volumen. Esto no hará que la aplicación esté disponible para ningún usuario ajeno a las empresas seleccionadas en la sección **distribución y visibilidad** .

Si quieres que la aplicación esté disponible para la empresa a través de licencias sin conexión, puedes activar la casilla **Licencias desconectadas (sin conexión)** también.

Para obtener más información, consulta [Opciones de licencias organizativas](organizational-licensing.md).

#### <a name="age-ratings"></a>Clasificaciones por edades

Para las aplicaciones de LOB, el paso [clasificación por edades](age-ratings.md) del proceso de envío funciona igual que para aplicaciones de comercios minoristas, pero también tiene una opción adicional que permite indicar la clasificación por edades de la Tienda de la aplicación manualmente, en lugar de completar el cuestionario o importando un id. de clasificación de IARC existente. Esta clasificación manual solo se puede usar con la distribución de LOB, por lo que si alguna vez cambia la configuración de **visibilidad** de la aplicación a la **distribución comercial** , deberá tomar el cuestionario de clasificación por edades para poder publicar el envío.

### <a name="enterprise-deployment-of-lob-apps"></a>Implementación de aplicaciones de LOB para empresas

Después de hacer clic **Enviar a la Tienda** , la aplicación pasará por el proceso de certificación. Una vez que esté listo, el administrador de la empresa debe agregarlo a su almacén privado en el portal de Microsoft Store para empresas o Microsoft Store para educación. Luego la empresa puede implementar la aplicación para sus usuarios.

> [!NOTE]
> Para obtener la aplicación de LOB, la organización debe encontrarse en un [mercado compatible](/windows/whats-new/windows-store-for-business-overview#supported-markets)y no debe haber [excluido dicho mercado](./define-market-selection.md) al enviar la aplicación. 

Para obtener más información, consulta [Trabajar con aplicaciones de línea de negocio](/microsoft-store/working-with-line-of-business-apps) y [Distribuir aplicaciones a través de la tienda privada](/microsoft-store/distribute-apps-from-your-private-store).

### <a name="update-lob-apps"></a>Actualizar aplicaciones LOB

Para publicar actualizaciones de una aplicación que ya publicaste como LOB, simplemente crea un nuevo envío. Puedes cargar nuevos paquetes o hacer cambios y luego solo tienes que hacer clic en **Enviar a la Tienda** para que la versión actualizada esté disponible. Asegúrese de mantener la **visibilidad** de las selecciones empresariales en la misma, a menos que desee realizar cambios como, por ejemplo, seleccionar una empresa adicional para adquirir la aplicación o quitar una de las empresas en las que lo distribuyó previamente.

Si quieres dejar de ofrecer una aplicación que ya publicaste como línea de negocio y evitar nuevas compras, tienes que crear un nuevo envío. En primer lugar, deberá cambiar la selección de **visibilidad** de la **distribución de línea de negocio (LOB)** a **distribución comercial** . Después, en la sección [detectabilidad](choose-visibility-options.md#discoverability) , elija **hacer que este producto esté disponible pero no se pueda detectar en el almacén** con la opción **detener adquisición** .

Después de que el envío pase por el proceso de certificación, la aplicación ya no estará disponible para nuevas adquisiciones (aunque quienes ya la tienen podrán seguir usándola).

> [!NOTE]
> Al cambiar una aplicación a una **distribución comercial** , deberá completar el cuestionario de [clasificación por edades](age-ratings.md) si aún no lo ha hecho, incluso si la aplicación no estará disponible para nuevas adquisiciones.
