---
author: muhsinking
Description: XAML gives you a flexible layout system to create a responsive UI.
title: Diseños con capacidad de respuesta con XAML
ms.author: mukin
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0b45196a83edf45a69f6b79ab82542cef6817703
ms.sourcegitcommit: 3727445c1d6374401b867c78e4ff8b07d92b7adc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2018
ms.locfileid: "2916872"
---
# <a name="responsive-layouts-with-xaml"></a>Diseños adaptativos con XAML

El sistema de diseño XAML te permite usar ajustes de tamaño automático, paneles de diseño, estados visuales e incluso definiciones de interfaz de usuario independientes para crear una interfaz de usuario adaptativa. Con un diseño adaptativo, puedes hacer que la aplicación tenga un gran aspecto en pantallas con diferentes tamaños de ventana de aplicación, resoluciones, densidades de píxeles y orientaciones. También puedes usar XAML para cambiar la posición, cambiar el tamaño, redistribuir, mostrar u ocultar, reemplazar y rediseñar la interfaz de usuario de tu aplicación, como se explica en [Técnicas de diseño con capacidad de respuesta](responsive-design.md). Aquí describimos cómo implementar diseños adaptativos con XAML.

## <a name="fluid-layouts-with-properties-and-panels"></a>Diseños fluidos con propiedades y paneles

La base de un diseño adaptativo es hacer un uso adecuado de las propiedades y los paneles de diseño de XAML para cambiar la posición, cambiar el tamaño y redistribuir el contenido de forma fluida. 

El sistema de diseño de XAML admite diseños tanto estáticos como fluidos. En un diseño estático, se definen en los controles tamaños de píxeles y posiciones explícitas. Cuando el usuario cambia la resolución u orientación de su dispositivo, la interfaz de usuario no cambia. Los diseños estáticos pueden recortarse en distintos factores de forma y tamaños de pantalla. Por otro lado, los diseños fluidos se reducen, crecen y vuelven a fluir para responder al espacio visual disponible en un dispositivo. 

En la práctica, usa una combinación de elementos estáticos y fluidos para crear la interfaz de usuario. Seguirás usando elementos y valores estáticos en algunas partes, pero debes asegurarte de que el conjunto de la interfaz de usuario tenga capacidad de respuesta y se adapte a las distintas resoluciones, tamaños de pantalla y vistas.

Aquí describimos cómo usar las propiedades de XAML y los paneles de diseño para crear un diseño fluido.

### <a name="layout-properties"></a>Propiedades de diseño
Las propiedades de diseños controlan el tamaño y la posición de un elemento. Para crear un diseño fluido, usa variaciones de tamaño automáticas o proporcionales para los elementos, y permite que los paneles de diseño establezcan la posición de sus elementos secundarios según sea necesario. 

Aquí se muestran algunas propiedades de diseño habituales y cómo usarlas para crear diseños fluidos.

**Alto y ancho**

Las propiedades [**Height**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.height.aspx) y [**Width**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.width.aspx) especifican el tamaño de un elemento. Puedes usar valores fijos medidos en píxeles efectivos o puedes usar la variación de tamaño automática o proporcional. 

La variación de tamaño automática cambia el tamaño de los elementos de la interfaz de usuario de modo que se ajustan a su contenido o contenedor primario. La variación de tamaño automática también se puede usar con las filas y columnas de una cuadrícula. Para usar la variación de tamaño automática, establece las propiedades Height o Width de los elementos de interfaz de usuario en **Auto**.

> [!NOTE]
> El hecho de que el tamaño de un elemento cambie para ajustarse a su contenido o contenedor depende de cómo el contenedor primario controle la variación de tamaño de sus elementos secundarios. Para más información, consulta [Paneles de diseños](#layout-panels) más adelante en este artículo.

La *variación de tamaño proporcional* distribuye el espacio disponible entre las filas y columnas de una cuadrícula en proporciones ponderadas. En XAML, los valores de variación de tamaño proporcional se expresan como \* (o *n*\* en una variación de tamaño proporcional ponderada). Por ejemplo, para especificar que una columna sea cinco veces más ancha que la segunda columna en un diseño de dos columnas, usa "5\*" y "\*" en las propiedades [**Width**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.columndefinition.width.aspx) de los elementos [**ColumnDefinition**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.columndefinition.aspx).

En este ejemplo, se combinan variaciones de tamaño fijas, automáticas y proporcionales en una clase [**Grid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.aspx) con 4 columnas.

&nbsp;|&nbsp;|&nbsp;
------|------|------
Columna_1 | **Automático** | La columna se ajustará a su contenido.
Columna_2 | * | Después de que se calculen las columnas Auto, la columna recibe parte del ancho restante. Columna_2 será la mitad de ancha que Columna_4.
Columna_3 | **44** | La columna tendrá un ancho de 44 píxeles.
Columna_4 | **2**\* | Después de que se calculen las columnas Auto, la columna recibe parte del ancho restante. Columna_4 será el doble de ancha que Columna_2.

El ancho de columna predeterminado es "*",, de modo que no es necesario establecer explícitamente el valor de la segunda columna.

```xaml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto"/>
        <ColumnDefinition/>
        <ColumnDefinition Width="44"/>
        <ColumnDefinition Width="2*"/>
    </Grid.ColumnDefinitions>
    <TextBlock Text="Column 1 sizes to its conent." FontSize="24"/>
</Grid>
```

En el Diseñador XAML de Visual Studio, el resultado tiene este aspecto.

![Una cuadrícula de 4 columnas en el diseñador de Visual Studio](images/xaml-layout-grid-in-designer.png)

Para obtener el tamaño de un elemento en tiempo de ejecución, usa las propiedades de solo lectura [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.actualheight.aspx) y [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.actualwidth.aspx) en lugar de Height y Width.

**Restricciones de tamaño**

Cuando usas la variación de tamaño automática en la interfaz de usuario, es posible que aún tengas que establecer restricciones en el tamaño de un elemento. Puedes establecer las propiedades [**MinWidth**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.minwidth.aspx)/[**MaxWidth**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.maxwidth.aspx) y [**MinHeight**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.minheight.aspx)/[**MaxHeight**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.maxheight.aspx) para especificar valores que limitan el tamaño de un elemento al tiempo que permiten un cambio de tamaño fluido.

En una Grid, MinWidth/MaxWidth también se puede usar con las definiciones de columna y MinHeight/MaxHeight se puede usar con las definiciones de fila.

**Alineación**

Usa las propiedades [**HorizontalAlignment**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.horizontalalignment.aspx) y [**VerticalAlignment**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.verticalalignment.aspx) para especificar cómo un elemento se debe colocar en su contenedor primario.
- Los valores de **HorizontalAlignment** son **Left**, **Center**, **Right** y **Stretch**.
- Los valores de **VerticalAlignment** son **Top**, **Center**, **Bottom** y **Stretch**.

Con la alineación **Stretch**, los elementos llenan todo el espacio que se les proporciona en el contenedor primario. Stretch es el valor predeterminado para ambas propiedades de alineación. Sin embargo, algunos controles, como [**Button**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.button.aspx), invalidan este valor en su estilo predeterminado.
Cualquier elemento que pueda tener elementos secundarios puede tratar el valor Stretch de las propiedades HorizontalAlignment y VerticalAlignment de manera exclusiva. Por ejemplo, un elemento que usa el valor predeterminado Stretch colocado en una Grid se ajusta para rellenar la celda que lo contiene. El mismo elemento se coloca en tamaños de Canvas según su contenido. Para obtener más información sobre cómo cada panel controla el valor Stretch, consulta el artículo [paneles de diseño](layout-panels.md).

Para más información, consulta el artículo [Alineación, margen y espaciado](alignment-margin-padding.md) y las páginas de referencia [**HorizontalAlignment**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.horizontalalignment.aspx) y [**VerticalAlignment**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.frameworkelement.verticalalignment.aspx).

**Visibilidad**

Puedes mostrar u ocultar un elemento si estableces su propiedad [**Visibility**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.visibility.aspx) en uno de los valores de enumeración [**Visibility**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.visibility.aspx): **Visible** o **Collapsed**. Cuando un elemento tiene el estado Collapsed, no ocupa espacio en el diseño de la interfaz de usuario.

Puedes cambiar la propiedad Visibility de un elemento en código o en un estado visual. Cuando el objeto Visibility de un elemento cambia, también cambian todos sus elementos secundarios. Puedes reemplazar secciones de la interfaz de usuario al mostrar un panel mientras contraes otro.

> [!Tip]
> Cuando haya elementos en la interfaz de usuario que están **contraídas** de forma predeterminada, los objetos se crean todavía en el inicio, aunque no son visibles. Puedes postergar la carga de estos elementos hasta que se muestren si estableces el **atributo x:DeferLoadStrategy** en "Lazy". Esto puede mejorar el rendimiento del inicio. Para obtener más información, consulta [atributo x:DeferLoadStrategy](../../xaml-platform/x-deferloadstrategy-attribute.md).

### <a name="style-resources"></a>Recursos de estilo

No hace falta establecer cada valor individualmente en un control. Suele ser más eficiente agrupar valores de propiedad en un recurso [**Style**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.style.aspx) y aplicar el Style a un control. Esto es especialmente cierto cuando necesites aplicar los mismos valores de propiedad a muchos controles. Para más información sobre los estilos, consulta [Controles de estilo](../controls-and-patterns/xaml-styles.md).

### <a name="layout-panels"></a>Paneles de diseño

Para colocar objetos visuales, debes ponerlos en un panel u otro objeto contenedor. El marco de XAML proporciona diversas clases Panel como [**Canvas**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.aspx), [**Grid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.aspx), [**RelativePanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.aspx) y [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.stackpanel.aspx), que sirven como contenedores y permiten colocar y organizar los elementos de la interfaz de usuario dentro de ellas.

La principal consideración al elegir un panel de diseño es cuál será la posición del panel y el tamaño de sus elementos secundarios. Es posible que también debas considerar cómo se colocarán los elementos secundarios cuando se superpongan.

Esta es una comparación de las principales funciones de los controles de panel que se proporcionan en el marco XAML.

Control de panel | Descripción
--------------|------------
[**Lienzo**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.canvas.aspx) | La clase **Canvas** no admite las interfaces de usuario fluidas; tú controlas todos los aspectos de posición y cambio de tamaño de los elementos secundarios. Normalmente se usa para casos especiales, como la creación de gráficos o la definición de áreas estáticas pequeñas de una interfaz de usuario adaptativa de mayor tamaño. Puedes usar código o estados visuales para cambiar la posición de los elementos en tiempo de ejecución.<li>Los elementos se colocan de forma absoluta mediante las propiedades adjuntas Canvas.Top y Canvas.Left.</li><li>La disposición se puede especificar explícitamente mediante la propiedad adjunta Canvas.ZIndex.</li><li>Los valores Stretch de HorizontalAlignment/VerticalAlignment se omiten. Si el tamaño de un elemento no se establece explícitamente, su tamaño se ajusta a su contenido.</li><li>El contenido secundario no se recorta visualmente si su tamaño es mayor que el panel. </li><li>El contenido secundario no se ve restringido por los límites del panel.</li>
[**Grid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.grid.aspx) | La clase **Grid** admite el cambio de tamaño fluido de los elementos secundarios. Puedes usar código o estados visuales para cambiar la posición y redistribuir elementos.<li>Los elementos se organizan en filas y columnas mediante las propiedades adjuntas Grid.Row y Grid.Column.</li><li>Los elementos pueden abarcar varias filas y columnas mediante las propiedades adjuntas Grid.RowSpan y Grid.ColumnSpan.</li><li>Los valores Stretch de HorizontalAlignment/VerticalAlignment se respetan. Si el tamaño de un elemento no se establece explícitamente, se amplía para rellenar el espacio disponible en la celda de la cuadrícula.</li><li>El contenido secundario se recorta visualmente si su tamaño es mayor que el panel.</li><li>El tamaño del contenido se ve restringido por los límites del panel, por lo que el contenido desplazable muestra barras de desplazamiento si es necesario.</li>
[**RelativePanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.aspx) | <li>Los elementos se organizan en relación con el borde o el centro del panel y en relación entre ellos.</li><li>Los elementos se colocan mediante una variedad de propiedades adjuntas que controlan la alineación del panel, la alineación de elementos de mismo nivel y la posición de los elemento del mismo nivel. </li><li>Los valores Stretch de HorizontalAlignment/VerticalAlignment se omiten, a menos que las propiedades adjuntas RelativePanel de la alineación provoquen una ampliación (por ejemplo, un elemento se alinea a los bordes derecho e izquierdo del panel). Si el tamaño de un elemento no se establece explícitamente y no se amplía, su tamaño se ajusta a su contenido.</li><li>El contenido secundario se recorta visualmente si su tamaño es mayor que el panel.</li><li>El tamaño del contenido se ve restringido por los límites del panel, por lo que el contenido desplazable muestra barras de desplazamiento si es necesario.</li>
[**StackPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.stackpanel.aspx) |<li>Los elementos se apilan en una única línea vertical u horizontal.</li><li>Los valores Stretch de HorizontalAlignment/VerticalAlignment se respetan en la dirección opuesta a la propiedad Orientation. Si el tamaño de un elemento no se establece explícitamente, se amplía para rellenar el ancho disponible (o el alto si la Orientation es Horizontal). En la dirección especificada por la propiedad Orientation, el tamaño de un elemento se ajusta a su contenido.</li><li>El contenido secundario se recorta visualmente si su tamaño es mayor que el panel.</li><li>El tamaño del contenido no está restringido por los límites del panel en la dirección especificada por la propiedad Orientation, por lo que el contenido desplazable se amplía más allá de los límites del panel y no se muestran barras de desplazamiento. Tienes que restringir explícitamente el alto (o el ancho) del contenido secundario para mostrar sus barras de desplazamiento.</li>
[**VariableSizedWrapGrid**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.variablesizedwrapgrid.aspx) |<li>Los elementos se organizan en filas o en columnas que se ajustan automáticamente a una nueva fila o columna cuando se alcanza el valor MaximumRowsOrColumns.</li><li>La organización de los elementos en filas o en columnas se especifica mediante la propiedad Orientation.</li><li>Los elementos pueden abarcar varias filas y columnas mediante las propiedades adjuntas VariableSizedWrapGrid.RowSpan y VariableSizedWrapGrid.ColumnSpan.</li><li>Los valores Stretch de HorizontalAlignment/VerticalAlignment se omiten. El tamaño de los elementos se ajusta según se especifica en las propiedades ItemHeight y ItemWidth. Si no se establecen estas propiedades, el tamaño del elemento de la primera se ajusta a su contenido y todas las demás celdas heredan este tamaño.</li><li>El contenido secundario se recorta visualmente si su tamaño es mayor que el panel.</li><li>El tamaño del contenido se ve restringido por los límites del panel, por lo que el contenido desplazable muestra barras de desplazamiento si fuera necesario.</li>

Para obtener información detallada y ejemplos de estos paneles, consulta [Paneles de diseño](layout-panels.md). Consulta también la [Muestra de técnicas de capacidad de respuesta](http://go.microsoft.com/fwlink/p/?LinkId=620024).

Los paneles de diseño permiten organizar la interfaz de usuario en grupos lógicos de controles. Cuando los usas con valores de propiedad adecuados, obtienes algo de compatibilidad con la variación de tamaño automática, el cambio de posición y la redistribución de elementos de interfaz de usuario. Sin embargo, la mayoría de los diseños de interfaz de usuario requiere más modificaciones cuando hay cambios significativos en el tamaño de ventana. Para ello, puedes usar estados visuales.

## <a name="adaptive-layouts-with-visual-states-and-state-triggers"></a>Diseños adaptativos con estados visuales y desencadenadores de estado
Usa estados visuales para hacer modificaciones importantes en tu interfaz de usuario en relación con el tamaño de la ventana u otros cambios.

Cuando el tamaño de la ventana de tu aplicación aumenta o disminuye más allá de una cantidad determinada, es recomendable cambiar las propiedades de diseño para cambiar la posición, cambiar el tamaño, redistribuir, mostrar o reemplazar secciones de la interfaz de usuario. Puedes definir diferentes estados visuales para la interfaz de usuario y aplicarlos cuando el ancho o el alto de la ventana cruce un umbral especificado. 

Un [**AdaptiveTrigger**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.adaptivetrigger.aspx) proporciona una forma fácil de establecer el umbral (también llamado 'punto de interrupción') en el que se aplica un estado. Un [**VisualState**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.visualstate.aspx) define los valores de propiedad que se aplican a un elemento cuando este se encuentra en un estado concreto. Los estados visuales se agrupan en un [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.visualstatemanager.aspx) que aplica el VisualState adecuado cuando se cumplen las condiciones especificadas.

### <a name="set-visual-states-in-code"></a>Establecer estados visuales en el código

Para aplicar un estado visual a partir de código, llama al método [**VisualStateManager.GoToState**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.visualstatemanager.gotostate.aspx). Por ejemplo, para aplicar un estado cuando la ventana de la aplicación tiene un tamaño determinado, controla el evento [**SizeChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.window.sizechanged.aspx) y llama a **GoToState** para aplicar el estado pertinente.

Aquí, un [**VisualStateGroup**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.visualstategroup.aspx) contiene dos definiciones VisualState. La primera, `DefaultState`, está vacía. Cuando se aplica, se aplican los valores definidos en la página XAML. La segunda, `WideState`, cambia la propiedad [**DisplayMode**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.splitview.displaymode.aspx) de la [**SplitView**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.splitview.aspx) a **Inline** y abre el panel. Este estado se aplica en el controlador de eventos SizeChanged si la ventana tiene un ancho superior a 640 píxeles efectivos.

> [!NOTE]
> Windows no proporciona un método para que la aplicación detecte el dispositivo específico en el que esta se ejecuta. Puede indicar la familia de dispositivos (móvil, de escritorio, etc.) en que se ejecuta la aplicación, la resolución efectiva y la cantidad de espacio de pantalla disponible en la aplicación (el tamaño de la ventana de la aplicación). Te recomendamos definir estados visuales para [tamaños de pantalla y puntos de interrupción](screen-sizes-and-breakpoints-for-responsive-design.md).


```xaml
<Page ...>
    <Grid>
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup>
                <VisualState x:Name="DefaultState">
                        <Storyboard>
                        </Storyboard>
                    </VisualState>

                <VisualState x:Name="WideState">
                    <Storyboard>
                        <ObjectAnimationUsingKeyFrames
                            Storyboard.TargetProperty="SplitView.DisplayMode"
                            Storyboard.TargetName="mySplitView">
                            <DiscreteObjectKeyFrame KeyTime="0">
                                <DiscreteObjectKeyFrame.Value>
                                    <SplitViewDisplayMode>Inline</SplitViewDisplayMode>
                                </DiscreteObjectKeyFrame.Value>
                            </DiscreteObjectKeyFrame>
                        </ObjectAnimationUsingKeyFrames>
                        <ObjectAnimationUsingKeyFrames
                            Storyboard.TargetProperty="SplitView.IsPaneOpen"
                            Storyboard.TargetName="mySplitView">
                            <DiscreteObjectKeyFrame KeyTime="0" Value="True"/>
                        </ObjectAnimationUsingKeyFrames>
                    </Storyboard>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

        <SplitView x:Name="mySplitView" DisplayMode="CompactInline"
                   IsPaneOpen="False" CompactPaneLength="20">
            <!-- SplitView content -->

            <SplitView.Pane>
                <!-- Pane content -->
            </SplitView.Pane>
        </SplitView>
    </Grid>
</Page>
```

```csharp
private void CurrentWindow_SizeChanged(object sender, Windows.UI.Core.WindowSizeChangedEventArgs e)
{
    if (e.Size.Width > 640)
        VisualStateManager.GoToState(this, "WideState", false);
    else
        VisualStateManager.GoToState(this, "DefaultState", false);
}
```

### <a name="set-visual-states-in-xaml-markup"></a>Establecer estados visuales en el marcado XAML

Antes de Windows 10, las definiciones de VisualState necesitaban objetos [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.animation.storyboard.aspx) para los cambios de propiedad y hacía falta llamar a **GoToState** en el código para aplicar el estado. Esto se muestra en el ejemplo anterior. Verás muchos ejemplos que usan esta sintaxis, o bien quizás ya tengas código existente que la usa.

A partir de Windows 10, puedes usar la sintaxis [**Setter**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.setter.aspx) simplificada que se muestra aquí y puedes usar los objetos [**StateTrigger**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.statetrigger.aspx) en el marcado XAML para aplicar el estado. Los desencadenadores de estado se usan para crear reglas simples que desencadenan automáticamente cambios de estado visual en respuesta a un evento de la aplicación.

En este ejemplo se hace lo mismo que en el ejemplo anterior, pero se usa la sintaxis **Setter** simplificada en lugar de un Storyboard para definir los cambios de propiedad. Además, en lugar de llamar a GoToState, se usa el desencadenador de estado [**AdaptiveTrigger**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.adaptivetrigger.aspx) para aplicar el estado. Cuando usas desencadenadores de estado, no es necesario definir un `DefaultState` vacío. La configuración predeterminada se vuelve a aplicar automáticamente cuando las condiciones del desencadenador de estado ya no se cumplen.

```xaml
<Page ...>
    <Grid>
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup>
                <VisualState>
                    <VisualState.StateTriggers>
                        <!-- VisualState to be triggered when the
                             window width is >=720 effective pixels. -->
                        <AdaptiveTrigger MinWindowWidth="640" />
                    </VisualState.StateTriggers>

                    <VisualState.Setters>
                        <Setter Target="mySplitView.DisplayMode" Value="Inline"/>
                        <Setter Target="mySplitView.IsPaneOpen" Value="True"/>
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

        <SplitView x:Name="mySplitView" DisplayMode="CompactInline"
                   IsPaneOpen="False" CompactPaneLength="20">
            <!-- SplitView content -->

            <SplitView.Pane>
                <!-- Pane content -->
            </SplitView.Pane>
        </SplitView>
    </Grid>
</Page>
```

> [!Important]
> En el ejemplo anterior, se establece la propiedad adjunta VisualStateManager.VisualStateGroups en el elemento de **cuadrícula** . Cuando usas objetos StateTrigger, para que los desencadenadores se apliquen automáticamente, asegúrate siempre de que VisualStateGroups se adjunte al primer elemento secundario de la raíz. (Aquí, **Grid** es el primer elemento secundario del elemento **Page** raíz).

### <a name="attached-property-syntax"></a>Sintaxis de propiedad adjunta

En un VisualState, normalmente se establece un valor para una propiedad de control, o bien para una de las propiedades adjuntas del panel que contiene el control. Al establecer una propiedad adjunta, usa paréntesis alrededor del nombre de la propiedad adjunta.

En este ejemplo se muestra cómo establecer la propiedad adjunta [**RelativePanel.AlignHorizontalCenterWithPanel**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.relativepanel.alignhorizontalcenterwithpanel.aspx) en un TextBox denominado `myTextBox`. El primer código XAML usa la sintaxis [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.animation.objectanimationusingkeyframes.aspx) y el segundo usa **Setter**.

```xaml
<!-- Set an attached property using ObjectAnimationUsingKeyFrames. -->
<ObjectAnimationUsingKeyFrames
    Storyboard.TargetProperty="(RelativePanel.AlignHorizontalCenterWithPanel)"
    Storyboard.TargetName="myTextBox">
    <DiscreteObjectKeyFrame KeyTime="0" Value="True"/>
</ObjectAnimationUsingKeyFrames>

<!-- Set an attached property using Setter. -->
<Setter Target="myTextBox.(RelativePanel.AlignHorizontalCenterWithPanel)" Value="True"/>
```

### <a name="custom-state-triggers"></a>Desencadenadores de estado personalizados

Puedes ampliar la clase [**StateTrigger**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.statetrigger.aspx) y crear desencadenadores personalizados para una amplia variedad de escenarios. Por ejemplo, puedes crear un StateTrigger para desencadenar estados diferentes según el tipo de entrada y luego aumentar los márgenes alrededor de un control cuando el tipo de entrada es la entrada táctil. O bien, puedes crear un objeto StateTrigger para aplicar distintos estados en función de la familia de dispositivos en la que se ejecuta la aplicación. Para obtener ejemplos de cómo crear desencadenadores personalizados y usarlos para crear experiencias de interfaz de usuario optimizadas desde dentro de una única vista XAML, consulta la [Muestra de desencadenadores de estado](http://go.microsoft.com/fwlink/p/?LinkId=620025).

### <a name="visual-states-and-styles"></a>Estados visuales y estilos

Puedes usar los recursos Style en los estados visuales para aplicar un conjunto de cambios de propiedades a varios controles. Para obtener más información sobre los estilos, consulta [Controles de estilo](../controls-and-patterns/xaml-styles.md).

En este código XAML simplificado de la Muestra de desencadenadores de estado, un recurso Style se aplica a un objeto Button para ajustar el tamaño y los márgenes de la entrada de mouse o entrada táctil. Para obtener el código completo y la definición del desencadenador de estado personalizado, consulta [Muestra de desencadenadores de estado](http://go.microsoft.com/fwlink/p/?LinkId=620025).

```xaml
<Page ... >
    <Page.Resources>
        <!-- Styles to be used for mouse vs. touch/pen hit targets -->
        <Style x:Key="MouseStyle" TargetType="Rectangle">
            <Setter Property="Margin" Value="5" />
            <Setter Property="Height" Value="20" />
            <Setter Property="Width" Value="20" />
        </Style>
        <Style x:Key="TouchPenStyle" TargetType="Rectangle">
            <Setter Property="Margin" Value="15" />
            <Setter Property="Height" Value="40" />
            <Setter Property="Width" Value="40" />
        </Style>
    </Page.Resources>

    <RelativePanel>
        <!-- ... -->
        <Button Content="Color Palette Button" x:Name="MenuButton">
            <Button.Flyout>
                <Flyout Placement="Bottom">
                    <RelativePanel>
                        <Rectangle Name="BlueRect" Fill="Blue"/>
                        <Rectangle Name="GreenRect" Fill="Green" RelativePanel.RightOf="BlueRect" />
                        <!-- ... -->
                    </RelativePanel>
                </Flyout>
            </Button.Flyout>
        </Button>
        <!-- ... -->
    </RelativePanel>
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="InputTypeStates">
            <!-- Second set of VisualStates for building responsive UI optimized for input type.
                 Take a look at InputTypeTrigger.cs class in CustomTriggers folder to see how this is implemented. -->
            <VisualState>
                <VisualState.StateTriggers>
                    <!-- This trigger indicates that this VisualState is to be applied when MenuButton is invoked using a mouse. -->
                    <triggers:InputTypeTrigger TargetElement="{x:Bind MenuButton}" PointerType="Mouse" />
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="BlueRect.Style" Value="{StaticResource MouseStyle}" />
                    <Setter Target="GreenRect.Style" Value="{StaticResource MouseStyle}" />
                    <!-- ... -->
                </VisualState.Setters>
            </VisualState>
            <VisualState>
                <VisualState.StateTriggers>
                    <!-- Multiple trigger statements can be declared in the following way to imply OR usage.
                         For example, the following statements indicate that this VisualState is to be applied when MenuButton is invoked using Touch OR Pen.-->
                    <triggers:InputTypeTrigger TargetElement="{x:Bind MenuButton}" PointerType="Touch" />
                    <triggers:InputTypeTrigger TargetElement="{x:Bind MenuButton}" PointerType="Pen" />
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="BlueRect.Style" Value="{StaticResource TouchPenStyle}" />
                    <Setter Target="GreenRect.Style" Value="{StaticResource TouchPenStyle}" />
                    <!-- ... -->
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Page>
```

## <a name="tailored-layouts"></a>Diseños personalizados

Cuando haces cambios importantes en el diseño de la interfaz de usuario en diferentes dispositivos, puede que te resulte más cómodo definir un archivo de interfaz de usuario independiente con un diseño adaptado para el dispositivo, en lugar de adaptar una sola interfaz de usuario. Si la funcionalidad es la misma en todos los dispositivos, puedes definir vistas de XAML separadas que comparten el mismo archivo de código. Si tanto la vista y como la funcionalidad varían significativamente entre dispositivos, puedes definir objetos Page independientes y elegir el objeto Page al que navegar cuando se cargue la aplicación.

### <a name="separate-xaml-views-per-device-family"></a>Vistas de XAML separadas por familia de dispositivos

Usa vistas de XAML para crear definiciones de interfaz de usuario diferentes que comparten el mismo código subyacente. Puedes proporcionar una única definición de interfaz de usuario para cada familia de dispositivos. Sigue estos pasos para agregar una vista XAML a tu aplicación.

**Agregar una vista XAML a una aplicación**
1. Selecciona Proyecto > Agregar nuevo elemento. Se abrirá el cuadro de diálogo Agregar nuevo elemento,
    > **Sugerencia**&nbsp;&nbsp;Asegúrate de que una carpeta o el proyecto, y no la solución, estén seleccionados en el Explorador de soluciones.
2. En Visual C# o Visual Basic, en el panel izquierdo, elige el tipo de plantilla XAML.
3. En el panel central, selecciona Vista XAML.
4. Escribe el nombre de la vista. El nombre de la vista se debe asignar correctamente. Para más información sobre la asignación de nombres, consulta el resto de esta sección.
5. Haz clic en Agregar. El archivo se agrega al proyecto.

En los pasos anteriores se crea un archivo XAML, pero no en un archivo de código subyacente asociado. En cambio, la vista XAML está asociada con un archivo de código subyacente existente con un calificador "DeviceName" que forma parte del nombre de archivo o carpeta. Este nombre de calificador se puede asignar a un valor de cadena que representa la familia de dispositivos del dispositivo en el que se ejecuta la aplicación, como por ejemplo, "Escritorio", "Tableta" y los nombres de otras familias de dispositivos (consulta [**ResourceContext.QualifierValues**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.resources.core.resourcecontext.qualifiervalues.aspx)).

Puedes agregar el calificador al nombre de archivo o agregar el archivo a una carpeta que contiene el nombre del calificador.

**Usar el nombre de archivo**

Para usar el nombre de calificador con el archivo, usa este formato: *[pageName]*.DeviceFamily-*[qualifierString]*.xaml.

Veamos un ejemplo de un archivo denominado MainPage.xaml. Para crear una vista para tabletas, denomina la vista XAML MainPage.DeviceFamily-Tablet.xaml. Para crear una vista para dispositivos de PC, denomina la vista MainPage.DeviceFamily-Desktop.xaml. Este es el aspecto de la solución en Microsoft Visual Studio.

![Vistas XAML con nombres de archivo completo](images/xaml-layout-view-ex-1.png)

**Usar el nombre de carpeta**

Para organizar las vistas en el proyecto de Visual Studio con carpetas, puedes usar el nombre de calificador con la carpeta. Para ello, asigna un nombre a la carpeta del modo siguiente: DeviceFamily-*[qualifierString]*. En este caso, cada archivo de vista XAML tiene el mismo nombre. No incluyas el calificador en el nombre de archivo.

Este es un ejemplo, para un archivo denominado MainPage.xaml.. Para crear una vista para tabletas, crea una carpeta denominada "DeviceFamily-Tablet" y coloca una vista XAML denominada MainPage.xaml en ella. Para crear una vista para dispositivos PC, crea una carpeta denominada "DeviceFamily-Desktop" y coloca otra vista XAML denominada MainPage.xaml en ella. Este es el aspecto de la solución en Visual Studio.

![Vistas XAML en carpetas](images/xaml-layout-view-ex-2.png)

En ambos casos, se usa una vista única para tabletas y PC. El archivo MainPage.xaml se usa si el dispositivo en el que se ejecuta no coincide con ninguna de las vistas específicas de la familia de dispositivos.

### <a name="separate-xaml-pages-per-device-family"></a>Páginas XAML independientes por familia de dispositivos

Para proporcionar vistas y funcionalidad únicas, puedes crear archivos Page (XAML y código) independientes y luego navegar a la página adecuada cuando se necesite la página.

**Agregar una página XAML a una aplicación**
1. Selecciona Proyecto > Agregar nuevo elemento. Se abrirá el cuadro de diálogo Agregar nuevo elemento,
    > **Sugerencia**&nbsp;&nbsp;Asegúrate de que el proyecto, y no la solución, esté seleccionado en el Explorador de soluciones.
2. En Visual C# o Visual Basic, en el panel izquierdo, elige el tipo de plantilla XAML.
3. En el panel central, selecciona Página en blanco.
4. Escribe el nombre para la página. Por ejemplo, "MainPage_Tablet". Se crean archivos de código MainPage_Tablet.xaml y MainPage_Tablet.xaml.cs/vb/cpp.
5. Haz clic en Agregar. El archivo se agrega al proyecto.

En tiempo de ejecución, comprueba que la familia de dispositivos en la que se ejecuta la aplicación y navega a la página correcta de este modo.

```csharp
if (Windows.System.Profile.AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Tablet")
{
    rootFrame.Navigate(typeof(MainPage_Tablet), e.Arguments);
}
else
{
    rootFrame.Navigate(typeof(MainPage), e.Arguments);
}
```

También puedes usar distintos criterios para determinar a qué página navegar. Para obtener más ejemplos, consulta la muestra [Varias vistas adaptadas](http://go.microsoft.com/fwlink/p/?LinkId=620636) en la que se usa la función [**GetIntegratedDisplaySize**](https://msdn.microsoft.com/library/windows/apps/xaml/dn904185.aspx) para comprobar el tamaño físico de una pantalla integrada.

## <a name="related-topics"></a>Artículos relacionados
- [Tutorial: Creación de diseños adaptativos](../basics/xaml-basics-adaptive-layout.md)
- [Ejemplo de técnicas de capacidad de respuesta (GitHub)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlResponsiveTechniques)
- [Muestra de desencadenadores de estado (GitHub)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlStateTriggers)
- [Muestra de varias vistas personalizadas (GitHub)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlTailoredMultipleViews)
