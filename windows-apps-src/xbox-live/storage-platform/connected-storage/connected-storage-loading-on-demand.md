---
title: Conectado a la carga de almacenamiento bajo demanda
description: Obtenga información sobre cómo cargar datos de almacenamiento conectado a petición, en lugar de a la vez.
ms.assetid: a0797a14-c972-4017-864c-c6ba0d5a3363
ms.date: 02/27/2018
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox, almacenamiento conectado
ms.localizationpriority: medium
ms.openlocfilehash: 31c1893f09e6d56682b4e718ee8b905ce72c7ad8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57631720"
---
# <a name="connected-storage-loading-on-demand"></a>Conectado a la carga de almacenamiento bajo demanda

`GetSyncOnDemandForUserAsync` permite cargar datos de respaldo en la nube de un espacio de almacenamiento conectadas "a petición" en lugar de a la vez. Esto puede mejorar el rendimiento a través de `GetForUserAsync` casos donde guarda los archivos son especialmente grandes.

## <a name="to-load-data-from-a-connected-storage-space-on-demand"></a>Para cargar datos de un espacio de almacenamiento conectado a petición

### <a name="1--call-getsyncondemandforuserasync"></a>1:  Llamar a `GetSyncOnDemandForUserAsync`

Esto desencadena una sincronización parcial que descarga una lista de contenedores y sus metadatos de la nube, pero no su contenido. Esta operación es rápida y, en condiciones de red buena, el usuario no debería ver cualquier interfaz de usuario de carga.

```cpp
auto op = ConnectedStorageSpace::GetSyncOnDemandForUserAsync(user);
op->Completed = ref new AsyncOperationCompletedHandler<ConnectedStorageSpace^>(
    [=](IAsyncOperation<ConnectedStorageSpace^>^ operation, Windows::Foundation::AsyncStatus status)
{
    switch (status)
    {
    case Windows::Foundation::AsyncStatus::Completed:
        auto storageSpace = operation->GetResults();
        break;
    case Windows::Foundation::AsyncStatus::Error:
    case Windows::Foundation::AsyncStatus::Canceled:
        // Present user option: ?Would you like to continue without saving progress??
        if (GetUserInputYesOrNo())
            SetGameState(LoadSaveState::NO_SAVE_MODE);
        else
            SetGameState(LoadSaveState::RETRY_LOAD);
        break;
    }
});
```

```csharp
var users = await Windows.System.User.FindAllAsync();

GameSaveProvider gameSaveProvider;

GameSaveProviderGetResult gameSaveTask = await GameSaveProvider.GetSyncOnDemandForUserAsync(users[0], context.AppConfig.ServiceConfigurationId); 
//Paramaters
//Windows.System.User user
//string SCID

if(gameSaveTask.Status == GameSaveErrorStatus.Ok)
{
    gameSaveProvider = gameSaveTask.Value;
}
```


### <a name="2--perform-a-container-query-using-getcontainerinfo2async"></a>2:  Realizar una consulta de contenedor mediante `GetContainerInfo2Async`

Esto devolverá una colección de `ContainerInfo2`, que contiene 3 nuevos campos de metadatos:

    -   `DisplayName`: Cualquier nombre para mostrar que se ha escrito mediante la sobrecarga de `SubmitUpdatesAsync` que toma una cadena de nombre para mostrar como un parámetro. Se recomienda usar siempre este campo para almacenar un nombre descriptivo.
    -   `LastModifiedTime`: La última vez que se modificó este contenedor. Tenga en cuenta que en el caso de conflicto locales y remotos las marcas de tiempo, se usa las remotas.
    -   `NeedsSync`: Un valor booleano que indica si este contenedor tiene datos que debe descargarse desde la nube.

    Con estos metadatos adicionales, puede presentar al usuario con la información principal acerca de sus juegos guardados (incluidos nombre, usa la última vez y, si selecciona una requerirá una descarga) sin tener que realizar una descarga completa de la nube.

```cpp
auto containerQuery = storageSpace->CreateContainerInfoQuery(nullptr); //return list of containers in ConnectedStorageSpace
auto queryOperation = containerQuery->GetContainerInfo2Async();

queryOperation->Completed = ref new AsyncOperationCompletedHandler<IVectorView<ContainerInfo2>^ >( 
    [=] (IAsyncOperation<IVectorView<ContainerInfo2>^ >^ operation, Windows::Foundation::AsyncStatus status)
    {
        switch (status)
        {
        case Windows::Foundation::AsyncStatus::Completed:
            // get the resulting vector of container info
            auto infoVector = operation->GetResults();
            break;
        case Windows::Foundation::AsyncStatus::Error:
        case Windows::Foundation::AsyncStatus::Canceled:
            // handle error cases
            break;
        }
    });
```

```csharp
GameSaveContainerInfoQuery infoQuery = gameSaveProvider.createContainerInfoQuery();
GameSaveContainerInfoGetResult containerInfoResult = await infoQuery.GetContainerInfoAsync();
var containerInfoList;

if(containerInfoResult.Status == GameSaveErrorStatus.Ok)
{
    containerInfoList = containerInfoResult.value;
}

// Use the containerInfoList to inform further actions or display container data to user. 
```

### <a name="3--trigger-a-sync"></a>3:  Desencadenar una sincronización

Se desencadenará una synce almacenamiento conectado mediante una llamada a cualquiera de la siguiente API de almacenamiento conectados existente:

**C++**

    -   BlobInfoQueryResult::GetBlobInfoAsync
    -   BlobInfoQueryResult::GetItemCountAsync
    -   ConnectedStorageContainer::GetAsync
    -   ConnectedStorageContainer::ReadAsync
    -   ConnectedStorageSpace::DeleteContainerAsync

**C#**

    -   GameSaveBlobInfoQuery.GetBlobInfoAsync
    -   GameSaveBlobInfoQuery.GetItemCountAsync
    -   GameSaveContainer.GetAsync
    -   GameSaveContainer.ReadAsync
    -   GameSaveProvider.DeleteContainerAsync

Esto hará que el usuario ver la barra de la interfaz de usuario y el progreso de sincronización habitual, tal como se descargan datos de su contenedor seleccionado. Tenga en cuenta que solo los datos desde el contenedor seleccionado está sincronizados; no se descargan datos desde otros contenedores.

Al llamar a estas API en el contexto de una sincronización a petición en, estas operaciones pueden todos producir los nuevos códigos de error siguiente:

-   `ConnectedStorageErrorStatus::ContainerSyncFailed`(`GameSaveErrorStatus.ContainerSyncFailed` en UWP C# API): Este error indica que el error en la operación y el contenedor no se pudo sincronizar con la nube. La causa más probable es que las condiciones de red del usuario produjo errores de la sincronización. Desean vuelva a intentarlo una vez que han solucionado su red o que pueden optar por usar un contenedor que no tienen sincronización. La interfaz de usuario debe permitir que cualquiera de estas opciones. Ningún cuadro de diálogo de reintento es necesaria, puesto que ya habrá visto el cuadro de diálogo de reintento de la interfaz de usuario del sistema.

-   `ConnectedStorageErrorStatus::ContainerNotInSync`(`GameSaveErrorStatus.ContainerNotInSync` en UWP C# API): Este error indica que el título incorrectamente intentó escribir en un contenedor no están sincronizado. Una llamada a `ConnectedStorageContainer::SubmitUpdatesAsync`(`GameSaveContainer.SubmitUpdatesAsync` en UWP C# API) en un contenedor que tiene la marca NeedsSync establecido en true no se permite. En primer lugar, debe leer un contenedor para desencadenar una sincronización antes de escribir en él. Si se escribe en un contenedor sin leer de él, es probable que el título tiene un error que podría perder el progreso de usuario.

Este comportamiento es diferente cuando un usuario juega sin conexión. Aunque sin conexión, no hay ninguna indicación de si los contenedores están sincronizados, por lo que es el usuario quien debe resolver los conflictos en un momento posterior. En este caso, sin embargo, el sistema sepa que el usuario debe realizar la sincronización, por lo que no les permitirá obtener en mal estado mediante un contenedor obsoleto (aunque si lo desean, todavía puede reiniciar el título y reproducirla sin conexión).

### <a name="4--use-the-rest-of-the-connected-storage-api-as-normal"></a>4:  Usar el resto de la API de almacenamiento conectado como normal

Comportamiento de almacenamiento conectado permanece sin cambios durante la sincronización a petición.
