---
ms.assetid: 569E8C27-FA01-41D8-80B9-1E3E637D5B99
title: Optimizar el marcado XAML
description: El análisis del marcado XAML para crear objetos en la memoria requiere mucho tiempo para una interfaz de usuario compleja. Estas son algunas acciones que puedes realizar para mejorar el análisis del marcado XAML, el tiempo de carga y la eficiencia de la memoria de tu aplicación.
ms.date: 08/10/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: beb6dde4036019e004d94e5f60e8f3583c78d775
ms.sourcegitcommit: de34aabd90a92a083dfa17d4f8a98578597763f4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/28/2019
ms.locfileid: "72980023"
---
# <a name="optimize-your-xaml-markup"></a>Optimizar el marcado XAML


El análisis del marcado XAML para crear objetos en la memoria requiere mucho tiempo para una interfaz de usuario compleja. Estas son algunas acciones que puedes realizar para mejorar el análisis y el tiempo de carga del marcado XAML y la eficiencia de la memoria de tu aplicación.

Al iniciar la aplicación, limita el marcado XAML que se carga para que la interfaz de usuario inicial use solo lo que es necesario. Examina el marcado en la página inicial (incluidos los recursos de página) y confirma que no se están cargando elementos adicionales que no se necesitan inmediatamente. Estos elementos pueden proceder de una variedad de orígenes, como los diccionarios de recursos, los elementos que inicialmente están contraídos y elementos que se dibujan sobre otros elementos.

Optimizar la eficiencia de tu código XAML necesita un equilibrio, no siempre hay una única solución para cada situación. Aquí echaremos un vistazo a algunos de los problemas comunes y proporcionaremos instrucciones detalladas que puedes usar para realizar el equilibrio adecuado para tu aplicación.

## <a name="minimize-element-count"></a>Minimizar el recuento de elementos

Si bien la plataforma XAML es capaz de mostrar un gran número de elementos, puedes hacer que tu aplicación diseñe y represente contenidos más rápido usando la menor cantidad de elementos para lograr los elementos visuales que quieras.

Las decisiones que tomas a la hora de diseñar los controles de interfaz de usuario afectará el número de elementos de interfaz de usuario que se crean al iniciar la aplicación. Para obtener más información más detallada sobre la optimización del diseño, consulta [Optimiza tu diseño XAML](optimize-your-xaml-layout.md).

El recuento de elementos es muy importante en las plantillas de datos, porque cada elemento se vuelve a crear para cada elemento de datos. Para obtener información acerca de cómo reducir el recuento de elementos en una lista o cuadrícula, consulta *Reducción del elemento por elemento* en el artículo [Optimización de interfaz de usuario de ListView y GridView](optimize-gridview-and-listview.md).

Aquí vamos a ver algunas otras maneras de reducir el número de elementos que la aplicación tiene que cargar al inicio.

### <a name="defer-item-creation"></a>Aplazar la creación de elementos

Si el marcado XAML contiene elementos que no muestran inmediatamente, puedes posponer la carga de esos elementos hasta que se muestren. Por ejemplo, puedes retrasar la creación de contenido no visible, como una pestaña secundaria en una interfaz de usuario similar a una pestaña. O bien, puedes mostrar elementos en una vista de cuadrícula de forma predeterminada, pero debes proporcionar una opción para que el usuario pueda ver los datos en una lista. Puedes retrasar la carga de la lista hasta que se necesite.

Usa [x:Load attribute](../xaml-platform/x-load-attribute.md) en lugar de la propiedad [Visibility](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.Visibility) para controlar cuándo se muestra un elemento. Cuando la visibilidad de un elemento se marca como **Contraído**, se omitirá durante el pase de representación, pero todavía pagas los costos de la instancia del objeto en la memoria. Si usas x:Load en su lugar, el marco no creará la instancia del objeto hasta que se necesite, por lo que los costos de memoria son más bajos aún. El inconveniente es el costo de una pequeña sobrecarga de memoria (unos 600 bytes) cuando no se puede cargar la interfaz de usuario.

> [!NOTE]
> Puedes retrasar la carga de elementos mediante el atributo [x:Load](../xaml-platform/x-load-attribute.md) o [x:DeferLoadStrategy](../xaml-platform/x-deferloadstrategy-attribute.md). El atributo x:Load está disponible a partir de Windows 10 Creators Update (versión 1703, compilación de SDK 15063). Para poder usar x:Load, la versión mínima del proyecto de Visual Studio debe ser *Windows 10 Creators Update (10.0, compilación 15063)* . Para seleccionar versiones anteriores, usa x:DeferLoadStrategy.

Los siguientes ejemplos muestran la diferencia en el recuento de elementos y el uso de memoria cuando se usan técnicas diferentes para ocultar elementos de la interfaz de usuario. Los controles ListView y GridView que contienen elementos idénticos se colocan en la Grid de la página de la raíz. La ListView no está visible pero se muestra la GridView. El código XAML en cada uno de estos ejemplos produce la misma interfaz de usuario en la pantalla. Usamos las herramientas de Visual Studio [de generación de perfiles y rendimiento](tools-for-profiling-and-performance.md) para comprobar el uso de memoria y el recuento de elementos.

#### <a name="option-1---inefficient"></a>Opción 1: Ineficaz

Aquí se carga la clase ListView, pero no está visible porque su ancho es 0. La ListView y cada uno de sus elementos secundarios se crean en el árbol visual y se cargan en la memoria.

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <ListView x:Name="List1" Width="0">
        <ListViewItem>Item 1</ListViewItem>
        <ListViewItem>Item 2</ListViewItem>
        <ListViewItem>Item 3</ListViewItem>
        <ListViewItem>Item 4</ListViewItem>
        <ListViewItem>Item 5</ListViewItem>
        <ListViewItem>Item 6</ListViewItem>
        <ListViewItem>Item 7</ListViewItem>
        <ListViewItem>Item 8</ListViewItem>
        <ListViewItem>Item 9</ListViewItem>
        <ListViewItem>Item 10</ListViewItem>
    </ListView>

    <GridView x:Name="Grid1">
        <GridViewItem>Item 1</GridViewItem>
        <GridViewItem>Item 2</GridViewItem>
        <GridViewItem>Item 3</GridViewItem>
        <GridViewItem>Item 4</GridViewItem>
        <GridViewItem>Item 5</GridViewItem>
        <GridViewItem>Item 6</GridViewItem>
        <GridViewItem>Item 7</GridViewItem>
        <GridViewItem>Item 8</GridViewItem>
        <GridViewItem>Item 9</GridViewItem>
        <GridViewItem>Item 10</GridViewItem>
    </GridView>
</Grid>
```

El árbol visual activo con el control ListView se cargan. El número total de los elementos de la página es 89.

![Árbol visual con vista de lista](images/visual-tree-1.png)

ListView y sus elementos secundarios se cargan en la memoria.

![Árbol visual con vista de lista](images/memory-use-1.png)

#### <a name="option-2---better"></a>Opción 2: Mejor

Aquí, la visibilidad de ListView se establece como contraída (el otro código XAML es idéntico al original). Se crea el control ListView en el árbol visual, pero no sus elementos secundarios. Sin embargo, sí se cargan en la memoria, por lo que el uso de memoria es idéntico al ejemplo anterior.

```xaml
    <ListView x:Name="List1" Visibility="Collapsed">
```

El árbol visual activo con el control ListView se contraen. El número total de los elementos de la página es 46.

![Árbol visual con vista de lista contraída](images/visual-tree-2.png)

ListView y sus elementos secundarios se cargan en la memoria.

![Árbol visual con vista de lista](images/memory-use-1.png)

#### <a name="option-3---most-efficient"></a>Opción 3 - Más eficaz

Aquí ListView tiene el atributo x:Load establecido en **False** (el otro código XAML es idéntico al original). La ListView no se crea en el árbol visual ni se carga en la memoria en el inicio.

```xaml
    <ListView x:Name="List1" Visibility="Collapsed" x:Load="False">
```

No se carga el árbol visual activo con el control ListView. El número total de los elementos de la página es 45.

![No se carga el árbol visual con vista de lista](images/visual-tree-3.png)

ListView y sus elementos secundarios no se cargan en la memoria.

![Árbol visual con vista de lista](images/memory-use-3.png)

> [!NOTE]
> Los recuentos de elementos y el uso de memoria en estos ejemplos son muy pequeños y se muestran únicamente para demostrar el concepto. En estos ejemplos, la sobrecarga que implica usar x:Load es mayor que el ahorro de memoria, por lo que la aplicación no se beneficiaría. Debes usar las herramientas de generación de perfiles en tu aplicación para determinar si la aplicación se beneficiará de la carga aplazada.

### <a name="use-layout-panel-properties"></a>Usar propiedades del panel de diseño

Los paneles de diseño tienen una propiedad [Background](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.panel.background), por lo tanto, no es necesario colocar una clase [Rectangle](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) delante de un panel solo para colorearlo.

**Ineficaz**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<Grid>
    <Rectangle Fill="Black"/>
</Grid>
```

**Eficaz**

```xaml
<Grid Background="Black"/>
```

Los paneles de diseño también tienen propiedades de borde integradas, por lo que no necesitas colocar un elemento [Border](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.border) alrededor de un panel de diseño. Consulta [Optimizar el diseño XAML](optimize-your-xaml-layout.md) para más información y ejemplos.

### <a name="use-images-in-place-of-vector-based-elements"></a>Usar imágenes en lugar de elementos basados en vector

Si debes volver a usar el mismo elemento basado en vectores numerosas veces, te resultará más útil usar un elemento [Image](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image) en su lugar. Los elementos basados en vectores pueden ser más engorrosos porque la CPU debe crear cada elemento individual por separado. Ten en cuenta que el archivo de imagen debe descodificarse una sola vez.

## <a name="optimize-resources-and-resource-dictionaries"></a>Optimizar los recursos y los diccionarios de recursos

Normalmente se usan [diccionarios de recursos](../design/controls-and-patterns/resourcedictionary-and-xaml-resource-references.md) para almacenar, en un nivel algo global, recursos que a los que quieras hacer referencia en varios lugares en la aplicación. Por ejemplo, estilos, pinceles, plantillas, etc.

En general, hemos optimizado el elemento [ResourceDictionary](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary) para que no cree instancias de los recursos a menos que se pidan. Pero hay situaciones en las que debes evitar para que no se cree una instancia de recursos de forma innecesaria.

### <a name="resources-with-xname"></a>Recursos con x:Name

Usa el [atributo x:Key](../xaml-platform/x-key-attribute.md) para hacer referencia a los recursos. Los recursos con el atributo [x:Name](../xaml-platform/x-name-attribute.md) no sacarán partido de la optimización de la plataforma, en cambio, se creará una instancia de ellos en cuanto se cree el objeto ResourceDictionary. Esto sucede porque x:Name indica a la plataforma que la aplicación necesita acceso de campo a este recurso, por lo que debe crear algo a que hacer referencia.

### <a name="resourcedictionary-in-a-usercontrol"></a>ResourceDictionary en un elemento UserControl.

Un elemento ResourceDictionary definido en un objeto [UserControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.usercontrol) resulta en una penalización. La plataforma crea una copia de este tipo de objeto ResourceDictionary para todas las instancias del objeto UserControl. Si tiene un UserControl que se usa mucho, mueva el ResourceDictionary fuera del UserControl y colóquelo en el nivel de página.

### <a name="resource-and-resourcedictionary-scope"></a>Ámbito de Resource y ResourceDictionary

Si una página hace referencia a un control de usuario o un recurso definido en otro archivo, el marco también analizará ese archivo.

Aquí, debido a que_InitialPage.xaml_ usa un recurso de _ExampleResourceDictionary.xaml_, la totalidad de _ExampleResourceDictionary.xaml_ debe analizarse en el inicio.

**InitialPage. Xaml.**

```xaml
<Page x:Class="ExampleNamespace.InitialPage" ...>
    <Page.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <ResourceDictionary Source="ExampleResourceDictionary.xaml"/>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Page.Resources>

    <Grid>
        <TextBox Foreground="{StaticResource TextBrush}"/>
    </Grid>
</Page>
```

**ExampleResourceDictionary. Xaml.**

```xaml
<ResourceDictionary>
    <SolidColorBrush x:Key="TextBrush" Color="#FF3F42CC"/>

    <!--This ResourceDictionary contains many other resources that
        are used in the app, but are not needed during startup.-->
</ResourceDictionary>
```

Si usas un recurso en muchas páginas en toda la aplicación, almacenarlo en _App.xaml_ es una buena práctica y evita la duplicación. Sin embargo, _App.xaml_ se analiza en el inicio de la aplicación por lo tanto, cualquier recurso que se use en únicamente una página (a menos que esa página sea la página inicial) se debe colocar en los recursos locales de la página. Este ejemplo de contador muestra _App.xaml_ que contiene recursos usados en únicamente una página (que no es la página inicial). Esto aumenta innecesariamente el tiempo de inicio de la aplicación.

**App. Xaml**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<Application ...>
     <Application.Resources>
        <SolidColorBrush x:Key="DefaultAppTextBrush" Color="#FF3F42CC"/>
        <SolidColorBrush x:Key="InitialPageTextBrush" Color="#FF3F42CC"/>
        <SolidColorBrush x:Key="SecondPageTextBrush" Color="#FF3F42CC"/>
        <SolidColorBrush x:Key="ThirdPageTextBrush" Color="#FF3F42CC"/>
    </Application.Resources>
</Application>
```

**InitialPage. Xaml.**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<Page x:Class="ExampleNamespace.InitialPage" ...>
    <StackPanel>
        <TextBox Foreground="{StaticResource InitialPageTextBrush}"/>
    </StackPanel>
</Page>
```

**SecondPage. Xaml.**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<Page x:Class="ExampleNamespace.SecondPage" ...>
    <StackPanel>
        <Button Content="Submit" Foreground="{StaticResource SecondPageTextBrush}" />
    </StackPanel>
</Page>
```

La manera de hacer que el ejemplo anterior de contador sea más eficiente es mover `SecondPageTextBrush` a _SecondPage.xaml_ y mover `ThirdPageTextBrush` a _ThirdPage.xaml_. `InitialPageTextBrush` puede permanecer en _app. Xaml_ porque los recursos de la aplicación deben analizarse en el inicio de la aplicación en cualquier caso.

### <a name="consolidate-multiple-brushes-that-look-the-same-into-one-resource"></a>Consolidar varios pinceles que tengan el mismo aspecto en un recurso

La plataforma XAML trata de almacenar en caché los objetos usados frecuentemente para que puedan volver a usarse siempre que sea posible. De todos modos, el lenguaje XAML no puede determinar fácilmente si un pincel declarado en una pieza de marcado es el mismo que está declarado en otra. Este ejemplo usa [SolidColorBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) como demostración, pero el caso resulta ser más probable y más importante mediante [GradientBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.GradientBrush). Busca también pinceles que usen los colores predefinidos, por ejemplo, `"Orange"` y `"#FFFFA500"` son el mismo color.

**Ineficaz.**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<Page ... >
    <StackPanel>
        <TextBlock>
            <TextBlock.Foreground>
                <SolidColorBrush Color="#FFFFA500"/>
            </TextBlock.Foreground>
        </TextBlock>
        <Button Content="Submit">
            <Button.Foreground>
                <SolidColorBrush Color="#FFFFA500"/>
            </Button.Foreground>
        </Button>
    </StackPanel>
</Page>
```

Para corregir la duplicación, define el pincel como un recurso. Si los controles de otras páginas usan el mismo pincel, muévelo a _App.xaml_.

**Eficaz.**

```xaml
<Page ... >
    <Page.Resources>
        <SolidColorBrush x:Key="BrandBrush" Color="#FFFFA500"/>
    </Page.Resources>

    <StackPanel>
        <TextBlock Foreground="{StaticResource BrandBrush}" />
        <Button Content="Submit" Foreground="{StaticResource BrandBrush}" />
    </StackPanel>
</Page>
```

## <a name="minimize-overdrawing"></a>Minimizar el exceso de dibujo

El exceso de dibujo es donde se dibuja más de un objeto en los mismos píxeles de pantalla. Ten en cuenta que, a veces, todo se reduce a encontrar el equilibrio entre esta guía y el deseo de minimizar el número de elementos.

Usa [**DebugSettings.IsOverdrawHeatMapEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.debugsettings.isoverdrawheatmapenabled) como diagnóstico visual. Es posible que veas que se dibujan objetos que no sabías que habían aparecido en escena.

### <a name="transparent-or-hidden-elements"></a>Elementos transparentes u ocultos

Si un elemento no es visible porque es transparente o está oculto detrás de otros elementos y no contribuye al diseño, elimínalo. Si el elemento no es visible en el estado visual inicial, pero es visible en otros estados visuales, usa x:Load para controlar su estado o establece [Visibility](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.visibility) to **Contraída** en el propio elemento y cambia el valor a **Visible** en los estados apropiados. Encontrarás algunas excepciones a esta heurística: en general, el valor que una propiedad tiene en los principales estados visuales se establece mejor de forma local en el elemento.

### <a name="composite-elements"></a>Elementos compuestos

Usa un elemento compuesto en lugar de disponer varios elementos para crear un efecto. En este ejemplo, el resultado es una forma de dos tonos donde la mitad superior es negra (del fondo de [Grid](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid)) y la inferior es gris (blanco semitransparente de la clase [Rectangle](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) con combinación alfa sobre el fondo negro de **Grid**). Aquí, se rellena el 150 % de los píxeles necesarios para lograr el resultado.

**Ineficaz.**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<Grid Background="Black">
    <Grid.RowDefinitions>
        <RowDefinition Height="*"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <Rectangle Grid.Row="1" Fill="White" Opacity=".5"/>
</Grid>
```

**Eficaz.**

```xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="*"/>
        <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <Rectangle Fill="Black"/>
    <Rectangle Grid.Row="1" Fill="#FF7F7F7F"/>
</Grid>
```

### <a name="layout-panels"></a>Paneles de diseño

Un panel de diseño puede tener dos propósitos: colorear un área y diseñar elementos secundarios. Si un elemento de atrás correspondiente al orden z ya está coloreando un área, no será necesario que un panel de diseño de la parte frontal pinte esa área. En su lugar, puede centrarse en el diseño de los elementos secundarios. Aquí tienes un ejemplo.

**Ineficaz.**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<GridView Background="Blue">
    <GridView.ItemTemplate>
        <DataTemplate>
            <Grid Background="Blue"/>
        </DataTemplate>
    </GridView.ItemTemplate>
</GridView>
```

**Eficaz.**

```xaml
<GridView Background="Blue">
    <GridView.ItemTemplate>
        <DataTemplate>
            <Grid/>
        </DataTemplate>
    </GridView.ItemTemplate>
</GridView>
```

Si debes realizar la prueba de posicionamiento de [Grid](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid), te recomendamos que establezcas el valor de fondo "Transparent" en esta propiedad.

### <a name="borders"></a>Bordes

Usa un elemento [Border](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.border) para dibujar un borde alrededor de un objeto. En este ejemplo, se usa una clase [Grid](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid) como un borde provisional alrededor de una clase [TextBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox). Sin embargo, todos los píxeles en la celda central se dibujan sobre esta.

**Ineficaz.**

```xaml
<!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE. -->
<Grid Background="Blue" Width="300" Height="45">
    <Grid.RowDefinitions>
        <RowDefinition Height="5"/>
        <RowDefinition/>
        <RowDefinition Height="5"/>
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="5"/>
        <ColumnDefinition/>
        <ColumnDefinition Width="5"/>
    </Grid.ColumnDefinitions>
    <TextBox Grid.Row="1" Grid.Column="1"></TextBox>
</Grid>
```

**Eficaz.**

```xaml
 <Border BorderBrush="Blue" BorderThickness="5" Width="300" Height="45">
     <TextBox/>
</Border>
```

### <a name="margins"></a>Márgenes

Ten en cuenta los márgenes. Dos elementos vecinos se superpondrán (posiblemente accidentalmente) si los márgenes negativos de uno se extienden a lo largo de los límites de representación del otro y provocan un exceso de dibujo.

### <a name="cache-static-content"></a>Almacenar el contenido estático en caché

Otra fuente de exceso de dibujo es una forma realizada a partir de muchos elementos superpuestos. Si estableces [CacheMode](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.CacheMode) en **BitmapCache** en la clase [UIElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) que contiene la forma compuesta, la plataforma representará el elemento en un mapa de bits una vez y, a continuación, usará ese mapa de bits en cada fotograma en lugar de realizar el exceso de dibujo.

**Ineficaz.**

```xaml
<Canvas Background="White">
    <Ellipse Height="40" Width="40" Fill="Blue"/>
    <Ellipse Canvas.Left="21" Height="40" Width="40" Fill="Blue"/>
    <Ellipse Canvas.Top="13" Canvas.Left="10" Height="40" Width="40" Fill="Blue"/>
</Canvas>
```

![Diagrama de Venn con tres círculos sólidos](images/solidvenn.png)

La imagen anterior es el resultado, pero este es un mapa de las regiones que se dibujan más de una vez. El rojo oscuro indica una mayor cantidad de actividad de dibujo.

![Diagrama de Venn que muestra las áreas superpuestas](images/translucentvenn.png)

**Eficaz.**

```xaml
<Canvas Background="White" CacheMode="BitmapCache">
    <Ellipse Height="40" Width="40" Fill="Blue"/>
    <Ellipse Canvas.Left="21" Height="40" Width="40" Fill="Blue"/>
    <Ellipse Canvas.Top="13" Canvas.Left="10" Height="40" Width="40" Fill="Blue"/>
</Canvas>
```

Ten en cuenta el uso de [CacheMode](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.CacheMode). No uses esta técnica si alguna de las subformas es animada, ya que es probable que la memoria caché de mapas de bits se tenga que regenerar en cada fotograma, lo que anula este propósito.

## <a name="use-xbf2"></a>Usar XBF2

XBF2 es una representación binaria de marcado XAML que evita todos los costos de análisis de texto en tiempo de ejecución. También optimiza el código binario para la creación de la carga y el árbol, además de permitir rutas de acceso rápido para que los tipos XAML mejoren los costos de creación de montones y objetos, por ejemplo VSM, ResourceDictionary, Styles, etc. Se asigna completamente a la memoria, por lo que no se necesita superficie de montones para la carga y lectura de una página XAML. Además, reduce la superficie de disco de las páginas XAML almacenadas en un appx. XBF2 es una representación más compacta y puede reducir en hasta un 50 % la superficie de disco de los archivos XAML/XBF1 comparativos. Por ejemplo, la aplicación integrada Fotos vio una reducción de un 60 % tras la conversión a XBF, de aproximadamente 1 MB de activos XBF1 a 400 kB de activos XBF2. También hemos visto aplicaciones que reducen el uso de la CPU entre un 15 % y un 20 % y del montón de Win32 entre un 10 % y un 15 %.

Los controles y diccionarios integrados de XAML que proporciona el marco ya están totalmente habilitados para XBF2. Para tu propia aplicación, asegúrate de que el archivo de proyecto declare TargetPlatformVersion 8.2 o posterior.

Para comprobar si tienes XBF2, abre la aplicación en un editor de código binario; los bytes 12 y 13 son 00 02 si tienes XBF2.

## <a name="related-articles"></a>Artículos relacionados

- [Prácticas recomendadas para el rendimiento de inicio de la aplicación](best-practices-for-your-app-s-startup-performance.md)
- [Optimiza tu diseño XAML](optimize-your-xaml-layout.md)
- [Optimización de interfaz de usuario de ListView y GridView](optimize-gridview-and-listview.md)
- [Herramientas para la generación de perfiles y el rendimiento](tools-for-profiling-and-performance.md)
