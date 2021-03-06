---
title: Convertir un servicio de aplicaciones para que se ejecute en el mismo proceso que su aplicación host
description: Puedes convertir el código de servicio de aplicaciones que se ejecutaba en un proceso en segundo plano independiente en un código que se ejecute en el mismo proceso que el proveedor de servicios de aplicaciones.
ms.date: 11/03/2017
ms.topic: article
keywords: Windows 10, UWP, App Service
ms.assetid: 30aef94b-1b83-4897-a2f1-afbb4349696a
ms.localizationpriority: medium
ms.openlocfilehash: d1bdd8b72cafe3dd719f7ee1733e13e1531f8c7e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162719"
---
# <a name="convert-an-app-service-to-run-in-the-same-process-as-its-host-app"></a>Convertir un servicio de aplicaciones para que se ejecute en el mismo proceso que su aplicación host

Un [AppServiceConnection](/uwp/api/windows.applicationmodel.appservice.appserviceconnection) permite que otra aplicación reactive la aplicación en segundo plano e inicie una línea directa de comunicación con él.

Con la introducción App Services dentro del proceso, dos aplicaciones en ejecución en primer plano pueden tener una línea directa de comunicación a través de una conexión de App Services. App Services ahora puede ejecutarse en el mismo proceso que la aplicación en primer plano, lo que hace que la comunicación entre aplicaciones resulte mucho más sencilla y elimina la necesidad de separar el código de servicio en un proyecto independiente.

La conversión de un App Service de modelo fuera del proceso a un modelo dentro del proceso requiere dos cambios. El primero es un cambio de manifiesto.

> ```xml
> <Package
>    ...
>   <Applications>
>       <Application Id=...
>           ...
>           EntryPoint="...">
>           <Extensions>
>               <uap:Extension Category="windows.appService">
>                   <uap:AppService Name="InProcessAppService" />
>               </uap:Extension>
>           </Extensions>
>           ...
>       </Application>
>   </Applications>
> ```

Quite el `EntryPoint` atributo del `<Extension>` elemento porque Now [OnBackgroundActivated ()](/uwp/api/windows.ui.xaml.application.onbackgroundactivated) es el punto de entrada que se usará cuando se invoque App Service.

El segundo cambio consiste en mover la lógica de servicio de su proyecto de tarea en segundo plano independiente a métodos que se puedan llamar desde **OnBackgroundActivated()**.

Ahora, tu aplicación puede ejecutar directamente el Servicio de aplicaciones. Por ejemplo, en App.xaml.cs:

[!NOTE] El código siguiente es diferente del proporcionado por el ejemplo 1 (servicio fuera de proceso). El código siguiente se proporciona solo con fines ilustrativos y no debe usarse como parte del ejemplo 2 (servicio in-Process).  Para continuar la transición del artículo del ejemplo 1 (servicio fuera de proceso) al ejemplo 2 (servicio en proceso), siga usando el código proporcionado por el ejemplo 1 en lugar del código ilustrativo a continuación.

``` cs
using Windows.ApplicationModel.AppService;
using Windows.ApplicationModel.Background;
...

sealed partial class App : Application
{
  private AppServiceConnection _appServiceConnection;
  private BackgroundTaskDeferral _appServiceDeferral;

  ...

  protected override void OnBackgroundActivated(BackgroundActivatedEventArgs args)
  {
      base.OnBackgroundActivated(args);
      IBackgroundTaskInstance taskInstance = args.TaskInstance;
      AppServiceTriggerDetails appService = taskInstance.TriggerDetails as AppServiceTriggerDetails;
      _appServiceDeferral = taskInstance.GetDeferral();
      taskInstance.Canceled += OnAppServicesCanceled;
      _appServiceConnection = appService.AppServiceConnection;
      _appServiceConnection.RequestReceived += OnAppServiceRequestReceived;
      _appServiceConnection.ServiceClosed += AppServiceConnection_ServiceClosed;
  }

  private async void OnAppServiceRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
  {
      AppServiceDeferral messageDeferral = args.GetDeferral();
      ValueSet message = args.Request.Message;
      string text = message["Request"] as string;

      if ("Value" == text)
      {
          ValueSet returnMessage = new ValueSet();
          returnMessage.Add("Response", "True");
          await args.Request.SendResponseAsync(returnMessage);
      }
      messageDeferral.Complete();
  }

  private void OnAppServicesCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
  {
      _appServiceDeferral.Complete();
  }

  private void AppServiceConnection_ServiceClosed(AppServiceConnection sender, AppServiceClosedEventArgs args)
  {
      _appServiceDeferral.Complete();
  }
}
```

En el código anterior, el método `OnBackgroundActivated` controla la activación del servicio de aplicaciones. Se registran todos los eventos necesarios para la comunicación a través de [AppServiceConnection](/uwp/api/windows.applicationmodel.appservice.appserviceconnection) y se almacena el objeto de aplazamiento de la tarea para que se pueda marcar como completado cuando haya finalizado la comunicación entre las aplicaciones.

Cuando la aplicación recibe una solicitud y lee la clase [ValueSet](/uwp/api/windows.foundation.collections.valueset) proporcionada para ver si las cadenas `Key` y `Value` están presentes. Si están presentes, a continuación, el servicio de aplicaciones devuelve un par de valores de cadena `Response` y `True` de nuevo a la aplicación en el otro lado de **AppServiceConnection**.

Obtén más información sobre la conexión y la comunicación con otras aplicaciones en [Crear y consumir un servicio de aplicación](./how-to-create-and-consume-an-app-service.md?f=255&MSPPError=-2147217396).