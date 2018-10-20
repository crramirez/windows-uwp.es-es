---
author: andrewleader
Description: Adaptive and interactive toast notifications let you create flexible pop-up notifications with more content, optional inline images, and optional user interaction.
title: Contenido de notificaciones del sistema
ms.assetid: 1FCE66AF-34B4-436A-9FC9-D0CF4BDA5A01
label: Toast content
template: detail.hbs
ms.author: mijacobs
ms.date: 11/20/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, notificaciones del sistema, notificaciones del sistema interactivas, notificaciones del sistema adaptables, contenido de notificaciones del sistema, carga de notificaciones del sistema
ms.localizationpriority: medium
ms.openlocfilehash: de999528d07e6bd7d243e53708e9afc465004af7
ms.sourcegitcommit: 72835733ec429a5deb6a11da4112336746e5e9cf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2018
ms.locfileid: "5159106"
---
# <a name="toast-content"></a>Contenido de notificaciones del sistema

Las notificaciones del sistema interactivas y adaptables permiten crear notificaciones flexibles con texto, imágenes y botones/entradas.

> **API importantes**: [paquete NuGet del kit de herramientas de la comunidad de UWP](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)

> [!NOTE]
> Para ver las plantillas heredadas de Windows 8.1 y Windows Phone 8.1, consulta el [catálogo de plantillas de notificación del sistema heredado](https://msdn.microsoft.com/library/windows/apps/hh761494).


## <a name="getting-started"></a>Introducción

**Instalar la biblioteca de notificaciones.** Si quieres usar C# en lugar de XML para generar notificaciones, instala el paquete de NuGet denominado [Microsoft.Toolkit.Uwp.Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) (buscar "notificaciones para UWP"). Los ejemplos de C# proporcionados en este artículo usan la versión 1.0.0 del paquete NuGet.

**Instala Notifications Visualizer** Al igual que la vista de editor o de diseño XAML de Visual Studio, esta aplicación para UWP gratuita te ayuda a diseñar notificaciones del sistema interactivas proporcionando una vista previa visual instantánea de la notificación a medida que la editas. Consulta [Notifications Visualizer](notifications-visualizer.md) para obtener más información o bien, [descargar Notifications Visualizer de la Store](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1).


## <a name="sending-a-toast-notification"></a>Envío de una notificación del sistema

Para obtener información sobre cómo enviar una notificación, consulta [Enviar notificación del sistema local](send-local-toast.md). Esta documentación solo abarca la creación del contenido de la notificación del sistema.


## <a name="toast-notification-structure"></a>Estructura de notificación del sistema

Las notificaciones del sistema son una combinación de algunas propiedades de datos como etiqueta o grupo (que te permiten identificar la notificación) y el *contenido del sistema*.

Los componentes principales del contenido de notificación del sistema son...
* **launch**: define qué argumentos se devolverán a la aplicación cuando el usuario haga clic en la notificación del sistema, lo que te permite vincular en profundidad en el contenido correcto que muestra la notificación del sistema. Para obtener más información, consulta [Enviar notificación del sistema local](send-local-toast.md).
* **visual**: la parte visual de la notificación del sistema, incluido el enlace genérico que contiene el texto y las imágenes.
* **actions**: la parte interactiva del sistema, incluyendo las entradas y las acciones.
* **audio**: controla el audio reproducido cuando se muestra la notificación del sistema al usuario.

El contenido de la notificación del sistema se define en XML sin formato, pero puedes usar nuestra [biblioteca de NuGet](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) para obtener un modelo de objetos de C# (o C++) para construir el contenido de notificación del sistema. En este artículo se documenta todo lo que se envía dentro del contenido de la notificación del sistema.

```csharp
ToastContent content = new ToastContent()
{
    Launch = "app-defined-string",
 
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric() { ... }
    },
 
    Actions = new ToastActionsCustom() { ... },
 
    Audio = new ToastAudio() { ... }
};
```

```xml
<toast launch="app-defined-string">

  <visual>
    <binding template="ToastGeneric">
      ...
    </binding>
  </visual>

  <actions>
    ...
  </actions>

  <audio src="ms-winsoundevent:Notification.Reminder"/>

</toast>
```

Esta es una representación visual del contenido de la notificación del sistema:

![estructura de notificación del sistema](images/adaptivetoasts-structure.jpg)


## <a name="visual"></a>Visual

Cada notificación del sistema debe especificar un elemento visual, donde debes proporcionar un enlace de notificación del sistema genérico, que puede contener texto, imágenes y mucho más. Estos elementos se representarán en varios dispositivos Windows, incluidos dispositivos de escritorio, teléfonos, tabletas y Xbox.

Para todos los atributos que se admiten en la sección visual y sus elementos secundarios, [consulta la documentación del esquema](toast-schema.md#toastvisual).

La identidad de tu aplicación en la notificación del sistema se transmite mediante el icono de la aplicación. Sin embargo, si usas la invalidación del logotipo de la aplicación, se mostrará el nombre de tu aplicación por debajo de las líneas de texto.

| Identidad de la aplicación para notificación del sistema normal | Identidad de la aplicación con appLogoOverride |
| -- | -- |
| <img src="images/adaptivetoasts-withoutapplogooverride.jpg" alt="notification without appLogoOverride" width="364"/> | <img alt="notification with appLogoOverride" src="images/adaptivetoasts-withapplogooverride.jpg" width="364"/> |


## <a name="text-elements"></a>Elementos de texto

Cada notificación del sistema debe tener al menos un elemento de texto y puede contener dos elementos de texto adicionales del tipo [**AdaptiveText**](toast-schema.md#adaptivetext).

<img alt="Toast with title and description" src="images/toast-title-and-description.jpg" width="364"/>

Desde la Actualización de aniversario de Windows 10, puedes controlar la cantidad de líneas de texto que se muestran mediante la propiedad **HintMaxLines** en el texto. El valor predeterminado (y máximo) es de hasta 2 líneas de texto para el título y hasta 4 líneas (combinadas) para los elementos de descripción adicionales (el segundo y tercer **AdaptiveText**).

```csharp
new ToastBindingGeneric()
{
    Children =
    {
        new AdaptiveText()
        {
            Text = "Adaptive Tiles Meeting",
            HintMaxLines = 1
        },

        new AdaptiveText()
        {
            Text = "Conf Room 2001 / Building 135"
        },

        new AdaptiveText()
        {
            Text = "10:00 AM - 10:30 AM"
        }
    }
}
```

```xml
<binding template="ToastGeneric">
    <text hint-maxLines="1">Adaptive Tiles Meeting</text>
    <text>Conf Room 2001 / Building 135</text>
    <text>10:00 AM - 10:30 AM</text>
</binding>
```


## <a name="app-logo-override"></a>Invalidación de logotipo de la aplicación

De manera predeterminada, la notificación del sistema mostrará el logotipo de la aplicación. Sin embargo, puedes invalidar este logotipo con tu propia imagen [**ToastGenericAppLogo**](toast-schema.md#toastgenericapplogo). Por ejemplo, si esta es una notificación de una persona, te recomendamos reemplazar el logotipo de la aplicación por una imagen de esa persona.

<img alt="Toast with app logo override" src="images/toast-applogooverride.jpg" width="364"/>

Puedes usar la propiedad **HintCrop** para cambiar el recorte de la imagen. Por ejemplo, **Circle** da como resultado una imagen recortada en forma de círculo. De lo contrario, la imagen es cuadrada. Las dimensiones de la imagen son 48x48 píxeles al 100% de escala.

```csharp
new ToastBindingGeneric()
{
    ...

    AppLogoOverride = new ToastGenericAppLogo()
    {
        Source = "https://picsum.photos/48?image=883",
        HintCrop = ToastGenericAppLogoCrop.Circle
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <image placement="appLogoOverride" hint-crop="circle" src="https://picsum.photos/48?image=883"/>
</binding>
```


## <a name="hero-image"></a>Imagen principal

**Novedad en la Actualización de aniversario**: las notificaciones del sistema pueden mostrar una imagen principal, que es una imagen destacada [**ToastGenericHeroImage**](toast-schema.md#toastgenericheroimage) que se muestra en un lugar destacado del banner de notificación del sistema y dentro del centro de actividades. Las dimensiones de la imagen son 360x180 píxeles al 100% de escala.

<img alt="Toast with hero image" src="images/toast-heroimage.jpg" width="364"/>

```csharp
new ToastBindingGeneric()
{
    ...

    HeroImage = new ToastGenericHeroImage()
    {
        Source = "https://picsum.photos/364/180?image=1043"
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <image placement="hero" src="https://picsum.photos/364/180?image=1043"/>
</binding>
```


## <a name="inline-image"></a>Imagen incorporada

Puedes proporcionar una imagen incorporada de ancho completo que aparece cuando expandes la notificación del sistema.

<img alt="Toast with additional image" src="images/toast-additionalimage.jpg" width="364"/>

```csharp
new ToastBindingGeneric()
{
    Children =
    {
        ...

        new AdaptiveImage()
        {
            Source = "https://picsum.photos/360/202?image=1043"
        }
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <image src="https://picsum.photos/360/202?image=1043" />
</binding>
```


## <a name="image-size-restrictions"></a>Restricciones de tamaño de imagen

Las imágenes que usas en la notificación del sistema pueden provenir de...

 - http://
 - ms-appx:///
 - ms-appdata:///

Para imágenes web remotas de http y https, hay límites en el tamaño del archivo de cada imagen individual. En la actualización de Fall Creators Update (16299), aumentamos el límite de 3MB, en conexiones normales y de 1MB en conexiones de uso medido. Antes, el límite de las imágenes siempre era de 200KB.

| Conexión normal | Conexión de uso medido | Antes de Fall Creators Update |
| - | - | - |
| 3 MB | 1 MB | 200KB |

Si una imagen supera el tamaño del archivo o no genera un error al descargar, o se agota el tiempo de espera, la imagen se eliminará y se mostrará el resto de la notificación.


## <a name="attribution-text"></a>Texto de atribución

**Novedad en la Actualización de aniversario**: si necesita hacer referencia al origen de su contenido, puedes usar el texto de atribución. Este texto siempre se muestra en la parte inferior de la notificación, junto con la identidad de tu aplicación o la marca de tiempo de la notificación.

En versiones anteriores de Windows que no admiten el texto de atribución, el texto simplemente se mostrará como otro elemento de texto (suponiendo que todavía no tienes el número máximo de tres elementos de texto).

<img alt="Toast with attribution text" src="images/toast-attributiontext.jpg" width="364"/>

```csharp
new ToastBindingGeneric()
{
    ...

    Attribution = new ToastGenericAttributionText()
    {
        Text = "Via SMS"
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <text placement="attribution">Via SMS</text>
</binding>
```


## <a name="custom-timestamp"></a>Marca de tiempo personalizada

**Novedad de la actualización Creators Update**: ahora puedes reemplazar la marca de tiempo proporcionada por el sistema por su propia marca de tiempo que representa de forma precisa cuándo se generó el mensaje, la información o el contenido. Esta marca de tiempo está visible en el centro de actividades.

<img alt="Toast with custom timestamp" src="images/toast-customtimestamp.jpg" width="396"/>

Para obtener más información sobre el uso de una marca de tiempo personalizada, consulta [marcas de tiempo personalizadas en notificaciones](custom-timestamps-on-toasts.md).

```csharp
ToastContent toastContent = new ToastContent()
{
    DisplayTimestamp = new DateTime(2017, 04, 15, 19, 45, 00, DateTimeKind.Utc),
    ...
};
```

```xml
<toast displayTimestamp="2017-04-15T19:45:00Z">
  ...
</toast>
```


## <a name="progress-bar"></a>Barra de progreso

**Novedad en Creators Update**: puedes proporcionar una barra de progreso en la notificación del sistema para mantener al usuario informado sobre el progreso de las operaciones, como descargas y mucho más.

<img alt="Toast with progress bar" src="images/toast-progressbar.png" width="364"/>

Para obtener más información sobre cómo usar una barra de progreso, consulta [barra de progreso de notificaciones del sistema](toast-progress-bar.md).


## <a name="headers"></a>Encabezados

**Novedad en Creators Update**: puedes agrupar notificaciones en los encabezados del Centro de actividades. Por ejemplo, puedes agrupar mensajes de un chat de grupo en un encabezado, o agrupar notificaciones de un tema común en un encabezado o más.

<img alt="Toasts with header" src="images/toast-headers-action-center.png" width="396"/>

Para obtener más información acerca del uso de encabezados, consulta [Encabezados del sistema](toast-headers.md).


## <a name="adaptive-content"></a>Contenido adaptable

**Novedad en la Actualización de aniversario**: además del contenido especificado anteriormente, también puedes mostrar más contenido adaptable que está visible cuando se expande la notificación del sistema.

Este contenido adicional se especifica con el diseño adaptable, sobre el que puedes obtener más información leyendo la [documentación de Iconos adaptables](create-adaptive-tiles.md).

Ten en cuenta que cualquier contenido adaptable debe estar dentro de un [**AdaptiveGroup**](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/toast-schema#adaptivegroup). De lo contrario, no se representará con diseño adaptable.


### <a name="columns-and-text-elements"></a>Columnas y elementos de texto

Este es un ejemplo en el que se usan columnas y algunos de elementos de texto adaptable avanzados. Puesto que los elementos de texto se encuentra dentro de un **AdaptiveGroup**, admiten todas las propiedades de estilo adaptable enriquecidas.

<img alt="Toast with additional text" src="images/toast-additionaltext.jpg" width="364"/>

```csharp
new ToastBindingGeneric()
{
    Children =
    {
        ...

        new AdaptiveGroup()
        {
            Children =
            {
                new AdaptiveSubgroup()
                {
                    Children =
                    {
                        new AdaptiveText()
                        {
                            Text = "52 attendees",
                            HintStyle = AdaptiveTextStyle.Base
                        },
                        new AdaptiveText()
                        {
                            Text = "23 minute drive",
                            HintStyle = AdaptiveTextStyle.CaptionSubtle
                        }
                    }
                },
                new AdaptiveSubgroup()
                {
                    Children =
                    {
                        new AdaptiveText()
                        {
                            Text = "1 Microsoft Way",
                            HintStyle = AdaptiveTextStyle.CaptionSubtle,
                            HintAlign = AdaptiveTextAlign.Right
                        },
                        new AdaptiveText()
                        {
                            Text = "Bellevue, WA 98008",
                            HintStyle = AdaptiveTextStyle.CaptionSubtle,
                            HintAlign = AdaptiveTextAlign.Right
                        }
                    }
                }
            }
        }
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <group>
        <subgroup>
            <text hint-style="base">52 attendees</text>
            <text hint-style="captionSubtle">23 minute drive</text>
        </subgroup>
        <subgroup>
            <text hint-style="captionSubtle" hint-align="right">1 Microsoft Way</text>
            <text hint-style="captionSubtle" hint-align="right">Bellevue, WA 98008</text>
        </subgroup>
    </group>
</binding>
```


## <a name="buttons"></a>Botones

Los botones convierte la notificación del sistema en interactiva, lo que permite al usuario realizar acciones rápidas en la notificación del sistema sin interrumpir su flujo de trabajo actual. Por ejemplo, los usuarios pueden responder a un mensaje directamente desde dentro de una notificación del sistema o eliminar un correo electrónico sin tan siquiera abrir la aplicación de correo electrónico. Los botones aparecen en la parte expandida de la notificación.

Para obtener más información acerca de la implementación de botones de un extremo a otro, consulta [Enviar notificaciones del sistema local](send-local-toast.md).

Los botones pueden realizar las siguientes acciones diferentes...

-   Activar la aplicación en primer plano con un argumento que se puede usar para navegar a un página o un contexto específico.
-   Activar la tarea en segundo plano de la aplicación, para un escenario de respuesta rápida o similar.
-   Activar otra aplicación a través del inicio de protocolo.
-   Realizar una acción del sistema, como posponer o descartar la notificación.

> [!NOTE]
> Solo puedes tener un máximo de 5 botones (incluidos los elementos de menú contextual que analizaremos más adelante).

<img alt="notification with actions, example 1" src="images/adaptivetoasts-xmlsample02.jpg" width="364"/>

```csharp
ToastContent content = new ToastContent()
{
    ...
 
    Actions = new ToastActionsCustom()
    {
        Buttons =
        {
            new ToastButton("See more details", "action=viewdetails&contentId=351")
            {
                ActivationType = ToastActivationType.Foreground
            },

            new ToastButton("Remind me later", "action=remindlater&contentId=351")
            {
                ActivationType = ToastActivationType.Background
            }
        }
    }
};
```

```xml
<toast launch="app-defined-string">

    ...

    <actions>

        <action
            content="See more details"
            arguments="action=viewdetails&amp;contentId=351"
            activationType="foreground"/>

        <action
            content="Remind me later"
            arguments="action=remindlater&amp;contentId=351"
            activationType="background"/>

    </actions>

</toast>
```


### <a name="buttons-with-icons"></a>Botones con iconos

Puedes agregar iconos a los botones. Estos iconos son imágenes de 16x16píxeles transparentes en blanco al 100% de escala y no deben incluir relleno en la propia imagen. Si decides proporcionar iconos en una notificación del sistema, debes proporcionar iconos para TODOS los botones de la notificación, ya que transforma el estilo de tus botones en botones de icono.

> [!NOTE]
> Para accesibilidad, asegúrate de incluir una versión en blanco de contraste del icono (icono negro para fondos de color blanco), de forma que cuando el usuario activa el modo de blanco en contraste alto, tu icono esté visible. Obtén más información en la [página de accesibilidad de notificaciones del sistema](tile-toast-language-scale-contrast.md).

<img src="images\adaptivetoasts-buttonswithicons.png" width="364" alt="Toast that has buttons with icons"/>

```csharp
new ToastButton("Dismiss", "dismiss")
{
    ActivationType = ToastActivationType.Background,
    ImageUri = "Assets/ToastButtonIcons/Dismiss.png"
}
```


```xml
<action
    content="Dismiss"
    imageUri="Assets/ToastButtonIcons/Dismiss.png"
    arguments="dismiss"
    activationType="background"/>
```


### <a name="buttons-with-pending-update-activation"></a>Botones con activación de actualización pendiente

**Novedad en la actualización de Fall Creators Update**: en botones de activación en segundo plano, puedes usar un comportamiento tras la activación de **PendingUpdate** para crear interacciones de varios pasos en las notificaciones del sistema. Cuando el usuario hace clic en el botón, se activa tu tarea en segundo plano y la notificación del sistema se coloca en un estado de "actualización pendiente", donde permanece en pantalla hasta que la tarea en segundo plano reemplaza la notificación del sistema por una nueva notificación del sistema.

Para obtener información sobre cómo implementar esto, consulta [Actualización pendiente de notificaciones del sistema](toast-pending-update.md).

![Notificación del sistema con actualización pendiente](images/toast-pendingupdate.gif)


### <a name="context-menu-actions"></a>Acciones de menú contextual

**Novedad de la Actualización de aniversario**: puedes agregar acciones de menú contextual adicionales al menú contextual existente que aparece cuando el usuario hace clic con el botón derecho en la notificación del sistema desde dentro del Centro de actividades. Ten en cuenta que este menú solo aparece cuando se hace clic con el botón derecho desde el Centro de actividades. No aparece al hacer clic con el botón derecho en un banner de elementos emergentes de notificaciones del sistema.

> [!NOTE]
> En dispositivos más antiguos, estas acciones de menú contextual adicionales aparecerán simplemente como botones normales en tu notificación del sistema.

Las acciones de menú contextual adicionales que agregas (como "Cambiar ubicación"). aparecen encima de las dos entradas del sistema predeterminadas.

<img alt="Toast with context menu" src="images/toast-contextmenu.png" width="444"/>

```csharp
ToastContent content = new ToastContent()
{
    ...
 
    Actions = new ToastActionsCustom()
    {
        ContextMenuItems =
        {
            new ToastContextMenuItem("Change location", "action=changeLocation")
        }
    }
};
```

```xml
<toast>

    ...

    <actions>

        <action
            placement="contextMenu"
            content="Change location"
            arguments="action=changeLocation"/>

    </actions>

</toast>
```

> [!NOTE]
> Los elementos de menú contextual adicionales contribuyen al límite total de 5 botones de una notificación del sistema.

La activación de los elementos de menú contextual adicionales se controla de manera idéntica a la de los botones de las notificaciones del sistema.


## <a name="inputs"></a>Entradas

Las entradas se especifican en la región Acciones de la región de notificación del sistema de la notificación del sistema, lo que significa que solo son visibles cuando se expande la notificación del sistema.


### <a name="quick-reply-text-box"></a>Cuadro de texto de respuesta rápida

Para habilitar un cuadro de texto de respuesta rápida, como un escenario de mensajería, agrega una entrada de texto y un botón, y haz referencia al identificador de la entrada de texto para que el botón se muestre junto a la entrada.

<img alt="notification with text input and actions" src="images/adaptivetoasts-xmlsample05.jpg" width="364"/>

```csharp
ToastContent content = new ToastContent()
{
    ...
 
    Actions = new ToastActionsCustom()
    {
        Inputs =
        {
            new ToastTextBox("tbReply")
            {
                PlaceholderContent = "Type a reply"
            }
        },

        Buttons =
        {
            new ToastButton("Reply", "action=reply&convId=9318")
            {
                ActivationType = ToastActivationType.Background,

                // To place the button next to the text box,
                // reference the text box's Id and provide an image
                TextBoxId = "tbReply",
                ImageUri = "Assets/Reply.png"
            }
        }
    }
};
```

```xml
<toast launch="app-defined-string">

    ...

    <actions>

        <input id="textBox" type="text" placeHolderContent="Type a reply"/>

        <action
            content="Send"
            arguments="action=reply&amp;convId=9318"
            activationType="background"
            hint-inputId="textBox"
            imageUri="Assets/Reply.png"/>

    </actions>

</toast>
```


### <a name="inputs-with-buttons-bar"></a>Entradas con barra de botones

También puedes tener una (o muchas) entradas con botones normales mostradas debajo de las entradas.

<img alt="notification with text and input actions" src="images/adaptivetoasts-xmlsample04.jpg" width="364"/>

```csharp
ToastContent content = new ToastContent()
{
    ...
 
    Actions = new ToastActionsCustom()
    {
        Inputs =
        {
            new ToastTextBox("tbReply")
            {
                PlaceholderContent = "Type a reply"
            }
        },

        Buttons =
        {
            new ToastButton("Reply", "action=reply&threadId=9218")
            {
                ActivationType = ToastActivationType.Background
            },

            new ToastButton("Video call", "action=videocall&threadId=9218")
            {
                ActivationType = ToastActivationType.Foreground
            }
        }
    }
};
```

```xml
<toast launch="app-defined-string">

    ...

    <actions>

        <input id="textBox" type="text" placeHolderContent="Type a reply"/>

        <action
            content="Reply"
            arguments="action=reply&amp;threadId=9218"
            activationType="background"/>

        <action
            content="Video call"
            arguments="action=videocall&amp;threadId=9218"
            activationType="foreground"/>

    </actions>

</toast>
```


### <a name="selection-input"></a>Entrada de selección

Además de los cuadros de texto, también puedes usar un menú de selección.

<img alt="notification with selection input and actions" src="images/adaptivetoasts-xmlsample06.jpg" width="364"/>

```csharp
ToastContent content = new ToastContent()
{
    ...
 
    Actions = new ToastActionsCustom()
    {
        Inputs =
        {
            new ToastSelectionBox("time")
            {
                DefaultSelectionBoxItemId = "lunch",
                Items =
                {
                    new ToastSelectionBoxItem("breakfast", "Breakfast"),
                    new ToastSelectionBoxItem("lunch", "Lunch"),
                    new ToastSelectionBoxItem("dinner", "Dinner")
                }
            }
        },

        Buttons = { ... }
};
```

```xml
<toast launch="app-defined-string">

    ...

    <actions>

        <input id="time" type="selection" defaultInput="lunch">
            <selection id="breakfast" content="Breakfast" />
            <selection id="lunch" content="Lunch" />
            <selection id="dinner" content="Dinner" />
        </input>

        ...

    </actions>

</toast>
```


### <a name="snoozedismiss"></a>Posponer/descartar

Con un menú de selección y dos botones, podemos crear una notificación de recordatorio que utiliza las acciones posponer y descartar del sistema. Asegúrate de que establecer el escenario en Recordatorio para que la notificación se comporte como un recordatorio.

<img alt="reminder notification" src="images/adaptivetoasts-xmlsample07.jpg" width="364"/>

Vinculamos el botón Posponer a la entrada del menú de selección con la propiedad **SelectionBoxId** en el botón de la notificación del sistema.

```csharp
ToastContent content = new ToastContent()
{
    Scenario = ToastScenario.Reminder,

    ...
 
    Actions = new ToastActionsCustom()
    {
        Inputs =
        {
            new ToastSelectionBox("snoozeTime")
            {
                DefaultSelectionBoxItemId = "15",
                Items =
                {
                    new ToastSelectionBoxItem("5", "5 minutes"),
                    new ToastSelectionBoxItem("15", "15 minutes"),
                    new ToastSelectionBoxItem("60", "1 hour"),
                    new ToastSelectionBoxItem("240", "4 hours"),
                    new ToastSelectionBoxItem("1440", "1 day")
                }
            }
        },

        Buttons =
        {
            new ToastButtonSnooze()
            {
                SelectionBoxId = "snoozeTime"
            },
 
            new ToastButtonDismiss()
        }
    }
};
```

```xml
<toast scenario="reminder" launch="action=viewEvent&amp;eventId=1983">
   
  ...
 
  <actions>
     
    <input id="snoozeTime" type="selection" defaultInput="15">
      <selection id="1" content="1 minute"/>
      <selection id="15" content="15 minutes"/>
      <selection id="60" content="1 hour"/>
      <selection id="240" content="4 hours"/>
      <selection id="1440" content="1 day"/>
    </input>
 
    <action activationType="system" arguments="snooze" hint-inputId="snoozeTime" content="" />
 
    <action activationType="system" arguments="dismiss" content=""/>
     
  </actions>
   
</toast>
```

Para usar las acciones de posponer y descartar del sistema:

-   Especificar un **ToastButtonSnooze** o **ToastButtonDismiss**
-   Tienes la opción de especificar una cadena de contenido personalizada:
    -   Si no proporcionas una cadena, automáticamente usaremos las cadenas localizadas de "Posponer" y "Descartar".
-   Tienes la opción de especificar el **SelectionBoxId**:
    -   Si no quieres que el usuario seleccione un intervalo de aplazamiento y, en su lugar, solo quieres que la notificación se posponga solo una vez en un intervalo de tiempo definido por el sistema (que es coherente en todo el sistema operativo), no crees ningún &lt;input&gt; en absoluto.
    -   Si quieres proporcionar las selecciones de intervalo de aplazamiento:
        -   Especificar **SelectionBoxId** en la acción de posponer
        -   Haz coincidir el identificador de la entrada con el **SelectionBoxId** de la acción de posponer
        -   Especifica el valor del **ToastSelectionBoxItem** que sea un nonNegativeInteger que represente el intervalo de aplazamiento en minutos.



## <a name="audio"></a>Audio

Mobile siempre ha admitido el audio personalizado, y también se admite en la versión de escritorio 1511 (compilación 10586) o versiones posteriores. Se puede hacer referencia al audio personalizados a través de las siguientes rutas de acceso:

-   ms-appx:///
-   ms-appdata:///

Como alternativa, puedes elegir de la [lista de ms-winsoundevents](/uwp/schemas/tiles/toastschema/element-audio#attributes-and-elements) (en inglés), que siempre se ha admitido en ambas plataformas.

```csharp
ToastContent content = new ToastContent()
{
    ...

    Audio = new ToastAudio()
    {
        Src = new Uri("ms-appx:///Assets/NewMessage.mp3")
    }
}
```

```xml
<toast launch="app-defined-string">

    ...

    <audio src="ms-appx:///Assets/NewMessage.mp3"/>

</toast>
```

Consulta la [página de esquemas de audio](/uwp/schemas/tiles/toastschema/element-audio) (en inglés) para obtener información sobre el audio en las notificaciones del sistema. Para obtener información sobre cómo enviar una notificación del sistema con audio personalizado, consulta [audio personalizado en notificaciones del sistema](custom-audio-on-toasts.md).


## <a name="alarms-reminders-and-incoming-calls"></a>Alarmas, recordatorios y llamadas entrantes

Para crear alarmas, avisos y notificaciones de llamadas entrantes, simplemente usas una notificación del sistema normal con un valor de escenario asignado. El escenario ajusta algunos comportamientos para crear una experiencia de usuario unificada y coherente.

> [!IMPORTANT]
> Cuando uses Recordatorio o alarma, debes proporcionar al menos un botón en la notificación del sistema. De lo contrario, la notificación del sistema se tratará como una notificación del sistema normal.

* **Aviso**: la notificación permanecerá en pantalla hasta que el usuario la descarte o realice una acción. En Windows Mobile, la notificación del sistema también aparecerá previamente expandida. Se reproducirá un sonido de aviso.
* **Alarma**: además de los comportamientos de aviso, las alarmas repetirán además con un sonido de alarma predeterminado.
* **IncomingCall**: las notificaciones de llamadas entrantes se muestran en pantalla completa en los dispositivos Windows Mobile. De lo contrario, tienen los mismos comportamientos que las alarmas excepto en que usan audio de tono y sus botones tienen un estilo diferente.

```csharp
ToastContent content = new ToastContent()
{
    Scenario = ToastScenario.Reminder,

    ...
}
```

```xml
<toast scenario="reminder" launch="app-defined-string">

    ...

</toast>
```


## <a name="localization-and-accessibility"></a>Localización y accesibilidad

Las ventanas y notificaciones del sistema pueden cargar cadenas y archivos adaptados al idioma, el factor de escala de visualización, el contraste alto y otros contextos de tiempo de ejecución. Para obtener más información, consulta [Compatibilidad de ventanas y notificaciones del sistema para el idioma, la escala y el contraste alto](tile-toast-language-scale-contrast.md).


## <a name="handling-activation"></a>Control de la activación
Para obtener información sobre cómo controlar activaciones de notificación del sistema (haciendo clic el usuario en la notificación del sistema o los botones de la notificación del sistema), consulta [Enviar notificación del sistema local](send-local-toast.md).
 
## <a name="related-topics"></a>Temas relacionados

* [Enviar una notificación del sistema local y administrar la aplicación](send-local-toast.md)
* [Biblioteca de notificaciones en GitHub (parte del Kit de herramientas de la comunidad de UWP)](https://github.com/Microsoft/UWPCommunityToolkit/tree/master/Microsoft.Toolkit.Uwp.Notifications)
* [Compatibilidad de ventanas y notificaciones del sistema para el idioma, la escala y el contraste alto.](tile-toast-language-scale-contrast.md)
