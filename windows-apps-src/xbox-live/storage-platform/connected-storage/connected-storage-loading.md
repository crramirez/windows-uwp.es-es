---
title: Usar almacenamiento conectado para cargar datos
description: Aprenda a usar almacenamiento conectado para cargar datos.
ms.assetid: c660a456-fe7d-453a-ae7b-9ecaa2ba0a15
ms.date: 02/27/2018
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox, almacenamiento conectado
ms.localizationpriority: medium
ms.openlocfilehash: a2f7498e8063e290dc506de72b34d2c77d29b26e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57637140"
---
# <a name="use-connected-storage-to-load-data"></a>Usar almacenamiento conectado para cargar datos

Los datos se leen de forma asincrónica mediante el `ReadAsync` o `GetAsync` conectado el método de almacenamiento.

### <a name="to-load-data-from-connected-storage"></a>Para cargar datos de almacenamiento conectado

1.  Recuperar un `ConnectedStorageSpace` para el usuario mediante una llamada a `GetForUserAsync`.

    En el ejemplo XDK devuelto `ConnectedStorageSpace` se agrega a un mapa para permitir una administración sencilla de `ConnectedStorageSpace` objetos para varios usuarios.

2.  Crear un `ConnectedStorageContainer` mediante una llamada a `CreateContainer` en el `ConnectedStorageSpace`.
3.  Recuperar los datos mediante una llamada a `ReadAsync` o `GetAsync` en el `ConnectedStorageContainer`. `ReadAsync` requieren que se pasen en un búfer mientras `GetAsync` asigna nuevos búferes para almacenar los datos que se leen.

## <a name="c-xdk-sample"></a>Ejemplo de C++ XDK

```cpp
auto gConnectedStorageSpaceForUsers = ref new Platform::Collections::Map<unsigned int, Windows::Xbox::Storage::ConnectedStorageSpace^>();

bool GetUserInputYesOrNo() { return true; };

User^ gCurrentUser;
IBuffer^ WrapRawBuffer(void* ptr, size_t size);

// Acquire a Connected Storage space for a user. A Connected Storage space is required to manipulate Connected Storage Data.
void PrepareConnectedStorage(User^ user)
{
  auto op = ConnectedStorageSpace::GetForUserAsync(user);
  op->Completed = ref new AsyncOperationCompletedHandler<ConnectedStorageSpace^>(
    [=](IAsyncOperation<ConnectedStorageSpace^>^ operation, Windows::Foundation::AsyncStatus status)
    {
      switch(status)
      {
        case Windows::Foundation::AsyncStatus::Completed:
          gConnectedStorageSpaceForUsers->Insert(user->Id, operation->GetResults());
          break;
        case Windows::Foundation::AsyncStatus::Error:
        case Windows::Foundation::AsyncStatus::Canceled:
          // Present user option: ?Would you like to continue without saving progress??
          if( GetUserInputYesOrNo() )
            //If the users opts yes, continue in offline mode
          else
            //If the users opts no, retry.
          break;
      }
    });
}

void OnLoadCompleted(IAsyncAction^ action, Windows::Foundation::AsyncStatus status);

// Load data from a fixed container/blob name into an IBuffer
void Load(Windows::Storage::Streams::IBuffer^ buffer)
{

    auto reads = ref new Platform::Collections::Map<Platform::String^, Windows::Storage::Streams::IBuffer^>();
    reads->Insert("data", buffer);

    auto storageSpace = gConnectedStorageSpaceForUsers->Lookup(gCurrentUser->Id);
    auto container = storageSpace->CreateContainer("Saves/Checkpoint");

    //Save Data is Loading

    auto op = container->ReadAsync(reads->GetView());

    op->Completed = ref new AsyncActionCompletedHandler(OnLoadCompleted);
}

void OnLoadCompleted(IAsyncAction^ action, Windows::Foundation::AsyncStatus status)
{
    switch (action->Status)
    {
    case Windows::Foundation::AsyncStatus::Completed:
        //Successful load logic here.
        break;

    case Windows::Foundation::AsyncStatus::Error:
    case Windows::Foundation::AsyncStatus::Canceled:
        //Fail logic here
        break;

    default:
        //all other possible values of action->status are also failures, alternate fail logic here. 
        break;
    }
}
```

Puede encontrar que las API de almacenamiento conectado XDK documentados en el archivo .chm XDK bajo la ruta de acceso: **Xbox uno XDK >> referencia de API >> referencia de API de plataforma >> referencia de la API del sistema >> Windows.Xbox.Storage**.
Las API XDK también se documentan en el [developer.microsoft.com sitio](https://developer.microsoft.com/en-us/games/xbox/docs/xdk/storage-xbox-microsoft-n).
El vínculo a las API XDK requiere que tenga un Account(MSA) Microsoft que se ha habilitado para el acceso de Kit(XDK) para desarrolladores de Xbox.
Windows.Xbox.Storage es el nombre del espacio de nombres de almacenamiento conectado para las consolas Xbox One.

## <a name="c-uwp-sample"></a>C#Ejemplo UWP

Aunque juegos XDK y aplicaciones para UWP pueden usar diferentes API, la API de UWP se modela después de la API XDK muy de cerca. Para cargar los datos todavía deberá seguir los mismos pasos básicos al realizar la nota de algunos cambios de nombre de espacio de nombres y clase. En lugar de usar el espacio de nombres `Windows::Xbox::Storage` usará `Windows.Gaming.XboxLive.Storage`. La clase `ConnectedStorageSpace`, es equivalente a `GameSaveProvider`. La clase `ConnectedStorageContainer` es equivalente a `GameSaveContainer`. Estos cambios se describen más detalladamente en la sección de almacenamiento conectados de [trasladar Xbox Live código desde XDK a UWP](../../using-xbox-live/porting-xbox-live-code-from-xdk-to-uwp.md).

```csharp
//Namespace Required
Windows.Gaming.XboxLive.Storage

//Get The User
var users = await Windows.System.User.FindAllAsync();

int intData = 0;
const string c_saveBlobName = "Jersey";
const string c_saveContainerDisplayName = "GameSave";
const string c_saveContainerName = "GameSaveContainer";
GameSaveProvider gameSaveProvider;

GameSaveProviderGetResult gameSaveTask = await GameSaveProvider.GetForUserAsync(users[0], context.AppConfig.ServiceConfigurationId); 
//Parameters
//Windows.System.User user
//string SCID

if(gameSaveTask.Status == GameSaveErrorStatus.Ok)
{
    gameSaveProvider = gameSaveTask.Value;
}
else
{
    return;
    //throw new Exception("Game Save Provider Initialization failed");;
}

//Now you have a GameSaveProvider
//Next you need to call CreateContainer to get a GameSaveContainer

GameSaveContainer gameSaveContainer = gameSaveProvider.CreateContainer(c_saveContainerName);
//Parameter
//string name (name of the GameSaveContainer Created)

//form an array of strings containing the blob names you would like to read.
string[] blobsToRead = new string[] { c_saveBlobName };

// GetAsync allocates a new Dictionary to hold the retrieved data. You can also use ReadAsync
// to provide your own preallocated Dictionary.
GameSaveBlobGetResult result = await gameSaveContainer.GetAsync(blobsToRead);

int loadedData = 0;

//Check status to make sure data was read from the container
if(result.Status == GameSaveErrorStatus.Ok)
{
    //prepare a buffer to receive blob
    IBuffer loadedBuffer;

    //retrieve the named blob from the GetAsync result, place it in loaded buffer.
    result.Value.TryGetValue(c_saveBlobName, out loadedBuffer);

    if(loadedBuffer == null)
    {

        throw new Exception(String.Format("Didn't find expected blob \"{0}\" in the loaded data.", c_saveBlobName));

    }
    DataReader reader = DataReader.FromBuffer(loadedBuffer);
    loadedData = reader.ReadInt32();
}
```

Conectado a las API de almacenamiento para las aplicaciones para UWP se documentan en el [referencia de API de Xbox Live](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.xboxlive.storage).
Para ver otro ejemplo que usa la comprobación de almacenamiento conectado a la [Xbox Live API ejemplos juego Guardar proyecto](https://github.com/Microsoft/xbox-live-samples/tree/master/Samples/ID%40XboxSDK/GameSave).