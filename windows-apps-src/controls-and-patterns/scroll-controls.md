---
author: Jwmsft
Description: "El movimiento panorámico y el desplazamiento permiten a los usuarios acceder a contenido que va más allá de los límites de la pantalla."
title: Directrices para barras de desplazamiento
ms.assetid: 1BFF0E81-BF9C-43F7-95F6-EFC6BDD5EC31
label: Scroll bars
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: eb6744968a4bf06a3766c45b73b428ad690edc06
ms.openlocfilehash: 3dd5912bdd210751257bb9e495c5a95ce0be20a5

---
# Barras de desplazamiento

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

El movimiento panorámico y el desplazamiento permiten a los usuarios acceder a contenido que va más allá de los límites de la pantalla.

Un control de visor de desplazamiento está compuesto por tanto contenido como quepa en la ventanilla y una o dos barras de desplazamiento. Se pueden usar gestos táctiles para acceder a una vista de movimiento panorámico y zoom (las barras de desplazamiento solo aparecen en fundido mientras se manipulan) y el puntero puede usarse para los desplazamientos. El gesto de toque genera un movimiento panorámico con inercia.

**Nota**  Windows tiene dos vistas para la cuadrícula de desplazamiento, que se basan en el modo de entrada del usuario: indicadores de la cuadrícula de desplazamiento cuando se usa la función táctil o el controlador para juegos como el mouse, teclado y lápiz.

![Muestra del aspecto de los controles de barra de desplazamiento e indicador de movimiento panorámico estándar](images/SCROLLBAR.png)


<div class="important-apis" >
<b>API importantes</b><br/>
<ul>
<li><a href="https://msdn.microsoft.com/library/windows/apps/br209527"><strong>Clase ScrollViewer</strong></a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.scrollbar.aspx"><strong>Clase ScrollBar</strong></a></li>
</ul>

</div>
</div>






## Ejemplos

Un [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.aspx) permite mostrar el contenido en un área más pequeña que su tamaño real. Cuando el contenido del visor de desplazamiento no está visible por completo, el visor de desplazamiento muestra barras de desplazamiento que el usuario puede usar para mover el área de contenido que está visible. El área que incluye todo el contenido del visor de desplazamiento es el *tamaño*. El área visible del contenido es la *ventanilla*.

![Captura de pantalla que ilustra el control de barra de desplazamiento estándar](images/ScrollBar_Standard.jpg)

## Crear un visor de desplazamiento
Para agregar el desplazamiento vertical a la página, ajusta el contenido de la página en un visor de desplazamiento.

```xaml
<Page
    x:Class="App1.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:App1">

    <ScrollViewer>
        <StackPanel>
            <TextBlock Text="My Page Title" Style="{StaticResource TitleTextBlockStyle}"/>
            <!-- more page content -->
        </StackPanel>
    </ScrollViewer>
</Page>
```
Este XAML muestra cómo colocar una imagen en un visor de desplazamiento y habilitar el zoom.

```xaml
<ScrollViewer ZoomMode="Enabled" MaxZoomFactor="10"
              HorizontalScrollMode="Enabled" HorizontalScrollBarVisibility="Visible"
              Height="200" Width="200">
    <Image Source="Assets/Logo.png" Height="400" Width="400"/>
</ScrollViewer>
```

## ScrollViewer en una plantilla de control

Es habitual que exista un control ScrollViewer como elemento compuesto de otros controles. Un elemento ScrollViewer, junto con la clase [**ScrollContentPresenter**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollcontentpresenter.aspx) para compatibilidad, mostrará una ventanilla junto con las barras de desplazamiento solo cuando el espacio de diseño del control de host tenga una limitación menor que el tamaño del contenido expandido. Esto suele ser así en el caso de las listas, por lo que las plantillas [**ListView**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.listview.aspx) y [**GridView**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.gridview.aspx) siempre incluyen un ScrollViewer. [**TextBox**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.textbox.aspx) y [**RichEditBox**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.richeditbox.aspx) también incluyen un ScrollViewer en sus plantillas.

Cuando un elemento **ScrollViewer** existe en un control, el control de host a menudo tiene el control de eventos integrado para ciertos eventos de entrada y manipulaciones que permiten que el contenido se desplace. Por ejemplo, un GridView interpreta un gesto de deslizamiento del dedo y esto hace que el contenido se desplace horizontalmente. Los eventos de entrada y manipulaciones sin procesar que recibe el control de host se consideran administradas por el control, y los eventos de nivel inferior, como [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.pointerpressed.aspx), no subirán de nivel y tampoco se propagarán a los contenedores principales. Puedes cambiar parte de la administración de los controles integrados reemplazando una clase de control y los métodos virtuales **On***por eventos o volver a crear plantillas del control. Pero, en cualquier caso, reproducir el comportamiento predeterminado original no es algo trivial, que suele estar allí para que el control reaccione de formas esperadas a los eventos y a las acciones de entrada y los gestos de un usuario. Por lo tanto, debes considerar si realmente necesitas que ese evento de entrada se active. Es posible que desee investigar si hay otros eventos de entrada o gestos que no administra el control y usarlos en el diseño de interacción de control o de la aplicación.

Para que los controles que incluyen un ScrollViewer puedan influir en algunos de los comportamientos y las propiedades que están dentro del elemento ScrollViewer, ScrollViewer define un número de propiedades adjuntas de XAML que pueden establecerse en los estilos y usarse en enlaces de plantillas. Para obtener más información sobre las propiedades adjuntas, consulta [Attached properties overview](../xaml-platform/attached-properties-overview.md) (Introducción a las propiedades adjuntas).

**Propiedades adjuntas de XAML de ScrollViewer**

ScrollViewer define las siguientes propiedades adjuntas de XAML:
- [ScrollViewer.BringIntoViewOnFocusChange](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.bringintoviewonfocuschange.aspx)
- [ScrollViewer.HorizontalScrollBarVisibility](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.horizontalscrollbarvisibility.aspx)
- [ScrollViewer.HorizontalScrollMode](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.horizontalscrollmode.aspx)
- [ScrollViewer.IsDeferredScrollingEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.isdeferredscrollingenabled.aspx)
- [ScrollViewer.IsHorizontalRailEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.ishorizontalrailenabled.aspx)
- [ScrollViewer.IsHorizontalScrollChainingEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.ishorizontalscrollchainingenabled.aspx)
- [ScrollViewer.IsScrollInertiaEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.isscrollinertiaenabled.aspx)
- [ScrollViewer.IsVerticalRailEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.isverticalrailenabled.aspx)
- [ScrollViewer.IsVerticalScrollChainingEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.isverticalscrollchainingenabled.aspx)
- [ScrollViewer.IsZoomChainingEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.iszoominertiaenabled.aspx)
- [ScrollViewer.IsZoomInertiaEnabled](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.iszoominertiaenabled.aspx)
- [ScrollViewer.VerticalScrollBarVisibility](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.verticalscrollbarvisibilityproperty.aspx)
- [ScrollViewer.VerticalScrollMode](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.verticalscrollmode.aspx)
- [ScrollViewer.ZoomMode](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.scrollviewer.zoommode.aspx)

Estas propiedades adjuntas de XAML están destinadas para los casos donde ScrollViewer es implícito, como cuando ScrollViewer existe en la plantilla predeterminada para un ListView o GridView y quieres poder influir en el comportamiento de desplazamiento del control sin tener acceso a elementos de plantilla.

Por ejemplo, aquí te mostramos cómo hacer que las barras de desplazamiento vertical estén siempre visibles para un visor de desplazamiento integrado de ListView.
```xaml
<ListView ScrollViewer.VerticalScrollBarVisibility="Visible"/>
```

En los casos donde un ScrollViewer es explícito en el XAML, como se muestra en el código de ejemplo, no necesitas usar una sintaxis de propiedad adjunta. Solo tienes que usar la sintaxis de atributo, por ejemplo `<ScrollViewer VerticalScrollBarVisibility="Visible"/>`.


## Recomendaciones

-   Siempre que sea posible, diseña para el desplazamiento vertical en lugar del horizontal.
-   Usa movimiento panorámico en un eje para las áreas de contenido que se extienden más allá de un límite de la ventanilla (vertical u horizontal). Usa movimiento panorámico en dos ejes para las áreas de contenido que se extienden más allá de ambos límites de la ventanilla (vertical y horizontal).
-   Usa la funcionalidad de desplazamiento integrada de los siguientes elementos: vista de lista, vista de cuadrícula, cuadro combinado, cuadro de texto, cuadro de entrada de texto y controles de navegación centralizada. Con estos controles, si hay demasiados elementos que mostrar al mismo tiempo, el usuario puede desplazarse horizontal o verticalmente por la lista de elementos.
-   Si quieres que el usuario pueda generar una vista panorámica en ambas direcciones sobre un área mayor, y quizá también aplicar zoom también, por ejemplo, si quieres que el usuario pueda generar una vista panorámica y aplicar zoom sobre una imagen a tamaño completo (en lugar de una imagen con tamaño ajustado a la pantalla), coloca la imagen dentro de un visor de desplazamiento.
-   Si el usuario va a desplazarse sobre un fragmento de texto largo, configura el visor para un desplazamiento solamente vertical.
-   El visor de desplazamiento que uses debe contener un solo objeto. Ten en cuenta que ese objeto puede ser un panel de diseño que contenga a su vez cualquier número de objetos.
-   No coloques un control [dinámico](tabs-pivot.md) dentro de un visor de desplazamiento para evitar conflictos con la lógica de desplazamiento del control dinámico.

## Temas relacionados

**Para desarrolladores (XAML)**
* [**Clase ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/br209527)



<!--HONumber=Aug16_HO3-->


