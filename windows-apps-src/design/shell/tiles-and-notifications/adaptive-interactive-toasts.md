---
description: Las notificaciones del sistema adaptables e interactivas permiten crear notificaciones emergentes flexibles con más contenido, imágenes en línea opcionales e interacción del usuario opcional.
title: Contenido de notificaciones del sistema
ms.assetid: 1FCE66AF-34B4-436A-9FC9-D0CF4BDA5A01
label: Toast content
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP, notificaciones del sistema, notificaciones interactivas, notificaciones de sistema adaptables, contenido del sistema, carga del sistema
ms.localizationpriority: medium
ms.openlocfilehash: f148938f8c8e3bb5ac305a82d1863545005fd802
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034298"
---
# <a name="toast-content"></a>Contenido de notificaciones del sistema

Las notificaciones del sistema interactivas y adaptables permiten crear notificaciones flexibles con texto, imágenes y botones y entradas.

> **API importantes** : [paquete NuGet de notificaciones del kit de herramientas de la comunidad de UWP](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)

> [!NOTE]
> Para ver las plantillas heredadas de Windows 8.1 y Windows Phone 8,1, consulte el [Catálogo de plantillas de notificaciones de sistema heredado](/previous-versions/windows/apps/hh761494(v=win.10)).


## <a name="getting-started"></a>Introducción

**Instalar la biblioteca de notificaciones.** Si quieres usar C# en lugar de XML para generar notificaciones, instala el paquete de NuGet denominado [Microsoft.Toolkit.Uwp.Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) (buscar "notificaciones para UWP"). Los ejemplos de C# proporcionados en este artículo usan la versión 1.0.0 del paquete NuGet.

**Instala Notifications Visualizer** Esta aplicación Windows gratuita le ayuda a diseñar notificaciones del sistema interactivas al proporcionar una vista previa visual instantánea de la notificación del sistema a medida que la edita, similar a la vista de diseño o editor XAML de Visual Studio. Vea el [visualizador de notificaciones](notifications-visualizer.md) para obtener más información o [descargar el visualizador de notificaciones de la tienda](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1).


## <a name="sending-a-toast-notification"></a>Envío de una notificación del sistema

Para obtener información sobre cómo enviar una notificación, consulte envío de notificaciones de [sistema local](send-local-toast.md). Esta documentación solo trata la creación del contenido del sistema.


## <a name="toast-notification-structure"></a>Estructura de notificación del sistema

Las notificaciones del sistema son una combinación de algunas propiedades de datos como etiqueta/grupo (que permiten identificar la notificación) y el *contenido del sistema* .

Los componentes principales del contenido del sistema...
* **Launch** : define qué argumentos se devolverán a la aplicación cuando el usuario hace clic en la notificación del sistema, lo que le permite vincular en profundidad el contenido correcto que se muestra en la notificación del sistema. Para obtener más información, consulte [envío de notificaciones de sistema local](send-local-toast.md).
* **Visual** : la parte visual de la notificación del sistema, incluido el enlace genérico que contiene texto e imágenes.
* **acciones** : la parte interactiva de la notificación del sistema, incluidas las entradas y las acciones.
* **audio** : controla el audio que se reproduce cuando se muestra al usuario.

El contenido del sistema se define en XML sin formato, pero puede usar nuestra [biblioteca de NuGet](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) para obtener un modelo de objetos de C# (o C++) para crear el contenido del sistema. En este artículo se documenta todo lo que va dentro del contenido del sistema.

#### <a name="builder-syntax"></a>[Sintaxis del compilador](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    .AddToastActivationInfo("app-defined-string", ToastActivationType.Foreground)
    .AddText("Some text")
    .AddButton("Archive", ToastActivationType.Background, "archive")
    .AddAudio(new Uri("ms-appx:///Sound.mp3"));
```

#### <a name="xml"></a>[XML](#tab/xml)

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

---

Esta es una representación visual del contenido de la notificación del sistema:

![estructura de notificación del sistema](images/adaptivetoasts-structure.jpg)


## <a name="visual"></a>Visual

Cada notificación del sistema debe especificar un visual, donde debe proporcionar un enlace de notificación genérico, que puede contener texto, imágenes, etc. Estos elementos se representarán en varios dispositivos Windows, como escritorio, teléfonos, tabletas y Xbox.

Para ver todos los atributos admitidos en la sección visual y sus elementos secundarios, [vea la documentación del esquema](toast-schema.md#toastvisual).

La identidad de la aplicación en la notificación del sistema se transmite a través del icono de la aplicación. Sin embargo, si usa la invalidación del logotipo de la aplicación, se mostrará el nombre de la aplicación debajo de las líneas de texto.

| Identidad de la aplicación para la notificación normal | Identidad de la aplicación con appLogoOverride |
| -- | -- |
| <img src="images/adaptivetoasts-withoutapplogooverride.jpg" alt="notification without appLogoOverride" width="364"/> | <img alt="notification with appLogoOverride" src="images/adaptivetoasts-withapplogooverride.jpg" width="364"/> |


## <a name="text-elements"></a>Elementos de texto

Cada notificación del sistema debe tener al menos un elemento de texto y puede contener dos elementos de texto adicionales, todo de tipo [**AdaptiveText**](toast-schema.md#adaptivetext).

<img alt="Toast with title and description" src="images/toast-title-and-description.jpg" width="364"/>

Desde la actualización de aniversario de Windows 10, puede controlar el número de líneas de texto que se muestran mediante la propiedad **HintMaxLines** del texto. Los valores predeterminados (y máximo) son hasta 2 líneas de texto para el título y hasta 4 líneas (combinadas) para los dos elementos de Descripción adicionales (el segundo y el tercer **AdaptiveText** ).

#### <a name="builder-syntax"></a>[Sintaxis del compilador](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    .AddText("Adaptive Tiles Meeting", hintMaxLines: 1)
    .AddText("Conf Room 2001 / Building 135")
    .AddText("10:00 AM - 10:30 AM");
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<binding template="ToastGeneric">
    <text hint-maxLines="1">Adaptive Tiles Meeting</text>
    <text>Conf Room 2001 / Building 135</text>
    <text>10:00 AM - 10:30 AM</text>
</binding>
```

---


## <a name="app-logo-override"></a>Invalidación del logotipo de la aplicación

De forma predeterminada, la notificación del sistema mostrará el logotipo de la aplicación. Sin embargo, puede invalidar este logotipo con su propia imagen de [**ToastGenericAppLogo**](toast-schema.md#toastgenericapplogo) . Por ejemplo, si se trata de una notificación de una persona, se recomienda invalidar el logotipo de la aplicación con una imagen de esa persona.

<img alt="Toast with app logo override" src="images/toast-applogooverride.jpg" width="364"/>

Puede usar la propiedad **HintCrop** para cambiar el recorte de la imagen. Por ejemplo, el **círculo** da como resultado una imagen recortada con un círculo. De lo contrario, la imagen es cuadrada. Las dimensiones de la imagen son de 48x48 píxeles con una escala del 100%.

#### <a name="builder-syntax"></a>[Sintaxis del compilador](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    ...
    
    .AddAppLogoOverride(new Uri("https://picsum.photos/48?image=883"), NotificationAppLogoCrop.Circle);
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<binding template="ToastGeneric">
    ...
    <image placement="appLogoOverride" hint-crop="circle" src="https://picsum.photos/48?image=883"/>
</binding>
```

---



## <a name="hero-image"></a>Imagen de Hero

**Novedad en la actualización de aniversario** : las notificaciones del sistema pueden mostrar una imagen prominente, que es un [**ToastGenericHeroImage**](toast-schema.md#toastgenericheroimage) destacado que se muestra de forma destacada en la pancarta del sistema y en el centro de actividades. Las dimensiones de la imagen son de 364x180 píxeles con una escala del 100%.

<img alt="Toast with hero image" src="images/toast-heroimage.jpg" width="364"/>

#### <a name="builder-syntax"></a>[Sintaxis del compilador](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    ...
    
    .AddHeroImage(new Uri("https://picsum.photos/364/180?image=1043"));
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<binding template="ToastGeneric">
    ...
    <image placement="hero" src="https://picsum.photos/364/180?image=1043"/>
</binding>
```

---


## <a name="inline-image"></a>Imagen alineada

Puede proporcionar una imagen en línea de ancho completo que aparece al expandir la notificación del sistema.

<img alt="Toast with additional image" src="images/toast-additionalimage.jpg" width="364"/>

#### <a name="builder-syntax"></a>[Sintaxis del compilador](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    ...
    
    .AddInlineImage(new Uri("https://picsum.photos/360/202?image=1043"));
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<binding template="ToastGeneric">
    ...
    <image src="https://picsum.photos/360/202?image=1043" />
</binding>
```

---


## <a name="image-size-restrictions"></a>Restricciones de tamaño de imagen

Las imágenes que se usan en la notificación del sistema se pueden incluir desde...

 - http://
 - ms-appx:///
 - ms-appdata:///

En el caso de las imágenes web remotas http y https, hay límites en el tamaño de archivo de cada imagen individual. En Fall Creators Update (16299), aumentamos el límite a 3 MB en las conexiones normales y 1 MB en las conexiones de uso medido. Antes de eso, las imágenes siempre se limitaban a 200 KB.

| Conexión normal | Conexión de uso medido | Antes de la actualización de Fall Creators |
| - | - | - |
| 3 MB | 1 MB | 200 KB |

Si una imagen supera el tamaño del archivo, o si no se puede descargar o se agota el tiempo de espera, se quitará la imagen y se mostrará el resto de la notificación.


## <a name="attribution-text"></a>Texto de atribución

**Novedades de la actualización de aniversario** : Si necesita hacer referencia al origen del contenido, puede usar el texto de atribución. Este texto siempre se muestra en la parte inferior de la notificación, junto con la identidad de la aplicación o la marca de tiempo de la notificación.

En versiones anteriores de Windows que no admiten texto de atribución, el texto se mostrará simplemente como otro elemento de texto (suponiendo que ya no tenga el máximo de tres elementos de texto).

<img alt="Toast with attribution text" src="images/toast-attributiontext.jpg" width="364"/>

#### <a name="builder-syntax"></a>[Sintaxis del compilador](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    ...
    
    .AddAttributionText("Via SMS");
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<binding template="ToastGeneric">
    ...
    <text placement="attribution">Via SMS</text>
</binding>
```

---


## <a name="custom-timestamp"></a>Marca de tiempo personalizada

**Novedad en Creators Update** : ahora puede invalidar la marca de tiempo proporcionada por el sistema con su propia marca de tiempo que representa con precisión cuándo se generó el mensaje, la información y el contenido. Esta marca de tiempo es visible en el centro de actividades.

<img alt="Toast with custom timestamp" src="images/toast-customtimestamp.jpg" width="396"/>

Para obtener más información sobre el uso de una marca de tiempo personalizada, consulte [marcas de tiempo personalizadas en la notificación del sistema](custom-timestamps-on-toasts.md).

#### <a name="builder-syntax"></a>[Sintaxis del compilador](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    ...
    
    .AddCustomTimeStamp(new DateTime(2017, 04, 15, 19, 45, 00, DateTimeKind.Utc));
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<toast displayTimestamp="2017-04-15T19:45:00Z">
  ...
</toast>
```

---


## <a name="progress-bar"></a>Barra de progreso

**Novedad en Creators Update** : puede proporcionar una barra de progreso en la notificación del sistema para mantener informado al usuario del progreso de las operaciones, como las descargas.

<img alt="Toast with progress bar" src="images/toast-progressbar.png" width="364"/>

Para obtener más información sobre el uso de una barra de progreso, consulte la [barra de progreso del sistema](toast-progress-bar.md).


## <a name="headers"></a>Encabezados

**Nuevo en Creators Update** : puede agrupar notificaciones en encabezados dentro del centro de actividades. Por ejemplo, puede agrupar los mensajes de un chat de grupo en un encabezado o agrupar las notificaciones de un tema común bajo un encabezado, o más.

<img alt="Toasts with header" src="images/toast-headers-action-center.png" width="396"/>

Para obtener más información sobre el uso de encabezados, consulte los [encabezados del sistema](toast-headers.md).


## <a name="adaptive-content"></a>Contenido adaptable

**Novedad en la actualización de aniversario** : además del contenido especificado anteriormente, también puede mostrar contenido adaptable adicional que está visible cuando se expande la notificación del sistema.

Este contenido adicional se especifica mediante Adaptive, en el que puede obtener más información leyendo la [documentación de iconos adaptables](create-adaptive-tiles.md).

Tenga en cuenta que cualquier contenido adaptable debe estar contenido dentro de un [**AdaptiveGroup**](./toast-schema.md#adaptivegroup). De lo contrario, no se representará mediante Adaptive.


### <a name="columns-and-text-elements"></a>Columnas y elementos de texto

Este es un ejemplo en el que se usan columnas y algunos elementos de texto adaptables avanzados. Dado que los elementos de texto están dentro de un **AdaptiveGroup** , admiten todas las propiedades de estilo adaptables enriquecidas.

<img alt="Toast with additional text" src="images/toast-additionaltext.jpg" width="364"/>

#### <a name="builder-syntax"></a>[Sintaxis del compilador](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    ...
    
    .AddVisualChild(new AdaptiveGroup()
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
    });
```

#### <a name="xml"></a>[XML](#tab/xml)

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

---


## <a name="buttons"></a>Botones

Los botones hacen que la notificación del sistema sea interactiva, lo que permite que el usuario realice acciones rápidas en la notificación del sistema sin interrumpir su flujo de trabajo actual. Por ejemplo, los usuarios pueden responder a un mensaje directamente desde una notificación del sistema o eliminar un correo electrónico sin necesidad de abrir la aplicación de correo electrónico. Los botones aparecen en la parte expandida de la notificación.

Para obtener más información sobre la implementación de botones de un extremo a otro, consulte envío de un [sistema de notificación local](send-local-toast.md).

Los botones pueden realizar las siguientes acciones diferentes...

-   Activar la aplicación en primer plano, con un argumento que se puede utilizar para navegar a una página o contexto concretos.
-   Activación de la tarea en segundo plano de la aplicación para una respuesta rápida o un escenario similar.
-   Activar otra aplicación a través del inicio de protocolo.
-   Realizar una acción del sistema, como posponer o descartar la notificación.

> [!NOTE]
> Solo puede tener hasta 5 botones (incluidos los elementos de menú contextual que se describen más adelante).

<img alt="notification with actions, example 1" src="images/adaptivetoasts-xmlsample02.jpg" width="364"/>

#### <a name="builder-syntax"></a>[Sintaxis del compilador](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    ...
    
    .AddButton("See more details", ToastActivationType.Foreground, "action=viewdetails&contentId=351")
    .AddButton("Remind me later", ToastActivationType.Background, "action=remindlater&contentId=351");
```

#### <a name="xml"></a>[XML](#tab/xml)

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

---


### <a name="buttons-with-icons"></a>Botones con iconos

Puede Agregar iconos a los botones. Estos iconos son imágenes de x 16 píxeles transparentes en blanco con una escala del 100% y no deben tener ningún relleno incluido en la propia imagen. Si elige proporcionar iconos en una notificación del sistema, debe proporcionar iconos para todos los botones de la notificación, ya que transforma el estilo de los botones en botones de icono.

> [!NOTE]
> Para obtener accesibilidad, asegúrese de incluir una versión en blanco de contraste del icono (un icono negro para el fondo blanco), de modo que cuando el usuario encienda contraste alto modo blanco, el icono esté visible. Obtenga más información en la [Página accesibilidad del sistema](tile-toast-language-scale-contrast.md).

<img src="images\adaptivetoasts-buttonswithicons.png" width="364" alt="Toast that has buttons with icons"/>

#### <a name="builder-syntax"></a>[Sintaxis del compilador](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    ...
    
    .AddButton(
        "Dismiss",
        ToastActivationType.Foreground,
        "dismiss", new Uri("Assets/NotificationButtonIcons/Dismiss.png", UriKind.Relative));
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<action
    content="Dismiss"
    imageUri="Assets/NotificationButtonIcons/Dismiss.png"
    arguments="dismiss"
    activationType="background"/>
```

---


### <a name="buttons-with-pending-update-activation"></a>Botones con activación de actualización pendiente

**Novedad en Fall Creators Update** : en los botones de activación en segundo plano, puede usar un comportamiento de activación después de **PendingUpdate** para crear interacciones de varios pasos en las notificaciones del sistema. Cuando el usuario hace clic en el botón, se activa la tarea en segundo plano y la notificación del sistema se coloca en un estado "pendiente de actualización", donde permanece en pantalla hasta que la tarea en segundo plano reemplaza la notificación del sistema por una nueva notificación.

Para obtener información sobre cómo implementar esto, consulte [actualización pendiente del sistema](toast-pending-update.md).

![Notificación del sistema con actualización pendiente](images/toast-pendingupdate.gif)


### <a name="context-menu-actions"></a>Acciones del menú contextual

**Novedades de la actualización de aniversario** : puede Agregar acciones adicionales del menú contextual al menú contextual existente que aparece cuando el usuario hace clic con el botón derecho en la notificación del sistema desde el centro de actividades. Tenga en cuenta que este menú solo aparece cuando se hace clic con el botón derecho desde el centro de actividades. No aparece al hacer clic con el botón derecho en un banner emergente del sistema.

> [!NOTE]
> En los dispositivos más antiguos, estas acciones adicionales del menú contextual aparecerán simplemente como botones normales en la notificación del sistema.

Las acciones adicionales del menú contextual que agregue (como "Cambiar ubicación") aparecen sobre las dos entradas predeterminadas del sistema.

<img alt="Toast with context menu" src="images/toast-contextmenu.png" width="444"/>

#### <a name="builder-syntax"></a>[Sintaxis del compilador](#tab/builder-syntax)

La sintaxis del compilador no es compatible con las acciones del menú contextual, por lo que se recomienda usar la sintaxis del inicializador.

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

#### <a name="xml"></a>[XML](#tab/xml)

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

---

> [!NOTE]
> Los elementos de menú contextual adicionales contribuyen al límite total de 5 botones en una notificación del sistema.

La activación de los elementos de menú contextual adicionales se controla de forma idéntica a los botones del sistema.


## <a name="inputs"></a>Entradas

Las entradas se especifican dentro de la región de acciones de la región del sistema de notificación, lo que significa que solo están visibles cuando se expande la notificación del sistema.


### <a name="quick-reply-text-box"></a>Cuadro de texto de respuesta rápida

Para habilitar un cuadro de texto de respuesta rápida (por ejemplo, en una aplicación de mensajería), agregue una entrada de texto y un botón, y haga referencia al identificador del campo de entrada de texto para que el botón se muestre al lado del campo de entrada. El icono del botón debe ser una imagen de 32x32 píxeles sin relleno, píxeles blancos establecidos en transparentes y escala del 100%.

<img alt="notification with text input and actions" src="images/adaptivetoasts-xmlsample05.jpg" width="364"/>

#### <a name="builder-syntax"></a>[Sintaxis del compilador](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    ...
    
    .AddInputTextBox("tbReply", "Type a reply")

    .AddButton(
        textBoxId: "tbReply", // To place button next to text box, reference text box's id
        content: "Reply",
        activationType: ToastActivationType.Background,
        arguments: "action=reply&convId=9318",
        imageUri: new Uri("Assets/Reply.png", UriKind.Relative));
```

#### <a name="xml"></a>[XML](#tab/xml)

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

---



### <a name="inputs-with-buttons-bar"></a>Entradas con barra de botones

También puede tener una o varias entradas con botones normales que se muestran debajo de las entradas.

<img alt="notification with text and input actions" src="images/adaptivetoasts-xmlsample04.jpg" width="364"/>

#### <a name="builder-syntax"></a>[Sintaxis del compilador](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    ...
    
    .AddInputTextBox("tbReply", "Type a reply")

    .AddButton("Reply", ToastActivationType.Background, "action=reply&threadId=9218")
    .AddButton("Video call", ToastActivationType.Foreground, "action=videocall&threadId=9218");
```

#### <a name="xml"></a>[XML](#tab/xml)

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

---


### <a name="selection-input"></a>Entrada de selección

Además de los cuadros de texto, también puede usar un menú de selección.

<img alt="notification with selection input and actions" src="images/adaptivetoasts-xmlsample06.jpg" width="364"/>

#### <a name="builder-syntax"></a>[Sintaxis del compilador](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    ...
    
    .AddToastInput(new ToastSelectionBox("time")
    {
        DefaultSelectionBoxItemId = "lunch",
        Items =
        {
            new ToastSelectionBoxItem("breakfast", "Breakfast"),
            new ToastSelectionBoxItem("lunch", "Lunch"),
            new ToastSelectionBoxItem("dinner", "Dinner")
        }
    })

    .AddButton(...)
    .AddButton(...);
```

#### <a name="xml"></a>[XML](#tab/xml)

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

---



### <a name="snoozedismiss"></a>Posponer/descartar

Con un menú de selección y dos botones, podemos crear una notificación de recordatorio que use las acciones posponer y descartar el sistema. Asegúrese de establecer el escenario para que se requiera que la notificación se comporte como un recordatorio.

<img alt="reminder notification" src="images/adaptivetoasts-xmlsample07.jpg" width="364"/>

Vinculamos el botón posponer a la entrada del menú de selección mediante la propiedad **SelectionBoxId** del botón notificación del sistema.

#### <a name="builder-syntax"></a>[Sintaxis del compilador](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    .SetToastScenario(ToastScenario.Reminder)
    
    ...
    
    .AddToastInput(new ToastSelectionBox("snoozeTime")
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
    })

    .AddButton(new ToastButtonSnooze() { SelectionBoxId = "snoozeTime" })
    .AddButton(new ToastButtonDismiss());
```

#### <a name="xml"></a>[XML](#tab/xml)

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

---

Para usar las acciones posponer y descartar el sistema:

-   Especifique un **ToastButtonSnooze** o **ToastButtonDismiss**
-   Opcionalmente, especifique una cadena de contenido personalizado:
    -   Si no proporciona una cadena, usaremos automáticamente cadenas localizadas para "posponer" y "descartar".
-   Opcionalmente, especifique **SelectionBoxId** :
    -   Si no quieres que el usuario seleccione un intervalo de aplazamiento y, en su lugar, solo quieres que la notificación se posponga solo una vez en un intervalo de tiempo definido por el sistema (que es coherente en todo el sistema operativo), no crees ningún &lt;input&gt; en absoluto.
    -   Si quieres proporcionar las selecciones de intervalo de aplazamiento:
        -   Especificar **SelectionBoxId** en la acción de posponer
        -   Coincide con el identificador de la entrada con el **SelectionBoxId** de la acción de posponer.
        -   Especifique el valor de **ToastSelectionBoxItem** para que sea un nonNegativeInteger que represente el intervalo de aplazamiento en minutos.



## <a name="audio"></a>Audio

Mobile audio siempre es compatible con Mobile y se admite en la versión de escritorio 1511 (compilación 10586) o posterior. Se puede hacer referencia a un audio personalizado a través de las siguientes rutas de acceso:

-   ms-appx:///
-   ms-appdata:///

Como alternativa, puede elegir en la [lista de MS-winsoundevents](/uwp/schemas/tiles/toastschema/element-audio#attributes-and-elements), que siempre se admiten en ambas plataformas.

#### <a name="builder-syntax"></a>[Sintaxis del compilador](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    ...
    
    .AddAudio(new Uri("ms-appx:///Assets/NewMessage.mp3"));
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<toast launch="app-defined-string">

    ...

    <audio src="ms-appx:///Assets/NewMessage.mp3"/>

</toast>
```

---


Consulte la [página esquema de audio](/uwp/schemas/tiles/toastschema/element-audio) para obtener información sobre el audio en las notificaciones del sistema. Para obtener información sobre cómo enviar una notificación del sistema con audio personalizado, consulte el artículo [sobre el audio personalizado en la notificación del sistema](custom-audio-on-toasts.md).


## <a name="alarms-reminders-and-incoming-calls"></a>Alarmas, recordatorios y llamadas entrantes

Para crear alarmas, recordatorios y notificaciones de llamadas entrantes, solo tiene que usar una notificación normal del sistema con un valor de escenario asignado. El escenario adusts algunos comportamientos para crear una experiencia de usuario coherente y unificada.

> [!IMPORTANT]
> Al usar recordatorios o alarmas, debe proporcionar al menos un botón en la notificación del sistema. De lo contrario, la notificación del sistema se tratará como una notificación normal.

* **Recordatorio** : la notificación permanecerá en pantalla hasta que el usuario la descarte o tome medidas. En Windows Mobile, la notificación del sistema también se mostrará expandida previamente. Se reproducirá un sonido de recordatorio.
* **Alarm** : además de los comportamientos de recordatorio, las alarmas también repetirán el audio con un sonido de alarma predeterminado.
* **IncomingCall** : las notificaciones de llamada entrantes se muestran en pantalla completa en dispositivos móviles de Windows. De lo contrario, tienen los mismos comportamientos que las alarmas, salvo que usan audio de tonos y sus botones tienen un estilo diferente.

#### <a name="builder-syntax"></a>[Sintaxis del compilador](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    .SetToastScenario(ToastScenario.Reminder)
    ...
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<toast scenario="reminder" launch="app-defined-string">

    ...

</toast>
```

---


## <a name="localization-and-accessibility"></a>Localización y accesibilidad

Los iconos y las notificaciones del sistema pueden cargar cadenas e imágenes personalizadas para el idioma de visualización, mostrar el factor de escala, el contraste alto y otros contextos en tiempo de ejecución. Para obtener más información, consulte [compatibilidad con las notificaciones de icono y del sistema en el idioma, la escala y el contraste alto](tile-toast-language-scale-contrast.md).


## <a name="handling-activation"></a>Controlar la activación
Para obtener información sobre cómo controlar las activaciones del sistema (el usuario hace clic en la notificación del sistema o los botones de la notificación del sistema), consulte envío de una notificación del sistema [local](send-local-toast.md).
 
## <a name="related-topics"></a>Temas relacionados

* [Enviar una notificación del sistema local y controlar la activación](send-local-toast.md)
* [Biblioteca de notificaciones en GitHub (parte del kit de herramientas de la comunidad de UWP)](https://github.com/windows-toolkit/WindowsCommunityToolkit/tree/master/Microsoft.Toolkit.Uwp.Notifications)
* [Compatibilidad con las notificaciones de icono y del sistema para el idioma, la escala y el contraste alto](tile-toast-language-scale-contrast.md)
