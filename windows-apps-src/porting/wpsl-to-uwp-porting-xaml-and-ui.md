---
description: La práctica para definir la interfaz de usuario en forma de marcado XAML declarativo se traslada muy bien tanto para Windows Phone Silverlight como para aplicaciones para la Plataforma universal de Windows (UWP).
title: Migración de XAML y la interfaz de usuario de Windows Phone Silverlight a UWP
ms.assetid: 49aade74-5dc6-46a5-89ef-316dbeabbebe
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9a6f78d30b1366078f2094aa17ab15c65a050c43
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164349"
---
#  <a name="porting-windowsphone-silverlight-xaml-and-ui-to-uwp"></a>Migración de XAML y la interfaz de usuario de Windows Phone Silverlight a UWP



El tema anterior era [Solución de problemas](wpsl-to-uwp-troubleshooting.md).

La práctica para definir la interfaz de usuario en forma de marcado XAML declarativo se traslada muy bien tanto para Windows Phone Silverlight como para aplicaciones para la Plataforma universal de Windows (UWP). Encontrarás que grandes secciones del marcado son compatibles, una vez hayas actualizado las referencias de clave de recurso del sistema, cambiado algunos nombres de tipos de elementos y cambiado "clr-namespace" por "using". Parte del código imperativo de la capa de presentación (modelos de vistas y código que manipula elementos de interfaz de usuario) también será sencillo de migrar.

## <a name="a-first-look-at-the-xaml-markup"></a>Una primera mirada al marcado XAML

En el tema anterior se mostraba cómo copiar los archivos XAML y de código subyacente en el nuevo proyecto de Visual Studio de Windows 10. Uno de los primeros problemas que se resaltará en el diseñador de XAML de Visual Studio es que el elemento `PhoneApplicationPage` en la raíz del archivo XAML no es válido para un proyecto de la Plataforma universal de Windows (UWP). En el tema anterior, guardaste una copia de los archivos XAML que Visual Studio había generado al crear el proyecto de Windows 10. Si abres esta versión de MainPage.xaml, verás que en la raíz está el tipo [**Page**](/uwp/api/Windows.UI.Xaml.Controls.Page), que se encuentra en el espacio de nombres [**Windows.UI.Xaml.Controls**](/uwp/api/Windows.UI.Xaml.Controls). Por lo tanto, es posible cambiar todos los elementos `<phone:PhoneApplicationPage>` por `<Page>` (no te olvides de la sintaxis del elemento de propiedad) y eliminar la declaración `xmlns:phone` .

Para obtener un enfoque más general para buscar el tipo de UWP que corresponde a un tipo de Windows Phone Silverlight, puede hacer referencia a las [asignaciones de espacio de nombres y de clase](wpsl-to-uwp-namespace-and-class-mappings.md).

## <a name="xaml-namespace-prefix-declarations"></a>Declaraciones de prefijo de espacio de nombres XAML


Si usas instancias de tipos personalizados en las vistas (quizás una instancia de modelo de vista o un convertidor de valores), tendrás las declaraciones de prefijo de espacio de nombres XAML en el marcado XAML. La sintaxis de estas es diferente entre Windows Phone Silverlight y UWP. Estos son algunos ejemplos:

```xml
    xmlns:ContosoTradingCore="clr-namespace:ContosoTradingCore;assembly=ContosoTradingCore"
    xmlns:ContosoTradingLocal="clr-namespace:ContosoTradingLocal"
```

Cambia "clr-namespace" por "using" y elimina los tokens de ensamblado y los puntos y coma (el ensamblado se inferirá). El resultado tiene el aspecto siguiente:

```xml
    xmlns:ContosoTradingCore="using:ContosoTradingCore"
    xmlns:ContosoTradingLocal="using:ContosoTradingLocal"
```

Puede que tengas un recurso cuyo tipo esté definido por el sistema:

```xml
    xmlns:System="clr-namespace:System;assembly=mscorlib"
    /* ... */
    <System:Double x:Key="FontSizeLarge">40</System:Double>
```

En UWP, omite la declaración de prefijo "System" y usa el prefijo "x" (ya declarado) en su lugar:

```xml
    <x:Double x:Key="FontSizeLarge">40</x:Double>
```

## <a name="imperative-code"></a>Código imperativo


Los modelos de vista son un lugar donde hay código imperativo que hace referencia a tipos de interfaz de usuario. Otro lugar es cualquier archivo de código subyacente que manipula directamente elementos de interfaz de usuario. Por ejemplo, puede ser que una línea de código como este no se compile aún:


```csharp
    return new BitmapImage(new Uri(this.CoverImagePath, UriKind.Relative));
```

**BitmapImage** se encuentra en el espacio de nombres **System.Windows.Media.Imaging** en Windows Phone Silverlight, y una directiva "using" en el mismo archivo permite usar **BitmapImage** sin la calificación de espacio de nombres como en el fragmento de código anterior. En casos como este, puedes hacer clic con el botón secundario en el nombre de tipo (**BitmapImage**) en Visual Studio y usar el comando **Resolver** en el menú contextual para agregar una nueva directiva de espacio de nombres al archivo. En este caso, se agrega el espacio de nombres [**Windows.UI.Xaml.Media.Imaging**](/uwp/api/Windows.UI.Xaml.Media.Imaging), que es donde reside el tipo en Windows en UWP. Puedes quitar la directiva using **System.Windows.Media.Imaging** y será todo lo necesario para migrar código como en el fragmento de código anterior. Cuando termines, habrás quitado todos los espacios de nombres de Windows Phone Silverlight.

En casos sencillos como este, donde se asignan los tipos de un espacio de nombres anterior a los mismos tipos de uno nuevo, puedes usar el comando **Buscar y reemplazar** de Visual Studio para realizar cambios masivos en el código fuente. El comando **Resolver** es una excelente manera de detectar un nuevo espacio de nombres de un tipo. Otro ejemplo podría ser reemplazar todas las apariciones de "System.Windows" por "Windows.UI.Xaml". Esto migrará básicamente todas las directivas using y todos los nombres de tipo completos que hacen referencia a ese espacio de nombres.

Una vez eliminadas todas las directivas using anteriores y después de agregar las nuevas, puedes usar el comando **Organizar instrucciones Using** de Visual Studio para ordenar las directivas y quitar las que no se usan.

En ocasiones, la corrección del código imperativo será tan mínima como cambiar el tipo de un parámetro. En otras ocasiones, tendrá que usar Windows Runtime API en lugar de las API de .NET para las aplicaciones de Windows Runtime 8. x. Para identificar las API admitidas, use el resto de esta guía de migración en combinación con [.net para obtener información general sobre las aplicaciones de Windows Runtime 8. x](/previous-versions/windows/apps/br230302(v=vs.140)) y la [referencia de Windows Runtime](/uwp/api/).

Asimismo, si solo quieres llegar a la etapa donde se crea el proyecto, puedes establecer como comentario o código auxiliar cualquier código que no sea esencial. Después itera, un problema cada vez, y consulta los siguientes temas de esta sección (y el tema anterior: [Solución de problemas](wpsl-to-uwp-troubleshooting.md)), hasta que se zanjen todos los problemas de compilación y de tiempo de ejecución y la migración se complete.

## <a name="adaptiveresponsive-ui"></a>Interfaz de usuario adaptativa y dinámica

Dado que la aplicación de Windows 10 se puede usar en una amplia gama de dispositivos (cada uno con su propio tamaño de pantalla y resolución) es aconsejable ir más allá de los pasos mínimos para migrar la aplicación y adaptar la interfaz de usuario para que tenga el mejor aspecto en esos dispositivos. Puedes usar la función adaptativa Visual State Manager para detectar dinámicamente el tamaño de ventana y cambiar el diseño en respuesta. Se muestra un ejemplo de cómo hacerlo en la sección [Interfaz de usuario adaptativa](wpsl-to-uwp-case-study-bookstore2.md) en el tema de caso práctico Bookstore2.

## <a name="alarms-and-reminders"></a>Alarmas y recordatorios

El código que usa las clases **Alarm** o **Reminder** debe migrarse para que use la clase [**BackgroundTaskBuilder**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) y así crear y registrar una tarea en segundo plano, además de mostrar una notificación del sistema en el momento pertinente. Consulte [procesamiento en segundo plano](wpsl-to-uwp-business-and-data.md) y [notificaciones del sistema](#toasts).

## <a name="animation"></a>Animación

Como alternativa preferida a las animaciones de fotogramas clave y las animaciones de origen y destino, la biblioteca de animaciones de UWP está disponible ahora para aplicaciones para UWP. Estas animaciones se han diseñado y ajustado para que se ejecuten sin problemas, tengan un aspecto genial y para que la aplicación parezca integrada en Windows como lo hacen las aplicaciones integradas. Consulta [Inicio rápido: Animación de la interfaz de usuario con animaciones de la biblioteca](/previous-versions/windows/apps/hh452703(v=win.10)).

Si usas animaciones de fotogramas clave o animaciones de origen y destino en las aplicaciones para UWP, es conveniente comprender la distinción entre animaciones dependientes e independientes que ha introducido la nueva plataforma. Consulta [Optimizar las animaciones y multimedia](../debug-test-perf/optimize-animations-and-media.md). Las animaciones que se ejecutan en el subproceso de la interfaz de usuario (las que animan las propiedades de diseño, por ejemplo) se conocen como animaciones dependientes. Cuando se ejecutan en la nueva plataforma no tienen ningún efecto a menos que se realice una de estas dos acciones. Puedes redireccionarlas para que animen propiedades diferentes, como [**RenderTransform**](/uwp/api/windows.ui.xaml.uielement.rendertransform), haciendo de este modo que sean independientes. O puedes establecer `EnableDependentAnimation="True"` en el elemento de animación para confirmar tu intención de ejecutar una animación sin garantías de que la acción se lleve a cabo sin problemas. Si usas Blend para Visual Studio para crear nuevas animaciones, esta propiedad se establecerá automáticamente en caso necesario.

## <a name="back-button-handling"></a>Control del botón Atrás

En una aplicación de Windows 10, puedes usar un único enfoque para controlar el botón Atrás y este funcionará en todos los dispositivos. En los dispositivos móviles, el botón se proporciona automáticamente como un botón capacitivo en el dispositivo o como un botón en el shell. En un dispositivo de escritorio, se agrega un botón al cromo de la aplicación siempre que la navegación hacia atrás es posible dentro de la aplicación; esto aparece en la barra de título para aplicaciones en ventana o en la barra de tareas para el modo de tableta. El evento de botón Atrás es un concepto universal en todas las familias de dispositivos y los botones implementados en el hardware o el software originan el mismo evento [**BackRequested**](/uwp/api/windows.ui.core.systemnavigationmanager.backrequested).

El siguiente ejemplo funciona para todas las familias de dispositivos y resulta útil para los casos donde el mismo procesamiento se aplica a todas las páginas y cuando no es necesario confirmar la navegación (por ejemplo, para advertir sobre cambios sin guardar).

```csharp
   // app.xaml.cs

    protected override void OnLaunched(LaunchActivatedEventArgs e)
    {
        [...]

        Windows.UI.Core.SystemNavigationManager.GetForCurrentView().BackRequested += App_BackRequested;
        rootFrame.Navigated += RootFrame_Navigated;
    }

    private void RootFrame_Navigated(object sender, NavigationEventArgs e)
    {
        Frame rootFrame = Window.Current.Content as Frame;

        // Note: On device families that have no title bar, setting AppViewBackButtonVisibility can safely execute 
        // but it will have no effect. Such device families provide a back button UI for you.
        if (rootFrame.CanGoBack)
        {
            Windows.UI.Core.SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility = 
                Windows.UI.Core.AppViewBackButtonVisibility.Visible;
        }
        else
        {
            Windows.UI.Core.SystemNavigationManager.GetForCurrentView().AppViewBackButtonVisibility = 
                Windows.UI.Core.AppViewBackButtonVisibility.Collapsed;
        }
    }

    private void App_BackRequested(object sender, Windows.UI.Core.BackRequestedEventArgs e)
    {
        Frame rootFrame = Window.Current.Content as Frame;

        if (rootFrame.CanGoBack)
        {
            rootFrame.GoBack();
        }
    }
```

También es un método único para todas las familias de dispositivos para salir de la aplicación mediante programación.

```csharp
   Windows.UI.Xaml.Application.Current.Exit();
```

## <a name="binding-and-compiled-bindings-with-xbind"></a>Enlace y enlaces compilados con {x:Bind}

El tema sobre los enlaces incluye:

-   El enlace de un elemento de interfaz de usuario a "data" (es decir, a las propiedades y los comandos de un modelo de vista)
-   El enlace de un elemento de interfaz de usuario a otro elemento de interfaz de usuario
-   Escribir un modelo de vista que sea observable (es decir, que genera notificaciones cuando cambia el valor de una propiedad y cuando cambia la disponibilidad de un comando)

Todos estos aspectos siguen siendo prácticamente compatibles, pero existen diferencias de espacio de nombres. Por ejemplo, **System.Windows.Data.Binding** se asigna a [**Windows.UI.Xaml.Data.Binding**](/uwp/api/Windows.UI.Xaml.Data.Binding), **System.ComponentModel.INotifyPropertyChanged** se asigna a [**Windows.UI.Xaml.Data.INotifyPropertyChanged**](/uwp/api/Windows.UI.Xaml.Data.INotifyPropertyChanged) y **System.Collections.Specialized.INotifyPropertyChanged** se asigna a [**Windows.UI.Xaml.Interop.INotifyCollectionChanged**](/uwp/api/Windows.UI.Xaml.Interop.INotifyCollectionChanged).

Las barras de la aplicación y los botones de las barras de la aplicación de Windows Phone Silverlight no se pueden enlazar igual que en una aplicación para UWP. Puede que tengas código imperativo que cree la barra de la aplicación y sus botones, los enlace a propiedades y cadenas localizadas, y controle sus eventos. Si es así, ahora tienes la opción de migrar este código imperativo reemplazándolo por marcado declarativo enlazado a propiedades y comandos, así como por referencias a recursos estáticos, lo que hace que la aplicación sea progresivamente más segura y más fácil de mantener. Puedes usar Visual Studio o Blend para Visual Studio para enlazar y aplicar estilo a botones de la barra de la aplicación para UWP igual que para cualquier otro elemento XAML. Tenga en cuenta que en una aplicación para UWP los nombres de tipo que se usan son [**CommandBar**](/uwp/api/Windows.UI.Xaml.Controls.CommandBar) y [**AppBarButton**](/uwp/api/Windows.UI.Xaml.Controls.AppBarButton).

Las funciones relacionadas con el enlace de las aplicaciones para UWP tienen actualmente las siguientes limitaciones:

-   No hay compatibilidad integrada para la validación de entrada de datos y las interfaces [**IDataErrorInfo**](/dotnet/api/system.componentmodel.idataerrorinfo) e [**INotifyDataErrorInfo**](/dotnet/api/system.componentmodel.inotifydataerrorinfo).
-   La clase [**Binding**](/uwp/api/Windows.UI.Xaml.Data.Binding) no incluye las propiedades de formato extendido disponibles en Windows Phone Silverlight. Sin embargo, aún puedes implementar [**IValueConverter**](/uwp/api/Windows.UI.Xaml.Data.IValueConverter) para proporcionar formato personalizado.
-   Los métodos [**IValueConverter**](/uwp/api/Windows.UI.Xaml.Data.IValueConverter) toman cadenas de lenguaje como parámetros en lugar de objetos [**CultureInfo**](/dotnet/api/system.globalization.cultureinfo).
-   La clase [**CollectionViewSource**](/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource) no proporciona compatibilidad integrada para ordenar y filtrar, y el agrupamiento funciona de manera diferente. Para obtener más información, consulte [enlace de datos en profundidad](../data-binding/data-binding-in-depth.md) y el [ejemplo de enlace de datos](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlBind).

Aunque todavía se admiten en gran medida las mismas funciones de enlace, Windows 10 ofrece la opción de un nuevo mecanismo de enlace con mejor rendimiento denominado enlaces compilados, que usa la extensión de marcado {x:Bind}. Consulte [enlace de datos: aumente el rendimiento de las aplicaciones mediante nuevas mejoras en el enlace de datos XAML](https://channel9.msdn.com/Events/Build/2015/3-635)y el [ejemplo x:Bind](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlBind).

## <a name="binding-an-image-to-a-view-model"></a>Enlazar una imagen a un modelo de vista

Puedes enlazar la propiedad [**Image.Source**](/uwp/api/windows.ui.xaml.controls.image.source) a cualquier propiedad de un modelo de vista que sea de tipo [**ImageSource**](/uwp/api/Windows.UI.Xaml.Media.ImageSource). Esta es una implementación típica de este tipo de propiedad en una aplicación Windows Phone Silverlight:

```csharp
    // this.BookCoverImagePath contains a path of the form "/Assets/CoverImages/one.png".
    return new BitmapImage(new Uri(this.CoverImagePath, UriKind.Relative));
```

En una aplicación para UWP, se usa el [esquema de URI](/previous-versions/windows/apps/jj655406(v=win.10)) ms-appx. Para que puedas mantener el resto del código sin modificar, puedes usar una sobrecarga distinta del constructor **System.Uri** para poner el esquema de URI ms-appx en un URI base y anexarle el resto de la ruta de acceso. Así:

```csharp
    // this.BookCoverImagePath contains a path of the form "/Assets/CoverImages/one.png".
    return new BitmapImage(new Uri(new Uri("ms-appx://"), this.CoverImagePath));
```

De este modo, el resto del modelo de vista, los valores de ruta de acceso de la propiedad de ruta de acceso de la imagen y los enlaces en el marcado XAML, pueden permanecen exactamente igual.

## <a name="controls-and-control-stylestemplates"></a>Controles y estilos y plantillas de control

Las aplicaciones Windows Phone Silverlight usan controles definidos en los espacios de nombres **Microsoft.Phone.Controls** y **System.Windows.Controls**. Las aplicaciones para UWP XAML usan controles definidos en el espacio de nombres [**Windows.UI.Xaml.Controls**](/uwp/api/Windows.UI.Xaml.Controls). La arquitectura y el diseño de los controles XAML en UWP son prácticamente iguales que en los controles de Windows Phone Silverlight. No obstante, se han realizado algunos cambios para mejorar el conjunto de controles disponibles y para unificarlos con las aplicaciones de Windows. A continuación se muestran ejemplos específicos.

| Nombre del control | Cambio |
|--------------|--------|
| ApplicationBar | La propiedad [Page.TopAppBar](/uwp/api/windows.ui.xaml.controls.page.topappbar). |
| ApplicationBarIconButton | El equivalente de UWP es la propiedad [Glyph](/uwp/api/windows.ui.xaml.controls.fonticon.glyph). PrimaryCommands es la propiedad de contenido de CommandBar. El analizador XAML interpreta el xml interno de un elemento como el valor de su propiedad de contenido. |
| ApplicationBarMenuItem | El equivalente de UWP es la [AppBarButton.Label](/uwp/api/windows.ui.xaml.controls.appbarbutton.label) establecida en el texto del elemento de menú. |
| ContextMenu (en el kit de herramientas de Windows Phone) | En el caso de una sola selección, use el [control flotante](/uwp/api/Windows.UI.Xaml.Controls.Flyout). |
| Clase ControlTiltEffect.TiltEffect | Las animaciones de la biblioteca de animaciones de UWP están integradas en los estilos predeterminados de los controles comunes. Vea las [acciones de puntero de animación](/previous-versions/windows/apps/jj649432(v=win.10)). |
| LongListSelector con datos agrupados | LongListSelector de Windows Phone Silverlight funciona de dos maneras, que se pueden usar conjuntamente. En primer lugar, es capaz de mostrar datos agrupados por una clave como, por ejemplo, una lista de nombres agrupados por su letra inicial. En segundo lugar, es capaz de aplicar "zoom" entre dos vistas semánticas: la lista agrupada de elementos (por ejemplo, nombres) y una lista de únicamente las claves de grupo (por ejemplo, letras iniciales). Con UWP, puede mostrar datos agrupados con las [directrices para los controles de vista de lista y cuadrícula](../design/controls-and-patterns/lists.md). |
| LongListSelector con datos planos | Por motivos de rendimiento, en el caso de listas muy largas, te recomendamos que uses LongListSelector en lugar de un cuadro de lista de Windows Phone Silverlight, incluso para datos planos no agrupados. En una aplicación para UWP, se prefiere [GridView](/uwp/api/Windows.UI.Xaml.Controls.GridView) para listas de elementos largas independientemente de si los datos son susceptibles de agruparse. |
| Panorámica | El control panorama de Silverlight de Windows Phone se asigna a las [directrices para los controles de concentrador en las aplicaciones Windows Runtime 8. x](../design/basics/navigation-basics.md) y las directrices para el control de central. <br/> Ten en cuenta que un control Panorama se ajusta automáticamente desde la última sección a la primera y su imagen de fondo se mueve en parallax en relación con las secciones. Las secciones [Hub](/uwp/api/Windows.UI.Xaml.Controls.Hub) no se ajustan automáticamente y no se usa parallax. |
| Dinamización | El equivalente de UWP del control dinámico de Windows Phone Silverlight es [Windows.UI.Xaml.Controls.Pivot](/uwp/api/Windows.UI.Xaml.Controls.Pivot). Está disponible para todas las familias de dispositivos. |

**Nota:**    El estado visual PointerOver es relevante en estilos y plantillas personalizados en aplicaciones de Windows 10, pero no en Windows Phone aplicaciones de Silverlight. Existen otros motivos por los que los estilos o plantillas personalizados existentes pueden no ser adecuados para aplicaciones de Windows 10, incluidas las claves de recursos del sistema que usas, los cambios en los conjuntos de estados visuales usados y las mejoras de rendimiento realizadas en los estilos o plantillas predeterminadas de Windows 10. Te recomendamos que editar una nueva copia de una plantilla de control predeterminada para Windows 10 y, a continuación, volverle a aplicar la personalización de plantillas y estilos.

Para obtener información sobre los controles de UWP, consulta [Controles por función](../design/controls-and-patterns/controls-by-function.md), [Lista de controles](../design/controls-and-patterns/index.md) y [Directrices sobre controles](../design/controls-and-patterns/index.md).

##  <a name="design-language-in-windows10"></a>Lenguaje de diseño en Windows 10

Existen algunas diferencias en el lenguaje de diseño entre las aplicaciones de Windows Phone Silverlight y las aplicaciones de Windows 10. Para obtener detalles, consulta [Diseño](https://developer.microsoft.com/windows/apps/design). A pesar de los cambios del lenguaje de diseño, nuestros principios de diseño siguen siendo coherentes: prestar atención a los detalles, pero siempre lograr la simplicidad centrándonos en el contenido y no en el embellecimiento, reducir drásticamente los elementos visuales y mantenernos auténticos al dominio digital; usar jerarquía visual especialmente con tipografía; diseño en cuadrícula; y hacer que tus experiencias cobren vida con animaciones fluidas.

## <a name="localization-and-globalization"></a>Localización y globalización

Para las cadenas localizadas, puedes volver a usar el archivo .resx del proyecto de Windows Phone Silverlight en el proyecto de aplicación para UWP. Copia el archivo, agrégalo al proyecto y cambia su nombre por Resources.resw para que el mecanismo de búsqueda lo encuentre de manera predeterminada. Establece **Acción de compilación** en **PRIResource** y **Copiar en el directorio de salida** en **No copiar**. A continuación, puedes usar las cadenas en el marcado especificando el atributo **x:Uid** en los elementos XAML. Vea [Inicio rápido: uso de recursos de cadena](/previous-versions/windows/apps/hh965329(v=win.10)).

Las aplicaciones Windows Phone Silverlight usan la clase **CultureInfo** para ayudar a globalizar una aplicación. Las aplicaciones para UWP usan MRT (Modern Resource Technology), que permite la carga dinámica de recursos de aplicación (localización, escala y tema) en tiempo de ejecución y en la superficie de diseño de Visual Studio. Para obtener más información, vea [directrices para archivos, datos y globalización](../design/usability/index.md).

El tema [**ResourceContext.QualifierValues**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.qualifiervalues) describe cómo cargar recursos específicos de la familia de dispositivos según el factor de selección de recursos de la familia de dispositivos.

## <a name="media-and-graphics"></a>Multimedia y gráficos

Al leer acerca de multimedia y gráficos de UWP, ten en cuenta que los principios de diseño de Windows animan a una reducción feroz de todo lo superfluo, incluidos el desorden y la complejidad gráfica. El diseño de Windows está representado por el movimiento, la tipografía y los elementos visuales limpios y claros. Si la aplicación sigue los mismos principios, se parecerá más a las aplicaciones integradas.

Windows Phone Silverlight tiene un tipo **RadialGradientBrush** que no está presente en UWP, aunque otros tipos [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) sí lo estén. En algunos casos, podrás obtener un efecto similar con un mapa de bits. Ten en cuenta que puedes [crear un pincel de degradado radial](/windows/desktop/Direct2D/how-to-create-a-radial-gradient-brush) con Direct2D en una aplicación para UWP C++, XAML y [Microsoft DirectX](/windows/desktop/directx) .

Windows Phone Silverlight tiene la propiedad **System.Windows.UIElement.OpacityMask** pero esta propiedad no es un miembro del tipo [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) de UWP. En algunos casos, podrás obtener un efecto similar con un mapa de bits. Asimismo, puedes [crear una máscara de opacidad](/windows/desktop/Direct2D/opacity-masks-overview) con Direct2D en una aplicación para UWP C++, XAML y [Microsoft DirectX](/windows/desktop/directx). No obstante, un caso de uso habitual para **OpacityMask** es usar un único mapa de bits que se adapte a temas claros y oscuros. Para los gráficos vectoriales, puedes usar pinceles de reconocimiento de tema del sistema (por ejemplo, los gráficos circulares que se muestran a continuación). No obstante, para crear un mapa de bits de reconocimiento de tema (por ejemplo, las marcas de verificación que se muestran a continuación) se requiere un enfoque diferente.

![un mapa de bits de reconocimiento de tema](images/wpsl-to-uwp-case-studies/wpsl-to-uwp-theme-aware-bitmap.png)

En una aplicación Windows Phone Silverlight, la técnica es usar una máscara alfa (en forma de un mapa de bits) como **OpacityMask** para un relleno **Rectángulo** con el pincel de primer plano:

```xml
    <Rectangle Fill="{StaticResource PhoneForegroundBrush}" Width="26" Height="26">
        <Rectangle.OpacityMask>
            <ImageBrush ImageSource="/Assets/wpsl_check.png"/>
        </Rectangle.OpacityMask>
    </Rectangle>
```

La manera más sencilla de migrar esto a una aplicación para UWP es usar un [**BitmapIcon**](/uwp/api/Windows.UI.Xaml.Controls.BitmapIcon), como este:

```xml
    <BitmapIcon UriSource="Assets/winrt_check.png" Width="21" Height="21"/>
```

Aquí, winrt \_check.png es una máscara alfa en forma de mapa de bits como wpsl \_check.png es y podría ser el mismo archivo. Sin embargo, puede que desee proporcionar varios tamaños diferentes de winrt \_check.png que se usarán para diferentes factores de escala. Para obtener más información sobre esto y para obtener una explicación de los cambios en los valores de **ancho** y **alto** , consulte [píxeles de vista o efectivos, visualización de la distancia y factores de escala](#view-or-effective-pixels-viewing-distance-and-scale-factors) de este tema.

Un enfoque más general, que es adecuado si existen diferencias entre la forma de tema claro y oscuro de un mapa de bits, es usar dos recursos de imagen: uno con un primer plano oscuro (para el tema claro) y otro con un primer plano claro (para el tema oscuro). Para obtener más información sobre cómo asignar un nombre a este conjunto de recursos de mapa de bits, vea [adaptar los recursos para el idioma, la escala y otros calificadores](../app-resources/tailor-resources-lang-scale-contrast.md). Una vez que se ha asignado correctamente un nombre a los conjuntos de archivos de imagen, puedes hacer referencia a ellos en el resumen, usando su nombre raíz, como este:

```xml
    <Image Source="Assets/winrt_check.png" Stretch="None"/>
```

En Windows Phone Silverlight, la propiedad **UIElement.Clip** puede tener cualquier forma que puedas expresar con una **Geometry** y normalmente se serializa en el marcado XAML en el minilenguaje de **StreamGeometry**. En UWP, el tipo de la propiedad [**Clip**](/uwp/api/windows.ui.xaml.uielement.clip) es [**RectangleGeometry**](/uwp/api/Windows.UI.Xaml.Media.RectangleGeometry), de modo que solo se puede recortar una región rectangular. Permitir la definición de un rectángulo usando minilenguaje sería demasiado permisivo. Por lo tanto, para migrar una región de recorte en el marcado, reemplaza la sintaxis del atributo **Clip** y conviértela en sintaxis de elemento de propiedad similar al siguiente:

```xml
    <UIElement.Clip>
        <RectangleGeometry Rect="10 10 50 50"/>
    </UIElement.Clip>
```

Ten en cuenta que puedes [usar geometría arbitraria como una máscara en una capa](/windows/desktop/Direct2D/direct2d-layers-overview) con Direct2D en una aplicación para UWP C++, XAML y [Microsoft DirectX](/windows/desktop/directx).

## <a name="navigation"></a>Navegación

Cuando se navega a una página en una aplicación Windows Phone Silverlight, se usa un esquema de direccionamiento de identificador uniforme de recursos (URI):

```csharp
    NavigationService.Navigate(new Uri("/AnotherPage.xaml", UriKind.Relative)/*, navigationState*/);
```

En una aplicación para UWP, se llama al método [**Frame.Navigate**](/uwp/api/windows.ui.xaml.controls.frame.navigate) y se especifica el tipo de la página de destino (tal como define el atributo **x:Class** de la definición de marcado XAML de la página):


```csharp
    // In a page:
    this.Frame.Navigate(typeof(AnotherPage)/*, parameter*/);

    // In a view model, perhaps inside an ICommand implementation:
    var rootFrame = Windows.UI.Xaml.Window.Current.Content as Windows.UI.Xaml.Controls.Frame;
    rootFrame.Navigate(typeof(AnotherPage)/*, parameter*/);
```

La página de inicio de una aplicación Windows Phone Silverlight se define en WMAppManifest.xmll:

```xml
    <DefaultTask Name="_default" NavigationPage="MainPage.xaml" />
```

En una aplicación para UWP, se usa código imperativo para definir la página de inicio. Este es parte del código de App.xaml.cs que ilustra cómo hacerlo:

```csharp
    if (!rootFrame.Navigate(typeof(MainPage), e.Arguments))
```

La navegación por fragmentos y asignación de URI son técnicas de navegación de URI y, por lo tanto, no son aplicables a la navegación de UWP, que no se basa en los URI. La asignación de URI existe en respuesta a la naturaleza poco tipificada de la identificación de una página de destino con una cadena URI, lo que conduce a problemas de fragilidad y mantenimiento si la página se mueve a otra carpeta y, por tanto, a una ruta de acceso relativa diferente. Las aplicaciones para UWP usan la navegación basada en tipos, que está muy tipificada y se comprueba por el compilador, y no presenta los problemas que resuelve la asignación de URI. El caso de uso para la navegación por fragmentos es pasar algún contexto a la página de destino para que esta pueda hacer que un fragmento en particular de su contenido se desplace en la vista o se muestre de otra forma. El mismo objetivo puede conseguirse pasando un parámetro de navegación cuando se llama al método [**Navigate**](/uwp/api/windows.ui.xaml.controls.frame.navigate).

Para obtener más información, consulte [navegación](../design/basics/navigation-basics.md).

## <a name="resource-key-reference"></a>Referencia a claves de recursos

El lenguaje de diseño ha evolucionado para Windows 10 y, en consecuencia, han cambiado ciertos estilos de sistema y muchas claves de recursos del sistema se han quitado o cambiado de nombre. El editor de marcado XAML en Visual Studio resalta las referencias a las claves de recursos que no puede resolver. Por ejemplo, el editor de marcado XAML subrayará una referencia a la clave de estilo `PhoneTextNormalStyle` con una línea ondulada roja. Si no se corrige, la aplicación finalizará inmediatamente cuando intentes implementarla en el emulador o el dispositivo. Por tanto, es importante prestar atención a la corrección del marcado XAML. Encontrarás que Visual Studio es una herramienta excelente para detectar esos problemas.

Consulta también [Texto](#text) más adelante.

## <a name="status-bar-system-tray"></a>Barra de estado (bandeja del sistema)

La bandeja del sistema (establecida en el marcado XAML con `shell:SystemTray.IsVisible`) se llama ahora "barra de estado" y aparece de manera predeterminada. Puedes controlar su visibilidad en código imperativo llamando a los métodos [**Windows.UI.ViewManagement.StatusBar.ShowAsync**](/uwp/api/windows.ui.viewmanagement.statusbar.showasync) y [**HideAsync**](/uwp/api/windows.ui.viewmanagement.statusbar.hideasync).

## <a name="text"></a>Texto

El texto (o tipografía) es un aspecto importante de una aplicación para UWP y, durante la migración, es aconsejable que vuelvas a visitar los diseños de elementos visuales de las vistas para que estén en consonancia con el nuevo lenguaje de diseño. Usa estas ilustraciones para encontrar los estilos de sistema **TextBlock** de UWP que están disponibles. Encuentra los que corresponden a los estilos de Windows Phone Silverlight usados. Como alternativa, puedes crear tus propios estilos universales y copiar las propiedades de los estilos del sistema de Windows Phone Silverlight en ellos.

![system textblock styles fo windows 10 apps](images/label-uwp10stylegallery.png)

Estilos de TextBlock del sistema para aplicaciones de Windows 10

En una aplicación de Windows Phone Silverlight, la familia de fuentes predeterminada es Segoe WP. En una aplicación de Windows 10, la familia de fuentes predeterminada es Segoe UI. Como resultado, las métricas de fuente en tu aplicación pueden parecer diferentes. Si quieres reproducir el aspecto de tu texto de Windows Phone Silverlight, puedes establecer tus propias métricas mediante propiedades como [**LineHeight**](/uwp/api/windows.ui.xaml.controls.textblock.lineheight) y [**LineStackingStrategy**](/uwp/api/windows.ui.xaml.controls.textblock.linestackingstrategy). Para obtener más información, consulte [directrices para fuentes](https://docs.microsoft.com/windows/uwp/controls-and-patterns/fonts) y [diseño de aplicaciones para UWP](https://developer.microsoft.com/windows/apps/design).

## <a name="theme-changes"></a>Cambios de tema

Para una aplicación de Windows Phone Silverlight, el tema predeterminado es oscuro siempre. Para los dispositivos Windows 10, ha cambiado el tema predeterminado, pero se puede controlar el tema usado declarando un tema solicitado en App.xaml. Por ejemplo, para usar un tema oscuro en todos los dispositivos, agrega `RequestedTheme="Dark"` al elemento Application de raíz.

## <a name="tiles"></a>Iconos

A pesar de algunas diferencias, los iconos para las aplicaciones para UWP tienen comportamientos similares a los iconos dinámicos de las aplicaciones Windows Phone Silverlight. Por ejemplo, el código que llama al método **Microsoft.Phone.Shell.ShellTile.Create** para crear iconos secundarios debe migrarse para llamar a [**SecondaryTile.RequestCreateAsync**](/uwp/api/windows.ui.startscreen.secondarytile.requestcreateasync). Este es un ejemplo de antes y después, primero la versión de Windows Phone Silverlight:


```csharp
    var tileData = new IconicTileData()
    {
        Title = this.selectedBookSku.Title,
        WideContent1 = this.selectedBookSku.Title,
        WideContent2 = this.selectedBookSku.Author,
        SmallIconImage = this.SmallIconImageAsUri,
        IconImage = this.IconImageAsUri
    };

    ShellTile.Create(this.selectedBookSku.NavigationUri, tileData, true);
```

Y el equivalente de UWP:

```csharp
    var tile = new SecondaryTile(
        this.selectedBookSku.Title.Replace(" ", string.Empty),
        this.selectedBookSku.Title,
        this.selectedBookSku.ArgumentString,
        this.IconImageAsUri,
        TileSize.Square150x150);

    await tile.RequestCreateAsync();
```

El código que actualiza un icono con el método **Microsoft.Phone.Shell.ShellTile.Update** o la clase **Microsoft.Phone.Shell.ShellTileSchedule** debe migrarse para usar las clases [**TileUpdateManager**](/uwp/api/Windows.UI.Notifications.TileUpdateManager), [**TileUpdater**](/uwp/api/Windows.UI.Notifications.TileUpdater), [**TileNotification**](/uwp/api/Windows.UI.Notifications.TileNotification) y [**ScheduledTileNotification**](/uwp/api/Windows.UI.Notifications.ScheduledTileNotification).

Para obtener más información sobre iconos, notificaciones del sistema, distintivos, mensajes emergentes y notificaciones, consulta [Creación de iconos](/previous-versions/windows/apps/hh868260(v=win.10)) y [Trabajar con iconos, distintivos y notificaciones del sistema](/previous-versions/windows/apps/hh868259(v=win.10)). Para obtener información específica acerca de los tamaños de los recursos visuales que se usan para los iconos de UWP, consulte [activos visuales del sistema y de mosaicos](/previous-versions/windows/apps/hh781198(v=win.10)).

## <a name="toasts"></a>Notificaciones del sistema

El código que muestra una notificación del sistema con la clase **Microsoft.Phone.Shell.ShellToast**, debe migrarse para usar las clases [**ToastNotificationManager**](/uwp/api/Windows.UI.Notifications.ToastNotificationManager), [**ToastNotifier**](/uwp/api/Windows.UI.Notifications.ToastNotifier), [**ToastNotification**](/uwp/api/Windows.UI.Notifications.ToastNotification) y [**ScheduledToastNotification**](/uwp/api/Windows.UI.Notifications.ScheduledToastNotification). Ten en cuenta que en dispositivos móviles, el término que se muestra al consumidor para "notificación del sistema" es "mensaje emergente".

Vea [trabajar con iconos, distintivos y notificaciones del sistema](/previous-versions/windows/apps/hh868259(v=win.10)).

## <a name="view-or-effective-pixels-viewing-distance-and-scale-factors"></a>Ver píxeles efectivos, la distancia de visualización y los factores de escala

Las aplicaciones de Windows Phone Silverlight y las aplicaciones de Windows 10 difieren en la forma en que abstraen el tamaño y el diseño de los elementos de interfaz de usuario fuera el tamaño físico real y la resolución de dispositivos. Una aplicación Windows Phone Silverlight usa píxeles de visualización para hacerlo. Con Windows 10, el concepto de píxeles de visualización se ha perfeccionado en el de píxeles efectivos. Esta es una explicación de este término, lo que significa y el valor adicional que ofrece.

El término "resolución" se refiere a una medida de densidad de píxeles y no, como se suele considerar, al número de píxeles. La "resolución eficaz" es la forma en que los píxeles físicos que componen una imagen o un glifo se resuelve a la vista, dadas las diferencias en la distancia de visualización y el tamaño de los píxeles físicos del dispositivo (la densidad de píxeles es recíproca del tamaño de píxeles físicos). La resolución eficiente es una buena métrica para crear una experiencia porque se centra en el usuario. Mediante la comprensión de todos los factores y el control del tamaño de los elementos de la interfaz de usuario, puedes hacer que la experiencia del usuario sea positiva.

Para una aplicación Windows Phone Silverlight, todas las pantallas de teléfono tienen exactamente un ancho de 480 píxeles de visualización, sin ninguna excepción, independientemente del número de píxeles físicos que tenga la pantalla, de su densidad de píxeles o de su tamaño físico. Esto significa que un elemento **Image** con `Width="48"` tendrá exactamente una décima parte del ancho de la pantalla de cualquier teléfono que puede ejecutar la aplicación Windows Phone Silverlight.

Para una aplicación de Windows 10, *no* todos los dispositivos tienen un ancho con un número fijo de píxeles efectivos. Esto es probablemente obvio, dada la gran variedad de dispositivos en los que se puede ejecutar una aplicación para UWP. Los dispositivos distintos tienen un número diferente de píxeles efectivos de ancho, que van desde los 320 epx en los dispositivos más pequeños hasta los 1024 epx en un monitor de tamaño reducido, y mucho más allá hasta anchos muy superiores. Lo único que tienes que hacer es continuar usando elementos de tamaño automático y paneles de diseño dinámico como siempre. También habrá algunos casos en loa que establecerás las propiedades de tus elementos de interfaz de usuario en un tamaño fijo en el marcado XAML. Un factor de escala se aplica automáticamente a la aplicación en función del dispositivo en que se ejecuta y de la configuración de visualización que tenga el usuario. Y ese factor de escala sirve para mantener cualquier elemento de interfaz de usuario con un tamaño fijo con un destino táctil (y de lectura) de tamaño más o menos constante para el usuario, a través de una amplia variedad de tamaños de pantalla. Además, junto con el diseño dinámico, la interfaz de usuario no solo se escalará visualmente en dispositivos distintos, sino que hará lo que sea necesario para ajustar la cantidad adecuada de contenido en el espacio disponible.

Como anteriormente 480 era el ancho fijo en píxeles de visualización para una pantalla de tamaño de teléfono y ese valor es ahora normalmente inferior en píxeles efectivos, una regla general es multiplicar cualquier dimensión en el marcado de la aplicación Windows Phone Silverlight por un factor de 0,8.

Para que la aplicación ofrezca la mejor experiencia en todas las pantallas, te recomendamos que crees cada recurso de mapa de bits en una variedad de tamaños, cada uno adecuado para un factor de escala particular. Ofrecer recursos con una escala del 100 %, 200 % y 400 % (en este orden de prioridad), te dará excelentes resultados en la mayoría de los casos en todos los factores de escala intermedios.

**Nota:**    Si, por cualquier motivo, no puede crear recursos en más de un tamaño, cree activos a escala de 100%. En Microsoft Visual Studio, la plantilla de proyecto predeterminada para aplicaciones para UWP proporciona recursos de personalización de marca (imágenes de icono y logotipos) en un solo tamaño, pero no tiene la escala del 100 %. Al crear recursos para tu propia aplicación, sigue las instrucciones de esta sección y ofrece tamaños del 100 %, 200 % y 400 %, además de usar paquetes de recursos.

Si tienes ilustraciones intrincadas, puede que quieras ofrecer tus recursos en más tamaños aún. Si estás empezando con el arte vectorial, es relativamente fácil generar recursos de alta calidad con cualquier factor de escala.

No te recomendamos que intentes admitir todos los factores de escala, pero la lista completa de factores de escala para las aplicaciones de Windows 10 es 100 %, 125 %, 150 %, 200 %, 250 %, 300 % y 400 %. Si los proporcionas, la Tienda elegirá los recursos de tamaño correcto para cada dispositivo y solo se descargarán estos recursos. La Tienda selecciona los recursos para descargar en función de los valores de PPP del dispositivo.

Para obtener más información, consulte [diseño con capacidad de respuesta 101 para aplicaciones UWP](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md).

## <a name="window-size"></a>Tamaño de la ventana

En la aplicación para UWP, puedes especificar un tamaño mínimo (ancho y alto) con código imperativo. El tamaño mínimo predeterminado es 500 x 320epx, que también es el tamaño mínimo más pequeño aceptado. El tamaño mínimo mayor aceptado es 500x500 epx.

```csharp
   Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetPreferredMinSize
        (new Size { Width = 500, Height = 500 });
```

El siguiente tema es la [migración del modelo de e/s, el dispositivo y la aplicación](wpsl-to-uwp-input-and-sensors.md).

## <a name="related-topics"></a>Temas relacionados

* [Asignaciones de espacios de nombres y clases](wpsl-to-uwp-namespace-and-class-mappings.md)