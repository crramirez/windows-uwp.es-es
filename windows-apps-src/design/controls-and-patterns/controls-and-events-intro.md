---
author: Jwmsft
Description: You create the UI for your app by using controls such as buttons, text boxes, and combo boxes to display data and get user input. Here, we show you how to add controls to your app.
title: Introducción a los controles y patrones
ms.assetid: 64740BF2-CAA1-419E-85D1-42EE7E15F1A5
label: Intro to controls and patterns
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 6f8f86a6988e68e3ff8d2dfef32512633b3761fd
ms.sourcegitcommit: 194ab5aa395226580753869c6b66fce88be83522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "4155514"
---
# <a name="intro-to-controls-and-patterns"></a>Introducción a los controles y patrones

En el desarrollo de aplicaciones para UWP, un *control* es un elemento de la interfaz de usuario que muestra contenido o permite la interacción. Crea la interfaz de usuario de la aplicación con controles, como botones, cuadros de texto y cuadros combinados para mostrar los datos y las entradas de texto del usuario.

> **API importantes**: [Espacio de nombres Windows.UI.Xaml.Controls](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.aspx)

Un *patrón* es una receta para modificar un control o combinar varios controles con el fin de hacer algo nuevo. Por ejemplo, el patrón de [maestro y detalles](master-details.md) es una forma que puedes usar un control [SplitView](split-view.md) para la navegación de la aplicación. Del mismo modo, puedes personalizar la plantilla de un control de [NavigationView](navigationview.md) para implementar el patrón de tabulación.

En muchos casos, puedes usar el control tal cual. Sin embargo, los controles de XAML separan la función de la estructura y la apariencia, por lo que puedes realizar varios niveles de modificaciones que se adapten a tus necesidades. En la sección [Estilo](../style/index.md), aprenderás cómo usar [Estilos XAML](xaml-styles.md) y [plantillas de control](control-templates.md) para modificar un control.

En esta sección, se proporcionan instrucciones para cada uno de los controles XAML que puedes usar para crear la interfaz de usuario de la aplicación. Para empezar, en este artículo se muestra cómo agregar controles a tu aplicación. Hay 3 pasos clave para usar controles en la aplicación:

- Agrega un control a la interfaz de usuario de la aplicación.
- Establece las propiedades para el control, como el ancho, el alto o el color de primer plano.
- Agrega código a los controladores de eventos del control para que realice una acción. 

## <a name="add-a-control"></a>Agregar controles
Puedes agregar un control a una aplicación de varias formas:
 
- Usa una herramienta de diseño como Blend para Visual Studio o el diseñador de lenguaje XAML de Microsoft Visual Studio. 
- Agrega el control al marcado XAML en el editor XAML de Visual Studio. 
- Agrega el control en el código. Los controles que agregas en el código aparecen visibles cuando se ejecuta la aplicación, pero no lo están en el diseñador XAML de Visual Studio.

En Visual Studio, cuando agregas y manipulas controles en tu aplicación, puedes usar muchas de las características del programa, como el cuadro de herramientas, el diseñador XAML, el editor XAML y la ventana Propiedades. 

El Cuadro de herramientas de Visual Studio muestra muchos de los controles que puedes usar en tu aplicación. Para agregar un control a tu aplicación, haz doble clic en él en el Cuadro de herramientas. Por ejemplo, cuando haces doble clic en el control TextBox, se agrega este XAML a la vista XAML. 

```xaml
<TextBox HorizontalAlignment="Left" Text="TextBox" VerticalAlignment="Top"/>
```

También puedes arrastrar el control desde el Cuadro de herramientas hasta el diseñador XAML.

## <a name="set-the-name-of-a-control"></a>Establecer el nombre de un control

Para trabajar con un control en el código, hay que establecer su atributo [x:Name](../../xaml-platform/x-name-attribute.md) y hacer referencia a este por su nombre en el código. Puedes definir el nombre en la ventana Propiedades de VisualStudio o en XAML. Esta es la forma de establecer el nombre del control seleccionado actualmente usando el cuadro de texto Nombre de la parte superior de la ventana Propiedades.

Para definir el nombre de un control
1. Selecciona el elemento que quieras denominar.
2. En el panel Propiedades, escribe un nombre en el cuadro de texto Nombre.
3. Presiona Entrar para confirmar el nombre.

![Propiedad Name en el diseñador de Visual Studio](images/add-controls-control-name-designer.png)

Esta es la forma de establecer el nombre de un control en el editor XAML agregando el atributo x:Name.

```xaml
<Button x:Name="Button1" Content="Button"/>
```

## <a name="set-the-control-properties"></a>Establecer las propiedades el control 

Usas las propiedades para especificar la apariencia, el contenido y otros atributos de los controles. Cuando agregas un control con una herramienta de diseño, es posible que Visual Studio establezca automáticamente algunas propiedades que controlan el tamaño, la posición y el contenido. Puedes cambiar algunas propiedades, como Width, Height o Margin, si seleccionas y manipulas el control en la ventana Diseño. Esta ilustración muestra algunas de las herramientas de cambio de tamaño disponibles en la vista Diseño. 

![Herramientas de cambio de tamaño en el diseñador de Visual Studio](images/add-controls-resizing-designer.png)

Te resultará conveniente permitir que el tamaño y la posición del control se establezcan de manera automática. En este caso, puedes restablecer las propiedades de tamaño y posición que Visual Studio estableció automáticamente.

Para restablecer una propiedad
1. En el panel Propiedades, haz clic en el marcador de propiedades junto al valor de propiedad. Se abre el menú de propiedad.
2. En el menú de propiedad, haz clic en Restablecer.

![La propiedad Visual Studio restablece la opción de menú](images/add-controls-property-reset.png)

Puedes establecer las propiedades del control en la ventana Propiedades, en XAML o en el código. Por ejemplo, para cambiar el color de primer plano para un Button, estableces la propiedad Foreground del control. Esta ilustración muestra cómo establecer la propiedad Foreground mediante el Selector de colores en la ventana Propiedades. 

![Selector de colores en el diseñador de Visual Studio](images/add-controls-foreground-designer.png)

Así se establece la propiedad Foreground en el editor XAML. Observa la ventana IntelliSense de VisualStudio que se abre para ayudarte con la sintaxis. 

![IntelliSense en la parte 1 de XAML](images/add-controls-foreground-xaml.png)

![IntelliSense en la parte 2 de XAML](images/add-controls-foreground-xaml-2.png)

Este es el código XAML resultante después de establecer la propiedad Foreground. 

```xaml
<Button x:Name="Button1" Content="Button" 
        HorizontalAlignment="Left" VerticalAlignment="Top"
        Foreground="Beige"/>
```

Así se establece la propiedad Foreground en el código. 

```csharp
Button1.Foreground = new SolidColorBrush(Windows.UI.Colors.Beige);
```

## <a name="create-an-event-handler"></a>Crear un controlador de eventos 

Cada control tiene eventos que te permiten responder a acciones del usuario u otros cambios en la aplicación. Por ejemplo, un control Button contiene un evento Click que se genera cuando un usuario hace clic en Button. Creas un método, llamado controlador de eventos, para controlar el evento. Puedes asociar el evento de un control con un método del controlador de eventos en la ventana Propiedades, en XAML o en el código. Para más información acerca de los eventos, consulta el tema de [introducción a los eventos y eventos enrutados](../../xaml-platform/events-and-routed-events-overview.md).

Para crear un controlador de eventos, selecciona el control y después haz clic en la pestaña Eventos en la parte superior de la ventana Propiedades. En la ventana Propiedades, aparecen todos los eventos disponibles para ese control. Estos son algunos de los eventos para un Button.

![Lista de eventos de Visual Studio](images/add-controls-add-event-designer.png)

Para crear un controlador de eventos con el nombre predeterminado, haz doble clic en el cuadro de texto junto al nombre del evento en la ventana Propiedades. Para crear un controlador de eventos con un nombre personalizado, escribe el nombre que prefieras en el cuadro de texto y presiona Entrar. Se crea el controlador de eventos y se abre el archivo de código subyacente en el editor de código. El método del controlador de eventos tiene dos parámetros. El primero es `sender`, que es una referencia al objeto donde se adjunta el controlador. El parámetro `sender` es un tipo **Object**. Por lo general, transmites el objeto `sender` a un tipo más preciso si esperas comprobar o cambiar el estado en el propio objeto `sender`. En función del diseño de tu aplicación, esperas que el tipo al que se transmita el contenido `sender` sea seguro, según la ubicación en la que se adjuntó el controlador. El segundo valor son los datos del evento, que generalmente aparecen en las firmas como el parámetro `e` o `args`.

Este es el código que controla el evento Click de un Button llamado `Button1`. Al hacer clic en el botón, la propiedad Foreground del Button en el que hiciste clic se establece en blue. 

```csharp
private void Button_Click(object sender, RoutedEventArgs e)
{
    Button b = (Button)sender;
    b.Foreground = new SolidColorBrush(Windows.UI.Colors.Blue);
}
```

También puedes asociar un controlador de eventos en XAML. En el editor XAML, escribe el nombre del evento que quieras controlar. Visual Studio muestra una ventana IntelliSense cuando empiezas a escribir. Después de especificar el evento, puedes hacer doble clic en `<New Event Handler>` en la ventana IntelliSense para crear un nuevo controlador de eventos con el nombre predeterminado, o bien puedes seleccionar un controlador de eventos existente de la lista. 

Esta es la ventana IntelliSense que aparece. Ayuda a crear un nuevo controlador de eventos o a seleccionar un controlador de eventos existente.

![IntelliSense para el evento Click](images/add-controls-add-event-xaml.png)

Este ejemplo muestra cómo asociar un evento Click con un controlador de eventos llamado Button_Click en XAML. 

```xaml
<Button Name="Button1" Content="Button" Click="Button_Click"/>
```

También puedes asociar un evento con su controlador de eventos en el código subyacente. Así es como se asocia un controlador de eventos en el código.

```csharp
Button1.Click += new RoutedEventHandler(Button_Click);
```

## <a name="related-topics"></a>Temas relacionados

-   [Índice de controles por función](controls-by-function.md)
-   [Espacio de nombre de Windows.UI.Xaml.Controls](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.aspx)
-   [Diseño](../layout/index.md)
-   [Estilo](../style/index.md)
-   [Facilidad de uso](../usability/index.md)
