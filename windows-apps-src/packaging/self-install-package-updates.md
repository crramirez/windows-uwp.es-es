---
ms.assetid: 414ACC73-2A72-465C-BD15-1B51CB2334F2
title: Descargar e instalar actualizaciones de paquetes desde Store
description: Obtén información sobre cómo marcar paquetes como obligatorios en el Centro de partners y cómo escribir código en la aplicación para descargar e instalar actualizaciones de paquetes.
ms.date: 04/04/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bec08dfa19f0adab12d6337c00e19bd98459bf3a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164339"
---
# <a name="download-and-install-package-updates-from-the-store"></a>Descargar e instalar actualizaciones de paquetes desde Store

A partir de Windows 10, versión 1607, puedes usar los métodos de la clase [StoreContext](/uwp/api/windows.services.store.storecontext) en el espacio de nombres [Windows.Services.Store](/uwp/api/windows.services.store) para comprobar si hay actualizaciones del paquete de la aplicación actual en Microsoft Store, y descargar e instalar los paquetes actualizados. También puedes consultar los paquetes que has marcado como obligatorios en el Centro de partners y deshabilitar la funcionalidad de la aplicación hasta que se instale la actualización obligatoria.

Los métodos [StoreContext](/uwp/api/windows.services.store.storecontext) adicionales introducidos en Windows 10 versión 1803 permiten descargar e instalar actualizaciones del paquete de forma silenciosa (sin mostrar al usuario una notificación de la interfaz de usuario), desinstalar un [paquete opcional](/windows/msix/package/optional-packages) y obtener información sobre los paquetes en la cola de descarga e instalación de tu aplicación.

Estas características te ayudan a mantener tu base de usuarios actualizada con la última versión de la aplicación, paquetes opcionales y los servicios relacionados de manera automática en Microsoft Store.

## <a name="download-and-install-package-updates-with-the-users-permission"></a>Descargar e instalar actualizaciones de paquete con el permiso del usuario

Este ejemplo de código muestra cómo usar el método [GetAppAndOptionalStorePackageUpdatesAsync](/uwp/api/windows.services.store.storecontext.getappandoptionalstorepackageupdatesasync) para detectar todas las actualizaciones disponibles de paquete de Microsoft Store y, a continuación, llamar al método [RequestDownloadAndInstallStorePackageUpdatesAsync](/uwp/api/windows.services.store.storecontext.requestdownloadandinstallstorepackageupdatesasync) para descargar e instalar las actualizaciones. Al usar este método para descargar e instalar actualizaciones, el sistema operativo muestra un cuadro de diálogo que solicita el permiso del usuario antes de descargar las actualizaciones.

> [!NOTE]
> Estos métodos admiten [paquetes opcionales](/windows/msix/package/optional-packages) y necesarios para tu aplicación. Los paquetes opcionales son útiles para complementos de contenido descargable (DLC), y divide una aplicación grande por restricciones de tamaño o para enviar contenido adicional aparte de la aplicación principal. Para obtener el permiso necesario para enviar una aplicación que usa paquetes opcionales (incluidos complementos de DLC) a Microsoft Store, consulta [Soporte técnico para desarrolladores de Windows](https://developer.microsoft.com/windows/support).

En este ejemplo de código se da por supuesto que:

* El código se ejecuta en el contexto de un elemento [Page](/uwp/api/windows.ui.xaml.controls.page).
* El elemento **Page** contiene un elemento [ProgressBar](/uwp/api/windows.ui.xaml.controls.progressbar) denominado ```downloadProgressBar``` para proporcionar el estado de la operación de descarga.
* El archivo de código tiene una instrucción **using** para los espacios de nombres **Windows.Services.Store**, **Windows.Threading.Tasks** y **Windows.UI.Popups**.
* La aplicación es una de usuario único que se ejecuta solamente en el contexto del usuario que la inició. Para una [aplicación multiusuario](../xbox-apps/multi-user-applications.md), usa el método [GetForUser](/uwp/api/windows.services.store.storecontext.User) para obtener un objeto **StoreContext** en lugar del método [GetDefault](/uwp/api/windows.services.store.storecontext.GetDefault).

```csharp
private StoreContext context = null;

public async Task DownloadAndInstallAllUpdatesAsync()
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }

    // Get the updates that are available.
    IReadOnlyList<StorePackageUpdate> updates =
        await context.GetAppAndOptionalStorePackageUpdatesAsync();

    if (updates.Count > 0)
    {
        // Alert the user that updates are available and ask for their consent
        // to start the updates.
        MessageDialog dialog = new MessageDialog(
            "Download and install updates now? This may cause the application to exit.", "Download and Install?");
        dialog.Commands.Add(new UICommand("Yes"));
        dialog.Commands.Add(new UICommand("No"));
        IUICommand command = await dialog.ShowAsync();

        if (command.Label.Equals("Yes", StringComparison.CurrentCultureIgnoreCase))
        {
            // Download and install the updates.
            IAsyncOperationWithProgress<StorePackageUpdateResult, StorePackageUpdateStatus> downloadOperation =
                context.RequestDownloadAndInstallStorePackageUpdatesAsync(updates);

            // The Progress async method is called one time for each step in the download
            // and installation process for each package in this request.
            downloadOperation.Progress = async (asyncInfo, progress) =>
            {
                await this.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
                () =>
                {
                    downloadProgressBar.Value = progress.PackageDownloadProgress;
                });
            };

            StorePackageUpdateResult result = await downloadOperation.AsTask();
        }
    }
}
```

> [!NOTE]
> Para descargar solo (sin instalar) las actualizaciones de paquetes disponibles, usa el método [RequestDownloadStorePackageUpdatesAsync](/uwp/api/windows.services.store.storecontext.requestdownloadstorepackageupdatesasync).

### <a name="display-download-and-install-progress-info"></a>Mostrar información de progreso de la descarga e instalación

Cuando llames al método [RequestDownloadStorePackageUpdatesAsync](/uwp/api/windows.services.store.storecontext.requestdownloadstorepackageupdatesasync) o [RequestDownloadAndInstallStorePackageUpdatesAsync](/uwp/api/windows.services.store.storecontext.requestdownloadandinstallstorepackageupdatesasync), puedes asignar un controlador [Progress](/uwp/api/windows.foundation.iasyncoperationwithprogress-2.progress) que se llama una vez para cada paso del proceso de la descarga (o descarga e instalación) para cada paquete de la solicitud. El controlador recibe un objeto [StorePackageUpdateStatus](/uwp/api/windows.services.store.storepackageupdatestatus) que proporciona información sobre el paquete de actualización que genera la notificación de progreso. En el ejemplo anterior se utiliza el campo **PackageDownloadProgress** del objeto **StorePackageUpdateStatus** para mostrar el progreso del proceso de descarga e instalación.

Ten en cuenta que, al llamar al método **RequestDownloadAndInstallStorePackageUpdatesAsync** para descargar e instalar actualizaciones del paquete en una sola operación, el campo **PackageDownloadProgress** aumenta de 0,0 a 0,8 durante el proceso de descarga para un paquete y luego aumenta de 0,8 a 1,0 durante la instalación. Por lo tanto, si asignas el porcentaje que se muestra en la interfaz de usuario de progreso personalizada directamente en el valor del campo **PackageDownloadProgress**, la interfaz de usuario mostrará 80 % cuando el paquete se haya descargado por completo y el sistema operativo muestra el cuadro de diálogo de instalación. Si quieres que tu interfaz de usuario de progreso personalizada muestre 100 % cuando el paquete se haya descargado y esté listo para instalarse, puedes modificar el código para asignar el valor de 100 % a la interfaz de usuario de progreso cuando el campo **PackageDownloadProgress** alcance 0,8.

## <a name="download-and-install-package-updates-silently"></a>Descargar e instalar actualizaciones de paquete de forma silenciosa

A partir de Windows 10, versión 1803, puedes usar los métodos [TrySilentDownloadStorePackageUpdatesAsync](/uwp/api/windows.services.store.storecontext.trysilentdownloadstorepackageupdatesasync) y [TrySilentDownloadAndInstallStorePackageUpdatesAsync](/uwp/api/windows.services.store.storecontext.trysilentdownloadandinstallstorepackageupdatesasync) para descargar e instalar actualizaciones de paquetes de forma silenciosa, sin mostrar una interfaz de usuario de notificación al usuario. Esta operación se ejecutará correctamente solo si el usuario ha habilitado la opción **Actualizar aplicaciones automáticamente** en Microsoft Store y el usuario no está en una red de uso medido. Antes de llamar a estos métodos, puedes comprobar la propiedad [CanSilentlyDownloadStorePackageUpdates](/uwp/api/windows.services.store.storecontext.cansilentlydownloadstorepackageupdates) para determinar si actualmente se cumplen estas condiciones.

Este ejemplo de código muestra cómo usar el método [GetAppAndOptionalStorePackageUpdatesAsync](/uwp/api/windows.services.store.storecontext.getappandoptionalstorepackageupdatesasync) para detectar todas las actualizaciones de paquetes disponibles y, a continuación, llamar a los métodos [TrySilentDownloadStorePackageUpdatesAsync](/uwp/api/windows.services.store.storecontext.trysilentdownloadstorepackageupdatesasync) y [TrySilentDownloadAndInstallStorePackageUpdatesAsync](/uwp/api/windows.services.store.storecontext.trysilentdownloadandinstallstorepackageupdatesasync) para descargar e instalar las actualizaciones de forma silenciosa.

En este ejemplo de código se da por supuesto que:
* El archivo de código tiene una instrucción **using** para los espacios de nombres **Windows.Services.Store** y **System.Threading.Tasks**.
* La aplicación es una de usuario único que se ejecuta solamente en el contexto del usuario que la inició. Para una [aplicación multiusuario](../xbox-apps/multi-user-applications.md), usa el método [GetForUser](/uwp/api/windows.services.store.storecontext.User) para obtener un objeto **StoreContext** en lugar del método [GetDefault](/uwp/api/windows.services.store.storecontext.GetDefault).

> [!NOTE]
> Los métodos **IsNowAGoodTimeToRestartApp**, **RetryDownloadAndInstallLater** y **RetryInstallLater** a los que llama el código de este ejemplo son métodos de marcador de posición y se implementarán según sea necesario en función del diseño de tu aplicación.

```csharp
private StoreContext context = null;

public async Task DownloadAndInstallAllUpdatesInBackgroundAsync()
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }

    // Get the updates that are available.
    IReadOnlyList<StorePackageUpdate> storePackageUpdates =
        await context.GetAppAndOptionalStorePackageUpdatesAsync();

    if (storePackageUpdates.Count > 0)
    {

        if (!context.CanSilentlyDownloadStorePackageUpdates)
        {
            return;
        }

        // Start the silent downloads and wait for the downloads to complete.
        StorePackageUpdateResult downloadResult =
            await context.TrySilentDownloadStorePackageUpdatesAsync(storePackageUpdates);

        switch (downloadResult.OverallState)
        {
            case StorePackageUpdateState.Completed:
                // The download has completed successfully. At this point, confirm whether your app
                // can restart now and then install the updates (for example, you might only install
                // packages silently if your app has been idle for a certain period of time). The
                // IsNowAGoodTimeToRestartApp method is not implemented in this example, you should
                // implement it as needed for your own app.
                if (IsNowAGoodTimeToRestartApp())
                {
                    await InstallUpdate(storePackageUpdates);
                }
                else
                {
                    // Retry/reschedule the installation later. The RetryInstallLater method is not  
                    // implemented in this example, you should implement it as needed for your own app.
                    RetryInstallLater();
                    return;
                }
                break;
            // If the user cancelled the download or you can't perform the download for some other
            // reason (for example, Wi-Fi might have been turned off and the device is now on
            // a metered network) try again later. The RetryDownloadAndInstallLater method is not  
            // implemented in this example, you should implement it as needed for your own app.
            case StorePackageUpdateState.Canceled:
            case StorePackageUpdateState.ErrorLowBattery:
            case StorePackageUpdateState.ErrorWiFiRecommended:
            case StorePackageUpdateState.ErrorWiFiRequired:
            case StorePackageUpdateState.OtherError:
                RetryDownloadAndInstallLater();
                return;
            default:
                break;
        }
    }
}

private async Task InstallUpdate(IReadOnlyList<StorePackageUpdate> storePackageUpdates)
{
    // Start the silent installation of the packages. Because the packages have already
    // been downloaded in the previous method, the following line of code just installs
    // the downloaded packages.
    StorePackageUpdateResult downloadResult =
        await context.TrySilentDownloadAndInstallStorePackageUpdatesAsync(storePackageUpdates);

    switch (downloadResult.OverallState)
    {
        // If the user cancelled the installation or you can't perform the installation  
        // for some other reason, try again later. The RetryInstallLater method is not  
        // implemented in this example, you should implement it as needed for your own app.
        case StorePackageUpdateState.Canceled:
        case StorePackageUpdateState.ErrorLowBattery:
        case StorePackageUpdateState.OtherError:
            RetryInstallLater();
            return;
        default:
            break;
    }
}
```

## <a name="mandatory-package-updates"></a>Actualizaciones de paquete obligatorias

Al crear un envío de paquetes en el Centro de partners para una aplicación para Windows 10 versión 1607 o posteriores, puedes [marcar el paquete como obligatorio](../publish/upload-app-packages.md#mandatory-update), y la fecha y hora en que se convierte en obligatorio. Cuando se establece esta propiedad y la aplicación detecta que la actualización del paquete está disponible, la aplicación puede determinar si el paquete de actualización es obligatorio y modificar su comportamiento hasta que se instale la actualización (por ejemplo, la aplicación puede deshabilitar características).

> [!NOTE]
> El estado obligatorio de una actualización de paquete no lo exige Microsoft, y el sistema operativo no proporciona ninguna interfaz de usuario para indicar a los usuarios que deben instalar una actualización de la aplicación obligatoria. Se pretende que los desarrolladores usen la opción "mandatory" para aplicar las actualizaciones de la aplicación obligatorias en su propio código.  

Para marcar un envío de paquete como obligatorio:

1. Inicia sesión en el [Centro de partners](https://partner.microsoft.com/dashboard) y ve hasta la página de información general de la aplicación.
2. Haz clic en el nombre de la presentación que contiene la actualización de paquete que quieres que sea obligatoria.
3. Navega a la página **Paquetes** del envío. En la parte inferior de esta página, selecciona **Hacer que esta actualización sea obligatoria** y, a continuación, elige el día y la hora en que la actualización de paquete es obligatoria. Esta opción se aplica a todos los paquetes para UWP en el envío.

Para más información, consulta [Cargar paquetes de aplicaciones](../publish/upload-app-packages.md).

> [!NOTE]
> Si creas un [paquete piloto](../publish/package-flights.md), puedes marcar los paquetes como obligatorios con una interfaz de usuario similar en la página **Paquetes** del piloto. En este caso, la actualización del paquete obligatoria se aplica solo a los clientes que forman parte del grupo piloto.

### <a name="code-example-for-mandatory-packages"></a>Ejemplo de código para paquetes obligatorios

El siguiente ejemplo de código muestra cómo determinar si los paquetes de actualizaciones son obligatorios. Por lo general, debes degradar la experiencia de tu aplicación correctamente para el usuario en caso de que una actualización de paquete obligatoria no se descargue o instale correctamente.

```csharp
private StoreContext context = null;

// Downloads and installs package updates in separate steps.
public async Task DownloadAndInstallAllUpdatesAsync()
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }  

    // Get the updates that are available.
    IReadOnlyList<StorePackageUpdate> updates =
        await context.GetAppAndOptionalStorePackageUpdatesAsync();

    if (updates.Count != 0)
    {
        // Download the packages.
        bool downloaded = await DownloadPackageUpdatesAsync(updates);

        if (downloaded)
        {
            // Install the packages.
            await InstallPackageUpdatesAsync(updates);
        }
    }
}

// Helper method for downloading package updates.
private async Task<bool> DownloadPackageUpdatesAsync(IEnumerable<StorePackageUpdate> updates)
{
    bool downloadedSuccessfully = false;

    IAsyncOperationWithProgress<StorePackageUpdateResult, StorePackageUpdateStatus> downloadOperation =
        this.context.RequestDownloadStorePackageUpdatesAsync(updates);

    // The Progress async method is called one time for each step in the download process for each
    // package in this request.
    downloadOperation.Progress = async (asyncInfo, progress) =>
    {
        await this.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
        () =>
        {
            downloadProgressBar.Value = progress.PackageDownloadProgress;
        });
    };

    StorePackageUpdateResult result = await downloadOperation.AsTask();

    switch (result.OverallState)
    {
        case StorePackageUpdateState.Completed:
            downloadedSuccessfully = true;
            break;
        default:
            // Get the failed updates.
            var failedUpdates = result.StorePackageUpdateStatuses.Where(
                status => status.PackageUpdateState != StorePackageUpdateState.Completed);

            // See if any failed updates were mandatory
            if (updates.Any(u => u.Mandatory && failedUpdates.Any(
                failed => failed.PackageFamilyName == u.Package.Id.FamilyName)))
            {
                // At least one of the updates is mandatory. Perform whatever actions you
                // want to take for your app: for example, notify the user and disable
                // features in your app.
                HandleMandatoryPackageError();
            }
            break;
    }

    return downloadedSuccessfully;
}

// Helper method for installing package updates.
private async Task InstallPackageUpdatesAsync(IEnumerable<StorePackageUpdate> updates)
{
    IAsyncOperationWithProgress<StorePackageUpdateResult, StorePackageUpdateStatus> installOperation =
        this.context.RequestDownloadAndInstallStorePackageUpdatesAsync(updates);

    // The package updates were already downloaded separately, so this method skips the download
    // operatation and only installs the updates; no download progress notifications are provided.
    StorePackageUpdateResult result = await installOperation.AsTask();

    switch (result.OverallState)
    {
        case StorePackageUpdateState.Completed:
            break;
        default:
            // Get the failed updates.
            var failedUpdates = result.StorePackageUpdateStatuses.Where(
                status => status.PackageUpdateState != StorePackageUpdateState.Completed);

            // See if any failed updates were mandatory
            if (updates.Any(u => u.Mandatory && failedUpdates.Any(failed => failed.PackageFamilyName == u.Package.Id.FamilyName)))
            {
                // At least one of the updates is mandatory, so tell the user.
                HandleMandatoryPackageError();
            }
            break;
    }
}

// Helper method for handling the scenario where a mandatory package update fails to
// download or install. Add code to this method to perform whatever actions you want
// to take, such as notifying the user and disabling features in your app.
private void HandleMandatoryPackageError()
{
}
```

## <a name="uninstall-optional-packages"></a>Desinstalar paquetes opcionales

A partir de Windows 10, versión 1803, puedes usar los métodos [RequestUninstallStorePackageAsync](/uwp/api/windows.services.store.storecontext.requestuninstallstorepackageasync) o [RequestUninstallStorePackageByStoreIdAsync](/uwp/api/windows.services.store.storecontext.requestuninstallstorepackagebystoreidasync) para desinstalar un [paquete opcional](/windows/msix/package/optional-packages) (incluido un paquete DLC) para la aplicación actual. Por ejemplo, si has instalado una aplicación con contenido mediante paquetes opcionales, quizás quieras proporcionar una interfaz de usuario que permita a los usuarios desinstalar los paquetes opcionales para liberar espacio en disco.

En el ejemplo de código siguiente se muestra cómo llamar al método [RequestUninstallStorePackageAsync](/uwp/api/windows.services.store.storecontext.requestuninstallstorepackageasync). En este ejemplo se da por supuesto que:
* El archivo de código tiene una instrucción **using** para los espacios de nombres **Windows.Services.Store** y **System.Threading.Tasks**.
* La aplicación es una de usuario único que se ejecuta solamente en el contexto del usuario que la inició. Para una [aplicación multiusuario](../xbox-apps/multi-user-applications.md), usa el método [GetForUser](/uwp/api/windows.services.store.storecontext.User) para obtener un objeto **StoreContext** en lugar del método [GetDefault](/uwp/api/windows.services.store.storecontext.GetDefault).

```csharp
public async Task UninstallPackage(Windows.ApplicationModel.Package package)
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }

    var storeContext = StoreContext.GetDefault();
    IAsyncOperation<StoreUninstallStorePackageResult> uninstallOperation =
        storeContext.RequestUninstallStorePackageAsync(package);

    // At this point, you can update your app UI to show that the package
    // is installing.

    uninstallOperation.Completed += (asyncInfo, status) =>
    {
        StoreUninstallStorePackageResult result = uninstallOperation.GetResults();
        switch (result.Status)
        {
            case StoreUninstallStorePackageStatus.Succeeded:
                {
                    // Update your app UI to show the package as uninstalled.
                    break;
                }
            default:
                {
                    // Update your app UI to show that the package uninstall failed.
                    break;
                }
        }
    };
}
```

## <a name="get-download-queue-info"></a>Obtener información de la cola de descargas

A partir de Windows 10, versión 1803, puedes usar los métodos [GetAssociatedStoreQueueItemsAsync](/uwp/api/windows.services.store.storecontext.getassociatedstorequeueitemsasync) y [GetStoreQueueItemsAsync](/uwp/api/windows.services.store.storecontext.getstorequeueitemsasync) para obtener información sobre los paquetes que están en la cola actual de descargas e instalaciones de Microsoft Store. Estos métodos son útiles si la aplicación o el juego admiten paquetes opcionales grandes (incluidos DLC) que pueden tardar horas o días en descargarse e instalarse, y deseas controlar correctamente en caso de que un cliente cierre su aplicación o juego antes de que termine el proceso de descarga e instalación. Cuando el cliente inicia la aplicación o el juego de nuevo, tu código puede usar estos métodos para obtener información sobre el estado de los paquetes que aún están en la cola de descarga e instalación para poder mostrar al cliente el estado de cada paquete.

En el ejemplo de código siguiente se muestra cómo llamar a [GetAssociatedStoreQueueItemsAsync](/uwp/api/windows.services.store.storecontext.getassociatedstorequeueitemsasync) para obtener la lista de actualizaciones de paquete en curso de la aplicación actual y recuperar la información de estado de cada paquete. En este ejemplo se da por supuesto que:
* El archivo de código tiene una instrucción **using** para los espacios de nombres **Windows.Services.Store** y **System.Threading.Tasks**.
* La aplicación es una de usuario único que se ejecuta solamente en el contexto del usuario que la inició. Para una [aplicación multiusuario](../xbox-apps/multi-user-applications.md), usa el método [GetForUser](/uwp/api/windows.services.store.storecontext.User) para obtener un objeto **StoreContext** en lugar del método [GetDefault](/uwp/api/windows.services.store.storecontext.GetDefault).

> [!NOTE]
> Los métodos **MarkUpdateInProgressInUI**, **RemoveItemFromUI**, **MarkInstallCompleteInUI**, **MarkInstallErrorInUI** y **MarkInstallPausedInUI** a los que llama el código de este ejemplo son los métodos de marcador de posición destinados que se implementarán según sea necesario en función del diseño de tu aplicación.

```csharp
private StoreContext context = null;

private async Task GetQueuedInstallItemsAndBuildInitialStoreUI()
{
    if (context == null)
    {
        context = StoreContext.GetDefault();
    }

    // Get the Store packages in the install queue.
    IReadOnlyList<StoreQueueItem> storeUpdateItems = await context.GetAssociatedStoreQueueItemsAsync();

    foreach (StoreQueueItem storeItem in storeUpdateItems)
    {
        // In this example we only care about package updates.
        if (storeItem.InstallKind != StoreQueueItemKind.Update)
            continue;

        StoreQueueItemStatus currentStatus = storeItem.GetCurrentStatus();
        StoreQueueItemState installState = currentStatus.PackageInstallState;
        StoreQueueItemExtendedState extendedInstallState =
            currentStatus.PackageInstallExtendedState;

        // Handle the StatusChanged event to display current status to the customer.
        storeItem.StatusChanged += StoreItem_StatusChanged;

        switch (installState)
        {
            // Download and install are still in progress, so update the status for this  
            // item and provide the extended state info. The following methods are not
            // implemented in this example; you should implement them as needed for your
            // app's UI.
            case StoreQueueItemState.Active:
                MarkUpdateInProgressInUI(storeItem, extendedInstallState);
                break;
            case StoreQueueItemState.Canceled:
                RemoveItemFromUI(storeItem);
                break;
            case StoreQueueItemState.Completed:
                MarkInstallCompleteInUI(storeItem);
                break;
            case StoreQueueItemState.Error:
                MarkInstallErrorInUI(storeItem);
                break;
            case StoreQueueItemState.Paused:
                MarkInstallPausedInUI(storeItem, installState, extendedInstallState);
                break;
        }
    }
}

private void StoreItem_StatusChanged(StoreQueueItem sender, object args)
{
    StoreQueueItemStatus currentStatus = sender.GetCurrentStatus();
    StoreQueueItemState installState = currentStatus.PackageInstallState;
    StoreQueueItemExtendedState extendedInstallState = currentStatus.PackageInstallExtendedState;

    switch (installState)
    {
        // Download and install are still in progress, so update the status for this  
        // item and provide the extended state info. The following methods are not
        // implemented in this example; you should implement them as needed for your
        // app's UI.
        case StoreQueueItemState.Active:
            MarkUpdateInProgressInUI(sender, extendedInstallState);
            break;
        case StoreQueueItemState.Canceled:
            RemoveItemFromUI(sender);
            break;
        case StoreQueueItemState.Completed:
            MarkInstallCompleteInUI(sender);
            break;
        case StoreQueueItemState.Error:
            MarkInstallErrorInUI(sender);
            break;
        case StoreQueueItemState.Paused:
            MarkInstallPausedInUI(sender, installState, extendedInstallState);
            break;
    }
}
```

## <a name="related-topics"></a>Temas relacionados

* [Creación de paquetes opcionales y conjuntos relacionados](/windows/msix/package/optional-packages)