---
description: En este artículo se explica cómo recibir contenido en la aplicación de Plataforma universal de Windows (UWP) compartida desde otra aplicación mediante el uso del contrato para contenido compartido. Este contrato para contenido compartido permite que la aplicación se presente como una opción cuando el usuario invoca Compartir.
title: Recibir datos
ms.assetid: 0AFF9E0D-DFF4-4018-B393-A26B11AFDB41
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2bf345f3b8f72043c72f47d681aa3ed174eb0dc5
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175749"
---
# <a name="receive-data"></a>Recibir datos



En este artículo se explica cómo recibir contenido en la aplicación de Plataforma universal de Windows (UWP) compartida desde otra aplicación mediante el uso del contrato para contenido compartido. Este contrato para contenido compartido permite que la aplicación se presente como una opción cuando el usuario invoca Compartir.

## <a name="declare-your-app-as-a-share-target"></a>Declarar la aplicación como destino de recursos compartidos

Cuando un usuario invoca Compartir, el sistema muestra una lista de posibles aplicaciones de destino. Para que aparezca en la lista, la aplicación debe declarar que es compatible con el contrato para contenido compartido. Esto permite que el sistema sepa que tu aplicación está disponible para recibir contenido.

1.  Abre el archivo de manifiesto. Su nombre debe ser similar a **package.appxmanifest**.
2.  Abre la pestaña **Declaraciones**.
3.  Elige la opción **Compartir destino** en la lista **Declaraciones disponibles** y luego selecciona **Agregar**.

## <a name="choose-file-types-and-formats"></a>Elegir tipos de archivo y formatos

A continuación, debes decidir qué tipos de archivo y formatos de datos admites. Las API de recurso compartido admiten varios formatos estándar, como texto, HTML y mapas de bits. También puedes especificar tipos de archivo y formatos de datos personalizados. Si lo haces, recuerda que las aplicaciones de origen tienen que saber cuáles son esos tipos de formatos y archivos; de lo contrario, dichas aplicaciones no pueden usar los formatos para compartir datos.

Registra únicamente los formatos que tu aplicación pueda controlar. Cuando el usuario invoca Compartir, solo aparecen las aplicaciones de destino que admiten los datos que se van a compartir.

Para establecer los tipos de archivo:

1.  Abre el archivo de manifiesto. Su nombre debe ser similar a **package.appxmanifest**.
2.  En la sección **Tipos de archivo admitidos** de la página **Declaraciones**, selecciona **Agregar nuevo**.
3.  Escribe la extensión del nombre de archivo que quieres admitir, por ejemplo, ".docx". Debes incluir el punto. Si quieres admitir todos los tipos de archivo, activa la casilla **SupportsAnyFileType**.

Para establecer los formatos de datos:

1.  Abre el archivo de manifiesto.
2.  Abre la sección **Formatos de datos** de la página **Declaraciones** y selecciona **Agregar nueva**.
3.  Escribe el nombre del formato de datos que admites, por ejemplo, "Texto".

## <a name="handle-share-activation"></a>Controlar la activación de recursos compartidos

Cuando un usuario selecciona tu aplicación (por lo general, la selecciona desde la lista de aplicaciones de destino disponibles en la interfaz de usuario del recurso compartido), se genera un evento [**OnShareTargetActivated**](/uwp/api/Windows.UI.Xaml.Application#Windows_UI_Xaml_Application_OnShareTargetActivated_Windows_ApplicationModel_Activation_ShareTargetActivatedEventArgs_). Tu aplicación debe controlar este evento para procesar los datos que el usuario quiere compartir.

<!-- For some reason, the snippets in this file are all inline in the WDCML topic. Suggest moving to VS project with rest of snippets. -->
```cs
protected override async void OnShareTargetActivated(ShareTargetActivatedEventArgs args)
{
    // Code to handle activation goes here. 
} 
```

Los datos que el usuario quiere compartir están contenidos en un objeto [**ShareOperation**](/uwp/api/Windows.ApplicationModel.DataTransfer.ShareTarget.ShareOperation). Puedes usar este objeto para comprobar el formato de los datos que contiene.

```cs
ShareOperation shareOperation = args.ShareOperation;
if (shareOperation.Data.Contains(StandardDataFormats.Text))
{
    string text = await shareOperation.Data.GetTextAsync();

    // To output the text from this example, you need a TextBlock control
    // with a name of "sharedContent".
    sharedContent.Text = "Text: " + text;
} 
```

## <a name="report-sharing-status"></a>Notificar el estado del recurso compartido

En algunos casos, es posible que tu aplicación tarde en procesar los datos que quiere compartir. Entre los ejemplos se incluyen los usuarios que comparten colecciones de archivos o imágenes. Estos elementos son más grandes que una simple cadena de texto, por lo que tardan más tiempo en procesarse.

```cs
shareOperation.ReportStarted(); 
```

Después de llamar a [**ReportStarted**](/uwp/api/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reportstarted), no debes esperar más interacciones del usuario con tu aplicación. En consecuencia, no debes llamar a este método a menos que tu aplicación se encuentre en un punto en el que el usuario pueda descartarla.

Con un recurso compartido extendido, es posible que el usuario quiera descartar la aplicación de origen antes de que tu aplicación tenga todos los datos del objeto DataPackage. Por lo tanto, te recomendamos que, cuando tu aplicación haya adquirido los datos que necesita, informes al sistema. De este modo, el sistema puede suspender o finalizar la aplicación de origen según sea necesario.

```cs
shareOperation.ReportSubmittedBackgroundTask(); 
```

Si se produce algún problema, llama a [**ReportError**](/uwp/api/Windows.ApplicationModel.DataTransfer.ShareTarget.ShareOperation#Windows_ApplicationModel_DataTransfer_ShareTarget_ShareOperation_ReportError_System_String_) para enviar un mensaje de error al sistema. El usuario verá el mensaje al comprobar el estado del recurso compartido. En ese momento, se cierra la aplicación y finaliza el recurso compartido. El usuario tendrá que iniciar de nuevo para compartir el contenido de la aplicación. Según el escenario, es posible que decidas que un error en particular no es lo suficientemente grave como para terminar la operación de compartir. En ese caso, puedes optar por no llamar a **ReportError** y continuar con la acción de compartir.

```cs
shareOperation.ReportError("Could not reach the server! Try again later."); 
```

Por último, una vez que tu aplicación haya procesado correctamente el contenido compartido, debes llamar a [**ReportCompleted**](/uwp/api/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reportcompleted) para notificárselo al sistema.

```cs
shareOperation.ReportCompleted();
```

Cuando usas estos métodos, normalmente los llamas en el orden indicado anteriormente y no lo haces más de una vez. Sin embargo, hay veces en las que una aplicación de destino puede llamar a [**ReportDataRetrieved**](/uwp/api/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reportdataretrieved) antes de a [**ReportStarted**](/uwp/api/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reportstarted). Por ejemplo, la aplicación podría recuperar los datos como parte de una tarea del controlador de activación, pero no llamar a **ReportStarted** hasta que el usuario seleccione botón **Compartir**.

## <a name="return-a-quicklink-if-sharing-was-successful"></a>Devolver un objeto Quicklink en operaciones de uso compartido correctas

Cuando un usuario seleccione tu aplicación para recibir contenido, te recomendamos que crees un [**QuickLink**](/uwp/api/Windows.ApplicationModel.DataTransfer.ShareTarget.QuickLink). Un **QuickLink** es como un acceso directo que hace que a los usuarios les resulte más fácil compartir información con tu aplicación. Por ejemplo, tu aplicación puede crear un **QuickLink** que abra un nuevo mensaje de correo configurado previamente con la dirección de correo de un amigo.

Un **enlace rápido** debe tener un título, un icono y un identificador. El título (por ejemplo, "correo electrónico Mom") y el icono aparecen cuando el usuario puntea en el acceso al recurso compartido. El identificador es lo que tu aplicación usa para acceder a cualquier información personalizada, como una dirección de correo o unas credenciales de inicio de sesión. Cuando la aplicación crea un **QuickLink**, devuelve el **QuickLink** al sistema mediante una llamada a [**ReportCompleted**](/uwp/api/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reportcompleted).

Un **QuickLink** en realidad no almacena datos. En su lugar, contiene un identificador que, cuando se selecciona, se envía a tu aplicación. Tu aplicación es responsable de almacenar el identificador del **QuickLink** y los datos de usuario correspondientes. Cuando el usuario pulsa el objeto **QuickLink**, puedes obtener el identificador mediante la propiedad [**QuickLinkId**](/uwp/api/windows.applicationmodel.datatransfer.sharetarget.shareoperation.quicklinkid).

```cs
async void ReportCompleted(ShareOperation shareOperation, string quickLinkId, string quickLinkTitle)
{
    QuickLink quickLinkInfo = new QuickLink
    {
        Id = quickLinkId,
        Title = quickLinkTitle,

        // For quicklinks, the supported FileTypes and DataFormats are set 
        // independently from the manifest
        SupportedFileTypes = { "*" },
        SupportedDataFormats = { StandardDataFormats.Text, StandardDataFormats.Uri, 
                StandardDataFormats.Bitmap, StandardDataFormats.StorageItems }
    };

    StorageFile iconFile = await Windows.ApplicationModel.Package.Current.InstalledLocation.CreateFileAsync(
            "assets\\user.png", CreationCollisionOption.OpenIfExists);
    quickLinkInfo.Thumbnail = RandomAccessStreamReference.CreateFromFile(iconFile);
    shareOperation.ReportCompleted(quickLinkInfo);
}
```

## <a name="see-also"></a>Vea también 

* [Comunicación entre aplicaciones](index.md)
* [Compartir datos](share-data.md)
* [OnShareTargetActivated](/uwp/api/windows.ui.xaml.application.onsharetargetactivated)
* [ReportStarted](/uwp/api/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reportstarted)
* [ReportError](/uwp/api/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reporterror)
* [ReportCompleted](/uwp/api/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reportcompleted)
* [ReportDataRetrieved](/uwp/api/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reportdataretrieved)
* [ReportStarted](/uwp/api/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reportstarted)
* [QuickLink](/uwp/api/windows.applicationmodel.datatransfer.sharetarget.quicklink)
* [QuickLInkId](/uwp/api/windows.applicationmodel.datatransfer.sharetarget.quicklink.id)