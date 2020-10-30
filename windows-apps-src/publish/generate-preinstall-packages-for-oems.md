---
description: Si a la cuenta de desarrollador se le han concedido los permisos adecuados, puede generar y descargar paquetes de preinstalación para que un OEM pueda incluir la aplicación en su imagen de sistema operativo.
title: Generar paquetes preinstalados para OEM
ms.assetid: AC3A45E8-7BBD-44E9-B2D3-B74B7C9B2BC9
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e6b2aa9688b141318a9e96a26e5d0dc643306459
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034788"
---
# <a name="generate-preinstall-packages-for-oems"></a>Generar paquetes preinstalados para OEM

Si a la cuenta de desarrollador se le han concedido los permisos adecuados, puede generar y descargar paquetes de preinstalación para que un OEM pueda incluir la aplicación en su imagen de sistema operativo. Los permisos de preinstalación solo están habilitados en cuentas de desarrollador patrocinadas por OEM.


## <a name="important-preinstall-policy--limitations"></a>Directivas y limitaciones de preinstalación importantes

Las aplicaciones de preinstalación deben estar certificadas a través del [centro de Partners](https://partner.microsoft.com/dashboard) para tener la licencia de la tienda más reciente, de modo que puedan conectarse a la tienda y recibir actualizaciones de la aplicación.

Cualquier aplicación que esté preinstalada debe estar disponible en todos los mercados.


## <a name="generating-preinstall-packages"></a>Generar paquetes de preinstalación

Una vez que se habilite una cuenta con permisos de preinstalación, completa los siguientes pasos:

1.  En el centro de Partners, vaya a la aplicación que se va a preinstalar.
2.  En el menú de navegación izquierdo, expanda **Administración de aplicaciones** y seleccione **paquetes actuales** .
3.  En la sección **solicitar paquetes para la preinstalación del sistema operativo** , seleccione **Habilitar paquetes descargables** .
4.  En el cuadro de diálogo de confirmación, seleccione **Habilitar** .
5.  Busque el paquete que desea descargar y seleccione el vínculo **generar paquete** adecuado.

    > [!NOTE]
    > La hora de generación de los paquetes de preinstalación variará en función del tamaño del paquete que haya seleccionado. Puede salir de esta página y volver más tarde, o puede dejar la página abierta mientras se genera el paquete.

6.  Una vez generado el paquete, aparecerá un vínculo para **descargar el paquete** . Seleccione este vínculo para descargar el archivo. zip.

Después, puede proporcionar el archivo. zip al OEM para su inclusión en su imagen de so.


## <a name="support"></a>Soporte técnico

Si tienes más preguntas sobre la generación de paquetes de preinstalación, envía un correo electrónico a <partnerops@microsoft.com>.

 

 




