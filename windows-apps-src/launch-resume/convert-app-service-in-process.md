---
author: TylerMSFT
title: "Convertir un servicio de aplicaciones para que se ejecute en el mismo proceso que su aplicación host"
description: "Puedes convertir el código de servicio de aplicaciones que se ejecutaba en un proceso en segundo plano independiente en un código que se ejecute en el mismo proceso que el proveedor de servicios de aplicaciones."
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.assetid: 30aef94b-1b83-4897-a2f1-afbb4349696a
ms.openlocfilehash: 015a37c8d85174e6494690a67b4dc23b1cce5712
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="convert-an-app-service-to-run-in-the-same-process-as-its-host-app"></a>Convertir un servicio de aplicaciones para que se ejecute en el mismo proceso que su aplicación host

Un [AppServiceConnection](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.appservice.appserviceconnection.aspx) permite que otra aplicación reactive la aplicación en segundo plano e inicie una línea directa de comunicación con él.

Con la introducción App Services dentro del proceso, dos aplicaciones en ejecución en primer plano pueden tener una línea directa de comunicación a través de una conexión de App Services. App Services ahora puede ejecutarse en el mismo proceso que la aplicación en primer plano, lo que hace que la comunicación entre aplicaciones resulte mucho más sencilla y elimina la necesidad de separar el código de servicio en un proyecto independiente.

La conversión de un App Service de modelo fuera del proceso a un modelo dentro del proceso requiere dos cambios. El primero es un cambio de manifiesto.

> ```xml
>  <uap:Extension Category="windows.appService">
>          <uap:AppService Name="InProcessAppService" />
>  </uap:Extension>
> ```

Quita el atributo `EntryPoint`. Ahora, la devolución de llamada [OnBackgroundActivated()](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.application.onbackgroundactivated.aspx) se usará como método de devolución de llamada cuando se invoque el servicio de aplicaciones.

El segundo cambio consiste en mover la lógica de servicio de su proyecto de tarea en segundo plano independiente a métodos que se puedan llamar desde **OnBackgroundActivated()**.

Ahora, tu aplicación puede ejecutar directamente el Servicio de aplicaciones.  Por ejemplo:

> ``` cs
> private AppServiceConnection appServiceconnection;
> private BackgroundTaskDeferral appServiceDeferral;
> protected override async void OnBackgroundActivated(BackgroundActivatedEventArgs args)
> {
>     base.OnBackgroundActivated(args);
>     IBackgroundTaskInstance taskInstance = args.TaskInstance;
>     AppServiceTriggerDetails appService = taskInstance.TriggerDetails as AppServiceTriggerDetails;
>     appServiceDeferral = taskInstance.GetDeferral();
>     taskInstance.Canceled += OnAppServicesCanceled;
>     appServiceConnection = appService.AppServiceConnection;
>     appServiceConnection.RequestReceived += OnAppServiceRequestReceived;
>     appServiceConnection.ServiceClosed += AppServiceConnection_ServiceClosed;
> }
>
> private async void OnAppServiceRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
> {
>     AppServiceDeferral messageDeferral = args.GetDeferral();
>     ValueSet message = args.Request.Message;
>     string text = message["Request"] as string;
>              
>     if ("Value" == text)
>     {
>         ValueSet returnMessage = new ValueSet();
>         returnMessage.Add("Response", "True");
>         await args.Request.SendResponseAsync(returnMessage);
>     }
>     messageDeferral.Complete();
> }
>
> private void OnAppServicesCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
> {
>     appServiceDeferral.Complete();
> }
>
> private void AppServiceConnection_ServiceClosed(AppServiceConnection sender, AppServiceClosedEventArgs args)
> {
>     appServiceDeferral.Complete();
> }
> ```

En el código anterior, el método `OnBackgroundActivated` controla la activación del servicio de aplicaciones. Se registran todos los eventos necesarios para la comunicación a través de [AppServiceConnection](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.appservice.appserviceconnection.aspx) y se almacena el objeto de aplazamiento de la tarea para que se pueda marcar como completado cuando haya finalizado la comunicación entre las aplicaciones.

Cuando la aplicación recibe una solicitud y lee la clase [ValueSet](https://msdn.microsoft.com/library/windows/apps/windows.foundation.collections.valueset.aspx) proporcionada para ver si las cadenas `Key` y `Value` están presentes. Si están presentes, a continuación, el servicio de aplicaciones devuelve un par de valores de cadena `Response` y `True` de nuevo a la aplicación en el otro lado de **AppServiceConnection**.

Obtén más información sobre la conexión y la comunicación con otras aplicaciones en [Crear y consumir un servicio de aplicación](https://msdn.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service?f=255&MSPPError=-2147217396).
