---
Description: Puede registrar eventos personalizados desde la aplicación de UWP y revisar esos eventos en el informe de uso del centro de Partners.
title: Registrar eventos personalizados para el Centro de partners
ms.date: 06/01/2018
ms.topic: article
keywords: SDK de Windows 10, UWP, Microsoft Store Services, eventos de registro
ms.assetid: 4aa591e0-c22a-4c90-b316-0b5d0410af19
ms.localizationpriority: medium
ms.openlocfilehash: 5a1df08b62199bf1249af8bfbbb00921874a671c
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89364108"
---
# <a name="log-custom-events-for-partner-center"></a>Registrar eventos personalizados para el Centro de partners

El [Informe de uso](../publish/usage-report.md) del centro de Partners le permite obtener información sobre los eventos personalizados que ha definido en la aplicación plataforma universal de Windows (UWP). Un evento personalizado es una cadena arbitraria que representa un evento o una actividad en tu aplicación. Por ejemplo, un juego podría definir eventos personalizados denominados *firstLevelPassed*, *secondLevelPassed*, y así sucesivamente, que se registren cuando el usuario supere los distintos niveles del juego.

Para registrar un evento personalizado de tu aplicación, pasa la cadena de eventos personalizados al método [Log](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log) proporcionado por el Microsoft Store Services SDK. Puede revisar el número total de repeticiones de los eventos personalizados en la sección **eventos personalizados** del [Informe de uso](../publish/usage-report.md) del centro de Partners.

> [!NOTE]
> Los eventos personalizados que se registran en el centro de Partners no están relacionados con [los eventos de Windows](/windows/desktop/Events/windows-events)y no aparecen en **visor de eventos**.

## <a name="prerequisites"></a>Requisitos previos

Para poder revisar los eventos de registro personalizados en el **Informe de uso** de la aplicación en el centro de Partners, la aplicación debe estar Publicada en la tienda.

## <a name="how-to-log-custom-events"></a>Cómo registrar eventos personalizados

1. Si aún no lo has hecho, [instala el Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk) en el equipo de desarrollo.

2. Abra el proyecto en Visual Studio.

3. En el Explorador de soluciones, haz clic con el botón secundario en el nodo **Referencias** del proyecto y haz clic en **Agregar referencia**.

4. En el cuadro de diálogo **Administrador de referencias**, expande **Windows Universal** y haz clic en **Extensiones**.

5. En la lista de los SDK, haz clic en la casilla junto a **Microsoft Engagement Framework** y haz clic en **Aceptar**.

6. Agrega la siguiente instrucción en la parte superior de cada archivo de código en el que quieras registrar eventos personalizados.
    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/LogEvents.cs" id="EngagementNamespace":::

7. En cada sección del código en la que quieras registrar un evento personalizado, obtén un objeto [StoreServicesCustomEventLogger](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log) y, a continuación, llama al método [Log](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log). Pasa la cadena de eventos personalizada al método.
    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/LogEvents.cs" id="Log":::

    > [!NOTE]
    > El [Informe de uso](../publish/usage-report.md) puede tardar mucho tiempo en cargarse si la aplicación registra muchos eventos personalizados con nombres largos. Se recomienda usar nombres breves para los eventos personalizados. 

## <a name="related-topics"></a>Temas relacionados

* [Informe de uso](../publish/usage-report.md)
* [Método de registro](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log)
* [Microsoft Store Services SDK](./microsoft-store-services-sdk.md)
