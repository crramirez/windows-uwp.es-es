---
author: jnHs
Description: The Packages page is where you upload all of the package files (.appxupload, .appx, .appxbundle, and/or .xap) for the app that you're submitting.
title: Cargar paquetes de aplicaciones
ms.assetid: B1BB810D-3EAA-4FB5-B03C-1F01AFB2DE36
ms.author: wdg-dev-content
ms.date: 5/10/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, paquetes, carga, carga de paquete
ms.localizationpriority: medium
ms.openlocfilehash: 6013a238cff8db3b85dd98af58cccaf344a72f51
ms.sourcegitcommit: 914b38559852aaefe7e9468f6f53a7465bf36e30
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/06/2018
ms.locfileid: "3401707"
---
# <a name="upload-app-packages"></a>Cargar paquetes de aplicación

La página **Paquetes** es donde se cargan todos los archivos del paquete (.appx, .appxupload, .appxbundle o .xap) de la aplicación que vas a enviar. En este paso puedes cargar paquetes para cualquier sistema operativo de destino de la aplicación. Cada vez que un cliente descarga la aplicación, la Tienda le proporciona automáticamente el paquete que mejor se adapta a su dispositivo. Después de cargar los paquetes, verás una tabla que indica [qué paquetes que se ofrecerán a familias específicas de dispositivos Windows 10](#device-family-availability) (y a versiones anteriores del sistema operativo, si procede) ordenados según su clasificación.

Consulta [Requisitos del paquete de la aplicación](app-package-requirements.md) para obtener más información sobre lo que incluye un paquete y cómo debe estructurarse. También querrás obtener información sobre [cómo los números de versión pueden afectar a los paquetes que se entregan a clientes específicos](package-version-numbering.md) y [cómo se distribuyen los paquetes a diferentes sistemas operativos](guidance-for-app-package-management.md).

## <a name="uploading-packages-to-your-submission"></a>Cargar paquetes en el envío

Para cargar paquetes, arrástralos en el campo de carga o haz clic para examinar los archivos. La página **Paquetes** te permite cargar archivos .xap, .appx, .appxupload y .appxbundle.

> [!IMPORTANT]
> Para Windows10, se recomienda cargar aquí el archivo .appxupload en lugar de un .appx o .appxbundle.  Para obtener más información sobre cómo empaquetar aplicaciones para UWP para la Tienda, consulta [Empaquetar una aplicación para UWP con Visual Studio](../packaging/packaging-uwp-apps.md).

Si has creado algún [paquete piloto](package-flights.md) para tu aplicación, verás una lista desplegable con la opción para copiar los paquetes de uno de los paquetes piloto. Selecciona el paquete piloto que tiene los paquetes que quieres extraer. A continuación, podrás seleccionar varios o todos los paquetes, para incluirlos en este envío.

Si se detectan los errores con un paquete durante la validación, se mostrará un mensaje que te permiten saber cuál es el problema. Deberás quitar el paquete, corregir el problema y, a continuación, intenta cargarlo de nuevo. En otros casos se mostrarán advertencias para informarte de posibles problemas, aunque no se te impedirá que continúes con el envío.


## <a name="device-family-availability"></a>Disponibilidad de familias de dispositivos

Una vez hayas cargado los paquetes, la sección **Disponibilidad de familias de dispositivos** mostrará una tabla que indica qué paquetes se ofrecerán a familias específicas de dispositivos Windows 10 (y a versiones anteriores del sistema operativo, si procede), ordenados según su clasificación. En esta sección también podrás elegir si quieres o no ofrecer el envío a clientes de familias específicas de dispositivos Windows 10.

Para obtener más información, consulta [Disponibilidad de familias de dispositivos](device-family-availability.md).


## <a name="package-details"></a>Detalles del paquete

Aquí, se enumeran los paquetes cargados agrupados por sistema operativo de destino. Se mostrará el nombre, la versión y la arquitectura del paquete. Para obtener más información como, por ejemplo, los idiomas admitidos, las capacidades de la aplicación y el tamaño de archivo de cada paquete, haz clic en **Mostrar detalles**.

Si necesitas quitar un paquete de tu envío, haz clic en el vínculo **Quitar** en la parte inferior de la sección **Detalles** de cada paquete.


## <a name="removing-redundant-packages"></a>Quitar paquetes redundantes

Si se detecta que uno o varios de los paquetes son redundantes, se mostrará una advertencia para sugerirte que quites los paquetes redundantes de este envío. Esto sucede a menudo cuando has cargado paquetes previamente y ahora estás proporcionando paquetes de una versión posterior que admiten el mismo conjunto de clientes. En este caso, ningún cliente recibirá el paquete redundante porque hay un paquete mejor (de versión más reciente) para ellos.

Cuando detectemos paquetes redundantes, proporcionaremos una opción para quitarlos de este envío automáticamente. Si así lo prefieres, también puedes quitar paquetes del envío de forma individual.


## <a name="gradual-package-rollout"></a>Lanzamiento gradual del paquete

Si el envío es una actualización de una aplicación publicada anteriormente, verás una casilla denominada **Lanzar gradualmente la actualización después de publicar este envío (solo a clientes de Windows 10)**. Esta opción te permitirá seleccionar un porcentaje de clientes a los que enviar los paquetes, y así podrás supervisar sus comentarios y los datos analíticos para asegurarte de que la actualización es correcta antes de realizar una distribución más amplia. Asimismo, puedes incrementar el porcentaje (o detener la actualización) en cualquier momento, sin tener que crear un nuevo envío. 

Para obtener más información, consulta [Gradual package rollout (Lanzamiento gradual del paquete)](gradual-package-rollout.md).


## <a name="mandatory-update"></a>Actualización obligatoria

Si el envío es una actualización de una aplicación publicada anteriormente, verás una casilla denominada **Hacer que esta actualización sea obligatoria**. Esto te permitirá establecer la fecha y hora para realizar una actualización obligatoria, siempre y cuando hayas usado las API de Windows.Services.Store para permitir que la aplicación, mediante programación, busque actualizaciones de paquetes, las descargue y las instale. La aplicación debe estar destinada a la versión 1607 (o posterior) de Windows 10 para poder usar esta opción.

Para obtener más información, consulta [Download and install package updates for your app (Descargar e instalar actualizaciones del paquete de la aplicación](../packaging/self-install-package-updates.md).

 




