---
author: stevewhims
description: Detrás de la interfaz de usuario se encuentran las capas de negocio y de datos.
title: Migración de empresas de WindowsPhone Silverlight y capas de datos y UWP
ms.assetid: 27c66759-2b35-41f5-9f7a-ceb97f4a0e3f
ms.author: stwhi
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 301dcbe95d7509db07d5b7dd11a16460063bbffe
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/19/2018
ms.locfileid: "7306560"
---
#  <a name="porting-windowsphone-silverlight-business-and-data-layers-to-uwp"></a>Migración de empresas de WindowsPhone Silverlight y capas de datos y UWP


El tema anterior era [Migración de modelo de E/S, dispositivos y aplicaciones](wpsl-to-uwp-input-and-sensors.md).

Detrás de la interfaz de usuario se encuentran las capas de negocio y de datos. El código de estas capas llama a las API del sistema operativo y de .NET Framework (por ejemplo, proceso en segundo plano, ubicación, cámara, sistema de archivos, red y acceso a otros datos). La mayoría de ellas están [disponibles para una aplicación para la Plataforma universal de Windows (UWP)](https://msdn.microsoft.com/library/windows/apps/br211369), por lo que se puede esperar poder migrar gran parte de este código sin cambios.

## <a name="asynchronous-methods"></a>Métodos asincrónicos

Una de las prioridades de la Plataforma universal de Windows (UWP) es permitirte compilar aplicaciones realmente dinámicas y coherentes. Las animaciones son siempre ágiles y las interacciones táctiles, como el movimiento panorámico y el deslizamiento rápido, son instantáneas y sin retraso, lo que se percibe como si la interfaz de usuario estuviera pegada al dedo. Para lograrlo, se han convertido en asincrónicas las API de UWP que no pueden garantizar que se vayan a completar en 50 ms y su nombre lleva el sufijo **Async**. El subproceso de interfaz de usuario se devuelve de inmediato al llamar un método **Async** y el trabajo se llevará a cabo en otro subproceso. El consumo de un método **Async** se facilita en gran medida, sintácticamente, con el operador **await** de C#, los objetos de promesa de JavaScript y las continuaciones de C++. Para obtener más información, consulta [Programación asincrónica](https://msdn.microsoft.com/library/windows/apps/mt187335).

## <a name="background-processing"></a>Proceso en segundo plano

Una aplicación WindowsPhone Silverlight usar un objeto **ScheduledTaskAgent** administrado para realizar una tarea mientras la aplicación no está en primer plano. Una aplicación para UWP usa la clase [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) para crear y registrar una tarea en segundo plano de una forma similar. Se define una clase que implementa el trabajo de la tarea en segundo plano. El sistema ejecuta la tarea en segundo plano periódicamente, llamando al método [**Run**](https://msdn.microsoft.com/library/windows/apps/br224811) de su clase para ejecutar el trabajo. En una aplicación para UWP, recuerda establecer la declaración **Tareas en segundo plano** en el manifiesto del paquete de la aplicación. Para obtener más información, consulta [Dar soporte a tu aplicación mediante tareas en segundo plano](https://msdn.microsoft.com/library/windows/apps/mt299103).

Para transferir archivos grandes de datos en segundo plano, una aplicación WindowsPhone Silverlight usa la clase **BackgroundTransferService** . Una aplicación para UWP usa para ello las API en el espacio de nombres [**Windows.Networking.BackgroundTransfer**](https://msdn.microsoft.com/library/windows/apps/br207242). Las funciones usan un patrón similar para iniciar las transferencias, pero en la nueva API se han mejorado las funcionalidades y el rendimiento. Para obtener más información, consulta [Transferencia de datos en segundo plano](https://msdn.microsoft.com/library/windows/apps/xaml/hh452975).

Una aplicación WindowsPhone Silverlight usa las clases administradas del espacio de nombres **Microsoft.Phone.BackgroundAudio** para reproducir audio mientras la aplicación no está en primer plano. La UWP usa el modelo de aplicación de la Tienda de Windows Phone; consulta [Audio en segundo plano](https://msdn.microsoft.com/library/windows/apps/mt282140) y la muestra [Audio en segundo plano](http://go.microsoft.com/fwlink/p/?linkid=619997).

## <a name="cloud-services-networking-and-databases"></a>Servicios en la nube, redes y bases de datos

El hospedaje de datos y servicios de aplicaciones en la nube es posible con Azure. Consulta [Introducción a los servicios móviles](http://go.microsoft.com/fwlink/p/?LinkID=403138). Para obtener soluciones que necesitan datos en línea y sin conexión, consulta: [Uso de la sincronización de datos sin conexión en los Servicios móviles](http://azure.microsoft.com/documentation/articles/mobile-services-windows-store-dotnet-get-started-offline-data/).

La UWP es parcialmente compatible con la clase **System.Net.HttpWebRequest**, pero no lo es con la clase **System.Net.WebClient**. La alternativa recomendada y vanguardista es la clase [**Windows.Web.Http.HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) (o [System.Net.Http.HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.118).aspx) si necesitas que el código sea portable a otras plataformas compatibles con .NET). Estas API usan [System.Net.Http.HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage.aspx) para representar una solicitud HTTP.

Las aplicaciones para UWP actualmente no incluyen compatibilidad integrada para escenarios de uso intensivo de datos, como escenarios de línea de negocio (LOB). Sin embargo, puedes usar SQLite para los servicios de bases de datos transaccionales locales. Para obtener información, consulta [SQLite](https://visualstudiogallery.msdn.microsoft.com/4913e7d5-96c9-4dde-a1a1-69820d615936).

Pasa los URI absolutos, los URI no relativos, a tipos de Windows en tiempo de ejecución. Consulta [Pasar un URI a Windows en tiempo de ejecución](https://msdn.microsoft.com/library/hh763341.aspx).

## <a name="launchers-and-choosers"></a>Iniciadores y selectores

Con los iniciadores y selectores (que se encuentra en el espacio de nombres **Microsoft.Phone.Tasks** ), una aplicación WindowsPhone Silverlight puede interactuar con el sistema operativo para realizar operaciones comunes como redactar un correo electrónico, elegir una foto o compartir determinados tipos de datos con otra aplicación. Buscar **Microsoft.Phone.Tasks** en el tema de [Windows Phone Silverlight para asignaciones de espacios de nombres y clases de Windows 10](wpsl-to-uwp-namespace-and-class-mappings.md) para encontrar el tipo equivalente de UWP. Estos van desde mecanismos similares, llamados iniciadores y selectores, hasta la implementación de un contrato para compartir datos entre aplicaciones.

Una aplicación WindowsPhone Silverlight se puede colocar en un estado inactivo o marcada para exclusión incluso cuando se usa, por ejemplo, la tarea de selector de fotos. Una aplicación para UWP permanece activa y en ejecución cuando se usa la clase [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847).

## <a name="monetization-trial-mode-and-in-app-purchases"></a>Rentabilidad: modo de prueba y compras desde la aplicación

Una aplicación de WindowsPhone Silverlight puede usar la clase [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) UWP para la mayoría de sus funcionalidades de compra desde la aplicación y el modo de prueba para que no es necesario portar código. Sin embargo, una aplicación WindowsPhone Silverlight llama a **MarketplaceDetailTask.Show** para ofrecer la aplicación para la compra:

```csharp
    private void Buy()
    {
        MarketplaceDetailTask marketplaceDetailTask = new MarketplaceDetailTask();
        marketplaceDetailTask.ContentType = MarketplaceContentType.Applications;
        marketplaceDetailTask.Show();
    }
```

Porta el código para llamar al método [**RequestAppPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/hh967813) de UWP:

```csharp
    private async void Buy()
    {
        await Windows.ApplicationModel.Store.CurrentApp.RequestAppPurchaseAsync(false);
    }
```

Si tienes código que simula las funciones de compra y de compra desde la aplicación con fines de prueba, puedes portarlo para usar en su lugar la clase [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766).

## <a name="notifications-for-tile-or-toast-updates"></a>Notificaciones de las actualizaciones de iconos o de notificaciones del sistema

Las notificaciones son una extensión del modelo de notificación de inserción para las aplicaciones WindowsPhone Silverlight. Cuando recibas una notificación de los Servicios de notificaciones de inserción de Windows (WNS), puedes exponer la información en la interfaz de usuario con una actualización de icono o con una notificación del sistema. Para portar el lado de la interfaz de usuario correspondiente a las funciones de notificación, consulta [Iconos y notificaciones del sistema](w8x-to-uwp-porting-xaml-and-ui.md).

Si quieres más información sobre el uso de notificaciones en una aplicación para UWP, consulta [Envío de notificaciones del sistema](https://msdn.microsoft.com/library/windows/apps/xaml/hh868266).

Si quieres obtener más información y tutoriales sobre el uso de iconos, notificaciones del sistema, distintivos, mensajes emergentes y notificaciones en una aplicación de Windows en tiempo de ejecución con C++, C# o Visual Basic, consulta [Trabajar con iconos, distintivos y notificaciones del sistema](https://msdn.microsoft.com/library/windows/apps/xaml/hh868259).

## <a name="storage-file-access"></a>Almacenamiento (acceso a archivos)

Código de WindowsPhone Silverlight que almacena la configuración de la aplicación como pares de clave-valor en el almacenamiento aislado se porta fácilmente. Este es un ejemplo de antes y después, en primer lugar la versión de WindowsPhone Silverlight:

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

Aunque un subconjunto del espacio de nombres **Windows.Storage** está disponible para ellos, muchas de las aplicaciones de WindowsPhone Silverlight realizan archivo e/s con el **IsolatedStorageFile** clase porque ha sido compatible para ya. Suponiendo que se usa **IsolatedStorageFile** , este es un ejemplo de antes y después de escribir y leer un archivo, primero la versión de WindowsPhone Silverlight:

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

Una aplicación WindowsPhone Silverlight tiene acceso de solo lectura a la tarjeta SD opcional. Una aplicación para UWP tiene acceso de lectura y escritura a la tarjeta SD. Para obtener información, consulta [Acceso a la tarjeta SD](https://msdn.microsoft.com/library/windows/apps/mt188699).

Para obtener información sobre cómo obtener acceso a fotos, música y archivos de vídeo en una aplicación para UWP, consulta [Archivos y carpetas de las bibliotecas de música, imágenes y vídeos](https://msdn.microsoft.com/library/windows/apps/mt188703).

Para obtener más información, consulta [Archivos, carpetas y bibliotecas](https://msdn.microsoft.com/library/windows/apps/mt185399).

El siguiente tema es [Migración para factor de forma y experiencia de usuario](wpsl-to-uwp-form-factors-and-ux.md).

## <a name="related-topics"></a>Temas relacionados

* [Asignaciones de espacios de nombres y clases](wpsl-to-uwp-namespace-and-class-mappings.md)
 

