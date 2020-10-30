---
description: Aprenda a usar encabezados para agrupar visualmente las notificaciones del sistema en el centro de actividades.
title: Encabezados del sistema
label: Toast headers
template: detail.hbs
ms.date: 12/07/2017
ms.topic: article
keywords: Windows 10, UWP, notificación del sistema, encabezado, encabezados del sistema, notificación, notificaciones de grupo, centro de actividades
ms.localizationpriority: medium
ms.openlocfilehash: 1afc354b15b7c916426ca3c0a7130b777c21e0cf
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033078"
---
# <a name="toast-headers"></a>Encabezados del sistema

Puede agrupar visualmente un conjunto de notificaciones relacionadas en el centro de actividades mediante el uso de un encabezado del sistema en las notificaciones.

> [!IMPORTANT]
> **Requiere Desktop Creators Update y 1.4.0 de la biblioteca de notificaciones** : debe estar ejecutando desktop Build 15063 o superior para ver los encabezados del sistema. Debe usar la versión 1.4.0 o posterior de la [biblioteca de NuGet de notificaciones](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) de la comunidad de UWP para construir el encabezado en el contenido de la notificación del sistema. Los encabezados solo se admiten en el escritorio.

Como se muestra a continuación, esta conversación de grupo está unificada en un único encabezado, "acampada!". Cada mensaje individual de la conversación es una notificación del sistema independiente que comparte el mismo encabezado del sistema.

<img alt="Toasts with header" src="images/toast-headers-action-center.png" width="396"/>

También puede agrupar visualmente las notificaciones por categoría, como recordatorios de vuelos, seguimiento de paquetes, etc.

## <a name="add-a-header-to-a-toast"></a>Agregar un encabezado a una notificación del sistema

Aquí se muestra cómo agregar un encabezado a una notificación del sistema.

> [!NOTE]
> Los encabezados solo se admiten en el escritorio. Los dispositivos que no admiten encabezados simplemente omitirán el encabezado.

#### <a name="builder-syntax"></a>[Sintaxis del compilador](#tab/builder-syntax)

```csharp
new ToastContentBuilder()
    .AddHeader("6289", "Camping!!", "action=openConversation&id=6289")
    .AddText("Anyone have a sleeping bag I can borrow?");
```

#### <a name="xml"></a>[XML](#tab/xml)

```xml
<toast>

    <header
        id="6289"
        title="Camping!!"
        arguments="action=openConversation&amp;id=6289"/>

    <visual>
        ...
    </visual>

</toast>
```

---

En resumen...

1. Agregue el **encabezado** a su **ToastContent**
2. Asignar las propiedades de **identificador** , **título** y **argumentos** necesarios
3. Enviar la notificación ([más información](send-local-toast.md))
4. En otra notificación, utilice el mismo **identificador** de encabezado para unificarlos en el encabezado. El **identificador** es la única propiedad que se usa para determinar si se deben agrupar las notificaciones, lo que significa que el **título** y los **argumentos** pueden ser diferentes. Se usan el **título** y los **argumentos** de la notificación más reciente dentro de un grupo. Si se quita esa notificación, el **título** y los **argumentos** recurren a la siguiente notificación más reciente.


## <a name="handle-activation-from-a-header"></a>Controlar la activación desde un encabezado

Los usuarios pueden hacer clic en los encabezados para que el usuario pueda hacer clic en el encabezado para obtener más información de la aplicación.

Por lo tanto, las aplicaciones pueden proporcionar **argumentos** en el encabezado, de forma similar a los argumentos Launch en el propio sistema.

La activación se controla de manera idéntica a la [activación normal del sistema](send-local-toast.md#step-4-handling-activation), lo que significa que puede recuperar estos argumentos en el método **onactivated** de `App.xaml.cs` igual que cuando el usuario hace clic en el cuerpo de la notificación del sistema o en un botón de la notificación del sistema.

```csharp
protected override void OnActivated(IActivatedEventArgs e)
{
    // Handle toast activation
    if (e is ToastNotificationActivatedEventArgs)
    {
        // Arguments specified from the header
        string arguments = (e as ToastNotificationActivatedEventArgs).Argument;
    }
}
```


## <a name="additional-info"></a>Información adicional

El encabezado separa visualmente y agrupa las notificaciones. No cambia ningún otro valor de logística sobre el número máximo de notificaciones que puede tener una aplicación (20) y el comportamiento del primero en el primero en salir de la lista de notificaciones.

El orden de las notificaciones dentro de los encabezados es el siguiente... En el caso de una aplicación determinada, la notificación más reciente de la aplicación (y el grupo de encabezado completo si es parte de un encabezado) aparecerá en primer lugar.

El **identificador** puede ser cualquier cadena que elija. No hay restricciones de longitud o de caracteres en ninguna de las propiedades de **ToastHeader** . La única restricción es que todo el contenido del sistema XML no puede ser superior a 5 KB.

La creación de encabezados no cambia el número de notificaciones que se muestran dentro del centro de actividades antes de que aparezca el botón "ver más" (este número es 3 de forma predeterminada y puede ser configurado por el usuario para cada aplicación en la configuración del sistema para las notificaciones).

Al hacer clic en el encabezado, al igual que al hacer clic en el título de la aplicación, no se borran las notificaciones que pertenecen a este encabezado (la aplicación debe usar las API del sistema de notificación para borrar las notificaciones pertinentes).


## <a name="related-topics"></a>Temas relacionados

- [Enviar una notificación del sistema local y controlar la activación](send-local-toast.md)
- [Documentación del contenido del sistema](adaptive-interactive-toasts.md)
