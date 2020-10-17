---
Description: Use las notificaciones de icono de seguimiento para averiguar lo que la aplicación muestra en su icono dinámico cuando el usuario hace clic en ella.
title: Notificaciones de iconos rastreables
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Chaseable tile notifications
template: detail.hbs
ms.date: 06/13/2017
ms.topic: article
keywords: Windows 10, UWP, mosaicos de seguimiento, mosaicos dinámicos, notificaciones de icono de seguimiento
ms.localizationpriority: medium
ms.openlocfilehash: 951dc891fb34ae4be7551c08ff47eabc19ae9eb6
ms.sourcegitcommit: c5df8832e9df8749d0c3eee9e85f4c2d04f8b27b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2020
ms.locfileid: "92100283"
---
# <a name="chaseable-tile-notifications"></a>Notificaciones de iconos rastreables

Las notificaciones de icono de seguimiento le permiten determinar qué notificaciones de icono se muestran en el icono dinámico de la aplicación cuando el usuario hizo clic en el icono.  
Por ejemplo, una aplicación de noticias podría usar esta característica para determinar en qué noticia se mostraba el icono dinámico cuando el usuario lo inició; podría asegurarse de que la historia se muestre de forma destacada para que el usuario pueda encontrarla. 

> [!IMPORTANT]
> **Requiere actualización de aniversario**: para usar notificaciones de icono de seguimiento con aplicaciones de UWP basadas en C#, C++ o VB, debe tener como destino el SDK 14393 y ejecutar la compilación 14393 o posterior. En el caso de las aplicaciones para UWP basadas en JavaScript, debe tener como destino el SDK 17134 y ejecutar la compilación 17134 o posterior. 


> **API importantes**: [propiedad LaunchActivatedEventArgs. TileActivatedInfo](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.TileActivatedInfo), [clase TileActivatedInfo](/uwp/api/windows.applicationmodel.activation.tileactivatedinfo)


## <a name="how-it-works"></a>Cómo funciona

Para habilitar las notificaciones de icono de seguimiento, use la propiedad **arguments** en la carga de notificación de icono, similar a la propiedad Launch en la carga de notificación del sistema, para insertar información sobre el contenido en la notificación de icono.

Cuando la aplicación se inicia mediante el icono dinámico, el sistema devuelve una lista de argumentos de las notificaciones de icono actual o recientemente mostradas.


## <a name="when-to-use-chaseable-tile-notifications"></a>Cuándo usar notificaciones de icono con seguimiento

Las notificaciones de icono de seguimiento se suelen usar cuando se usa la cola de notificación en el icono dinámico (lo que significa que está pasando por un máximo de 5 notificaciones diferentes). También resultan útiles cuando el contenido del icono dinámico puede no estar sincronizado con el contenido más reciente de la aplicación. Por ejemplo, la aplicación de noticias actualiza su icono dinámico cada 30 minutos, pero cuando se inicia la aplicación, carga las últimas noticias (lo que puede no incluir algo que estaba en el icono del último intervalo de sondeo). Cuando esto sucede, es posible que el usuario se sienta frustrado al no poder encontrar el caso que vio en su icono dinámico. Aquí es donde las notificaciones de icono de persecución pueden ayudar, permitiéndole asegurarse de que el usuario que vio en su icono es fácilmente reconocible.

## <a name="what-to-do-with-a-chaseable-tile-notifications"></a>Qué hacer con las notificaciones de un icono de seguimiento

Lo más importante que hay que tener en cuenta es que en la mayoría de los escenarios **no debe navegar directamente a la notificación específica** que estaba en el icono cuando el usuario hizo clic en él. El icono dinámico se usa como punto de entrada para la aplicación. Puede haber dos escenarios en los que un usuario haga clic en el icono dinámico: (1) deseaba iniciar la aplicación normalmente, o (2) quería ver más información sobre una notificación específica que estaba en el icono dinámico. Dado que no hay ninguna manera de que el usuario indique explícitamente qué comportamiento quieren, la experiencia ideal es **iniciar la aplicación normalmente, asegurándose de que la notificación que vio el usuario es fácilmente reconocible**.

Por ejemplo, al hacer clic en el icono dinámico de la aplicación MSN News se inicia la aplicación normalmente: se muestra la Página principal o el artículo que el usuario ha leído anteriormente. Sin embargo, en la Página principal, la aplicación garantiza que la historia del icono dinámico sea fácilmente reconocible. De este modo, se admiten ambos escenarios: el escenario en el que solo quiere iniciar o reanudar la aplicación y el escenario en el que desea ver la historia específica.


## <a name="how-to-include-the-arguments-property-in-your-tile-notification-payload"></a>Cómo incluir la propiedad arguments en la carga de notificación de icono

En una carga de notificación, la propiedad arguments permite que la aplicación proporcione datos que puede usar para identificar posteriormente la notificación. Por ejemplo, los argumentos podrían incluir el identificador del caso, de modo que cuando se inicie, pueda recuperar y mostrar el caso. La propiedad acepta una cadena, que se puede serializar como se desea (cadena de consulta, JSON, etc.), pero normalmente se recomienda el formato de cadena de consulta, ya que es ligero y codifica XML de manera excelente.

La propiedad se puede establecer en los elementos **TileVisual** y **TileBinding** , y se pondrá en cascada. Si desea los mismos argumentos en cada tamaño de mosaico, simplemente establezca los argumentos en el **TileVisual**. Si necesita argumentos específicos para tamaños de icono específicos, puede establecer los argumentos en elementos **TileBinding** individuales.

En este ejemplo se crea una carga de notificación que usa la propiedad arguments para que la notificación se pueda identificar más adelante. 

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

La mayoría de las aplicaciones tienen un archivo App.xaml.cs que contiene una invalidación para el método [onlaunched](/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_OnLaunched_Windows_ApplicationModel_Activation_LaunchActivatedEventArgs_) . Como su nombre sugiere, la aplicación llama a este método cuando se inicia. Toma un único argumento, un objeto [LaunchActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs) .

El objeto LaunchActivatedEventArgs tiene una propiedad que habilita las notificaciones de seguimiento: la [propiedad TileActivatedInfo](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs.TileActivatedInfo), que proporciona acceso a un [objeto TileActivatedInfo](/uwp/api/windows.applicationmodel.activation.tileactivatedinfo). Cuando el usuario inicia la aplicación desde su icono (en lugar de la lista de aplicaciones, la búsqueda o cualquier otro punto de entrada), la aplicación inicializa esta propiedad.

El [objeto TileActivatedInfo](/uwp/api/windows.applicationmodel.activation.tileactivatedinfo) contiene una propiedad denominada [RecentlyShownNotifications](/uwp/api/windows.applicationmodel.activation.tileactivatedinfo.RecentlyShownNotifications), que contiene una lista de notificaciones que se han mostrado en el icono en los últimos 15 minutos. El primer elemento de la lista representa la notificación que se encuentra actualmente en el icono y los elementos siguientes representan las notificaciones que el usuario vio antes de la actual. Si el icono se ha borrado, esta lista está vacía.

Cada ShownTileNotification tiene una propiedad arguments. La propiedad arguments se inicializará con la cadena arguments de la carga de notificación del icono, o null si la carga no incluye la cadena arguments.

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


### <a name="accessing-onlaunched-from-desktop-applications"></a>Acceso a Onlaunched desde aplicaciones de escritorio

Las aplicaciones de escritorio (por ejemplo, WPF, etc.) que usan el [puente de escritorio](https://developer.microsoft.com/windows/bridges/desktop), también pueden usar mosaicos de seguimiento. La única diferencia es el acceso a los argumentos Onlaunched. Tenga en cuenta que primero debe [empaquetar la aplicación con el puente de escritorio](/windows/msix/desktop/source-code-overview).

> [!IMPORTANT]
> **Requiere la actualización de octubre de 2018**: para usar la `AppInstance.GetActivatedEventArgs()` API, debe tener como destino el SDK 17763 y ejecutar la compilación 17763 o posterior.

En el caso de las aplicaciones de escritorio, para tener acceso a los argumentos de inicio, haga lo siguiente...

```csharp

static void Main()
{
    Application.EnableVisualStyles();
    Application.SetCompatibleTextRenderingDefault(false);

    // API only available on build 17763 or higher
    var args = AppInstance.GetActivatedEventArgs();
    switch (args.Kind)
    {
        case ActivationKind.Launch:

            var launchArgs = args as LaunchActivatedEventArgs;

            // If clicked on from tile
            if (launchArgs.TileActivatedInfo != null)
            {
                // If tile notification(s) were present
                if (launchArgs.TileActivatedInfo.RecentlyShownNotifications.Count > 0)
                {
                    // Get arguments from the notifications that were recently displayed
                    string[] allTileArgs = launchArgs.TileActivatedInfo.RecentlyShownNotifications
                    .Select(i => i.Arguments)
                    .ToArray();
     
                    // TODO: Highlight each story in the app
                }
            }
    
            break;
```


## <a name="raw-xml-example"></a>Ejemplo de XML sin formato

Si utiliza XML sin formato en lugar de la biblioteca de notificaciones, este es el código XML.

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

- [Propiedad LaunchActivatedEventArgs. TileActivatedInfo](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs#Windows_ApplicationModel_Activation_LaunchActivatedEventArgs_TileActivatedInfo_)
- [Clase TileActivatedInfo](/uwp/api/windows.applicationmodel.activation.tileactivatedinfo)