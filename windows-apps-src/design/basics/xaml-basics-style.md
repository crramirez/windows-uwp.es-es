---
title: Creación de estilos personalizados
description: Siga este tutorial para aprender a crear estilos personalizados y controles deslizantes para personalizar la interfaz de usuario de una aplicación XAML.
keywords: XAML, UWP, Getting Started
ms.date: 09/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1ff5df0d56d5be6b2cbb93814d01fed48a626797
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2020
ms.locfileid: "91219098"
---
# <a name="tutorial-create-custom-styles"></a>Tutorial: Creación de estilos personalizados

Este tutorial muestra cómo personalizar la interfaz de usuario de nuestra aplicación XAML. Advertencia: este tutorial podría implicar a un unicornio. (¡Lo hace!)

La aplicación de ejemplo PhotoLab tiene dos páginas. La _página principal_ muestra una vista de la galería de fotos, junto con información acerca de cada archivo de imagen.

![MainPage](../basics/images/xaml-basics/mainpage.png)

La *página de detalles* muestra una sola foto después de que se haya seleccionado. Un menú de edición flotante permite modificar la foto, cambiar su nombre y guardarla.

![DetailPage](../basics/images/xaml-basics/detailpage.png)

## <a name="prerequisites"></a>Requisitos previos

+ Visual Studio 2019: [Descargar Visual Studio 2019](https://visualstudio.microsoft.com/downloads/) (la versión Community Edition es gratuita).
+ SDK de Windows 10 (versión 10.0.17763.0 o posterior):  [Descargar el último SDK de Windows (gratuito)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
+ Windows 10, versión 1809 o posterior

## <a name="part-0-get-the-starter-code-from-github"></a>Parte 0: Obtener el código de inicio de GitHub

En este tutorial, empezaremos con una versión simplificada del ejemplo PhotoLab.

1. Vaya a la página de GitHub que contiene el ejemplo: [https://github.com/Microsoft/Windows-appsample-photo-lab](https://github.com/Microsoft/Windows-appsample-photo-lab).
2. A continuación deberás clonar o descarga dicho ejemplo. Selecciona el botón **Clone or download** (Clonar o descargar). Aparecerá un submenú.
    Menú ![Clone or download](images/xaml-basics/clone-repo.png) (Clonar o descargar) de la página de GitHub del ejemplo de PhotoLab

    **Si no conoces GitHub muy bien:**

    a. Selecciona **Download ZIP** (Descargar ZIP) y guarda el archivo localmente. Esto descargará un archivo .zip que contiene todos los archivos de proyecto que necesitas.

    b. Extrae el archivo. Use el Explorador de archivos para ir al archivo .zip que acaba de descargar, haga clic en este con el botón derecho y seleccione **Extraer todo**.

    c. Busca la copia local del ejemplo y ve al directorio `Windows-appsample-photo-lab-master\xaml-basics-starting-points\style`.

    **Si ya conoces GitHub:**

    a. Clona la bifurcación principal del repositorio localmente.

    b. Dirígete al directorio `Windows-appsample-photo-lab\xaml-basics-starting-points\style`.

3. Haga doble clic en `Photolab.sln` para abrir la solución en Visual Studio.

## <a name="part-1-create-a-fancy-slider-control"></a>Parte 1: Creación de un control deslizante sofisticado

La aplicación de Windows proporciona varias maneras de personalizar la apariencia de una aplicación. Desde la configuración de fuentes y tipografía hasta los colores y gradientes o los efectos de desenfoque, hay muchas opciones.

Para la primera parte del tutorial, vamos a animar algunos de los controles de edición de fotos.

![Un sencillo control deslizante con el estilo predeterminado.](../basics/images/xaml-basics/slider-start.png)

Estos controles deslizantes son útiles, hacen todas las cosas que debiera hacer un control deslizante, pero no son muy sofisticados. Vamos a solucionarlo.

El control deslizante de exposición ajusta la exposición de la imagen: si se desliza hacia la izquierda, se oscurecerá la imagen, mientras que si se desliza hacia la derecha, se aclarará. Vamos a hacer que el control deslizante sea más guay. Para ello, haremos que el fondo vaya del negro al blanco. De esta forma no solo mejorará la apariencia del control deslizante, lo que es estupendo, sino que también proporcionará una pista visual sobre la funcionalidad que proporciona el control deslizante.

Presiona F5 para compilar y ejecutar la aplicación. La primera pantalla muestra una galería de imágenes. Haz clic en una de ellas para ir a su página de detalles. Una vez allí, haz clic en el botón Editar para ver los controles de edición sobre los que trabajaremos. Sal de la aplicación y vuelve a Visual Studio.

### <a name="customize-a-slider-control"></a>Personalización de un control deslizante

1. En el panel del Explorador de soluciones, haz doble clic en DetailPage.xaml para abrirlo.

    ![El archivo DetailPage.xaml en el Explorador de soluciones de Visual Studio.](../basics/images/xaml-basics/style-detail-page-explorer.png)

1. Use un elemento `Polygon` para crear una forma de fondo para el control deslizante de exposición.

    El [espacio de nombres Windows.UI.Xaml.Shapes](/uwp/api/Windows.UI.Xaml.Shapes) proporciona siete formas para elegir. Hay una elipse, un rectángulo y un elemento denominado Trazado, que puede hacer cualquier tipo de forma (hasta un unicornio).

    ![Un unicornio](../basics/images/xaml-basics/unicorn.png)

    > **Información al respecto:** en el artículo [Dibujo de formas](../controls-and-patterns/shapes.md) se explica todo lo necesario acerca de las formas XAML.

    Queremos crear un widget con aspecto de triángulo, algo parecido a la forma que verías en el control de volumen de un equipo de música estéreo.

    ![Control deslizante de volumen](../basics/images/xaml-basics/style-volume-slider.png)

    Parece una tarea para la forma `Polygon`. Para definir un polígono, especifica un conjunto de puntos y asígnale un relleno. Vamos a crear un polígono que sea aproximadamente de 200 píxeles de ancho y 20 píxeles de alto, con un relleno degradado.

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
    * Si observa el XAML circundante, verá que estos elementos están en un objeto `Grid`. Colocamos el polígono en la misma fila que el control deslizante de exposición (`Grid.Row="2"`) para que aparezcan en el mismo punto. Colocamos el polígono delante del control deslizante para que este último represente la parte superior de la forma.
    * Establecemos `Stretch="Fill"` y `HorizontalAlignment="Stretch"` en el polígono, con el fin de que el triángulo se ajuste para rellenar el espacio disponible. Si el ancho del control deslizante aumenta o disminuye, el polígono crecerá o se encogerá en consecuencia.

1. Compila la aplicación y ejecútala. La apariencia del control deslizante debería ser impresionante:

    ![Control deslizante de exposición sofisticado](../basics/images/xaml-basics/style-exposure-slider-done.png)

1. Vamos a actualizar el siguiente control deslizante, el de temperatura. Este control deslizante cambia la temperatura del color de la imagen; si se desliza hacia la izquierda, la imagen es más azul, mientras que se si se desliza hacia la derecha, la imagen es más amarilla.

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

1. Compila la aplicación y ejecútala. Ahora deberías tener dos controles deslizantes decorativos.

    ![Dos controles deslizantes decorativos](../basics/images/xaml-basics/style-2sliders-done.png)

1. **Crédito adicional**

    Agrega una forma de fondo para el control deslizante de tinte que tenga un degradado de verde a rojo.

    ![Tres controles deslizantes decorativos](../basics/images/xaml-basics/style-3sliders-done.png)

Enhorabuena, has completado la parte 1. Si se ha quedado atascado o quiere ver la solución final, puede encontrar el código completo en [https://github.com/Microsoft/Windows-appsample-photo-lab](https://github.com/Microsoft/Windows-appsample-photo-lab).

## <a name="part-2-create-basic-styles"></a>Parte 2: Creación de estilos básicos

Una de las ventajas de los estilos de XAML es que pueden reducir considerablemente la cantidad de código que hay que escribir, y pueden facilitar mucho la actualización del aspecto de la aplicación.

Para definir un estilo, agrega un elemento [Style](/uwp/api/Windows.UI.Xaml.Style) a la propiedad [Resources](/uwp/api/windows.ui.xaml.frameworkelement.Resources) de un elemento que contenga el control al que quieres aplicar estilo.  Si agregas el estilo a la propiedad `Page.Resources`, se podrá acceder tus estilos desde toda la página. Si agregas el estilo a la propiedad `Application.Resources` en el archivo App.xaml, se podrá acceder a él desde toda la aplicación.

Puedes crear tanto estilos con nombre como estilos generales. Los estilos con nombre deben aplicarse explícitamente a controles concretos, mientras que los estilos generales se aplican a cualquier control que coincida con el `TargetType` especificado.

En este ejemplo, el primer estilo tiene un atributo `x:Key` y su tipo de destino es `Button`. La propiedad `Style` del primer botón se establece en esta clave, por lo que este estilo es un estilo con nombre y debe aplicarse explícitamente. El segundo estilo se aplica automáticamente al segundo botón porque su tipo de destino es `Button` y el estilo no tiene un atributo `x:Key`.

```xaml
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

Vamos a agregar un estilo a la aplicación. En DetailsPage.xaml, echa un vistazo a los bloques de texto situados junto a nuestros controles deslizantes de exposición, temperatura y tinte. Cada uno de estos bloques de texto muestra el valor de un control deslizante. Este es el bloque de texto del control deslizante de exposición. Ten en cuenta que las propiedades `Margin`, `VerticalAlignment` y `Padding` están establecidas.

```xaml
<TextBlock Grid.Row="2"
            Grid.Column="1"
            Margin="10,8,0,0" VerticalAlignment="Center" Padding="0"
            Text="{x:Bind item.Exposure.ToString('N', culture), Mode=OneWay}" />
```

Mira los demás bloques de texto, observa que esas mismas propiedades se establecen en los mismos valores. Parece un buen candidato para un estilo...

### <a name="create-a-value-text-block-style"></a>Creación de un estilo de bloque de texto con valor

1. Abre DetailsPage.xaml.

2. Busca el control `Grid` denominado **EditControlsGrid**. Contiene nuestros los controles deslizantes y cuadros de texto. Observa que la cuadrícula ya define un estilo para los controles deslizantes.

    ```xaml
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

    ```xaml
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

    ```xaml
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

4. Vamos a hacer que esto sea un estilo con nombre, dcdon el fin de poder especificar a qué controles de `TextBlock` se aplica. Establezca la propiedad `x:Key` del estilo en "ValueTextBox".

    **Antes**

    ```xaml
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

    ```xaml
            <Style TargetType="TextBlock"
                   x:Key="ValueTextBlock">
                <Setter Property="Margin"
                        Value="10,8,0,0" />
                <Setter Property="VerticalAlignment"
                        Value="Center" />
                <Setter Property="Padding"
                        Value="0" />
            </Style>
    ```

5. Para cada `TextBlock`, quite sus propiedades `Margin`, `VerticalAlignment`y `Padding`, y establezca su propiedad `Style` en "{StaticResource ValueTextBlock}".

    **Antes**

    ```xaml
     <TextBlock Grid.Row="2"
                Grid.Column="1"
                Margin="10,8,0,0" VerticalAlignment="Center" Padding="0"
                Text="{x:Bind item.Exposure.ToString('N', culture), Mode=OneWay}" />
    ```

    **Después**

    ```xaml
     <TextBlock Grid.Row="2"
                Grid.Column="1"
                Style="{StaticResource ValueTextBlock}"
                Text="{x:Bind item.Exposure.ToString('N', culture), Mode=OneWay}" />
    ```

    Realiza este cambio en los seis controles TextBlock asociados con los controles deslizantes.

6. Compila la aplicación y ejecútala. Debería tener el mismo aspecto. A pesar de todo, deberías disfrutar de ese maravilloso sentido de satisfacción que se obtiene al escribir código eficaz y fácil de mantener.

Enhorabuena, has completado la parte 2.

## <a name="part-3-use-a-control-template-to-make-a-fancy-slider"></a>3\.ª parte: Uso de una plantilla de control para crear un bonito control deslizante

¿Recuerdas que en la parte 1 agregamos una forma detrás del control deslizante para darle un aspecto atractivo?

Eso ya lo hicimos, pero hay una forma mejor de lograr el mismo efecto: crear una plantilla de control.

1. En el panel del Explorador de soluciones, haz doble clic en DetailPage.xaml.

1. A continuación, usaremos la plantilla de control predeterminada para el control deslizante como punto de partida. Agrega este XAML al elemento `Page.Resources`. (el elemento `Page.Resources` está hacia el principio de la página).

    Esta aplicación usa Windows UI Library (WinUI), por lo que puede copiar la plantilla predeterminada del repositorio de GitHub de WinUI: [Slider_themeresources.xaml](https://github.com/microsoft/microsoft-ui-xaml/blob/master/dev/Slider/Slider_themeresources.xaml).


    ```xaml
    <ControlTemplate x:Key="FancySliderControlTemplate" TargetType="Slider"
      xmlns:contract6Present="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractPresent(Windows.Foundation.UniversalApiContract,6)"
      xmlns:contract6NotPresent="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractNotPresent(Windows.Foundation.UniversalApiContract,6)"
      xmlns:contract7Present="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractPresent(Windows.Foundation.UniversalApiContract,7)"
      xmlns:contract7NotPresent="http://schemas.microsoft.com/winfx/2006/xaml/presentation?IsApiContractNotPresent(Windows.Foundation.UniversalApiContract,7)">
      <Grid Margin="{TemplateBinding Padding}">
        <Grid.Resources>
            <Style TargetType="Thumb" x:Key="SliderThumbStyle">
                <Setter Property="BorderThickness" Value="0" />
                <Setter Property="Background" Value="{ThemeResource SliderThumbBackground}" />
                <Setter Property="Template">
                    <Setter.Value>
                        <ControlTemplate TargetType="Thumb">
                            <Border
                                Background="{TemplateBinding Background}"
                                BorderBrush="{TemplateBinding BorderBrush}"
                                BorderThickness="{TemplateBinding BorderThickness}"
                                CornerRadius="{ThemeResource SliderThumbCornerRadius}" />
                        </ControlTemplate>
                    </Setter.Value>
                </Setter>
            </Style>
        </Grid.Resources>

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

        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <ContentPresenter x:Name="HeaderContentPresenter"
            Grid.Row="0"
            Content="{TemplateBinding Header}"
            ContentTemplate="{TemplateBinding HeaderTemplate}"
            FontWeight="{ThemeResource SliderHeaderThemeFontWeight}"
            Foreground="{ThemeResource SliderHeaderForeground}"
            Margin="{ThemeResource SliderTopHeaderMargin}"
            TextWrapping="Wrap"
            Visibility="Collapsed"
            x:DeferLoadStrategy="Lazy"/>
        <Grid x:Name="SliderContainer"
            Grid.Row="1"
            Background="{ThemeResource SliderContainerBackground}"
            Control.IsTemplateFocusTarget="True">
            <Grid x:Name="HorizontalTemplate" MinHeight="{ThemeResource SliderHorizontalHeight}">

                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>

                <Grid.RowDefinitions>
                    <RowDefinition Height="{ThemeResource SliderPreContentMargin}" />
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="{ThemeResource SliderPostContentMargin}" />
                </Grid.RowDefinitions>

                <Rectangle x:Name="HorizontalTrackRect"
                    Fill="{TemplateBinding Background}"
                    Height="{ThemeResource SliderTrackThemeHeight}"
                    Grid.Row="1"
                    Grid.ColumnSpan="3"
                    contract7Present:RadiusX="{Binding CornerRadius, RelativeSource={RelativeSource TemplatedParent}, Converter={StaticResource TopLeftCornerRadiusDoubleValueConverter}}"
                    contract7Present:RadiusY="{Binding CornerRadius, RelativeSource={RelativeSource TemplatedParent}, Converter={StaticResource BottomRightCornerRadiusDoubleValueConverter}}"
                    contract7NotPresent:RadiusX="{Binding Source={ThemeResource ControlCornerRadius}, Converter={StaticResource TopLeftCornerRadiusDoubleValueConverter}}"
                    contract7NotPresent:RadiusY="{Binding Source={ThemeResource ControlCornerRadius}, Converter={StaticResource BottomRightCornerRadiusDoubleValueConverter}}" />
                <Rectangle x:Name="HorizontalDecreaseRect" Fill="{TemplateBinding Foreground}" Grid.Row="1"
                    contract7Present:RadiusX="{Binding CornerRadius, RelativeSource={RelativeSource TemplatedParent}, Converter={StaticResource TopLeftCornerRadiusDoubleValueConverter}}"
                    contract7Present:RadiusY="{Binding CornerRadius, RelativeSource={RelativeSource TemplatedParent}, Converter={StaticResource BottomRightCornerRadiusDoubleValueConverter}}"
                    contract7NotPresent:RadiusX="{Binding Source={ThemeResource ControlCornerRadius}, Converter={StaticResource TopLeftCornerRadiusDoubleValueConverter}}"
                    contract7NotPresent:RadiusY="{Binding Source={ThemeResource ControlCornerRadius}, Converter={StaticResource BottomRightCornerRadiusDoubleValueConverter}}" />
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
                    Height="{ThemeResource SliderHorizontalThumbHeight}"
                    Width="{ThemeResource SliderHorizontalThumbWidth}"
                    Grid.Row="0"
                    Grid.RowSpan="3"
                    Grid.Column="1"
                    FocusVisualMargin="-14,-6,-14,-6"
                    AutomationProperties.AccessibilityView="Raw" />
            </Grid>
            <Grid x:Name="VerticalTemplate" MinWidth="{ThemeResource SliderVerticalWidth}" Visibility="Collapsed">

                <Grid.RowDefinitions>
                    <RowDefinition Height="*" />
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                </Grid.RowDefinitions>

                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="{ThemeResource SliderPreContentMargin}" />
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="{ThemeResource SliderPostContentMargin}" />
                </Grid.ColumnDefinitions>

                <Rectangle x:Name="VerticalTrackRect"
                    Fill="{TemplateBinding Background}"
                    Width="{ThemeResource SliderTrackThemeHeight}"
                    Grid.Column="1"
                    Grid.RowSpan="3"
                    contract7Present:RadiusX="{Binding CornerRadius, RelativeSource={RelativeSource TemplatedParent}, Converter={StaticResource TopLeftCornerRadiusDoubleValueConverter}}"
                    contract7Present:RadiusY="{Binding CornerRadius, RelativeSource={RelativeSource TemplatedParent}, Converter={StaticResource BottomRightCornerRadiusDoubleValueConverter}}"
                    contract7NotPresent:RadiusX="{Binding Source={ThemeResource ControlCornerRadius}, Converter={StaticResource TopLeftCornerRadiusDoubleValueConverter}}"
                    contract7NotPresent:RadiusY="{Binding Source={ThemeResource ControlCornerRadius}, Converter={StaticResource BottomRightCornerRadiusDoubleValueConverter}}" />
                <Rectangle x:Name="VerticalDecreaseRect"
                    Fill="{TemplateBinding Foreground}"
                    Grid.Column="1"
                    Grid.Row="2"
                    contract7Present:RadiusX="{Binding CornerRadius, RelativeSource={RelativeSource TemplatedParent}, Converter={StaticResource TopLeftCornerRadiusDoubleValueConverter}}"
                    contract7Present:RadiusY="{Binding CornerRadius, RelativeSource={RelativeSource TemplatedParent}, Converter={StaticResource BottomRightCornerRadiusDoubleValueConverter}}"
                    contract7NotPresent:RadiusX="{Binding Source={ThemeResource ControlCornerRadius}, Converter={StaticResource TopLeftCornerRadiusDoubleValueConverter}}"
                    contract7NotPresent:RadiusY="{Binding Source={ThemeResource ControlCornerRadius}, Converter={StaticResource BottomRightCornerRadiusDoubleValueConverter}}" />
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
                    Width="{ThemeResource SliderVerticalThumbWidth}"
                    Height="{ThemeResource SliderVerticalThumbHeight}"
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

1. En la plantilla `ControlTemplate` que acabas de agregar, busca el control de cuadrícula denominado `HorizontalTemplate`. Esta cuadrícula define la parte de la plantilla que queremos cambiar.

    ```xaml
    <Grid x:Name="HorizontalTemplate" MinHeight="{ThemeResource SliderHorizontalHeight}">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="{ThemeResource SliderPreContentMargin}" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="{ThemeResource SliderPostContentMargin}" />
        </Grid.RowDefinitions>
    ```

1.  Crea un polígono que sea como el que creaste para el control deslizante de exposición en la parte 1. Agrega el polígono después de la etiqueta de cierre `Grid.RowDefinitions`. Establece `Grid.Row` en "0", `Grid.RowSpan` en "3" y `Grid.ColumnSpan` en "3".

    **Antes**

    ```xaml
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

    ```xaml
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

1. Quite la configuración `Polygon.Fill`. Establezca `Fill` en `"{TemplateBinding Background}"`. Esto hace que al establecer la propiedad `Background` del control deslizante se establezca la propiedad `Fill` del polígono.

    **Antes**

    ```xaml
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

    ```xaml
        <Polygon Grid.Row="0" Grid.RowSpan="3"  Grid.ColumnSpan="3" Stretch="Fill"
                    Points="0,20 200,20 200,0" HorizontalAlignment="Stretch"
                    VerticalAlignment="Center" Height="20"
                    Fill="{TemplateBinding Background}">
        </Polygon>
    ```

1. Inmediatamente después del polígono que has agregado, hay un rectángulo denominado `HorizontalTrackRect`. Quita el valor `Fill` del rectángulo, para que este no se vea y no bloquee la forma del polígono. (no conviene quitar completamente el rectángulo porque la plantilla de control también lo usa para los elementos visuales de interacción, como el movimiento del ratón).

    **Antes**

    ```xaml
        <Rectangle x:Name="HorizontalTrackRect"
                    Fill="{TemplateBinding Background}"
                    Height="{ThemeResource SliderTrackThemeHeight}"
                    Grid.Row="1"
                    Grid.ColumnSpan="3" />
    ```

    **Después**

    ```xaml
        <Rectangle x:Name="HorizontalTrackRect"
                    Height="{ThemeResource SliderTrackThemeHeight}"
                    Grid.Row="1"
                    Grid.ColumnSpan="3" />
    ```

    Ya has acabado con la plantilla. Ahora, necesitamos aplicarla a los controles deslizantes.

1. Vamos a actualizar el control deslizante de exposición.

    * Establezca la propiedad `Template` del control deslizante en `"{StaticResource FancySliderControlTemplate}"`.
    * Quite la configuración de `Background="Transparent"` del control deslizante.
    * Establezca el objeto `Background` del control deslizante en un degradado lineal que pase de negro a blanco.
    * Quita el polígono de fondo que creamos en la parte 1.

    **Antes**

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
            Grid.Row="2" Background="Transparent" Foreground="Transparent"
            Value="{x:Bind item.Exposure, Mode=TwoWay}"
            Minimum="-2"
            Maximum="2" />
    ```

    **Después**

    ```xaml
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

1. Realiza las mismas actualizaciones en el control deslizante de temperatura.

    **Antes**

    ```xaml
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

    ```xaml
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

1. Realiza las mismas actualizaciones al control deslizante de tinte.

    **Antes**

    ```xaml
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

    ```xaml
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

1. Compila la aplicación y ejecútala.

    ![Los mejores controles deslizantes del mundo](../basics/images/xaml-basics/style-sliders-templates.png)

    Como puedes ver, las actualizaciones mejoraron la posición del polígono; ahora, la parte inferior de este se alinea con la parte inferior de la miniatura del control deslizante.

Has finalizado el tutorial. Si se ha quedado atascado y quiere ver la solución final, puede encontrar el ejemplo completo en [https://github.com/Microsoft/Windows-appsample-photo-lab](https://github.com/Microsoft/Windows-appsample-photo-lab).
