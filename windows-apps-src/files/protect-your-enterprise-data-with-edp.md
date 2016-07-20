---
author: TylerMSFT
Description: "En este tema se muestran ejemplos de las tareas de codificación necesarias para lograr realizar algunos de los escenarios más habituales de Enterprise Data Protection (EDP) relacionados con los archivos."
MS-HAID: dev\_files.protect\_your\_enterprise\_data\_with\_edp
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Usar Enterprise Data Protection (EDP) para proteger archivos
translationtype: Human Translation
ms.sourcegitcommit: 9b9e9ecb70f3a0bb92038ae94f45ddcee3357dbd
ms.openlocfilehash: a31fc65599f43be5b302b568774a51ab77065300

---

# Usar Enterprise Data Protection (EDP) para proteger archivos

> [!NOTE]
> La directiva de Enterprise Data Protection (EDP) no se puede aplicar en Windows10, versión 1511 (compilación 10586), ni en versiones anteriores.

En este tema se muestran ejemplos de las tareas de codificación necesarias para lograr realizar algunos de los escenarios más habituales de Enterprise Data Protection (EDP) relacionados con los archivos. Para obtener una perspectiva de desarrollador completa sobre cómo se relaciona EDP con los archivos, las secuencias, el portapapeles, las redes, las tareas en segundo plano y la protección de datos con la pantalla bloqueada, consulta [Enterprise Data Protection (EDP) (Protección de datos de empresa [EDP])](../enterprise/edp-hub.md).

> [!NOTE]
> En la [muestra de Enterprise Data Protection (EDP)](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409) se abarcan muchos de los escenarios que se muestran en este tema.

## Requisitos previos

-   **Prepararse para EDP**

    Consulta [Configurar el equipo para EDP](../enterprise/edp-hub.md#set-up-your-computer-for-EDP).

-   **Confirmar la compilación de una aplicación habilitada para empresas**

    Una aplicación que garantiza de forma independiente que los datos de la empresa se mantienen bajo el control administrativo de la empresa se conoce como una aplicación habilitada para empresas. Una aplicación habilitada es más eficaz, inteligente, flexible y confiable que una que no lo está. La aplicación indica al sistema que está habilitada al declarar la funcionalidad **enterpriseDataPolicy** restringida. Aun así, además de configurar una funcionalidad, hay que realizar otras operaciones para habilitar la aplicación. Para obtener más información, consulta [Aplicaciones habilitadas para empresas](../enterprise/edp-hub.md#enterprise-enlightened-apps).

-   **Comprender la programación asincrónica de las aplicaciones de la Plataforma universal de Windows (UWP)**

    Para aprender a escribir aplicaciones asincrónicas en C\# o Visual Basic, consulta [Llamar a API asincrónicas en C\# o Visual Basic](https://msdn.microsoft.com/library/windows/apps/mt187337). Para aprender a escribir aplicaciones asincrónicas en C++, consulta [Asynchronous programming in C++ (Programación asincrónica en C++)](https://msdn.microsoft.com/library/windows/apps/mt187334).

## Ruta de acceso de la carpeta local y vista de los archivos protegidos en el Explorador de archivos


Puedes crear una aplicación que hospede los ejemplos de código de este tema para probarlos. Los ejemplos de código escriben archivos en la carpeta local de la aplicación, y la ubicación exacta de esa carpeta en el disco es única para tu aplicación. Para determinar la ubicación de la carpeta local de la aplicación, puedes agregar el código siguiente.

```CSharp
// Put a breakpoint on the line after the line of code below. Use the value of localFolderPath
// in File Explorer to find the output file that the later code examples create.
string localFolderPath = ApplicationData.Current.LocalFolder.Path;
```

Cuando tengas la ruta de acceso, podrás usar el Explorador de archivos para buscar fácilmente los archivos que crea la aplicación. De esta forma, podrás confirmar que están protegidos y con la identidad correcta.

En el Explorador de archivos, selecciona **Cambiar opciones de carpeta y búsqueda** y, en la pestaña **Vista**, activa **Mostrar los archivos cifrados en color**. Usa también el comando **Vista**&gt;**Agregar columnas** del Explorador de archivos para agregar la columna **Cifrado para** y poder ver la identidad de la empresa a la que estás protegiendo los archivos.

## Proteger los datos de la empresa en un archivo nuevo (para una aplicación interactiva)


Existen muchas formas en las que los datos de la empresa pueden entrar en tu aplicación como, por ejemplo, desde determinados extremos de red, archivos, el Portapapeles o desde el contrato para contenido compartido. También es posible que tu aplicación cree nuevos datos empresariales. Sean cuales sean los medios por los cuales la aplicación habilitada reciba datos empresariales, la aplicación deberá tener cuidado para proteger los datos para la identidad de la empresa administrada cuando conserva los datos en un nuevo archivo.

Los pasos básicos consisten en usar una API de almacenamiento normal para crear el archivo, usar una API de EDP para proteger el archivo en la identidad empresarial y, a continuación, (de nuevo, mediante las API de almacenamiento normal) escribir en el archivo. Asegúrate de proteger el archivo antes de escribir en él (tal como se muestra en el ejemplo siguiente). Usa el método [**FileProtectionManager.ProtectAsync**](https://msdn.microsoft.com/library/windows/apps/dn705157) para proteger el archivo. Recuerda que, como siempre, solo tiene sentido proteger el archivo en una identidad si esa identidad está administrada. Para obtener más información sobre por qué se produce ese caso y la manera en que tu aplicación puede determinar la identidad de la empresa en la que se está ejecutando, consulta [Confirming an identity is managed (Confirmar que una identidad está administrada)](../enterprise/edp-hub.md#confirming-an-identity-is-managed).

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage;

...

private async void SaveEnterpriseDataToFile(string enterpriseData, string identity)
{
    // You should only protect to an identity that is managed.
    if (!ProtectionPolicyManager.IsIdentityManaged(identity)) return false;

    StorageFolder storageFolder = ApplicationData.Current.LocalFolder;
    StorageFile storageFile = storageFolder.CreateFileAsync("sample.txt",
        CreationCollisionOption.ReplaceExisting);

    // It's important to protect a file *before* writing enterprise data to it.
    FileProtectionInfo fileProtectionInfo =
        await FileProtectionManager.ProtectAsync(storageFile, identity);

    // If the file is now protected, to the intended identity, then we can write to it.
    if (fileProtectionInfo.Identity == identity &&
        fileProtectionInfo.Status == FileProtectionStatus.Protected)
        await Windows.Storage.FileIO.WriteTextAsync(storageFile, enterpriseData);
}
```

## Proteger los datos empresariales en un archivo nuevo (para una tarea en segundo plano)


La API [**FileProtectionManager.ProtectAsync**](https://msdn.microsoft.com/library/windows/apps/dn705157) que hemos usado en la sección anterior solo es adecuada para aplicaciones interactivas. En el caso de una tarea en segundo plano, el código puede ejecutarse mientras la pantalla está bloqueada. Además, es posible que la empresa esté administrando una directiva de seguridad de protección de datos con la pantalla bloqueada (DPL), en la que las claves de cifrado que se necesitan para obtener acceso a los recursos protegidos se quitan temporalmente de la memoria del dispositivo cuando este último esté bloqueado. Esto impide la pérdida de datos si el dispositivo se pierde. La función también quita las claves asociadas con los archivos protegidos cuando se cierran sus identificadores. Sin embargo, es posible crear nuevos archivos protegidos mientras la ventana de bloqueo está activa (el tiempo entre el bloqueo del dispositivo y su desbloqueo) y obtener acceso a ellos mientras se mantiene abierto el identificador de archivo. 
            **StorageFolder.CreateFileAsync** cierra el identificador cuando se crea el archivo, por lo que este algoritmo no se puede usar.

1.  Crea un archivo nuevo mediante **StorageFolder.CreateFileAsync**.
2.  Cífralo mediante **FileProtectionManager.ProtectAsync**.
3.  Abre el archivo y escribe en él.

Dado que el paso 1 implica cerrar el identificador de archivo (incluso si en el paso 1 no se cierra el identificador, se haría en el paso 2), no es posible realizar el paso 3 porque las claves de cifrado que se necesitan para acceder a ese archivo ya no están disponibles.

Para abordar este escenario, puedes usar [**FileProtectionManager.CreateProtectedAndOpenAsync**](https://msdn.microsoft.com/library/windows/apps/dn705153) para crear un archivo protegido y devolverle un objeto **IRandomAccessStream**. Esto permite a las aplicaciones realizar varias escrituras en un archivo mientras se mantiene abierto el identificador de archivo.

En el código de ejemplo se muestra una aplicación de correo sencilla que escribe un nuevo archivo cuando se recibe correo nuevo. Las aplicaciones de correo deben sincronizar el correo electrónico cuando el dispositivo esté bloqueado. Cuando esta aplicación recibe correo nuevo, crea un nuevo archivo mediante [**CreateProtectedAndOpenAsync**](https://msdn.microsoft.com/library/windows/apps/dn705153), recupera una secuencia de salida y escribe el cuerpo del correo en ella.

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage;
using Windows.Storage.Streams;

...

private async void SaveEnterpriseDataToFile(string enterpriseData, string identity)
{
    // You should only protect to an identity that is managed.
    if (!ProtectionPolicyManager.IsIdentityManaged(identity)) return false;

    StorageFolder storageFolder = ApplicationData.Current.LocalFolder;

    ProtectedFileCreateResult protectedFileCreateResult =
        await FileProtectionManager.CreateProtectedAndOpenAsync(storageFolder,
            "sample.txt", identity, CreationCollisionOption.ReplaceExisting);

    // It's important to successfully protect a file *before* writing enterprise data to it.
    if (protectedFileCreateResult.ProtectionInfo.Identity == identity &&
        protectedFileCreateResult.ProtectionInfo.Status == FileProtectionStatus.Protected)
    {
        using (IOutputStream outputStream =
            protectedFileCreateResult.Stream.GetOutputStreamAt(0))
        {
            using (DataWriter writer = new DataWriter(outputStream))
            {
                writer.WriteString(enterpriseData);
                await writer.StoreAsync();
                await writer.FlushAsync();
            }
        }
    }
    else if (protectedFileCreateResult.ProtectionInfo.Status == FileProtectionStatus.AccessSuspended)
    {
        // Perform any special processing for the access suspended case.
    }
}
```

## Proteger una carpeta cuyo propósito es contener datos empresariales


Si lo deseas, también puedes hacer que todos los elementos de dentro de una carpeta estén protegidos. Puedes usar [**FileProtectionManager.ProtectAsync**](https://msdn.microsoft.com/library/windows/apps/dn705157) para proteger una carpeta vacía. A continuación, se protegerá cualquier archivo o carpeta que se cree posteriormente dentro de la carpeta. Puedes proteger una carpeta existente o crear una carpeta para proteger (en el ejemplo siguiente se crea una carpeta). En cualquier caso, para que la protección se realice correctamente, la carpeta debe estar vacía en ese momento. Si no lo está, [**FileProtectionInfo.Status**](https://msdn.microsoft.com/library/windows/apps/dn705150) contendrá un valor de la enumeración [**FileProtectionStatus.NotProtectable**](https://msdn.microsoft.com/library/windows/apps/dn279147).

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage;

...

private async void CreateANewFolderAndProtectItAsync(string folderName, string identity)
{
    if (!ProtectionPolicyManager.IsIdentityManaged(identity)) return false;

    StorageFolder storageFolder = ApplicationData.Current.LocalFolder;
    StorageFolder newStorageFolder =
        await storageFolder.CreateFolderAsync(folderName);

    FileProtectionInfo fileProtectionInfo =
        await FileProtectionManager.ProtectAsync(newStorageFolder, identity);

    if (fileProtectionInfo.Identity != identity ||
        fileProtectionInfo.Status != FileProtectionStatus.Protected)
    {
        // Protection failed.
    }
}
```

## Copiar la protección de un archivo a otro


En este caso, ya existe un archivo que sabes que está protegido en la identidad de la empresa adecuada. Puedes replicar esa protección en otro archivo muy fácilmente. Ni siquiera necesitas saber cuál es la identidad: solo necesitas saber que es la correcta.

Para realizar una copia simple de un archivo protegido, puedes llamar a [**StorageFile.CopyAsync**](https://msdn.microsoft.com/library/windows/apps/br227190). La copia del archivo resultante tendrá la misma protección que el original.

Para proteger un archivo desprotegido ya existente antes de escribir datos empresariales en él, un método alternativo a realizar una llamada a [**FileProtectionManager.ProtectAsync**](https://msdn.microsoft.com/library/windows/apps/dn705157) (el cual vimos en un escenario anterior y para el que debes pasar una identidad administrada), es hacer una llamada a [**FileProtectionManager.CopyProtectionAsync**](https://msdn.microsoft.com/library/windows/apps/dn705152), tal como se muestra en el ejemplo de código.

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage;

...

private async void CopyProtectionFromOneFileToAnother
    (StorageFile sourceStorageFile, StorageFile targetStorageFile)
{
    bool copyResult = await
        FileProtectionManager.CopyProtectionAsync(sourceStorageFile, targetStorageFile);

    if (!copyResult)
    {
        // Copying failed. To diagnose, you could check the file's status.
        // (call FileProtectionManager.GetProtectionInfoAsync and
        // check FileProtectionInfo.Status).
    }
}
```

## Controlar el acceso denegado a un archivo que has protegido


En este caso, la aplicación intenta obtener acceso a un archivo que anteriormente protegió y al que se le deniega el acceso. Deberás comprobar el estado del archivo para ver dónde está el error. En este ejemplo de código, la aplicación llama a la API [**FileProtectionManager.GetProtectionInfoAsync**](https://msdn.microsoft.com/library/windows/apps/dn705154) para consultar el estado del archivo y determinar si el motivo es que el acceso al mismo se revocó como resultado de la administración remota.

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage;

...

private async System.Threading.Tasks.Task<bool> IsFileProtectionStatusRevoked
    (StorageFile storageFile)
{
    FileProtectionInfo fileProtectionInfo =
        await FileProtectionManager.GetProtectionInfoAsync(storageFile);

    if (fileProtectionInfo.Status == FileProtectionStatus.Revoked)
    {
        // Inform the user that their data has been revoked.
    }
    return fileProtectionInfo.Status == FileProtectionStatus.Revoked;
}
```

## Habilitar la aplicación de directivas de la interfaz de usuario en función de la identidad de un archivo protegido


Cuando la aplicación está por mostrar el contenido de un archivo protegido (como un archivo PDF) en su interfaz de usuario, debe habilitar la aplicación de directivas de interfaz de usuario en función de la identidad en la que el archivo está protegido. Debes consultar la información de protección del archivo y habilitar la aplicación de directivas de la interfaz de usuario del sistema desde la identidad recuperada.

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage;

...

private async void EnableUIPolicyFromFile(StorageFile storageFile)
{
    FileProtectionInfo fileProtectionInfo = 
        await FileProtectionManager.GetProtectionInfoAsync(storageFile);

    if (fileProtectionInfo.Status != FileProtectionStatus.Protected)
    {
        // No policy enforcement required, unless the file is inaccessible
        // (Revoked, ProtectedToOtherIdentity) in which case that should
        // be handled in an app-specific way.
        return;
    }

    ProtectionPolicyManager.TryApplyProcessUIPolicy(fileProtectionInfo.Identity);
}
```

> '!NOTA] Este artículo está orientado a desarrolladores de Windows10 que programan aplicaciones para la Plataforma universal de Windows (UWP). Si estás desarrollando para Windows8.x o Windows Phone8.x, consulta la [documentación archivada](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

## Temas relacionados

- [Muestra de Enterprise data protection (EDP)](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409)

- [**Espacio de nombres Windows.Security.EnterpriseData**](https://msdn.microsoft.com/library/windows/apps/dn279153)








<!--HONumber=Jul16_HO1-->


