---
description: Aprende a actualizar la aplicación para que use las últimas bibliotecas admitidas de Microsoft Advertising y a asegurarte de que siga recibiendo anuncios de banner.
title: Usar las bibliotecas de publicidad más recientes para anuncios de banner
ms.date: 02/18/2020
ms.topic: article
keywords: windows 10, Windows 10, uwp, UWP, ads, anuncios, advertising, publicidad, AdControl, AdControl, AdMediatorControl, AdMediatorControl, migrate, migrar
ms.assetid: f8d5b2ad-fcdb-4891-bd68-39eeabdf799c
ms.localizationpriority: medium
ms.openlocfilehash: a8ccc8e9c76fc0f16bcdfc619d8048307fdfbc57
ms.sourcegitcommit: 6af7ce0e3c27f8e52922118deea1b7aad0ae026e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77463887"
---
# <a name="update-your-app-to-the-latest-advertising-libraries-for-banner-ads"></a>Actualizar la aplicación a las bibliotecas de publicidad más recientes de anuncios de banner

>[!WARNING]
> A partir del 1 de junio de 2020, se cerrará la plataforma de monetización de Microsoft ad para aplicaciones UWP de Windows. [Más información](https://aka.ms/ad-monetization-shutdown)

A partir del 1 de abril de 2017, ya no se proporcionan anuncios de banner a aplicaciones que usan una versión no compatible del SDK de publicidad. Si usas **AdControl** para mostrar anuncios de banner en la aplicación de Plataforma universal de Windows (UWP), usa la información incluida en este artículo para determinar si usas un SDK de publicidad no compatible y migrar la aplicación a un SDK compatible.

## <a name="overview"></a>Información general

Las aplicaciones para UWP que muestran anuncios de banner deben usar **AdControl** desde las bibliotecas de publicidad que se distribuyen en el [SDK de Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK). Este SDK admite un conjunto mínimo de capacidades de publicidad, incluida la posibilidad de ofrecer contenido multimedia enriquecido de HTML5 a través de la [especificación Mobile Rich-media Ad Interface Definitions (MRAID) 1.0](https://www.iab.com/wp-content/uploads/2015/08/IAB_MRAID_VersionOne.pdf) de Interactive Advertising Bureau (IAB). Muchos de nuestros anunciantes buscan estas capacidades, y exigimos que los desarrolladores de aplicaciones usen una de estas versiones del SDK con el fin de ayudar a que nuestro ecosistema de aplicaciones sea más atractivo para los anunciantes y, en última instancia, generar más ingresos para ti.

Antes de que este SDK se lanzara, proporcionamos anteriormente la clase **AdControl** en varias versiones antiguas del SDK de publicidad. Ya no se admiten estas versiones anteriores del SDK de publicidad porque no son compatibles con las capacidades mínimas de publicidad descritas anteriormente. A partir del 1 de abril de 2017, ya no se proporcionan anuncios de banner a aplicaciones que usan una versión no compatible del SDK de publicidad. Si tienes una aplicación que aún usa una versión no compatible del SDK de publicidad, verás el siguiente comportamiento:

* Ya no se ofrecerán anuncios de banner al control **AdControl** de tu aplicación y ya no obtendrás ingresos publicitarios por esos controles.

* Cuando el control **AdControl** de tu aplicación solicite un nuevo anuncio, se generará el evento **ErrorOccurred** del control y la propiedad **ErrorCode** de los argumentos del evento tendrá el valor **NoAdAvailable**.

* Se desactivarán las unidades de anuncios que están asociadas a tu aplicación. No se pueden quitar estas unidades de anuncio desactivadas de la cuenta de DePartnerv Center. Si actualizas la aplicación para usar el [SDK de Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK), pasa por alto estas unidades de anuncios y crea otras nuevas.

* Los anuncios de banner también dejarán de proporcionarse para las unidades de anuncios que se usan en más de una aplicación. Asegúrate de que las unidades de anuncios se usen cada una solo en una aplicación.

Si tienes una aplicación (que ya está en la Store o que aún está en desarrollo) que muestra anuncios de banner con **AdControl** y no estás seguro de que SDK de publicidad usa la aplicación, sigue las instrucciones de este artículo para determinar si tienes que actualizar la aplicación a un SDK compatible. Si se produce algún problema o necesitas ayuda, [ponte en contacto con soporte técnico](https://support.microsoft.com/getsupport/hostpage.aspx?locale=EN-US&supportregion=EN-US&ccfcode=US&ln=EN-US&pesid=14654&oaspworkflow=start_1.0.0.0&tenant=store&supporttopic_L1=32136151).

> [!NOTE]
> Si la aplicación ya usa el [SDK de Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) (para aplicaciones para UWP), no necesitas realizar más cambios en tu aplicación.

## <a name="prerequisites"></a>Requisitos previos

* El código fuente completo y los archivos del proyecto de Visual Studio de la aplicación que usa el control **AdControl**.
* El paquete .appx de la aplicación.

> [!NOTE]
> Si ya no tienes el paquete .appx de la aplicación, pero todavía tienes un equipo de desarrollo con la versión de Visual Studio y el SDK de publicidad que se usó para crear la aplicación, puedes volver a generar el paquete .appx en Visual Studio.

<span id="part-1" />

## <a name="part-1-determine-whether-you-need-to-update-your-uwp-app"></a>Parte 1: determinar si es necesario actualizar la aplicación para UWP

Sigue las instrucciones de las siguientes secciones para determinar si necesitas actualizar la aplicación.

1. Crea una copia del paquete .appx de la aplicación para no alterar el original, cambia el nombre de la copia para que tenga una extensión .zip y extrae el contenido del archivo.

2. Comprueba el contenido extraído del paquete de la aplicación:
  * Si ves un archivo Microsoft.Advertising.dll, la aplicación usa un SDK anterior y debes actualizar el proyecto siguiendo las instrucciones que aparecen en las secciones a continuación. Ve a la [Parte 2](update-your-app-to-the-latest-advertising-libraries.md#part-2).
  * Si no ves un archivo Microsoft.Advertising.dll, la aplicación para UWP ya usa el SDK de publicidad más reciente disponible y no es necesario hacer cambios en el proyecto.


<span id="part-2" />

## <a name="part-2-install-the-latest-sdk"></a>Parte 2: instalar el SDK más reciente

Si la aplicación usa una versión anterior del SDK, sigue estas instrucciones para asegurarte de que tienes el SDK más reciente en el equipo de desarrollo.

1. Asegúrate de que el equipo de desarrollo tenga Visual Studio 2015 o una versión posterior instalada.
    > [!NOTE]
    > Si Visual Studio está abierto en el equipo de desarrollo, ciérralo antes de seguir los pasos a continuación.

1.  Desinstala todas las versiones anteriores del SDK de Microsoft Advertising y del SDK de Ad Mediator del equipo de desarrollo.

2.  Abre una ventana **Símbolo del sistema** y ejecuta los siguientes comandos para limpiar todas las versiones de SDK que se hayan instalado con Visual Studio, pero que podrían no aparecer en la lista de programas instalados en el equipo:
    ```syntax
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  Instala el [SDK de Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK).

## <a name="part-3-update-your-project"></a>Parte 3: actualizar el proyecto

Elimina todas las referencias existentes a las bibliotecas de publicidad de Microsoft del proyecto y sigue [estas instrucciones](install-the-microsoft-advertising-libraries.md#reference) para agregar las referencias necesarias. Esto garantizará que el proyecto use las bibliotecas correctas. Puedes conservar el código y el marcado existentes.

## <a name="part-4-test-and-republish-your-app"></a>Parte 4: probar y volver a publicar la aplicación

Prueba la aplicación para asegurarte de que muestra anuncios de banner según lo previsto.

Si la versión anterior de la aplicación ya está disponible en el almacén, [cree un nuevo envío](../publish/app-submissions.md) de la aplicación actualizada en el centro de partners para volver a publicar la aplicación.
