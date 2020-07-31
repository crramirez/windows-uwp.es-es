---
title: Tutorial para crear una interfaz de usuario
description: Este artículo describe los conceptos básicos de creación de interfaces de usuario en XAML
keywords: XAML, UWP, Getting Started
ms.date: 08/30/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 23cadaca6f82d7cb43834f5a86c56a3fa34cb00e
ms.sourcegitcommit: e1104689fc1db5afb85701205c2580663522ee6d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997802"
---
# <a name="tutorial-create-a-user-interface"></a>Tutorial: Crear una interfaz de usuario

En este tutorial aprenderás a crear una interfaz de usuario básica para un programa de edición de imágenes de las siguientes formas: 

+ Mediante las herramientas de XAML en Visual Studio, como el Diseñador XAML, Caja de herramientas, el editor XAML, el panel **Propiedades** y Esquema del documento, para agregar controles y contenido a la interfaz de usuario.
+ Mediante algunos de los paneles de diseño de XAML más comunes, como **RelativePanel**, **Grid** y **StackPanel**.

El programa de edición de imágenes tiene dos páginas. La *página principal* muestra una vista de la galería de fotos, junto con información acerca de cada archivo de imagen.

![Página principal](images/xaml-basics/mainpage.png)

La *página de detalles* muestra una sola foto después de que se haya seleccionado. Un menú de edición flotante permite modificar la foto, cambiar su nombre y guardarla.

![Página de detalles](images/xaml-basics/detailpage.png)


## <a name="prerequisites"></a>Requisitos previos

* Visual Studio 2019: [Descargar Visual Studio 2019 Community (gratuito)](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15&campaign=WinDevCenter&ocid=wdgcx-windevcenter-community-download) 
* SDK de Windows 10 (10.0.15063.468 o posterior):  [Descargar el último SDK de Windows (gratuito)](https://developer.microsoft.com/windows/downloads/windows-10-sdk)

## <a name="part-0-get-the-starter-code-from-github"></a>Parte 0: Obtener el código de inicio de GitHub

En este tutorial, empezaremos con una versión simplificada del ejemplo PhotoLab. 

1. Ve a la [página de GitHub que contiene el ejemplo](https://github.com/Microsoft/Windows-appsample-photo-lab). 
2. A continuación deberás clonar o descarga dicho ejemplo. Selecciona el botón **Clone or download** (Clonar o descargar). Aparecerá un submenú.
    Menú ![Clone or download](images/xaml-basics/clone-repo.png) (Clonar o descargar) de la página de GitHub del ejemplo de PhotoLab
    
    **Si no conoces GitHub muy bien:**
    
    a. Selecciona **Download ZIP** (Descargar ZIP) y guarda el archivo localmente. Se descargará un archivo .zip que contiene todos los archivos de proyecto necesarios.

    b. Extrae el archivo. Usa el Explorador de archivos para ir al archivo .zip que acabas de descargar, haz clic en él con el botón derecho y selecciona **Extract All**  (Extraer todo).

    c. Busca la copia local del ejemplo y ve al directorio `Windows-appsample-photo-lab-master\xaml-basics-starting-points\user-interface`.    

    **Si ya conoces GitHub:**

    a. Clona la bifurcación principal del repositorio localmente.

    b. Dirígete al directorio `Windows-appsample-photo-lab\xaml-basics-starting-points\user-interface`.

3. Abre el proyecto, para lo que debes seleccionar **Photolab.sln**.

## <a name="part-1-add-a-textblock-control-by-using-xaml-designer"></a>Parte 1: Agregar un control TextBlock mediante el Diseñador XAML

Visual Studio proporciona varias herramientas que te ayudarán a crear más fácilmente la interfaz de usuario de XAML. El Diseñador XAML se usa para arrastrar controles a la superficie de diseño y ver cómo serán antes de ejecutar la aplicación. El panel **Propiedades** permite ver y establecer todas las propiedades del control que están activas en el diseñador. Esquema del documento muestra la estructura de elementos primarios y secundarios del árbol visual de XAML de la interfaz de usuario. El editor XAML se usa para escribir directamente y modificar el marcado XAML.

Esta es la interfaz de usuario de Visual Studio con las herramientas etiquetadas.

![Diseño de Visual Studio](images/xaml-basics/visual-studio-tools.png)

Cada una de estas herramientas facilita la creación de una interfaz de usuario, así que en este tutorial las usaremos todas. Comenzarás por usar el Diseñador de XAML para agregar un control. 

Para agregar un control mediante el Diseñador XAML:

1. Haz doble clic en el archivo **MainPage.xaml** en el Explorador de soluciones para abrirlo. Se muestra la página principal de la aplicación sin ningún elemento de la interfaz de usuario.

2. Antes de continuar, debes realizar algunos ajustes en Visual Studio:

    - Asegúrate de que la plataforma de solución está establecida en x86 o x64, no en ARM.
    - Establece la página principal del Diseñador XAML para que muestre la vista previa de escritorio de 13,3".

    Deberías ver ambas configuraciones junto a la parte superior de la ventana, como se muestra aquí.

    ![Configuración de Visual Studio](images/xaml-basics/layout-vs-settings.png)

    Puedes ejecutar la aplicación ahora, pero no verás gran cosa. Vamos a agregar algunos elementos de interfaz de usuario para que las cosas sean más interesantes.

3. En Caja de herramientas, expande **Controles XAML comunes** y busca el control [TextBlock](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock). Arrastra un control **TextBlock** a la superficie de diseño, cerca de la esquina superior izquierda de la página.

    El control **TextBlock** se agrega a la página y el diseñador establece algunas propiedades automáticamente para el diseño que quieres. El control **TextBlock** aparece resaltado en azul, lo que indica que ahora es el objeto activo. Observa los márgenes y otros valores de configuración que ha agregado el diseñador. 
    
    Tu código XAML será similar al siguiente. No te preocupes si el formato no es exactamente el mismo. Aquí se ha abreviado para facilitar la lectura.

    ```xaml
    <TextBlock x:Name="textBlock"
               HorizontalAlignment="Left"
               Margin="351,44,0,0"
               TextWrapping="Wrap"
               Text="TextBlock"
               VerticalAlignment="Top"/>
    ```

    En los siguientes pasos, actualizarás estos valores.

4. En el panel **Propiedades**, cambia el valor del campo **Name** (Nombre) del control **TextBlock** de **textBlock** a **TitleTextBlock**. (asegúrate de que el control **TextBlock** sigue siendo el objeto activo).

5. En **Common** (Común), cambia el valor de **Text** (Texto) a **Collection**.

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

6. Para colocar el control **TextBlock**, primero hay que quitar los valores de propiedades que Visual Studio ha agregado. En Esquema del documento, haz clic con el botón derecho en **TitleTextBlock** y selecciona **Layout** > **Reset All** (Diseño > Restablecer todo).

    ![Esquema del documento](images/xaml-basics/doc-outline-reset.png)

7. En el panel **Propiedades**, especifica **margin** en el cuadro de búsqueda para encontrar fácilmente la propiedad **Margin**. Establece los márgenes izquierdo y derecho en 24.

    ![Márgenes de TextBlock](images/xaml-basics/margins.png)

    Los márgenes proporcionan el posicionamiento más básico de un elemento en la página. Son útiles para ajustar el diseño, pero se recomienda no usar valores grandes en los márgenes, como los que agrega Visual Studio, ya que dificultan la adaptación de la interfaz de usuario a los distintos tamaños de pantalla.

    Para obtener más información, consulta [Alineación, márgenes y relleno](../layout/alignment-margin-padding.md).

8. En Esquema del documento, haz clic con el botón derecho en **TitleTextBlock** y, luego, selecciona **Edit Style** > **Apply Resource** > **TitleTextBlockStyle** (Editar estilo > Aplicar recurso > TitleTextBlockStyle). De esta forma se aplica un estilo definido por el sistema al texto del título.

    ```xaml
    <TextBlock x:Name="TitleTextBlock"
               TextWrapping="Wrap"
               Text="Collection"
               Margin="24,0,0,24"
               Style="{StaticResource TitleTextBlockStyle}"/>
    ```

9. En el panel **Propiedades**, escribe **textwrapping** en el cuadro de búsqueda para buscar la propiedad **TextWrapping**. Selecciona el _marcador de propiedad_ de la propiedad **TextWrapping** para abrir su menú (el marcador de propiedad es el símbolo de cuadro pequeño situado a la derecha del valor de cada propiedad. Su color es negro para indicar que la propiedad se ha establecido en un valor no predeterminado). En el menú **Property** (Propiedad), selecciona **Reset** (Restablecer) para restablecer la propiedad **TextWrapping**.

    Visual Studio agrega esta propiedad, pero ya está establecida en el estilo que aplicaste, por lo que no necesitas hacerlo aquí.

Has agregado la primera parte de la interfaz de usuario a la aplicación. Ejecuta la aplicación para ver qué aspecto tiene.

Es posible que hayas observado que en el Diseñador XAML, la aplicación mostraba el texto en blanco sobre un fondo negro. Sin embargo, al ejecutar la aplicación, el texto aparecía negro y el fondo blanco. Eso es porque Windows tiene un tema oscuro y un tema claro y el tema predeterminado varía según el dispositivo. En un PC, el tema predeterminado es el claro. Para que el aspecto de la aplicación sea el mismo en el PC y en el Diseñador XAML, selecciona el icono del engranaje en la parte superior del Diseñador XAML para abrir la configuración de vista previa del dispositivo y cambiar el tema a claro.

> [!NOTE]
> En esta parte del tutorial, has agregado un control arrastrándolo. También puedes agregar un control haciendo doble clic en él, en Caja de herramientas. Pruébalo y observa las diferencias en el código XAML que Visual Studio genera.

## <a name="part-2-add-a-gridview-control-by-using-the-xaml-editor"></a>Parte 2: Agregar un control GridView desde el editor de XAML

En la primera parte, has recibido algunas nociones básicas acerca del uso del Diseñador XAML y de otras herramientas proporcionadas por Visual Studio. Aquí, usarás el editor XAML para trabajar directamente con el marcado XAML. A medida que te vayas familiarizando con XAML, es posible que resulte una manera más eficaz de trabajar.

En primer lugar, vas a reemplazar el diseño raíz [Grid](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.grid) por [RelativePanel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.relativepanel). **RelativePanel** facilita la reorganización de los fragmentos de la interfaz de usuario relativos al panel o a otras partes de la interfaz de usuario. Verás su utilidad en el tutorial [Diseños adaptables de XAML](xaml-basics-adaptive-layout.md). 

A continuación, agregarás un control [GridView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.gridview) para mostrar los datos.

Para agregar un control desde el editor de XAML:

1. En el editor de XAML, cambia la raíz **Grid** a **RelativePanel**.

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

    Para más información sobre el diseño mediante **RelativePanel**, consulta [Paneles de diseño](https://docs.microsoft.com/windows/uwp/layout/layout-panels#relativepanel).

2. Debajo del elemento **TextBlock**, agrega un control **GridView** denominado **ImageGridView**. Establece las _propiedades adjuntas_ de **RelativePanel** para colocar el control debajo del texto del título y estirarlo a todo el ancho de la pantalla.

    **Agrega este XAML**

    ```xaml
    <GridView x:Name="ImageGridView"
              Margin="0,0,0,8"
              RelativePanel.AlignLeftWithPanel="True"
              RelativePanel.AlignRightWithPanel="True"
              RelativePanel.Below="TitleTextBlock"/>
    ```

    **Después de TextBlock**
    ```xaml
    <RelativePanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <TextBlock x:Name="TitleTextBlock"
                   Text="Collection"
                   Margin="24,0,0,24"
                   Style="{StaticResource TitleTextBlockStyle}"/>
        
        <!-- Add the GridView control here. -->

    </RelativePanel>
    ```

    Para más información sobre las propiedades adjuntas del panel, consulta [Paneles de diseño](https://docs.microsoft.com/windows/uwp/layout/layout-panels).

3. Para que el control **GridView** muestre algo, es preciso asignarle la colección de datos que debe mostrar. Abre MainPage.xaml.cs y busca el método **GetItemsAsync**. Este método rellena una colección denominada **Images**, que es una propiedad que se ha agregado a **MainPage**.

    Después del bucle **foreach** de **GetItemsAsync**, agrega esta línea de código.

    ```csharp
    ImageGridView.ItemsSource = Images;
    ```

    En este paso se establece la propiedad **ItemsSource** del control **GridView** en la colección de **Images** de la aplicación. También proporciona al control **GridView** elementos que puede mostrar.

Este es un buen lugar para ejecutar la aplicación y asegurarse de que todo funcione. Debería tener un aspecto similar a este.

![Punto de control 1 de la interfaz de usuario de la aplicación](images/xaml-basics/layout-0.png)

Observarás que la aplicación todavía no muestra imágenes. De manera predeterminada, muestra el valor **ToString** del tipo de datos que está en la colección. A continuación, crearás una plantilla de datos para definir cómo se muestran los datos.

> [!NOTE]
> Puedes encontrar más información sobre el diseño mediante **RelativePanel** en el artículo acerca de los [paneles de diseño](https://docs.microsoft.com/windows/uwp/layout/layout-panels#relativepanel). Una vez que lo hayas leído, prueba los diferentes diseños, para lo que debes establecer las propiedades adjuntas de **RelativePanel** en **TextBlock**k y **GridView**.

## <a name="part-3-add-a-datatemplate-object-to-display-your-data"></a>3\.ª parte: Agregar un objeto DataTemplate para mostrar los datos

Ahora, vas a crear un objeto [DataTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.datatemplate) que indique al control **GridView** cómo debe mostrar sus datos. Para obtener una explicación completa de las plantillas de datos, consulta [contenedores y plantillas de elementos](../controls-and-patterns/item-containers-templates.md).

Por ahora, solo vas a agregar marcadores de posición que te ayuden a crear el diseño que quieres. En el tutorial acerca del [enlace de datos XAML](../../data-binding/xaml-basics-data-binding.md), vas a reemplazar estos marcadores de posición por datos reales de la clase **ImageFileInfo**. Puedes abrir ahora el archivo ImageFileInfo.cs, si quieres ver el aspecto que tiene el objeto de datos.

Para agregar una plantilla de datos a una vista de cuadrícula:

1. Abre MainPage.xaml.

2. Para mostrar la clasificación, usa el control **RadRating** del paquete NuGet [Telerik UI for UWP](https://github.com/telerik/UI-For-UWP). Agrega una referencia de espacio de nombres XAML que especifique el espacio de nombres para los controles de Telerik. Colócala en la etiqueta **Page** inicial, inmediatamente después del resto de entradas `xmlns:`.

    **Agrega este XAML**

    ```xaml
    xmlns:telerikInput="using:Telerik.UI.Xaml.Controls.Input"
    ```

    **Después de la última entrada`xmlns:`**

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

3. En Esquema del documento, haz clic con el botón derecho en **ImageGridView**. En el menú contextual, selecciona **Edit Additional Templates** > **Edit Generated Items (ItemTemplate)**  > **Create Empty** (Editar plantillas adicionales > Editar elementos generados (ItemTemplate) > Crear vacío). Se abre el cuadro de diálogo **Create Resource** (Crear recurso).

4. En el cuadro de diálogo, cambia el valor **Name (key)** [Nombre (clave)] a **ImageGridView_DefaultItemTemplate** y selecciona **Aceptar**.

    Al seleccionar **Aceptar**, se producen varias acciones:

    - Se agrega un objeto **DataTemplate** a la sección `Page.Resources` de MainPage.xaml.

        ```xaml
        <Page.Resources>
            <DataTemplate x:Key="ImageGridView_DefaultItemTemplate">
                <Grid/>
            </DataTemplate>
        </Page.Resources>
        ```

    - Como ámbito de Esquema del documento se selecciona este objeto **DataTemplate**.

        Cuando hayas terminado de crear la plantilla de datos, puedes seleccionar la flecha de la esquina superior izquierda de Esquema del documento para volver al ámbito de la página.

    - La propiedad **ItemTemplate** del control **GridView** se establece en el recurso **DataTemplate**.

       ```xaml
           <GridView x:Name="ImageGridView"
                     Margin="0,0,0,8"
                     RelativePanel.AlignLeftWithPanel="True"
                     RelativePanel.AlignRightWithPanel="True"
                     RelativePanel.Below="TitleTextBlock"
                     ItemTemplate="{StaticResource ImageGridView_DefaultItemTemplate}"/>
       ```

5. En el recurso **ImageGridView_DefaultItemTemplate**, asigna a la raíz **Grid** un alto y un ancho de **300**, y un margen de **8**. Después, agrega dos filas y establece el alto de la segunda fila en **Auto** (Automático).

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

    Para más información sobre los diseños de **Grid**, consulta [Paneles de diseño](https://docs.microsoft.com/windows/uwp/layout/layout-panels#grid).

6. Agregue controles al diseño **Grid**.

    a. Agrega un control **Image** en la primera fila de la cuadrícula. Aquí es donde se mostrará la imagen, pero, por ahora, vas a usar el logotipo de la tienda de aplicaciones como marcador de posición.

    b. Agrega controles **TextBlock** para mostrar el nombre, el tipo de archivo y las dimensiones de la imagen. Para ello, usa los controles **StackPanel** para organizar los bloques de texto.

    Para más información sobre el diseño **StackPanel**, consulta [Paneles de diseño](https://docs.microsoft.com/windows/uwp/layout/layout-panels#stackpanel).

    c. Agrega el control **RadRating** al control **StackPanel** exterior (vertical). Colócalo después del control **StackPanel** interior (horizontal).

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

Ejecuta la aplicación para ver el control **GridView** con la plantilla de elementos que acabas de crear. Sin embargo, es posible que no veas el control de clasificación, ya que tiene estrellas blancas sobre fondo blanco. A continuación, cambiarás el color de fondo.

![Punto de control 2 de la interfaz de usuario de la aplicación](images/xaml-basics/layout-1.png)

## <a name="part-4-modify-the-item-container-style"></a>4\.ª parte: Modificar el estilo del contenedor de elementos

La plantilla de control de un elemento contiene los elementos visuales que muestran el estado, como la selección, el paso del puntero y el foco. Estos elementos visuales se representan encima o debajo de la plantilla de datos. Aquí, modificarás las propiedades **Background** y **Margin** de la plantilla de control para dar a los elementos de **GridView** un fondo gris.

Para modificar el contenedor de elementos:

1. En Esquema del documento, haz clic con el botón derecho en **ImageGridView**. En el menú contextual, selecciona **Edit Additional Templates** > **Edit Generated Item Container (ItemContainerStyle)**  > **Edit a Copy** Editar plantillas adicionales > Editar contenedor de elementos generados (ItemContainerStyle) > Editar una copia). Se abre el cuadro de diálogo **Create Resource** (Crear recurso).

2. En el cuadro de diálogo, cambia el valor **Name (key)** [Nombre (clave)] a **ImageGridView_DefaultItemContainerStyle** y selecciona **Aceptar**.

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

    Como en el paso anterior, la propiedad **ItemContainerStyle** del control **GridView** se establece en el valor del nuevo recurso **Style**.

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

Ejecuta la aplicación y mira su aspecto actual. Cambiar el tamaño de la ventana de la aplicación. El control **GridView** se encarga de reorganizar las imágenes automáticamente, pero en el caso de determinados anchos queda mucho de espacio en el lado derecho de la ventana de la aplicación. Sería mejor si se centraran las imágenes. No ocuparemos de esto a continuación.

![Punto de control 3 de la interfaz de usuario de la aplicación](images/xaml-basics/layout-2.png)

> [!Note]
> Si quieres experimentar, intenta establecer las propiedades **Background** y **Margin** en distintos valores y comprueba el efecto de dicho cambio.

## <a name="part-5-apply-some-final-adjustments-to-the-layout"></a>5\.ª parte: Aplicar algunos ajustes finales al diseño

Para centrar las imágenes en la página, necesitas ajustar la alineación del control **Grid** en la página. ¿O necesitas ajustar la alineación de las imágenes en **GridView**? ¿Importa eso? Vamos a verlo.

Para obtener más información sobre alineación, consulta [Alineación, márgenes y relleno](../layout/alignment-margin-padding.md).

(en este paso, puedes intentar establecer la propiedad **Background** de **GridView** en tu color favorito. Te permitirá ver más claramente lo qué está sucediendo con el diseño).

Para modificar la alineación de las imágenes:

1. En **GridView**, establece la propiedad **HorizontalAlignment** en **Center** (Centro).

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

    Las imágenes aparecen centradas, lo que da mejor aspecto. Sin embargo, la barra de desplazamiento está alineada con el borde del control **GridView**, en lugar de con el borde de la ventana. Para corregir este problema, tendrás que centrar las imágenes en **GridView**, en lugar de centrar **GridView** en la página. Es un poco más de trabajo, pero al final tendrá un aspecto mejor.

3. Quitar la configuración **HorizontalAlignment** del paso anterior.

4. En Esquema del documento, haz clic con el botón derecho en **ImageGridView**. En el menú contextual, selecciona **Edit Additional Templates** > **Edit Layout of Items (ItemsPanel)**  > **Edit A Copy** [Editar plantillas adicionales > Editar el diseño de elementos (ItemsPanel) > Editar una copia]. Se abre el cuadro de diálogo **Create Resource** (Crear recurso).

5. En el cuadro de diálogo, cambia el valor de **Name (key**)[Nombre (clave)] a **ImageGridView_ItemsPanelTemplate**y selecciona **Aceptar**.

    Se agregará una copia de la **ItemsPanelTemplate** predeterminada a la sección **Page.Resources** de tu XAML. (y, como antes, **GridView** se actualiza para hacer referencia a este recurso).

    ```xaml
    <ItemsPanelTemplate x:Key="ImageGridView_ItemsPanelTemplate">
        <ItemsWrapGrid Orientation="Horizontal" />
    </ItemsPanelTemplate>
    ```

    De la misma forma que has usado diversos paneles para crear el diseño de los controles de la aplicación, **GridView** tiene un panel interno que administra el diseño de sus elementos. Ahora que tienes acceso a este panel (**ItemsWrapGrid**), puedes modificar sus propiedades para cambiar el diseño de elementos de dentro del control **GridView**.

6. En **ItemsWrapGrid**, establece la propiedad **HorizontalAlignment** en **Center** (Centro).

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

Ahora, la barra de desplazamiento está alineada con el borde de la ventana. Bien hecho. Has creado la interfaz de usuario básica de tu aplicación.

## <a name="go-further"></a>Continúa

Una vez que has creado la interfaz de usuario básica, puedes consultar estos otros tutoriales. También se basan en el ejemplo de PhotoLab. 

* Agrega imágenes reales y datos en el [tutorial de enlace de datos XAML](../../data-binding/xaml-basics-data-binding.md).
* Haz que la interfaz de usuario se adapte a diferentes tamaños de pantalla con el [tutorial de diseño adaptativo XAML](xaml-basics-adaptive-layout.md).


## <a name="get-the-final-version-of-the-photolab-sample"></a>Obtener la versión final del ejemplo de PhotoLab

Este tutorial no crea la aplicación de edición fotográfica completa. Por tanto, asegúrese de examinar la [versión final](https://github.com/Microsoft/Windows-appsample-photo-lab) para ver otras funciones, como las animaciones personalizadas y los diseños adaptables.
