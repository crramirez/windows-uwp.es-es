---
author: manoskow
Description: Learn how to group notifications in Action Center using collections.
title: Colecciones de notificaciones del sistema
label: Toast Collections
template: detail.hbs
ms.author: mijacobs
ms.date: 05/16/2018
ms.topic: article
keywords: windows 10, uwp, notificación, colecciones, colección, agrupar notificaciones, agrupación de notificaciones, agrupar, organizar el centro de actividades, notificación del sistema
ms.localizationpriority: medium
ms.openlocfilehash: be7c4ec2e9a47eeeb00663ae94f89e44c6751352
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "6848771"
---
# <a name="grouping-toast-notifications-with-collections"></a>Agrupación de notificaciones del sistema con colecciones
Usa colecciones para organizar las notificaciones del sistema de la aplicación en el Centro de actividades. Las colecciones ayudan a los usuarios a encontrar con mayor facilidad información en el Centro de actividades y permitir a los desarrolladores administrar mejor sus notificaciones.  Las API siguientes permiten la eliminación, creación y actualización de colecciones de notificaciones.

> [!IMPORTANT]
> **Requiere la actualización Creators Update**: debes utilizar SDK 15063 y estar ejecutando la compilación 15063 o superior para usar las colecciones de notificaciones del sistema. Entre las API relacionadas se incluyen [Windows.UI.Notifications.ToastCollection](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastcollection) y [Windows.UI.Notifications.ToastCollectionManager](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastcollectionmanager).

Puedes ver el ejemplo siguiente con una aplicación de mensajería que separa las notificaciones basadas en el grupo de chat; cada título (chat de proyecto Comp Sci 160A, mensajes directos, Lacrosse Team Chat) es una colección independiente.  Ten en cuenta cómo se agrupan de manera diferente las notificaciones como si procedieran de una aplicación independiente, aunque todas las notificaciones provienen de la misma aplicación.  Si buscando una manera más sutil de organizar las notificaciones, consulta [encabezados de notificación del sistema](toast-headers.md).  
![Ejemplo de colección con dos grupos diferentes de notificaciones](images/toast-collection-example.png)

## <a name="creating-collections"></a>Creación de colecciones
Al crear cada colección, debes proporcionar un nombre para mostrar y un icono, que se muestran en el Centro de actividades como parte del título de la colección, como se muestra en la imagen anterior. Las colecciones también requieren un argumento de inicio para ayudar a la aplicación a navegar a la ubicación correcta dentro de la aplicación cuando el usuario hace clic en el título de la colección.  

### <a name="create-a-collection"></a>Crear una colección

``` csharp 
public const string toastCollectionId = "ToastCollection";

// Create a toast collection
public async void CreateToastCollection()
{
    string displayName = "Work Email"; 
    string launchArg = "NavigateToWorkEmailInbox"; 
    Uri icon = new Windows.Foundation.Uri("ms-appx:///Assets/workEmail.png");

    // Constructor
    ToastCollection workEmailToastCollection = new ToastCollection(MainPage.toastCollectionId, 
        displayName,
        launchArg, 
        icon);

    // Calls the platform to create the collection
    await ToastNotificationManager.GetDefault().GetToastCollectionManager().SaveToastCollectionAsync(workEmailToastCollection);                                 
}
```

## <a name="sending-notifications-to-a-collection"></a>Enviar notificaciones a una colección
Trataremos el envío de notificaciones desde tres canalizaciones de notificaciones del sistema diferentes: local, programada y de inserción.  Para cada uno de estos ejemplos crearemos una notificación del sistema de muestra para enviarla con el código que se muestra inmediatamente a continuación y, a continuación nos centraremos en cómo agregar la notificación del sistema a una colección mediante cada canalización.

Construir la carga de notificación:

``` csharp
public const string toastCollectionId = "MyToastCollection";

public async void SendToastToToastCollection()
{
    // Construct the notification Content
    string toastXmlString = 
        $@"<toast launch=’’>
            <visual>
                <binding template=’ToastGeneric’>
                    <text>Hello,</text>
                    <text>it’s me</text>
                </binding>
            </visual>
        </toast>";
    // Convert to XML
    XmlDocument toastXml = new XmlDocment();
    toastXml.LoadXml(toastXmlString);
    ToastNotification toast = new ToastNotification(toastXml);
```

### <a name="send-a-toast-to-a-collection"></a>Enviar una notificación del sistema a una colección

```csharp
// Get the collection notifier
var notifier = await ToastNotificationManager.GetDefault().GetToastNotifierForToastCollectionIdAsync(MainPage.toastCollectionId);

// And show the toast
notifier.Show(toast);
```

### <a name="add-a-scheduled-toast-to-a-collection"></a>Agregar una notificación del sistema programada a una colección

``` csharp
// Create scheduled toast from XML above
ScheduledToastNotification scheduledToast = new ScheduledToastNotification(toastXml, DateTimeOffset.Now.AddSeconds(10));

// Get notifier
var notifier = await ToastNotificationManager.GetDefault().GetToastNotifierForToastCollectionIdAsync(MainPage.toastCollectionId);
    
// Add to schedule
notifier.AddToSchedule(scheduledToast);
```

### <a name="send-a-push-toast-to-a-collection"></a>Enviar una notificación del sistema de inserción a una colección
Para notificaciones del sistema de inserción, debes agregar el encabezado X-WNS-CollectionId a los mensajes POST.
```csharp
// Add header to HTTP request
request.Headers.Add("X-WNS-CollectionId", collectionId); 

```

## <a name="managing-collections"></a>Gestión de colecciones
#### <a name="create-the-toast-collection-manager"></a>Crear el administrador de la colección del sistema
Para el resto de los fragmentos de código de esta sección "Administrar colecciones" vamos a usar el siguiente collectionManager.
```csharp
ToastCollectionManger collectionManager = ToastNotificationManager.GetDefault().GetToastCollectionManager();
```

#### <a name="get-all-collections"></a>Obtener todas las colecciones

``` csharp
IReadOnlyList<ToastCollection> collections = await collectionManager.FindAllToastCollectionsAsync();
``` 

#### <a name="get-the-number-of-collections-created"></a>Obtener el número de colecciones creadas

``` csharp
int toastCollectionCount = (await collectionManager.FindAllToastCollectionsAsync()).Count;
```

#### <a name="remove-a-collection"></a>Quitar una colección

``` csharp
await collectionManager.RemoveToastCollectionAsync(MainPage.toastCollectionId);
```

#### <a name="update-a-collection"></a>Actualizar una colección
Para actualizar colecciones, crea una nueva colección con el mismo id. y guarda la nueva instancia de la colección.
``` csharp
string displayName = "Updated Display Name"; 
string launchArg = "UpdatedLaunchArgs"; 
Uri icon = new Windows.Foundation.Uri("ms-appx:///Assets/updatedPicture.png");

// Construct a new toast collection with the same collection id
ToastCollection updatedToastCollection = new ToastCollection(MainPage.toastCollectionId, 
            displayName,
            launchArg, 
            icon);

// Calls the platform to update the collection by saving the new instance
await collectionManager.SaveToastCollectionAsync(updatedToastCollection);                               
```
## <a name="managing-toasts-within-a-collection"></a>Administrar notificaciones del sistema dentro de una colección
#### <a name="group-and-tag-properties"></a>Propiedades de grupos y etiquetas
Las propiedades de grupos y etiquetas identifican de manera única y conjunta una notificación dentro de una colección.  Grupo (y etiqueta) actúa como clave principal compuesta (más de un identificador) para identificar la notificación. Por ejemplo, si quieres quitar o reemplazar una notificación, tienes que poder especificar *qué notificación* quieres quitar o reemplazar. Para ello, especifica la etiqueta y el grupo. Un ejemplo es una aplicación de mensajería.  El desarrollador podría usar el id. de conversación como el grupo y el id. del mensaje como la etiqueta.

#### <a name="remove-a-toast-from-a-collection"></a>Quitar una notificación del sistema de una colección
Puedes quitar notificaciones del sistema individuales con los id. de etiquetas y grupos, o borrar todas las notificaciones del sistema de una colección.
``` csharp
// Get the history
var collectionHistory = await ToastNotificationManager.GetDefault().GetHistoryForToastCollectionAsync(MainPage.toastCollectionId);

// Remove toast
collectionHistory.Remove(tag, group); 
```

#### <a name="clear-all-toasts-within-a-collection"></a>Borrar todas las notificaciones del sistema dentro de una colección
``` csharp
// Get the history
var collectionHistory = await ToastNotificationManager.GetDefault().GetHistoryForToastCollectionAsync(MainPage.toastCollectionId);

// Remove toast
collectionHistory.Clear();
```


## <a name="collections-in-notifications-visualizer"></a>Colecciones de Notifications Visualizer
Puedes usar la herramienta [Notifications Visualizer](notifications-visualizer.md) para ayudar a diseñar tus colecciones. Sigue los pasos siguientes:

* Haz clic en el icono del engranaje de la esquina inferior derecha. 
* Selecciona "Colecciones de notificaciones del sistema".
* Encima de la vista previa de la notificación del sistema, hay un menú desplegable "Colección de notificaciones del sistema". Selecciona Administrar colecciones.
* Haz clic en "Agregar colección", rellena los datos de la colección y guarda.
* Puedes agregar más colecciones o hacer clic fuera del cuadro Administrar colecciones para volver a la pantalla principal.
* Selecciona la colección a la que quieres agregar la notificación del sistema en el menú desplegable "Colección de notificaciones del sistema".
* Cuando desencadenes la notificación del sistema, se agregará a la colección correspondiente en el Centro de actividades.


## <a name="other-details"></a>Otros detalles
Las colecciones de notificaciones del sistema que crees también se reflejarán en la configuración de notificaciones del usuario.  Los usuarios pueden alternar la configuración de cada colección individual para activar o desactivar estos subgrupos.  Si las notificaciones estén desactivadas en el nivel superior de la aplicación, todas las notificaciones de las colecciones también se desactivarán.  Además, de manera predeterminada cada colección mostrará 3 notificaciones en el Centro de actividades y el usuario podrá expandirla para mostrar hasta 20 notificaciones.

## <a name="related-topics"></a>Temas relacionados

* [Contenido de notificaciones del sistema](adaptive-interactive-toasts.md)
* [Encabezados de notificaciones del sistema](toast-headers.md)
* [Biblioteca de notificaciones en GitHub (parte del Kit de herramientas de Comunidad Windows)](https://github.com/Microsoft/UWPCommunityToolkit/tree/master/Microsoft.Toolkit.Uwp.Notifications)