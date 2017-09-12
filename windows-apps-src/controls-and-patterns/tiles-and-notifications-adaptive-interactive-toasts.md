---
author: mijacobs
Description: "Las notificaciones del sistema adaptables e interactivas permiten crear notificaciones emergentes flexibles con más contenido, imágenes en línea opcionales e interacción del usuario opcional."
title: Notificaciones del sistema interactivas y adaptables
ms.assetid: 1FCE66AF-34B4-436A-9FC9-D0CF4BDA5A01
label: Adaptive and interactive toast notifications
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: c8e77773b9118c3177dc958ddc7b51d32a452fa5
ms.sourcegitcommit: 9d1ca16a7edcbbcae03fad50a4a10183a319c63a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/09/2017
---
# <a name="adaptive-and-interactive-toast-notifications"></a>Notificaciones del sistema interactivas y adaptables

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Las notificaciones del sistema interactivas y adaptables permiten crear notificaciones flexibles con texto, imágenes y botones/entradas.

> **API importantes**: [paquete NuGet del kit de herramientas de la comunidad de UWP](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)

> [!NOTE]
> Para ver las plantillas heredadas de Windows 8.1 y Windows Phone 8.1, consulta el [catálogo de plantillas de notificación del sistema heredado](https://msdn.microsoft.com/library/windows/apps/hh761494).


## <a name="getting-started"></a>Introducción

**Instalar la biblioteca de notificaciones.** Si quieres usar C# en lugar de XML para generar notificaciones, instala el paquete de NuGet denominado [Microsoft.Toolkit.Uwp.Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) (buscar "notificaciones para UWP"). Los ejemplos de C# proporcionados en este artículo usan la versión 1.0.0 del paquete NuGet.

**Instala Notifications Visualizer** Al igual que la vista de editor o de diseño XAML de Visual Studio, esta aplicación para UWP gratuita te ayuda a diseñar notificaciones del sistema interactivas proporcionando una vista previa visual instantánea de la notificación a medida que la editas. Puedes leer [esta entrada de blog](http://blogs.msdn.com/b/tiles_and_toasts/archive/2015/09/22/introducing-notifications-visualizer-for-windows-10.aspx) para obtener más información y puedes descargar Notifications Visualizer [aquí](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1).


## <a name="sending-a-toast-notification"></a>Envío de una notificación del sistema

Para obtener información sobre cómo enviar una notificación, consulta [Enviar notificación del sistema local](tiles-and-notifications-send-local-toast.md). Esta documentación solo abarca la creación del contenido de la notificación del sistema.


## <a name="toast-notification-structure"></a>Estructura de notificación del sistema

Las notificaciones del sistema son una combinación de algunas propiedades de datos como etiqueta o grupo (que te permiten identificar la notificación) y el *contenido del sistema*.

Los componentes principales del contenido de notificación del sistema son...
* **launch**: define qué argumentos se devolverán a la aplicación cuando el usuario haga clic en la notificación del sistema, lo que te permite vincular en profundidad en el contenido correcto que muestra la notificación del sistema. Para obtener más información, consulta [Enviar notificación del sistema local](tiles-and-notifications-send-local-toast.md).
* **visual**: la parte visual de la notificación del sistema, incluido el enlace genérico que contiene el texto, las imágenes y los logotipos de las aplicaciones.
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

Cada notificación del sistema debe especificar un elemento visual, donde debes proporcionar un enlace de notificación del sistema genérico, que puede contener texto, imágenes, logotipos y mucho más. Estos elementos se representarán en varios dispositivos de Windows, incluidos dispositivos de escritorio, teléfonos, tabletas y Xbox.

Para todos los atributos que se admiten en la sección visual y sus elementos secundarios, [consulta la documentación del esquema](tiles-and-notifications-toast-schema.md#toastvisual).

La identidad de tu aplicación en la notificación del sistema se transmite mediante el icono de la aplicación. Sin embargo, si usas la invalidación del logotipo de la aplicación, se mostrará el nombre de tu aplicación por debajo de las líneas de texto.

| Notificación del sistema normal                                                                              | Notificaciones del sistema con appLogoOverride                                                          |
| ----------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| ![Notificación sin appLogoOverride](images/adaptivetoasts-withoutapplogooverride.jpg) | ![Notificación con appLogoOverride](images/adaptivetoasts-withapplogooverride.jpg) |


## <a name="text-elements"></a>Elementos de texto

Cada notificación del sistema debe tener al menos un elemento de texto y puede contener dos elementos de texto adicionales del tipo [AdaptiveText](tiles-and-notifications-toast-schema.md#adaptivetext).

![Notificaciones del sistema con título y descripción](images/toast-title-and-description.jpg)

Desde la actualización de aniversario, puedes controlar la cantidad de líneas de texto que se muestran mediante la propiedad **HintMaxLines** en el texto. De manera predeterminada, el título muestra hasta 2 líneas de texto y cada una de las líneas de la descripción muestran hasta 4 líneas de texto.

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

De manera predeterminada, la notificación del sistema mostrará el logotipo de la aplicación. Sin embargo, puedes invalidar este logotipo con tu propia imagen [ToastGenericAppLogo](tiles-and-notifications-toast-schema.md#toastgenericapplogo). Por ejemplo, si esta es una notificación de una persona, te recomendamos reemplazar el logotipo de la aplicación por una imagen de esa persona.

![Notificación del sistema con invalidación del logotipo de la aplicación](images/toast-applogooverride.jpg)

Puedes usar la propiedad **HintCrop** para cambiar el recorte de la imagen. Por ejemplo, *circle* da como resultado una imagen recortada en forma de círculo. De lo contrario, la imagen es cuadrada. Las dimensiones de la imagen son 64 x 64 píxeles al 100% de escala.

```csharp
new ToastBindingGeneric()
{
    ...

    AppLogoOverride = new ToastGenericAppLogo()
    {
        Source = "https://unsplash.it/64?image=883",
        HintCrop = ToastGenericAppLogoCrop.Circle
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <image placement="appLogoOverride" hint-crop="circle" src="https://unsplash.it/64?image=883"/>
</binding>
```


## <a name="hero-image"></a>Imagen principal

**Novedad en la Actualización de aniversario**: las notificaciones del sistema pueden mostrar una imagen principal, que es una imagen destacada [ToastGenericHeroImage](tiles-and-notifications-toast-schema.md#toastgenericheroimage) que se muestra en un lugar destacado del banner de notificación del sistema y dentro del centro de actividades. Las dimensiones de la imagen son 360 x 180 píxeles al 100% de escala.

![Notificación del sistema con imagen principal](images/toast-heroimage.jpg)

```csharp
new ToastBindingGeneric()
{
    ...

    HeroImage = new ToastHeroImage()
    {
        Source = "https://unsplash.it/360/180?image=1043"
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <image placement="hero" src="https://unsplash.it/360/180?image=1043"/>
</binding>
```


## <a name="inline-image"></a>Imagen incorporada

Puedes proporcionar una imagen incorporada de ancho completo que aparece cuando expandes la notificación del sistema.

![Notificación del sistema con imagen adicional](images/toast-additionalimage.jpg)

```csharp
new ToastBindingGeneric()
{
    Children =
    {
        ...

        new AdaptiveImage()
        {
            Source = "https://unsplash.it/360/180?image=1043"
        }
    }
}
```

```xml
<binding template="ToastGeneric">
    ...
    <image src="https://unsplash.it/360/180?image=1043" />
</binding>
```


## <a name="attribution-text"></a>Texto de atribución

**Novedad en la Actualización de aniversario**: si necesita hacer referencia al origen de su contenido, puedes usar el texto de atribución. Este texto siempre se muestra en la parte inferior de la notificación, junto con la identidad de tu aplicación o la marca de tiempo de la notificación.

En versiones anteriores de Windows que no admiten el texto de atribución, el texto simplemente se mostrará como otro elemento de texto (suponiendo que todavía no tienes el número máximo de tres elementos de texto).

![Notificación del sistema con texto de atribución](images/toast-attributiontext.jpg)

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

![Notificación del sistema con la marca de tiempo personalizada](images/toast-customtimestamp.jpg)

Para obtener más información sobre el uso de una marca de tiempo personalizada, [consulta este blog](https://blogs.msdn.microsoft.com/tiles_and_toasts/2017/01/09/custom-timestamp-on-toast-notifications-windows-10-creators-update/).

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


## <a name="adaptive-content"></a>Contenido adaptable

**Novedad en la Actualización de aniversario**: además del contenido especificado anteriormente, también puedes mostrar más contenido adaptable que está visible cuando se expande la notificación del sistema.

Este contenido adicional se especifica con el diseño adaptable, sobre el que puedes obtener más información leyendo la [documentación de Iconos adaptables](tiles-and-notifications-create-adaptive-tiles.md).

Ten en cuenta que cualquier contenido adaptable debe estar dentro de un AdaptiveGroup. De lo contrario, no se representará con diseño adaptable.


### <a name="columns-and-text-elements"></a>Columnas y elementos de texto

Este es un ejemplo en el que se usan columnas y algunos de elementos de texto adaptable avanzados. Puesto que los elementos de texto se encuentra dentro de un AdaptiveGroup, admiten todas las propiedades de estilo adaptable enriquecidas.

![Notificación del sistema con texto adicional](images/toast-additionaltext.jpg)

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


## <a name="inputs-and-buttons"></a>Entradas y botones

Las entradas y los botones se especifican en la región Acciones de la región de notificación del sistema de la notificación del sistema, lo que significa que solo son visibles cuando se expande la notificación del sistema.


### <a name="quick-reply-text-box"></a>Cuadro de texto de respuesta rápida

Para habilitar un cuadro de texto de respuesta rápida, como un escenario de mensajería, agrega una entrada de texto y un botón, y haz referencia al identificador de la entrada de texto para que el botón se muestre junto a la entrada.

![notificación con entrada de texto y acciones](images/adaptivetoasts-xmlsample05.jpg)

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

        <input id="textBox" type="text" placeholderContent="Type a reply"/>

        <action
            content="Send",
            arguments="action=reply&amp;convId=9318"
            activationType="background"
            hint-inputId="textBox"
            imageUri="Assets/Reply.png"/>

    </actions>

</toast>
```


### <a name="inputs-with-buttons-bar"></a>Entradas con barra de botones

También puedes tener una (o muchas) entradas con botones normales mostradas debajo de las entradas.

![notificación con entrada de texto y acciones](images/adaptivetoasts-xmlsample04.jpg)

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

        <input id="textBox" type="text" placeholderContent="Type a reply"/>

        <action
            content="Reply",
            arguments="action=reply&amp;threadId=9218"
            activationType="background"/>

        <action
            content="Video call",
            arguments="action=videocall&amp;threadId=9218"
            activationType="foreground"/>

    </actions>

</toast>
```


### <a name="selection-input"></a>Entrada de selección

Además de los cuadros de texto, también puedes usar un menú de selección.

![notificación con entrada de selección y acciones](images/adaptivetoasts-xmlsample06.jpg)

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


## <a name="buttons"></a>Botones

Los botones convierte la notificación del sistema en interactiva, lo que permite al usuario realizar acciones rápidas en la notificación del sistema sin interrumpir su flujo de trabajo actual. Por ejemplo, los usuarios pueden responder a un mensaje directamente desde dentro de una notificación del sistema o eliminar un correo electrónico sin tan siquiera abrir la aplicación de correo electrónico.

Los botones pueden realizar las siguientes acciones diferentes...

-   Activar la aplicación en primer plano con un argumento que se puede usar para navegar a un página o un contexto específico.
-   Activar la tarea en segundo plano de la aplicación, para un escenario de respuesta rápida o similar.
-   Activar otra aplicación a través del inicio de protocolo.
-   Realizar una acción del sistema, como posponer o descartar la notificación.

Ten en cuenta que solo puedes tener un máximo de 5 botones (incluidos los elementos de menú contextual que analizaremos más adelante).

![notificación con acciones, ejemplo 1](images/adaptivetoasts-xmlsample02.jpg)

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
            content="See more details",
            arguments="action=viewdetails&amp;contentId=351"
            activationType="foreground"/>

        <action
            content="Remind me later",
            arguments="action=remindlater&amp;contentId=351"
            activationType="background"/>

    </actions>

</toast>
```


### <a name="snoozedismiss-buttons"></a>Botones de posponer/descartar

Con un menú de selección y dos botones, podemos crear una notificación de recordatorio que utiliza las acciones posponer y descartar del sistema. Asegúrate de que establecer el escenario en Recordatorio para que la notificación se comporte como un recordatorio.

![notificación de recordatorio](images/adaptivetoasts-xmlsample07.jpg)

Vinculamos el botón Posponer a la entrada del menú de selección con la propiedad *SepectionBoxId* en el botón de la notificación del sistema.

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

Para usar las acciones de posponer y descartar del sistema, haz lo siguiente:

-   Especificar un ToastButtonSnooze o ToastButtonDismiss
-   Tienes la opción de especificar una cadena de contenido personalizada:
    -   Si no proporcionas una cadena, automáticamente usaremos las cadenas localizadas de "Posponer" y "Descartar".
-   Tienes la opción de especificar el *SelectionBoxId*:
    -   Si no quieres que el usuario seleccione un intervalo de aplazamiento y, en su lugar, solo quieres que la notificación se posponga solo una vez en un intervalo de tiempo definido por el sistema (que es coherente en todo el sistema operativo), no crees ningún &lt;input&gt; en absoluto.
    -   Si quieres proporcionar las selecciones de intervalo de aplazamiento:
        -   Especificar *SelectionBoxId* en la acción de posponer
        -   Haz coincidir el identificador de la entrada con el *SelectionBoxId* de la acción de posponer
        -   Especifica el valor del *ToastSelectionBoxItem* que sea un nonNegativeInteger que represente el intervalo de aplazamiento en minutos.



## <a name="audio"></a>Audio

Mobile siempre ha admitido el audio personalizado, y también se admite en la versión de escritorio 1511 (compilación 10586) o versiones posteriores. Se puede hacer referencia al audio personalizados a través de las siguientes rutas de acceso:

-   ms-appx:///
-   ms-appdata:///

Como alternativa, puedes elegir de la [lista de ms-winsoundevents](https://msdn.microsoft.com/library/windows/apps/br230842) (en inglés), que siempre se ha admitido en ambas plataformas.

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

Consulta la [página de esquemas de audio](https://msdn.microsoft.com/library/windows/apps/br230842) (en inglés) para obtener información sobre el audio en las notificaciones del sistema. Para obtener información sobre cómo enviar una notificación del sistema con audio personalizado, [consulta este blog](https://blogs.msdn.microsoft.com/tiles_and_toasts/2016/06/18/quickstart-sending-a-toast-notification-with-custom-audio/) (en inglés).


## <a name="alarms-reminders-and-incoming-calls"></a>Alarmas, recordatorios y llamadas entrantes

Para crear alarmas, avisos y notificaciones de llamadas entrantes, simplemente usas una notificación del sistema normal con un valor de escenario asignado. El escenario ajusta algunos comportamientos para crear una experiencia de usuario unificada y coherente.

* **Aviso**: la notificación permanecerá en pantalla hasta que el usuario la descarte o realice una acción. En Windows Mobile, la notificación del sistema también aparecerá previamente expandida. Se reproducirá un sonido de aviso.
* **Alarma**: además de los comportamientos de aviso, las alarmas repetirán además con un sonido de alarma predeterminado.
* **IncomingCall**: las notificaciones de llamadas entrantes se muestran en pantalla completa en los dispositivos Windows Mobile. De lo contrario, tienen los mismos comportamientos que las alarmas excepto en que usan audio de tono.

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


## <a name="handling-activation"></a>Control de la activación
Para obtener información sobre cómo controlar activaciones de notificación del sistema (haciendo clic el usuario en la notificación del sistema o los botones de la notificación del sistema), consulta [Enviar notificación del sistema local](tiles-and-notifications-send-local-toast.md).


 
## <a name="related-topics"></a>Temas relacionados

* [Enviar una notificación del sistema local y administrar la aplicación](tiles-and-notifications-send-local-toast.md)
* [Biblioteca de notificaciones en GitHub](https://github.com/Microsoft/UWPCommunityToolkit/tree/dev/Notifications)