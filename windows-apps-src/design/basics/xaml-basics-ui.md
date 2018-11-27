---
title: Tutorial para crear una interfaz de usuario
description: Este artículo describe los conceptos básicos de creación de interfaces de usuario en XAML
keywords: XAML, UWP, Introducción
ms.date: 08/30/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1bae8455f1062b3ad62aeac3807c6c58ae274a1b
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2018
ms.locfileid: "7700963"
---
# <a name="tutorial-create-a-user-interface"></a>Tutorial: Crear una interfaz de usuario

En este tutorial aprenderás a crear una interfaz de usuario básica para un programa de edición de imágenes de las siguientes formas: 

+ El uso de las herramientas XAML de Visual Studio, como Diseñador XAML, Caja de herramientas, Editor XAML, el panel Propiedades y Esquema de documento, para agregar controles y contenido a la interfaz de usuario
+ El uso de algunos de los paneles de diseño XAML más comunes, como RelativePanel, Grid y StackPanel.

El programa de edición de imágenes tiene dos páginas o pantallas:

La **página principal**, que muestra una vista de galería de fotos, junto con alguna información sobre cada archivo de imagen.

![MainPage](images/xaml-basics/mainpage.png)

La **página de detalles**, que muestra una sola foto después de que se haya seleccionado. Un menú de edición flotante permite modificar la foto, cambiar su nombre y guardarla.

![DetailPage](images/xaml-basics/detailpage.png)


## <a name="prerequisites"></a>Requisitos previos

* Visual Studio 2017: [Descargar Visual Studio 2017 Community (gratuito)](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15&campaign=WinDevCenter&ocid=wdgcx-windevcenter-community-download) 
* Windows 10 SDK (10.0.15063.468 o posterior): [Descargar el Windows SDK más reciente (gratuito)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)

## <a name="part-0-get-the-starter-code-from-github"></a>Parte 0: Obtener el código de inicio de GitHub

En este tutorial, empezaremos con una versión simplificada del ejemplo PhotoLab. 

1. Ve a [https://github.com/Microsoft/Windows-appsample-photo-lab](https://github.com/Microsoft/Windows-appsample-photo-lab). Esto te llevará a la página de GitHub que contiene el ejemplo. 
2. A continuación deberás clonar o descarga dicho ejemplo. Haz clic en el botón **Clone or download**. Aparecerá un submenú.
    <figure>
        <img src="images/xaml-basics/clone-repo.png" alt="The Clone or download menu on GitHub">
        <figcaption>El menú <b>Clone or download</b> de la página de GitHub del ejemplo de PhotoLab.</figcaption>
    </figure>

    **Si no conoces GitHub muy bien:**
    
    a. Haz clic en **Download ZIP** y guarda el archivo localmente. Esto descargará un archivo .zip que contiene todos los archivos de proyecto que necesitas.
    b. Extrae el archivo. Usa el Explorador de archivos para ir al archivo .zip que acabas de descargar, haz clic en él con el botón derecho y selecciona **Extraer todo...**. c. Dirígete a la copia local del ejemplo y ve al directorio `Windows-appsample-photo-lab-master\xaml-basics-starting-points\user-interface`.    

    **Si ya conoces GitHub:**

    a. Clona la bifurcación principal del repositorio localmente.
    b. Dirígete al directorio `Windows-appsample-photo-lab\xaml-basics-starting-points\user-interface`.

3. Abre el proyecto haciendo clic en `Photolab.sln`.

## <a name="part-1-add-a-textblock-using-xaml-designer"></a>Parte 1: Agregar un TextBlock con el Diseñador XAML

Visual Studio proporciona varias herramientas que te ayudarán a crear más fácilmente la interfaz de usuario de XAML. Diseñador de XAML te permite arrastrar controles a la superficie de diseño y ver qué te parecen antes de ejecutar la aplicación. El panel Propiedades te permite ver y establecer todas las propiedades del control que está activo en el diseñador. Esquema del documento muestra la estructura de elementos primarios y secundarios del árbol visual XAML para la interfaz de usuario. El editor XAML te permite escribir directamente y modificar el marcado XAML.

Esta es la interfaz de usuario de Visual Studio con las herramientas etiquetadas.

![Diseño de Visual Studio](images/xaml-basics/visual-studio-tools.png)

Cada una de estas herramientas te ayudarán a crear la interfaz de usuario más fácilmente, así que usaremos todas ellas en este tutorial. Comenzarás por usar el Diseñador de XAML para agregar un control. 

**Agregar un control con el Diseñador XAML:**

1. Haz doble clic en el archivo **MainPage.xaml** en el Explorador de soluciones para abrirlo. Esto muestra la página principal de la aplicación sin elementos de interfaz de usuario agregados.

2. Antes de ir más allá, deberás realizar algunos ajustes en Visual Studio.

    - Asegúrate de que la plataforma de soluciones esté establecida en x86 o x64, no ARM.
    - Establece la página principal del Diseñador de XAML para mostrar la vista previa de escritorio de 13,3".

    Deberías ver ambas configuraciones junto a la parte superior de la ventana, como se muestra aquí.

    ![Configuración de VS](images/xaml-basics/layout-vs-settings.png)

    Puedes ejecutar la aplicación ahora, pero no verás gran cosa. Vamos a agregar algunos elementos de interfaz de usuario para que las cosas sean más interesantes.

3. En Caja de herramientas, expande **Controles XAML comunes** y busca el control [TextBlock](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock). Arrastra un TextBlock a la superficie de diseño, cerca de la esquina superior izquierda de la página.

    El TextBlock se agrega a la página y el diseñador establece algunas propiedades según su mejor criterio para el diseño que quieres. Aparece un resaltado en azul en torno a TextBlock, para indicar ahora es el objeto activo. Ten en cuenta los márgenes y otras opciones de configuración agregadas por el diseñador. Tu XAML debe tener este aspecto similar a este: No te preocupes si no tiene el formato exactamente igual; hemos resumido esto para facilitar la lectura.

    ```xaml
    <TextBlock x:Name="textBlock"
               HorizontalAlignment="Left"
               Margin="351,44,0,0"
               TextWrapping="Wrap"
               Text="TextBlock"
               VerticalAlignment="Top"/>
    ```

    En los siguientes pasos, actualizarás estos valores.

4. En el panel Propiedades, cambia el valor Nombre de TextBlock de **textBlock** a **TitleTextBlock**. (Asegúrate de que el TextBlock siga siendo el objeto activo)

5. En **Comunes**, cambia el valor Texto a **Colección**.

    ![Propiedades de TextBlock](images/xaml-basics/text-block-properties.png)

    En el editor XAML, el código XAML tendrá ahora este aspecto.

    ```xaml
    <TextBlock x:Name="TitleTextBlock"
               HorizontalAlignment="Left"
               Margin="351,44,0,0"
               TextWrapping="Wrap"
               Text="Collection"
               VerticalAlignment="Top"/>
    ```

6. Para colocar el TextBlock, primero debes quitar los valores de propiedades que se agregaron mediante Visual Studio. En Esquema del documento, haz clic en **TitleTextBlock** y, a continuación, selecciona **Diseño > Restablecer todo**.

![Esquema del documento](images/xaml-basics/doc-outline-reset.png)

7. En el panel Propiedades, escribe **margen** en el cuadro de búsqueda para encontrar fácilmente la propiedad **Margen**. Establece los márgenes izquierdo y derecho en 24.

    ![Márgenes de TextBlock](images/xaml-basics/margins.png)

    Los márgenes proporcionan el posicionamiento más básico de un elemento en la página. Son útiles para ajustar el diseño, pero usar valores de margen grandes como los agregados mediante Visual Studio dificulta la posibilidad de que tu interfaz de usuario se adapte a diferentes tamaños de pantalla, por lo que debe evitarse.

    Para obtener más información, consulta [Alineación, márgenes y relleno](../layout/alignment-margin-padding.md).

8. En el panel Esquema del documento, haz clic con el botón derecho en **TitleTextBlock** y luego selecciona **Editar estilo > Aplicar recurso > TitleTextBlockStyle**. Esto aplica un estilo definido por el sistema para el texto del título.

    ```xaml
    <TextBlock x:Name="TitleTextBlock"
               TextWrapping="Wrap"
               Text="Collection"
               Margin="24,0,0,24"
               Style="{StaticResource TitleTextBlockStyle}"/>
    ```

9. En el panel Propiedades, escribe **textwrapping** en el cuadro de búsqueda para encontrar la propiedad **TextWrapping**. Haz clic en el _marcador de propiedades_ de la propiedad **TextWrapping** para abrir su menú. (El _marcador de propiedades_ es el símbolo de cuadro de pequeño tamaño situado a la derecha de cada valor de propiedad. El _marcador de propiedades_ es negro para indicar que la propiedad está establecida en un valor no predeterminado). En el menú **Propiedad**, selecciona **Restablecer** para restablecer la propiedad TextWrapping.

    Visual Studio agrega esta propiedad, pero ya está establecida en el estilo que aplicaste, por lo que no necesitas hacerlo aquí.

¡Has agregado la primera parte de la interfaz de usuario a tu aplicación! Ejecuta la aplicación para ver qué aspecto tiene.

Habrás notado que, en el Diseñador de XAML, la aplicación mostraba texto blanco sobre un fondo negro, pero cuando se ejecutaba, mostraba texto negro sobre un fondo blanco. Eso es porque Windows tiene un tema oscuro y un tema claro y el tema predeterminado varía según el dispositivo. En un PC, el tema predeterminado es el claro. Puedes hacer clic en el icono de engranaje en la parte superior de Diseñador de XAML para abrir la configuración de vista previa del dispositivo y cambiar el tema a claro, para que la aplicación tenga el mismo aspecto en Diseñador XAML que en tu PC.

> [!NOTE]
> En esta parte del tutorial, has agregado un control mediante arrastrar y soltar. También puedes agregar un control haciendo doble clic en él, en Caja de herramientas. Pruébalo y observa las diferencias en el código XAML que Visual Studio genera.

## <a name="part-2-add-a-gridview-control-using-the-xaml-editor"></a>Parte 2: agregar un control GridView mediante el editor de XAML

En la parte 1, tenías alguna idea de cómo usar el Diseñador XAML y algunas de las herramientas proporcionadas por Visual Studio. Aquí, usarás el editor XAML para trabajar directamente con el marcado XAML. A medida que te vayas familiarizando con XAML, es posible que resulte una manera más eficaz de trabajar.

En primer lugar, reemplaza el diseño raíz [cuadrícula](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid) con un [**RelativePanel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel). RelativePanel facilita la reorganización de los fragmentos de la interfaz de usuario relativos al panel u otras partes de la interfaz de usuario. Podrás ver su utilidad en el tutorial [Diseño adaptativo XAML](xaml-basics-adaptive-layout.md). 

A continuación, agregarás un control [GridView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.gridview) para mostrar los datos.

**Agregar un control mediante el editor de XAML**

1. En el editor de XAML, cambia la raíz **Cuadrícula** a un **RelativePanel**.

    **Antes**
    ```xaml
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
          <TextBlock x:Name="TitleTextBlock"
                     Text="Collection"
                     Margin="24,0,0,24"
                     Style="{StaticResource TitleTextBlockStyle}"/>
    </Grid>
    ```

    **Después**
    ```xaml
    <RelativePanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <TextBlock x:Name="TitleTextBlock"
                   Text="Collection"
                   Margin="24,0,0,24"
                   Style="{StaticResource TitleTextBlockStyle}"/>
    </RelativePanel>
    ```

    Para obtener más información sobre el diseño mediante un **RelativePanel**, consulta [Paneles de diseño](https://docs.microsoft.com/windows/uwp/layout/layout-panels#relativepanel).

2. Por debajo del elemento **TextBlock**, agrega un **Control GridView** denominado 'ImageGridView'. Establece el **RelativePanel**, _propiedades adjuntas_, para colocar el control a continuación del texto del título y hacer que se estire por todo el ancho de la pantalla.

    **Agrega este XAML**

    ```xaml
    <GridView x:Name="ImageGridView"
              Margin="0,0,0,8"
              RelativePanel.AlignLeftWithPanel="True"
              RelativePanel.AlignRightWithPanel="True"
              RelativePanel.Below="TitleTextBlock"/>
    ```

    **Después del TextBlock**
    ```xaml
    <RelativePanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <TextBlock x:Name="TitleTextBlock"
                   Text="Collection"
                   Margin="24,0,0,24"
                   Style="{StaticResource TitleTextBlockStyle}"/>
        
        <!-- Add the GridView here. -->

    </RelativePanel>
    ```

    Para obtener más información sobre las propiedades adjuntas de Panel, consulta [Paneles de diseño](https://docs.microsoft.com/windows/uwp/layout/layout-panels).

3. Para que **GridView** muestre algo, deberás asignarle una colección de datos a mostrar. Abre MainPage.xaml.cs y busca el método **GetItemsAsync**. Este método rellena una colección denominada Imágenes, que es una propiedad que hemos agregado a MainPage.

    Después del bucle **foreach** de **GetItemsAsync**, agrega esta línea de código.

    ```csharp
    ImageGridView.ItemsSource = Images;
    ```

    Esto establece la propiedad **ItemsSource** de GridView en la colección **Imágenes** de la aplicación y ofrece a **GridView** algo que mostrar.

Este es un buen lugar para ejecutar la aplicación y asegurarse de que todo funcione. Debería tener un aspecto similar a este.

![Punto de control 1 de la interfaz de usuario de la aplicación](images/xaml-basics/layout-0.png)

Observarás que la aplicación todavía no muestra imágenes. De manera predeterminada, se muestra el valor de ToString del tipo de datos que está en la colección. A continuación, crearás una plantilla de datos para definir cómo se muestran los datos.

> [!NOTE]
> Puedes encontrar más información sobre diseño usando un **RelativePanel** en el artículo [Paneles de diseño](https://docs.microsoft.com/windows/uwp/layout/layout-panels#relativepanel). Echa un vistazo y, a continuación, prueba con algunos diseños diferentes estableciendo las propiedades adjuntas de RelativePanel de **TextBlock** y **GridView**.

## <a name="part-3-add-a-datatemplate-to-display-your-data"></a>Parte 3: agregar una DataTemplate para mostrar los datos

Ahora crearás una [DataTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.datatemplate) que dirá a GridView cómo mostrar los datos. Para obtener una explicación completa de las plantillas de datos, consulta [contenedores y plantillas de elementos](../controls-and-patterns/item-containers-templates.md).

Por ahora, solo agregarás marcadores de posición para ayudarte a crear el diseño que quieres. En el tutorial [enlace de datos XAML](../../data-binding/xaml-basics-data-binding.md), reemplazarás estos marcadores de posición con datos reales de la clase **ImageFileInfo**. Puedes abrir ahora el archivo ImageFileInfo.cs, si quieres ver el aspecto que tiene el objeto de datos.

**Agregar una plantilla de datos a una vista de cuadrícula**

1. Abre MainPage.xaml.

2. Para mostrar la clasificación, usa el control **RadRating** desde el paquete NuGet [Telerik UI para UWP](https://github.com/telerik/UI-For-UWP). Agrega una referencia de espacio de nombres XAML que especifique el espacio de nombres para los controles de Telerik. Coloca esto en la etiqueta de apertura de **Página**, después del resto de entradas 'xmlns:'.

    **Agrega este XAML**

    ```xaml
    xmlns:telerikInput="using:Telerik.UI.Xaml.Controls.Input"
    ```

    **Después de la última entrada 'xmlns:'**

    ```xaml
    <Page x:Name="page"
      x:Class="PhotoLab.MainPage"
      xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
      xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
      xmlns:local="using:PhotoLab"
      xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
      xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
      xmlns:telerikInput="using:Telerik.UI.Xaml.Controls.Input"
      mc:Ignorable="d"
      NavigationCacheMode="Enabled">
    ```

    Para obtener más información sobre espacios de nombres XAML, consulta [Espacios de nombres XAML y asignación de espacios de nombres](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-namespaces-and-namespace-mapping).

3. En Esquema del documento, haz clic en **ImageGridView**. En el menú contextual, selecciona **Editar plantillas adicionales > Editar elementos generados (ItemTemplate) > Crear (vacío)... **. Se abrirá el cuadro de diálogo **Crear recurso**.

4. En el cuadro de diálogo, cambia el valor de Nombre (clave) a **ImageGridView_DefaultItemTemplate**y, a continuación, haz clic en **Aceptar**.

    Ocurren varias cosas al hacer clic en **Aceptar**.

    - Se agrega una **DataTemplate** a la sección Page.Resources de MainPage.xaml.

        ```xaml
        <Page.Resources>
            <DataTemplate x:Key="ImageGridView_DefaultItemTemplate">
                <Grid/>
            </DataTemplate>
        </Page.Resources>
        ```

    - El ámbito del esquema del documento se establece en esta **DataTemplate**.

        Cuando hayas terminado de crear la plantilla de datos, puedes hacer clic en la flecha arriba de la esquina superior izquierda del esquema del documento, para volver al ámbito de página.

    - La propiedad **ItemTemplate** de GridView se establece en el recurso **DataTemplate**.

    ```xaml
        <GridView x:Name="ImageGridView"
                  Margin="0,0,0,8"
                  RelativePanel.AlignLeftWithPanel="True"
                  RelativePanel.AlignRightWithPanel="True"
                  RelativePanel.Below="TitleTextBlock"
                  ItemTemplate="{StaticResource ImageGridView_DefaultItemTemplate}"/>
    ```

5. En el recurso **ImageGridView_DefaultItemTemplate**, asigna a la raíz **Cuadrícula** una altura y anchura de **300**y margen de **8**. A continuación, agrega dos filas y establece la altura de la segunda fila en **Automática**.

    **Antes**
    ```xaml
    <Grid/>
    ```

    **Después**
    ```xaml
    <Grid Height="300"
          Width="300"
          Margin="8">
        <Grid.RowDefinitions>
            <RowDefinition />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>
    </Grid>
    ```

    Para obtener más información sobre diseños de cuadrícula, consulta [Paneles de diseño](https://docs.microsoft.com/windows/uwp/layout/layout-panels#grid).

6. Agregar controles a la cuadrícula.

    a. Agrega un control de **Imagen** en la primera fila de la cuadrícula. Este es el lugar donde se mostrará la imagen pero, por ahora, usarás el logotipo de la tienda de aplicaciones como marcador de posición.

    b. Agrega controles **TextBlock** para mostrar el nombre, el tipo de archivo y las dimensiones de la imagen. Para ello, usa los controles **StackPanel** para organizar los bloques de texto.

    Para obtener más información sobre diseño **StackPanel**, consulta [Paneles de diseño](https://docs.microsoft.com/windows/uwp/layout/layout-panels#stackpanel).

    c. Agregar el control **RadRating** al **StackPanel** exterior (vertical). Colócalo después del **StackPanel** interior (horizontal).

    **Plantilla final**

    ```xaml
    <Grid Height="300"
          Width="300"
          Margin="8">
        <Grid.RowDefinitions>
            <RowDefinition />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>

        <Image x:Name="ItemImage"
               Source="Assets/StoreLogo.png"
               Stretch="Uniform" />

        <StackPanel Orientation="Vertical"
                    Grid.Row="1">
            <TextBlock Text="ImageTitle"
                       HorizontalAlignment="Center"
                       Style="{StaticResource SubtitleTextBlockStyle}" />
            <StackPanel Orientation="Horizontal"
                        HorizontalAlignment="Center">
                <TextBlock Text="ImageFileType"
                           HorizontalAlignment="Center"
                           Style="{StaticResource CaptionTextBlockStyle}" />
                <TextBlock Text="ImageDimensions"
                           HorizontalAlignment="Center"
                           Style="{StaticResource CaptionTextBlockStyle}"
                           Margin="8,0,0,0" />
            </StackPanel>

            <telerikInput:RadRating Value="3"
                                    IsReadOnly="True">
                <telerikInput:RadRating.FilledIconContentTemplate>
                    <DataTemplate>
                        <SymbolIcon Symbol="SolidStar"
                                    Foreground="White" />
                    </DataTemplate>
                </telerikInput:RadRating.FilledIconContentTemplate>
                <telerikInput:RadRating.EmptyIconContentTemplate>
                    <DataTemplate>
                        <SymbolIcon Symbol="OutlineStar"
                                    Foreground="White" />
                    </DataTemplate>
                </telerikInput:RadRating.EmptyIconContentTemplate>
            </telerikInput:RadRating>

        </StackPanel>
    </Grid>
    ```

Ejecuta la aplicación ahora para ver la **GridView** con la plantilla de elementos que acabas de crear. Sin embargo, es posible que no veas el control de clasificación, ya que tiene estrellas blancas sobre fondo blanco. A continuación, cambiarás el color de fondo.

![Punto de control 2 de la interfaz de usuario de la aplicación](images/xaml-basics/layout-1.png)

## <a name="part-4-modify-the-item-container-style"></a>Parte 4: modificar el estilo del contenedor de elementos

La plantilla de control de un elemento contiene los elementos visuales que muestran el estado, como la selección, el paso del puntero y el foco. Estos elementos visuales se representan encima o debajo de la plantilla de datos. Aquí, modificarás las propiedades **Segundo plano** y **Margen** de la plantilla de control, para dar a los elementos de **GridView** un fondo gris.

**Modificar el contenedor de elementos**

1. En Esquema del documento, haz clic en **ImageGridView**. En el menú contextual, selecciona **Editar plantillas adicionales > Editar el contenedor de elementos generados (ItemContainerStyle) > Editar una copia...**. Se abrirá el cuadro de diálogo **Crear recurso**.

2. En el cuadro de diálogo, cambia el valor de Nombre (clave) a **ImageGridView_DefaultItemContainerStyle**y, a continuación, haz clic en **Aceptar**.

    Se agregará una copia del estilo predeterminado a la sección **Page.Resources** de tu XAML.

    ```xaml
    <Style x:Key="ImageGridView_DefaultItemContainerStyle" TargetType="GridViewItem">
        <Setter Property="FontFamily" Value="{ThemeResource ContentControlThemeFontFamily}"/>
        <Setter Property="FontSize" Value="{ThemeResource ControlContentThemeFontSize}"/>
        <Setter Property="Background" Value="{ThemeResource GridViewItemBackground}"/>
        <Setter Property="Foreground" Value="{ThemeResource GridViewItemForeground}"/>
        <Setter Property="TabNavigation" Value="Local"/>
        <Setter Property="IsHoldingEnabled" Value="True"/>
        <Setter Property="HorizontalContentAlignment" Value="Center"/>
        <Setter Property="VerticalContentAlignment" Value="Center"/>
        <Setter Property="Margin" Value="0,0,4,4"/>
        <Setter Property="MinWidth" Value="{ThemeResource GridViewItemMinWidth}"/>
        <Setter Property="MinHeight" Value="{ThemeResource GridViewItemMinHeight}"/>
        <Setter Property="AllowDrop" Value="False"/>
        <Setter Property="UseSystemFocusVisuals" Value="True"/>
        <Setter Property="FocusVisualMargin" Value="-2"/>
        <Setter Property="FocusVisualPrimaryBrush" Value="{ThemeResource GridViewItemFocusVisualPrimaryBrush}"/>
        <Setter Property="FocusVisualPrimaryThickness" Value="2"/>
        <Setter Property="FocusVisualSecondaryBrush" Value="{ThemeResource GridViewItemFocusVisualSecondaryBrush}"/>
        <Setter Property="FocusVisualSecondaryThickness" Value="1"/>
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="GridViewItem">
                <!-- XAML removed for clarity
                    <ListViewItemPresenter ... />
                -->   
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
    ```

    El estilo predeterminado **GridViewItem** establece una gran cantidad de propiedades. Debes comenzar siempre por una copia del estilo predeterminado y modificar solo las propiedades que necesites. De lo contrario, es probable que los elementos visuales no se muestren según lo previsto, porque algunas propiedades pudieran no estar establecidas correctamente.

    Y, como se muestra en del paso anterior, la propiedad **ItemContainerStyle** de GridView está establecida en el valor del nuevo recurso **Estilo**.

    ```xaml
        <GridView x:Name="ImageGridView"
                  Margin="0,0,0,8"
                  RelativePanel.AlignLeftWithPanel="True"
                  RelativePanel.AlignRightWithPanel="True"
                  RelativePanel.Below="TitleTextBlock"
                  ItemTemplate="{StaticResource ImageGridView_DefaultItemTemplate}"
                  ItemContainerStyle="{StaticResource ImageGridView_DefaultItemContainerStyle}"/>
    ```

3. Cambia el valor de la propiedad **Fondo** a **Gris**.

    **Antes**
    ```xaml
        <Setter Property="Background" Value="{ThemeResource GridViewItemBackground}"/>
    ```

    **Después**
    ```xaml
        <Setter Property="Background" Value="Gray"/>
    ```

4. Cambia el valor de la propiedad **Margen** a **8**.

    **Antes**
    ```xaml
        <Setter Property="Margin" Value="0,0,4,4"/>
    ```

    **Después**
    ```xaml
        <Setter Property="Margin" Value="8"/>
    ```

Ejecuta la aplicación y mira su aspecto actual. Cambiar el tamaño de la ventana de la aplicación. **GridView** se encarga de reorganizar las imágenes para ti, pero con determinados anchos queda gran cantidad de espacio en el lado derecho de la ventana de la aplicación. Sería mejor si se centraran las imágenes. No ocuparemos de esto a continuación.

![Punto de control 3 de la interfaz de usuario de la aplicación](images/xaml-basics/layout-2.png)

> [!Note]
> Si quieres experimentar, intenta establecer las propiedades Fondo y Margen en distintos valores y mira los efectos que tiene.

## <a name="part-5-apply-some-final-adjustments-to-the-layout"></a>Parte 5: aplicar algunos ajustes finales al diseño

Para centrar las imágenes en la página, debes ajustar la alineación de la cuadrícula de la página. ¿O es necesario ajustar la alineación de las imágenes en **GridView**? ¿Importa eso? Vamos a verlo.

Para obtener más información sobre alineación, consulta [Alineación, márgenes y relleno](../layout/alignment-margin-padding.md).

(En este paso, podrías intentar establecer el **Fondo** de **GridView** a tu color favorito. Te permitirá ver más claramente lo qué está sucediendo con el diseño).

**Modificar la alineación de las imágenes**

1. En **Gridview**, establece la propiedad **HorizontalAlignment** en **Centro**.

    **Antes**
    ```xaml
        <GridView x:Name="ImageGridView"
                  Margin="0,0,0,8"
                  RelativePanel.AlignLeftWithPanel="True"
                  RelativePanel.AlignRightWithPanel="True"
                  RelativePanel.Below="TitleTextBlock"
                  ItemTemplate="{StaticResource ImageGridView_DefaultItemTemplate}"
                  ItemContainerStyle="{StaticResource ImageGridView_DefaultItemContainerStyle}"/>
    ```

    **Después**
    ```xaml
        <GridView x:Name="ImageGridView"
                  Margin="0,0,0,8"
                  RelativePanel.AlignLeftWithPanel="True"
                  RelativePanel.AlignRightWithPanel="True"
                  RelativePanel.Below="TitleTextBlock"
                  ItemTemplate="{StaticResource ImageGridView_DefaultItemTemplate}"
                  ItemContainerStyle="{StaticResource ImageGridView_DefaultItemContainerStyle}" 
                  HorizontalAlignment="Center"/>
    ```

2. Ejecuta la aplicación y cambia el tamaño de la ventana. Desplázate hacia abajo para ver más imágenes.

    Las imágenes aparecen centradas, lo que da mejor aspecto. Sin embargo, la barra de desplazamiento está alineada con el borde de **GridView** en lugar de con el borde de la ventana. Para corregir esto, tendrás que centrar las imágenes en **GridView**, en lugar de centrar **GridView** en la página. Es un poco más de trabajo, pero al final tendrá un aspecto mejor.

3. Quitar la configuración **HorizontalAlignment** del paso anterior.

4. En Esquema del documento, haz clic en **ImageGridView**. En el menú contextual, selecciona **Editar plantillas adicionales > Editar el diseño de elementos (ItemsPanel) > Editar una copia...**. Se abrirá el cuadro de diálogo **Crear recurso**.

5. En el cuadro de diálogo, cambia el valor de Nombre (clave) a **ImageGridView_ItemsPanelTemplate**y, a continuación, haz clic en **Aceptar**.

    Se agregará una copia de la **ItemsPanelTemplate** predeterminada a la sección **Page.Resources** de tu XAML. (Y, como antes, **GridView** se actualiza para hacer referencia a este recurso).

    ```xaml
    <ItemsPanelTemplate x:Key="ImageGridView_ItemsPanelTemplate">
        <ItemsWrapGrid Orientation="Horizontal" />
    </ItemsPanelTemplate>
    ```

    Al igual que usaste diversos paneles para diseñar los controles de tu aplicación, **GridView** tiene un panel interno que administra el diseño de sus elementos. Ahora que tienes acceso a este panel (**ItemsWrapGrid**), puedes modificar sus propiedades para cambiar el diseño de elementos de dentro de **GridView**.

6. En **ItemsWrapGrid**, establece la propiedad **HorizontalAlignment** en **Centro**.

    **Antes**
    ```xaml
    <ItemsPanelTemplate x:Key="ImageGridView_ItemsPanelTemplate">
        <ItemsWrapGrid Orientation="Horizontal" />
    </ItemsPanelTemplate>
    ```

    **Después**
    ```xaml
    <ItemsPanelTemplate x:Key="ImageGridView_ItemsPanelTemplate">
        <ItemsWrapGrid Orientation="Horizontal"
                       HorizontalAlignment="Center"/>
    </ItemsPanelTemplate>
    ```

7. Ejecuta la aplicación y cambia otra vez el tamaño de la ventana. Desplázate hacia abajo para ver más imágenes.

![Punto de control 4 de la interfaz de usuario de la aplicación](images/xaml-basics/layout-3.png)

Ahora, la barra de desplazamiento se alinea con el borde de la ventana. Bien hecho. Has creado la interfaz de usuario básica de tu aplicación.

## <a name="going-further"></a>Ir más allá

Ahora que has creado la interfaz de usuario básica de la aplicación, echa un vistazo a estos otros tutoriales, que también se basan en el ejemplo PhotoLab: 

* Agrega imágenes reales y datos en el [tutorial de enlace de datos XAML](../../data-binding/xaml-basics-data-binding.md).
* Haga que la interfaz de usuario se adapte a diferentes tamaños de pantalla en el [tutorial de diseño adaptativo XAML](xaml-basics-adaptive-layout.md).


## <a name="get-the-final-version-of-the-photolab-sample"></a>Obtener la versión final del ejemplo de PhotoLab

Este tutorial no crea la aplicación completa de edición de fotos, así que asegúrate de echar un vistazo a la [versión final](https://github.com/Microsoft/Windows-appsample-photo-lab) para ver otras funciones, como las animaciones personalizadas y la compatibilidad con teléfonos.

