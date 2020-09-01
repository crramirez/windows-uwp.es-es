---
description: Use una barra de progreso dentro de la notificación del sistema para transmitir el estado de las operaciones de ejecución prolongada al usuario.
title: Barra de progreso del sistema y enlace de datos
label: Toast progress bar and data binding
template: detail.hbs
ms.date: 12/07/2017
ms.topic: article
keywords: Windows 10, UWP, notificación del sistema, barra de progreso, barra de progreso del sistema, notificación, enlace de datos del sistema
ms.localizationpriority: medium
ms.openlocfilehash: 4219154a3fe3241b9c1871c07a1fbbb2b63f2348
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174609"
---
# <a name="toast-progress-bar-and-data-binding"></a>Barra de progreso del sistema y enlace de datos

El uso de una barra de progreso dentro de la notificación del sistema le permite transmitir el estado de las operaciones de ejecución prolongada al usuario, como las descargas, la representación de vídeo, los objetivos del ejercicio y mucho más.

> [!IMPORTANT]
> **Requiere Creators Update y 1.4.0 de la biblioteca de notificaciones**: debe tener como destino el SDK 15063 y ejecutar la compilación 15063 o superior para usar barras de progreso en notificaciones del sistema. Debe usar la versión 1.4.0 o posterior de la [biblioteca de NuGet de notificaciones](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) de la comunidad de UWP para construir la barra de progreso en el contenido de la notificación del sistema.

Una barra de progreso dentro de una notificación del sistema puede ser "indeterminada" (sin valor específico, puntos animados que indican que se está produciendo una operación) o "determinando" (se rellena un porcentaje específico de la barra, como 60%).

> **API importantes**: [clase NotificationData](/uwp/api/windows.ui.notifications.notificationdata), [método ToastNotifier. Update](/uwp/api/Windows.UI.Notifications.ToastNotifier.Update), [clase ToastNotification](/uwp/api/Windows.UI.Notifications.ToastNotification)

> [!NOTE]
> Solo Desktop admite barras de progreso en las notificaciones del sistema. En otros dispositivos, la barra de progreso se quitará de la notificación.

En la imagen siguiente se muestra una barra de progreso de determinación con todas sus propiedades correspondientes etiquetadas.

<img alt="Toast with progress bar properties labeled" src="images/toast-progressbar-annotated.png" width="626"/>

| Propiedad | Tipo | Obligatorio | Descripción |
|---|---|---|---|
| **Título** | string o [BindableString](toast-schema.md#bindablestring) | false | Obtiene o establece una cadena de título opcional. Admite el enlace de datos. |
| **Valor** | Double o [AdaptiveProgressBarValue](toast-schema.md#adaptiveprogressbarvalue) o [BindableProgressBarValue](toast-schema.md#bindableprogressbarvalue) | false | Obtiene o establece el valor de la barra de progreso. Admite el enlace de datos. El valor predeterminado es 0. Puede ser un valor de tipo Double entre 0,0 y 1,0, `AdaptiveProgressBarValue.Indeterminate` o `new BindableProgressBarValue("myProgressValue")` . |
| **ValueStringOverride** | string o [BindableString](toast-schema.md#bindablestring) | false | Obtiene o establece una cadena opcional que se va a mostrar en lugar de la cadena de porcentaje predeterminada. Si no se proporciona, se mostrará algo como "70%". |
| **Estado** | string o [BindableString](toast-schema.md#bindablestring) | true | Obtiene o establece una cadena de estado (obligatorio), que se muestra debajo de la barra de progreso de la izquierda. Esta cadena debe reflejar el estado de la operación, como "descargando..." o "instalando..." |


Aquí se muestra cómo generaría la notificación más arriba...

```csharp
ToastContent content = new ToastContent()
{
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = "Downloading your weekly playlist..."
                },
 
                new AdaptiveProgressBar()
                {
                    Title = "Weekly playlist",
                    Value = 0.6,
                    ValueStringOverride = "15/26 songs",
                    Status = "Downloading..."
                }
            }
        }
    }
};
```

```xml
<toast>
    <visual>
        <binding template="ToastGeneric">
            <text>Downloading your weekly playlist...</text>
            <progress
                title="Weekly playlist"
                value="0.6"
                valueStringOverride="15/26 songs"
                status="Downloading..."/>
        </binding>
    </visual>
</toast>
```

Sin embargo, deberá actualizar dinámicamente los valores de la barra de progreso para que realmente sea "activo". Esto puede hacerse mediante el enlace de datos para actualizar la notificación del sistema.


## <a name="using-data-binding-to-update-a-toast"></a>Usar el enlace de datos para actualizar una notificación del sistema

El uso del enlace de datos implica los siguientes pasos...

1. Construir contenido del sistema de notificación que use campos enlazados a datos
2. Asignar una **etiqueta** (y, opcionalmente, un **Grupo**) a su **ToastNotification**
3. Definir los valores de **datos** iniciales en el **ToastNotification**
4. Enviar la notificación del sistema
5. Uso de **etiquetas** y **grupos** para actualizar los valores de **datos** con nuevos valores

En el fragmento de código siguiente se muestran los pasos 1-4. El siguiente fragmento de código mostrará cómo actualizar los valores de **datos** del sistema.

```csharp
using Windows.UI.Notifications;
using Microsoft.Toolkit.Uwp.Notifications;
 
public void SendUpdatableToastWithProgress()
{
    // Define a tag (and optionally a group) to uniquely identify the notification, in order update the notification data later;
    string tag = "weekly-playlist";
    string group = "downloads";
 
    // Construct the toast content with data bound fields
    var content = new ToastContent()
    {
        Visual = new ToastVisual()
        {
            BindingGeneric = new ToastBindingGeneric()
            {
                Children =
                {
                    new AdaptiveText()
                    {
                        Text = "Downloading your weekly playlist..."
                    },
    
                    new AdaptiveProgressBar()
                    {
                        Title = "Weekly playlist",
                        Value = new BindableProgressBarValue("progressValue"),
                        ValueStringOverride = new BindableString("progressValueString"),
                        Status = new BindableString("progressStatus")
                    }
                }
            }
        }
    };
 
    // Generate the toast notification
    var toast = new ToastNotification(content.GetXml());
 
    // Assign the tag and group
    toast.Tag = tag;
    toast.Group = group;
 
    // Assign initial NotificationData values
    // Values must be of type string
    toast.Data = new NotificationData();
    toast.Data.Values["progressValue"] = "0.6";
    toast.Data.Values["progressValueString"] = "15/26 songs";
    toast.Data.Values["progressStatus"] = "Downloading...";
 
    // Provide sequence number to prevent out-of-order updates, or assign 0 to indicate "always update"
    toast.Data.SequenceNumber = 1;
 
    // Show the toast notification to the user
    ToastNotificationManager.CreateToastNotifier().Show(toast);
}
```

Después, si desea cambiar los valores de **datos** , use el método [**Update**](/uwp/api/Windows.UI.Notifications.ToastNotifier.Update) para proporcionar los nuevos datos sin volver a construir toda la carga del sistema.

```csharp
using Windows.UI.Notifications;
 
public void UpdateProgress()
{
    // Construct a NotificationData object;
    string tag = "weekly-playlist";
    string group = "downloads";
 
    // Create NotificationData and make sure the sequence number is incremented
    // since last update, or assign 0 for updating regardless of order
    var data = new NotificationData
    {
        SequenceNumber = 2
    };

    // Assign new values
    // Note that you only need to assign values that changed. In this example
    // we don't assign progressStatus since we don't need to change it
    data.Values["progressValue"] = "0.7";
    data.Values["progressValueString"] = "18/26 songs";

    // Update the existing notification's data by using tag/group
    ToastNotificationManager.CreateToastNotifier().Update(data, tag, group);
}
```

El uso del método **Update** en lugar de reemplazar toda la notificación del sistema también garantiza que la notificación del sistema se mantiene en la misma posición en el centro de actividades y no se desplaza hacia arriba o hacia abajo. Sería bastante confuso para el usuario si la notificación del sistema estaba pasando a la parte superior del centro de actividades cada pocos segundos mientras se rellena la barra de progreso.

El método **Update** devuelve una enumeración, [**NotificationUpdateResult**](/uwp/api/windows.ui.notifications.notificationupdateresult), que le permite saber si la actualización se ha realizado correctamente o si no se ha podido encontrar la notificación (lo que significa que el usuario probablemente haya descartado la notificación y debe dejar de enviarle actualizaciones). No se recomienda extraer otra notificación de sistema hasta que se haya completado la operación de progreso (como cuando se complete la descarga).


## <a name="elements-that-support-data-binding"></a>Elementos que admiten el enlace de datos
Los siguientes elementos de las notificaciones del sistema admiten el enlace de datos

- Todas las propiedades de **AdaptiveProgress**
- Propiedad de **texto** en los elementos **AdaptiveText** de nivel superior


## <a name="update-or-replace-a-notification"></a>Actualizar o reemplazar una notificación

Desde Windows 10, siempre podía **reemplazar** una notificación mediante el envío de una nueva notificación con la misma **etiqueta** y el mismo **Grupo**. ¿Cuál es la diferencia entre **reemplazar** la notificación del sistema y **Actualizar** los datos del sistema de notificaciones?

| | Reemplace | Actualizando |
| -- | -- | --
| **Posición en el centro de actividades** | Mueve la notificación a la parte superior del centro de actividades. | Deja la notificación en el centro de actividades. |
| **Modificar contenido** | Puede cambiar completamente todo el contenido o el diseño de la notificación del sistema | Solo se pueden cambiar las propiedades que admiten el enlace de datos (barra de progreso y texto de nivel superior) |
| **Mostrar como emergente** | Puede volver a aparecer como una ventana emergente del sistema si deja **SuppressPopup** establecido en `false` (o se establece en true para enviarlo al centro de actividades de forma silenciosa) | No volverá a aparecer como un elemento emergente; los datos del sistema se actualizan de forma silenciosa en el centro de actividades |
| **Usuario descartado** | Independientemente de si el usuario ha descartado la notificación anterior, siempre se enviará la notificación de sustitución | Si el usuario ha descartado la notificación del sistema, se producirá un error en la actualización del sistema |

En general, la **actualización es útil para...**

- Información que cambia con frecuencia en un breve período de tiempo y no requiere que se ponga al frente de la atención del usuario
- Cambios sutiles en el contenido de la notificación del sistema, como el cambio del 50% al 65%

A menudo, una vez completada la secuencia de actualizaciones (al igual que el archivo se ha descargado), se recomienda reemplazar para el paso final, ya que...

- La notificación final probablemente tiene cambios de diseño drásticos, como la eliminación de la barra de progreso, la adición de nuevos botones, etc.
- Es posible que el usuario haya descartado la notificación de progreso pendiente, ya que no le importa ver la descarga, pero aún desea recibir una notificación cuando se complete la operación.


## <a name="related-topics"></a>Temas relacionados

- [Muestra de código completo en GitHub](https://github.com/WindowsNotifications/quickstart-toast-progress-bar)
- [Documentación del contenido del sistema](adaptive-interactive-toasts.md)