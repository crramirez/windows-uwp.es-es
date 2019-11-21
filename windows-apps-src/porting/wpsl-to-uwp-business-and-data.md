---
description: Detrás de la interfaz de usuario se encuentran las capas de negocio y de datos.
title: Trasladar Windows Phone las capas de datos y empresariales de Silverlight a UWP
ms.assetid: 27c66759-2b35-41f5-9f7a-ceb97f4a0e3f
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 25d8bba5e1b26613185017642d63128cc2b1f7f6
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259089"
---
#  <a name="porting-windowsphone-silverlight-business-and-data-layers-to-uwp"></a>Trasladar Windows Phone las capas de datos y empresariales de Silverlight a UWP


El tema anterior era [Migración de modelo de E/S, dispositivos y aplicaciones](wpsl-to-uwp-input-and-sensors.md).

Detrás de la interfaz de usuario se encuentran las capas de negocio y de datos. El código de estas capas llama a las API del sistema operativo y de .NET Framework (por ejemplo, proceso en segundo plano, ubicación, cámara, sistema de archivos, red y acceso a otros datos). La mayoría de ellas están [disponibles para una aplicación para la Plataforma universal de Windows (UWP)](https://docs.microsoft.com/previous-versions/windows/br211369(v=win.10)), por lo que se puede esperar poder migrar gran parte de este código sin cambios.

## <a name="asynchronous-methods"></a>Métodos asincrónicos

Una de las prioridades de la Plataforma universal de Windows (UWP) es permitirte compilar aplicaciones realmente dinámicas y coherentes. Las animaciones son siempre ágiles y las interacciones táctiles, como el movimiento panorámico y el deslizamiento rápido, son instantáneas y sin retraso, lo que se percibe como si la interfaz de usuario estuviera pegada al dedo. Para lograrlo, se han convertido en asincrónicas las API de UWP que no pueden garantizar que se vayan a completar en 50 ms y su nombre lleva el sufijo **Async**. El subproceso de interfaz de usuario se devuelve de inmediato al llamar un método **Async** y el trabajo se llevará a cabo en otro subproceso. El consumo de un método **Async** se facilita en gran medida, sintácticamente, con el operador **await** de C#, los objetos de promesa de JavaScript y las continuaciones de C++. Para obtener más información, consulta [Programación asincrónica](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-universal-windows-platform-apps).

## <a name="background-processing"></a>Proceso en segundo plano

Una aplicación de Windows Phone Silverlight puede usar un objeto **ScheduledTaskAgent** administrado para realizar una tarea mientras la aplicación no está en primer plano. Una aplicación para UWP usa la clase [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) para crear y registrar una tarea en segundo plano de una forma similar. Se define una clase que implementa el trabajo de la tarea en segundo plano. El sistema ejecuta la tarea en segundo plano periódicamente, llamando al método [**Run**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) de su clase para ejecutar el trabajo. En una aplicación para UWP, recuerda establecer la declaración **Tareas en segundo plano** en el manifiesto del paquete de la aplicación. Para obtener más información, consulta [Dar soporte a tu aplicación mediante tareas en segundo plano](https://docs.microsoft.com/windows/uwp/launch-resume/support-your-app-with-background-tasks).

Para transferir archivos de datos de gran tamaño en segundo plano, una aplicación de Windows Phone Silverlight usa la clase **BackgroundTransferService** . Una aplicación para UWP usa para ello las API en el espacio de nombres [**Windows.Networking.BackgroundTransfer**](https://docs.microsoft.com/uwp/api/Windows.Networking.BackgroundTransfer). Las funciones usan un patrón similar para iniciar las transferencias, pero en la nueva API se han mejorado las funcionalidades y el rendimiento. Para obtener más información, consulta [Transferencia de datos en segundo plano](https://docs.microsoft.com/previous-versions/windows/apps/hh452975(v=win.10)).

Una aplicación de Windows Phone Silverlight usa las clases administradas en el espacio de nombres **Microsoft. Phone. BackgroundAudio** para reproducir audio mientras la aplicación no está en primer plano. La UWP usa el modelo de aplicación de la Tienda de Windows Phone; consulta [Audio en segundo plano](https://docs.microsoft.com/windows/uwp/audio-video-camera/background-audio) y la muestra [Audio en segundo plano](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundAudio).

## <a name="cloud-services-networking-and-databases"></a>Servicios en la nube, redes y bases de datos

El hospedaje de datos y servicios de aplicaciones en la nube es posible con Azure. Consulta [Introducción a los servicios móviles](https://azure.microsoft.com/documentation/articles/mobile-services-windows-store-get-started/). Para obtener soluciones que necesitan datos en línea y sin conexión, consulta: [Uso de la sincronización de datos sin conexión en los Servicios móviles](https://azure.microsoft.com/documentation/articles/mobile-services-windows-store-dotnet-get-started-offline-data/).

La UWP es parcialmente compatible con la clase **System.Net.HttpWebRequest**, pero no lo es con la clase **System.Net.WebClient**. La alternativa recomendada y vanguardista es la clase [**Windows.Web.Http.HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) (o [System.Net.Http.HttpClient](https://docs.microsoft.com/previous-versions/visualstudio/hh193681(v=vs.118)) si necesitas que el código sea portable a otras plataformas compatibles con .NET). Estas API usan [System.Net.Http.HttpRequestMessage](https://docs.microsoft.com/previous-versions/visualstudio/hh159020(v=vs.118)) para representar una solicitud HTTP.

Las aplicaciones para UWP actualmente no incluyen compatibilidad integrada para escenarios de uso intensivo de datos, como escenarios de línea de negocio (LOB). Sin embargo, puedes usar SQLite para los servicios de bases de datos transaccionales locales. Para obtener información, consulta [SQLite](https://marketplace.visualstudio.com/items?itemName=SQLiteDevelopmentTeam.SQLiteforUniversalWindowsPlatform).

Pasa los URI absolutos, los URI no relativos, a tipos de Windows en tiempo de ejecución. Consulta [Pasar un URI a Windows en tiempo de ejecución](https://docs.microsoft.com/dotnet/standard/cross-platform/passing-a-uri-to-the-windows-runtime).

## <a name="launchers-and-choosers"></a>Iniciadores y selectores

Con los iniciadores y los separadores (que se encuentran en el espacio de nombres **Microsoft. Phone. Tasks** ), una aplicación de Windows Phone Silverlight puede interactuar con el sistema operativo para realizar operaciones comunes, como la creación de un correo electrónico, la elección de una foto o el uso compartido de determinados tipos de datos con otra aplicación. Busque **Microsoft. Phone. Tasks** en el tema [Windows Phone Silverlight a las asignaciones de espacio de nombres y clases de Windows 10](wpsl-to-uwp-namespace-and-class-mappings.md) para buscar el tipo de UWP equivalente. Estos van desde mecanismos similares, llamados iniciadores y selectores, hasta la implementación de un contrato para compartir datos entre aplicaciones.

Una aplicación de Windows Phone Silverlight se puede poner en un estado inactivo o incluso en un marcadores de exclusión cuando se usa, por ejemplo, la tarea selector de fotografías. Una aplicación para UWP permanece activa y en ejecución cuando se usa la clase [**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker).

## <a name="monetization-trial-mode-and-in-app-purchases"></a>Rentabilidad: modo de prueba y compras desde la aplicación

Una aplicación Windows Phone Silverlight puede usar la clase [**CurrentApp**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) de UWP para la mayor parte del modo de prueba y la funcionalidad de compra desde la aplicación, por lo que no es necesario migrar el código. Sin embargo, una aplicación de Windows Phone Silverlight llama a **MarketplaceDetailTask. show** para ofrecer la aplicación para la compra:

```csharp
    private void Buy()
    {
        MarketplaceDetailTask marketplaceDetailTask = new MarketplaceDetailTask();
        marketplaceDetailTask.ContentType = MarketplaceContentType.Applications;
        marketplaceDetailTask.Show();
    }
```

Porta el código para llamar al método [**RequestAppPurchaseAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestapppurchaseasync) de UWP:

```csharp
    private async void Buy()
    {
        await Windows.ApplicationModel.Store.CurrentApp.RequestAppPurchaseAsync(false);
    }
```

Si tienes código que simula las funciones de compra y de compra desde la aplicación con fines de prueba, puedes portarlo para usar en su lugar la clase [**CurrentAppSimulator**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator).

## <a name="notifications-for-tile-or-toast-updates"></a>Notificaciones de las actualizaciones de iconos o de notificaciones del sistema

Las notificaciones son una extensión del modelo de notificaciones de envío para Windows Phone aplicaciones de Silverlight. Cuando recibas una notificación de los Servicios de notificaciones de inserción de Windows (WNS), puedes exponer la información en la interfaz de usuario con una actualización de icono o con una notificación del sistema. Para portar el lado de la interfaz de usuario correspondiente a las funciones de notificación, consulta [Iconos y notificaciones del sistema](w8x-to-uwp-porting-xaml-and-ui.md).

Si quieres más información sobre el uso de notificaciones en una aplicación para UWP, consulta [Envío de notificaciones del sistema](https://docs.microsoft.com/previous-versions/windows/apps/hh868266(v=win.10)).

Si quieres obtener más información y tutoriales sobre el uso de iconos, notificaciones del sistema, distintivos, mensajes emergentes y notificaciones en una aplicación de Windows en tiempo de ejecución con C++, C# o Visual Basic, consulta [Trabajar con iconos, distintivos y notificaciones del sistema](https://docs.microsoft.com/previous-versions/windows/apps/hh868259(v=win.10)).

## <a name="storage-file-access"></a>Almacenamiento (acceso a archivos)

Windows Phone código de Silverlight que almacena la configuración de la aplicación como pares clave-valor en el almacenamiento aislado se traslada fácilmente. Este es un ejemplo de antes y después, en el que primero se Windows Phone la versión de Silverlight:

```csharp
    var propertySet = IsolatedStorageSettings.ApplicationSettings;
    const string key = "favoriteAuthor";
    propertySet[key] = "Charles Dickens";
    propertySet.Save();
    string myFavoriteAuthor = propertySet.Contains(key) ? (string)propertySet[key] : "<none>";
```

Y el equivalente de UWP:

```csharp
    var propertySet = Windows.Storage.ApplicationData.Current.LocalSettings.Values;
    const string key = "favoriteAuthor";
    propertySet[key] = "Charles Dickens";
    string myFavoriteAuthor = propertySet.ContainsKey(key) ? (string)propertySet[key] : "<none>";
```

Aunque un subconjunto del espacio de nombres **Windows. Storage** está disponible para ellos, muchas Windows Phone aplicaciones de Silverlight realizan la e/s de archivos con la clase **IsolatedStorageFile** porque se ha admitido durante más tiempo. Suponiendo que se está usando **IsolatedStorageFile** , aquí se muestra un ejemplo antes y después de escribir y leer un archivo, en primer lugar la versión Windows Phone Silverlight:

```csharp
    const string filename = "FavoriteAuthor.txt";
    using (var store = IsolatedStorageFile.GetUserStoreForApplication())
    {
        using (var streamWriter = new StreamWriter(store.CreateFile(filename)))
        {
            streamWriter.Write("Charles Dickens");
        }
        using (var StreamReader = new StreamReader(store.OpenFile(filename, FileMode.Open, FileAccess.Read)))
        {
            string myFavoriteAuthor = StreamReader.ReadToEnd();
        }
    }
```

Y la misma función con UWP:

```csharp
    const string filename = "FavoriteAuthor.txt";
    var store = Windows.Storage.ApplicationData.Current.LocalFolder;
    Windows.Storage.StorageFile file = await store.CreateFileAsync(filename, Windows.Storage.CreationCollisionOption.ReplaceExisting);
    await Windows.Storage.FileIO.WriteTextAsync(file, "Charles Dickens");
    file = await store.GetFileAsync(filename);
    string myFavoriteAuthor = await Windows.Storage.FileIO.ReadTextAsync(file);
```

Una aplicación de Windows Phone Silverlight tiene acceso de solo lectura a la tarjeta SD opcional. Una aplicación para UWP tiene acceso de lectura y escritura a la tarjeta SD. Para obtener más información, consulta [Acceder a la tarjeta SD](https://docs.microsoft.com/windows/uwp/files/access-the-sd-card).

Para obtener información sobre cómo obtener acceso a fotos, música y archivos de vídeo en una aplicación para UWP, consulta [Archivos y carpetas de las bibliotecas de música, imágenes y vídeos](https://docs.microsoft.com/windows/uwp/files/quickstart-managing-folders-in-the-music-pictures-and-videos-libraries).

Para obtener más información, consulta [Archivos, carpetas y bibliotecas](https://docs.microsoft.com/windows/uwp/files/index).

El siguiente tema es [Migración para factor de forma y experiencia de usuario](wpsl-to-uwp-form-factors-and-ux.md).

## <a name="related-topics"></a>Temas relacionados

* [Asignaciones de espacio de nombres y clases](wpsl-to-uwp-namespace-and-class-mappings.md)
 

