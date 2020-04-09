---
Description: El movimiento panorámico y el desplazamiento permiten a los usuarios acceder a contenido que va más allá de los límites de la pantalla.
title: Controles del visor de desplazamiento
ms.assetid: 1BFF0E81-BF9C-43F7-95F6-EFC6BDD5EC31
label: Scrollbars
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: Abarlow, pagildea
design-contact: ksulliv
dev-contact: regisb
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: a2123c8baa93356a0bb5adcfb2a32ac71af173cc
ms.sourcegitcommit: af4050f69168c15b0afaaa8eea66a5ee38b88fed
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/21/2020
ms.locfileid: "80081582"
---
# <a name="scroll-viewer-controls"></a>Controles del visor de desplazamiento



Cuando la interfaz de usuario tenga más contenido que el que se pueda mostrar en un área, usa el control del visor de desplazamiento.

> **API importantes**: [Clase ScrollViewer](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer), [Clase ScrollBar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.scrollbar)

Los visores de desplazamiento permiten que el contenido se extienda más allá de los límites de la ventanilla (área visible). Para acceder a dicho contenido, los usuario manipulan la superficie del visor de desplazamiento mediante entrada táctil, la rueda del mouse, el teclado o un controlador para juegos, o bien mediante el uso del cursor del ratón o del lápiz óptico para interactuar con la barra de desplazamiento del visor de desplazamiento. Esta imagen muestra varios ejemplos de controles del visor de desplazamiento.

![Captura de pantalla que ilustra el control de barra de desplazamiento estándar](images/ScrollBar_Standard.jpg)

En función de la situación, la barra de desplazamiento del visor de desplazamiento usa dos visualizaciones diferentes, que se muestran en la siguiente ilustración: el indicador de movimiento panorámico (izquierda) y la barra de desplazamiento tradicional (derecha).

![Un ejemplo de la apariencia de la barra de desplazamiento y el indicador de movimiento panorámico estándar](images/SCROLLBAR.png)

El visor de desplazamiento es consciente del método de entrada del usuario y lo usa para determinar la visualización que se va a usar.

* Cuando la región se desplaza sin manipular directamente la barra de desplazamiento, por ejemplo de manera táctil, aparece el indicador de movimiento panorámico y muestra la posición de desplazamiento actual.
* Cuando se mueve el cursor del ratón o del lápiz sobre el indicador de movimiento panorámico, se transforma en la barra de desplazamiento tradicional.  Al arrastrar la barra de desplazamiento, la región de desplazamiento se manipula con el pulgar.

<!--
<div class="microsoft-internal-note">
See complete redlines in [UNI]http://uni/DesignDepot.FrontEnd/#/ProductNav/3378/0/dv/?t=Windows|Controls|ScrollControls&f=RS2
</div>
-->

![Barras de desplazamiento en acción](images/conscious-scroll.gif)

> [!NOTE]
> Cuando la barra de desplazamiento está visible, se superpone como 16 píxeles al contenido dentro del ScrollViewer. Para garantizar un buen diseño de la experiencia del usuario, querrás asegurarte de que ningún contenido interactivo queda oculto por esta superposición. Además, si prefieres que no se superponga la experiencia del usuario, deja 16 píxeles de espaciado interno en el borde de la ventanilla para la barra de desplazamiento.

## <a name="examples"></a>Ejemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">Galería de controles XAML</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/ScrollViewer">abrirla y ver ScrollViewer en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-scroll-viewer"></a>Crear un visor de desplazamiento

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

Este XAML muestra cómo habilitar el desplazamiento horizontal, colocar una imagen en un visor de desplazamiento y habilitar el zoom.

```xaml
<ScrollViewer ZoomMode="Enabled" MaxZoomFactor="10"
              HorizontalScrollMode="Enabled" HorizontalScrollBarVisibility="Visible"
              Height="200" Width="200">
    <Image Source="Assets/Logo.png" Height="400" Width="400"/>
</ScrollViewer>
```

## <a name="scrollviewer-in-a-control-template"></a>ScrollViewer en una plantilla de control

Es habitual que exista un control ScrollViewer como elemento compuesto de otros controles. Un elemento ScrollViewer, junto con la clase [ScrollContentPresenter](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollContentPresenter) para compatibilidad, mostrará una ventanilla junto con las barras de desplazamiento solo cuando el espacio de diseño del control de host tenga una limitación menor que el tamaño del contenido expandido. Esto suele suceder con las listas, por lo que las plantillas [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) y [GridView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) siempre incluyen un ScrollViewer. [TextBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) y [RichEditBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichEditBox) también incluyen un ScrollViewer en sus plantillas.

Cuando un elemento **ScrollViewer** existe en un control, el control de host a menudo tiene el control de eventos integrado para ciertos eventos de entrada y manipulaciones que permiten que el contenido se desplace. Por ejemplo, un GridView interpreta un gesto de deslizamiento del dedo y esto hace que el contenido se desplace horizontalmente. Se considera que tanto de los eventos de entrada como de las manipulaciones sin procesar que recibe el control de host se encarga el control y los eventos de nivel inferior, como [PointerPressed](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed), no subirán de nivel ni se propagarán a los contenedores principales. Puedes cambiar parte de la administración de los controles integrados reemplazando una clase de control y los métodos virtuales **On**_Event_ por eventos o volver a crear plantillas del control. Pero, en cualquier caso, reproducir el comportamiento predeterminado original no es algo trivial, que suele estar allí para que el control reaccione de formas esperadas a los eventos y a las acciones de entrada y los gestos de un usuario. Por lo tanto, debes considerar si realmente necesitas que ese evento de entrada se active. Es posible que desee investigar si hay otros eventos de entrada o gestos que no administra el control y usarlos en el diseño de interacción de control o de la aplicación.

Para que los controles que incluyen un ScrollViewer puedan influir en algunos de los comportamientos y las propiedades que están dentro del elemento ScrollViewer, ScrollViewer define un número de propiedades adjuntas de XAML que pueden establecerse en los estilos y usarse en enlaces de plantillas. Para obtener más información sobre las propiedades adjuntas, consulta [Introducción a las propiedades adjuntas](../../xaml-platform/attached-properties-overview.md).

**Propiedades adjuntas de XAML de ScrollViewer**

ScrollViewer define las siguientes propiedades adjuntas de XAML:

- [ScrollViewer.BringIntoViewOnFocusChange](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.bringintoviewonfocuschange)
- [ScrollViewer.HorizontalScrollBarVisibility](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.horizontalscrollbarvisibility)
- [ScrollViewer.HorizontalScrollMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.horizontalscrollmode)
- [ScrollViewer.IsDeferredScrollingEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.isdeferredscrollingenabled)
- [ScrollViewer.IsHorizontalRailEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.ishorizontalrailenabled)
- [ScrollViewer.IsHorizontalScrollChainingEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.ishorizontalscrollchainingenabled)
- [ScrollViewer.IsScrollInertiaEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.isscrollinertiaenabled)
- [ScrollViewer.IsVerticalRailEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.isverticalrailenabled)
- [ScrollViewer.IsVerticalScrollChainingEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.isverticalscrollchainingenabled)
- [ScrollViewer.IsZoomChainingEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.iszoominertiaenabled)
- [ScrollViewer.IsZoomInertiaEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.iszoominertiaenabled)
- [ScrollViewer.VerticalScrollBarVisibility](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.verticalscrollbarvisibilityproperty)
- [ScrollViewer.VerticalScrollMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.verticalscrollmode)
- [ScrollViewer.ZoomMode](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.scrollviewer.zoommode)

Estas propiedades adjuntas de XAML están destinadas para los casos donde ScrollViewer es implícito, como cuando ScrollViewer existe en la plantilla predeterminada para un ListView o GridView y quieres poder influir en el comportamiento de desplazamiento del control sin tener acceso a elementos de plantilla.

Por ejemplo, así que como se hace que las barras de desplazamiento vertical estén siempre visibles en un visor de desplazamiento integrado de ListView.

```xaml
<ListView ScrollViewer.VerticalScrollBarVisibility="Visible"/>
```

En los casos donde un ScrollViewer es explícito en el XAML, como se muestra en el código de ejemplo, no necesitas usar una sintaxis de propiedad adjunta. Solo tienes que usar la sintaxis de atributo, por ejemplo `<ScrollViewer VerticalScrollBarVisibility="Visible"/>`.


## <a name="dos-and-donts"></a>Cosas que hacer y cosas que evitar

- Siempre que sea posible, diseña para el desplazamiento vertical en lugar del horizontal.
- Usa movimiento panorámico en un eje para las áreas de contenido que se extienden más allá de un límite de la ventanilla (vertical u horizontal). Usa movimiento panorámico en dos ejes para las áreas de contenido que se extienden más allá de ambos límites de la ventanilla (vertical y horizontal).
- Usa la funcionalidad de desplazamiento integrada de los siguientes elementos: vista de lista, vista de cuadrícula, cuadro combinado, cuadro de texto, cuadro de entrada de texto y controles de navegación centralizada. Con estos controles, si hay demasiados elementos que mostrar al mismo tiempo, el usuario puede desplazarse horizontal o verticalmente por la lista de elementos.
- Si quieres que el usuario pueda generar una vista panorámica en ambas direcciones sobre un área mayor, y quizá también aplicar zoom también, por ejemplo, si quieres que el usuario pueda generar una vista panorámica y aplicar zoom sobre una imagen a tamaño completo (en lugar de una imagen con tamaño ajustado a la pantalla), coloca la imagen dentro de un visor de desplazamiento.
- Si el usuario va a desplazarse sobre un fragmento de texto largo, configura el visor para un desplazamiento solamente vertical.
- El visor de desplazamiento que uses debe contener un solo objeto. Ten en cuenta que ese objeto puede ser un panel de diseño que contenga a su vez cualquier número de objetos.
- No coloques un control [dinámico](pivot.md) dentro de un visor de desplazamiento para evitar conflictos con la lógica de desplazamiento del control dinámico.

## <a name="get-the-sample-code"></a>Obtención del código de ejemplo

- [Muestra de XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery): Vea todos los controles XAML en un formato interactivo.

## <a name="related-topics"></a>Temas relacionados

**Para desarrolladores (XAML)**

* [Clase ScrollViewer](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer)
