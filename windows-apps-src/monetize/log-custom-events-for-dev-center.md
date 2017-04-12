---
author: mcleanbyron
Description: "Puedes registrar eventos personalizados desde la aplicación para UWP y revisar los eventos en el informe de uso en el panel del Centro de desarrollo de Windows."
title: Registrar eventos personalizados para el Centro de desarrollo
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, Microsoft Store Services SDK, registrar eventos, log events
ms.assetid: 4aa591e0-c22a-4c90-b316-0b5d0410af19
ms.openlocfilehash: b2fad3ea78a2007b2519e4b0b27276f9f003af2c
ms.sourcegitcommit: d053f28b127e39bf2aee616aa52bb5612194dc53
translationtype: HT
---
# <a name="log-custom-events-for-dev-center"></a>Registrar eventos personalizados para el Centro de desarrollo

El [informe de uso](https://msdn.microsoft.com/windows/uwp/publish/usage-report) del panel Centro de desarrollo de Windows te permite obtener información sobre los eventos personalizados que definiste en tu aplicación para la Plataforma universal de Windows (UWP). Un evento personalizado es una cadena arbitraria que representa un evento o una actividad en tu aplicación. Por ejemplo, un juego podría definir eventos personalizados denominados *firstLevelPassed*, *secondLevelPassed*, y así sucesivamente, que se registren cuando el usuario supere los distintos niveles del juego.

Para registrar un evento personalizado de tu aplicación, pasa la cadena de eventos personalizados al método [Log](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.log.aspx) proporcionado por el Microsoft Store Services SDK. Puedes revisar el total de repeticiones para los eventos personalizados en la sección **Eventos personalizados** del informe de [uso](https://msdn.microsoft.com/windows/uwp/publish/usage-report) en el panel del Centro de desarrollo.

> [!NOTE]
> Los eventos personalizados que registras en el Centro de desarrollo no están relacionados con los [eventos de Windows](https://msdn.microsoft.com/library/windows/desktop/aa964766.aspx) y no aparecen en el **Visor de eventos**.

## <a name="prerequisites"></a>Requisitos previos

Para poder revisar los eventos de registro personalizado en el **informe de uso** de tu aplicación en el panel, la aplicación debe publicarse en la Tienda.

## <a name="how-to-log-custom-events"></a>Cómo registrar eventos personalizados

1. Si aún no lo has hecho, [instala el Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk) en el equipo de desarrollo. Además de la API para registrar eventos personalizados, este SDK también proporciona API para otras características, como ejecutar experimentos en las aplicaciones con pruebas A/B y mostrar anuncios.
2. Abre el proyecto en Visual Studio.
3. En el Explorador de soluciones, haz clic con el botón secundario en el nodo **Referencias** del proyecto y haz clic en **Agregar referencia**.
4. En el cuadro de diálogo **Administrador de referencias**, expande **Windows Universal** y haz clic en **Extensiones**.
5. En la lista de los SDK, haz clic en la casilla junto a **Microsoft Engagement Framework** y haz clic en **Aceptar**.
7. Agrega la siguiente instrucción en la parte superior de cada archivo de código en el que quieras registrar eventos personalizados.

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[EventLogger](./code/StoreSDKSamples/cs/LogEvents.cs#EngagementNamespace)]

8. En cada sección del código en la que quieras registrar un evento personalizado, obtén un objeto [StoreServicesCustomEventLogger](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.log.aspx) y, a continuación, llama al método [Log](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.log.aspx). Pasa la cadena de eventos personalizada al método.

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[EventLogger](./code/StoreSDKSamples/cs/LogEvents.cs#Log)]

## <a name="related-topics"></a>Temas relacionados

* [Informe de uso](https://msdn.microsoft.com/windows/uwp/publish/usage-report)
* [Método de registro](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.log.aspx)
* [Microsoft Store Services SDK](https://msdn.microsoft.com/windows/uwp/monetize/microsoft-store-services-sdk)
