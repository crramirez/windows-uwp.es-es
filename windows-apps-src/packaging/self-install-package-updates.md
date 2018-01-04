---
author: mcleanbyron
ms.assetid: 414ACC73-2A72-465C-BD15-1B51CB2334F2
title: "Descargar e instalar actualizaciones de paquete para tu aplicación."
description: "Obtén información sobre cómo marcar paquetes como obligatorios en el panel del Centro de desarrollo y escribir código en tu aplicación para descargar e instalar las actualizaciones del paquete."
ms.author: mcleans
ms.date: 03/15/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: high
ms.openlocfilehash: 62dc1cf81bd26ca5ba4adf181cc9f710e41565c2
ms.sourcegitcommit: c80b9e6589a1ee29c5032a0b942e6a024c224ea7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/22/2017
---
# <a name="download-and-install-package-updates-for-your-app"></a>Descargar e instalar actualizaciones de paquetes para tu aplicación


A partir de la versión 1607 de Windows 10, puedes usar una API en el espacio de nombres [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) para comprobar actualizaciones de paquete de la aplicación actual y descargar e instalar los paquetes actualizados mediante programación. También puedes consultar los paquetes que se [marcaron como obligatorios en el panel del Centro de desarrollo de Windows](#mandatory-dashboard) y deshabilitar la funcionalidad en tu aplicación hasta que se instale la actualización obligatoria.

Estas características te ayudan a mantener tu base de usuarios actualizada con la última versión de la aplicación y los servicios relacionados de manera automática.

## <a name="api-overview"></a>Introducción a API

Las aplicaciones destinadas a la versión 1607 de Windows 10 u otras posteriores pueden usar los siguientes métodos de la clase [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) para descargar e instalar actualizaciones de paquete.

|  Método  |  Descripción  |
|----------|---------------|
| [GetAppAndOptionalStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext#Windows_Services_Store_StoreContext_GetAppAndOptionalStorePackageUpdatesAsync) | Llama a este método para obtener la lista de actualizaciones de paquete que hay disponibles. Ten en cuenta que puede haber un retraso de hasta un día entre el momento en el que un paquete supera el proceso de certificación y cuando el método **GetAppAndOptionalStorePackageUpdatesAsync** reconoce que la actualización de paquete está disponible para la aplicación. |
| [RequestDownloadStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext#Windows_Services_Store_StoreContext_RequestDownloadStorePackageUpdatesAsync_Windows_Foundation_Collections_IIterable_Windows_Services_Store_StorePackageUpdate__) | Llama a este método para descargar (pero no instalar) las actualizaciones de paquete disponibles. Este sistema operativo muestra un cuadro de diálogo que pide permiso al usuario para descargar las actualizaciones. |
| [RequestDownloadAndInstallStorePackageUpdatesAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext#Windows_Services_Store_StoreContext_RequestDownloadAndInstallStorePackageUpdatesAsync_Windows_Foundation_Collections_IIterable_Windows_Services_Store_StorePackageUpdate__) | Llama a este método para descargar e instalar las actualizaciones de paquete disponibles. El sistema operativo muestra cuadros de diálogo que piden permiso al usuario para descargar e instalar las actualizaciones. Si ya has descargado las actualizaciones de paquete mediante una llamada a **RequestDownloadStorePackageUpdatesAsync**, este método omite el proceso de descarga y solo instala las actualizaciones.  |

<span/>

Estos métodos usan objetos [StorePackageUpdate](https://docs.microsoft.com/uwp/api/windows.services.store.storepackageupdate) para manifestar los paquetes de actualización disponibles. Usa las siguientes propiedades **StorePackageUpdate** para obtener información sobre un paquete de actualización.

|  Propiedad  |  Descripción  |
|----------|---------------|
| [Mandatory](https://docs.microsoft.com/uwp/api/windows.services.store.storepackageupdate#Windows_Services_Store_StorePackageUpdate_Mandatory) | Usa esta propiedad para determinar si el paquete está marcado como obligatorio en el panel del Centro de desarrollo. |
| [Package](https://docs.microsoft.com/uwp/api/windows.services.store.storepackageupdate#Windows_Services_Store_StorePackageUpdate_Package) | Usa esta propiedad para acceder a los datos subyacentes relacionados con el paquete. |

<span/>

## <a name="code-examples"></a>Ejemplos de código

Los ejemplos de código siguientes muestran cómo descargar e instalar las actualizaciones de paquete en tu aplicación. Estos ejemplos suponen que:
* El código se ejecuta en el contexto de un elemento [Page](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page.aspx).
* El elemento **Page** contiene un elemento [ProgressBar](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.progressbar.aspx) denominado ```downloadProgressBar``` para proporcionar el estado de la operación de descarga.
* El archivo de código tiene una instrucción **using** para los espacios de nombres **Windows.Services.Store**, **Windows.Threading.Tasks** y **Windows.UI.Popups**.
* La aplicación es una aplicación de usuario único que se ejecuta solamente en el contexto del usuario que la inició. Para una [aplicación multiusuario](https://msdn.microsoft.com/windows/uwp/xbox-apps/multi-user-applications), usa el método [GetForUser](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext#Windows_Services_Store_StoreContext_GetForUser_Windows_System_User_) para obtener un objeto **StoreContext** en lugar del método [GetDefault](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext#Windows_Services_Store_StoreContext_GetDefault).

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

### <a name="display-progress-info-for-the-download-and-install"></a>Mostrar información de progreso de la descarga e instalación

Cuando llames al método **RequestDownloadStorePackageUpdatesAsync** o **RequestDownloadAndInstallStorePackageUpdatesAsync**, puedes asignar un controlador [Progress](https://docs.microsoft.com/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_#Windows_Foundation_IAsyncOperationWithProgress_2_Progress) que se llama una vez para cada paso del proceso de la descarga (o descarga e instalación) para cada paquete de la solicitud. El controlador recibe un objeto [StorePackageUpdateStatus](https://docs.microsoft.com/uwp/api/windows.services.store.storepackageupdatestatus) que proporciona información sobre el paquete de actualización que genera la notificación de progreso. En los ejemplos anteriores se utiliza el campo **PackageDownloadProgress** del objeto **StorePackageUpdateStatus** para mostrar el progreso del proceso de descarga e instalación.

Ten en cuenta que al llamar al método **RequestDownloadAndInstallStorePackageUpdatesAsync** para descargar e instalar actualizaciones del paquete en una sola operación, el campo **PackageDownloadProgress** aumenta de 0,0 a 0,8 durante el proceso de descarga para un paquete y luego aumenta de 0,8 1,0 durante la instalación. Por lo tanto, si asignas el porcentaje que se muestra en la interfaz de usuario de progreso personalizada directamente en el valor del campo **PackageDownloadProgress**, la interfaz de usuario mostrará 80% cuando el paquete se haya descargado por completo y el sistema operativo muestra el cuadro de diálogo de instalación. Si quieres que tu interfaz de usuario de progreso personalizada muestre 100% cuando el paquete se haya descargado y esté listo para instalarse, puedes modificar el código para asignar el valor de 100% a la interfaz de usuario de progreso cuando el campo **PackageDownloadProgress** alcance 0,8.

<span id="mandatory-dashboard" />
## <a name="make-a-package-submission-mandatory-in-the-dev-center-dashboard"></a>Hacer que un envío de paquete sea obligatorio en el panel del Centro de desarrollo

Cuando creas un envío de paquete para una aplicación destinada a la versión de Windows 10 o posterior, puedes marcar el paquete como obligatorio y la fecha y hora a partir de la cual es obligatorio. Cuando se establece esta propiedad y la aplicación detecta que la actualización de paquete está disponible mediante la API que se describió anteriormente en este artículo, la aplicación puede determinar si el paquete de actualización es obligatorio y modificar su comportamiento hasta que se instale la actualización (por ejemplo, la aplicación puede deshabilitar características).

> [!NOTE]
> El estado obligatorio de una actualización de paquete no lo exige Microsoft, y el sistema operativo no proporciona ninguna interfaz de usuario para indicar a los usuarios que deben instalar una actualización de la aplicación obligatoria. Se pretende que los desarrolladores usen la opción "mandatory" para aplicar las actualizaciones de la aplicación obligatorias en su propio código.  

Para marcar un envío de paquete como obligatorio:

1. Inicia sesión en el [panel del Centro de desarrollo](https://dev.windows.com/overview) y navega hasta la página de información general de la aplicación.
2. Haz clic en el nombre de la presentación que contiene la actualización de paquete que quieres que sea obligatoria.
3. Navega a la página **Paquetes** del envío. En la parte inferior de esta página, selecciona **Hacer que esta actualización sea obligatoria** y, a continuación, elige el día y la hora en que la actualización de paquete es obligatoria. Esta opción se aplica a todos los paquetes para UWP en el envío.

Para obtener más información acerca de cómo configurar paquetes en el panel del Centro de desarrollo, consulta [Cargar paquetes de aplicación](../publish/upload-app-packages.md).

  > [!NOTE]
  > Si creas un [paquete piloto](../publish/package-flights.md), puedes marcar los paquetes como obligatorios con una interfaz de usuario similar en la página **Paquetes** para el piloto. En este caso, la actualización del paquete obligatoria se aplica solo a los clientes que forman parte del grupo piloto.
