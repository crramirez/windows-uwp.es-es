---
Description: Puede registrar eventos personalizados de la aplicación UWP y revisar los eventos en el informe de uso en el centro de partners.
title: Registrar eventos personalizados para el Centro de partners
ms.date: 06/01/2018
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Services SDK, registrar eventos, log events
ms.assetid: 4aa591e0-c22a-4c90-b316-0b5d0410af19
ms.localizationpriority: medium
ms.openlocfilehash: e45b14daf6951142cb0d0ed8714e981eb6a55628
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371041"
---
# <a name="log-custom-events-for-partner-center"></a>Registrar eventos personalizados para el Centro de partners

El [informe de uso](https://docs.microsoft.com/windows/uwp/publish/usage-report) en le permite de centro de partners para obtener información sobre los eventos personalizados que haya definido en la aplicación de plataforma Universal de Windows (UWP). Un evento personalizado es una cadena arbitraria que representa un evento o una actividad en tu aplicación. Por ejemplo, un juego podría definir eventos personalizados denominados *firstLevelPassed*, *secondLevelPassed*, y así sucesivamente, que se registren cuando el usuario supere los distintos niveles del juego.

Para registrar un evento personalizado de tu aplicación, pasa la cadena de eventos personalizados al método [Log](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log) proporcionado por el Microsoft Store Services SDK. Puede revisar las apariciones totales para los eventos personalizados en el **eventos personalizados** sección de la [informe de uso](https://docs.microsoft.com/windows/uwp/publish/usage-report) en el centro de partners.

> [!NOTE]
> Eventos personalizados que inicie sesión en el centro de partners son no relacionada con [eventos de Windows](https://docs.microsoft.com/windows/desktop/Events/windows-events), y no aparecen en **Visor de eventos**.

## <a name="prerequisites"></a>Requisitos previos

Para poder revisar eventos de registro personalizado en el **informe de uso** para la aplicación en el centro de partners, se debe publicar la aplicación en el Store.

## <a name="how-to-log-custom-events"></a>Cómo registrar eventos personalizados

1. Si aún no lo has hecho, [instala el Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk) en el equipo de desarrollo.

2. Abre el proyecto en Visual Studio.

3. En el Explorador de soluciones, haz clic con el botón secundario en el nodo **Referencias** del proyecto y haz clic en **Agregar referencia**.

4. En el cuadro de diálogo **Administrador de referencias**, expande **Windows Universal** y haz clic en **Extensiones**.

5. En la lista de los SDK, haz clic en la casilla junto a **Microsoft Engagement Framework** y haz clic en **Aceptar**.

6. Agrega la siguiente instrucción en la parte superior de cada archivo de código en el que quieras registrar eventos personalizados.
    [!code-csharp[EventLogger](./code/StoreSDKSamples/cs/LogEvents.cs#EngagementNamespace)]

7. En cada sección del código en la que quieras registrar un evento personalizado, obtén un objeto [StoreServicesCustomEventLogger](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log) y, a continuación, llama al método [Log](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log). Pasa la cadena de eventos personalizada al método.
    [!code-csharp[EventLogger](./code/StoreSDKSamples/cs/LogEvents.cs#Log)]

    > [!NOTE]
    > E [Informe de uso](https://docs.microsoft.com/windows/uwp/publish/usage-report) puede tardar mucho tiempo en cargarse si tu aplicación registra muchos eventos personalizados con nombres largos. Te recomendamos que uses nombres breves para tus eventos personalizados. 

## <a name="related-topics"></a>Temas relacionados

* [Informe de uso](https://docs.microsoft.com/windows/uwp/publish/usage-report)
* [Método de registro](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log)
* [Microsoft Store Services SDK](https://docs.microsoft.com/windows/uwp/monetize/microsoft-store-services-sdk)
