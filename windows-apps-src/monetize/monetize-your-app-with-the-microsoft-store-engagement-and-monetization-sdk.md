---
Description: El SDK de Microsoft Store Engagement and Monetization proporciona bibliotecas y herramientas que puedes usar para agregar características a las aplicaciones que te ayudarán a obtener más dinero y a ganar clientes.
title: Rentabilizar la aplicación y atraer clientes con el SDK de Microsoft Store Engagement and Monetization.
ms.assetid: 518516DB-70A7-49C4-B3B6-CD8A98320B9C
---

# Rentabilizar la aplicación y atraer clientes con el SDK de Microsoft Store Engagement and Monetization.

El SDK de Microsoft Store Engagement and Monetization proporciona bibliotecas y herramientas que te ayudarán a obtener más dinero y a ganar clientes, por ejemplo, mostrando anuncios en tus aplicaciones y ejecutando experimentos mediante pruebas A/B. Este SDK reemplaza el SDK de cliente de anuncios universal de Microsoft y evolucionará con el tiempo para incluir nuevas características de participación y rentabilidad.


## Características disponibles en el SDK

El SDK de Microsoft Store Engagement and Monetization proporciona bibliotecas y herramientas que admiten las siguientes características.

### Ejecutar experimentos con pruebas A/B para aplicaciones para UWP

Ejecuta pruebas A/B en las aplicaciones para la Plataforma universal de Windows (UWP) para medir la eficacia de las características en algunos clientes antes de lanzar las características para todo el mundo. Después de definir un experimento en el panel del Centro de desarrollo, usa la clase [ExperimentClient](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.engagementclient.aspx) para obtener las variaciones del experimento en tu aplicación, usa estos datos para modificar el comportamiento de la característica de prueba y, a continuación, usa el método [Log](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomevents.log.aspx) para enviar eventos de vista y eventos de conversión al Centro de desarrollo. Por último, usa el panel para ver los resultados y administrar el experimento.

Para obtener más información, consulta [Ejecutar experimentos con pruebas A/B](run-app-experiments-with-a-b-testing.md).

### Comentarios de la aplicación para aplicaciones para UWP

Usa la clase [Feedback](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.feedback.aspx) en tus aplicaciones para UWP para dirigir a los clientes de Windows 10 al Centro de opiniones, donde pueden enviar sus problemas, sugerencias y votos a favor. A continuación, administra esta información en el [Informe de comentarios](../publish/feedback-report.md) en el panel del Centro de desarrollo.

Para obtener más información, consulta [Iniciar el Centro de opiniones desde la aplicación](launch-feedback-hub-from-your-app.md).

### Mostrar anuncios en tus aplicaciones

Aumenta los ingresos mostrando anuncios en banner o anuncios intersticiales en vídeo de Microsoft en aplicaciones para UWP, así como en aplicaciones de Windows 8.1 y Windows Phone 8.x. También puedes maximizar la velocidad de relleno de los anuncios mediante la mediación de anuncios para mostrar anuncios desde varios proveedores de red de anuncios.

Para obtener más información, consulta [Mostrar anuncios en tu aplicación](display-ads-in-your-app.md).

>**Nota** Las características de publicidad de las versiones anteriores del SDK de cliente de anuncios universal, la extensión Ad Mediator y SDK de Microsoft Advertising ahora están incluidas en el SDK de Microsoft Store Engagement and Monetization.

### Referencia de API

Para ver documentación de referencia sobre las API en el SDK, consulta [Referencia de API del SDK de Microsoft Store Engagement and Monetization](https://msdn.microsoft.com/library/windows/apps/mt691886.aspx).

## Instalar el SDK

Para instalar el SDK de Microsoft Store Engagement and Monetization:

1.  Cierra todas las instancias de Visual Studio 2013 o Visual Studio 2015 y desinstala las versiones anteriores del SDK de cliente de anuncios universal, de la extensión Ad Mediator o el SDK de Microsoft Advertising.
2.  Descargue e instale el [SDK](http://aka.ms/store-em-sdk). Puede tardar unos minutos en instalarse. Espera a que finalice el proceso.
3.  Reinicia Visual Studio.

Microsoft publica periódicamente nuevas versiones del SDK de Microsoft Store Engagement and Monetization con nuevas características y mejoras de rendimiento. Si tienes proyectos existentes que usan el SDK de Microsoft Store Engagement and Monetization y quieres usar la versión más reciente, simplemente descarga e instala la versión más reciente del SDK.

Las características de publicidad de las versiones anteriores del SDK de cliente de anuncios universal, la extensión Ad Mediator y SDK de Microsoft Advertising ahora están incluidas en el SDK de Microsoft Store Engagement and Monetization. Si tienes proyectos existentes de Visual Studio 2015 o proyectos de Visual Studio 2013 que usan características de publicidad de una de las versiones anteriores, puedes seguir trabajando con los proyectos sin cambios después de instalar el SDK de Microsoft Store Engagement and Monetization.

>**Nota**  Para instalar el SDK de Microsoft Store Engagement and Monetization con Visual Studio 2015, debes tener instalada la versión 1.1 o posterior de Visual Studio Tools para aplicaciones universales de Windows. Para obtener más información sobre esta actualización a Visual Studio Tools para aplicaciones universales de Windows, consulta las [notas de la versión](http://go.microsoft.com/fwlink/?LinkID=624516).

## Paquetes de marcos en el SDK

La siguiente biblioteca en el SDK de Microsoft Store Engagement and Monetization está configurada como un *paquete de marcos*:

* Microsoft.Advertising.dll (solo para proyectos de aplicaciones para UWP). Esta biblioteca contiene las API de publicidad en los espacios de nombres **Microsoft.Advertising** y **Microsoft.Advertising.WinRT.UI**.

Esto significa que después de instalar el SDK en el equipo de desarrollo, esta biblioteca se actualiza automáticamente mediante Windows Update siempre que publiquemos nuevas versiones de las bibliotecas con correcciones y mejoras de rendimiento. Esto ayuda a garantizar que siempre tengas la versión más reciente de la biblioteca instalada en el equipo de desarrollo.

Además, después de que un usuario instale una versión de la aplicación que use esta biblioteca, la biblioteca automáticamente se actualizará en su dispositivo siempre que publiquemos nuevas versiones de la biblioteca con correcciones y mejoras de rendimiento. Esto significa que los usuarios siempre tienen la versión más actual de la biblioteca, sin necesidad de publicar versiones actualizadas de la aplicación en la Tienda.

Sin embargo, si publicamos una nueva versión del SDK que presente nuevas API o características de esta biblioteca, necesitarás instalar la versión más reciente del SDK para usar dichas características. En este escenario, también necesitarás publicar la aplicación actualizada en la Tienda.

Otras bibliotecas en el SDK, como Microsoft.Advertising.dll para otras plataformas de destino y las bibliotecas de mediación de anuncios no están configuradas actualmente como bibliotecas de marcos.

## Temas relacionados

* [Ejecutar experimentos con pruebas A/B](run-app-experiments-with-a-b-testing.md)
* [Referencia de API de SDK de Microsoft Store Engagement and Monetization](https://msdn.microsoft.com/library/windows/apps/mt691886.aspx)
* [Iniciar el Centro de opiniones desde la aplicación](launch-feedback-hub-from-your-app.md)


<!--HONumber=Mar16_HO5-->


