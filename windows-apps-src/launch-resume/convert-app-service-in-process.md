---
author: TylerMSFT
title: "Convertir un servicio de aplicaciones para que se ejecute en el mismo proceso que su aplicación host"
description: "Puedes convertir el código de servicio de aplicaciones que se ejecutaba en un proceso en segundo plano independiente en código que se ejecute en el mismo proceso que el proveedor de servicios de aplicaciones."
translationtype: Human Translation
ms.sourcegitcommit: 7d1c160f8b725cd848bf8357325c6ca284b632ae
ms.openlocfilehash: 80402d12a51ea970f5927dc50cae587be8809b87

---

# Convertir un servicio de aplicaciones para que se ejecute en el mismo proceso que su aplicación host

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



<!--HONumber=Nov16_HO1-->

