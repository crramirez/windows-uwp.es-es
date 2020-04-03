---
Description: Usa el control ParallaxView para agregar profundidad y movimiento a la aplicación.
title: Use Parallax para agregar profundidad y movimiento a la aplicación.
ms.assetid: ''
label: Parallax View
template: detail.hbs
ms.date: 08/09/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: abarlow
design-contact: conrwi
dev-contact: stpete
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: ac195916e76ad7b3f03adc39a293422d0d58f7a4
ms.sourcegitcommit: 8be8ed1ef4e496055193924cd8cea2038d2b1525
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/02/2020
ms.locfileid: "80614085"
---
# <a name="parallax"></a>Parallax

Parallax es un efecto visual donde elementos más cercanos al usuario se mueven más rápido que los elementos en el fondo. Parallax crea una sensación de profundidad, perspectiva y movimiento. En una aplicación para UWP, puedes usar el control ParallaxView para crear un efecto de paralaje.  

> **API de la biblioteca de interfaz de usuario de Windows:** [clase ParallaxView](/uwp/api/Microsoft.UI.Xaml.Controls.Parallaxview), [propiedad VerticalShift](/uwp/api/Microsoft.UI.Xaml.Controls.Parallaxview.VerticalShift), [propiedad HorizontalShift](/uwp/api/Microsoft.UI.Xaml.Controls.Parallaxview.HorizontalShift)
>
> **API de plataforma**: [clase ParallaxView](/uwp/api/Windows.UI.Xaml.Controls.Parallaxview), [propiedad VerticalShift](/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.VerticalShift), [propiedad HorizontalShift](/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.HorizontalShift)

## <a name="examples"></a>Ejemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">Galería de controles XAML</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/ParallaxView">abrir la aplicación y ver ParallaxView en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="parallax-and-the-fluent-design-system"></a>Parallax y Fluent Design System

 Fluent Design System te ayuda a crear interfaces de usuario modernas y claras que incorporan luz, profundidad, movimiento, materiales y escala. Parallax es un componente de Fluent Design System que agrega movimiento, profundidad y escala a tu aplicación. Para más información, consulta la [Introducción a Fluent Design para UWP](/windows/apps/fluent-design-system).

## <a name="how-it-works-in-a-user-interface"></a>Cómo funciona en una interfaz de usuario

En una interfaz de usuario, puedes crear un efecto de paralaje si mueves distintos objetos a diferentes velocidades cuando la interfaz de usuario se desplaza o realiza un movimiento panorámico. <!-- Parallax is an important tool in adding depth to applications along with other techniques like transition animations, perspective tilt, and layering. --> Para demostrar, echemos un vistazo a dos capas de contenido, una lista y una imagen de fondo.  La lista se coloca encima de la imagen de fondo, lo que ya proporciona la ilusión de que la lista puede estar más cerca del usuario.  Ahora, para lograr el efecto de Parallax, queremos que el objeto más cercano a nosotros viaje "más rápido" que el objeto que está más lejos.  Cuando el usuario desplaza la interfaz, la lista se mueve a una velocidad más rápida que la imagen de fondo, lo que crea la ilusión de profundidad.

 ![Ejemplo de efecto de paralaje con una lista y una imagen en segundo plano](images/_Parallax_v2.gif)

 
## <a name="using-the-parallaxview-control-to-create-a-parallax-effect"></a>Usar el control de ParallaxView para crear un efecto de paralaje

Para crear un efecto de paralaje, usa el control [ParallaxView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Parallaxview). Este control vincula la posición de desplazamiento de un elemento en primer plano, como una lista, a un elemento en el fondo, como una imagen. Al desplazarte por el elemento en primer plano, se anima el elemento en el fondo para crear un efecto de paralaje. 

Para usar el control ParallaxView, proporciona un elemento Source, un elemento de fondo y establece las propiedades [VerticalShift](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.VerticalShift) (para el desplazamiento vertical) o [HorizontalShift](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.HorizontalShift) (para el desplazamiento horizontal) en un valor mayor que cero. 
* La propiedad Source toma una referencia al elemento en primer plano. Para que se produzca el efecto de paralaje, el primer plano debe ser una clase [ScrollViewer](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) o un elemento que contenga una clase ScrollViewer, como una clase [ListView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listview) o [RichTextBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichEditBox). 

* Para establecer el elemento de fondo, agrega ese elemento como elemento secundario del control ParallaxView. El elemento de fondo puede ser cualquier [UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement), como una clase [Image](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image) o un panel que contenga elementos adicionales de la interfaz de usuario. 

Para crear un efecto de paralaje, ParallaxView tiene que estar detrás del elemento en primer plano. Los paneles [Grid](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid) y [Canvas](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.canvas) te permiten ubicar elementos en capas uno encima de otro, por lo que funcionan bien con el control ParallaxView.  

En este ejemplo se crea un efecto de paralaje para una lista:
 
```xaml
<Grid>
    <ParallaxView Source="{x:Bind ForegroundElement}" VerticalShift="50"> 
    
        <!-- Background element --> 
        <Image x:Name="BackgroundImage" Source="Assets/turntable.png"
               Stretch="UniformToFill"/>
    </ParallaxView>
    
    <!-- Foreground element -->
    <ListView x:Name="ForegroundElement">
       <x:String>Item 1</x:String> 
       <x:String>Item 2</x:String> 
       <x:String>Item 3</x:String> 
       <x:String>Item 4</x:String> 
       <x:String>Item 5</x:String>     
       <x:String>Item 6</x:String> 
       <x:String>Item 7</x:String> 
       <x:String>Item 8</x:String> 
       <x:String>Item 9</x:String> 
       <x:String>Item 10</x:String>     
       <x:String>Item 11</x:String> 
       <x:String>Item 13</x:String> 
       <x:String>Item 14</x:String> 
       <x:String>Item 15</x:String> 
       <x:String>Item 16</x:String>     
       <x:String>Item 17</x:String> 
       <x:String>Item 18</x:String> 
       <x:String>Item 19</x:String> 
       <x:String>Item 20</x:String> 
       <x:String>Item 21</x:String>        
    </ListView>
</Grid>
```    

ParallaxView ajusta automáticamente el tamaño de la imagen para que funcione con la operación de Parallax, por lo que no tiene que preocuparse de la imagen que se desplaza fuera de la vista.

## <a name="customizing-the-parallax-effect"></a>Personalizar el efecto de paralaje 

Las propiedades VerticalShift y HorizontalShift te permiten controlar grado del efecto de paralaje.

* La propiedad VerticalShift especifica cuánto queremos que se desplace verticalmente el fondo durante toda la operación de paralaje. Un valor de 0 significa que el fondo no se mueve en absoluto.
* La propiedad HorizontalShift especifica cuánto queremos que se desplace horizontalmente el fondo durante toda la operación de paralaje. Un valor de 0 significa que el fondo no se mueve en absoluto.

Los valores más grandes crean un efecto más impactante. 

Para obtener la lista completa de maneras de personalizar el paralaje, consulta la clase ParallaxView. 

## <a name="dos-and-donts"></a>Cosas que hacer y cosas que evitar

- Usa el efecto de paralaje en listas con una imagen de fondo.
- Considera usar el paralaje en ListViewItems cuando ListViewItems contenga una imagen.
- No lo use en todas partes, el uso excesivo puede reducir su impacto

## <a name="related-articles"></a>Artículos relacionados

- [Clase ParallaxView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Parallaxview) 
- [Fluent Design para UWP](/windows/apps/fluent-design-system)
- [Ciencia en el sistema: diseño y profundidad fluidas](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f)
