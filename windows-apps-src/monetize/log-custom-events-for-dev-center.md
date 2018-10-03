---
author: mcleanbyron
Description: You can log custom events from your UWP app and review those events in the Usage report on the Windows Dev Center dashboard.
title: Registrar eventos personalizados para el Centro de desarrollo
ms.author: mcleans
ms.date: 06/01/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, Microsoft Store Services SDK, registrar eventos, log events
ms.assetid: 4aa591e0-c22a-4c90-b316-0b5d0410af19
ms.localizationpriority: medium
ms.openlocfilehash: 2b9cd4d7c527001bb382596c9c805be4ad5e7b08
ms.sourcegitcommit: 1938851dc132c60348f9722daf994b86f2ead09e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2018
ms.locfileid: "4259846"
---
# <a name="log-custom-events-for-dev-center"></a>Registrar eventos personalizados para el Centro de desarrollo

El [informe de uso](https://msdn.microsoft.com/windows/uwp/publish/usage-report) del panel Centro de desarrollo de Windows te permite obtener información sobre los eventos personalizados que definiste en tu aplicación para la Plataforma universal de Windows (UWP). Un evento personalizado es una cadena arbitraria que representa un evento o una actividad en tu aplicación. Por ejemplo, un juego podría definir eventos personalizados denominados *firstLevelPassed*, *secondLevelPassed*, y así sucesivamente, que se registren cuando el usuario supere los distintos niveles del juego.

Para registrar un evento personalizado de tu aplicación, pasa la cadena de eventos personalizados al método [Log](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log) proporcionado por el Microsoft Store Services SDK. Puedes revisar el total de repeticiones para los eventos personalizados en la sección **Eventos personalizados** del informe de [uso](https://msdn.microsoft.com/windows/uwp/publish/usage-report) en el panel del Centro de desarrollo.

> [!NOTE]
> Los eventos personalizados que registras en el Centro de desarrollo no están relacionados con los [eventos de Windows](https://msdn.microsoft.com/library/windows/desktop/aa964766.aspx) y no aparecen en el **Visor de eventos**.

## <a name="prerequisites"></a>Requisitos previos

Para poder revisar los eventos de registro personalizado en el **informe de uso** de tu aplicación en el panel, la aplicación debe publicarse en la Tienda.

## <a name="how-to-log-custom-events"></a>Cómo registrar eventos personalizados

1. Si aún no lo has hecho, [instala el Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk) en el equipo de desarrollo.

2. Abre el proyecto en Visual Studio.

3. En el Explorador de soluciones, haz clic con el botón secundario en el nodo **Referencias** del proyecto y haz clic en **Agregar referencia**.

4. En el cuadro de diálogo **Administrador de referencias**, expande **Windows Universal** y haz clic en **Extensiones**.

5. En la lista de los SDK, haz clic en la casilla junto a **Microsoft Engagement Framework** y haz clic en **Aceptar**.

6. Agrega la siguiente instrucción en la parte superior de cada archivo de código donde quieras registrar eventos personalizados.
    [!code-cs[EventLogger](./code/StoreSDKSamples/cs/LogEvents.cs#EngagementNamespace)]

7. En cada sección del código en la que quieras registrar un evento personalizado, obtén un objeto [StoreServicesCustomEventLogger](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log) y, a continuación, llama al método [Log](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log). Pasa la cadena de eventos personalizada al método.
    [!code-cs[EventLogger](./code/StoreSDKSamples/cs/LogEvents.cs#Log)]

    > [!NOTE]
    > E [Informe de uso](https://msdn.microsoft.com/windows/uwp/publish/usage-report) puede tardar mucho tiempo en cargarse si tu aplicación registra muchos eventos personalizados con nombres largos. Te recomendamos que uses nombres breves para tus eventos personalizados. 

## <a name="related-topics"></a>Temas relacionados

* [Informe de uso](https://msdn.microsoft.com/windows/uwp/publish/usage-report)
* [Método de registro](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log)
* [Microsoft Store Services SDK](https://msdn.microsoft.com/windows/uwp/monetize/microsoft-store-services-sdk)
