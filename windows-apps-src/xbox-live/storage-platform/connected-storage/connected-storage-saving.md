---
title: Usar almacenamiento conectado para guardar los datos
description: Obtenga información sobre cómo usar el almacenamiento conectado para guardar los datos.
ms.assetid: ccf7488c-5d55-480e-b3aa-412220d03104
ms.date: 02/27/2018
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox, almacenamiento conectado
ms.localizationpriority: medium
ms.openlocfilehash: 4140e3bbe0f0ab229e3637008e01892f4179292e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57617620"
---
# <a name="use-connected-storage-to-save-data"></a>Usar almacenamiento conectado para guardar los datos


Los datos se guardan de forma asincrónica mediante la creación de un `ConnectedStorageContainer` en un `ConnectedStorageSpace` para un usuario y llamar a la `SubmitUpdatesAsync` método en el contenedor.

> [!IMPORTANT]
> Las dependencias de datos entre los contenedores de almacenamiento conectados no son seguras. Por ejemplo, la carga de un contenedor a la nube podría completarse, mientras que otro puede permanecer en la cola para cargar. Si el usuario se mueve a otra consola, la operación de sincronización permitiría que el primer contenedor sincronizado y tener acceso en la consola de la segunda, sin el primer contenedor está presente.

## <a name="to-save-data-to-connected-storage"></a>Para guardar datos en almacenamiento conectado

1.  Recuperar un `ConnectedStorageSpace` objeto para el usuario mediante una llamada a `GetForUserAsync`.

    En el ejemplo XDK devuelto `ConnectedStorageSpace` objeto se agrega a un mapa para permitir una administración sencilla de `ConnectedStorageSpace` objetos para varios usuarios.

2.  Crear un `ConnectedStorageContainer` objeto mediante una llamada a `CreateContainer` en el `ConnectedStorageSpace` objeto.
3.  Llame a `SubmitUpdatesAsync` en el `ConnectedStorageContainer` con juegos de almacenamiento blob de datos como el `blobsToWrite` parámetro.

## <a name="c-xdk-sample"></a>Ejemplo de C++ XDK

```cpp
auto gConnectedStorageSpaceForUsers = ref new Platform::Collections::Map<unsigned int, Windows::Xbox::Storage::ConnectedStorageSpace^>();

bool GetUserInputYesOrNo() {return true;};

User^ gCurrentUser;
IBuffer^ WrapRawBuffer( void* ptr, size_t size );

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

enum Color { RED, BLUE };
enum EngineSize { BIG, SMALL };

struct CarData
{
    Color color;
    bool hasWheels;
    bool hasFancyRims;
    EngineSize engineSize;
};


const int MAX_CARS = 10;

struct SaveData
{
    CarData cars[MAX_CARS];
    int numCars;
    int currentCar;
    int cash;
};

SaveData gMySaveData;

void SaveCheckpoint(Windows::Storage::Streams::IBuffer^ buffer, User^ user);

bool ItIsTimeToSaveACheckpoint() {return true;};

void Update()
{
    if (ItIsTimeToSaveACheckpoint())
        SaveCheckpoint(WrapRawBuffer(&gMySaveData,sizeof(SaveData)),gCurrentUser);
}


void SaveCheckpoint(Windows::Storage::Streams::IBuffer^ buffer, User^ user)
{
     auto storageSpace = gConnectedStorageSpaceForUsers->Lookup( user->Id );

     auto container = storageSpace->CreateContainer("Saves/Checkpoint");

     auto updates = ref new Platform::Collections::Map<Platform::String^, Windows::Storage::Streams::IBuffer^>();
     updates->Insert("data", buffer);

     auto op = container->SubmitUpdatesAsync(updates->GetView(), nullptr);

     //Save is happening here asynchronously

     op->Completed = ref new AsyncActionCompletedHandler(
               [=](IAsyncAction^ a, Windows::Foundation::AsyncStatus status){
                   //Save function has completed
                   //This area can be filled with further post save logic.
     });
}
```

Puede encontrar que las API de almacenamiento conectado XDK documentados en el archivo .chm XDK bajo la ruta de acceso: **Xbox uno XDK >> referencia de API >> referencia de API de plataforma >> referencia de la API del sistema >> Windows.Xbox.Storage**.
Las API XDK también se documentan en el [developer.microsoft.com sitio](https://developer.microsoft.com/en-us/games/xbox/docs/xdk/storage-xbox-microsoft-n).
El vínculo a las API XDK requiere que tenga un Account(MSA) Microsoft que se ha habilitado para el acceso de Kit(XDK) para desarrolladores de Xbox.
Windows.Xbox.Storage es el nombre del espacio de nombres de almacenamiento conectado para las consolas Xbox One.

## <a name="c-uwp-sample"></a>C#Ejemplo UWP

Aunque juegos XDK y aplicaciones para UWP pueden usar diferentes API, la API de UWP se modela después de la API XDK muy de cerca. Para guardar los datos todavía deberá seguir los mismos pasos básicos al realizar la nota de algunos cambios de nombre de espacio de nombres y clase. En lugar de usar el espacio de nombres `Windows::Xbox::Storage` usará `Windows.Gaming.XboxLive.Storage`. La clase `ConnectedStorageSpace`, es equivalente a `GameSaveProvider`. La clase `ConnectedStorageContainer` es equivalente a `GameSaveContainer`. Estos cambios se describen más detalladamente en la sección de almacenamiento conectados de [trasladar Xbox Live código desde XDK a UWP](../../using-xbox-live/porting-xbox-live-code-from-xdk-to-uwp.md).

```csharp
//Namespace Required
Windows.Gaming.XboxLive.Storage

//Get The User
var users = await Windows.System.User.FindAllAsync();

int intData = 23;
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
    //throw new Exception("Game Save Provider Initialization failed");
}

//Now you have a GameSaveProvider (formerly ConnectedStorageSpace)
//Next you need to call CreateContainer to get a GameSaveContainer (formerly ConnectedStorageContainer)

GameSaveContainer gameSaveContainer = gameSaveProvider.CreateContainer(c_saveContainerName); // this will create a new named game save container with the name = to the input name
//Parameter
//string name

// To store a value in the container, it needs to be written into a buffer, then stored with
// a blob name in a Dictionary.

DataWriter writer = new DataWriter();

writer.WriteInt32(intData); //some number you want to save, in this case 23.

IBuffer dataBuffer = writer.DetachBuffer();

var blobsToWrite = new Dictionary<string, IBuffer>();

blobsToWrite.Add(c_saveBlobName, dataBuffer);

GameSaveOperationResult gameSaveOperationResult = await gameSaveContainer.SubmitUpdatesAsync(blobsToWrite, null, c_saveContainerDisplayName);
//IReadOnlyDictionary<String, IBuffer> blobsToWrite
//IEnumerable<string> blobsToDelete
//string displayName
```

Conectado a las API de almacenamiento para las aplicaciones para UWP se documentan en el [referencia de API de Xbox Live](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.xboxlive.storage).
Para ver otro ejemplo que usa la comprobación de almacenamiento conectado a la [Xbox Live API ejemplos juego Guardar proyecto](https://github.com/Microsoft/xbox-live-samples/tree/master/Samples/ID%40XboxSDK/GameSave).