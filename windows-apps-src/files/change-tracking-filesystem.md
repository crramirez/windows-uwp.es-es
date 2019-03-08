---
title: Realizar un seguimiento de los cambios del sistema de archivos en segundo plano
description: Describe cómo realizar el seguimiento de cambios en archivos y carpetas en segundo plano como usuarios moverlos por el sistema.
ms.date: 12/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b0ec7762fd64f0f0b8de65faa1aaf079bdaba3a3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57621580"
---
# <a name="track-file-system-changes-in-the-background"></a>Realizar un seguimiento de los cambios del sistema de archivos en segundo plano

**API importantes**

-   [**StorageLibraryChangeTracker**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageLibraryChangeTracker)
-   [**StorageLibraryChangeReader**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader)
-   [**StorageLibraryChangedTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.StorageLibraryContentChangedTrigger)
-   [**StorageLibrary**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary)

El [ **StorageLibraryChangeTracker** ](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageLibraryChangeTracker) clase permite que las aplicaciones realizar un seguimiento de cambios en archivos y carpetas a los usuarios les desplazarse por el sistema. Mediante el **StorageLibraryChangeTracker** (clase), una aplicación puede realizar un seguimiento:

- Operaciones de agregar archivos, eliminar, modificar.
- Operaciones de carpeta, como cambios de nombre y las eliminaciones.
- Los archivos y carpetas mover en la unidad.

Utilice esta guía para conocer el modelo de programación para trabajar con el seguimiento de cambios, ver algunos ejemplos de código y comprender los distintos tipos de operaciones de archivo que realiza el seguimiento **StorageLibraryChangeTracker**.

**StorageLibraryChangeTracker** funciona para las bibliotecas de usuario, o para cualquier carpeta en el equipo local. Esto incluye unidades secundarias o las unidades extraíbles, pero no incluye las unidades NAS o unidades de red.

## <a name="using-the-change-tracker"></a>Usar el seguimiento de cambios

El seguimiento de cambios se implementa en el sistema como un búfer circular almacenar la última *N* las operaciones de sistema de archivos. Las aplicaciones pueden leer los cambios en el búfer y, a continuación, procesarlos en sus propias experiencias. Una vez finalizada la aplicación con los cambios marca los cambios como procesado y nunca verlos nuevamente.

Para usar el seguimiento de cambios en una carpeta, siga estos pasos:

1. Habilitar el seguimiento de cambios para la carpeta.
2. Espere a que los cambios.
3. Leer los cambios.
4. Aceptar cambios.

Las secciones siguientes se guiarán a través de cada uno de los pasos con algunos ejemplos de código. Ejemplo de código completo se proporciona al final del artículo.

### <a name="enable-the-change-tracker"></a>Habilitar el seguimiento de cambios

Lo primero que la aplicación debe hacer es indicar al sistema que está interesada en una biblioteca concreta de seguimiento de cambios. Esto hace una llamada a la [ **habilitar** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable) método en el seguimiento de cambios para la biblioteca de interés.

```csharp
StorageLibrary videosLib = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
StorageLibraryChangeTracker videoTracker = videosLib.ChangeTracker;
videoTracker.Enable();
```

Algunas notas importantes:

- Asegúrese de que la aplicación tiene permiso para la biblioteca en el manifiesto correcta antes de crear el [ **StorageLibrary** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary) objeto. Consulte [permisos de acceso de archivo](https://docs.microsoft.com/en-us/windows/uwp/files/file-access-permissions) para obtener más detalles.
- [**Habilitar** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable) es seguro para subprocesos, no se restablecerá el puntero y pueden llamarse tantas veces como sea necesario (volveremos sobre esto más adelante).

![Habilitar un seguimiento de cambios vacío](images/changetracker-enable.png)

### <a name="wait-for-changes"></a>Espere a que los cambios

Una vez inicializado el seguimiento de cambios, comenzará a registrar todas las operaciones que se producen dentro de una biblioteca, aunque no se está ejecutando la aplicación. Las aplicaciones pueden registrarse para activarse siempre que hay un cambio al registrarse para obtener el [ **StorageLibraryChangedTrigger** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.StorageLibraryContentChangedTrigger) eventos.

![Cambios que se va a agregar al seguimiento de cambios sin la aplicación leerlos](images/changetracker-waiting.png)

### <a name="read-the-changes"></a>Leer los cambios

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
> Es la segunda llamada a habilitar para defenderse contra una condición de carrera si el usuario agrega otra carpeta a la biblioteca mientras la aplicación lee los cambios. Sin la llamada adicional a **habilitar** el código se producirá un error con ecSearchFolderScopeViolation (0 x 80070490) si el usuario está cambiando las carpetas en la biblioteca

### <a name="accept-the-changes"></a>Aceptar los cambios

Después de realiza la aplicación de procesamiento de los cambios, debe indicar al sistema nunca volver a mostrar esos cambios llamando el [ **AcceptChangesAsync** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.acceptchangesasync) método.

```csharp
await changeReader.AcceptChangesAsync();
```

![Marcar los cambios como leídos por lo que nunca se mostrarán nuevo](images/changetracker-accepting.png)

La aplicación ahora solo recibirá nuevos cambios al leer el seguimiento de cambios en el futuro.

- Si se han producido cambios entre llamar a [ **ReadBatchAsync** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.readbatchasync) y [AcceptChangesAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.acceptchangesasync), será el puntero solo avanza hasta el último cambio que ha visto la aplicación. Esos otros cambios todavía estará disponibles la próxima vez que llama a **ReadBatchAsync**.
- No acepta los cambios harán que el sistema devolver el mismo conjunto de cambios la próxima vez las llamadas de aplicación **ReadBatchAsync**.

## <a name="important-things-to-remember"></a>Cosas importantes que recordar

Al usar el seguimiento de cambios, hay algunas cosas que debe tener en cuenta para asegurarse de que todo funciona correctamente.

### <a name="buffer-overruns"></a>Saturaciones del búfer

Aunque se intenta reservar suficiente espacio en el seguimiento de cambios para contener todas las operaciones que se producen en el sistema hasta que la aplicación puede leerlas, es muy fácil imaginar un escenario donde la aplicación no lee los cambios antes de que el búfer circular sobrescribe propio. Especialmente si el usuario es restaurar datos desde una copia de seguridad o la sincronización de una colección grande de imágenes desde su teléfono con cámara.

En este caso, **ReadBatchAsync** devolverá el código de error [ **StorageLibraryChangeType.ChangeTrackingLost**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetype). Si la aplicación recibe este código de error, significa que un par de cosas:

* El búfer ha sobrescrito a sí mismo desde la última vez que se ve. La mejor forma de acción es volver a rastrear la biblioteca, porque toda la información de la herramienta de seguimiento estará incompleta.
* El seguimiento de cambios no devolverá más cambios hasta que llame a [ **restablecer**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.reset). Después de la aplicación llama a restablecer, se moverá el puntero para el cambio más reciente y seguimiento se reanudará con normalidad.

Debería ser excepcional para obtener estos casos, pero en escenarios donde el usuario está moviendo un gran número de archivos en su disco no queremos que el seguimiento de cambios para aumentar y tardar demasiado almacenamiento. Esto debe permitir que las aplicaciones reaccionar a las operaciones del sistema de archivos grandes y no dañar la experiencia del cliente en Windows.

### <a name="changes-to-a-storagelibrary"></a>Cambios realizados en un StorageLibrary

El [ **StorageLibrary** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary) clase existe como un grupo virtual de las carpetas raíz que contiene otras carpetas. Para conciliar esto con un seguimiento de cambios de archivo del sistema, hemos realizado las siguientes opciones:

- Los cambios realizados en los descendientes de las carpetas raíz de la biblioteca se representará en el seguimiento de cambios. Las carpetas raíz de la biblioteca se pueden encontrar mediante el [ **carpetas** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.folders) propiedad.
- Agregar o quitar carpetas raíz de un **StorageLibrary** (a través de [ **RequestAddFolderAsync** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.requestaddfolderasync) y [ **RequestRemoveFolderAsync**  ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.requestremovefolderasync)) no creará una entrada en el seguimiento de cambios. Se pueden realizar el seguimiento de estos cambios a través de la [ **DefinitionChanged** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.definitionchanged) eventos, o bien recorriendo las carpetas raíz de la biblioteca con el [ **carpetas** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.folders) propiedad.
- Si una carpeta con contenido en él se agrega a la biblioteca, no habrá un cambio de notificación o cambio entradas de seguimiento generadas. Los cambios subsiguientes a los descendientes de esa carpeta generará las notificaciones y cambiar las entradas de seguimiento.

### <a name="calling-the-enable-method"></a>Una llamada al método Enable

Las aplicaciones deben llamar [ **habilitar** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable) en cuanto inicie el sistema de archivos de seguimiento y antes de cada enumeración de los cambios. Así se asegurará de que se capturarán todos los cambios realizados por el seguimiento de cambios.  

## <a name="putting-it-together"></a>Incorporación de todos

Aquí está todo el código que se usa para registrar los cambios de la biblioteca de vídeos y extraer los cambios desde el seguimiento de cambios.

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
