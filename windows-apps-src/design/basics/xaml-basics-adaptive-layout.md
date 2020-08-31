---
title: Crear tutoriales sobre diseños adaptativos
description: Este artículo describe los conceptos básicos del diseño adaptativo en XAML
keywords: XAML, UWP, Getting Started
ms.date: 08/20/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: b172f2da7fa8953045db4eab3818df02ce43e00c
ms.sourcegitcommit: 8e0e4cac79554e86dc7f035c4b32cb1f229142b0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/26/2020
ms.locfileid: "88942895"
---
# <a name="tutorial-create-adaptive-layouts"></a>Tutorial: Crear diseños adaptables

En este tutorial se explican los conceptos básicos del uso de las características de diseño adaptativo y XAML, que permiten crear aplicaciones que se vean correctamente con cualquier tamaño. Aprenderá a agregar puntos de interrupción de ventana, creará un nuevo objeto DataTemplate y usará la clase VisualStateManager para personalizar el diseño de la aplicación. Utilizará estas herramientas para optimizar un programa de edición de imágenes para tamaños de pantalla menores.

El programa de edición de imágenes tiene dos páginas. La _página principal_ muestra una vista de la galería de fotos, junto con información acerca de cada archivo de imagen.

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
2. A continuación deberás clonar o descarga dicho ejemplo. Haz clic en el botón **Clone or download**. Aparecerá un submenú.
    Menú ![Clone or download](images/xaml-basics/clone-repo.png) (Clonar o descargar) de la página de GitHub del ejemplo de PhotoLab

    **Si no conoces GitHub muy bien:**

    a. Haz clic en **Download ZIP** y guarda el archivo localmente. Esto descargará un archivo .zip que contiene todos los archivos de proyecto que necesitas.

    b. Extrae el archivo. Use el Explorador de archivos para buscar el archivo .zip que acaba de descargar, haga clic en este con el botón derecho y seleccione **Extraer todo**.

    c. Busque la copia local del ejemplo y vaya al directorio `Windows-appsample-photo-lab-master\xaml-basics-starting-points\adaptive-layout`.

    **Si ya conoces GitHub:**

    a. Clona la bifurcación principal del repositorio localmente.

    b. Dirígete al directorio `Windows-appsample-photo-lab\xaml-basics-starting-points\adaptive-layout`.

3. Haga doble clic en `Photolab.sln` para abrir la solución en Visual Studio.

## <a name="part-1-define-window-breakpoints"></a>Parte 1: Definir puntos de interrupción de ventana

Ejecute la aplicación. Se ve correctamente a pantalla completa, pero la interfaz de usuario (UI) no es ideal cuando la ventana se reduce demasiado. Para asegurarse de que la experiencia del usuario final siempre sea satisfactoria, utilice la clase [VisualStateManager](/uwp/api/windows.ui.xaml.visualstatemanager) para adaptar la interfaz de usuario a los distintos tamaños de ventana.

![Ventana pequeña: antes](../basics/images/xaml-basics/adaptive-layout-small-before.png)

Para obtener más información sobre el diseño de la aplicación, consulte la sección [Diseño](/windows/uwp/design/layout/) de los documentos.

### <a name="add-window-breakpoints"></a>Agregar puntos de interrupción de ventana

El primer paso es definir los _puntos de interrupción_ en los que se aplicarán los distintos estados visuales. Consulte [Tamaños de pantalla y puntos de interrupción](/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design) para obtener más información acerca de los puntos de interrupción para pantallas pequeñas, medianas y grandes.

Abra el archivo App.xaml desde el Explorador de soluciones y agregue el siguiente código después de `MergedDictionaries`, justo antes de la etiqueta de cierre `</ResourceDictionary>`.

```xaml
    <!--  Window width adaptive breakpoints.  -->
    <x:Double x:Key="MinWindowBreakpoint">0</x:Double>
    <x:Double x:Key="MediumWindowBreakpoint">641</x:Double>
    <x:Double x:Key="LargeWindowBreakpoint">1008</x:Double>
```

Esta acción crea 3 puntos de interrupción, lo que permite crear nuevos estados visuales para 3 intervalos de tamaños de ventana:

+ Pequeño (0 - 640 píxeles de ancho)
+ Medio (641 a 1007 píxeles de ancho)
+ Grande (> 1007 píxeles de ancho)

En este ejemplo, se crea un nuevo aspecto solo para el tamaño de ventana pequeño. Los tamaños mediano y grande usan el mismo aspecto.

## <a name="part-2-add-a-data-template-for-small-window-sizes"></a>Parte 2: Agregar una plantilla de datos para tamaños de ventana pequeños

Para que esta aplicación tenga un aspecto correcto incluso en una ventana pequeña, puede crear una nueva plantilla de datos que optimice el modo en que se muestran las imágenes en la vista de la galería de imágenes cuando el usuario reduce la ventana.

### <a name="create-a-new-datatemplate"></a>Crear una nueva DataTemplate

 Abre MainPage.xaml desde el Explorador de soluciones y agrega el siguiente código dentro de las etiquetas `Page.Resources`.

```xaml
<DataTemplate x:Key="ImageGridView_SmallItemTemplate"
              x:DataType="local:ImageFileInfo">

    <!-- Create image grid -->
    <Grid Height="{Binding ItemSize, ElementName=page}"
          Width="{Binding ItemSize, ElementName=page}">

        <!-- Place image in grid, stretching it to fill the pane-->
        <Image x:Name="ItemImage"
               Source="{x:Bind ImageSource, Mode=OneWay}"
               Stretch="UniformToFill">
        </Image>

    </Grid>
</DataTemplate>
```

Esta plantilla de galería ahorra espacio real en pantalla al eliminar el borde alrededor de imágenes y deshaciéndose de los metadatos de imagen (nombre de archivo, clasificaciones y demás) de debajo de cada miniatura. En su lugar, mostrará cada miniatura como un simple cuadrado.

### <a name="add-metadata-to-a-tooltip"></a>Agregar metadatos a una información sobre herramientas

Aún seguirá queriendo que el usuario pueda acceder a los metadatos de cada imagen, por lo que debe agregar una información sobre herramientas a cada elemento de imagen. Agrega el siguiente código dentro de las etiquetas `Image` de la DataTemplate que acabas de crear.

```xaml
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
```

Esto mostrará el título, tipo de archivo y dimensiones de una imagen cuando se coloque el ratón sobre la miniatura (o mantenga presionado en una pantalla táctil).

## <a name="part-3-define-visual-states"></a>3\.ª parte: Definir estados visuales

Ahora ha creado un nuevo diseño para sus datos, pero la aplicación actualmente no tiene forma de saber cuándo usar este diseño en lugar de los estilos predeterminados. Para corregirlo, debe agregar una clase [VisualStateManager](/uwp/api/windows.ui.xaml.visualstatemanager) y definiciones de [VisualState](/uwp/api/windows.ui.xaml.visualstate).

### <a name="add-a-visualstatemanager"></a>Agregar una clase VisualStateManager

Agrega el siguiente código al elemento raíz de la página, `RelativePanel`.

```xaml
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>
    ...

        <!-- Large window VisualState -->
        <VisualState x:Key="LargeWindow">

        </VisualState>

        <!-- Medium window VisualState -->
        <VisualState x:Key="MediumWindow">

        </VisualState>

        <!-- Small window VisualState -->
        <VisualState x:Key="SmallWindow">

        </VisualState>

    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```

### <a name="create-statetriggers-to-apply-the-visual-state"></a>Crear objetos StateTriggers para aplicar el estado visual

A continuación, cree los objetos `StateTriggers` que se correspondan con cada punto de acoplamiento. En MainPage.xaml, agrega el siguiente código al `VisualStateManager` creado en la segunda parte.

```xaml
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

Cuando se desencadene cada estado visual, la aplicación usará todos los atributos de diseño asignados al objeto `VisualState` activo.

### <a name="set-properties-for-each-visual-state"></a>Establecer propiedades para cada estado visual

Por último, establezca las propiedades de cada estado visual para indicar a la clase `VisualStateManager` qué atributos se aplicarán cuando se desencadene el estado. Cada establecedor apunta a una propiedad de un elemento XAML determinado y lo establece al valor dado. Agregue este código al estado visual `SmallWindow` que acaba de crear, después de `StateTriggers`.

```xaml
    <!-- Small window setters -->
    <VisualState.Setters>

        <!-- Apply small template and styles -->
        <Setter Target="ImageGridView.ItemTemplate"
                Value="{StaticResource ImageGridView_SmallItemTemplate}" />
        <Setter Target="ImageGridView.ItemContainerStyle"
                Value="{StaticResource ImageGridView_SmallItemContainerStyle}" />

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
```

Estos establecedores definen el objeto `ItemTemplate` de la galería de imágenes en el nuevo objeto `DataTemplate` que creó en la sección anterior. También ajustan el control deslizante de zoom para una mejor adecuación a la pantalla pequeña.

### <a name="run-the-app"></a>Ejecución de la aplicación

Ejecute la aplicación. Cuando se cargue la aplicación, intenta cambiar el tamaño de la ventana. Si la ventana se reduce a un tamaño pequeño, verá cómo la aplicación cambia al diseño pequeño que creó en la segunda parte.

![Ventana pequeña: después](../basics/images/xaml-basics/adaptive-layout-small-after.png)

## <a name="going-further"></a>Ir más allá

Ahora que has completado este laboratorio, tienes suficiente información de diseño adaptable para experimentar más por tu cuenta. Para enfrentarse a un desafío mayor, intente optimizar el diseño para tamaños de pantalla mayores, como Surface Hub. Consulte [Probar aplicaciones de Surface Hub con Visual Studio](/windows/uwp/debug-test-perf/test-surface-hub-apps-using-visual-studio) si desea probar un diseño de Surface Hub.

Si te quedas bloqueado, puedes encontrar más información en estas secciones de [Definir diseños de página con XAML](../layout/layouts-with-xaml.md).

+ [Estados visuales y desencadenadores de estado](/windows/uwp/design/layout/layouts-with-xaml#visual-states-and-state-triggers)
+ [Diseños personalizados](/windows/uwp/design/layout/layouts-with-xaml#tailored-layouts)

Como alternativa, si quieres obtener más información sobre cómo se creó la aplicación inicial de edición de fotos, consulta estos tutoriales de XAML, [interfaces de usuario](../basics/xaml-basics-ui.md) y [enlace de datos](../../data-binding/xaml-basics-data-binding.md).

## <a name="get-the-final-version-of-the-photolab-sample"></a>Obtener la versión final del ejemplo de PhotoLab

Este tutorial no crea la aplicación completa de edición de fotos, así que asegúrese de echar un vistazo a la [versión final](https://github.com/Microsoft/Windows-appsample-photo-lab) para ver otras funciones, como las animaciones personalizadas y los estilos.
