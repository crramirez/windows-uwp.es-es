---
title: Colecciones del sistema
description: Obtenga información sobre cómo organizar las notificaciones del sistema para la aplicación mediante la creación, actualización o eliminación de las colecciones de notificaciones en el centro de actividades.
label: Toast Collections
template: detail.hbs
ms.date: 05/16/2018
ms.topic: article
keywords: Windows 10, UWP, notificación, recopilaciones, recopilación, notificaciones de grupo, notificaciones de agrupación, grupo, organizar, centro de actividades, notificación del sistema
ms.localizationpriority: medium
ms.openlocfilehash: aff6b933e04611013761c10ad7a76824f7347855
ms.sourcegitcommit: eb725a47c700131f5975d737bd9d8a809e04943b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "88970073"
---
# <a name="grouping-toast-notifications-with-collections"></a>Agrupación de notificaciones del sistema con colecciones
Use recopilaciones para organizar las notificaciones del sistema de la aplicación en el centro de actividades. Las colecciones ayudan a los usuarios a encontrar información en el centro de actividades más fácilmente y permiten a los desarrolladores administrar mejor las notificaciones.  Las API siguientes permiten quitar, crear y actualizar colecciones de notificaciones.

> [!IMPORTANT]
> **Requiere Creators Update**: debe tener como destino el SDK 15063 y ejecutar la compilación 15063 o superior para usar las recopilaciones del sistema. Entre las API relacionadas se incluyen [Windows. UI. notifications. ToastCollection](https://docs.microsoft.com/uwp/api/windows.ui.notifications.toastcollection)y [Windows. UI. notifications. ToastCollectionManager](https://docs.microsoft.com/uwp/api/windows.ui.notifications.toastcollectionmanager)

Puede ver el ejemplo siguiente con una aplicación de mensajería que separa las notificaciones basadas en el grupo de chat. cada título (COMP SCI 160A Project chat, Direct messages, lacrosse Team chat) es una colección independiente.  Observe cómo las notificaciones se agrupan de forma distintiva como si fueran de una aplicación independiente, aunque se trata de todas las notificaciones de la misma aplicación.  Si está buscando una forma más sutil de organizar las notificaciones, consulte encabezados del [sistema](toast-headers.md).  
![Ejemplo de colección con dos grupos de notificaciones diferentes](images/toast-collection-example.png)

## <a name="creating-collections"></a>Crear colecciones
Al crear cada colección, es necesario proporcionar un nombre para mostrar y un icono, que se muestran dentro del centro de actividades como parte del título de la colección, como se muestra en la imagen anterior. Las colecciones también requieren un argumento Launch para ayudar a la aplicación a navegar a la ubicación correcta dentro de la aplicación cuando el usuario hace clic en el título de la colección.  

### <a name="create-a-collection"></a>Creación de una colección

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

## <a name="sending-notifications-to-a-collection"></a>Envío de notificaciones a una colección
Trataremos el envío de notificaciones de tres canalizaciones del sistema: locales, programadas e inserciones.  En cada uno de estos ejemplos vamos a crear una notificación de ejemplo para enviar con el código que se muestra a continuación, nos centraremos en cómo agregar la notificación del sistema a una colección a través de cada canalización.

Construya la carga de notificación:

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

### <a name="add-a-scheduled-toast-to-a-collection"></a>Agregar un sistema de notificación programado a una recopilación

``` csharp
// Create scheduled toast from XML above
ScheduledToastNotification scheduledToast = new ScheduledToastNotification(toastXml, DateTimeOffset.Now.AddSeconds(10));

// Get notifier
var notifier = await ToastNotificationManager.GetDefault().GetToastNotifierForToastCollectionIdAsync(MainPage.toastCollectionId);
    
// Add to schedule
notifier.AddToSchedule(scheduledToast);
```

### <a name="send-a-push-toast-to-a-collection"></a>Enviar una notificación de envío a una recopilación
En el caso de las notificaciones de extracción, debe agregar el encabezado X-WNS-CollectionId al mensaje POST.
```csharp
// Add header to HTTP request
request.Headers.Add("X-WNS-CollectionId", collectionId); 

```

## <a name="managing-collections"></a>Administrar colecciones
#### <a name="create-the-toast-collection-manager"></a>Crear el administrador de la colección de notificaciones
En el resto de los fragmentos de código de esta sección "administrar colecciones", usaremos el collectionManager siguiente.
```csharp
ToastCollectionManger collectionManager = ToastNotificationManager.GetDefault().GetToastCollectionManager();
```

#### <a name="get-all-collections"></a>Obtener todas las colecciones

``` csharp
IReadOnlyList<ToastCollection> collections = await collectionManager.FindAllToastCollectionsAsync();
``` 

#### <a name="get-the-number-of-collections-created"></a>Obtiene el número de colecciones creadas

``` csharp
int toastCollectionCount = (await collectionManager.FindAllToastCollectionsAsync()).Count;
```

#### <a name="remove-a-collection"></a>Quitar una colección

``` csharp
await collectionManager.RemoveToastCollectionAsync(MainPage.toastCollectionId);
```

#### <a name="update-a-collection"></a>Actualizar una colección
Puede actualizar las colecciones creando una nueva colección con el mismo identificador y guardando la nueva instancia de la colección.
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
#### <a name="group-and-tag-properties"></a>Propiedades de grupo y etiqueta
Las propiedades de grupo y etiqueta juntas identifican de forma única una notificación dentro de una colección.  Group (y tag) sirve como clave principal compuesta (más de un identificador) para identificar la notificación. Por ejemplo, si desea quitar o reemplazar una notificación, tiene que ser capaz de especificar *la notificación* que desea quitar o reemplazar; para ello, especifique la etiqueta y el grupo. Un ejemplo es una aplicación de mensajería.  El desarrollador podría usar el identificador de conversación como grupo y el identificador del mensaje como etiqueta.

#### <a name="remove-a-toast-from-a-collection"></a>Quitar una notificación del sistema de una colección
Puede quitar notificaciones del sistema individuales mediante los ID. de etiqueta y de grupo, o borrar todas las notificaciones del sistema de una colección.
``` csharp
// Get the history
var collectionHistory = await ToastNotificationManager.GetDefault().GetHistoryForToastCollectionAsync(MainPage.toastCollectionId);

// Remove toast
collectionHistory.Remove(tag, group); 
```

#### <a name="clear-all-toasts-within-a-collection"></a>Borrar todas las notificaciones del sistema de una colección
``` csharp
// Get the history
var collectionHistory = await ToastNotificationManager.GetDefault().GetHistoryForToastCollectionAsync(MainPage.toastCollectionId);

// Remove toast
collectionHistory.Clear();
```


## <a name="collections-in-notifications-visualizer"></a>Recopilaciones en el visualizador de notificaciones
Puede usar la herramienta [visualizador de notificaciones](notifications-visualizer.md) como ayuda para diseñar sus colecciones. Siga estos pasos:

* Haga clic en el icono de engranaje en la esquina inferior derecha. 
* Seleccione "colecciones del sistema".
* Encima de la vista previa de la notificación del sistema, hay un menú desplegable "colección de notificaciones". Seleccione administrar recopilaciones.
* Haga clic en ' agregar colección ', rellene los detalles de la colección y guárdelos.
* Puede agregar más recopilaciones o hacer clic fuera del cuadro administrar recopilaciones para volver a la pantalla principal.
* Seleccione la colección a la que desea agregar la notificación del sistema en el menú desplegable "colección de notificaciones".
* Al activar la notificación del sistema, se agregará a la recopilación correspondiente en el centro de actividades.


## <a name="other-details"></a>Otros detalles
Las recopilaciones del sistema que cree también se reflejarán en la configuración de notificaciones del usuario.  Los usuarios pueden alternar la configuración de cada colección individual para activar o desactivar estos subgrupos.  Si las notificaciones están desactivadas en el nivel superior de la aplicación, también se desactivarán todas las notificaciones de recopilación.  Además, cada colección mostrará de forma predeterminada 3 notificaciones en el centro de actividades y el usuario puede expandirlo para mostrar hasta 20 notificaciones.

## <a name="related-topics"></a>Temas relacionados

* [Contenido de notificaciones del sistema](adaptive-interactive-toasts.md)
* [Encabezados del sistema](toast-headers.md)
* [Biblioteca de notificaciones en GitHub (parte del kit de herramientas de la comunidad de Windows)](https://github.com/windows-toolkit/WindowsCommunityToolkit/tree/master/Microsoft.Toolkit.Uwp.Notifications)