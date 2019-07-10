---
title: Creación de estilos personalizados
description: En este artículo se explican los conceptos básicos de la aplicación de estilos a los elementos de la interfaz de usuario en XAML
keywords: XAML, UWP, Getting Started
ms.date: 08/31/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d540b41620110a41676d08f5e6239efd0ef4ca46
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66361233"
---
# <a name="tutorial-create-custom-styles"></a>Tutorial: Creación de estilos personalizados

Este tutorial muestra cómo personalizar la interfaz de usuario de nuestra aplicación XAML. Advertencia: este tutorial podría implicar a un unicornio. (¡Lo hace!)  

## <a name="prerequisites"></a>Requisitos previos
* [Visual Studio 2017 y el SDK de Windows 10 (10.0.15063.468 o posterior)](https://developer.microsoft.com/windows/downloads)

## <a name="part-0-get-the-code"></a>Parte 0: Obtener el código
El punto de partida de este laboratorio se encuentra en el repositorio de ejemplo PhotoLab, en la [carpeta xaml-basics-starting-points/style/](https://github.com/Microsoft/Windows-appsample-photo-lab/tree/master/xaml-basics-starting-points/style). Después de clonar o descargar el repositorio, puedes editar el proyecto abriendo PhotoLab.sln con Visual Studio 2017.

La aplicación PhotoLab tiene dos páginas principales:

**MainPage.xaml:** muestra una vista de galería de fotos, junto con información acerca de cada archivo de imagen.
![MainPage](../basics/images/xaml-basics/mainpage.png)

**DetailPage.xaml:** muestra una sola foto después de que se haya seleccionado. Un menú de edición flotante permite modificar la foto, cambiar su nombre y guardarla.
![DetailPage](../basics/images/xaml-basics/detailpage.png)

## <a name="part-1-create-a-fancy-slider-control"></a>1ª parte: Creación de un control deslizante sofisticado  

La Plataforma universal de Windows (UWP) proporciona varias maneras de personalizar la apariencia de una aplicación. Desde la configuración de fuentes y tipografía hasta los colores y gradientes o los efectos de desenfoque, hay muchas opciones. 

Para la primera parte del tutorial, vamos a animar algunos de los controles de edición de fotos. 

<figure>
    <img src="../basics/images/xaml-basics/slider-start.png" />
    <figure>*Un sencillo control deslizante con el estilo predeterminado.*</figure>
</figure>

Estos controles deslizantes son útiles, hacen todas las cosas que debiera hacer un control deslizante, pero no son muy sofisticados. Vamos a solucionarlo. 

El control deslizante de exposición ajusta la exposición de la imagen: si se desliza hacia la izquierda, se oscurecerá la imagen, mientras que si se desliza hacia la derecha, se aclarará. Vamos a hacer que el control deslizante sea más guay. Para ello, haremos que el fondo vaya del negro al blanco. De esta forma no solo mejorará la apariencia del control deslizante, lo que es estupendo, sino que también proporcionará una pista visual sobre la funcionalidad que proporciona el control deslizante.

### <a name="customize-a-slider-control"></a>Personalización de un control deslizante

<!-- TODO: Update folder -->
1. Tras descargar el repositorio, abre **PhotoLab.sln**, que se encuentra en la carpeta xaml-basics-starting-points/style/, e indica que la plataforma de soluciones es x86 o x64 (no ARM). 

    Presiona F5 para compilar y ejecutar la aplicación. La primera pantalla muestra una galería de imágenes. Haz clic en una de ellas para ir a su página de detalles. Una vez allí, haz clic en el botón Editar para ver los controles de edición sobre los que trabajaremos. Sal de la aplicación y vuelve a Visual Studio.  

2. En el panel del Explorador de soluciones, haz doble clic en **DetailPage.xaml** para abrirlo. 

    ![El archivo DetailPage.xaml en el Explorador de soluciones de Visual Studio 2017.](../basics/images/xaml-basics/style-detail-page-explorer.png)

3. Usa un elemento Polígono para crear una forma de fondo para el control deslizante de exposición.

    El [espacio de nombres Windows.XAML.Ui.Shapes](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Shapes) proporciona siete formas para elegir. Hay una elipse, un rectángulo y un elemento denominado Trazado, que puede hacer cualquier tipo de forma (hasta un unicornio). 
    
    <!-- TODO reduce size -->
    ![Un unicornio](../basics/images/xaml-basics/unicorn.png)
    
    > **Información al respecto:** en el artículo [Dibujo de formas](https://docs.microsoft.com/en-us/windows/uwp/graphics/drawing-shapes) se explica todo lo necesario acerca de las formas XAML. 
    
    Queremos crear un widget con aspecto de triángulo, algo parecido a la forma que verías en el control de volumen de un equipo de música estéreo.
    
    ![Control deslizante de volumen](../basics/images/xaml-basics/style-volume-slider.png)
    
    Parece una tarea para la forma Polígono. Para definir un polígono, especifica un conjunto de puntos y asígnale un relleno. Vamos a crear un polígono que sea aproximadamente de 200 píxeles de ancho y 20 píxeles de alto, con un relleno degradado.
    
    En DetailPage.xaml, busca el código del control deslizante de exposición y luego crea un elemento Polígono inmediatamente antes: 

    * Establece **Grid.Row** en "2" para colocar el polígono en la misma fila que el control deslizante de exposición. 
    * Establece la propiedad **Points** en "0,20 200,20 200,0" para definir la forma del triángulo.
    * Establece la propiedad **Stretch** en "Relleno" y la propiedad **HorizontalAlignment** en "Ajustar".
    * Establece la propiedad **Height** en "20" y la **VerticalAlignment** en "Centro". 
    * Asigna al **Polígono** un relleno con degradado lineal.     
    * En el control deslizante de exposición, establece la propiedad **Foreground** en "Transparente", con el fin de que puedas ver el polígono. 

    **Antes**
    ```xaml
    <Slider Header="Exposure"
        Grid.Row="2"
        Value="{x:Bind item.Exposure, Mode=TwoWay}"
        Minimum="-2"
        Maximum="2" />
    ```
    **Después**
    ```xaml
    <Polygon Grid.Row="2" Stretch="Fill"
                Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"  
                VerticalAlignment="Center" Height="20">
        <Polygon.Fill>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Black" />
                    <GradientStop Offset="1" Color="White" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Polygon.Fill>
    </Polygon>
    <Slider Header="Exposure" 
        Grid.Row="2" 
        Foreground="Transparent"
        Value="{x:Bind item.Exposure, Mode=TwoWay}"
        Minimum="-2"
        Maximum="2" />
    ```

    Notas:
    * Si observas el XAML circundante, verás que estos elementos están en una cuadrícula. Colocamos el polígono en la misma fila que el control deslizante de exposición (Grid.Row="2") para que aparezcan en el mismo punto. Colocamos el polígono delante del control deslizante para que este último represente la parte superior de la forma.
    * Establecemos Stretch="Fill" y HorizontalAlignment="Stretch" en el polígono, con el fin de que el triángulo se ajuste para rellenar el espacio disponible. Si el ancho del control deslizante aumenta o disminuye, el polígono crecerá o se encogerá en consecuencia. 

4. Compila la aplicación y ejecútala. La apariencia del control deslizante debería ser impresionante:

    ![Control deslizante de exposición sofisticado](../basics/images/xaml-basics/style-exposure-slider-done.png)

5. Vamos a actualizar el siguiente control deslizante, el de temperatura. Este control deslizante cambia la temperatura del color de la imagen; si se desliza hacia la izquierda, la imagen es más azul, mientras que se si se desliza hacia la derecha, la imagen es más amarilla.

    Vamos a utilizar otro polígono para esta forma del fondo con las mismas dimensiones que el anterior, pero esta vez pondremos como relleno un degradado de azul a amarillo, en lugar de blanco y negro. 

    **Antes**
    ```xaml
    <TextBlock Grid.Row="2"
                Grid.Column="1"
                 Margin="10,8,0,0" VerticalAlignment="Center" Padding="0"
                Text="{x:Bind item.Exposure.ToString('N', culture), Mode=OneWay}" />
                
    <Slider Header="Temperature"
            Grid.Row="3" Background="Transparent" Foreground="Transparent"
            Value="{x:Bind item.Temperature, Mode=TwoWay}"
            Minimum="-1"
            Maximum="1" />
    ```
    **Después**
    ```xaml
    <TextBlock Grid.Row="2"
                Grid.Column="1"
                Margin="10,8,0,0" VerticalAlignment="Center" Padding="0"
                Text="{x:Bind item.Exposure.ToString('N', culture), Mode=OneWay}" />         
                
    <Polygon Grid.Row="3" Stretch="Fill"
                Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"  
                VerticalAlignment="Center" Height="20">
        <Polygon.Fill>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Blue" />
                    <GradientStop Offset="1" Color="Yellow" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Polygon.Fill>
    </Polygon>
    <Slider Header="Temperature"
            Grid.Row="3" Background="Transparent" Foreground="Transparent"
            Value="{x:Bind item.Temperature, Mode=TwoWay}"
            Minimum="-1"
            Maximum="1" />
    ```

6. Compila la aplicación y ejecútala. Ahora deberías tener dos controles deslizantes decorativos.

    ![Dos controles deslizantes decorativos](../basics/images/xaml-basics/style-2sliders-done.png)

7. **Crédito adicional**

    Agrega una forma de fondo para el control deslizante de tinte que tenga un degradado de verde a rojo. 

    ![Tres controles deslizantes decorativos](../basics/images/xaml-basics/style-3sliders-done.png)


Enhorabuena, has completado la parte 1. Si te has quedado atascado o quieres ver la solución final, puedes encontrar el código completo en **UWP Academy\XAML\Styling\Part1\Finish**.

 
    
## <a name="part-2-create-basic-styles"></a>2ª parte: Creación de estilos básicos

Una de las ventajas de los estilos de XAML es que pueden reducir considerablemente la cantidad de código que hay que escribir, y pueden facilitar mucho la actualización del aspecto de la aplicación.

Para definir un estilo, agrega un elemento [Style](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style) a la propiedad [Resources](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.frameworkelement.Resources) de un elemento que contenga el control al que quieres aplicar estilo.  Si agregas el estilo a la propiedad **Page.Resources**, se podrá acceder tus estilos desde toda la página. Si agregas el estilo a la propiedad **Application.Resources** en el archivo App.xaml, se podrá acceder a él desde toda la aplicación.

Puedes crear tanto estilos con nombre como estilos generales. Los estilos con nombre deben aplicarse explícitamente a controles concretos, mientras que los estilos generales se aplican a cualquier control que coincida con el **TargetType** especificado. 

En este ejemplo, el primer estilo tiene un atributo **x:Key** y su tipo de destino es **Botón**. La propiedad **Style** del primer botón se establece en esta clave, por lo que este estilo es un estilo con nombre y debe aplicarse explícitamente. El segundo estilo se aplica automáticamente al segundo botón porque su tipo de destino es **Botón** y el estilo no tiene un atributo **x:Key**.


```XAML
<Page.Resources>
    <Style x:Key="PurpleStyle" TargetType="Button">
        <Setter Property="FontFamily" Value="Lucida Sans Unicode"/>
        <Setter Property="FontStyle" Value="Italic"/>
        <Setter Property="FontSize" Value="14"/>
        <Setter Property="Foreground" Value="MediumOrchid"/>
    </Style>

    <Style TargetType="Button">
        <Setter Property="Foreground" Value="Orange"/>
    </Style>
</Page.Resources>

<Grid x:Name="LayoutRoot">
    <Button Content="Button" Style="{StaticResource PurpleStyle}"/>
    <Button Content="Button" />
</Grid>
```

Vamos a agregar un estilo a la aplicación. En DetailsPage.xaml, echa un vistazo a los bloques de texto situados junto a nuestros controles deslizantes de exposición, temperatura y tinte. Cada uno de estos bloques de texto muestra el valor de un control deslizante. Este es el bloque de texto del control deslizante de exposición. Ten en cuenta que las propiedades **Margin**, **VerticalAlignment** y **Padding** están establecidas.

```XAML
<TextBlock Grid.Row="2"
            Grid.Column="1"
            Margin="10,8,0,0" VerticalAlignment="Center" Padding="0"
            Text="{x:Bind item.Exposure.ToString('N', culture), Mode=OneWay}" />
```
Mira los demás bloques de texto, observa que esas mismas propiedades se establecen en los mismos valores. Parece un buen candidato para un estilo...

### <a name="create-a-value-text-block-style"></a>Creación de un estilo de bloque de texto con valor

<!-- TODO: add second starting point -->
1. Abre DetailsPage.xaml.

2. Busca el control **Grid** denominado **EditControlsGrid**. Contiene nuestros los controles deslizantes y cuadros de texto. Observa que la cuadrícula ya define un estilo para los controles deslizantes. 

    ```XAML
    <Grid x:Name="EditControlsGrid"
            HorizontalAlignment="Stretch"
            Margin="24,48,24,24">
        <Grid.Resources>
            <Style TargetType="Slider">
                <Setter Property="Margin"
                        Value="0,0,0,0" />
                <Setter Property="Padding"
                        Value="0" />
                <Setter Property="MinWidth"
                        Value="100" />
                <Setter Property="StepFrequency"
                        Value="0.1" />
                <Setter Property="TickFrequency"
                        Value="0.1" />
            </Style>
        </Grid.Resources>    
    ```
3. Crea un estilo para **TextBlock** que establezca el valor de **Margin** en "10,8,0,0"; el de **VerticalAlignment** en "Centro" y el de **Padding** en "0".

    **Antes**
    ```XAML
        <Grid.Resources>
            <Style TargetType="Slider">
                <Setter Property="Margin"
                        Value="0,0,0,0" />
                <Setter Property="Padding"
                        Value="0" />
                <Setter Property="MinWidth"
                        Value="100" />
                <Setter Property="StepFrequency"
                        Value="0.1" />
                <Setter Property="TickFrequency"
                        Value="0.1" />
            </Style>                           
        </Grid.Resources>
    ```

    **Después**
    ```XAML
        <Grid.Resources>
            <Style TargetType="Slider">
                <Setter Property="Margin"
                        Value="0,0,0,0" />
                <Setter Property="Padding"
                        Value="0" />
                <Setter Property="MinWidth"
                        Value="100" />
                <Setter Property="StepFrequency"
                        Value="0.1" />
                <Setter Property="TickFrequency"
                        Value="0.1" />
            </Style>
            <Style TargetType="TextBlock">
                <Setter Property="Margin"
                        Value="10,8,0,0" />
                <Setter Property="VerticalAlignment"
                        Value="Center" />
                <Setter Property="Padding"
                        Value="0" />
            </Style>                            
        </Grid.Resources>
    ```    

4. Vamos a hacer que esto sea un estilo con nombre, dcdon el fin de poder especificar a qué controles de **TextBlock** se aplica. Establece la propiedad **x:Key** del estilo en "ValueTextBox". 

    **Antes**
    ```XAML
            <Style TargetType="TextBlock">
                <Setter Property="Margin"
                        Value="10,8,0,0" />
                <Setter Property="VerticalAlignment"
                        Value="Center" />
                <Setter Property="Padding"
                        Value="0" />
            </Style>                            
    ```    

    **Después**
    ```XAML
            <Style TargetType="TextBlock"
                   x:Key="ValueTextBox">
                <Setter Property="Margin"
                        Value="10,8,0,0" />
                <Setter Property="VerticalAlignment"
                        Value="Center" />
                <Setter Property="Padding"
                        Value="0" />
            </Style>                            
    ```    

5. En todos los controles de **TextBlock**, quita sus propiedades **Margin**, **VerticalAlignment** y **Padding**, y establece su propiedad **Style** en "{StaticResource ValueTextBox}".

    **Antes**
    ```XAML
     <TextBlock Grid.Row="2"
                Grid.Column="1"
                Margin="10,8,0,0" VerticalAlignment="Center" Padding="0"
                Text="{x:Bind item.Exposure.ToString('N', culture), Mode=OneWay}" />   
    ```

    **Después**
    ```XAML
     <TextBlock Grid.Row="2"
                Grid.Column="1"
                Style="{StaticResource ValueTextBox}"
                Text="{x:Bind item.Exposure.ToString('N', culture), Mode=OneWay}" />   
    ```    

    Realiza este cambio en los seis controles TextBlock asociados con los controles deslizantes.

6. Compila la aplicación y ejecútala. Debería tener el mismo aspecto. A pesar de todo, deberías disfrutar de ese maravilloso sentido de satisfacción que se obtiene al escribir código eficaz y fácil de mantener.

<!-- TODO add new start/end points -->
Enhorabuena, has completado la parte 2.


## <a name="part-3-use-a-control-template-to-make-a-fancy-slider"></a>3\.ª parte: Uso de una plantilla de control para crear un bonito control deslizante

¿Recuerdas que en la parte 1 agregamos una forma detrás del control deslizante para darle un aspecto atractivo?

Eso ya lo hicimos, pero hay una forma mejor de lograr el mismo efecto: crear una plantilla de control. 

<!-- TODO add new starting points -->
1. En el panel del Explorador de soluciones, haz doble clic en **DetailPage.xaml**.

2. A continuación, usaremos la plantilla de control predeterminada para el control deslizante como punto de partida. Agrega este XAML al elemento **Page.Resources**. (el elemento **Page.Resources** está hacia el principio de la página).

    ```XAML
    <ControlTemplate x:Key="FancySliderControlTemplate" TargetType="Slider">
        <Grid Margin="{TemplateBinding Padding}">
            <Grid.Resources>
                <Style TargetType="Thumb" x:Key="SliderThumbStyle">
                    <Setter Property="BorderThickness" Value="0" />
                    <Setter Property="Background" Value="{ThemeResource SliderThumbBackground}" />
                    <Setter Property="Template">
                        <Setter.Value>
                            <ControlTemplate TargetType="Thumb">
                                <Border Background="{TemplateBinding Background}"
                                            BorderBrush="{TemplateBinding BorderBrush}"
                                            BorderThickness="{TemplateBinding BorderThickness}"
                                            CornerRadius="4" />
                            </ControlTemplate>
                        </Setter.Value>
                    </Setter>
                </Style>
            </Grid.Resources>
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto" />
                <RowDefinition Height="*" />
            </Grid.RowDefinitions>
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup x:Name="CommonStates">
                    <VisualState x:Name="Normal" />
                    <VisualState x:Name="Pressed">
                        <Storyboard>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalTrackRect" Storyboard.TargetProperty="Fill">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackFillPressed}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalTrackRect" Storyboard.TargetProperty="Fill">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackFillPressed}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalThumb" Storyboard.TargetProperty="Background">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderThumbBackgroundPressed}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalThumb" Storyboard.TargetProperty="Background">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderThumbBackgroundPressed}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="SliderContainer" Storyboard.TargetProperty="Background">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderContainerBackgroundPressed}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalDecreaseRect" Storyboard.TargetProperty="Fill">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackValueFillPressed}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalDecreaseRect" Storyboard.TargetProperty="Fill">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackValueFillPressed}" />
                            </ObjectAnimationUsingKeyFrames>
                        </Storyboard>
                    </VisualState>
                    <VisualState x:Name="Disabled">
                        <Storyboard>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HeaderContentPresenter" Storyboard.TargetProperty="Foreground">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderHeaderForegroundDisabled}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalDecreaseRect" Storyboard.TargetProperty="Fill">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackValueFillDisabled}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalTrackRect" Storyboard.TargetProperty="Fill">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackFillDisabled}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalDecreaseRect" Storyboard.TargetProperty="Fill">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackValueFillDisabled}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalTrackRect" Storyboard.TargetProperty="Fill">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackFillDisabled}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalThumb" Storyboard.TargetProperty="Background">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderThumbBackgroundDisabled}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalThumb" Storyboard.TargetProperty="Background">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderThumbBackgroundDisabled}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="TopTickBar" Storyboard.TargetProperty="Fill">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTickBarFillDisabled}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="BottomTickBar" Storyboard.TargetProperty="Fill">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTickBarFillDisabled}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="LeftTickBar" Storyboard.TargetProperty="Fill">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTickBarFillDisabled}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="RightTickBar" Storyboard.TargetProperty="Fill">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTickBarFillDisabled}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="SliderContainer" Storyboard.TargetProperty="Background">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderContainerBackgroundDisabled}" />
                            </ObjectAnimationUsingKeyFrames>
                        </Storyboard>
                    </VisualState>
                    <VisualState x:Name="PointerOver">
                        <Storyboard>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalTrackRect" Storyboard.TargetProperty="Fill">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackFillPointerOver}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalTrackRect" Storyboard.TargetProperty="Fill">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackFillPointerOver}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalThumb" Storyboard.TargetProperty="Background">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderThumbBackgroundPointerOver}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalThumb" Storyboard.TargetProperty="Background">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderThumbBackgroundPointerOver}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="SliderContainer" Storyboard.TargetProperty="Background">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderContainerBackgroundPointerOver}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalDecreaseRect" Storyboard.TargetProperty="Fill">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackValueFillPointerOver}" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalDecreaseRect" Storyboard.TargetProperty="Fill">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="{ThemeResource SliderTrackValueFillPointerOver}" />
                            </ObjectAnimationUsingKeyFrames>
                        </Storyboard>
                    </VisualState>
                </VisualStateGroup>
                <VisualStateGroup x:Name="FocusEngagementStates">
                    <VisualState x:Name="FocusDisengaged" />
                    <VisualState x:Name="FocusEngagedHorizontal">
                        <Storyboard>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="SliderContainer" Storyboard.TargetProperty="(Control.IsTemplateFocusTarget)">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="False" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="HorizontalThumb" Storyboard.TargetProperty="(Control.IsTemplateFocusTarget)">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="True" />
                            </ObjectAnimationUsingKeyFrames>
                        </Storyboard>
                    </VisualState>
                    <VisualState x:Name="FocusEngagedVertical">
                        <Storyboard>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="SliderContainer" Storyboard.TargetProperty="(Control.IsTemplateFocusTarget)">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="False" />
                            </ObjectAnimationUsingKeyFrames>
                            <ObjectAnimationUsingKeyFrames Storyboard.TargetName="VerticalThumb" Storyboard.TargetProperty="(Control.IsTemplateFocusTarget)">
                                <DiscreteObjectKeyFrame KeyTime="0" Value="True" />
                            </ObjectAnimationUsingKeyFrames>
                        </Storyboard>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
            <ContentPresenter x:Name="HeaderContentPresenter"
                        x:DeferLoadStrategy="Lazy"
                        Visibility="Collapsed"
                        Foreground="{ThemeResource SliderHeaderForeground}"
                        Margin="{ThemeResource SliderHeaderThemeMargin}"
                        Content="{TemplateBinding Header}"
                        ContentTemplate="{TemplateBinding HeaderTemplate}"
                        FontWeight="{ThemeResource SliderHeaderThemeFontWeight}"
                        TextWrapping="Wrap" />
            <Grid x:Name="SliderContainer"
                        Background="{ThemeResource SliderContainerBackground}"
                        Grid.Row="1"
                        Control.IsTemplateFocusTarget="True">
                <Grid x:Name="HorizontalTemplate" MinHeight="44">
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="Auto" />
                        <ColumnDefinition Width="Auto" />
                        <ColumnDefinition Width="*" />
                    </Grid.ColumnDefinitions>
                    <Grid.RowDefinitions>
                        <RowDefinition Height="18" />
                        <RowDefinition Height="Auto" />
                        <RowDefinition Height="18" />
                    </Grid.RowDefinitions>
                    <Rectangle x:Name="HorizontalTrackRect"
                                Fill="{TemplateBinding Background}"
                                Height="{ThemeResource SliderTrackThemeHeight}"
                                Grid.Row="1"
                                Grid.ColumnSpan="3" />
                    <Rectangle x:Name="HorizontalDecreaseRect" Fill="{TemplateBinding Foreground}" Grid.Row="1" />
                    <TickBar x:Name="TopTickBar"
                                Visibility="Collapsed"
                                Fill="{ThemeResource SliderTickBarFill}"
                                Height="{ThemeResource SliderOutsideTickBarThemeHeight}"
                                VerticalAlignment="Bottom"
                                Margin="0,0,0,4"
                                Grid.ColumnSpan="3" />
                    <TickBar x:Name="HorizontalInlineTickBar"
                                Visibility="Collapsed"
                                Fill="{ThemeResource SliderInlineTickBarFill}"
                                Height="{ThemeResource SliderTrackThemeHeight}"
                                Grid.Row="1"
                                Grid.ColumnSpan="3" />
                    <TickBar x:Name="BottomTickBar"
                                Visibility="Collapsed"
                                Fill="{ThemeResource SliderTickBarFill}"
                                Height="{ThemeResource SliderOutsideTickBarThemeHeight}"
                                VerticalAlignment="Top"
                                Margin="0,4,0,0"
                                Grid.Row="2"
                                Grid.ColumnSpan="3" />
                    <Thumb x:Name="HorizontalThumb"
                                Style="{StaticResource SliderThumbStyle}"
                                DataContext="{TemplateBinding Value}"
                                Height="24"
                                Width="8"
                                Grid.Row="0"
                                Grid.RowSpan="3"
                                Grid.Column="1"
                                FocusVisualMargin="-14,-6,-14,-6"
                                AutomationProperties.AccessibilityView="Raw" />
                </Grid>
                <Grid x:Name="VerticalTemplate" MinWidth="44" Visibility="Collapsed">
                    <Grid.RowDefinitions>
                        <RowDefinition Height="*" />
                        <RowDefinition Height="Auto" />
                        <RowDefinition Height="Auto" />
                    </Grid.RowDefinitions>
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="18" />
                        <ColumnDefinition Width="Auto" />
                        <ColumnDefinition Width="18" />
                    </Grid.ColumnDefinitions>
                    <Rectangle x:Name="VerticalTrackRect"
                                Fill="{TemplateBinding Background}"
                                Width="{ThemeResource SliderTrackThemeHeight}"
                                Grid.Column="1"
                                Grid.RowSpan="3" />
                    <Rectangle x:Name="VerticalDecreaseRect"
                                Fill="{TemplateBinding Foreground}"
                                Grid.Column="1"
                                Grid.Row="2" />
                    <TickBar x:Name="LeftTickBar"
                                Visibility="Collapsed"
                                Fill="{ThemeResource SliderTickBarFill}"
                                Width="{ThemeResource SliderOutsideTickBarThemeHeight}"
                                HorizontalAlignment="Right"
                                Margin="0,0,4,0"
                                Grid.RowSpan="3" />
                    <TickBar x:Name="VerticalInlineTickBar"
                                Visibility="Collapsed"
                                Fill="{ThemeResource SliderInlineTickBarFill}"
                                Width="{ThemeResource SliderTrackThemeHeight}"
                                Grid.Column="1"
                                Grid.RowSpan="3" />
                    <TickBar x:Name="RightTickBar"
                                Visibility="Collapsed"
                                Fill="{ThemeResource SliderTickBarFill}"
                                Width="{ThemeResource SliderOutsideTickBarThemeHeight}"
                                HorizontalAlignment="Left"
                                Margin="4,0,0,0"
                                Grid.Column="2"
                                Grid.RowSpan="3" />
                    <Thumb x:Name="VerticalThumb"
                                Style="{StaticResource SliderThumbStyle}"
                                DataContext="{TemplateBinding Value}"
                                Width="24"
                                Height="8"
                                Grid.Row="1"
                                Grid.Column="0"
                                Grid.ColumnSpan="3"
                                FocusVisualMargin="-6,-14,-6,-14"
                                AutomationProperties.AccessibilityView="Raw" />
                </Grid>
            </Grid>
        </Grid>
    </ControlTemplate>
    ```

    El XAML es muy grande. Las plantillas de control son una característica potente, pero pueden ser bastante complejas, por el que normalmente se recomienda empezar con la plantilla predeterminada. 
    
3. En la plantilla **ControlTemplate** que acabas de agregar, busca el control de cuadrícula denominado **HorizontalTemplate**. Esta cuadrícula define la parte de la plantilla que queremos cambiar.

    ```XAML
    <Grid x:Name="HorizontalTemplate" MinHeight="44">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="18" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="18" />
        </Grid.RowDefinitions>
    ```

5.  Crea un polígono que sea como el que creaste para el control deslizante de exposición en la parte 1. Agrega el polígono después de la etiqueta de cierre **Grid.RowDefinitions**. Establece **Grid.Row** en "0", **Grid.RowSpan** en "3" y **Grid.ColumnSpan** en "3". 

    **Antes**
    ```XAML
    <Grid x:Name="HorizontalTemplate" MinHeight="44">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="18" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="18" />
        </Grid.RowDefinitions>        
    ```

    **Después**
    ```XAML
    <Grid x:Name="HorizontalTemplate" MinHeight="44">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="18" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="18" />
        </Grid.RowDefinitions>
        <Polygon Grid.Row="0" Grid.RowSpan="3"  Grid.ColumnSpan="3" Stretch="Fill"
                    Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"  
                    VerticalAlignment="Center" Height="20" >
            <Polygon.Fill>
                <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                    <LinearGradientBrush.GradientStops>
                        <GradientStop Offset="0" Color="Black" />
                        <GradientStop Offset="1" Color="White" />
                    </LinearGradientBrush.GradientStops>
                </LinearGradientBrush>
            </Polygon.Fill>
        </Polygon>           
    ```

6. Quita el valor **Polygon.Fill**. Establece **Fill** en "{TemplateBinding Background}". Esto hace que al establecer la propiedad **Background** del control deslizante se establezca la propiedad **Fill** del polígono. 

    **Antes**
    ```XAML
        <Polygon Grid.Row="0" Grid.RowSpan="3"  Grid.ColumnSpan="3" Stretch="Fill"
                    Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"  
                    VerticalAlignment="Center" Height="20" >
            <Polygon.Fill>
                <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                    <LinearGradientBrush.GradientStops>
                        <GradientStop Offset="0" Color="Black" />
                        <GradientStop Offset="1" Color="White" />
                    </LinearGradientBrush.GradientStops>
                </LinearGradientBrush>
            </Polygon.Fill>
        </Polygon>           
    ```
    
    **Después**
    ```XAML
        <Polygon Grid.Row="0" Grid.RowSpan="3"  Grid.ColumnSpan="3" Stretch="Fill"
                    Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"  
                    VerticalAlignment="Center" Height="20" 
                    Fill="{TemplateBinding Background}">
        </Polygon>           
    ```    

7. Inmediatamente después del polígono que has agregado, hay un rectángulo denominado **HorizontalTrackRect**. Quita el valor **Fill** del rectángulo, para que este no se vea y no bloquee la forma del polígono. (no conviene quitar completamente el rectángulo porque la plantilla de control también lo usa para los elementos visuales de interacción, como el movimiento del ratón).

    **Antes**
    ```XAML
        <Rectangle x:Name="HorizontalTrackRect"
                    Fill="{TemplateBinding Background}"
                    Height="{ThemeResource SliderTrackThemeHeight}"
                    Grid.Row="1"
                    Grid.ColumnSpan="3" />          
    ```
    
    **Después**
    ```XAML
        <Rectangle x:Name="HorizontalTrackRect"
                    Height="{ThemeResource SliderTrackThemeHeight}"
                    Grid.Row="1"
                    Grid.ColumnSpan="3" />
    ```

    Ya has acabado con la plantilla. Ahora, necesitamos aplicarla a los controles deslizantes. 
    
8. Vamos a actualizar el control deslizante de exposición.

    * Establece la propiedad **Template** del control deslizante en "{StaticResource FancySliderControlTemplate}".
    * Quita el valor Background="Transparent" del control deslizante. 
    * Establece que el fondo del control deslizante sea un degradado lineal que pase de negro a blanco.
    * Quita el polígono de fondo que creamos en la parte 1.
        
    **Antes**
    ```XAML
    <Polygon Grid.Row="2" Stretch="Fill"
                Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"  
                VerticalAlignment="Center" Height="20">
        <Polygon.Fill>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Black" />
                    <GradientStop Offset="1" Color="White" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Polygon.Fill>
    </Polygon>
    <Slider Header="Exposure" 
            Grid.Row="2" Background="Transparent" Foreground="Transparent"
            Value="{x:Bind item.Exposure, Mode=TwoWay}"
            Minimum="-2"
            Maximum="2"
            Template="{StaticResource FancySliderControlTemplate}"/>    
    ```
    
    **Después**
    ```XAML
    <Slider Header="Exposure" 
            Grid.Row="2"  Foreground="Transparent"
            Value="{x:Bind item.Exposure, Mode=TwoWay}"
            Minimum="-2"
            Maximum="2"
            Template="{StaticResource FancySliderControlTemplate}">
        <Slider.Background>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Black" />
                    <GradientStop Offset="1" Color="White" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Slider.Background>
    </Slider>
    ```        
9. Realiza las mismas actualizaciones en el control deslizante de temperatura.

    **Antes**
    ```XAML
    <Polygon Grid.Row="3" Stretch="Fill"
                Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"  
                VerticalAlignment="Center" Height="20">
        <Polygon.Fill>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Blue" />
                    <GradientStop Offset="1" Color="Yellow" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Polygon.Fill>
    </Polygon>
    <Slider Header="Temperature"
            Grid.Row="3" Background="Transparent" Foreground="Transparent"
            Value="{x:Bind item.Temperature, Mode=TwoWay}"
            Minimum="-1"
            Maximum="1" />
    ```
    
    **Después**
    ```XAML
    <Slider Header="Temperature"
            Grid.Row="3" Foreground="Transparent"
            Value="{x:Bind item.Temperature, Mode=TwoWay}"
            Minimum="-1"
            Maximum="1"
            Template="{StaticResource FancySliderControlTemplate}">
        <Slider.Background>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Blue" />
                    <GradientStop Offset="1" Color="Yellow" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Slider.Background>
    </Slider>
    ```    

10. Realiza las mismas actualizaciones al control deslizante de tinte.

    **Antes**
    ```XAML
    <Polygon Grid.Row="4" Stretch="Fill"
                Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"  
                VerticalAlignment="Center" Height="20">
        <Polygon.Fill>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Red" />
                    <GradientStop Offset="1" Color="Green" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Polygon.Fill>
    </Polygon>
    <Slider Header="Tint"
            Grid.Row="4" Background="Transparent" Foreground="Transparent"
            Value="{x:Bind item.Tint, Mode=TwoWay}"
            Minimum="-1"
            Maximum="1" />
    ```
    
    **Después**
    ```XAML
    <Slider Header="Tint"
            Grid.Row="4" Foreground="Transparent"
            Value="{x:Bind item.Tint, Mode=TwoWay}"
            Minimum="-1"
            Maximum="1"
            Template="{StaticResource FancySliderControlTemplate}">
        <Slider.Background>
            <LinearGradientBrush StartPoint="0,0.5" EndPoint="1,0.5">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Red" />
                    <GradientStop Offset="1" Color="Green" />
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Slider.Background>
    </Slider>
    ```        

11. Compila la aplicación y ejecútala. 

    ![Los mejores controles deslizantes del mundo](../basics/images/xaml-basics/style-sliders-templates.png)
    
    Como puedes ver, las actualizaciones mejoraron la posición del polígono; ahora, la parte inferior de este se alinea con la parte inferior de la miniatura del control deslizante.
    
<!-- TODO correct folder -->
Has finalizado el tutorial. Si te has quedado atascado y quieres ver la solución final, puedes encontrar el ejemplo completo en el [repositorio de ejemplo de aplicaciones UWP](https://github.com/Microsoft/Windows-universal-samples).