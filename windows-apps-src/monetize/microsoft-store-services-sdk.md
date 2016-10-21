---
author: mcleanbyron
Description: "Microsoft Store Services SDK proporciona bibliotecas y herramientas que puedes usar para agregar características a tus aplicaciones y que te ayudan a obtener más dinero y a ganar clientes."
title: Microsoft Store Services SDK
ms.assetid: 518516DB-70A7-49C4-B3B6-CD8A98320B9C
translationtype: Human Translation
ms.sourcegitcommit: 2f0835638f330de0ac2d17dae28347686cc7ed97
ms.openlocfilehash: 98675e9cb06b05e55d49ca625818626aea5a5346

---

# Microsoft Store Services SDK

Microsoft Store Services SDK proporciona bibliotecas y herramientas que te ayudan a obtener más dinero y a ganar clientes en tus aplicaciones para Plataforma universal de Windows (UWP), por ejemplo, mostrando anuncios en tus aplicaciones y ejecutando experimentos mediante pruebas A/B. Este SDK evolucionará con el tiempo para incluir nuevas características de participación y monetización.


## Características disponibles en el SDK

Microsoft Store Services SDK proporciona bibliotecas y herramientas que admiten las siguientes características.

### Ejecutar experimentos con pruebas A/B para aplicaciones para UWP

Ejecuta pruebas A/B en las aplicaciones para la Plataforma universal de Windows (UWP) para medir la eficacia de las características en algunos clientes antes de lanzar las características para todo el mundo. Después de definir un experimento en el panel del Centro de desarrollo, usa la clase [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) para obtener las variaciones del experimento en tu aplicación, usa estos datos para modificar el comportamiento de la característica que estés probando y luego usa el método [LogForVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation.aspx) para enviar eventos de visualización y eventos de conversión al Centro de desarrollo. Por último, usa el panel para ver los resultados y administrar el experimento.

Para obtener más información, consulta [Ejecutar experimentos con pruebas A/B](run-app-experiments-with-a-b-testing.md).

### Comentarios de la aplicación para aplicaciones para UWP

Usa la clase [StoreServicesFeedbackLauncher](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesfeedbacklauncher.aspx) en tus aplicaciones para UWP para dirigir a los clientes de Windows 10 al Centro de opiniones, donde pueden enviar sus problemas, sugerencias y votos a favor. A continuación, administra esta información en el [Informe de comentarios](../publish/feedback-report.md) en el panel del Centro de desarrollo.

Para obtener más información, consulta [Iniciar el Centro de opiniones desde la aplicación](launch-feedback-hub-from-your-app.md).

### Mostrar anuncios en tus aplicaciones

Para aumentar los ingresos, muestra anuncios en banner o anuncios intersticiales en vídeo de Microsoft en las aplicaciones para UWP. También puedes maximizar la velocidad de relleno de los anuncios mediante la mediación de anuncios para mostrar anuncios desde varios proveedores de red de anuncios.

Para obtener más información, consulta [Mostrar anuncios en tu aplicación](display-ads-in-your-app.md).

>**Nota**&nbsp;&nbsp;Microsoft Store Services SDK solo admite aplicaciones para UWP. Para mostrar anuncios en las aplicaciones de Windows 8.1 y Windows Phone 8.x, usa el [SDK de Microsoft Advertising para Windows y Windows Phone 8.x](http://aka.ms/store-8-sdk).

### Referencia de las API

Para obtener documentación de referencia sobre las API incluidas en Microsoft Store Services SDK, consulta [Microsoft Store Services SDK API reference (Referencia de las API de Microsoft Store Services SDK)](https://msdn.microsoft.com/library/windows/apps/mt691886.aspx).

## Instalar el SDK

Para instalar Microsoft Store Services SDK:

1.  Cierra todas las instancias de Visual Studio 2013 o Visual Studio 2015 y desinstala las versiones anteriores del SDK de Microsoft Store Engagement and Monetization, el SDK de cliente de anuncios universal, la extensión Ad Mediator o el SDK de Microsoft Advertising.
2.  Descarga e instala el [SDK](http://aka.ms/store-em-sdk). Puede tardar unos minutos en instalarse. Espera a que finalice el proceso.
3.  Reinicia Visual Studio.

Microsoft publica periódicamente nuevas versiones de Microsoft Store Services SDK con nuevas características y mejoras de rendimiento. Si tienes proyectos existentes que usan Microsoft Store Services SDK y quieres usar la versión más reciente, simplemente descarga e instala la versión más reciente del SDK.

Las características de publicidad, destinadas a aplicaciones para UWP, de las versiones anteriores del SDK de Microsoft Store Engagement and Monetization, el SDK de cliente de anuncios universal, la extensión Ad Mediator y el SDK de Microsoft Advertising ahora se incluyen en Microsoft Store Services SDK. Si tienes proyectos existentes de UWP que usan las características de publicidad de una de estas versiones anteriores, puedes seguir trabajando con los proyectos sin necesidad de cambios después de instalar Microsoft Store Services SDK.

>**Nota**  Para instalar Microsoft Store Services SDK con Visual Studio 2015, debes tener instalada la versión 1.1 o posterior de Visual Studio Tools para aplicaciones universales de Windows. Para obtener más información sobre esta actualización a Visual Studio Tools para aplicaciones universales de Windows, consulta las [notas de la versión](http://go.microsoft.com/fwlink/?LinkID=624516).

## Paquetes de marcos en el SDK

Las siguientes bibliotecas incluidas en Microsoft Store Services SDK están configuradas como *paquetes de marcos*:

* Microsoft.Advertising.dll. Esta biblioteca contiene las API de publicidad en los espacios de nombres [Microsoft.Advertising](https://msdn.microsoft.com/en-us/library/windows/apps/mt313187.aspx) y [Microsoft.Advertising.WinRT.UI](https://msdn.microsoft.com/en-us/library/windows/apps/microsoft.advertising.winrt.ui.aspx).
* Microsoft.Services.Store.Engagement.dll. Esta biblioteca contiene las API en el espacio de nombres [Microsoft.Services.Store.Engagement](https://msdn.microsoft.com/en-us/library/windows/apps/microsoft.services.store.engagement.aspx).

Esto significa que después de instalar el SDK en el equipo de desarrollo, dichas bibliotecas se actualizan automáticamente mediante Windows Update cada vez que publicamos nuevas versiones de las bibliotecas con correcciones y mejoras de rendimiento. Esto ayuda a garantizar que siempre tengas la versión más reciente de las bibliotecas instalada en el equipo de desarrollo.

Además, después de que un usuario instale una versión de tu aplicación que use estas bibliotecas, las bibliotecas se actualizarán automáticamente en su dispositivo cada vez que publiquemos nuevas versiones de las bibliotecas con correcciones y mejoras de rendimiento. Esto significa que los usuarios siempre tendrán la versión más actual de las bibliotecas, sin necesidad de que publiques versiones actualizadas de tu aplicación en la Tienda.

Sin embargo, si publicamos una nueva versión del SDK que incorpora nuevas API o características en estas bibliotecas, tendrás que instalar la versión más reciente del SDK para usar dichas características. En este escenario, también necesitarás publicar la aplicación actualizada en la Tienda.

## Temas relacionados

* [Microsoft Store Services SDK API reference (Referencia de las API de Microsoft Store Services SDK)](https://msdn.microsoft.com/library/windows/apps/mt691886.aspx)
* [Ejecutar experimentos con pruebas A/B](run-app-experiments-with-a-b-testing.md)
* [Iniciar el Centro de opiniones desde la aplicación](launch-feedback-hub-from-your-app.md)
* [Mostrar anuncios en tu aplicación](display-ads-in-your-app.md)



<!--HONumber=Sep16_HO2-->


