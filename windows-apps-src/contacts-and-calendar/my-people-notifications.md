---
title: Notificaciones de Mis allegados
description: Explica cómo crear y usar notificaciones de Mis allegados, un nuevo tipo de notificaciones del sistema.
ms.date: 10/25/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: db25954b7fc6541ac5f5900236e61cb8da488be6
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2018
ms.locfileid: "8922386"
---
# <a name="my-people-notifications"></a>Notificaciones de Mis allegados

Las notificaciones de Mis allegados proporcionan una nueva forma para que los usuarios puedan conectarse con las personas que les importan mediante gestos rápidos. Este artículo muestra cómo diseñar e implementar las notificaciones de Mis allegados en tu aplicación. Para implementaciones completas, consulta el [ejemplo de notificaciones de Mis allegados.](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/MyPeopleNotifications)

![notificación de emoji de corazón](images/heart-emoji-notification-small.gif)

## <a name="requirements"></a>Requisitos

+ Windows 10 y Microsoft Visual Studio 2017. Para obtener detalles sobre la instalación, consulta [Prepararse para Visual Studio](https://docs.microsoft.com/en-us/windows/uwp/get-started/get-set-up).
+ Conocimientos básicos de C# o algún lenguaje de programación orientado a objetos similar. Para comenzar con C#, consulta [Crear una aplicación "Hello, world"](https://docs.microsoft.com/en-us/windows/uwp/get-started/create-a-hello-world-app-xaml-universal).

## <a name="how-it-works"></a>Funcionamiento

Como alternativa a las notificaciones del sistema genéricas, ahora puedes enviar notificaciones a través de la función Mis allegados para proporcionar una experiencia más personal a los usuarios. Se trata de un nuevo tipo de notificación del sistema, que se envía desde un contacto anclado a la barra de tareas del usuario con la función Mis allegados. Al recibir la notificación, la imagen del contacto del remitente se animará en la barra de tareas y se reproducirá un sonido, lo que indicará que se está iniciando la notificación de Mis allegados. Se mostrará la animación o imagen especificada en la carga durante 5 segundos (o bien, si la carga es una animación que dura menos de 5 segundos, se repetirá hasta que transcurra este tiempo).

## <a name="supported-image-types"></a>Tipos de archivo admitidos

+ GIF
+ Imagen estática (JPEG, PNG)
+ Spritesheet (solo en vertical)

> [!NOTE]
> Una spritesheet es una animación que se deriva de una imagen estática (JPEG o PNG). Los fotogramas individuales se organizan verticalmente, de manera que el primer fotograma queda en la parte superior (aunque puedes especificar un fotograma de inicio diferente en la carga de notificaciones). Cada fotograma debe tener la misma altura, y con ellos el programa realizará un bucle para crear una secuencia animada (como un folioscopio con sus páginas dispuestas verticalmente). A continuación se muestra un ejemplo de spritesheet.

![spritesheet arco iris](images/shoulder-tap-rainbow-spritesheet.png)

## <a name="notification-parameters"></a>Parámetros de notificación
Las notificaciones de Mis allegados usan el marco de [notificaciones del sistema](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md), pero requieren un nodo de enlace adicional en la carga de notificaciones. Este segundo enlace debe incluir el parámetro siguiente:

```xml
experienceType=”shoulderTap”
```

Esto indica que la notificación del sistema debe tratarse como una notificación de Mis allegados.

El nodo de la imagen del interior del enlace debe incluir los siguientes parámetros:

+ **src**
    + La URI del activo. Esta puede ser una URI de web HTTP/HTTPS, una URI msappx o una ruta de acceso a un archivo local.
+ **spritesheet-src**
    + La URI del activo. Esta puede ser una URI de web HTTP/HTTPS, una URI msappx o una ruta de acceso a un archivo local. Solo es necesaria para animaciones de spritesheet.
+ **spritesheet-height**
    + Altura del fotograma (en píxeles). Solo es necesaria para animaciones de spritesheet.
+ **spritesheet-fps**
    + Fotogramas por segundo (FPS). Solo es necesario para animaciones de spritesheet. Solo se admiten los valores 1-120.
+ **spritesheet-startingFrame**
    + Número de fotogramas para comenzar la animación. Solo se usa para animaciones de spritesheet y, de no facilitarse, su valor predeterminado es 0.
+ **alt**
    + Cadena de texto que se usa para la narración del lector de pantalla.

> [!NOTE]
> Al realizar una notificación animada, deberás especificar una imagen estática en el parámetro "src". Se usará como una reserva si no puede mostrarse la animación.

Además, el nodo del sistema de nivel superior debe incluir el parámetro **hint-people** para especificar el contacto remitente. Este parámetro puede tener cualquiera de los valores siguientes:

+ **Dirección de correo electrónico** 
    + P. ej. mailto:johndoe@mydomain.com
+ **Número de teléfono** 
    + P. ej. tel:888-888-8888
+ **ID remoto** 
    + P. ej. remoteid:1234

> [!NOTE]
> Si tu aplicación utiliza las [API ContactStore](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.contacts.contactstore) y la propiedad [StoredContact.RemoteId](https://docs.microsoft.com/en-us/uwp/api/Windows.Phone.PersonalInformation.StoredContact.RemoteId) para vincular contactos almacenados en el PC con contactos almacenados de forma remota, es esencial que el valor de la propiedad RemoteId sea único y estable. Esto significa que el identificador remoto debe identificar de forma consistente una sola cuenta de usuario y debería contener una etiqueta única que no entre en conflicto con los identificadores remotos de otros contactos del PC, incluidos los contactos propiedad de otras aplicaciones.
> Si no se garantiza que los identificadores remotos utilizados por tu aplicación sean únicos y estables, puedes utilizar la [clase RemoteIdHelper](https://msdn.microsoft.com/en-us/library/windows/apps/jj207024(v=vs.105).aspx#BKMK_UsingtheRemoteIdHelperclass) para agregar una etiqueta única a todos tus identificadores remotos antes de agregarlos al sistema. O puedes decidir no utilizar en absoluto la propiedad RemoteId y en su lugar crear una propiedad personalizada extendida en la que almacenar los identificadores remotos de tus contactos.

Además del segundo enlace y la carga, debes incluir otra carga en el primer enlace para la notificación del sistema reserva. La notificación usará esto si se fuerza a revertir a una notificación del sistema normal (explicado detalladamente al [final de este artículo](https://review.docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/my-people-notifications#falling-back-to-toast)).

## <a name="creating-the-notification"></a>Creación de la notificación
Puedes crear una plantilla de notificación de Mis allegados tal como lo harías para una [notificación del sistema](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md).

Este es un ejemplo de cómo crear una notificación de Mis allegados con una carga de imagen estática:

```xml
<toast hint-people="mailto:johndoe@mydomain.com">
    <visual lang="en-US">
        <binding template="ToastGeneric">
            <text hint-style="body">Toast fallback</text>
            <text>Add your fallback toast content here</text>
        </binding>
        <binding template="ToastGeneric" experienceType="shoulderTap">
            <image src="https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/images/shoulder-tap-static-payload.png"/>
        </binding>
    </visual>
</toast>
```

Cuando inicies la notificación, debería tener el siguiente aspecto:

![notificación de imagen estática](images/static-image-notification-small.gif)

Este es un ejemplo de cómo crear una notificación con una carga spritesheet animado. Esta spritesheet tiene una altura de fotograma de 80 píxeles y la animaremos a 25 fotogramas por segundo. Establecemos el fotograma inicial en 15 y le proporcionamos una imagen estática de reserva en el parámetro "src". Si se produce un error al mostrar la animación de la spritesheet, se usará la imagen de reserva.

```xml
<toast hint-people="mailto:johndoe@mydomain.com">
    <visual lang="en-US">
        <binding template="ToastGeneric">
            <text hint-style="body">Toast fallback</text>
            <text>Add your fallback toast content here</text>
        </binding>
        <binding template="ToastGeneric" experienceType="shoulderTap">
            <image src="https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/images/shoulder-tap-pizza-static.png"
                spritesheet-src="https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/images/shoulder-tap-pizza-spritesheet.png"
                spritesheet-height='80' spritesheet-fps='25' spritesheet-startingFrame='15'/>
        </binding>
    </visual>
</toast>
```

Cuando inicies la notificación, debería tener el siguiente aspecto:

![notificación spritesheet](images/pizza-notification-small.gif)

## <a name="starting-the-notification"></a>Inicio de la notificación
Para iniciar una notificación de Mis allegados, necesitamos convertir la plantilla de notificación del sistema en un objeto [XmlDocument](https://msdn.microsoft.com/en-us/library/windows/apps/windows.data.xml.dom.xmldocument.aspx). Cuando definas la notificación del sistema en un archivo XML (aquí denominado "content.xml"), puedes usar este código para iniciarla:

```CSharp
string xmlText = File.ReadAllText("content.xml");
XmlDocument xmlContent = new XmlDocument();
xmlContent.LoadXml(xmlText);
```

A continuación, puedes usar este código para crear y enviar la notificación:

```CSharp
ToastNotification notification = new ToastNotification(xmlContent);
ToastNotificationManager.CreateToastNotifier().Show(notification);
```

## <a name="falling-back-to-toast"></a>Vuelta a notificación del sistema
No obstante, hay algunos casos en los que una notificación de Mis allegados se mostrará como una notificación del sistema normal. Una notificación de Mis allegados volverá a ser del sistema en las siguientes condiciones:

+ La notificación no consigue mostrarse
+ El destinatario no tiene habilitadas las notificaciones de Mis allegados
+ El contacto del remitente no está anclado a la barra de tareas del destinatario

Si una notificación de Mis allegados vuelve a ser notificación del sistema, se omite el segundo enlace específico de Mis allegados, y solo se usa el primer enlace para visualizar la notificación del sistema. Por eso es fundamental proporcionar una carga de reserva en el primer enlace de notificación del sistema.

## <a name="see-also"></a>Ver también
+ [Ejemplo de notificaciones de Mis allegados](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/MyPeopleNotifications)
+ [Agregar compatibilidad con Mis allegados](my-people-support.md)
+ [Notificaciones del sistema adaptables](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md)
+ [Clase ToastNotification](https://docs.microsoft.com/en-us/uwp/api/windows.ui.notifications.toastnotification)