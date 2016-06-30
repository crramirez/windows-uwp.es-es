---
author: mcleblanc
ms.assetid: 569E8C27-FA01-41D8-80B9-1E3E637D5B99
title: Optimizar el marcado XAML
description: "El análisis del marcado XAML para crear objetos en la memoria requiere mucho tiempo para una interfaz de usuario compleja. Estas son algunas acciones que puedes realizar para mejorar el análisis del marcado XAML, el tiempo de carga y la eficiencia de la memoria de tu aplicación."
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: c2131b084d8bb989f1f7767f54db697e1cdd8dcf

---
# Optimizar el marcado XAML

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

El análisis del marcado XAML para crear objetos en la memoria requiere mucho tiempo en una interfaz de usuario compleja. Estas son algunas acciones que puedes realizar para mejorar el análisis del marcado XAML, el tiempo de carga y la eficiencia de la memoria de tu aplicación.

Al iniciar la aplicación, limita el marcado XAML que se carga para que la interfaz de usuario inicial use solo lo que es necesario. Examina el marcado en la página inicial y confirma que contenga nada que no sea necesario. Si una página hace referencia a un control de usuario o un recurso definido en otro archivo, el marco también analizará ese archivo.

En este ejemplo, debido a que InitialPage.xaml usa un recurso de ExampleResourceDictionary.xaml, la totalidad de ExampleResourceDictionary.xaml debe analizarse en el inicio.

**InitialPage.xaml.**

```xml
<Page x:Class="ExampleNamespace.InitialPage" ...> 
    <UserControl.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <ResourceDictionary Source="ExampleResourceDictionary.xaml"/>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </UserControl.Resources>

    <Grid>
        <TextBox Foreground="{StaticResource TextBrush}"/>
    </Grid>
</Page>
```

**ExampleResourceDictionary.xaml.**

```xml
<ResourceDictionary>
    <SolidColorBrush x:Key="TextBrush" Color="#FF3F42CC"/>

    <!--This ResourceDictionary contains many other resources that
    used in the app, but not during startup.-->
</ResourceDictionary>
```

Si usas un recurso en muchas páginas en toda la aplicación, almacenarlo en App.xaml es una buena práctica y evita la duplicación. Sin embargo, App.xaml se analiza en el inicio de la aplicación por lo tanto, cualquier recurso que se use en únicamente una página (a menos que esa página sea la página inicial) se debe colocar en los recursos locales de la página. Este ejemplo de contador muestra App.xaml que contiene recursos usados en únicamente una página (que no es la página inicial). Esto aumenta innecesariamente el tiempo de inicio de la aplicación.

**InitialPage.xaml.**

```xml
<Page ...>  <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->   
    <StackPanel>  
        <TextBox Foreground="{StaticResource InitialPageTextBrush}"/> 
    </StackPanel> 
</Page> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

**SecondPage.xaml.**

```xml
<Page ...>  <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
    <StackPanel> 
        <Button Content="Submit" Foreground="{StaticResource SecondPageTextBrush}" /> 
    </StackPanel> 
</Page> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

**App.xaml**

```xml
<Application ...> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
     <Application.Resources>  
        <SolidColorBrush x:Key="DefaultAppTextBrush" Color="#FF3F42CC"/> 
        <SolidColorBrush x:Key="InitialPageTextBrush" Color="#FF3F42CC"/> 
        <SolidColorBrush x:Key="SecondPageTextBrush" Color="#FF3F42CC"/> 
        <SolidColorBrush x:Key="ThirdPageTextBrush" Color="#FF3F42CC"/> 
    </Application.Resources> 
</Application> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

La manera de hacer que el ejemplo anterior de contador sea más eficiente es mover `SecondPageTextBrush` a SecondPage.xaml y mover `ThirdPageTextBrush` a ThirdPage.xaml. `InitialPageTextBrush` puede permanecer en App.xaml porque los recursos de la aplicación deben analizarse en el inicio de la aplicación en cualquier caso.

## Minimizar el recuento de elementos

Si bien la plataforma XAML es capaz de mostrar un gran número de elementos, puedes hacer que tu aplicación diseñe y represente contenidos más rápido usando la menor cantidad de elementos para lograr los elementos visuales que quieras.

-   Los paneles de diseño tienen una propiedad [**Background**](https://msdn.microsoft.com/library/windows/apps/BR227512), por lo tanto, no es necesario colocar una clase [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371) delante de un panel solo para colorearlo.

**Ineficaz.**

```xml
<Grid> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
        <Rectangle Fill="Black"/> 
    </Grid> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

**Eficaz.**

```xml
<Grid Background="Black"/>
```

-   Si debes volver a usar el mismo elemento basado en vectores numerosas veces, te resultará más útil usar un elemento [**Image**](https://msdn.microsoft.com/library/windows/apps/BR242752) en su lugar. Los elementos basados en vectores pueden ser más engorrosos porque la CPU debe crear cada elemento individual por separado. Ten en cuenta que el archivo de imagen debe descodificarse una sola vez.

## Consolidar varios pinceles que tengan el mismo aspecto en un recurso

La plataforma XAML trata de almacenar en caché los objetos usados frecuentemente para que puedan volver a usarse siempre que sea posible. De todos modos, el lenguaje XAML no puede determinar fácilmente si un pincel declarado en una pieza de marcado es el mismo que está declarado en otra. Este ejemplo usa [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) como demostración, pero el caso resulta ser más probable y más importante mediante [**GradientBrush**](https://msdn.microsoft.com/library/windows/apps/BR210068).

**Ineficaz.**

```xml
<Page ... > <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
    <StackPanel>  
        <TextBlock>  
            <TextBlock.Foreground>  
                <SolidColorBrush Color="#FFFFA500"/> 
            </TextBlock.Foreground> 
        </TextBox> 
        <Button Content="Submit"> 
            <Button.Foreground> 
                <SolidColorBrush Color="#FFFFA500"/> 
            </Button.Foreground> 
        </Button> 
    </StackPanel> 
</Page> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

Busca también pinceles que usen los colores predefinidos: `"Orange"` y `"#FFFFA500"` son el mismo color. Para corregir la duplicación, define el pincel como un recurso. Si los controles de otras páginas usan el mismo pincel, muévelo a App.xaml.

**Eficaz.**

```xml
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

## Minimizar el exceso de dibujo

El exceso de dibujo es donde se dibuja más de un objeto en los mismos píxeles de pantalla. Ten en cuenta que, a veces, todo se reduce a encontrar el equilibrio entre esta guía y el deseo de minimizar el número de elementos.

-   Si un elemento no es visible porque es transparente o está oculto detrás de otros elementos y no contribuye al diseño, elimínalo. Si el elemento no es visible en el estado visual inicial, pero es visible en otros estados visuales, establece [**Visibility**](https://msdn.microsoft.com/library/windows/apps/BR208992) en **Collapsed** en el propio elemento y cambia el valor a **Visible** en los estados apropiados. Encontrarás algunas excepciones a esta heurística: en general, el valor que una propiedad tiene en los principales estados visuales se establece mejor de forma local en el elemento.
-   Usa un elemento compuesto en lugar de disponer varios elementos para crear un efecto. En este ejemplo, el resultado es una forma de dos tonos donde la mitad superior es negra (del fondo de [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704)) y la inferior es gris (blanco semitransparente de la clase [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371) con combinación alfa sobre el fondo negro de **Grid**). Aquí, se rellena el 150 % de los píxeles necesarios para lograr el resultado.

**Ineficaz.**
    
```xml
    <Grid Background="Black"> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
        <Grid.RowDefinitions> 
            <RowDefinition Height="*"/> 
            <RowDefinition Height="*"/> 
        </Grid.RowDefinitions> 
        <Rectangle Grid.Row="1" Fill="White" Opacity=".5"/> 
    </Grid> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

**Eficaz.**

```xml
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        <Rectangle Fill="Black"/>
        <Rectangle Grid.Row="1" Fill="#FF7F7F7F"/>
    </Grid>
```

-   Un panel de diseño puede tener dos propósitos: colorear un área y diseñar elementos secundarios. Si un elemento de atrás correspondiente al orden z ya está coloreando un área, no será necesario que un panel de diseño de la parte frontal pinte esa área. En su lugar, puede centrarse en el diseño de los elementos secundarios. A continuación te mostramos un ejemplo.

**Ineficaz.**

```xml
    <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
    <GridView Background="Blue">  
        <GridView.ItemTemplate> 
            <DataTemplate> 
                <Grid Background="Blue"/> 
            </DataTemplate> 
        </GridView.ItemTemplate> 
    </GridView> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

**Eficaz.**

```xml
    <GridView Background="Blue">  
        <GridView.ItemTemplate> 
            <DataTemplate> 
                <Grid/> 
            </DataTemplate>
        </GridView.ItemTemplate>
    </GridView> 
```

Si debes realizar la prueba de posicionamiento de [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704), te recomendamos que establezcas el valor de fondo "Transparent" en esta propiedad.

-   Usa un elemento [**Border**](https://msdn.microsoft.com/library/windows/apps/BR209253) para dibujar un borde alrededor de un objeto. En este ejemplo, se usa una clase [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704) como un borde provisional alrededor de una clase [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683). Sin embargo, todos los píxeles en la celda central se dibujan sobre esta.

**Ineficaz.**

```xml
    <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
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
    </Grid> <!-- NOTE: EXAMPLE OF INEFFICIENT CODE; DO NOT COPY-PASTE.-->
```

**Eficaz.**

```xml
    <Border BorderBrush="Blue" BorderThickness="5" Width="300" Height="45">
        <TextBox/>
    </Border>
```

-   Ten en cuenta los márgenes. Dos elementos vecinos se superpondrán (posiblemente accidentalmente) si los márgenes negativos de uno se extienden a lo largo de los límites de representación del otro y provocan un exceso de dibujo.

Usa [**DebugSettings.IsOverdrawHeatMapEnabled**](https://msdn.microsoft.com/library/windows/apps/Hh701823) como diagnóstico visual. Es posible que veas que se dibujan objetos que antes no habían aparecido en escena.

## Almacenar el contenido estático en caché

Otra fuente de exceso de dibujo es una forma realizada a partir de muchos elementos superpuestos. Si estableces [**CacheMode**](https://msdn.microsoft.com/library/windows/apps/BR228084) en **BitmapCache** en la clase [**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911) que contiene la forma compuesta, la plataforma representará el elemento en un mapa de bits una vez y, a continuación, usará ese mapa de bits en cada fotograma en lugar de realizar el exceso de dibujo.

**Ineficaz.**

```xml
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

```xml
<Canvas Background="White" CacheMode="BitmapCache">
    <Ellipse Height="40" Width="40" Fill="Blue"/>
    <Ellipse Canvas.Left="21" Height="40" Width="40" Fill="Blue"/>
    <Ellipse Canvas.Top="13" Canvas.Left="10" Height="40" Width="40" Fill="Blue"/>
</Canvas>
```

Ten en cuenta el uso de [**CacheMode**](https://msdn.microsoft.com/library/windows/apps/BR228084). No uses esta técnica si alguna de las subformas es animada, ya que es probable que la memoria caché de mapas de bits se tenga que regenerar en cada fotograma, lo que anula este propósito.

## ResourceDictionaries

Los objetos ResourceDictionaries generalmente se usan para almacenar los recursos en un nivel algo global. Los recursos a los que la aplicación quiere hacer referencia en varios lugares. Por ejemplo, estilos, pinceles, plantillas, etc. En general, hemos optimizado el objeto ResourceDictionaries para no crear instancias de los recursos, a menos que se pidan. Sin embargo, hay varios lugares en los que debes tener algo de cuidado.

**Recurso con x:Name**. Los recursos con x:Name no sacarán partido de la optimización de la plataforma, En cambio, se creará una instancia de ellos en cuanto se cree el objeto ResourceDictionary. Esto sucede porque x:Name le indica a la plataforma que tu aplicación necesita acceso de campo a este recurso, por lo que debe crear algo a que hacer referencia.

**ResourceDictionaries en un elemento UserControl**. Los objetos ResourceDictionaries definidos en un objeto UserControl resultan en una penalización. La plataforma creará una copia de este tipo de objeto ResourceDictionary para todas las instancias del objeto UserControl. Si tienes un objeto UserControl que se usa con mucha frecuencia, saca el objeto ResourceDictionary del objeto UserControl y ponlo en el nivel de página.

## Usar XBF2

XBF2 es una representación binaria de marcado XAML que evita todos los costos de análisis de texto en tiempo de ejecución. También optimiza el código binario para la creación de la carga y el árbol, además de permitir rutas de acceso rápido para que los tipos XAML mejoren los costos de creación de montones y objetos, por ejemplo VSM, ResourceDictionary, Styles, etc. Se asigna completamente a la memoria, por lo que no se necesita superficie de montones para la carga y lectura de una página XAML. Además, reduce la superficie de disco de las páginas XAML almacenadas en un appx. XBF2 es una representación más compacta y puede reducir en hasta un 50 % la superficie de disco de los archivos XAML/XBF1 comparativos. Por ejemplo, la aplicación integrada Fotos vio una reducción de un 60 % tras la conversión a XBF, de aproximadamente 1 MB de activos XBF1 a 400 kB de activos XBF2. También hemos visto aplicaciones que reducen el uso de la CPU entre un 15 % y un 20 % y del montón de Win32 entre un 10 % y un 15 %.

Los controles y diccionarios integrados de XAML que proporciona el marco ya están totalmente habilitados para XBF2. Para tu propia aplicación, asegúrate de que el archivo de proyecto declare TargetPlatformVersion 8.2 o posterior.

Para comprobar si tienes XBF2, abre la aplicación en un editor de código binario; los bytes 12 y 13 son 00 02 si tienes XBF2.




<!--HONumber=Jun16_HO4-->


