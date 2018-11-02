---
author: stevewhims
title: Introducción a la navegación
description: Introducción a la navegación
ms.assetid: F4DF5C5F-C886-4483-BBDA-498C4E2C1BAF
ms.author: stwhi
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9cb4550a7da3b9b547a1d723d5ae8da260149ba2
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2018
ms.locfileid: "5940169"
---
# <a name="getting-started-navigation"></a>Introducción: navegación


## <a name="adding-navigation"></a>Agregar navegación

iOS ofrece la clase **UINavigationController** para ayudar con la navegación en la aplicación: puedes insertar y extraer vistas para crear la jerarquía de elementos **UIViewController** que definen la aplicación.

En cambio, una aplicación de Windows 10 que contiene varias vistas requiere más que un enfoque de sitio web para la navegación. Puedes esperar que los usuarios pasen de una página a otra conforme hacen clic en controles para abrirse camino por la aplicación. Para obtener información, consulta [Conceptos básicos de diseño de la navegación](https://msdn.microsoft.com/library/windows/apps/dn958438).

Una de las formas de administrar esta navegación en una aplicación de Windows 10 es usar la clase de [**marco**](https://msdn.microsoft.com/library/windows/apps/br242682) . En el tutorial siguiente se muestra cómo puedes probarlo.

Para seguir con la solución que iniciaste anteriormente, abre el archivo **MainPage.xaml** y agrega un botón en la vista **Diseño**. Cambia la propiedad **Content** del botón de "Botón" a "Ir a la página". A continuación, crea un controlador para el evento **Click** del botón, como se muestra en la siguiente ilustración. Si no recuerdas cómo hacerlo, revisa el tutorial de la sección anterior (sugerencia: haz doble clic en el botón de la vista **Diseño**).

![agregar un botón y su evento click en visual studio](images/ios-to-uwp/vs-go-to-page.png)

Vamos a agregar una página nueva. En la vista **Solución**, pulsa el menú **Proyecto** y **Agregar nuevo elemento**. Pulsa **Página en blanco** como se muestra en la ilustración siguiente y después pulsa **Agregar**.

![agregar una nueva página a visual studio](images/ios-to-uwp/vs-add-new-page.png)

A continuación, agrega un botón al archivo BlankPage.xaml. Se usará el control AppBarButton y le asignaremos una imagen de flecha atrás: en la vista **XAML**, agrega ` <AppBarButton Icon="Back"/>` entre los elementos `<Grid> </Grid>`.

Ahora, vamos a agregar un controlador de eventos al botón: haz doble clic en el control en la vista **Diseño**. Microsoft Visual Studio agrega el texto "AppBarButton\_Click" al cuadro **Click**, como se aprecia en la siguiente figura, y luego agrega y muestra el controlador de eventos correspondiente en el archivo BlankPage.xaml.cs.

![agregar un botón atrás y su evento click en visual studio](images/ios-to-uwp/vs-add-back-button.png)

Si vuelves a la vista **XAML** del archivo BlankPage.xaml, el código de lenguajeXAML del elemento `<AppBarButton>` debería parecerse a lo siguiente:

` <AppBarButton Icon="Back" Click="AppBarButton_Click"/>`

Vuelve al archivo BlankPage.xaml.cs y agrega este código para ir a la página anterior después de que el usuario pulse el botón.

```csharp
private void AppBarButton_Click(object sender, RoutedEventArgs e)
{
    // Add the following line of code.    
    Frame.GoBack();
}
```

Por último, abre el archivo MainPage.xaml.cs y agrega este código. Abre BlankPage después de que el usuario pulse el botón.

```csharp
private void Button_Click(object sender, RoutedEventArgs e)
{
    // Add the following line of code.
    Frame.Navigate(typeof(BlankPage1));
}
```

Ahora, ejecuta el programa. Pulsa el botón "Ir a la página" para ir a la otra página y después pulsa el botón de flecha atrás para volver a la página anterior.

La clase [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) administra la navegación de páginas. Como la clase **UINavigationController** en iOS usa métodos **pushViewController** y **popViewController** , la clase de **marco** para aplicaciones para UWP proporciona métodos [**Navigate**](https://msdn.microsoft.com/library/windows/apps/br242694) y [**GoBack**](https://msdn.microsoft.com/library/windows/apps/dn996568) . La clase **Frame** también tiene un método denominado [**GoForward**](https://msdn.microsoft.com/library/windows/apps/br242693), que hace lo que su nombre en inglés (ir adelante) sugiere.

En este tutorial se crea una nueva instancia de BlankPage cada vez que el usuario navega a ella. (Se *liberará* la instancia anterior automáticamente). Si no quieres que se cree una nueva instancia cada vez, agrega este código al constructor de la clase de BlankPage en el archivo BlankPage.xaml.cs. Así se habilitará el comportamiento [**NavigationCacheMode**](https://msdn.microsoft.com/library/windows/apps/br227506).

```csharp
public BlankPage()
{
    this.InitializeComponent();
    // Add the following line of code.
    this.NavigationCacheMode = Windows.UI.Xaml.Navigation.NavigationCacheMode.Enabled;
}
```

También puedes obtener o establecer la propiedad [**CacheSize**](https://msdn.microsoft.com/library/windows/apps/br242683) de la clase **Frame** para administrar el número de páginas del historial de navegación que se pueden almacenar en caché.

Para más información sobre la navegación, consulta [Navegación](https://msdn.microsoft.com/library/windows/apps/mt187344) y [Ejemplo de animaciones de personalidad XAML](http://go.microsoft.com/fwlink/p/?LinkID=242401).

**Nota**para obtener información sobre la navegación para aplicaciones para UWP con JavaScript y HTML, consulta [Inicio rápido: usar la navegación de página](https://msdn.microsoft.com/library/windows/apps/hh452768).
 
### <a name="next-step"></a>Paso siguiente

[Introducción: Animación](getting-started-animation.md)

