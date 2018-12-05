---
title: Realizar un seguimiento de cambios de sistema de archivos en segundo plano
description: Describe cómo realizar un seguimiento de los cambios en los archivos y carpetas en segundo plano mientras los usuarios les moverse por el sistema.
ms.date: 11/20/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: e90692753924572a932767b9c188ed6d24f94593
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2018
ms.locfileid: "8701867"
---
# <a name="track-file-system-changes-in-the-background"></a>Realizar un seguimiento de cambios de sistema de archivos en segundo plano

La clase [StorageLibraryChangeTracker](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageLibraryChangeTracker) permite que las aplicaciones realizar un seguimiento de los cambios en archivos y carpetas como los usuarios les moverse por el sistema. Uso de la clase **StorageLibraryChangeTracker** , una aplicación puede realizar un seguimiento de:

- Archivo operaciones como agregar, eliminar, modificar.
- Operaciones de carpeta como cambia el nombre y eliminaciones.
- Los archivos y carpetas mover en la unidad.

Usa a esta guía para obtener el modelo de programing para trabajar con la herramienta de seguimiento de cambio, ver código de ejemplo y comprender los distintos tipos de operaciones de archivo que están registrados por **StorageLibraryChangeTracker**.

**StorageLibraryChangeTracker** funciona para las bibliotecas de usuario, o para cualquier carpeta en el equipo local. Esto incluye las unidades secundarias o unidades extraíbles, pero no incluye las unidades NAS o unidades de red.

## <a name="using-the-change-tracker"></a>Usar la herramienta de seguimiento de cambio

El seguimiento de cambios se implementa en el sistema como un búfer circular almacenar las operaciones de sistema de archivos de *N* últimas. Las aplicaciones son capaces de leer los cambios en el búfer y, a continuación, procesarlos en sus propias experiencias. Una vez que finalice la aplicación con los cambios que marca los cambios como procesado y nunca verlos de nuevo.

Para usar la herramienta de seguimiento de cambios en una carpeta, sigue estos pasos:

1. Habilitar el seguimiento de cambios para la carpeta.
2. Esperar a que los cambios.
3. Leer los cambios.
4. Acepta los cambios.

Las secciones siguientes guiarán a través de cada uno de los pasos con algunos ejemplos de código. El ejemplo de código completo se proporciona al final del artículo.

### <a name="enable-the-change-tracker"></a>Habilitar el seguimiento de cambios

Lo primero que la aplicación debe hacer es indicar al sistema que está interesado en una determinada biblioteca de seguimiento de cambios. Esto lo hace llamando al método [Habilitar](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable) en el seguimiento de cambios de la biblioteca de interés.

```csharp
StorageLibrary videosLib = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
StorageLibraryChangeTracker videoTracker = videosLib.ChangeTracker;
videoTracker.Enable();
```

Unas notas importantes:

- Asegúrate de que la aplicación tenga permiso a la biblioteca correcta en el manifiesto antes de crear el objeto [StorageLibrary](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary) . Para obtener más información, consulta [Los permisos de acceso de archivo](https://docs.microsoft.com/en-us/windows/uwp/files/file-access-permissions) .
- [Habilitar](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable) es segura para subprocesos, no se restablecerá el puntero y se puede llamar tantas veces como quieras (más información sobre esto más adelante).

![Habilitar un seguimiento de cambios vacía](images/changetracker-enable.png)

### <a name="wait-for-changes"></a>Espera a que los cambios

Después de inicializa el seguimiento de cambios, empezará a registrar todas las operaciones que se producen dentro de una biblioteca, incluso aunque no se está ejecutando la aplicación. Las aplicaciones pueden registrar para activarse en cualquier momento no hay un cambio por el registro para el evento [StorageLibraryChangedTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.StorageLibraryContentChangedTrigger) .

![Cambios que se agrega a la herramienta de seguimiento de cambios sin que la aplicación leerlos](images/changetracker-waiting.png)

### <a name="read-the-changes"></a>Leer los cambios

La aplicación puede, a continuación, hace un sondeo de los cambios desde el seguimiento de cambios y recibir una lista de los cambios desde la última vez que comprueban. El siguiente código muestra cómo obtener una lista de los cambios de la herramienta de seguimiento de cambio.

```csharp
StorageLibrary videosLibrary = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
videosLibrary.ChangeTracker.Enable();
StorageLibraryChangeReader videoChangeReader = videosLibrary.ChangeTracker.GetChangeReader();
IReadOnlyList changeSet = await changeReader.ReadBatchAsync();
```

La aplicación, a continuación, es responsable de procesar los cambios en su propia experiencia o la base de datos según sea necesario.

![Lectura de los cambios desde el seguimiento de cambios en una base de datos de aplicación](images/changetracker-reading.png)

> [!TIP]
> La segunda llamada para habilitar es a proteger contra una condición de carrera si el usuario agrega otra carpeta a la biblioteca, mientras que la aplicación que lee los cambios. Sin la llamada adicional para **Permitir que** el código se producirá un error con ecSearchFolderScopeViolation (0 x 80070490) si el usuario está cambiando las carpetas en su biblioteca

### <a name="accept-the-changes"></a>Aceptar los cambios

Después de que la aplicación haya terminado de procesar los cambios, debe indicar al sistema nunca volver a mostrar esos cambios llamando al método [AcceptChangesAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.acceptchangesasync) .

```csharp
await changeReader.AcceptChangesAsync();
```

![Marcar los cambios como leídos por lo que nunca se mostrarán nuevo](images/changetracker-accepting.png)

Ahora la aplicación solo recibirá nuevos cambios al leer el seguimiento de cambios en el futuro.

- Si los cambios de han ocurrido entre una llamada a [ReadBatchAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.readbatchasync) y [AcceptChangesAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.acceptchangesasync), será el puntero solo se avanzada para el cambio más reciente que se ha visto la aplicación. Los otros cambios seguirá estando disponibles la próxima vez que se llama a **ReadBatchAsync**.
- No se aceptan los cambios hará que el sistema devolver el mismo conjunto de cambios en la próxima vez que la aplicación llama a **ReadBatchAsync**.

## <a name="important-things-to-remember"></a>Cosas importantes que recordar

Al usar la herramienta de seguimiento de cambio, hay algunas cosas que debes tener en cuenta para asegurarte de que todo funciona correctamente.

### <a name="buffer-overruns"></a>Saturaciones del búfer

Aunque se intenta reservar suficiente espacio en el seguimiento de cambios para contener todas las operaciones ocurre en el sistema hasta que la aplicación pueda leerlos, es muy fácil imaginarse un escenario donde la aplicación no lee los cambios antes de que el búfer circular sobrescribe propio. Especialmente si el usuario está restaurar los datos desde una copia de seguridad o sincronizar una gran colección de imágenes de su teléfono de la cámara.

En este caso, **ReadBatchAsync** devolverá el código de error [StorageLibraryChangeType.ChangeTrackingLost](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetype). Si la aplicación recibe este código de error, significa que un par de cosas:

* El búfer se sobrescribe propio desde la última vez que has revisado. El mejor curso de acción es volver a rastrear la biblioteca, porque cualquier información de la herramienta de seguimiento estará incompleto.
* El seguimiento de cambios no devolverá los cambios más hasta que se llama a [Restablecer](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.reset). Después de que la aplicación llama a restablecimiento, el puntero se moverá al cambio más reciente y seguimiento se reanudará normalmente.

Debería ser raro para obtener estos casos, pero en escenarios donde el usuario mueve un gran número de archivos alrededor de su disco no queremos que el seguimiento de cambios para globo y ocupan demasiado almacenamiento. Esto debe permitir que las aplicaciones reaccionar a las operaciones del sistema de archivos grandes y no dañar la experiencia del cliente de Windows.

### <a name="changes-to-a-storagelibrary"></a>Cambios en un StorageLibrary

La clase [StorageLibrary](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary) existe como un grupo de carpetas de raíz que contienen otras carpetas virtual. Para conciliar esto con un seguimiento de cambios de sistema de archivos, hemos realizado las siguientes opciones:

- Los cambios a los descendientes de las carpetas de la biblioteca de raíz se representará en el seguimiento de cambios. Las carpetas de la biblioteca de raíz se pueden encontrar mediante la propiedad de [carpetas](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.folders) .
- Agregar o quitar carpetas de la raíz de un **StorageLibrary** (a través de [RequestAddFolderAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.requestaddfolderasync) y [RequestRemoveFolderAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.requestremovefolderasync)) no creará una entrada en el seguimiento de cambios. Estos cambios pueden hacer el seguimiento a través del evento [DefinitionChanged](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.definitionchanged) o al enumerar las carpetas de raíz de la biblioteca mediante la propiedad de [carpetas](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.folders) .
- Si una carpeta con contenido ya en él se agrega a la biblioteca, no habrá un cambio de notificación o cambiar entradas rastreador generadas. Cualquier cambio posterior a los descendientes de esa carpeta se generar notificaciones y cambiar las entradas de la herramienta de seguimiento.

### <a name="calling-the-enable-method"></a>Llamar al método Enable

Las aplicaciones deben llamar a [Habilitar](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable) tan pronto como que inician el sistema de archivos de seguimiento y antes de cada enumeración de los cambios. Esto garantiza que se va a capturar todos los cambios realizados por el seguimiento de cambios.  

## <a name="putting-it-together"></a>Colocarlo juntos

Aquí es todo el código que se usa para registrar los cambios de la biblioteca de vídeos y empezar a extraer los cambios desde el seguimiento de cambios.

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
