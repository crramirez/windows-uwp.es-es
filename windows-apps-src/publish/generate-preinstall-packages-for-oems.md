---
Description: Si tu cuenta de desarrollador tiene los permisos adecuados, puedes generar y descargar paquetes de preinstalación para que un OEM pueda incluir tu aplicación en su imagen de SO.
title: Generar paquetes preinstalados para OEM
ms.assetid: AC3A45E8-7BBD-44E9-B2D3-B74B7C9B2BC9
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1ab17adc80a643c04ac7793945486c3ff975fde5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57643140"
---
# <a name="generate-preinstall-packages-for-oems"></a>Generar paquetes preinstalados para OEM

Si tu cuenta de desarrollador tiene los permisos adecuados, puedes generar y descargar paquetes de preinstalación para que un OEM pueda incluir tu aplicación en su imagen de SO. Los permisos de preinstalación solo están habilitados en cuentas de desarrollador patrocinadas por OEM.


## <a name="important-preinstall-policy--limitations"></a>Directivas y limitaciones de preinstalación importantes

Deben estar certificadas preinstalar aplicaciones a través de [centro de partners](https://partner.microsoft.com/dashboard) tener la licencia de Store más reciente para que sean capaces de conectarse a la Store y recibir actualizaciones de aplicaciones.

Las aplicaciones que estén preinstaladas deben ser y permanecer gratuitas en todos los mercados.


## <a name="generating-preinstall-packages"></a>Generar paquetes de preinstalación

Una vez que se habilite una cuenta con permisos de preinstalación, completa los siguientes pasos:

1.  En el centro de partners, vaya a la aplicación que se esté preinstalado.
2.  En el menú de navegación izquierdo, expande **Administración de aplicaciones** y selecciona **Paquetes actuales**.
3.  En la sección **Solicitar paquetes para la preinstalación del SO**, selecciona **Habilitar los paquetes descargables**.
4.  En el cuadro de diálogo de confirmación, selecciona **Habilitar**.
5.  Busca el paquete que quieres descargar y selecciona el vínculo **Generar paquete** apropiado.

    > [!NOTE]
    > La hora de generación de los paquetes de preinstalación variará según el tamaño del paquete que hayas seleccionado. Puedes salir de esta página y volver más tarde o puedes dejar la página abierta mientras se genera el paquete.

6.  Después de generar el paquete, aparecerá un vínculo a **Descargar paquete**. Selecciónalo para descargar el archivo .zip.

Después puedes proporcionar el archivo .zip al OEM para que lo incluya en su imagen de SO.


## <a name="support"></a>Soporte

Si tiene más preguntas sobre la generación de paquetes de preinstalación, envíe un correo electrónico <partnerops@microsoft.com>.

 

 




