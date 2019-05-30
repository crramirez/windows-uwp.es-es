---
title: Introducción a la navegación
description: Introducción a la navegación
ms.assetid: F4DF5C5F-C886-4483-BBDA-498C4E2C1BAF
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: de9c5261afb7b76b2409599c9c1f88814d1dd6a1
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371779"
---
# <a name="getting-started-navigation"></a>Introducción: Navegación


## <a name="adding-navigation"></a>Agregar navegación

iOS ofrece la clase **UINavigationController** para ayudar con la navegación en la aplicación: puedes insertar y extraer vistas para crear la jerarquía de elementos **UIViewController** que definen la aplicación.

En cambio, una aplicación de Windows 10 que contiene varias vistas tarda más de un enfoque del sitio web a la navegación. Puedes esperar que los usuarios pasen de una página a otra conforme hacen clic en controles para abrirse camino por la aplicación. Para obtener información, consulta [Conceptos básicos de diseño de la navegación](https://docs.microsoft.com/windows/uwp/layout/navigation-basics).

Una de las formas para administrar esta navegación en una aplicación de Windows 10 consiste en usar el [ **marco** ](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) clase. En el tutorial siguiente se muestra cómo puedes probarlo.

Para seguir con la solución que iniciaste anteriormente, abre el archivo **MainPage.xaml** y agrega un botón en la vista **Diseño**. Cambia la propiedad **Content** del botón de "Botón" a "Ir a la página". A continuación, crea un controlador para el evento **Click** del botón, como se muestra en la siguiente ilustración. Si no recuerdas cómo hacerlo, revisa el tutorial de la sección anterior (sugerencia: haz doble clic en el botón de la vista **Diseño**).

![agregar un botón y su evento click en visual studio](images/ios-to-uwp/vs-go-to-page.png)

Vamos a agregar una página nueva. En la vista **Solución**, pulsa el menú **Proyecto** y **Agregar nuevo elemento**. Pulsa **Página en blanco** como se muestra en la ilustración siguiente y después pulsa **Agregar**.

![agregar una nueva página a visual studio](images/ios-to-uwp/vs-add-new-page.png)

A continuación, agrega un botón al archivo BlankPage.xaml. Se usará el control AppBarButton y le asignaremos una imagen de flecha atrás: en la vista **XAML**, agrega ` <AppBarButton Icon="Back"/>` entre los elementos `<Grid> </Grid>`.

Ahora, vamos a agregar un controlador de eventos al botón: haga doble clic en el control en el **diseño** vista y Microsoft Visual Studio agrega el texto "AppBarButton\_haga clic en" a la **haga clic en** cuadro, como se muestra en el Figura, siguiente y, a continuación, agrega y muestra el controlador de eventos correspondiente en el archivo BlankPage.xaml.cs.

![agregar un botón atrás y su evento click en visual studio](images/ios-to-uwp/vs-add-back-button.png)

Si vuelves a la vista **XAML** del archivo BlankPage.xaml, el código de lenguaje XAML del elemento `<AppBarButton>` debería parecerse a lo siguiente:

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

La clase [**Frame**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) administra la navegación de páginas. Como el **UINavigationController** clase usa iOS **pushViewController** y **popViewController** métodos, el **marco** clase Proporciona aplicaciones para UWP [ **Navigate** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.navigate) y [ **GoBack** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.goback) métodos. La clase **Frame** también tiene un método denominado [**GoForward**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.goforward), que hace lo que su nombre en inglés (ir adelante) sugiere.

En este tutorial se crea una nueva instancia de BlankPage cada vez que el usuario navega a ella. (Se *liberará* la instancia anterior automáticamente). Si no quieres que se cree una nueva instancia cada vez, agrega este código al constructor de la clase de BlankPage en el archivo BlankPage.xaml.cs. Así se habilitará el comportamiento [**NavigationCacheMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.navigationcachemode).

```csharp
public BlankPage()
{
    this.InitializeComponent();
    // Add the following line of code.
    this.NavigationCacheMode = Windows.UI.Xaml.Navigation.NavigationCacheMode.Enabled;
}
```

También puedes obtener o establecer la propiedad [**CacheSize**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame.cachesize) de la clase **Frame** para administrar el número de páginas del historial de navegación que se pueden almacenar en caché.

Para más información sobre la navegación, consulta [Navegación](https://docs.microsoft.com/windows/uwp/layout/navigation-basics) y [Ejemplo de animaciones de personalidad XAML](https://go.microsoft.com/fwlink/p/?LinkID=242401).

**Tenga en cuenta**  para obtener información sobre la navegación para aplicaciones UWP con JavaScript y HTML, consulte [inicio rápido: Usar la navegación de página única](https://docs.microsoft.com/previous-versions/windows/apps/hh452768(v=win.10)).
 
### <a name="next-step"></a>Paso siguiente

[Introducción: Animación](getting-started-animation.md)

