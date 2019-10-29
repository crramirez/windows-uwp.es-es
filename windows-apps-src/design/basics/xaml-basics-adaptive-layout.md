---
title: Crear tutoriales sobre diseños adaptativos
description: Este artículo describe los conceptos básicos del diseño adaptativo en XAML
keywords: XAML, UWP, Getting Started
ms.date: 08/30/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 71c04725fed4ab9d5d2158f2f892af038aefc8fd
ms.sourcegitcommit: 807dadf5eceb576aba3ad898a6e9bf12129e94a4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2019
ms.locfileid: "72680751"
---
# <a name="tutorial-create-adaptive-layouts"></a>Tutorial: Crear diseños adaptables

Este tutorial explica los conceptos básicos del uso de las funciones de diseño adaptativo y personalizado de XAML, que te permiten crear aplicaciones que parezcan hechas expresamente para cualquier dispositivo. Aprenderás a crear una nueva DataTemplate, agregar puntos de acoplamiento de ventana y adaptar el diseño de la aplicación mediante los elementos VisualStateManager y AdaptiveTrigger. Vamos a utilizar estas herramientas para optimizar un programa de edición de imágenes para pantallas de dispositivos más pequeñas. 

El programa de edición de imágenes en el que trabajarás tiene dos páginas o pantallas:

La **página principal**, que muestra una vista de galería de fotos, junto con alguna información sobre cada archivo de imagen.

![MainPage](../basics/images/xaml-basics/mainpage.png)

La **página de detalles**, que muestra una sola foto después de que se haya seleccionado. Un menú de edición flotante permite modificar la foto, cambiar su nombre y guardarla.

![DetailPage](../basics/images/xaml-basics/detailpage.png)

## <a name="prerequisites"></a>Requisitos previos

* Visual Studio 2019: [Descargar Visual Studio 2019 Community (gratuito)](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15&campaign=WinDevCenter&ocid=wdgcx-windevcenter-community-download) 
* SDK de Windows 10 (10.0.15063.468 o posterior):  [Descargar el último SDK de Windows (gratuito)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
* Emulador para Windows Mobile: [Descargar el emulador para Windows 10 Mobile (gratuito)](https://developer.microsoft.com/en-us/windows/downloads/sdk-archive)

## <a name="part-0-get-the-starter-code-from-github"></a>Parte 0: Obtener el código de inicio de GitHub

En este tutorial, empezaremos con una versión simplificada del ejemplo PhotoLab. 

1. Ve a [https://github.com/Microsoft/Windows-appsample-photo-lab](https://github.com/Microsoft/Windows-appsample-photo-lab). Esto te llevará a la página de GitHub que contiene el ejemplo. 
2. A continuación deberás clonar o descarga dicho ejemplo. Haz clic en el botón **Clone or download**. Aparecerá un submenú.
    <figure>
        <img src="../basics/images/xaml-basics/clone-repo.png" alt="The Clone or download menu on GitHub">
        <figcaption>El menú <b>Clone or download</b> de la página de GitHub del ejemplo de PhotoLab.</figcaption>
    </figure>

    **Si no conoces GitHub muy bien:**
    
    a. Haz clic en **Download ZIP** y guarda el archivo localmente. Esto descargará un archivo .zip que contiene todos los archivos de proyecto que necesitas.

    b. Extrae el archivo. Usa el Explorador de archivos para ir al archivo .zip que acabas de descargar, haz clic en él con el botón derecho y selecciona **Extraer todo...** . 

    c. Dirígete a la copia local del ejemplo y ve al directorio `Windows-appsample-photo-lab-master\xaml-basics-starting-points\adaptive-layout`.    

    **Si ya conoces GitHub:**

    a. Clona la bifurcación principal del repositorio localmente.

    b. Navegue al directorio `Windows-appsample-photo-lab\xaml-basics-starting-points\adaptive-layout`.

3. Abre el proyecto haciendo clic en `Photolab.sln`.

## <a name="part-1-run-the-mobile-emulator"></a>1ª parte: Ejecutar el emulador de dispositivo móvil

En la barra de herramientas de Visual Studio, asegúrate de que la plataforma de soluciones esté establecida en x86 o x64, no ARM y, a continuación, cambia el dispositivo de destino del equipo local a uno de los emuladores de dispositivo móvil que tengas instalados (por ejemplo, el emulador para Windows Mobile 10.0.15063 WVGA de 5 pulgadas y 1 GB). Prueba a ejecutar la aplicación Galería fotográfica en el emulador de dispositivo móvil que hayas seleccionado presionando **F5**.

En cuanto se inicie la aplicación, probablemente observarás que, mientras la aplicación funciona, no queda muy bien en una ventanilla tan pequeña. El elemento de cuadrícula líquida intenta acomodarse al el espacio real de pantalla limitada, reduciendo el número de columnas que se muestran, pero queda con un diseño que parece carente de inspiración y muy mal adaptado a una ventanilla tan pequeña.

![Diseño para dispositivos móviles: después](../basics/images/xaml-basics/adaptive-layout-mobile-before.png)

## <a name="part-2-build-a-tailored-mobile-layout"></a>2ª parte: Crear un diseño personalizado para dispositivos móviles
Para lograr que esta aplicación quede bien en dispositivos más pequeños, vamos a crear un conjunto de estilos distinto en nuestra página XAML, que se usará solo si se detecta un dispositivo móvil.

### <a name="create-a-new-datatemplate"></a>Crear una nueva DataTemplate
Vamos a adaptar la vista de galería de la aplicación mediante la creación de una nueva DataTemplate para las imágenes. Abre MainPage.xaml desde el Explorador de soluciones y agrega el siguiente código dentro de las etiquetas **Page.Resources**.

```XAML
<DataTemplate x:Key="ImageGridView_MobileItemTemplate"
              x:DataType="local:ImageFileInfo">

    <!-- Create image grid -->
    <Grid Height="{Binding ItemSize, ElementName=page}"
          Width="{Binding ItemSize, ElementName=page}">
        
        <!-- Place image in grid, stretching it to fill the pane-->
        <Image x:Name="ItemImage"
               Source="{x:Bind ImagePreview}"
               Stretch="UniformToFill">
        </Image>

    </Grid>
</DataTemplate>
```

Esta plantilla de galería ahorra espacio real en pantalla al eliminar el borde alrededor de imágenes y deshaciéndose de los metadatos de imagen (nombre de archivo, clasificaciones y demás) de debajo de cada miniatura. En su lugar, mostramos cada miniatura como un simple cuadrado.

### <a name="add-metadata-to-a-tooltip"></a>Agregar metadatos a una información sobre herramientas
Seguimos queriendo que el usuario pueda acceder a los metadatos de cada imagen, por lo que agregamos una información sobre herramientas a cada elemento de imagen. Agrega el siguiente código dentro de las etiquetas **Imagen** de la DataTemplate que acabas de crear.

```XAML
<Image ...>

    <!-- Add a tooltip to the image that displays metadata -->
    <ToolTipService.ToolTip>
        <ToolTip x:Name="tooltip">

            <!-- Arrange tooltip elements vertically -->
            <StackPanel Orientation="Vertical"
                        Grid.Row="1">

                <!-- Image title -->
                <TextBlock Text="{x:Bind ImageTitle, Mode=OneWay}"
                           HorizontalAlignment="Center"
                           Style="{StaticResource SubtitleTextBlockStyle}" />

                <!-- Arrange elements horizontally -->
                <StackPanel Orientation="Horizontal"
                            HorizontalAlignment="Center">

                    <!-- Image file type -->
                    <TextBlock Text="{x:Bind ImageFileType}"
                               HorizontalAlignment="Center"
                               Style="{StaticResource CaptionTextBlockStyle}" />

                    <!-- Image dimensions -->
                    <TextBlock Text="{x:Bind ImageDimensions}"
                               HorizontalAlignment="Center"
                               Style="{StaticResource CaptionTextBlockStyle}"
                               Margin="8,0,0,0" />
                </StackPanel>
            </StackPanel>
        </ToolTip>
    </ToolTipService.ToolTip>
</Image>
```

Esto mostrará el título, tipo de archivo y dimensiones de una imagen cuando se coloque el ratón sobre la miniatura (o mantenga presionado en una pantalla táctil).

### <a name="add-a-visualstatemanager-and-statetrigger"></a>Agregar un VisualStateManager y un StateTrigger

Ahora hemos creado un nuevo diseño para nuestros datos, pero la aplicación actualmente no tiene forma de saber cuándo usar este diseño en lugar de los estilos predeterminados. Para corregir esto, deberemos agregar un **VisualStateManager**. Agrega el siguiente código al elemento raíz de la página, **RelativePanel**.

```XAML
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>

        <!-- Add a new VisualState for mobile devices -->
        <VisualState x:Key="Mobile">

            <!-- Trigger visualstate when a mobile device is detected -->
            <VisualState.StateTriggers>
                <local:MobileScreenTrigger InteractionMode="Touch" />
            </VisualState.StateTriggers>

        </VisualState>
    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```

Esto agrega nuevas instancias de **VisualState** y **StateTrigger**, que se activarán cuando la aplicación detecte que se está ejecutando en un dispositivo móvil (la lógica para esta operación puede encontrarse en MobileScreenTrigger.cs, que viene incluida automáticamente en el directorio de PhotoLab). Cuando se inicie **StateTrigger**, la aplicación usará todos los atributos de diseño asignados a este **VisualState**.

### <a name="add-visualstate-setters"></a>Agregar establecedores de VisualState
A continuación, usaremos establecedores **VisualState** para indicar al **VisualStateManager** qué atributos deben aplicarse cuando se desencadena el estado. Cada establecedor apunta a una propiedad de un elemento XAML determinado y lo establece al valor dado. Agrega este código a la instancia de **VisualState** para dispositivo móvil que acabas de crear, debajo del elemento **VisualState.StateTriggers**. 

```XAML
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>

        <VisualState x:Key="Mobile">
            ...

            <!-- Add setters for mobile visualstate -->
            <VisualState.Setters>

                <!-- Move GridView about the command bar -->
                <Setter Target="ImageGridView.(RelativePanel.Above)"
                        Value="MainCommandBar" />

                <!-- Switch to mobile layout -->
                <Setter Target="ImageGridView.ItemTemplate"
                        Value="{StaticResource ImageGridView_MobileItemTemplate}" />

                <!-- Switch to mobile container styles -->
                <Setter Target="ImageGridView.ItemContainerStyle"
                        Value="{StaticResource ImageGridView_MobileItemContainerStyle}" />

                <!-- Move command bar to bottom of the screen -->
                <Setter Target="MainCommandBar.(RelativePanel.AlignBottomWithPanel)"
                        Value="True" />
                <Setter Target="MainCommandBar.(RelativePanel.AlignLeftWithPanel)"
                        Value="True" />
                <Setter Target="MainCommandBar.(RelativePanel.AlignRightWithPanel)"
                        Value="True" />

                <!-- Adjust the zoom slider to fit mobile screens -->
                <Setter Target="ZoomSlider.Minimum"
                        Value="80" />
                <Setter Target="ZoomSlider.Maximum"
                        Value="180" />
                <Setter Target="ZoomSlider.TickFrequency"
                        Value="20" />
                <Setter Target="ZoomSlider.Value"
                        Value="100" />
            </VisualState.Setters>

        </VisualState>
    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>

```

Estos establecedores establecen la plantilla de elementos **ItemTemplate** de la galería de imágenes en la nueva plantilla de datos **DataTemplate** que creamos en la primera parte, y alinean la barra de comandos y el control deslizante del zoom con la parte inferior de la pantalla, por lo que son fáciles de alcanzar con el pulgar en una pantalla de teléfono móvil.

### <a name="run-the-app"></a>Ejecuta la aplicación.
Ahora intenta ejecutar la aplicación con un emulador de dispositivo móvil. ¿Se muestra correctamente el nuevo diseño? Deberías ver una cuadrícula de miniaturas como aparece a continuación. Si sigues viendo el diseño antiguo, puede haber un error tipográfico en tu código de **VisualStateManager**.

![Diseño para dispositivos móviles: después](../basics/images/xaml-basics/adaptive-layout-mobile-after.png)

## <a name="part-3-adapt-to-multiple-window-sizes-on-a-single-device"></a>3\.ª parte: Adaptarse a varios tamaños de ventana en un solo dispositivo
Crear un nuevo diseño personalizado resuelve el problema de diseño dinámico para dispositivos móviles pero, ¿qué pasa con los equipos de escritorio y las tabletas? La aplicación puede quedar bien a pantalla completa, pero si el usuario reduce la ventana, puede acabar con una interfaz incómoda. Es posible garantizar que la experiencia del usuario final siempre tenga una apariencia y sensación correctas usando el **VisualStateManager** para adaptarse a varios tamaños de ventana en un solo dispositivo.

![Ventana pequeña: antes](../basics/images/xaml-basics/adaptive-layout-small-before.png)

### <a name="add-window-snap-points"></a>Agregar puntos de acoplamiento de ventana
El primer paso es definir los "puntos de acoplamiento" en los que se desencadenarán diferentes **VisualStates**. Abre App.xaml desde el Explorador de soluciones y agrega el siguiente código entre las etiquetas **Aplicación**.

```XAML
<Application.Resources>
    <!--  window width adaptive snap points  -->
    <x:Double x:Key="MinWindowSnapPoint">0</x:Double>
    <x:Double x:Key="MediumWindowSnapPoint">641</x:Double>
    <x:Double x:Key="LargeWindowSnapPoint">1008</x:Double>
</Application.Resources>
```

Esto nos da tres puntos de acoplamiento, que nos permiten crear nuevos **VisualStates** para tres intervalos de tamaños de ventana:
+ Pequeño (0 - 640 píxeles de ancho)
+ Medio (641 a 1007 píxeles de ancho)
+ Grande (> 1007 píxeles de ancho)

### <a name="create-new-visualstates-and-statetriggers"></a>Crear nuevos VisualStates y StateTriggers
A continuación, creamos los **VisualStates** y **StateTriggers** que corresponden a cada punto de acoplamiento. En MainPage.xaml, agrega el siguiente código al **VisualStateManager** creado en la segunda parte.

```XAML
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>
    ...

        <!-- Large window VisualState -->
        <VisualState x:Key="LargeWindow">

            <!-- Large window trigger -->
            <VisualState.StateTriggers>
                <AdaptiveTrigger MinWindowWidth="{StaticResource LargeWindowSnapPoint}"/>
            </VisualState.StateTriggers>
     
        </VisualState>

        <!-- Medium window VisualState -->
        <VisualState x:Key="MediumWindow">

            <!-- Medium window trigger -->
            <VisualState.StateTriggers>
                <AdaptiveTrigger MinWindowWidth="{StaticResource MediumWindowSnapPoint}"/>
            </VisualState.StateTriggers>
        
        </VisualState>

        <!-- Small window VisualState -->
        <VisualState x:Key="SmallWindow">

            <!-- Small window trigger -->
            <VisualState.StateTriggers >
                <AdaptiveTrigger MinWindowWidth="{StaticResource MinWindowSnapPoint}"/>
            </VisualState.StateTriggers>

        </VisualState>

    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```

### <a name="add-setters"></a>Agregar establecedores
Por último, agrega estos establecedores al estado **SmallWindow**.

```XAML

<VisualState x:Key="SmallWindow">
    ...

    <!-- Small window setters -->
    <VisualState.Setters>

        <!-- Apply mobile itemtemplate and styles -->
        <Setter Target="ImageGridView.ItemTemplate"
                Value="{StaticResource ImageGridView_MobileItemTemplate}" />
        <Setter Target="ImageGridView.ItemContainerStyle"
                Value="{StaticResource ImageGridView_MobileItemContainerStyle}" />

        <!-- Adjust the zoom slider to fit small windows-->
        <Setter Target="ZoomSlider.Minimum"
                Value="80" />
        <Setter Target="ZoomSlider.Maximum"
                Value="180" />
        <Setter Target="ZoomSlider.TickFrequency"
                Value="20" />
        <Setter Target="ZoomSlider.Value"
                Value="100" />
    </VisualState.Setters>

</VisualState>

```

Estos establecedores aplican la plantilla de datos **DataTemplate** para dispositivo móvil y los estilos a la aplicación de escritorio, siempre que la ventanilla sea inferior a 641 píxeles de ancho. También ajustan el control deslizante de zoom para una mejor adecuación a la pantalla pequeña.

### <a name="run-the-app"></a>Ejecución de la aplicación

En la barra de herramientas de Visual Studio, establece el dispositivo de destino en **Máquina local**, y ejecuta la aplicación. Cuando se cargue la aplicación, intenta cambiar el tamaño de la ventana. Si la ventana se reduce a un tamaño pequeño, verás cómo la aplicación cambia al diseño para dispositivo móvil que creaste en la segunda parte.

![Ventana pequeña: después](../basics/images/xaml-basics/adaptive-layout-small-after.png)

## <a name="going-further"></a>Ir más allá

Ahora que has completado este laboratorio, tienes suficiente información de diseño adaptable para experimentar más por tu cuenta. Intenta agregar un control de calificación a la información sobre herramientas solo para dispositivos móviles que agregaste anteriormente. O bien, para enfrentarte a un desafío más grande, intenta optimizar el diseño para tamaños de pantalla mayores (piensa en pantallas de televisión o Surface Studio)

Si te quedas bloqueado, puedes encontrar más información en estas secciones de [Definir diseños de página con XAML](../layout/layouts-with-xaml.md).

+ [Estados visuales y desencadenadores de estado](https://docs.microsoft.com/windows/uwp/design/layout/layouts-with-xaml#visual-states-and-state-triggers)
+ [Diseños personalizados](https://docs.microsoft.com/windows/uwp/design/layout/layouts-with-xaml#tailored-layouts)

Como alternativa, si quieres obtener más información sobre cómo se creó la aplicación inicial de edición de fotos, consulta estos tutoriales de XAML, [interfaces de usuario](../basics/xaml-basics-ui.md) y [enlace de datos](../../data-binding/xaml-basics-data-binding.md).

## <a name="get-the-final-version-of-the-photolab-sample"></a>Obtener la versión final del ejemplo de PhotoLab

Este tutorial no crea la aplicación completa de edición de fotos, así que asegúrate de echar un vistazo a la [versión final](https://github.com/Microsoft/Windows-appsample-photo-lab) para ver otras funciones, como las animaciones personalizadas y la compatibilidad con teléfonos.
