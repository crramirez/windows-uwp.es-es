---
author: mcleanbyron
ms.assetid: 414ACC73-2A72-465C-BD15-1B51CB2334F2
title: "Descargar e instalar actualizaciones de paquete para tu aplicación."
description: "Obtén información sobre cómo marcar paquetes como obligatorios en el panel del Centro de desarrollo y escribir código en tu aplicación para descargar e instalar las actualizaciones de los paquetes."
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: ffd1af9475791b8190f9364d85f7a7fa23548856
ms.lasthandoff: 02/07/2017

---
# <a name="download-and-install-package-updates-for-your-app"></a>Descargar e instalar actualizaciones de paquetes para tu aplicación

\[ Actualizado para las aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

A partir de la versión 1607 de Windows 10, puedes usar una API en el espacio de nombres [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) para comprobar actualizaciones de paquete de la aplicación actual y descargar e instalar los paquetes actualizados mediante programación. También puedes consultar los paquetes que se [marcaron como obligatorios en el panel del Centro de desarrollo de Windows](#mandatory-dashboard) y deshabilitar la funcionalidad en tu aplicación hasta que se instale la actualización obligatoria.

Estas características te ayudan a mantener tu base de usuarios actualizada con la última versión de la aplicación y los servicios relacionados de manera automática.

## <a name="api-overview"></a>Introducción a API

Las aplicaciones destinadas a la versión 1607 de Windows 10 u otras posteriores pueden usar los siguientes métodos de la clase [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) para descargar e instalar actualizaciones de paquete.

|  Método  |  Descripción  |
|----------|---------------|
| [GetAppAndOptionalStorePackageUpdatesAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getappandoptionalstorepackageupdatesasync.aspx) | Llama a este método para obtener la lista de actualizaciones de paquete que hay disponibles.<br/><br/>**Importante**&nbsp;&nbsp;Existe una latencia de hasta un día entre el momento en el que un paquete supera el proceso de certificación y cuando el método [GetAppAndOptionalStorePackageUpdatesAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getappandoptionalstorepackageupdatesasync.aspx) reconoce que la actualización de paquete está disponible para la aplicación. |
| [RequestDownloadStorePackageUpdatesAsync](https://msdn.microsoft.com/library/windows/apps/mt706586.aspx) | Llama a este método para descargar (pero no instalar) las actualizaciones de paquete disponibles. Este sistema operativo muestra un cuadro de diálogo que pide permiso al usuario para descargar las actualizaciones. |
| [RequestDownloadAndInstallStorePackageUpdatesAsync](https://msdn.microsoft.com/library/windows/apps/mt706585.aspx) | Llama a este método para descargar e instalar las actualizaciones de paquete disponibles. El sistema operativo muestra cuadros de diálogo que piden permiso al usuario para descargar e instalar las actualizaciones. Si ya has descargado las actualizaciones de paquete mediante una llamada a [RequestDownloadStorePackageUpdatesAsync](https://msdn.microsoft.com/library/windows/apps/mt706586.aspx), este método omite el proceso de descarga y solo instala las actualizaciones.  |

<span/>

Estos métodos usan objetos [StorePackageUpdate](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storepackageupdate.aspx) para manifestar los paquetes de actualización disponibles. Usa las siguientes propiedades [StorePackageUpdate](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storepackageupdate.aspx) para obtener información sobre un paquete de actualización.

|  Propiedad  |  Descripción  |
|----------|---------------|
| [Mandatory](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storepackageupdate.mandatory.aspx) | Usa esta propiedad para determinar si el paquete está marcado como obligatorio en el panel del Centro de desarrollo. |
| [Package](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storepackageupdate.package.aspx) | Usa esta propiedad para acceder a los datos subyacentes relacionados con el paquete. |

<span/>

## <a name="code-examples"></a>Ejemplos de código

Los ejemplos de código siguientes muestran cómo descargar e instalar las actualizaciones de paquete en tu aplicación. Estos ejemplos suponen que:
* El código se ejecuta en el contexto de un elemento [Page](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx).
* El elemento **Page** contiene un elemento [ProgressBar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressbar.aspx) denominado ```downloadProgressBar``` para proporcionar el estado de la operación de descarga.
* El archivo de código tiene una instrucción **using** para el espacio de nombres [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx).
* La aplicación es una aplicación de usuario único que se ejecuta solamente en el contexto del usuario que inició la aplicación. Para una [aplicación multiusuario](https://msdn.microsoft.com/windows/uwp/xbox-apps/multi-user-applications), usa el método [GetForUser](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getforuser.aspx) para obtener un objeto [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) en lugar del método [GetDefault](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getdefault.aspx).

<span/>

### <a name="download-and-install-all-package-updates"></a>Descargar e instalar todas las actualizaciones de paquete

El siguiente ejemplo de código muestra cómo descargar e instalar todas las actualizaciones de paquete disponibles.  

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

### <a name="handle-mandatory-package-updates"></a>Controlar las actualizaciones de paquete obligatorias

El siguiente ejemplo de código se basa en el ejemplo anterior y muestra cómo determinar si los paquetes de actualización se han [marcado como obligatorios en el panel del Centro de desarrollo de Windows](#mandatory-dashboard). Por lo general, debes degradar la experiencia de tu aplicación correctamente para el usuario en caso de que una actualización de paquete obligatoria no se descargue o instale correctamente.

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

<span id="mandatory-dashboard" />
## <a name="make-a-package-submission-mandatory-in-the-dev-center-dashboard"></a>Hacer que un envío de paquete sea obligatorio en el panel del Centro de desarrollo

Cuando creas un envío de paquete para una aplicación destinada a la versión de Windows 10 o posterior, puedes marcar el paquete como obligatorio y la fecha y hora a partir de la cual es obligatorio. Cuando se establece esta propiedad y la aplicación detecta que la actualización de paquete está disponible mediante la API que se describió anteriormente en este artículo, la aplicación puede determinar si el paquete de actualización es obligatorio y modificar su comportamiento hasta que se instale la actualización (por ejemplo, la aplicación puede deshabilitar características).

>**Nota**&nbsp;&nbsp;El estado obligatorio de una actualización de paquete no lo exige Microsoft, y el sistema operativo no proporciona ninguna interfaz de usuario para indicar a los usuarios que deben instalar una actualización de la aplicación obligatoria. Se pretende que los desarrolladores usen la opción "mandatory" para aplicar las actualizaciones de la aplicación obligatorias en su propio código.  

Para marcar un envío de paquete como obligatorio:

1. Inicia sesión en el [panel del Centro de desarrollo](https://dev.windows.com/overview) y navega hasta la página de información general de la aplicación.
2. Haz clic en el nombre de la presentación que contiene la actualización de paquete que quieres que sea obligatoria.
3. Navega a la página **Paquetes** del envío. En la parte inferior de esta página, selecciona **Hacer que esta actualización sea obligatoria** y, a continuación, elige el día y la hora en que la actualización de paquete es obligatoria. Esta opción se aplica a todos los paquetes para UWP en el envío.

Para obtener más información acerca de cómo configurar paquetes en el panel del Centro de desarrollo, consulta [Cargar paquetes de aplicación](https://msdn.microsoft.com/windows/uwp/publish/upload-app-packages).

  >**Nota**&nbsp;&nbsp;Si creas un [paquete piloto](https://msdn.microsoft.com/windows/uwp/publish/package-flights), puedes marcar los paquetes como obligatorios con una interfaz de usuario similar en la página **Paquetes** para el piloto. En este caso, la actualización del paquete obligatoria se aplica solo a los clientes que forman parte del grupo piloto.

