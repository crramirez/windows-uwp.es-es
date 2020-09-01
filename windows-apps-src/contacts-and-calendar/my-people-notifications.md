---
title: Notificaciones de Mis allegados
description: Explica cómo crear y usar notificaciones de mis personas, un nuevo tipo de notificación del sistema.
ms.date: 10/25/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3e00e3de9445a8b7c63ebaead70173c29b637b54
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166329"
---
# <a name="my-people-notifications"></a>Notificaciones de Mis allegados

Las notificaciones de mis personas proporcionan una nueva forma para que los usuarios se conecten con las personas que le interesan, a través de gestos rápidos de expresivo. En este artículo se muestra cómo diseñar e implementar notificaciones de mis personas en la aplicación. Para obtener implementaciones completas, consulte el [ejemplo de notificaciones de mis personas.](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/MyPeopleNotifications)

![notificación de emojis de corazón](images/heart-emoji-notification-small.gif)

## <a name="requirements"></a>Requisitos

+ Windows 10 y Microsoft Visual Studio 2019. Para obtener información detallada sobre la instalación, vea [configurar con Visual Studio](../get-started/get-set-up.md).
+ Conocimientos básicos de C# o algún lenguaje similar de programación orientado a objetos. Para empezar a trabajar con C#, vea [crear una aplicación "Hello, World"](../get-started/create-a-hello-world-app-xaml-universal.md).

## <a name="how-it-works"></a>Funcionamiento

Como alternativa a las notificaciones del sistema genéricas, ahora puede enviar notificaciones a través de la característica mis personas para ofrecer una experiencia más personal a los usuarios. Se trata de un nuevo tipo de notificación del sistema, enviado desde un contacto anclado en la barra de tareas del usuario con la característica mis personas. Cuando se reciba la notificación, la imagen del contacto del remitente se animará en la barra de tareas y se reproducirá un sonido, indicando que se está iniciando la notificación. La animación o imagen especificada en la carga se mostrará en 5 segundos (o, si la carga es una animación de menos de 5 segundos de duración, se repetirá hasta que transcurran 5 segundos).

## <a name="supported-image-types"></a>Tipos de imagen admitidos

+ GIF
+ Imagen estática (JPEG, PNG)
+ Spritesheet (solo vertical)

> [!NOTE]
> Un spritesheet es una animación derivada de una imagen estática (JPEG o PNG). Los fotogramas individuales se organizan verticalmente, de forma que el primer fotograma está en la parte superior (aunque puede especificar un marco de inicio diferente en la carga del sistema). Cada fotograma debe tener el mismo alto, que el programa recorre en bucle para crear una secuencia animada (como un libreto con sus páginas diseñadas verticalmente). A continuación se muestra un ejemplo de un spritesheet.

![arco iris spritesheet](images/shoulder-tap-rainbow-spritesheet.png)

## <a name="notification-parameters"></a>Parámetros de notificación
Las notificaciones de mis personas usan el marco de [notificación del sistema](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md) , pero requieren un nodo de enlace adicional en la carga del sistema. Este segundo enlace debe incluir el parámetro siguiente:

```xml
experienceType="shoulderTap"
```

Esto indica que la notificación del sistema debe tratarse como una notificación de mis personas.

El nodo imagen dentro del enlace debe incluir los siguientes parámetros:

+ **src**
    + El URI del recurso. Puede ser un URI Web HTTP/HTTPS, un URI de msappx o una ruta de acceso a un archivo local.
+ **spritesheet: src**
    + El URI del recurso. Puede ser un URI Web HTTP/HTTPS, un URI de msappx o una ruta de acceso a un archivo local. Solo se requiere para animaciones spritesheet.
+ **spritesheet: alto**
    + Alto del marco (en píxeles). Solo se requiere para animaciones spritesheet.
+ **spritesheet-fps**
    + Fotogramas por segundo (FPS). Solo se requiere para animaciones spritesheet. Solo se admiten los valores 1-120.
+ **spritesheet-startingFrame**
    + Número de marco en el que se va a comenzar la animación. Solo se usa para animaciones spritesheet y el valor predeterminado es 0 si no se proporciona.
+ **alt**
    + Cadena de texto que se usa para la narración del lector de pantalla.

> [!NOTE]
> Al crear una notificación animada, todavía debe especificar una imagen estática en el parámetro "src". Se usará como reversión Si la animación no se muestra.

Además, el nodo del sistema de notificación de nivel superior debe incluir el parámetro **Hint-People** para especificar el contacto de envío. Este parámetro puede tener los valores siguientes:

+ **Dirección de correo electrónico** 
    + Por ejemplo, ` mailto:johndoe@mydomain.com `
+ **Número de teléfono** 
    + Por ejemplo, Tel: 888-888-8888
+ **Id. remoto** 
    + Por ejemplo, remoteid: 1234

> [!NOTE]
> Si la aplicación usa las [API de ContactStore](/uwp/api/windows.applicationmodel.contacts.contactstore) y usa la propiedad [StoredContact. RemoteId](/uwp/api/Windows.Phone.PersonalInformation.StoredContact.RemoteId) para vincular contactos almacenados en el equipo con contactos almacenados de forma remota, es fundamental que el valor de la propiedad RemoteId sea estable y único. Esto significa que el identificador remoto debe identificar de forma coherente una sola cuenta de usuario y debe contener una etiqueta única para garantizar que no entra en conflicto con los identificadores remotos de otros contactos en el equipo, incluidos los contactos que son propiedad de otras aplicaciones.
> Si no se garantiza que los identificadores remotos usados por la aplicación sean estables y son únicos, puede usar la [clase RemoteIdHelper](/previous-versions/windows/apps/jj207024(v=vs.105)#BKMK_UsingtheRemoteIdHelperclass) para agregar una etiqueta única a todos los identificadores remotos antes de agregarlos al sistema. Como alternativa, puede optar por no utilizar la propiedad RemoteId en absoluto y, en su lugar, crear una propiedad extendida personalizada en la que almacenar los identificadores remotos para sus contactos.

Además del segundo enlace y carga, debe incluir otra carga en el primer enlace para la notificación del sistema de reserva. La notificación lo usará si se fuerza a revertir a una notificación del sistema normal (se explica con más detalle al [final de este artículo](#falling-back-to-toast)).

## <a name="creating-the-notification"></a>Creación de la notificación
Puede crear una plantilla de notificación mis personas tal como lo haría con una [notificación del sistema](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md).

Este es un ejemplo de cómo crear una notificación de mis personas con una carga de imagen estática:

```xml
<toast hint-people="mailto:johndoe@mydomain.com">
    <visual lang="en-US">
        <binding template="ToastGeneric">
            <text hint-style="body">Toast fallback</text>
            <text>Add your fallback toast content here</text>
        </binding>
        <binding template="ToastGeneric" experienceType="shoulderTap">
            <image src="https://docs.microsoft.com/windows/uwp/contacts-and-calendar/images/shoulder-tap-static-payload.png"/>
        </binding>
    </visual>
</toast>
```

Al iniciar la notificación, debe tener el siguiente aspecto:

![notificación de imagen estática](images/static-image-notification-small.gif)

Este es un ejemplo de cómo crear una notificación con una carga spritesheet animada. Este spritesheet tiene un alto de fotogramas de 80 píxeles, que se animará en 25 fotogramas por segundo. Establecemos el fotograma inicial en 15 y lo proporcionamos con una imagen de reserva estática en el parámetro "src". La imagen de reserva se usa si la animación spritesheet no se puede mostrar.

```xml
<toast hint-people="mailto:johndoe@mydomain.com">
    <visual lang="en-US">
        <binding template="ToastGeneric">
            <text hint-style="body">Toast fallback</text>
            <text>Add your fallback toast content here</text>
        </binding>
        <binding template="ToastGeneric" experienceType="shoulderTap">
            <image src="https://docs.microsoft.com/windows/uwp/contacts-and-calendar/images/shoulder-tap-pizza-static.png"
                spritesheet-src="https://docs.microsoft.com/windows/uwp/contacts-and-calendar/images/shoulder-tap-pizza-spritesheet.png"
                spritesheet-height='80' spritesheet-fps='25' spritesheet-startingFrame='15'/>
        </binding>
    </visual>
</toast>
```

Al iniciar la notificación, debe tener el siguiente aspecto:

![notificación spritesheet](images/pizza-notification-small.gif)

## <a name="starting-the-notification"></a>Inicio de la notificación
Para iniciar una notificación de mis personas, es necesario convertir la plantilla de notificación del sistema en un objeto [XmlDocument](/uwp/api/windows.data.xml.dom.xmldocument) . Cuando haya definido la notificación del sistema en un archivo XML (aquí llamado "content.xml"), puede usar este código para iniciarlo:

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

## <a name="falling-back-to-toast"></a>Vuelta a la notificación del sistema
Hay algunos casos en los que una notificación mis personas se muestra como una notificación del sistema normal. Una notificación mis personas revertirá a la notificación del sistema en las siguientes condiciones:

+ No se puede mostrar la notificación
+ El destinatario no habilita las notificaciones de mis personas
+ El contacto del remitente no está anclado a la barra de tareas del receptor

Si una notificación de mis personas recurre a la notificación del sistema, se omite el segundo enlace específico de My People y solo se usa el primer enlace para mostrar la notificación del sistema. Esta es la razón por la que es fundamental proporcionar una carga de reserva en el primer enlace del sistema.

## <a name="see-also"></a>Vea también
+ [Ejemplo de notificaciones de mis personas](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/MyPeopleNotifications)
+ [Incorporación de compatibilidad con mis personas](my-people-support.md)
+ [Notificaciones del sistema adaptables](../design/shell/tiles-and-notifications/adaptive-interactive-toasts.md)
+ [Clase ToastNotification](/uwp/api/windows.ui.notifications.toastnotification)