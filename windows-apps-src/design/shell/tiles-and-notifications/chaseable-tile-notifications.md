---
author: andrewleader
Description: Use chaseable tile notifications to find out what your app displayed on its Live Tile when the user clicked it.
title: Notificaciones de iconos rastreables
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Chaseable tile notifications
template: detail.hbs
ms.author: mijacobs
ms.date: 06/13/2017
ms.topic: article
keywords: windows 10, uwp, iconos rastreables, iconos dinámicos, notificaciones de iconos rastreables
ms.localizationpriority: medium
ms.openlocfilehash: 8126755dfb6f5f0e117d10daef85a83e8a171f1f
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "6853394"
---
# <a name="chaseable-tile-notifications"></a>Notificaciones de iconos rastreables

Las notificaciones de icono rastreables permiten determinar qué notificaciones de icono ha mostrado el Icono dinámico de la aplicación cuando el usuario ha hecho clic en dicho icono.  
Por ejemplo, una aplicación de noticias podría usar esta característica para determinar qué noticia mostraba su Icono dinámico cuando el usuario la ha iniciado; luego podría asegurarse de que la historia se muestre en un lugar destacado, para que el usuario pueda encontrarla. 

> [!IMPORTANT]
> **Requiere la Actualización de aniversario**: para usar las notificaciones de icono rastreable con C#, C++, o las aplicaciones para UWP basadas en VB, debes utilizar SDK 14393 y estar ejecutando la compilación 14393 o superior. Para las aplicaciones para UWP basadas en JavaScript, debes utilizar el SDK 17134 y ejecutar la compilación 17134 o superior. 


> **API importantes**: [Propiedad LaunchActivatedEventArgs.TileActivatedInfo](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.TileActivatedInfo), [clase TileActivatedInfo](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.tileactivatedinfo)


## <a name="how-it-works"></a>Funcionamiento

Para habilitar las notificaciones de iconos rastreables, usa la propiedad **Arguments** de la carga de notificaciones de iconos, similar a la propiedad launch en la carga de notificaciones del sistema, para insertar información sobre el contenido en la notificación de icono.

Cuando la aplicación se inicia mediante el Icono dinámico, el sistema devuelve una lista de argumentos desde las notificaciones de iconos actuales/mostradas recientemente.


## <a name="when-to-use-chaseable-tile-notifications"></a>Cuándo usar las notificaciones de iconos rastreables

Las notificaciones de iconos rastreables se usan normalmente al utilizar la cola de notificaciones del Icono dinámico (lo que significa que se realizan ciclos por hasta 5 notificaciones diferentes). También son útiles cuando el contenido del Icono dinámico es probable que no esté sincronizado con el contenido más reciente de la aplicación. Por ejemplo, la aplicación de noticias actualiza su Icono dinámico cada 30minutos, pero cuando se inicia la aplicación, carga las últimas noticias (que es posible que no incluyan algo que estaba en el icono desde el último intervalo de sondeo). Cuando sucede eso, el usuario puede frustrarse por no encontrar la noticia que estaba viendo en su Icono dinámico. Aquí es donde pueden ayudar las notificaciones de iconos rastreables, al permitirte asegurar que lo que el usuario ha visto en su icono sea fácil de encontrar.

## <a name="what-to-do-with-a-chaseable-tile-notifications"></a>Qué hacer con las notificaciones de iconos rastreables

Lo más importante a tener en cuenta es que, en la mayoría de casos, **NO debes navegar directamente a la notificación específica** que estuviera en el icono cuando el usuario hizo clic en él. El Icono dinámico sirve como punto de entrada a la aplicación. Puede haber dos escenarios cuando un usuario hace clic en el Icono dinámico: (1) quería iniciar la aplicación normalmente o (2) quería ver más información sobre una notificación específica que estaba en dicho Icono dinámico. Dado que no hay forma de que el usuario indique el comportamiento que quiere de manera explícita, lo ideal es **iniciar la aplicación normalmente y a la vez asegurarse de que la notificación que ha visto el usuario sea fácil de encontrar**.

Por ejemplo, al hacer clic en el Icono dinámico de la aplicación MSN Noticias, la aplicación se inicia normalmente: muestra la página principal o el artículo que el usuario estuviera leyendo anteriormente. Sin embargo, en la página principal, la aplicación se asegura de que la noticia del Icono dinámico sea fácil de encontrar. De este modo, se admiten ambos casos: el escenario en el que simplemente quieres iniciar o reanudar la aplicación y el escenario en el que deseas ver el artículo específico.


## <a name="how-to-include-the-arguments-property-in-your-tile-notification-payload"></a>Cómo incluir la propiedad Arguments en la carga activa de las notificaciones de iconos

En una carga útil de notificaciones, la propiedad Arguments permite a la aplicación proporcionar datos que puedes usar para identificar la notificación más adelante. Por ejemplo, los argumentos podrían incluir el identificador del artículo, para que cuando se inicie puedas recuperar y mostrar la noticia. La propiedad acepta una cadena, que se puede serializar como se quiera (cadena de consulta, JSON, etc.), pero normalmente recomendamos el formato de cadena de consulta, dado que es ligero y codifica XML bien.

La propiedad se puede establecer en los elementos **TileVisual** y **TileBinding** y se organiza en cascada hacia abajo. Si quieres los mismos argumentos en todos los tamaños de icono, simplemente establece los argumentos en **TileVisual**. Si necesitas argumentos específicos para tamaños de icono concretos, puedes establecer dichos argumentos en elementos **TileBinding** particulares.

Este ejemplo crea una carga de notificaciones que usa la propiedad arguments para que la notificación se pueda identificar más adelante. 

```csharp
// Uses the following NuGet packages
// - Microsoft.Toolkit.Uwp.Notifications
// - QueryString.NET
 
TileContent content = new TileContent()
{
    Visual = new TileVisual()
    {
        // These arguments cascade down to Medium and Wide
        Arguments = new QueryString()
        {
            { "action", "storyClicked" },
            { "story", "201c9b1" }
        }.ToString(),
 
 
        // Medium tile
        TileMedium = new TileBinding()
        {
            Content = new TileBindingContentAdaptive()
            {
                // Omitted
            }
        },
 
 
        // Wide tile is same as Medium
        TileWide = new TileBinding() { /* Omitted */ },
 
 
        // Large tile is an aggregate of multiple stories
        // and therefore needs different arguments
        TileLarge = new TileBinding()
        {
            Arguments = new QueryString()
            {
                { "action", "storiesClicked" },
                { "story", "43f939ag" },
                { "story", "201c9b1" },
                { "story", "d9481ca" }
            }.ToString(),
 
            Content = new TileBindingContentAdaptive() { /* Omitted */ }
        }
    }
};
```


## <a name="how-to-check-for-the-arguments-property-when-your-app-launches"></a>Cómo comprobar la propiedad arguments cuando se inicia la aplicación

La mayoría de aplicaciones tienen un archivo de App.xaml.cs que contiene una invalidación para el método [OnLaunched](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_OnLaunched_Windows_ApplicationModel_Activation_LaunchActivatedEventArgs_). Como sugiere su nombre, la aplicación llama a este método cuando se inicia. Toma un único argumento, un objeto [LaunchActivatedEventArgs](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs).

El objeto LaunchActivatedEventArgs tiene una propiedad que habilita las notificaciones rastreables: la [propiedad TileActivatedInfo](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.TileActivatedInfo), que proporciona acceso a un [objeto TileActivatedInfo](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.tileactivatedinfo). Cuando el usuario inicia la aplicación desde su icono (en lugar de desde la lista de aplicaciones, una búsqueda o cualquier otro punto de entrada), la aplicación inicializa esta propiedad.

El [objeto TileActivatedInfo](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.tileactivatedinfo) contiene una propiedad llamada [RecentlyShownNotifications](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.tileactivatedinfo.RecentlyShownNotifications), que contiene una lista de las notificaciones que se han mostrado en el icono durante de los últimos 15minutos. El primer elemento de la lista de representa la notificación que hay actualmente en el icono, y los elementos siguientes representan las notificaciones que el usuario ha visto antes de la actual. Si el icono se ha borrado, la lista está vacía.

Cada una Argumentsproperty ShownTileNotificationhas. El Argumentsproperty será inicializado con el argumentsstring de la carga de notificaciones de icono o null si la carga no incluía la argumentsstring.

```csharp
protected override void OnLaunched(LaunchActivatedEventArgs args)
{
    // If the API is present (doesn't exist on 10240 and 10586)
    if (ApiInformation.IsPropertyPresent(typeof(LaunchActivatedEventArgs).FullName, nameof(LaunchActivatedEventArgs.TileActivatedInfo)))
    {
        // If clicked on from tile
        if (args.TileActivatedInfo != null)
        {
            // If tile notification(s) were present
            if (args.TileActivatedInfo.RecentlyShownNotifications.Count > 0)
            {
                // Get arguments from the notifications that were recently displayed
                string[] allArgs = args.TileActivatedInfo.RecentlyShownNotifications
                .Select(i => i.Arguments)
                .ToArray();
 
                // TODO: Highlight each story in the app
            }
        }
    }
 
    // TODO: Initialize app
}
```


## <a name="raw-xml-example"></a>Ejemplo de XML sin formato

Si usas XML sin formato en lugar de la biblioteca de notificaciones, este es el código XML.

```xml
<tile>
  <visual arguments="action=storyClicked&amp;story=201c9b1">
 
    <binding template="TileMedium">
       
      <text>Kitten learns how to drive a car...</text>
      ... (omitted)
     
    </binding>
 
    <binding template="TileWide">
      ... (same as Medium)
    </binding>
     
    <!--Large tile is an aggregate of multiple stories-->
    <binding
      template="TileLarge"
      arguments="action=storiesClicked&amp;story=43f939ag&amp;story=201c9b1&amp;story=d9481ca">
   
      <text>Can your dog understand what you're saying?</text>
      ... (another story)
      ... (one more story)
   
    </binding>
 
  </visual>
</tile>
```



## <a name="related-articles"></a>Artículos relacionados

- [Propiedad LaunchActivatedEventArgs.TileActivatedInfo](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs#Windows_ApplicationModel_Activation_LaunchActivatedEventArgs_TileActivatedInfo_)
- [Clase TileActivatedInfo](https://docs.microsoft.com/uwp/api/windows.applicationmodel.activation.tileactivatedinfo)