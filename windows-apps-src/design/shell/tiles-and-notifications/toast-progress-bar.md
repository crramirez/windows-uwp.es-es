---
author: andrewleader
Description: Learn how to use a progress bar within your toast notification.
title: Enlace de datos y barra de progreso de notificación del sistema
label: Toast progress bar and data binding
template: detail.hbs
ms.author: mijacobs
ms.date: 12/7/2017
ms.topic: article
keywords: windows 10, uwp, notificación del sistema, barra de progreso, barra de progreso de notificación del sistema, notificación, enlace de datos de notificación del sistema
ms.localizationpriority: medium
ms.openlocfilehash: 6ca144f92676f87fcdade37b280c39640bc74624
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2018
ms.locfileid: "7570572"
---
# <a name="toast-progress-bar-and-data-binding"></a>Enlace de datos y barra de progreso de notificación del sistema

El uso de una barra de progreso dentro de la notificación del sistema te permite transmitir el estado de las operaciones de larga duración para el usuario, como descargas, representación de vídeo, objetivos de ejercicio y mucho más.

> [!IMPORTANT]
> **Requiere Creators Update y 1.4.0 de la biblioteca de notificaciones**: debes utilizar SDK 15063 y estar ejecutando la compilación 15063 o superior para usar las barras de progreso en las notificaciones del sistema. Debes usar la versión 1.4.0 o superior de la [Biblioteca NuGet de notificaciones del Kit de herramientas de la comunidad de UWP](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) para construir la barra de progreso en el contenido de tu notificación del sistema.

Una barra de progreso dentro de una notificación del sistema puede ser "indeterminada" (ningún valor específico, los puntos animados indican que se está produciendo una operación) o "determinada" (se rellena un porcentaje específico de la barra, como el 60%).

> **API importantes**: [clase NotificationData](https://docs.microsoft.com/uwp/api/windows.ui.notifications.notificationdata), [método ToastNotifier.Update](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotifier.Update), [clase ToastNotification](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotification)

> [!NOTE]
> Solo Escritorio admite barras de progreso en notificaciones del sistema. En otros dispositivos, la barra de progreso se quitará de la notificación.

La siguiente imagen muestra una barra de progreso determinada con todas sus propiedades etiquetadas correspondientes.

<img alt="Toast with progress bar properties labeled" src="images/toast-progressbar-annotated.png" width="626"/>

| Propiedad | Tipo | Obligatorio | Descripción |
|---|---|---|---|
| **Título** | cadena o [BindableString](toast-schema.md#bindablestring) | falso | Obtiene o establece una cadena de título opcional. Admite el enlace de datos. |
| **Valor** | doble o [AdaptiveProgressBarValue](toast-schema.md#adaptiveprogressbarvalue) o [BindableProgressBarValue](toast-schema.md#bindableprogressbarvalue) | falso | Obtiene o establece el valor de la barra de progreso. Admite el enlace de datos. El valor predeterminado es 0. Puede ser uno doble entre 0,0 y 1,0, `AdaptiveProgressBarValue.Indeterminate`, o `new BindableProgressBarValue("myProgressValue")`. |
| **ValueStringOverride** | cadena o [BindableString](toast-schema.md#bindablestring) | falso | Obtiene o establece una cadena opcional que se mostrará en lugar de la cadena de porcentaje predeterminada. Si no se proporciona, se mostrará algo similar a "70%". |
| **Estado** | cadena o [BindableString](toast-schema.md#bindablestring) | true | Obtiene o establece una cadena de estado (obligatoria), que se muestra debajo de la barra de progreso a la izquierda. Esta cadena debe reflejar el estado de la operación, como "Descargando…" o "Instalando..." |


Aquí es cómo generaría la notificación vista arriba...

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

Sin embargo, deberás actualizar dinámicamente los valores de la barra de progreso para que pueda ser realmente "dinámica". Esto se puede hacer mediante el enlace de datos para actualizar la notificación del sistema.


## <a name="using-data-binding-to-update-a-toast"></a>Usar el enlace de datos para actualizar una notificación del sistema

El uso del enlace de datos implica los pasos siguientes...

1. Construir el contenido de notificación del sistema que utiliza los campos enlazados de datos
2. Asignar una **Etiqueta** (y, opcionalmente, un **Grupo**) a tu **ToastNotification**
3. Definir tus valores de **Datos** iniciales en tu **ToastNotification**
4. Enviar la notificación del sistema
5. Usar **Etiqueta** y **Grupo** para actualizar los valores de **Datos** valores con los nuevos valores

El siguiente fragmento de código muestra los pasos del 1 al 4. El siguiente fragmento de código muestra cómo actualizar los valores de **Datos** de la notificación del sistema.

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

A continuación, cuando quieras cambiar tus valores de **Datos**, usa el método [**Update**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotifier.Update) para ofrecer los nuevos datos sin volver a crear la carga de notificación del sistema completo.

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

El uso del método **Update** en lugar de reemplazar la notificación del sistema completa también garantiza que la notificación del sistema permanece en la misma posición del Centro de actividades y que no se mueve hacia arriba o hacia abajo. Sería muy confuso para el usuario que la notificación del sistema se mantuviera saltando a la parte superior del Centro de actividades cada pocos segundos mientras se rellena la barra de progreso.

El método **Update** devuelve una enumeración [**NotificationUpdateResult**](https://docs.microsoft.com/uwp/api/windows.ui.notifications.notificationupdateresult), que te permitirá saber si la actualización se realizó correctamente o si no se encontró la notificación (lo que significa que el usuario probablemente ha descartado la notificación y debe dejar de enviarle actualizaciones). No se recomienda la aparición de otra notificación del sistema hasta que no haya finalizado la operación de progreso (por ejemplo, cuando se complete la descarga).


## <a name="elements-that-support-data-binding"></a>Elementos que admiten el enlace de datos
Los siguientes elementos de las notificaciones del sistema admiten el enlace de datos:

- Todas las propiedades de **AdaptiveProgress**
- La propiedad **Text** de los elementos **AdaptiveText** de nivel superior


## <a name="update-or-replace-a-notification"></a>Actualizar o reemplazar una notificación

Desde Windows 10, siempre podrías **reemplazar** una notificación enviando una notificación del sistema nueva con la misma **Etiqueta** y **Grupo**. ¿Cuál es la diferencia entre **reemplazar** la notificación del sistema y **actualizar** los datos de la notificación del sistema?

| | Reemplazar | Actualizándose |
| -- | -- | --
| **Posición en el Centro de actividades** | Mueve la notificación a la parte superior del Centro de actividades. | Deja la notificación colocada en el Centro de actividades. |
| **Modificar el contenido** | Puede cambiar completamente todo el contenido o el diseño de la notificación del sistema. | Solo puede cambiar las propiedades que admiten el enlace de datos (barra de progreso y texto de nivel superior). |
| **Volver a aparecer como emergente** | Puede volver a aparecer como un elemento emergente de notificación del sistema si dejas **SuppressPopup** establecido en `false` (o establecido en true para enviarlo de forma silenciosa al Centro de actividades). | No volverá a aparecer como elemento emergente; los datos de la notificación del sistema se actualizan de forma silenciosa en el Centro de actividades. |
| **Usuario descartado** | Con independencia de si el usuario descartó la notificación anterior, siempre se enviará la notificación del sistema de reemplazo. | Si el usuario descartó la notificación del sistema, se producirá un error en la actualización de la notificación del sistema. |

En general, **la actualización es útil para...**

- Información que cambia con frecuencia en un período breve de tiempo y que no requiere que se lleve a primera vista de la atención del usuario.
- Cambios sutiles del contenido de la notificación del sistema, como cambiar del 50% al 65%

En ocasiones, una vez que se haya completado la secuencia de las actualizaciones (como que se haya descargado el archivo), se recomienda reemplazar para el paso final, ya que...

- Tu notificación final probablemente tiene cambios de diseño drásticos, como la eliminación de la barra de progreso, la adición de nuevos botones, etc.
- Es posible que el usuario haya descartado la notificación de progreso pendiente dado que no le interesa ver su descarga, pero aun así desea recibir una notificación con una notificación del sistema emergente cuando se complete la operación.


## <a name="related-topics"></a>Temas relacionados

- [Muestra de código completo en GitHub](https://github.com/WindowsNotifications/quickstart-toast-progress-bar)
- [Documentación del contenido de la notificación del sistema](adaptive-interactive-toasts.md)