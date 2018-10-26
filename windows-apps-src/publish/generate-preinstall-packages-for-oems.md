---
author: jnHs
Description: If your developer account has been granted the appropriate permissions, you can generate and download preinstall packages so that an OEM can include your app in their OS image.
title: Generar paquetes preinstalados para OEM
ms.assetid: AC3A45E8-7BBD-44E9-B2D3-B74B7C9B2BC9
ms.author: wdg-dev-content
ms.date: 06/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8564d3dc7240bb556f3cb90c51165def9e2d4eba
ms.sourcegitcommit: b7e3d222e229cdbf04e837fcb94fb7d84a93de09
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2018
ms.locfileid: "5597568"
---
# <a name="generate-preinstall-packages-for-oems"></a>Generar paquetes preinstalados para OEM

Si tu cuenta de desarrollador tiene los permisos adecuados, puedes generar y descargar paquetes de preinstalación para que un OEM pueda incluir tu aplicación en su imagen de SO. Los permisos de preinstalación solo están habilitados en cuentas de desarrollador patrocinadas por OEM.


## <a name="important-preinstall-policy--limitations"></a>Directivas y limitaciones de preinstalación importantes

Las aplicaciones de preinstalación deben estar certificadas a través del Centro de desarrollo de Windows con la licencia más reciente de la Tienda para que puedan conectarse a la Tienda y recibir actualizaciones de aplicación.

Las aplicaciones que estén preinstaladas deben ser y permanecer gratuitas en todos los mercados.


## <a name="generating-preinstall-packages"></a>Generar paquetes de preinstalación

Una vez que se habilite una cuenta con permisos de preinstalación, completa los siguientes pasos:

1.  En el panel, ve a la aplicación que se va a preinstalar.
2.  En el menú de navegación izquierdo, expande **Administración de aplicaciones** y selecciona **Paquetes actuales**.
3.  En la sección **Solicitar paquetes para la preinstalación del SO**, selecciona **Habilitar los paquetes descargables**.
4.  En el cuadro de diálogo de confirmación, selecciona **Habilitar**.
5.  Busca el paquete que quieres descargar y selecciona el vínculo **Generar paquete** apropiado.

    > [!NOTE]
    > La hora de generación de los paquetes de preinstalación variará según el tamaño del paquete que hayas seleccionado. Puedes salir de esta página y volver más tarde o puedes dejar la página abierta mientras se genera el paquete.

6.  Después de generar el paquete, aparecerá un vínculo a **Descargar paquete**. Selecciónalo para descargar el archivo .zip.

Después puedes proporcionar el archivo .zip al OEM para que lo incluya en su imagen de SO.


## <a name="support"></a>Compatibilidad

Si tienes más preguntas sobre la generación de paquetes de preinstalación, envía un correo electrónico a <partnerops@microsoft.com>.

 

 




