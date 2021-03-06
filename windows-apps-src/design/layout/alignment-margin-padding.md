---
description: Usar propiedades de alineación, margen y espaciado interno para organizar el diseño de elementos en una página.
title: Alineación, margen y espaciado interno para el diseño
ms.date: 03/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: f2782118b2ed35578ac48f2996839ceefcf5b71b
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034898"
---
# <a name="alignment-margin-padding"></a>Alineación, margen, espaciado interno

En aplicaciones para UWP, se heredan la mayoría de los elementos de la interfaz de usuario desde la clase [**FrameworkElement**](/uwp/api/Windows.UI.Xaml.FrameworkElement). Cada FrameworkElement tiene dimensiones y propiedades de alineación, margen y espaciado interno que influyen en el comportamiento del diseño. En la siguiente guía se proporciona una introducción de cómo usar estas propiedades de diseño para asegurarte de que la interfaz de usuario de tu aplicación sea legible y fácil de usar en cualquier contexto.

## <a name="dimensions-height-width"></a>Dimensiones (alto y ancho)
Un tamaño correcto garantiza que todo el contenido sea claro y legible. Los usuarios no deberían desplazarse ni aplicar zoom para descifrar contenido principal.

![diagrama que muestra las dimensiones](images/dimensions.svg)

- [**Height**](/uwp/api/windows.ui.xaml.frameworkelement.height) y [**Width**](/uwp/api/windows.ui.xaml.frameworkelement.width) especifican el tamaño de un elemento. Los valores predeterminados son, matemáticamente hablando, NaN (no es un número). Puedes ajustar valores fijos medidos en [píxeles efectivos](../basics/design-and-ui-intro.md#effective-pixels-and-scaling), o bien puedes usar **Auto** o la [variación de tamaño proporcional](layout-panels.md#grid) para un comportamiento fluido.

- [**ActualHeight**](/uwp/api/windows.ui.xaml.frameworkelement.actualheight) y [**ActualWidth**](/uwp/api/windows.ui.xaml.frameworkelement.actualwidth) son propiedades de solo lectura que proporcionan el tamaño de un elemento en tiempo de ejecución. Si los diseños fluidos se amplían o se reducen, los valores cambian en un evento [**SizeChanged**](/uwp/api/windows.ui.xaml.frameworkelement.sizechanged). Ten en cuenta que un control [**RenderTransform**](/uwp/api/windows.ui.xaml.uielement.rendertransform) no cambiará los valores ActualHeight y ActualWidth.

- [**MinWidth**](/uwp/api/windows.ui.xaml.frameworkelement.minwidth)/[**MaxWidth**](/uwp/api/windows.ui.xaml.frameworkelement.maxwidth) y [**MinHeight**](/uwp/api/windows.ui.xaml.frameworkelement.minheight)/[**MaxHeight**](/uwp/api/windows.ui.xaml.frameworkelement.maxheight) especifican los valores que limitan el tamaño de un elemento al tiempo que permiten un cambio de tamaño fluido.

- [**FontSize**](/uwp/api/windows.ui.xaml.controls.textblock.fontsize) y otras propiedades de texto controlan el tamaño del diseño para los elementos de texto. Aunque los elementos de texto no tienen dimensiones declaradas de manera explícita, sí que han calculado ActualWidth y ActualHeight. 

## <a name="alignment"></a>Asociación
La alineación hace que la apariencia de tu interfaz de usuario sea más interesante, organizada y equilibrada, y también puede usarse para establecer una jerarquía visual y relaciones.

![diagrama que muestra la alineación](images/alignment.svg)

- [**HorizontalAlignment**](/uwp/api/windows.ui.xaml.frameworkelement.horizontalalignment) y [**VerticalAlignment**](/uwp/api/windows.ui.xaml.frameworkelement.verticalalignment) especifican cómo debe colocarse un elemento en su contenedor principal.
    - Los valores de **HorizontalAlignment** son **Left** , **Center** , **Right** y **Stretch**.
    - Los valores de **VerticalAlignment** son **Top** , **Center** , **Bottom** y **Stretch**.

- **Stretch** es el valor predeterminado para ambas propiedades, y los elementos ocupan todo el espacio que se les proporciona en el contenedor principal. Los valores de Height y Width con un número real anulan un valor Stretch, que en cambio actuará como un valor Center. Algunos controles, como Button, invalidan el valor predeterminado Stretch en su estilo predeterminado.

- [**HorizontalContentAlignment**](/uwp/api/windows.ui.xaml.controls.control.horizontalcontentalignment) y [**VerticalContentAlignment**](/uwp/api/windows.ui.xaml.controls.control.verticalcontentalignment) especifican cómo deben colocarse los elementos secundarios en un contenedor.

- La alineación puede afectar a los recortes en un panel de diseño. Por ejemplo, con `HorizontalAlignment="Left"`, el lado derecho del elemento se recorta si el contenido es mayor que ActualWidth.

- Los elementos de texto usan la propiedad [**TextAlignment**](/uwp/api/windows.ui.xaml.textalignment). Por lo general, te recomendamos que uses la alineación a la izquierda, el valor predeterminado. Para obtener más información sobre cómo aplicar estilo al texto, consulta [Tipografía](../style/typography.md).

## <a name="margin-and-padding"></a>Margen y espaciado
Las propiedades de margen y espaciado interno evitan que la interfaz de usuario parezca demasiado desordenada o demasiado dispersa, y también pueden conseguir que algunas entradas como el lápiz y la función táctil sean más fáciles de usar. Esta ilustración muestra los márgenes y el espaciado interno de un contenedor y su contenido.

![márgenes de XAML y diagrama de espaciado interno](images/xaml-layout-margins-padding.svg)

### <a name="margin"></a>Margen
[**Margin**](/uwp/api/windows.ui.xaml.frameworkelement.margin) controla la cantidad de espacio vacío alrededor de un elemento. El margen no agrega píxeles a ActualHeight y ActualWidth ni tampoco se considera como parte del elemento para las pruebas de acceso y el uso de eventos de entrada.

- Los valores del margen pueden ser uniformes o diferentes. Con `Margin="20"`, un margen uniforme de 20 píxeles se aplicaría al elemento a la izquierda, parte superior, derecha y parte inferior. Con `Margin="0,10,5,25"`, los valores se aplicarían a la izquierda, parte superior, derecha y parte inferior (en este orden). 

- Los márgenes se suman. Si tenemos dos elementos y cada uno especifica un margen uniforme de 10 píxeles y son elementos adyacentes del mismo nivel en cualquier orientación, la distancia entre los elementos es de 20 píxeles.

- Se permiten los márgenes negativos. No obstante, a menudo el uso de un margen negativo puede provocar recortes o que se sobredibujen los elementos del mismo nivel. Por tanto, usar márgenes negativos no es una técnica habitual.

- Los valores de margen son los últimos en restringirse, por lo tanto, debes tener cuidado con los márgenes porque los contenedores pueden recortar o delimitar elementos. Un valor de margen podría ser la causa de que no aparezca un elemento para representar; con un margen aplicado, la dimensión de un elemento puede restringirse a 0.

### <a name="padding"></a>Espaciado interno
[**Padding**](/uwp/api/windows.ui.xaml.frameworkelement.padding) controla la cantidad de espacio entre el borde interno de un elemento y su contenido o elementos secundarios. Un valor Padding positivo disminuye el área de contenido del elemento. 

A diferencia del margen, el espaciado interno no es una propiedad de FrameworkElement. Existen varias clases que definen su propia propiedad de espaciado interno:

-   [**Control.Padding**](/uwp/api/windows.ui.xaml.controls.control.padding): hereda en todas las clases derivadas de [**Control**](/uwp/api/windows.ui.xaml.controls). No todos los controles tienen contenido, de modo que, para algunos controles, establecer la propiedad no hace nada. Si el control tiene un borde, el espaciado interno se aplica dentro de dicho borde.
-   [**Border.Padding**](/uwp/api/windows.ui.xaml.controls.border.padding): define el espacio entre la línea de rectángulo creada por los objetos [**BorderThickness**](/uwp/api/windows.ui.xaml.controls.border.borderthickness)/[**BorderBrush**](/uwp/api/windows.ui.xaml.controls.border.borderbrush) y el elemento [**Child**](/uwp/api/windows.ui.xaml.controls.border.child).
-   [**ItemsPresenter.Padding**](/uwp/api/windows.ui.xaml.controls.itemspresenter.padding): contribuye a la apariencia de los efectos visuales generados para los elementos de los controles de elemento al colocar el espaciado interno especificado alrededor de cada elemento.
-   [**TextBlock.Padding**](/uwp/api/windows.ui.xaml.controls.textblock.padding) y [**RichTextBlock.Padding**](/uwp/api/windows.ui.xaml.controls.richtextblock.padding): amplían el rectángulo de selección alrededor del texto del elemento de texto. Estos elementos de texto no tienen ningún **Background** , por lo que puede resultar difícil verlos. Por ello, usa la configuración [**Margin**](/uwp/api/windows.ui.xaml.documents.block.margin) en contenedores [**Block**](/uwp/api/windows.ui.xaml.documents.block) en su lugar.

En cada uno de estos casos, los elementos también tienen una propiedad de margen. Si se aplica tanto el margen como el espaciado interno, se suman, es decir, la distancia aparente entre un contenedor externo y el contenido interno será el margen más el espaciado interno.

### <a name="example"></a>Ejemplo
Echemos un vistazo a los efectos de Margin y Padding en controles reales. Este es un TextBox dentro de una Grid con los valores predeterminados Margin y Padding establecidos en 0.

![Cuadro de texto con margen y espaciado de 0](images/xaml-layout-textbox-no-margins-padding.svg)

Este es el mismo TextBox y Grid con los valores Margin y Padding del TextBox, tal como se muestra en este código XAML.

```xaml
<Grid BorderBrush="Blue" BorderThickness="4" Width="200">
    <TextBox Text="This is text in a TextBox." Margin="20" Padding="16,24"/>
</Grid>
```

![Cuadro de texto con valores de margen y relleno positivos](images/xaml-layout-textbox-with-margins-padding.svg)


## <a name="style-resources"></a>Recursos de estilo
No hace falta establecer cada valor individualmente en un control. Suele ser más eficiente agrupar valores de propiedad en un recurso [**Style**](/uwp/api/Windows.UI.Xaml.Style) y aplicar el Style a un control. Esto es especialmente cierto cuando necesites aplicar los mismos valores de propiedad a muchos controles. Para más información sobre los estilos, consulta [Estilos XAML](../controls-and-patterns/xaml-styles.md).

## <a name="general-recommendations"></a>Recomendaciones generales
- Solo se aplican los valores de medida a ciertos elementos clave y se usa el comportamiento del diseño fluido para los demás elementos. Esto proporciona una [interfaz de usuario dinámica](responsive-design.md) cuando cambia el ancho de la ventana.

- Si usas los valores de medida, **todas las dimensiones, los márgenes y el espaciado interno deben aplicarse en incrementos de 4 epx**. Cuando la UWP usa [píxeles efectivos y ajustes de escala](../basics/design-and-ui-intro.md#effective-pixels-and-scaling) para que tu aplicación sea legible en todos los dispositivos y tamaños de pantalla, ajusta la escala de los elementos de interfaz de usuario en múltiplos de 4. Usar valores en incrementos de 4 ofrece la mejor representación mediante la alineación de píxeles completos.

- Para el ancho de ventana pequeña (menos de 640 píxeles), te recomendamos medianiles de 12 epx, y para el ancho de ventana más grande, te recomendamos medianiles de 24 epx.

![medianiles recomendados](images/12-gutter.svg)

## <a name="related-topics"></a>Temas relacionados
* [**FrameworkElement.Height**](/uwp/api/windows.ui.xaml.frameworkelement.height)
* [**FrameworkElement.Width**](/uwp/api/windows.ui.xaml.frameworkelement.width)
* [**FrameworkElement.HorizontalAlignment**](/uwp/api/windows.ui.xaml.frameworkelement.horizontalalignment)
* [**FrameworkElement.VerticalAlignment**](/uwp/api/windows.ui.xaml.frameworkelement.verticalalignment)
* [**FrameworkElement.Margin**](/uwp/api/windows.ui.xaml.frameworkelement.margin)
* [**Control.Padding**](/uwp/api/windows.ui.xaml.controls.control.padding)
