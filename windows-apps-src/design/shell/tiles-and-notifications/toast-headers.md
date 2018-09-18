---
author: andrewleader
Description: Learn how to use headers to visually group your toast notifications in Action Center.
title: Encabezados de notificaciones del sistema
label: Toast headers
template: detail.hbs
ms.author: mijacobs
ms.date: 12/7/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, notificación del sistema, encabezado, encabezados de notificación del sistema, notificación, notificaciones del sistema de grupo, Centro de actividades
ms.localizationpriority: medium
ms.openlocfilehash: 0b3c92a41832729b5a60411308d010c3cbb4470a
ms.sourcegitcommit: f5321b525034e2b3af202709e9b942ad5557e193
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "4022091"
---
# <a name="toast-headers"></a>Encabezados de notificaciones del sistema

Puedes agrupar visualmente un conjunto de notificaciones relacionadas dentro del Centro de actividades usando un encabezado de notificación del sistema en las notificaciones.

> [!IMPORTANT]
> **Requiere CreatorsUpdate de Escritorio y 1.4.0 de la biblioteca de notificaciones**: debes estar ejecutando Escritorio compilación 15063 o superior para ver los encabezados de notificaciones del sistema. Debes usar la versión 1.4.0 o superior de la [Biblioteca NuGet de notificaciones del Kit de herramientas de la comunidad de UWP](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) para construir el encabezado en el contenido de tu notificación del sistema. Los encabezados solo se admiten en Escritorio.

Tal como se muestra a continuación, esta conversación de grupo se unifica bajo un encabezado único, "Camping!!" (¡¡Acampada!!). Cada mensaje individual de la conversación es una notificación del sistema independiente que comparte el mismo encabezado del sistema.

<img alt="Toasts with header" src="images/toast-headers-action-center.png" width="396"/>

También puedes elegir agrupar visualmente tus notificaciones por categoría, como recordatorios de vuelo, seguimiento de paquetes y mucho más.

## <a name="add-a-header-to-a-toast"></a>Agregar un encabezado a una notificación del sistema

Así es cómo agregas un encabezado a una notificación del sistema.

> [!NOTE]
> Los encabezados solo se admiten en Escritorio. Los dispositivos que no admiten encabezados simplemente omitirán el encabezado.

```csharp
ToastContent toastContent = new ToastContent()
{
    Header = new ToastHeader()
    {
        Id = "6289",
        Title = "Camping!!",
        Arguments = "action=openConversation&id=6289",
    },

    Visual = new ToastVisual() { ... }
};
```

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

En resumen...

1. Agrega el **Encabezado** a tu **ToastContent**
2. Asigna las propiedades **Id**, **Title** y **Arguments** necesarias
3. Enviar tu notificación ([más información](send-local-toast.md))
4. En otra notificación, usa el mismo **Id** de encabezado para unificarlos bajo el encabezado. **Id** es la única propiedad que se usa para determinar si se deben agrupar las notificaciones, lo que significa que **Title** y **Arguments** pueden ser diferentes. Se usan **Title** y **Arguments** desde la notificación más reciente dentro de un grupo. Si se quita esa notificación, **Title** y **Arguments** vuelven a la siguiente notificación más reciente.


## <a name="handle-activation-from-a-header"></a>Controlar la activación desde un encabezado

Los usuarios pueden hacer clic en los encabezados para obtener más información de la aplicación.

Por lo tanto, las aplicaciones pueden proporcionar **Arguments** en el encabezado, de manera similar a los argumentos de inicio de la propia notificación del sistema.

La activación se controla de manera idéntica a la [activación de notificación del sistema normal](send-local-toast.md#handling-activation-1), lo que significa que puedes recuperar estos argumentos en el método **OnActivated** `App.xaml.cs` como lo harías si el usuario hace clic en el cuerpo de tu notificación del sistema o en un botón de tu notificación del sistema.

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

El encabezado separa visualmente las notificaciones y las agrupa. No cambia ninguna otra logística sobre el número máximo de notificaciones que una aplicación puede tener (20) y el comportamiento del primero en entrar es el primero en salir de la lista de notificaciones.

El orden de las notificaciones dentro de los encabezados es el siguiente: para una aplicación determinada, la notificación de la aplicación más reciente (y el grupo de encabezado completo si forma parte de un encabezado) aparecerá en primer lugar.

El valor de **Id** puede ser cualquier cadena que elijas. No existen restricciones de caracteres o longitud en cualquiera de las propiedades de **ToastHeader**. La única restricción es que el contenido de notificación del sistema XML completo no puede ser superior a 5KB.

La creación de encabezados no cambia el número de notificaciones que se muestran en el Centro de actividades antes de aparezca el botón "Ver más" (este número es 3 de manera predeterminada y lo puede configurar el usuario para cada aplicación de la Configuración del sistema para notificaciones).

Al hacer clic en el encabezado, igual que al hacer clic en el título de la aplicación, no borra todas las notificaciones que pertenecen a este encabezado (la aplicación debe usar las API de notificación del sistema para borrar las notificaciones relevantes).


## <a name="related-topics"></a>Temas relacionados

- [Enviar una notificación del sistema local y administrar la aplicación](send-local-toast.md)
- [Documentación del contenido de la notificación del sistema](adaptive-interactive-toasts.md)