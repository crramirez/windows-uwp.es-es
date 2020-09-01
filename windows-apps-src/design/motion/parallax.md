---
title: Uso de parallax para agregar profundidad y movimiento a la aplicación
description: Obtenga información sobre cómo usar el control ParallaxView en una aplicación de UWP para crear un efecto visual en el que los elementos más cercanos al visor se muevan más rápido que los elementos en segundo plano.
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
ms.openlocfilehash: 5eac1b5d95dff4887258278f9ff700adaf663194
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175549"
---
# <a name="parallax"></a>Parallax

Parallax es un efecto visual en el que los elementos más cercanos al visor se mueven más rápido que los elementos en segundo plano. Parallax crea una sensación de profundidad, perspectiva y movimiento. En una aplicación de UWP, puede usar el control ParallaxView para crear un efecto de Parallax.  

> **API de la biblioteca de interfaz de usuario de Windows:** [clase ParallaxView](/uwp/api/Microsoft.UI.Xaml.Controls.Parallaxview), [propiedad VerticalShift](/uwp/api/Microsoft.UI.Xaml.Controls.Parallaxview.VerticalShift), [propiedad HorizontalShift](/uwp/api/Microsoft.UI.Xaml.Controls.Parallaxview.HorizontalShift)
>
> **API de plataforma**: [clase ParallaxView](/uwp/api/Windows.UI.Xaml.Controls.Parallaxview), [propiedad VerticalShift](/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.VerticalShift), [propiedad HorizontalShift](/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.HorizontalShift)

## <a name="examples"></a>Ejemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>Si tiene instalada la aplicación de <strong style="font-weight: semi-bold">Galería de controles de XAML</strong> , haga clic aquí para <a href="xamlcontrolsgallery:/item/ParallaxView">abrir la aplicación y ver el ParallaxView en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="parallax-and-the-fluent-design-system"></a>Parallax y el sistema de diseño fluida

 Fluent Design System te ayuda a crear interfaces de usuario modernas y claras que incorporan luz, profundidad, movimiento, materiales y escala. Parallax es un componente del sistema de diseño fluida que agrega movimiento, profundidad y escala a la aplicación. Para más información, consulta la [Introducción a Fluent Design](/windows/apps/fluent-design-system).

## <a name="how-it-works-in-a-user-interface"></a>Cómo funciona en una interfaz de usuario

En una interfaz de usuario, puede crear un efecto de Parallax moviendo los distintos objetos a velocidades diferentes cuando la interfaz de usuario se desplaza o se gira. <!-- Parallax is an important tool in adding depth to applications along with other techniques like transition animations, perspective tilt, and layering. --> Para demostrar, echemos un vistazo a dos capas de contenido, una lista y una imagen de fondo.  La lista se coloca en la parte superior de la imagen de fondo, que ya proporciona la ilusión de que la lista podría estar más cerca del visor.  Ahora, para lograr el efecto de Parallax, queremos que el objeto más cercano a nosotros viaje "más rápido" que el objeto que está más lejos.  A medida que el usuario se desplaza por la interfaz, la lista se mueve a una velocidad mayor que la imagen de fondo, lo que crea una ilusión de profundidad.

 ![Ejemplo de efecto de paralaje con una lista y una imagen en segundo plano](images/_Parallax_v2.gif)

 
## <a name="using-the-parallaxview-control-to-create-a-parallax-effect"></a>Usar el control ParallaxView para crear un efecto parallax

Para crear un efecto de Parallax, use el control [ParallaxView](/uwp/api/Windows.UI.Xaml.Controls.Parallaxview) . Este control vincula la posición de desplazamiento de un elemento de primer plano, como una lista, a un elemento de fondo, como una imagen. A medida que se desplaza por el elemento Foreground, anima el elemento Background para crear un efecto parallax. 

Para usar el control ParallaxView, se proporciona un elemento de origen, un elemento de fondo y se establecen las propiedades [VerticalShift](/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.VerticalShift) (para desplazamiento vertical) y/o [HorizontalShift](/uwp/api/Windows.UI.Xaml.Controls.Parallaxview.HorizontalShift) (para desplazamiento horizontal) en un valor mayor que cero. 
* La propiedad Source toma una referencia al elemento Foreground. Para que se produzca el efecto de Parallax, el primer plano debe ser un [ScrollViewer](/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) o un elemento que contenga un control ScrollViewer, como [ListView](/uwp/api/windows.ui.xaml.controls.listview) o [RichTextBox](/uwp/api/Windows.UI.Xaml.Controls.RichEditBox). 

* Para establecer el elemento Background, agregue ese elemento como elemento secundario del control ParallaxView. El elemento Background puede ser cualquier [UIElement](/uwp/api/windows.ui.xaml.uielement), como una [imagen](/uwp/api/Windows.UI.Xaml.Controls.Image) o un panel que contenga elementos de interfaz de usuario adicionales. 

Para crear un efecto de Parallax, el ParallaxView debe estar detrás del elemento de primer plano. Los paneles de [cuadrícula](/uwp/api/windows.ui.xaml.controls.grid) y de [lienzo](/uwp/api/windows.ui.xaml.controls.canvas) permiten disponer elementos en capas entre sí, por lo que funcionan bien con el control ParallaxView.  

En este ejemplo se crea un efecto de Parallax para una lista:
 
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

## <a name="customizing-the-parallax-effect"></a>Personalización del efecto parallax 

Las propiedades VerticalShift y HorizontalShift permiten controlar el grado del efecto parallax.

* La propiedad VerticalShift especifica hasta qué punto se desea que el fondo cambie verticalmente durante toda la operación de Parallax. Un valor de 0 significa que el fondo no se mueve en absoluto.
* La propiedad HorizontalShift especifica hasta qué punto se desea que el fondo cambie horizontalmente durante toda la operación de Parallax. Un valor de 0 significa que el fondo no se mueve en absoluto.

Los valores más grandes crean un efecto más drástico. 

Para obtener una lista completa de las formas de personalizar Parallax, vea la clase ParallaxView. 

## <a name="dos-and-donts"></a>Cosas que hacer y cosas que evitar

- Usar Parallax en listas con una imagen de fondo
- Considere la posibilidad de usar Parallax en controles ListviewItems cuando controles ListviewItems contiene una imagen
- No lo use en todas partes, el uso excesivo puede reducir su impacto

## <a name="related-articles"></a>Artículos relacionados

- [Clase ParallaxView](/uwp/api/Windows.UI.Xaml.Controls.Parallaxview) 
- [Fluent Design para UWP](/windows/apps/fluent-design-system)
- [Ciencia en el sistema: Fluent Design y profundidad](https://medium.com/microsoft-design/science-in-the-system-fluent-design-and-depth-fb6d0f23a53f)