---
author: KarlErickson
title: Crear enlaces de datos
description: Este artículo describe los conceptos básicos del enlace de datos en XAML
keywords: XAML, UWP, Introducción
ms.author: karler
ms.date: 08/30/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8000d9105481bc177eb2fc64646aec009fd80d36
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "6850497"
---
# <a name="create-data-bindings"></a>Crear enlaces de datos

Imagina que has implementado una interfaz de usuario de buen aspecto rellena con imágenes de marcadores de posición, un texto "lorem ipsum" repetitivo y controles que por ahora no hacen nada. A continuación, querrás conectarlo a datos reales y transformarlo desde un prototipo de diseño en una aplicación viva. 

En este tutorial, aprenderás cómo sustituir tu esquema repetitivo por enlaces de datos y crear otros vínculos directos entre la interfaz de usuario y los datos. También aprenderás a dar formato o convertir los datos para visualizarlos y a mantener sincronizados los datos y la interfaz de usuario. Al completar este tutorial, podrás mejorar la simplicidad y la organización del código XAML y C#, agregando facilidad de mantenimiento y ampliación.

Empezaremos con una versión simplificada del ejemplo PhotoLab. Esta versión para principiantes incluye los diseños completos de la capa de datos y el XAML básico, dejando muchas funciones para hacer más fácil examinar el código. Este tutorial no crea aplicaciones completas, así que asegúrate de echar un vistazo a la versión final para ver funciones como las animaciones personalizadas y la compatibilidad con teléfonos. Puedes encontrar la versión final en la carpeta raíz del repositorio [Windows-appsample-photo-lab](https://github.com/Microsoft/Windows-appsample-photo-lab). 

## <a name="prerequisites"></a>Requisitos previos

* [Visual Studio 2017 y la versión más reciente del SDK de Windows 10](https://developer.microsoft.com/windows/downloads).

## <a name="part-0-get-the-code"></a>Parte 0: Obtener el código
El punto de partida de este laboratorio se encuentra en el repositorio de ejemplos PhotoLab, en la carpeta [xaml-basics-starting-points/data-binding](https://github.com/Microsoft/Windows-appsample-photo-lab/tree/master/xaml-basics-starting-points/data-binding). Después de clonar o descargar el repositorio, puedes editar el proyecto abriendo PhotoLab.sln con Visual Studio 2017.

La aplicación PhotoLab tiene dos páginas principales:

**MainPage.xaml:** Muestra una vista de galería de fotos, junto con alguna información sobre cada archivo de imagen.
![MainPage](../design/basics/images/xaml-basics/mainpage.png)

**DetailPage.xaml:** Muestra una sola foto después de que se haya seleccionado. Un menú de edición flotante permite modificar la foto, cambiar su nombre y guardarla.
![DetailPage](../design/basics/images/xaml-basics/detailpage.png)

## <a name="part-1-replace-the-placeholders"></a>Parte 1: Reemplazar los marcadores de posición

Aquí crearás enlaces de un solo uso en el XAML de la plantilla de datos para mostrar imágenes reales y metadatos de imágenes en lugar del contenido de los marcadores de posición. 

Los enlaces de un solo uso son para datos de solo lectura, que no cambian, lo que significa que son de alto rendimiento y fáciles de crear, lo que te permite mostrar grandes conjuntos de datos en controles **GridView** y **ListView**. 

**Reemplazar los marcadores de posición por enlaces de un solo uso**

1. Abre la carpeta xaml-basics-starting-points\data-binding e inicia el archivo PhotoLab.sln. 

2. Asegúrate de que la plataforma de soluciones esté establecida en x86 o x64, no ARM y, a continuación, ejecuta la aplicación. Muestra el estado de la aplicación con marcadores de posición de interfaz de usuario, antes de que se hayan agregado los enlaces. 

    ![Aplicación en ejecución con imágenes y texto de marcadores de posición](../design/basics/images/xaml-basics/gallery-with-placeholder-templates.png)

3. Abre MainPage.xaml y busca una **DataTemplate** denominada **ImageGridView_DefaultItemTemplate**. Actualizaremos esta plantilla para usar enlaces de datos. 

    **Antes:**
    ```xaml
    <DataTemplate x:Key="ImageGridView_DefaultItemTemplate">
    ```

    El valor **x:Key** lo usa **ImageGridView** para seleccionar esta plantilla para mostrar objetos de datos. 

4. Agrega un valor **x:DataType** a la plantilla. 

    **Después:**
    ```xaml
    <DataTemplate x:Key="ImageGridView_DefaultItemTemplate" 
                  x:DataType="local:ImageFileInfo">
    ```

    **x:DataType** indica para qué tipo es una plantilla. En este caso, es una plantilla para la clase **ImageFileInfo** (donde "local:" indica el espacio de nombres local, como se define en una declaración xmlns cerca de la parte superior del archivo).
    
    **x:DataType** es necesario al usar expresiones **x:Bind** en una plantilla de datos, como se describe a continuación. 

5. En la **DataTemplate**, busca el elemento **Image** denominado **ItemImage** y reemplaza su valor **Source** como se muestra. 

    **Antes:**
    ```xaml
    <Image x:Name="ItemImage" 
           Source="/Assets/StoreLogo.png" 
           Stretch="Uniform" />
    ```
    
    **Después:**
    ```xaml
    <Image x:Name="ItemImage" 
           Source="{x:Bind ImageSource}" 
           Stretch="Uniform" />
    ```
    
    **x:Name** identifica un elemento XAML, de manera que puedas hacerle referencia en otro lugar del XAML y en el código subyacente. 

    Las expresiones **x:Bind** proporcionan un valor a una propiedad de interfaz de usuario, obteniendo el valor de una propiedad **data-object**. En las plantillas, la propiedad indicada es una propiedad de lo que haya establecido **x:DataType**. Por lo tanto, en este caso, el origen de datos es la propiedad **ImageFileInfo.ImageSource**. 
    
    > [!NOTE] 
    > El valor **x: Bind** también permite hacer conocer el tipo de datos al editor, para que puedas usar IntelliSense en lugar de escribir el nombre de propiedad en una expresión **x: Bind**. Pruébalo en el código que acabas de pegar: coloca el cursor justo después **x:Bind** y presiona la barra espaciadora para ver una lista de propiedades a las que puedes enlazar.

6. Reemplaza los valores de los otros controles de interfaz de usuario de la misma manera. (¡Intenta hacerlo con IntelliSense en lugar de copiar y pegar!)

    **Antes:**
    ```xaml
    <TextBlock Text="Placeholder" ... />
    <StackPanel ... >
        <TextBlock Text="PNG file" ... />
        <TextBlock Text="50 x 50" ... />
    </StackPanel>
    <telerikInput:RadRating Value="3" ... />
    ```
    
    **Después:**
    ```xaml
    <TextBlock Text="{x:Bind ImageTitle}" ... />
    <StackPanel ... >
        <TextBlock Text="{x:Bind ImageFileType}" ... />
        <TextBlock Text="{x:Bind ImageDimensions}" ... />
    </StackPanel>
    <telerikInput:RadRating Value="{x:Bind ImageRating}" ... />
    ```

Ejecuta la aplicación para ver el aspecto que va teniendo. No hay más marcadores de posición. Ya estamos listos para un buen comienzo. 

![Aplicación en ejecución con reales imágenes y texto en lugar de marcadores de posición](../design/basics/images/xaml-basics/gallery-with-populated-templates.png)

> [!Note]
> Si deseas experimentar más, intenta agregar un nuevo TextBlock a la plantilla de datos y usa el truco de IntelliSense x: Bind para encontrar una propiedad para mostrar. 

## <a name="part-2-use-binding-to-connect-the-gallery-ui-to-the-images"></a>Parte 2: usar los enlaces para conectar la interfaz de usuario de la galería a las imágenes

Aquí, crearás de enlaces de una vez en la página XAML, para conectar la vista de galería a la colección de imágenes, reemplazando el código de procedimiento existente que realiza esta tarea en el código subyacente. También crearás un botón **Eliminar** para ver cómo cambia la vista de galería cuando se quitan las imágenes de la colección. Al mismo tiempo, aprenderás cómo enlazar eventos a controladores de eventos, para una mayor flexibilidad que la que ofrecen los controladores de eventos tradicionales. 

Todos los enlaces tratados hasta ahora están dentro de las plantillas de datos y hacen referencia a propiedades de la clase indicada por el valor **x:DataType**. ¿Qué sucede con el resto del XAML de la página? 

Las expresiones **x: Bind** de fuera de las plantillas de datos siempre están enlazadas a la propia página. Esto significa que puedes hacer referencia a lo que pongas en el código subyacente o declares en XAML, incluidas las propiedades personalizadas y las propiedades de otros controles de interfaz de usuario de la página (siempre que tengan un valor **x: Name**). 

En el ejemplo PhotoLab, un uso de un enlace como este es conectar el control principal **GridView** directamente a la colección de imágenes, en lugar de hacerlo en el código subyacente. Más adelante, podrás ver otros ejemplos. 

**Enlazar el control principal GridView a la colección de imágenes**

1. En MainPage.xaml.cs, busca el método **OnNavigatedTo** y quita el código que establece **ItemsSource**.

    **Antes:**
    ```c#
    ImageGridView.ItemsSource = Images;
    ```

    **Después:**
    ```c#
    // Replaced with XAML binding:
    // ImageGridView.ItemsSource = Images;
    ```

2. En MainPage.xaml, busca la **GridView** denominada **ImageGridView** y agrega un atributo **ItemsSource**. Para el valor, usa una expresión **x: Bind** que se refiera a las **imágenes** implementadas adecuadamente en el código subyacente. 

    **Antes:**
    ```xaml
    <GridView x:Name="ImageGridView" 
    ```

    **Después:**
    ```xaml
    <GridView x:Name="ImageGridView" 
              ItemsSource="{x:Bind Images}" 
    ```

    La propiedad **Imágenes** es de tipo **ObservableCollection\<ImageFileInfo\>**, de modo que los elementos individuales mostrados en la **GridView** son del tipo **ImageFileInfo**. Esto coincide con el valor **x:DataType** descrito en la parte 1. 

Todos los enlaces que hemos visto hasta ahora son enlaces de una vez, de solo lectura, que es el comportamiento predeterminado de expresiones **x:Bind** sencillas. Los datos se cargan solo durante la inicialización, lo que conviene para enlaces de alto rendimiento: perfectos para dar soporte a varias vistas complejas de grandes conjuntos de datos. 

Incluso el enlace **ItemsSource** que acabas de agregar es de una vez y de solo lectura para un valor de propiedad que no cambia, pero existe una diferencia importante que hay que señalar aquí. El valor que no cambia de la propiedad **Imágenes** es una instancia única de una colección, inicializada una vez, como se muestra aquí.

```Csharp
private ObservableCollection<ImageFileInfo> Images { get; }
    = new ObservableCollection<ImageFileInfo>();
```

El valor de la propiedad **Imágenes** nunca cambia pero, dado que la propiedad es de tipo **ObservableCollection\<T\>**, el *contenido* de la colección puede cambiar y el enlace notará automáticamente los cambios y actualizará la interfaz de usuario. 

Para probar esto, vamos a agregar temporalmente un botón que elimina la imagen seleccionada actualmente. Este botón no está en la versión final porque seleccionar una imagen te llevaría a una página de detalles. Sin embargo, el comportamiento de **ObservableCollection\<T\>** sigue siendo importante en la muestra final de PhotoLab, porque el código XAML se inicializa en el constructor de página (a través de la llamada al método **InitializeComponent**), pero la colección **Images** se rellena más adelante en el método **OnNavigatedTo**. 

**Agregar un botón Eliminar**

1. En MainPage.xaml, busca la **CommandBar** denominada **MainCommandBar** y agrega un nuevo botón antes del botón de zoom. (Los controles de zoom todavía no funcionan. Te dedicarás a ellos en la siguiente parte del tutorial).

    ```xaml
    <AppBarButton Icon="Delete"
                  Label="Delete selected image"
                  Click="{x:Bind DeleteSelectedImage}" />
    ```

    Si ya estás familiarizado con XAML, este valor **Click** podría tener un aspecto inusual. En versiones anteriores de XAML, tenías que establecer esto en un método con una firma específica del controlador de eventos, por lo general incluyendo parámetros para el remitente del evento y un objeto de argumentos de evento específico. Aún puedes usar esta técnica cuando necesites los argumentos del evento, pero con x:Bind, también puedes conectar con otros métodos. Por ejemplo, si no necesitas los datos del evento, puedes conectarte a los métodos que no tengan parámetros, como hacemos aquí.

    <!-- TODO add doc links about event binding - and doc links in general? -->

2. En MainPage.xaml.cs, agrega el método **DeleteSelectedImage**.

    ```c#
    private void DeleteSelectedImage() =>
        Images.Remove(ImageGridView.SelectedItem as ImageFileInfo);
    ```

    Este método simplemente elimina la imagen seleccionada de la colección **Imágenes**. 

Ahora ejecuta la aplicación y usa el botón para eliminar unas pocas imágenes. Como podrás ver, la interfaz de usuario se actualiza automáticamente, gracias a los enlaces de datos y el tipo **ObservableCollection\<T\>**. 

> [!Note]
> Como desafío, intenta agregar dos botones que muevan la imagen seleccionada hacia arriba o hacia abajo de la lista y, a continuación, realiza un x:Bind entre sus eventos Clic y dos nuevos métodos similares a DeleteSelectedImage.
 
## <a name="part-3-set-up-the-zoom-slider"></a>Parte 3: configurar el control deslizante de zoom 

En esta parte, crearás enlaces unidireccionales desde un control de la plantilla de datos al control deslizante de zoom, que está fuera de la plantilla. También aprenderás que puedes usar el enlace de datos con muchas propiedades de control, no solo con las más obvias, como **TextBlock.Text** e **Image.Source**. 

**Enlazar la plantilla de datos de imagen al control deslizante de zoom**

* Busca la **DataTemplate** denominada **ImageGridView_DefaultItemTemplate** y reemplaza los valores de **Altura** y **Anchura** del control **Rejilla** de la parte superior de la plantilla.

    **Antes**
    ```xaml
    <DataTemplate x:Key="ImageGridView_DefaultItemTemplate" 
                  x:DataType="local:ImageFileInfo">
        <Grid Height="200"
              Width="200"
              Margin="{StaticResource LargeItemMargin}">
    ```
    
    **Después**
    ```xaml
    <DataTemplate x:Key="ImageGridView_DefaultItemTemplate" 
                  x:DataType="local:ImageFileInfo">
        <Grid Height="{Binding Value, ElementName=ZoomSlider}"
              Width="{Binding Value, ElementName=ZoomSlider}"
              Margin="{StaticResource LargeItemMargin}">
    ```
    
    <!-- TODO talk about dependency properties --> 
    
    ¿Observaste que son expresiones **Enlace** y no **x:Bind**? Esta es la forma antigua de hacer enlaces de datos y mayormente está obsoleta. **x:Bind** hace casi todo lo que hace **Enlace** y mucho más. Sin embargo, cuando usas **x: Bind** en una plantilla de datos, enlaza con el tipo declarado en el valor **x:DataType**. ¿Cómo enlazas algo en la plantilla con algo de la página XAML o del código subyacente? Debes usar una expresión **Enlace** al estilo antiguo. 
    
    Las expresiones **Enlace** no reconocen el valor **x:DataType**, pero estas expresiones de **Enlace** tienen valores de **ElementName** que funcionan prácticamente de la misma manera. Estas indican al motor de enlace que **Valor de enlace** es un enlace a la propiedad **Valor** del elemento especificado en la página (es decir, el elemento con ese valor **x:Name**). Si quieres enlazar a una propiedad en el código subyacente, tendría un aspecto similar, como ```{Binding MyCodeBehindProperty, ElementName=page}```donde **página** hace referencia al valor **x:Name** establecido en el elemento **Página** en XAML. 
    
    > [!NOTE]
    > De manera predeterminada, las expresiones **Enlace** son de un *sentido*, lo que significa que actualizarán automáticamente la interfaz de usuario cuando cambie el valor de la propiedad enlazada. 
    > 
    > En cambio, el valor predeterminado de **x:Bind** es una *vez*, lo que significa que se ignoran los cambios en la propiedad enlazada. Esto está predeterminado, porque es la opción demás alto rendimiento y la mayoría de los enlaces son a datos estáticos de sólo lectura. 
    >
    > Lo que hay que aprender aquí es que si usas **x:Bind** con propiedades que pueden cambiar sus valores, asegúrate de agregar ```Mode=OneWay``` o ```Mode=TwoWay```. Podrás ver ejemplos en la sección siguiente.

Ejecuta la aplicación y usa el control deslizante para cambiar las dimensiones de la plantilla de imágenes. Como puedes ver, el efecto es muy eficaz, sin necesidad de mucho código. 

![Aplicación en ejecución mostrando el control deslizante de zoom](../design/basics/images/xaml-basics/gallery-with-zoom-control.png)

> [!NOTE]
> Como desafío, intenta enlazar otras propiedades de la interfaz de usuario a la propiedad **Valor** del control deslizante de zoom, o a otros controles deslizantes que agregues después del control deslizante de zoom. Por ejemplo, puedes enlazar la propiedad **FontSize** del **TitleTextBlock** a un nuevo control deslizante con un valor predeterminado de **24**. Asegúrate de establecer unos valores mínimos y máximos razonables.

## <a name="part-4-improve-the-zoom-experience"></a>Parte 4: mejorar la experiencia de zoom 

En esta parte, agregarás una propiedad personalizada **ItemSize** al código subyacente y crearás enlaces unidireccionales desde la plantilla de imagen a la nueva propiedad. El valor **ItemSize** se actualizará con el control deslizante de zoom y otros factores como la alternancia **Ajustar a la pantalla** y el tamaño de ventana, con lo que se obtendrá una experiencia más refinada. 

A diferencia de las propiedades de controles integrados, tus propiedades personalizadas no actualizan automáticamente la interfaz de usuario, incluso con enlaces unidireccionales y bidireccionales. Funcionan bien con enlaces de una **vez**, pero si quieres que los cambios de propiedad realmente aparezcan en la interfaz de usuario, deberás realizar algún trabajo. 

**Crear la propiedad ItemSize que actualice la interfaz de usuario**

1. En MainPage.xaml.cs, cambia la firma de la clase **MainPage** para que implemente la interfaz **INotifyPropertyChanged**.

    **Antes:**
    ```c#
    public sealed partial class MainPage : Page
    ```

    **Después:**
    ```c#
    public sealed partial class MainPage : Page, INotifyPropertyChanged
    ```

    Esto informa al sistema de enlace que MainPage tiene un evento PropertyChanged (agregado a continuación) que los enlaces pueden escuchar para actualizar la interfaz de usuario. 

2. Agregar un evento **PropertyChanged** a la clase **MainPage**.

    ```c#
    public event PropertyChangedEventHandler PropertyChanged;
    ```

    Este evento proporciona la implementación completa requerida por la interfaz **INotifyPropertyChanged**. Sin embargo, para que surta efecto, debes generar explícitamente el evento en tus propiedades personalizadas. 

3. Agrega una propiedad **ItemSize** y genera el evento **PropertyChanged** en su establecedor.

    ```c#
    public double ItemSize
    {
        get => _itemSize;
        set
        {
            if (_itemSize != value)
            {
                _itemSize = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(nameof(ItemSize)));
            }
        }
    }
    private double _itemSize;
    ```

    La propiedad **ItemSize** expone el valor de un campo privado **_itemSize**. El uso de un campo de respaldo como este permite a la propiedad comprobar si un nuevo valor es el mismo que el valor antiguo antes de generar un evento **PropertyChanged** posiblemente innecesario.

    El propio evento se genera mediante el método **Invocar**. El signo de interrogación comprueba si el evento **PropertyChanged** es nulo: es decir, si ya se han agregado los controladores de eventos. Cada enlace unidireccional o bidireccional agrega un controlador de eventos en segundo plano, pero si ninguno está escuchando, nada más sucederá aquí. Si **PropertyChanged** no es null, sin embargo, a continuación se llama a **Invoke** con una referencia al origen del evento (la propia página, representada por **esta** palabra clave) y un objeto **argumentos del evento** que indica el nombre de la propiedad. Con esta información, cualquier enlace unidireccional o bidireccional a la propiedad **ItemSize** recibirá informe de los cambios para poder actualizar la interfaz de usuario enlazada. 

4. En MainPage.xaml, busca la **DataTemplate** denominada **ImageGridView_DefaultItemTemplate** y reemplaza los valores de **Altura** y **Anchura** del control **Rejilla** de la parte superior de la plantilla. (Si hiciste el enlace de control a control en la parte anterior de este tutorial, los únicos cambios son reemplazar **Valor** con **ItemSize** y **ZoomSlider** con **página**. ¡Asegúrate de hacer esto tanto para Altura como para Anchura!)

    **Antes**
    ```xaml
    <DataTemplate x:Key="ImageGridView_DefaultItemTemplate" 
                  x:DataType="local:ImageFileInfo">
        <Grid Height="{Binding Value, ElementName=ZoomSlider}"
            Width="{Binding Value, ElementName=ZoomSlider}"
            Margin="{StaticResource LargeItemMargin}">
    ```
    
    **Después**
    ```xaml
    <DataTemplate x:Key="ImageGridView_DefaultItemTemplate" 
                  x:DataType="local:ImageFileInfo">
        <Grid Height="{Binding ItemSize, ElementName=page}"
              Width="{Binding ItemSize, ElementName=page}"
              Margin="{StaticResource LargeItemMargin}">
    ```

Ahora que la interfaz de usuario puede responder a los cambios de **ItemSize**, necesitas realmente realizar algunos cambios. Como se mencionó anteriormente, el valor **ItemSize** se calcula a partir del estado actual de diversos controles de interfaz de usuario, pero el cálculo debe realizarse siempre que estos controles cambien el estado. Para ello, usarás el enlace de eventos, para que determinados cambios de la interfaz de usuario llamen a un método auxiliar que actualiza **ItemSize**. 

**Actualizar el valor de la propiedad ItemSize**

1. Agrega el método **DetermineItemSize** a MainPage.xaml.cs.

    ```c#
    private void DetermineItemSize()
    {
        if (FitScreenToggle != null
            && FitScreenToggle.IsOn == true
            && ImageGridView != null
            && ZoomSlider != null)
        {
            // The 'margins' value represents the total of the margins around the
            // image in the grid item. 8 from the ItemTemplate root grid + 8 from
            // the ItemContainerStyle * (Right + Left). If those values change, 
            // this value needs to be updated to match.
            int margins = (int)this.Resources["LargeItemMarginValue"] * 4;
            double gridWidth = ImageGridView.ActualWidth -
                (int)this.Resources["DesktopWindowSidePaddingValue"];
    
            if (AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Mobile" &&
                UIViewSettings.GetForCurrentView().UserInteractionMode ==
                    UserInteractionMode.Touch)
            {
                margins = (int)this.Resources["SmallItemMarginValue"] * 4;
                gridWidth = ImageGridView.ActualWidth -
                    (int)this.Resources["MobileWindowSidePaddingValue"];
            }
    
            double ItemWidth = ZoomSlider.Value + margins;
            // We need at least 1 column.
            int columns = (int)Math.Max(gridWidth / ItemWidth, 1);
    
            // Adjust the available grid width to account for margins around each item.
            double adjustedGridWidth = gridWidth - (columns * margins);
    
            ItemSize = (adjustedGridWidth / columns);
        }
        else
        {
            ItemSize = ZoomSlider.Value;
        }
    }
    ```

2. En MainPage.xaml, navega hasta la parte superior del archivo y agrega un evento **SizeChanged** que enlace con el elemento **Página**.

    **Antes:**
    ```xaml
    <Page x:Name="page"  
    ```

    **Después:**
    ```xaml
    <Page x:Name="page" 
          SizeChanged="{x:Bind DetermineItemSize}"
    ```

3. Busca el **Control deslizante** denominado **ZoomSlider** y agrega un enlace a evento **ValueChanged**.

    **Antes:**
    ```xaml
    <Slider x:Name="ZoomSlider"
    ```

    **Después:**
    ```xaml
    <Slider x:Name="ZoomSlider"
            ValueChanged="{x:Bind DetermineItemSize}"
    ```

4. Busca el **ToggleSwitch** denominado **FitScreenToggle** y agrega un enlace a evento **Toggled**.

    **Antes:**
    ```xaml
    <ToggleSwitch x:Name="FitScreenToggle"
    ```

    **Después:**
    ```xaml
    <ToggleSwitch x:Name="FitScreenToggle"
                  Toggled="{x:Bind DetermineItemSize}"
    ```

Ejecuta la aplicación y usa el control deslizante de zoom y la alternancia **Ajustar a la pantalla** para cambiar las dimensiones de la plantilla de imagen. Como puedes ver, los cambios más recientes permiten una experiencia de cambio de tamaño o zoom más refinada mientras que se mantiene el código bien organizado. 

![Aplicación en ejecución con ajuste a la pantalla habilitado](../design/basics/images/xaml-basics/gallery-with-fit-to-screen.png)

> [!NOTE]
> Como desafío, intenta agregar un **TextBlock** después de **ZoomSlider** y enlazar la propiedad **Texto** a la propiedad **ItemSize**. Dado que no está en una plantilla de datos, puedes usar **x:Bind** en lugar de **Enlace** al igual que en los anteriores enlaces **ItemSize**.  
}

## <a name="part-5-enable-user-edits"></a>Parte 5: Habilitar ediciones del usuario

Aquí, crearás enlaces bidireccionales para permitir que los usuarios actualicen valores, como el título de la imagen, la clasificación y varios efectos visuales. 

Para lograrlo, actualizarás la **DetailPage** existente, que proporciona un visor de imagen única, control de zoom e interfaz de usuario de edición.  

En primer lugar, sin embargo, deberás adjuntar la **DetailPage** para que la aplicación vaya a ella cuando el usuario haga clic en una imagen de la vista de galería.

**Adjuntar la DetailPage**

1. En MainPage.xaml, busca la **GridView** denominada **ImageGridView** y agrega un valor **ItemClick**. 

    > [!TIP] 
    > Si escribes el cambio que viene a continuación, en lugar de copiar y pegar, verás una ventana emergente de IntelliSense que diga "\<Controlador de nuevo evento\>". Si presionas la tecla tabulador, se rellenará el valor con un nombre de controlador de método predeterminado y automáticamente se cerrará el método mostrado en el siguiente paso. A continuación, puedes presionar F12 para navegar al método en el código subyacente. 

    **Antes:**
    ```xaml
    <GridView x:Name="ImageGridView"
    ```

    **Después:**
    ```xaml
    <GridView x:Name="ImageGridView"
              ItemClick="ImageGridView_ItemClick"
    ```

    > [!NOTE] 
    > Estamos usando aquí un controlador de eventos convencional, en lugar de una expresión x:Bind. Esto es porque debemos ver los datos del evento, como se muestra a continuación. 

2. En MainPage.xaml.cs, agrega el controlador de eventos (o rellénalo, si has usado la sugerencia del último paso).

    ```c#
    private void ImageGridView_ItemClick(object sender, ItemClickEventArgs e)
    {
        this.Frame.Navigate(typeof(DetailPage), e.ClickedItem);
    }
    ```

    Este método simplemente navega a la página de detalles, pasando el elemento donde se hizo clic, que es un objeto **ImageFileInfo** utilizado por **DetailPage.OnNavigatedTo** para inicializar la página. No tendrás que implementar ese método en este tutorial, pero puedes echar un vistazo para ver lo que hace. 
    
3. (Opcional) Elimina o comenta los controles que agregaste en los puntos de juego anteriores que trabajan con la imagen seleccionada actualmente. Mantenerlos cerca no puede dañar nada, pero ahora es mucho más difícil seleccionar una imagen sin tener que navegar a la página de detalles. 

Ahora que has conectado las dos páginas, ejecuta la aplicación y echa un vistazo. Todo funciona excepto los controles de panel de edición, que no responden al intentar cambiar los valores. 

Como puedes ver, el cuadro de texto de título muestra el título y te permite escribir en los cambios. Tienes que cambiar el foco a otro control para confirmar los cambios, pero todavía no se actualiza el título de la esquina superior izquierda de la pantalla. 

Todos los controles ya están enlazados con las expresiones **x:Bind** sencillas que se trataron en la parte 1. Si recuerdas, esto significa que todos son enlaces de una vez, lo que explica por qué no se registran los cambios en los valores. Para corregir esto, todo lo que tenemos que hacer es convertirlos en enlaces bidireccionales. 

**Hacer que los controles de edición sean interactivos**

1. En DetailPage.xaml, busca el **TextBlock** denominado **TitleTextBlock** y el control **RadRating** después, y actualiza sus expresiones **x:Bind** para incluir **Mode=TwoWay**.

    **Antes:**
    ```xaml
    <TextBlock x:Name="TitleTextBlock"
               Text="{x:Bind item.ImageTitle}"
               ... >
    <telerikInput:RadRating Value="{x:Bind item.ImageRating}"
                            ... >
    ```

    **Después:**
    ```xaml
    <TextBlock x:Name="TitleTextBlock" 
               Text="{x:Bind item.ImageTitle, Mode=TwoWay}" 
               ... >
    <telerikInput:RadRating Value="{x:Bind item.ImageRating, Mode=TwoWay}" 
                            ... >
    ```

2. Haz lo mismo para todos los controles deslizantes de efectos que vienen detrás del control de clasificación.

    ```xaml
    <Slider Header="Exposure"    ... Value="{x:Bind item.Exposure, Mode=TwoWay}" ...
    <Slider Header="Temperature" ... Value="{x:Bind item.Temperature, Mode=TwoWay}" ... 
    <Slider Header="Tint"        ... Value="{x:Bind item.Tint, Mode=TwoWay}" ... 
    <Slider Header="Contrast"    ... Value="{x:Bind item.Contrast, Mode=TwoWay}" ... 
    <Slider Header="Saturation"  ... Value="{x:Bind item.Saturation, Mode=TwoWay}" ...
    <Slider Header="Blur"        ... Value="{x:Bind item.Blur, Mode=TwoWay}" ... 
    ```

El modo bidireccional, como cabría esperar, significa que los datos se mueven en ambas direcciones siempre que haya cambios en cualquier lado. 

Como los enlaces unidireccionales tratados anteriormente, estos enlaces bidireccionales ahora actualizan la interfaz de usuario siempre que cambien las propiedades enlazadas, gracias a la implementación **INotifyPropertyChanged** en la clase **ImageFileInfo**. Con los enlaces bidireccionales, sin embargo, los valores también se moverán de la interfaz de usuario a las propiedades enlazadas siempre que el usuario interactúe con el control. No se necesita nada más en el lado XAML. 

Ejecuta la aplicación y prueba los controles de edición. Como puedes ver, cuando realizas un cambio, ahora afecta a los valores de imagen y los cambios persisten al volver a la página principal. 

## <a name="part-6-format-values-through-function-binding"></a>Parte 6: valores de formato mediante el enlace de función

Queda un último problema. Cuando mueves los controles deslizantes de efecto, siguen sin cambiar las etiquetas junto a ellos. 

![Controles deslizantes de efecto con valores predeterminados de etiquetas](../design/basics/images/xaml-basics/effect-sliders-before-label-fix.png)

La parte final de este tutorial es para agregar enlaces que dan formato a los valores de control deslizante a mostrar.

**Enlazar las etiquetas de control deslizante de efecto y dar formato a los valores de pantalla**

1. Busca la **TextBlock** después del control deslizante **Exposición** y sustituye el valor **Texto** por la expresión de enlace que se muestra aquí.

    **Antes:**
    ```xaml
    <Slider Header="Exposure" ... />
    <TextBlock ... Text="0.00" />
    ```

    **Después:**
    ```xaml
    <Slider Header="Exposure" ... />
    <TextBlock ... Text="{x:Bind item.Exposure.ToString('N', culture), Mode=OneWay}" />
    ```

    Esto se denomina una función de enlace porque se enlaza con el valor devuelto de un método. El método debe ser accesible a través del código subyacente de la página o el tipo **x:DataType** si estás en una plantilla de datos. En este caso, el método es el método familiar .NET **ToString**, al que se accede mediante la propiedad de elemento de la página y, a continuación, a través de la propiedad **Exposición** del elemento. (Esto muestra cómo se puede enlazar a los métodos y propiedades que están profundamente anidados en una cadena de conexiones).

    El enlace de función es una manera ideal de dar formato a valores para pantalla, porque se puede pasar como argumentos de método otros orígenes de enlace, y la expresión de enlace escuchará los cambios a esos valores como se esperaba con el modo unidireccional. En este ejemplo, el argumento **cultura** es una referencia a un campo fijo que se implementa en código subyacente, pero podría haber sido igualmente una propiedad que generara eventos **PropertyChanged**. En ese caso, cualquier cambio en el valor de propiedad provocaría que la expresión **x:Bind** llamara a **ToString** con el nuevo valor y luego actualizara la interfaz de usuario con el resultado. 

2. Haz lo mismo para la **TextBlocks** que etiqueta los otros controles deslizantes de efecto.

    ```xaml
    <Slider Header="Temperature" ... />
    <TextBlock ... Text="{x:Bind item.Temperature.ToString('N', culture), Mode=OneWay}" />

    <Slider Header="Tint" ... />
    <TextBlock ... Text="{x:Bind item.Tint.ToString('N', culture), Mode=OneWay}" />

    <Slider Header="Contrast" ... />
    <TextBlock ... Text="{x:Bind item.Contrast.ToString('N', culture), Mode=OneWay}" />

    <Slider Header="Saturation" ... />
    <TextBlock ... Text="{x:Bind item.Saturation.ToString('N', culture), Mode=OneWay}" />

    <Slider Header="Blur" ... />
    <TextBlock ... Text="{x:Bind item.Blur.ToString('N', culture), Mode=OneWay}" />
    ```

Ahora, cuando ejecutas la aplicación, todo funciona, incluidas las etiquetas de los controles deslizantes. 

![Controles deslizantes de efecto con etiquetas de trabajo](../design/basics/images/xaml-basics/effect-sliders-after-label-fix.png)

> [!NOTE]
> Prueba a usar el enlace de función con el **TextBlock** desde el último punto de juego y enlazar a un nuevo método que devuelva una cadena en formato "000 x 000" al pasar el valor **ItemSize**.


## <a name="conclusion"></a>Conclusión

Este tutorial te habrá dado una idea del enlace de datos y te habrá permitido ver algunas de las funciones disponibles. Una palabra de advertencia antes de resumir: no todo el contenido se puede enlazar y, a veces los valores con que intentas conectar no son compatibles con las propiedades que intentas enlazar. Hay una gran cantidad de flexibilidad en el enlace, pero no funcionará en todas las situaciones.

Un ejemplo de un problema que no contempla el enlace es cuando un control no tiene propiedades adecuadas a donde enlazar, como la función de zoom de página de detalles. Este control deslizante de zoom debe interactuar con el **ScrollViewer** que muestra la imagen, pero **ScrollViewer** solo puede actualizarse a través de su método **ChangeView**. En este caso, usamos controladores de eventos convencionales para mantener el **ScrollViewer** y el control deslizante de zoom en sincronización; consulta los métodos **DetailPage** **ZoomSlider_ValueChanged** y **MainImageScroll_ViewChanged** para obtener más información.

No obstante, el enlace es una forma eficaz y flexible de simplificar el código y separar la lógica de interfaz de usuario de la lógica de datos. Esto hará que te resulte mucho más fácil ajustar cualquier lado de esta división a la vez que se reduce el riesgo de introducir errores en el otro lado. 

Un ejemplo de separación de la interfaz de usuario y los datos es la propiedad **ImageFileInfo.ImageTitle**. Esta propiedad (y la propiedad **ImageRating**) es ligeramente diferente a la propiedad **ItemSize** que creaste en la parte 4, porque el valor se almacena en los metadatos de archivo (expuestos a través del tipo **ImageProperties**) en lugar de en un campo. Además, **ImageTitle** devuelve el valor **ImageName** (establecido en el nombre de archivo) si no hay ningún título en los metadatos del archivo. 

```c#
public string ImageTitle
{
    get => String.IsNullOrEmpty(ImageProperties.Title) ? ImageName : ImageProperties.Title;
    set
    {
        if (ImageProperties.Title != value)
        {
            ImageProperties.Title = value;
            var ignoreResult = ImageProperties.SavePropertiesAsync();
            OnPropertyChanged();
        }
    }
}
```

Como puedes ver, el establecedor actualiza la propiedad **ImageProperties.Title** y, a continuación, llama a **SavePropertiesAsync** para escribir el nuevo valor en el archivo. (Esto es un método asíncrono, pero no podemos utilizar la palabra clave **await** en una propiedad; y eso no te gustaría, debido a que los captadores y establecedores finalizarían inmediatamente. Por lo tanto, en su lugar, llamarías al método e ignorarías el objeto **Tarea** que devuelve). 

<!-- TODO more screenshots? -->

## <a name="going-further"></a>Ir más allá

Ahora que has completado este laboratorio, tienes suficiente conocimiento sobre enlaces como para afrontar problemas por tu cuenta.

Como habrás notado, si cambias el nivel de zoom en la página de detalles, se restablece automáticamente cuando navegas hacia atrás y, a continuación, seleccionas de nuevo la misma imagen. ¿Puedes averiguar cómo conservar y restaurar el nivel de zoom para cada imagen individualmente? ¡Buena suerte!
    
Tienes toda la información que necesitas en este tutorial pero, si necesitas más información, los documentos de enlace de datos están a solo un clic de distancia. Comienza por aquí:

+ [Extensión de marcado {x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)
+ [Enlace de datos en profundidad](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)