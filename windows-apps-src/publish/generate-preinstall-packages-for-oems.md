---
author: jnHs
Description: "Si tu cuenta de desarrollador tiene los permisos adecuados, puedes generar y descargar paquetes de preinstalación que un OEM puede usar para incluir tu aplicación en su imagen."
title: Generar paquetes preinstalados para OEM
ms.assetid: AC3A45E8-7BBD-44E9-B2D3-B74B7C9B2BC9
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: e040f0ea1f2106da2d7da76464d4c818f39d6735
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="generate-preinstall-packages-for-oems"></a>Generar paquetes preinstalados para OEM


Si tu cuenta de desarrollador tiene los permisos adecuados, puedes generar y descargar paquetes de preinstalación que un OEM puede usar para incluir tu aplicación en su imagen. Los permisos de preinstalación solo están habilitados en cuentas de desarrollador patrocinadas por OEM.

## <a name="important-preinstall-policy--limitations"></a>Directivas y limitaciones de preinstalación importantes


Las aplicaciones de preinstalación deben estar certificadas a través del Centro de desarrollo de Windows con la licencia más reciente de la Tienda para que puedan conectarse a la Tienda y recibir actualizaciones de aplicación.

Las aplicaciones que ya estén preinstaladas deben ser y permanecer gratuitas en todos los mercados.

## <a name="generating-preinstall-packages"></a>Generar paquetes de preinstalación


Una vez que se habilite una cuenta con permisos de preinstalación, completa los siguientes pasos:

1.  En el panel, ve a la aplicación que se va a preinstalar.
2.  En el menú de navegación izquierdo, expande **Administración de aplicaciones** y, a continuación, haz clic en **Paquetes actuales**.
3.  En la sección **Solicitar paquetes para la preinstalación del SO**, haz clic en **Habilitar los paquetes descargables**.
4.  Aparecerá un cuadro de diálogo de confirmación que indica que las aplicaciones preinstaladas en un sistema operativo anterior a Windows 10 deben ser gratuitas. Selecciona **Habilitar**.
5.  Busca el paquete que quieres descargar y haz clic en el vínculo **Generar paquete** apropiado.
    > **Nota** La hora de generación de los paquetes de preinstalación variará según el tamaño del paquete que hayas seleccionado. Puede salir de esta página y volver más tarde o dejarla abierta.
6.  Cuando el paquete se haya generado, aparecerá un vínculo a **Descargar paquete**. Haz clic en él para descargar el archivo .zip.

Después puedes proporcionar este archivo .zip al OEM para que lo incluya en su imagen de SO.

## <a name="support"></a>Soporte técnico


Si tienes más preguntas sobre la generación de paquetes de preinstalación, envía un correo electrónico a <partnerops@microsoft.com>.

 

 




