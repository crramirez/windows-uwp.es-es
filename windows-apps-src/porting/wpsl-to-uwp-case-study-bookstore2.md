---
author: mcleblanc
ms.assetid: 333f67f5-f012-4981-917f-c6fd271267c6
description: "Este caso práctico, que se basa en la información proporcionada en Bookstore, comienza con una aplicación Windows Phone Silverlight que muestra datos agrupados en un LongListSelector."
title: "Caso práctico de Windows Phone Silverlight a UWP, Bookstore2"
ms.author: markl
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 7fbe63cba4a825641ea96b39c39d5845758051cb
ms.lasthandoff: 02/07/2017

---

# <a name="windows-phone-silverlight-to-uwp-case-study-bookstore2"></a>Caso práctico de Windows Phone Silverlight a UWP: Bookstore2

\[ Actualizado para las aplicaciones para UWP en Windows 10. Para leer artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Este caso práctico, que se basa en la información proporcionada en [Bookstore1](wpsl-to-uwp-case-study-bookstore1.md), comienza con una aplicación Windows Phone Silverlight que muestra datos agrupados en un **LongListSelector**. En el modelo de vista, cada instancia de la clase **Author** representa el grupo de los libros que ha escrito ese autor y, en **LongListSelector**, podemos ver la lista de libros agrupados por autor, o bien podemos alejar la vista para ver una lista de accesos directos a autores. La lista de accesos directos ofrece una navegación mucho más rápida que un desplazamiento por la lista de libros. Repasaremos los pasos de migración de la aplicación a la Plataforma universal de Windows (UWP) de Windows 10.

**Nota**   Cuando abras Bookstore2Universal\_10 en Visual Studio, si aparece el mensaje "Es necesario actualizar Visual Studio", sigue los pasos para configurar la versión de la plataforma de destino en [TargetPlatformVersion](w8x-to-uwp-troubleshooting.md).

## <a name="downloads"></a>Descargas

[Descargar la aplicación Bookstore2WPSL8 para la aplicación Windows Phone Silverlight](http://go.microsoft.com/fwlink/p/?linkid=522601).

[Descargar la aplicación Bookstore2Universal\_10 para Windows 10](http://go.microsoft.com/fwlink/?linkid=532952).

##  <a name="the-windows-phone-silverlight-app"></a>La aplicación Windows Phone Silverlight

En la siguiente ilustración se muestra el aspecto de Bookstore2WPSL8 (la aplicación que portaremos). Es un **LongListSelector** de desplazamiento vertical de libros agrupados por autor. Puedes alejar la lista de accesos directos y desde allí puedes navegar a cualquier grupo. Existen dos elementos principales en esta aplicación: el modelo de vista que proporciona el origen de datos agrupados y la interfaz de usuario que se enlaza a ese modelo de vista. Como veremos, ambas partes se pueden portar fácilmente de la tecnología de Windows Phone Silverlight a la Plataforma universal de Windows (UWP).

![aspecto de booksare2wpsl8](images/wpsl-to-uwp-case-studies/c02-01-wpsl-how-the-app-looks.png)

##  <a name="porting-to-a-windows-10-project"></a>Migración a un proyecto de Windows 10

Es una tarea muy rápida crear un nuevo proyecto en Visual Studio, copiar archivos en él desde Bookstore2WPSL8 e incluir los archivos copiados en el nuevo proyecto. Empieza creando un proyecto nuevo de Aplicación vacía (Windows Universal). Asígnale el nombre Bookstore2Universal\_10. Estos son los archivos que hay que copiar de Bookstore2WPSL8 a Bookstore2Universal\_10.

-   Copia la carpeta que contiene los archivos PNG de la imagen de portada de libro (la carpeta es \\Assets\\CoverImages). Después de copiar la carpeta, en el **Explorador de soluciones**, asegúrate de que **Mostrar todos los archivos** esté activado. Haz clic con el botń secundario en la carpeta que has copiado y haz clic en **Incluir en el proyecto**. Este comando es lo que conocemos como "incluir" archivos o carpetas en un proyecto. Cada vez que copies un archivo o carpeta, haz clic en **Actualizar** en el **Explorador de soluciones** y luego incluye el archivo o la carpeta en el proyecto. No es necesario hacer esto para los archivos que reemplaces en el destino.
-   Copia la carpeta que contiene el archivo de origen del modelo de vista (la carpeta es \\ViewModel).
-   Copia MainPage.xaml y reemplaza el archivo en el destino.

Podemos conservar los archivos App.xaml y App.xaml.cs que Visual Studio genera en el proyecto de Windows 10.

Edita el código fuente y los archivos de marcado que acabas de copiar y cambia las referencias al espacio de nombres Bookstore2WPSL8 por Bookstore2Universal\_10. Una forma rápida de hacerlo es usar la función **Reemplazar en archivos**. En el código imperativo del archivo de origen del modelo de vista, deben realizarse estos cambios de migración.

-   Cambia `System.ComponentModel.DesignerProperties` a `DesignMode` y luego usa el comando **Resolver** en él. Elimina la propiedad `IsInDesignTool` y usa IntelliSense para agregar el nombre de propiedad correcto: `DesignModeEnabled`.
-   Usa el comando **Resolver** en `ImageSource`.
-   Usa el comando **Resolve** en `BitmapImage`.
-   Elimina `using System.Windows.Media;` y `using System.Windows.Media.Imaging;`.
-   Cambia el valor devuelto por la propiedad **Bookstore2Universal\_10.BookstoreViewModel.AppName** de "BOOKSTORE2WPSL8" a "BOOKSTORE2UNIVERSAL".
-   Al igual que hicimos anteriormente en [Bookstore1](wpsl-to-uwp-case-study-bookstore1.md), actualiza la implementación de la propiedad **BookSku.CoverImage** (consulta [Enlazar una imagen a un modelo de vista](wpsl-to-uwp-case-study-bookstore1.md)).

En MainPage.xaml, debes realizar los siguientes cambios iniciales de migración.

-   Cambia `phone:PhoneApplicationPage` por `Page` (incluidas las repeticiones en la sintaxis del elemento de propiedad).
-   Elimina las declaraciones de prefijo del espacio de nombres `phone` y `shell`.
-   Cambia "clr-namespace" por "using" en la declaración de prefijo del espacio de nombres restante.
-   Elimina `SupportedOrientations="Portrait"`y `Orientation="Portrait"`, y configura **Portrait** en el manifiesto del paquete de la aplicación del nuevo proyecto.
-   Elimina `shell:SystemTray.IsVisible="True"`.
-   Los tipos de convertidores de elementos de la lista de accesos directos (que se encuentran en el marcado como recursos) se han movido al espacio de nombres [**Windows.UI.Xaml.Controls.Primitives**](https://msdn.microsoft.com/library/windows/apps/br209818). Por lo tanto, agrega la declaración de prefijo del espacio de nombres Windows\_UI\_Xaml\_Controls\_Primitives y asígnala a **Windows.UI.Xaml.Controls.Primitives**. En los recursos del convertidor del elemento de la lista de accesos directos, cambia el prefijo de `phone:` a `Windows_UI_Xaml_Controls_Primitives:`.
-   Al igual que hicimos en [Bookstore1](wpsl-to-uwp-case-study-bookstore1.md), reemplaza todas las referencias al estilo `PhoneTextExtraLargeStyle` **TextBlock** con una referencia a `SubtitleTextBlockStyle`, reemplaza `PhoneTextSubtleStyle` con `SubtitleTextBlockStyle`, reemplaza `PhoneTextNormalStyle` con `CaptionTextBlockStyle` y reemplaza `PhoneTextTitle1Style` con `HeaderTextBlockStyle`.
-   Existe una excepción en `BookTemplate`. El estilo del segundo **TextBlock** debe hacer referencia a `CaptionTextBlockStyle`.
-   Quita el atributo FontFamily de **TextBlock** dentro de `AuthorGroupHeaderTemplate` y establece el Background de **Border** de modo que haga referencia a `SystemControlBackgroundAccentBrush` y no a `PhoneAccentBrush`.
-   Debido a los [cambios relacionados con los píxeles de visualización](wpsl-to-uwp-porting-xaml-and-ui.md), pasa por el marcado y multiplica todas las dimensiones de tamaño fijo (márgenes, ancho, alto, etc.) por 0,8.

## <a name="replacing-the-longlistselector"></a>Reemplazar LongListSelector


El reemplazo de **LongListSelector** por un control [**SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/hh702601) requiere varios pasos, así que vamos a empezar. Un **LongListSelector** se enlaza directamente al origen de datos agrupados, pero un **SemanticZoom** contiene controles [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878) o [**GridView**](https://msdn.microsoft.com/library/windows/apps/br242705), que se enlazan indirectamente a los datos a través de un adaptador [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/br209833). **CollectionViewSource** debe estar presente en el marcado como un recurso, así que vamos a comenzar agregando eso en el marcado de MainPage.xaml, dentro de `<Page.Resources>`.

```xml
    <CollectionViewSource
        x:Name="AuthorHasACollectionOfBookSku"
        Source="{Binding Authors}"
        IsSourceGrouped="true"/>
```

Ten en cuenta que el enlace en **LongListSelector.ItemsSource** se convierte en el valor de **CollectionViewSource.Source**, y que **LongListSelector.IsGroupingEnabled** se convierte en **CollectionViewSource.IsSourceGrouped**. **CollectionViewSource** tiene un nombre (nota: no es una clave, como cabría esperar) para que podamos establecer enlaces con él.

A continuación, reemplaza `phone:LongListSelector` con este marcado, lo que nos proporcionará un **SemanticZoom** preliminar con el que trabajar.

```xml
    <SemanticZoom>
        <SemanticZoom.ZoomedInView>
            <ListView
                ItemsSource="{Binding Source={StaticResource AuthorHasACollectionOfBookSku}}"
                ItemTemplate="{StaticResource BookTemplate}">
                <ListView.GroupStyle>
                    <GroupStyle
                        HeaderTemplate="{StaticResource AuthorGroupHeaderTemplate}"
                        HidesIfEmpty="True"/>
                </ListView.GroupStyle>
            </ListView>
        </SemanticZoom.ZoomedInView>
        <SemanticZoom.ZoomedOutView>
            <ListView
                ItemsSource="{Binding CollectionGroups, Source={StaticResource AuthorHasACollectionOfBookSku}}"
                ItemTemplate="{StaticResource ZoomedOutAuthorTemplate}"/>
        </SemanticZoom.ZoomedOutView>
    </SemanticZoom>
```

La noción **LongListSelector** de los modos de lista plana y lista de accesos directos se responde en la noción **SemanticZoom** de una vista acercada y alejada, respectivamente. La vista ampliada es una propiedad y la puedes establecer en una instancia de un objeto **ListView**. En este caso, la vista alejada también se establece en un objeto **ListView** y ambos controles **ListView** están enlazados a nuestro objeto **CollectionViewSource**. La vista acercada usa la misma plantilla de elemento, plantilla de encabezado de grupo y opción de configuración **HideEmptyGroups** (ahora denominada **HidesIfEmpty**) que la lista plana del objeto **LongListSelector**. Asimismo, la vista alejada usa una plantilla de elemento muy similar a la que se encuentra en el interior del estilo de lista de accesos directos **LongListSelector** (`AuthorNameJumpListStyle`). Ten en cuenta también que la vista alejada se enlaza a una propiedad especial de **CollectionViewSource** denominada **CollectionGroups**, que es una colección que contiene los grupos en lugar de los elementos.

Ahora ya no necesitamos `AuthorNameJumpListStyle`, al menos no en su totalidad. Solo necesitamos la plantilla de datos para los grupos (que son los autores de esta aplicación) en la vista alejada. Por lo tanto, se debe eliminar el estilo `AuthorNameJumpListStyle` y reemplazarlo por esta plantilla de datos.

```xml
   <DataTemplate x:Key="ZoomedOutAuthorTemplate">
        <Border Margin="9.6,0.8" Background="{Binding Converter={StaticResource JumpListItemBackgroundConverter}}">
            <TextBlock Margin="9.6,0,9.6,4.8" Text="{Binding Group.Name}" Style="{StaticResource SubtitleTextBlockStyle}"
            Foreground="{Binding Converter={StaticResource JumpListItemForegroundConverter}}" VerticalAlignment="Bottom"/>
        </Border>
    </DataTemplate>
```

Ten en cuenta que, dado que el contexto de datos de esta plantilla de datos es un grupo en lugar de un elemento, establecemos un enlace a una propiedad especial denominada **Group**.

Ahora puedes compilar y ejecutar la aplicación. Este es su aspecto en el emulador móvil.

![la aplicación para uwp en versión móvil, con los cambios en el código fuente inicial](images/wpsl-to-uwp-case-studies/c02-02-mob10-initial-source-code-changes.png)

El modelo de vista y las vistas acercada y alejada funcionan juntos correctamente, aunque un problema es que debemos trabajar un poco más en estilos y plantillas. Por ejemplo, los estilos y pinceles correctos aún no se usan, por lo que el texto es invisible en los encabezados de grupo en los que puedes hacer clic para alejar. Si ejecutas la aplicación en un dispositivo de escritorio, verás un segundo problema, que es que la aplicación aún no adapta la interfaz de usuario para ofrecer la mejor experiencia y el mejor uso de espacio en dispositivos más grandes donde las ventanas pueden ser mucho mayores que la pantalla de un dispositivo móvil. En las siguientes secciones ([Aplicación inicial plantillas y estilos](#initial-styling-and-templating), [Interfaz de usuario adaptativa](#adaptive-ui) y [Aplicación de estilo final](#final-styling)) solucionaremos estos problemas.

## <a name="initial-styling-and-templating"></a>Aplicación inicial de plantillas y estilos

Para espaciar bien los encabezados de grupo, edita `AuthorGroupHeaderTemplate` y establece el valor **Margin** de **Border** en `"0,0,0,9.6"`.

Para espaciar bien los elementos de libro, edita `BookTemplate` y establece el valor **Margin** de ambos **TextBlock** en `"9.6,0"`.

Para diseñar el nombre de la aplicación y el título de página un poco mejor, dentro de `TitlePanel`, quita el **Margin** superior en el segundo **TextBlock** estableciendo el valor en `"7.2,0,0,0"`. En el propio `TitlePanel` , establece el margen en `0` (o en cualquier valor que te parezca adecuado).

Cambia el fondo de `LayoutRoot` a `"{ThemeResource ApplicationPageBackgroundThemeBrush}"`.

## <a name="adaptive-ui"></a>Interfaz de usuario adaptativa

Dado que empezamos con una aplicación de teléfono, no es ninguna sorpresa que el diseño de interfaz de usuario de nuestra aplicación migrada realmente solo tenga sentido para dispositivos pequeños y ventanas estrechas en esta etapa del proceso. Pero realmente queremos que el diseño de la interfaz de usuario se adapte y use mejor el espacio cuando la aplicación se ejecute en una ventana ancha (que solo es posible en un dispositivo con una pantalla grande) y que solo use la interfaz de usuario que tenemos actualmente cuando la ventana de la aplicación sea estrecha (lo que sucede en un dispositivo pequeño y también puede ocurrir en un dispositivo grande).

Se puede usar la función adaptativa de Visual State Manager para lograr esto. Estableceremos las propiedades en los elementos visuales para que, de manera predeterminada, la interfaz de usuario se disponga en el estado estrecho con las plantillas que estamos usando ahora. A continuación, detectaremos cuando la ventana de la aplicación es mayor o igual a un tamaño específico (que se mide en unidades de [píxeles funcionales](wpsl-to-uwp-porting-xaml-and-ui.md)) y, como respuesta, cambiaremos las propiedades de los elementos visuales para obtener un diseño más grande y ancho. Colocaremos esos cambios de propiedades en un estado visual y usaremos un desencadenador adaptable para supervisar de forma continua y determinar si se aplica ese estado visual o no, según el ancho de la ventana en píxeles efectivos. Estamos activando el ancho de la ventana en este caso, pero también es posible activar el alto de la ventana.

Un ancho mínimo de 548 píxeles efectivos es apropiado para este caso práctico porque es el tamaño del dispositivo más pequeño en el que queremos mostrar el diseño. Los teléfonos tienen normalmente menos de 548 píxeles efectivos, por lo que en un dispositivo pequeño de este tipo, mantendríamos el diseño estrecho de forma predeterminada. En un equipo, la ventana se iniciará de forma predeterminada con un ancho suficiente para desencadenar el cambio al estado ancho, que muestra elementos de tamaño 250 x 250. Desde allí podrás arrastrar la ventana estrecha lo suficiente para mostrar un mínimo de dos columnas de elementos de tamaño 250 x 250. Si la ventana es más estrecha, el desencadenador se desactiva, se quita el estado visual ancho y se muestra el diseño estrecho predeterminado.

Antes de fijar el elemento Visual State Manager adaptativo, primero tenemos que diseñar el estado ancho y esto significa agregar algunos nuevos elementos visuales y plantillas a nuestro marcado. Estos pasos describen cómo hacerlo. Mediante las convenciones de nomenclatura de los elementos visuales y las plantillas, incluimos la palabra "wide" en el nombre de cualquier elemento o plantilla que sea para el estado ancho. Si un elemento o plantilla no contiene la palabra "wide", puedes suponer que es para el estado estrecho, que es el predeterminado y cuyos valores de propiedades se establecen como valores locales en los elementos visuales de la página. Solo los valores de propiedad para el estado ancho se establecen a través de un estado visual real en el marcado.

-   Haz una copia del control [**SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/hh702601) en el marcado y establece `x:Name="narrowSeZo"` en la copia. En el original, establece `x:Name="wideSeZo"` y establece también `Visibility="Collapsed"` para que el control ancho no sea visible de forma predeterminada.
-   En `wideSeZo`, cambia **ListView** por **GridView** en la vista acercada y la vista alejada.
-   Haz una copia de los recursos `AuthorGroupHeaderTemplate`, `ZoomedOutAuthorTemplate` y `BookTemplate`, y agrega la palabra `Wide` a las claves de las copias. Actualiza también `wideSeZo` para que haga referencia a las claves de estos nuevos recursos.
-   Reemplaza el contenido de `AuthorGroupHeaderTemplateWide` por `<TextBlock Style="{StaticResource SubheaderTextBlockStyle}" Text="{Binding Name}"/>`.
-   Reemplaza el contenido de `ZoomedOutAuthorTemplateWide` por:

```xml
    <Grid HorizontalAlignment="Left" Width="250" Height="250" >
        <Border Background="{StaticResource ListViewItemPlaceholderBackgroundThemeBrush}"/>
        <StackPanel VerticalAlignment="Bottom" Background="{StaticResource ListViewItemOverlayBackgroundThemeBrush}">
          <TextBlock Foreground="{StaticResource ListViewItemOverlayForegroundThemeBrush}"
              Style="{StaticResource SubtitleTextBlockStyle}"
            Height="80" Margin="15,0" Text="{Binding Group.Name}"/>
        </StackPanel>
    </Grid>
```

-   Reemplaza el contenido de `BookTemplateWide` por:

```xml
    <Grid HorizontalAlignment="Left" Width="250" Height="250">
        <Border Background="{StaticResource ListViewItemPlaceholderBackgroundThemeBrush}"/>
        <Image Source="{Binding CoverImage}" Stretch="UniformToFill"/>
        <StackPanel VerticalAlignment="Bottom" Background="{StaticResource ListViewItemOverlayBackgroundThemeBrush}">
            <TextBlock Style="{StaticResource SubtitleTextBlockStyle}"
                Foreground="{StaticResource ListViewItemOverlaySecondaryForegroundThemeBrush}"
                TextWrapping="NoWrap" TextTrimming="CharacterEllipsis"
                Margin="12,0,24,0" Text="{Binding Title}"/>
            <TextBlock Style="{StaticResource CaptionTextBlockStyle}" Text="{Binding Author.Name}"
                Foreground="{StaticResource ListViewItemOverlaySecondaryForegroundThemeBrush}" TextWrapping="NoWrap"
                TextTrimming="CharacterEllipsis" Margin="12,0,12,12"/>
        </StackPanel>
    </Grid>
```

-   Para el estado ancho, los grupos en la vista acercada necesitarán más espacio vertical alrededor de ellos. Si creamos y hacemos referencia a una plantilla del panel de elementos, obtendremos los resultados que queremos. Este es el aspecto del marcado.

```xml
   <ItemsPanelTemplate x:Key="ZoomedInItemsPanelTemplate">
        <ItemsWrapGrid Orientation="Horizontal" GroupPadding="0,0,0,20"/>
    </ItemsPanelTemplate>
    ...

    <SemanticZoom x:Name="wideSeZo" ... >
        <SemanticZoom.ZoomedInView>
            <GridView
            ...
            ItemsPanel="{StaticResource ZoomedInItemsPanelTemplate}">
            ...
```

-   Por último, agrega el marcado de Visual State Manager adecuado como primer elemento secundario de `LayoutRoot`.

```xml
    <Grid x:Name="LayoutRoot" ... >
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup>
                <VisualState x:Name="WideState">
                    <VisualState.StateTriggers>
                        <AdaptiveTrigger MinWindowWidth="548"/>
                    </VisualState.StateTriggers>
                    <VisualState.Setters>
                        <Setter Target="wideSeZo.Visibility" Value="Visible"/>
                        <Setter Target="narrowSeZo.Visibility" Value="Collapsed"/>
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

    ...
```

## <a name="final-styling"></a>Aplicación de estilo final

Solo quedan algunos retoques finales de estilo.

-   En `AuthorGroupHeaderTemplate`, establece `Foreground="White"` en **TextBlock** para que se vea correctamente cuando se ejecute en la familia de dispositivos móviles.
-   Agrega `FontWeight="SemiBold"` a **TextBlock**, tanto en `AuthorGroupHeaderTemplate` como en `ZoomedOutAuthorTemplate`.
-   En `narrowSeZo`, los encabezados de grupo y los autores en la vista alejada se alinean a la izquierda en lugar de aparecer estirados, así que vamos a trabajar en eso. Crearemos un [**HeaderContainerStyle**](https://msdn.microsoft.com/library/windows/apps/dn251841) para la vista acercada, con [**HorizontalContentAlignment**](https://msdn.microsoft.com/library/windows/apps/br209417) establecido en `Stretch`. Asimismo, crearemos un [**ItemContainerStyle**](https://msdn.microsoft.com/library/windows/apps/br242817) para la vista alejada que contenga ese mismo [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817). Este es el aspecto que tiene.

```xml
   <Style x:Key="AuthorGroupHeaderContainerStyle" TargetType="ListViewHeaderItem">
        <Setter Property="HorizontalContentAlignment" Value="Stretch"/>
    </Style>

    <Style x:Key="ZoomedOutAuthorItemContainerStyle" TargetType="ListViewItem">
        <Setter Property="HorizontalContentAlignment" Value="Stretch"/>
    </Style>

    ...

    <SemanticZoom x:Name="narrowSeZo" ... >
        <SemanticZoom.ZoomedInView>
            <ListView
            ...
                <ListView.GroupStyle>
                    <GroupStyle
                    ...
                    HeaderContainerStyle="{StaticResource AuthorGroupHeaderContainerStyle}"
                    ...
        <SemanticZoom.ZoomedOutView>
            <ListView
                ...
                ItemContainerStyle="{StaticResource ZoomedOutAuthorItemContainerStyle}"
                ...
```

La última secuencia de operaciones de estilo deja la aplicación con la apariencia siguiente.

![la aplicación de Windows 10 portada ejecutándose en un dispositivo de escritorio, con vista ampliada y dos tamaños de ventana](images/w8x-to-uwp-case-studies/c02-07-desk10-zi-ported.png)

La aplicación de Windows 10 migrada que se estaba ejecutando en un dispositivo de escritorio, con vista ampliada y dos tamaños de ventana  
![la aplicación de Windows 10 migrada ejecutándose en un dispositivo de escritorio, con vista ampliada y dos tamaños de ventana](images/w8x-to-uwp-case-studies/c02-08-desk10-zo-ported.png)

La aplicación de Windows 10 portada ejecutándose en un dispositivo de escritorio, con vista alejada y dos tamaños de ventana

![la aplicación de Windows 10 portada ejecutándose en un dispositivo móvil, con vista ampliada](images/w8x-to-uwp-case-studies/c02-09-mob10-zi-ported.png)

La aplicación de Windows 10 portada ejecutándose en un dispositivo móvil, con vista ampliada

![la aplicación de Windows 10 portada ejecutándose en un dispositivo móvil, con vista alejada](images/w8x-to-uwp-case-studies/c02-10-mob10-zo-ported.png)

La aplicación de Windows 10 portada ejecutándose en un dispositivo móvil, con vista alejada

## <a name="making-the-view-model-more-flexible"></a>Hacer que el modelo de vista sea más flexible

Esta sección contiene un ejemplo de las instalaciones que se nos abren después de haber movido nuestra aplicación para usar UWP. A continuación, describimos pasos opcionales que puedes seguir para que el modelo de vista sea más flexible cuando se tenga acceso a través de un **CollectionViewSource**. El modelo de vista (el archivo de origen se encuentra en ViewModel\BookstoreViewModel.cs) que hemos portado de la aplicación Windows Phone Silverlight Bookstore2WPSL8 contiene una clase denominada Author, que se deriva de **List&lt;T&gt;**, donde **T** es BookSku. Esto significa que la clase Author *es un* grupo de BookSku.

Cuando enlazamos **CollectionViewSource.Source** a Authors, lo único que comunicamos es que cada autor de Authors es un grupo de *algo*. Dejamos a **CollectionViewSource** la determinación de que Author es, en este caso, un grupo de BookSku. Eso funciona, pero no es flexible. ¿Qué ocurre si queremos que Author sea *tanto* un grupo de BookSku *como* un grupo de las direcciones en las que ha vivido el autor? El Autor no puede *ser* ambos grupos. No obstante, el Autor puede *tener* cualquier número de grupos. Y esta es la solución: usa el patrón *has-a-group* en lugar del patrón *is-a-group* que estamos usando actualmente (o usa ambos). Se hace así:

-   Cambia Author de modo que ya no derive de **List&lt;T&gt;**.
-   Agrega este campo a Author: `private ObservableCollection<BookSku> bookSkus = new ObservableCollection<BookSku>();`.
-   Agrega esta propiedad a Author: `public ObservableCollection<BookSku> BookSkus { get { return this.bookSkus; } }`.
-   Por supuesto, podemos repetir los dos pasos anteriores para agregar tantos grupos en Author como necesitamos.
-   Cambia la implementación del método AddBookSku por `this.BookSkus.Add(bookSku);`.
-   Ahora que Author *tiene* al menos un grupo, necesitamos comunicar a **CollectionViewSource** cuál de ellos debe usar. Para ello, agrega esta propiedad a **CollectionViewSource**: `ItemsPath="BookSkus"`

Esos cambios no alteran la aplicación desde el punto de vista funcional, pero ahora ya sabes cómo puedes ampliar Author y **CollectionViewSource**, de ser necesario. Vamos a realizar un último cambio en Author para que, si los usamos *sin* especificar **CollectionViewSource.ItemsPath**, se use un grupo predeterminado de nuestra elección:

```csharp
    public class Author : IEnumerable<BookSku>
    {
        ...

        public IEnumerator<BookSku> GetEnumerator()
        {
            return this.BookSkus.GetEnumerator();
        }
        System.Collections.IEnumerator System.Collections.IEnumerable.GetEnumerator()
        {
            return this.BookSkus.GetEnumerator();
        }
    }
```

Ahora podemos optar por quitar `ItemsPath="BookSkus"` y la aplicación seguirá funcionando de la misma manera.

## <a name="conclusion"></a>Conclusión

En este caso práctico se ha observado una interfaz de usuario más ambiciosa que la anterior. Todas las funciones y conceptos del **LongListSelector** de Windows Phone Silverlight (y más) han resultado estar disponibles para una aplicación para UWP mediante **SemanticZoom**, **ListView**, **GridView** y **CollectionViewSource**. Hemos mostrado cómo volver a usar o copiar y editar marcado y código imperativos en una aplicación para UWP para lograr funcionalidad, una interfaz de usuario e interacciones adaptadas para que se ajusten a los factores de forma de dispositivos Windows más anchos y más estrechos, así como a todos los tamaños intermedios.

