---
title: Realizar un seguimiento de los cambios del sistema de archivos en segundo plano
description: Describe cómo realizar el seguimiento de cambios en archivos y carpetas en segundo plano a medida que los usuarios los mueven por el sistema.
ms.date: 12/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1cef2fb660681d3e382eb8ca7dcb92456756f627
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "75685233"
---
# <a name="track-file-system-changes-in-the-background"></a>Realizar un seguimiento de los cambios del sistema de archivos en segundo plano

**API importantes**

-   [**StorageLibraryChangeTracker**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageLibraryChangeTracker)
-   [**StorageLibraryChangeReader**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader)
-   [**StorageLibraryChangedTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.StorageLibraryContentChangedTrigger)
-   [**StorageLibrary**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary)

La clase [**StorageLibraryChangeTracker**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageLibraryChangeTracker) permite a las aplicaciones realizar un seguimiento de cambios en archivos y carpetas a medida que los usuarios los mueven por el sistema. Mediante la clase **StorageLibraryChangeTracker**, una aplicación puede realizar un seguimiento:

- Operaciones con archivos, incluidas agregar, eliminar, modificar.
- Operaciones con carpetas como cambios de nombre y eliminaciones.
- Archivos y carpetas que se mueven en la unidad de disco.

Utilice esta guía para conocer el modelo de programación para trabajar con el seguimiento de cambios, ver algunos ejemplos de código y comprender los distintos tipos de operaciones de archivo cuyo seguimiento realiza **StorageLibraryChangeTracker**.

**StorageLibraryChangeTracker** funciona para las bibliotecas de usuario o para cualquier carpeta en el equipo local. Esto incluye unidades secundarias o las unidades extraíbles, pero no incluye las unidades de servidor NAS o unidades de red.

## <a name="using-the-change-tracker"></a>Uso del seguimiento de cambios

El seguimiento de cambios se implementa en el sistema como un búfer circular que almacena las últimas operaciones de sistema de archivos *N*. Las aplicaciones pueden leer los cambios en el búfer y, a continuación, procesarlos en sus propias experiencias. Una vez que la aplicación termina con los cambios, los marca como procesados y nunca se verán nuevamente.

Para usar el seguimiento de cambios en una carpeta, siga estos pasos:

1. Habilite el seguimiento de cambios para la carpeta.
2. Espere los cambios.
3. Lea los cambios.
4. Acepte los cambios.

Las secciones siguientes lo guiarán a través de cada uno de los pasos con algunos ejemplos de código. El ejemplo de código completo se proporciona al final del artículo.

### <a name="enable-the-change-tracker"></a>Habilitar el seguimiento de cambios

Lo primero que la aplicación debe hacer es indicar al sistema que está interesada en una biblioteca concreta de seguimiento de cambios. Esto hace una llamada al método [**Enable**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable) en el seguimiento de cambios para la biblioteca de interés.

```csharp
StorageLibrary videosLib = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
StorageLibraryChangeTracker videoTracker = videosLib.ChangeTracker;
videoTracker.Enable();
```

Otras notas importantes:

- Asegúrese de que la aplicación tiene permiso para la biblioteca correcta en el manifiesto antes de crear el objeto [**StorageLibrary**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary). Para más información, consulte [Permisos de acceso de archivos](https://docs.microsoft.com/windows/uwp/files/file-access-permissions).
- [**Habilitar**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable) es seguro para subprocesos, no se restablecerá el puntero y puede llamarse tantas veces como sea necesario (volveremos sobre esto más adelante).

![Habilitar un seguimiento de cambios vacío](images/changetracker-enable.png)

### <a name="wait-for-changes"></a>Espere los cambios

Una vez inicializado el seguimiento de cambios, comenzará a registrar todas las operaciones que se producen dentro de una biblioteca, aunque no se está ejecutando la aplicación. Las aplicaciones pueden registrarse para activarse siempre que hay un cambio al registrarse para obtener el evento [**StorageLibraryChangedTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.StorageLibraryContentChangedTrigger).

![Los cambios se agregan al seguimiento de cambios sin que la aplicación los lea.](images/changetracker-waiting.png)

### <a name="read-the-changes"></a>Lea los cambios

La aplicación puede sondear en busca de cambios desde el seguimiento de cambios y recibir una lista de los cambios desde la última vez que se registró. El código siguiente muestra cómo obtener una lista de cambios desde el seguimiento de cambios.

```csharp
StorageLibrary videosLibrary = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
videosLibrary.ChangeTracker.Enable();
StorageLibraryChangeReader videoChangeReader = videosLibrary.ChangeTracker.GetChangeReader();
IReadOnlyList changeSet = await changeReader.ReadBatchAsync();
```

La aplicación es responsable de procesar los cambios en su propia experiencia o la base de datos según sea necesario.

![Lectura de los cambios desde el seguimiento de cambios en una base de datos de aplicación](images/changetracker-reading.png)

> [!TIP]
> La segunda llamada a habilitar es para defenderse contra una condición de carrera si el usuario agrega otra carpeta a la biblioteca mientras la aplicación lee los cambios. Sin la llamada adicional para **Habilitar** el código se producirá un error con ecSearchFolderScopeViolation (0 x 80070490) si el usuario está cambiando las carpetas en la biblioteca.

### <a name="accept-the-changes"></a>Acepte los cambios

Después de que la aplicación terminó de procesar los cambios, debe indicar al sistema nunca volver a mostrar esos cambios llamando el método [**AcceptChangesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.acceptchangesasync).

```csharp
await changeReader.AcceptChangesAsync();
```

![Marcar los cambios como leídos para que nunca se muestren de nuevo.](images/changetracker-accepting.png)

La aplicación ahora solo recibirá nuevos cambios al leer el seguimiento de cambios en el futuro.

- Si se han producido cambios entre la llamada a [**ReadBatchAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.readbatchasync) y [AcceptChangesAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.acceptchangesasync), el puntero solo avanzará hasta el último cambio que ha visto la aplicación. Esos otros cambios todavía estarán disponibles la próxima vez que llama a **ReadBatchAsync**.
- No aceptar los cambios hará que el sistema devuelva el mismo conjunto de cambios la próxima vez que la aplicación llame a **ReadBatchAsync**.

## <a name="important-things-to-remember"></a>Cosas que recordar

Al usar el seguimiento de cambios, hay algunas cosas que debe tener en cuenta para asegurarse de que todo funciona correctamente.

### <a name="buffer-overruns"></a>Saturaciones del búfer

Aunque intentamos reservar suficiente espacio en el seguimiento de cambios para contener todas las operaciones que se producen en el sistema hasta que la aplicación puede leerlas, es muy fácil imaginar un escenario donde la aplicación no lee los cambios antes de que el búfer circular se sobrescriba. Especialmente si el usuario restaura datos desde una copia de seguridad o sincroniza una colección grande de imágenes desde su teléfono con cámara.

En este caso, **ReadBatchAsync** devolverá el código de error [**StorageLibraryChangeType.ChangeTrackingLost**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetype). Si la aplicación recibe este código de error, significa un par de cosas:

* El búfer se ha sobrescrito desde la última vez que lo notó. El mejor procedimiento es volver a rastrear la biblioteca, porque toda la información de la herramienta de seguimiento estará incompleta.
* El seguimiento de cambios no devolverá más cambios hasta que llame a [**Restablecer**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.reset). Después de que la aplicación llama a restablecer, el puntero se moverá al cambio más reciente y el seguimiento se reanudará con normalidad.

Debería ser excepcional llegar a estos casos, pero en escenarios donde el usuario está moviendo un gran número de archivos en su disco no queremos que el seguimiento de cambios se hinche y ocupe demasiado espacio de almacenamiento. Esto debe permitir que las aplicaciones reaccionen a las operaciones del sistema de archivos grandes y no dañen la experiencia del cliente en Windows.

### <a name="changes-to-a-storagelibrary"></a>Cambios realizados en un StorageLibrary

La clase [**StorageLibrary**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary) existe como un grupo virtual de las carpetas raíz que contiene otras carpetas. Para conciliar esto con un seguimiento de cambios sistema de archivos, hemos realizado las siguientes opciones:

- Los cambios en los descendientes de las carpetas raíz de la biblioteca se representarán en el seguimiento de cambios. Las carpetas raíz de la biblioteca se pueden encontrar mediante la propiedad [**Carpetas**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.folders).
- Agregar o quitar carpetas raíz de un **StorageLibrary** (a través de [**RequestAddFolderAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.requestaddfolderasync) y [**RequestRemoveFolderAsync** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.requestremovefolderasync)) no creará una entrada en el seguimiento de cambios. Se pueden realizar el seguimiento de estos cambios a través del evento [**DefinitionChanged**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.definitionchanged) o bien recorriendo las carpetas raíz de la biblioteca con la propiedad [**carpetas**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.folders).
- Si una carpeta con contenido en ella se agrega a la biblioteca, no habrá una notificación de cambio ni se generarán entradas de seguimiento de cambio. Los cambios subsiguientes a los descendientes de esa carpeta generarán notificaciones y cambiarán las entradas de seguimiento.

### <a name="calling-the-enable-method"></a>Llamada al método Enable

Las aplicaciones deben llamar [**Enable**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable) en cuanto inicie seguimiento del sistema de archivos y antes de cada enumeración de los cambios. Así asegurará que se capturarán todos los cambios realizados por el seguimiento de cambios.  

## <a name="putting-it-together"></a>Reunión de todo

Aquí está todo el código que se usa para registrar los cambios de la biblioteca de vídeos e iniciar a extraer los cambios desde el seguimiento de cambios.

```csharp
private async void EnableChangeTracker()
{
    StorageLibrary videosLib = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
    StorageLibraryChangeTracker videoTracker = videosLib.ChangeTracker;
    videoTracker.Enable();
}

private async void GetChanges()
{
    StorageLibrary videosLibrary = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
    videosLibrary.ChangeTracker.Enable();
    StorageLibraryChangeReader videoChangeReader = videosLibrary.ChangeTracker.GetChangeReader();
    IReadOnlyList changeSet = await changeReader.ReadBatchAsync();


    //Below this line is for the blog post. Above the line is for the magazine
    foreach (StorageLibraryChange change in changeSet)
    {
        if (change.ChangeType == StorageLibraryChangeType.ChangeTrackingLost)
        {
            //We are in trouble. Nothing else is going to be valid.
            log("Resetting the change tracker");
            videosLibrary.ChangeTracker.Reset();
            return;
        }
        if (change.IsOfType(StorageItemTypes.Folder))
        {
            await HandleFileChange(change);
        }
        else if (change.IsOfType(StorageItemTypes.File))
        {
            await HandleFolderChange(change);
        }
        else if (change.IsOfType(StorageItemTypes.None))
        {
            if (change.ChangeType == StorageLibraryChangeType.Deleted)
            {
                RemoveItemFromDB(change.Path);
            }
        }
    }
    await changeReader.AcceptChangesAsync();
}
```
