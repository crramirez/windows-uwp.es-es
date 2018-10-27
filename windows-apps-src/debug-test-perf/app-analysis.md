---
author: jwmsft
title: Análisis de la aplicación
description: Analizar la aplicación para problemas de rendimiento
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 346e6790c6578bf861ba1dda937eae6d4d50f00f
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2018
ms.locfileid: "5703129"
---
# <a name="app-analysis-overview"></a>Información general sobre el análisis de la aplicación

El análisis de la aplicación es una herramienta que proporciona a los desarrolladores una notificación anticipada de los problemas de rendimiento. El análisis de la aplicación ejecuta el código de la aplicación respecto a un conjunto de directrices y procedimientos recomendados de rendimiento.

El análisis de la aplicación identifica los problemas a partir de un conjunto de reglas de problemas comunes de rendimiento que se encuentran las aplicaciones. Cuando corresponda, el análisis de la aplicación apuntará a la herramienta de línea de tiempo de Visual Studio, a la información de la fuente y a la documentación para proporcionarte los medios para investigar.

Las reglas del análisis de la aplicación se refieren a una directriz o a un procedimiento recomendado frente a los que se comprueba la aplicación.

## <a name="decoded-image-size-larger-than-render-size"></a>Tamaño de la imagen descodificada mayor que el tamaño de representación

Las imágenes se capturan en resoluciones muy altas, lo que puede provocar un mayor uso de la CPU por parte de las aplicaciones cuando descodifican los datos de imagen y de más memoria tras su descarga del disco. Pero no tiene sentido descodificar y guardar una imagen de alta resolución en la memoria si se va a mostrar únicamente en un tamaño menor que su tamaño nativo. En su lugar, crea una versión de la imagen que tenga el tamaño exacto que se mostrará en la pantalla mediante las propiedades DecodePixelWidth y DecodePixelHeight.

### <a name="impact"></a>Impacto

Mostrar imágenes con su tamaño no nativo puede tener un impacto negativo en el tiempo de la CPU (debido a la descodificación a su tamaño adecuado y al tiempo de la descarga) y la memoria.

### <a name="causes-and-solutions"></a>Causas y soluciones

#### <a name="image-is-not-being-set-asynchronously"></a>La imagen no se establece asincrónicamente

La aplicación está usando SetSource() en lugar de SetSourceAsync(). Evita usar [**SetSource**](https://msdn.microsoft.com/library/windows/apps/BR243255) y, en su lugar, emplea [**SetSourceAsync**](https://msdn.microsoft.com/library/windows/apps/JJ191522) al establecer una secuencia para decodificar imágenes de forma asíncrona. 

#### <a name="image-is-being-called-when-the-imagesource-is-not-in-the-live-tree"></a>Se llama a la imagen cuando la propiedad ImageSource no está en el árbol activo

La clase BitmapImage se conecta al árbol XAML activo después de configurar el contenido con SetSourceAsync o con UriSource. Te recomendamos que asocies siempre una clase [**BitmapImage**](https://msdn.microsoft.com/library/windows/apps/BR243235) al árbol activo antes de establecer el origen. Esto sucederá siempre que especifiques un elemento o pincel de imagen en el marcado. A continuación se proporcionan ejemplos. 

**Ejemplos de árbol activo**

Ejemplo 1 (bueno): Se especifica el identificador uniforme de recursos (URI) en el marcado.

```xml
<Image x:Name="myImage" UriSource="Assets/cool-image.png"/>
```

Ejemplo 2; marcado: se especifica el URI en el código subyacente.

```xml
<Image x:Name="myImage"/>
```

Ejemplo 2; código subyacente (bueno): conectar el elemento BitmapImage al árbol antes de establecer su UriSource.

```vb
var bitmapImage = new BitmapImage();
myImage.Source = bitmapImage;
bitmapImage.UriSource = new URI("ms-appx:///Assets/cool-image.png", UriKind.RelativeOrAbsolute);
```

Ejemplo 2; código subyacente (malo): establecer UriSource de BitmapImage antes de conectarlo al árbol.

```vb
var bitmapImage = new BitmapImage();
bitmapImage.UriSource = new URI("ms-appx:///Assets/cool-image.png", UriKind.RelativeOrAbsolute);
myImage.Source = bitmapImage;
```

#### <a name="image-brush-is-non-rectangular"></a>El pincel de imagen no es rectangular 

Cuando se usa una imagen para un pincel no rectangular, la imagen usará una ruta de acceso de rasterización de software, que en ningún momento escalará las imágenes. Además, debe almacenar una copia de la imagen tanto en la memoria de software como en la de hardware. Por ejemplo, si se usa una imagen como el pincel de una elipse, la totalidad de la imagen potencialmente grande se almacenará internamente dos veces. Al usar un pincel no rectangular, la aplicación debería escalar previamente sus imágenes a aproximadamente el tamaño en el que se presentarán.

Como alternativa, puedes establecer un tamaño de descodificación explícito para crear una versión de la imagen con el tamaño exacto que se mostrará en la pantalla mediante las propiedades [**DecodePixelWidth**](https://msdn.microsoft.com/library/windows/apps/BR243243) y [**DecodePixelHeight**](https://msdn.microsoft.com/library/windows/apps/BR243241).

```xml
<Image>
    <Image.Source>
    <BitmapImage UriSource="ms-appx:///Assets/highresCar.jpg" 
                 DecodePixelWidth="300" DecodePixelHeight="200"/>
    </Image.Source>
</Image>
```

De manera predeterminada, las unidades [**DecodePixelWidth**](https://msdn.microsoft.com/library/windows/apps/BR243243) y [**DecodePixelHeight**](https://msdn.microsoft.com/library/windows/apps/BR243241) son píxeles físicos. Puedes usar la propiedad [**DecodePixelType**](https://msdn.microsoft.com/library/windows/apps/Dn298545) para cambiar este comportamiento: al establecer **DecodePixelType** en **Logical** provocarás que se descodifique automáticamente el tamaño, teniendo en cuenta el factor de escala actual del sistema, el cual es similar a otro contenido XAML. Por lo tanto, sería conveniente establecer de forma general **DecodePixelType** en **Logical** si, por ejemplo, quieres que **DecodePixelWidth** y **DecodePixelHeight** coincidan con las propiedades Height y Width del control Image en el cual se mostrará la imagen. Con el comportamiento predeterminado de uso de píxeles físicos, debes tener en cuenta el factor de escala actual del sistema y debes prestar atención a las notificaciones de cambios de escala en caso de que el usuario cambie sus preferencias de visualización.

En algunos casos donde no se puede determinar con anterioridad un tamaño de descodificación adecuado, deberías dejarlo en manos de la descodificación automática del tamaño correcto de XAML, que hará un mejor intento de descodificación de la imagen con el tamaño adecuado si no se especifican las propiedades DecodePixelWidth/DecodePixelHeight explícitas.

Te recomendamos que establezcas un tamaño de descodificación explícito si conoces con anterioridad el tamaño del contenido de la imagen. Además, también deberías establecer [**DecodePixelType**](https://msdn.microsoft.com/library/windows/apps/Dn298545) en **Logical** si el tamaño de descodificación proporcionado está relacionado con otros tamaños de los elementos XAML. Por ejemplo, si estableces explícitamente el tamaño del contenido con Image.Width e Image.Height, puedes establecer DecodePixelType en DecodePixelType.Logical para que use las mismas dimensiones de píxeles lógicos como un control Image y luego usar explícitamente BitmapImage.DecodePixelWidth o BitmapImage.DecodePixelHeight para controlar el tamaño de la imagen con el fin de lograr ahorros de memoria potencialmente grande.

Recuerda que hay que tener en cuenta Image.Stretch al determinar el tamaño del contenido descodificado.

#### <a name="images-used-inside-of-bitmapicons-fall-back-to-decoding-to-natural-size"></a>Las imágenes que se usan dentro de BitmapIcons vuelven a la descodificación de tamaño natural 

Establece un tamaño de descodificación explícito para crear una versión de la imagen con el tamaño exacto que se mostrará en la pantalla mediante las propiedades [**DecodePixelWidth**](https://msdn.microsoft.com/library/windows/apps/BR243243) y [**DecodePixelHeight**](https://msdn.microsoft.com/library/windows/apps/BR243241).

#### <a name="images-that-appear-extremely-large-on-screen-fall-back-to-decoding-to-natural-size"></a>Las imágenes que se muestran extremadamente grandes en pantalla vuelven a la descodificación a tamaño natural 

Las imágenes que se muestran extremadamente grandes en pantalla vuelven a la descodificación a tamaño natural. Establece un tamaño de descodificación explícito para crear una versión de la imagen con el tamaño exacto que se mostrará en la pantalla mediante las propiedades [**DecodePixelWidth**](https://msdn.microsoft.com/library/windows/apps/BR243243) y [**DecodePixelHeight**](https://msdn.microsoft.com/library/windows/apps/BR243241).

#### <a name="image-is-hidden"></a>La imagen está oculta

Para ocultar la imagen, se establece la propiedad Opacity en 0 o Visibility en Collapsed en el elemento de imagen host, en el pincel o en cualquier elemento primario. Las imágenes que no son visibles en la pantalla debido a un recorte o a la transparencia pueden volver a la descodificación a tamaño natural. 

#### <a name="image-is-using-ninegrid-property"></a>La imagen usa la propiedad NineGrid

Cuando se usa una imagen para una propiedad [**NineGrid**](https://msdn.microsoft.com/library/windows/apps/BR242756), dicha imagen usará una ruta de acceso de rasterización de software, que en ningún momento escalará las imágenes. Además, debe almacenar una copia de la imagen tanto en la memoria de software como en la de hardware. Si se usa **NineGrid**, la aplicación deberá escalar previamente sus imágenes a aproximadamente el tamaño en el que se presentarán.

Las imágenes que usan la propiedad NineGrid volverán a la descodificación a tamaño natural. Considera la posibilidad de agregar el efecto de ninegrid a la imagen original.

#### <a name="decodepixelwidth-or-decodepixelheight-are-set-to-a-size-thats-larger-than-the-image-will-appear-on-screen"></a>Las propiedades DecodePixelWidth o DecodePixelHeight se establecen en un tamaño mayor que aquel con el que la imagen aparecerá en pantalla 

Si estableces DecodePixelWidth/Height para que sean explícitamente mayores que la imagen que se mostrará en pantalla, la aplicación usará innecesariamente más memoria (hasta 4 bytes por píxel), lo cual supondrá tener que usar más recursos para imágenes de gran tamaño. Igualmente, la imagen se reducirá usando el escalado bilineal, lo que podría provocar que se vea borrosa en factores de escala grandes.

#### <a name="image-is-decoded-as-part-of-producing-a-drag-and-drop-image"></a>La imagen se descodifica como parte de la producción de una imagen de arrastrar y colocar

Establece un tamaño de descodificación explícito para crear una versión de la imagen con el tamaño exacto que se mostrará en la pantalla mediante las propiedades [**DecodePixelWidth**](https://msdn.microsoft.com/library/windows/apps/BR243243) y [**DecodePixelHeight**](https://msdn.microsoft.com/library/windows/apps/BR243241).

## <a name="collapsed-elements-at-load-time"></a>Elementos contraídos en el tiempo de carga

Es un patrón común en las aplicaciones es ocultar inicialmente los elementos de la interfaz de usuario y mostrarlos en un momento posterior. En la mayoría de los casos, estos elementos deben aplazarse con x:Load o x:DeferLoadStrategy para evitar tener que pagar el costo de crear el elemento en el tiempo de carga.

Esto incluye los casos donde se usa un valor booleano para el convertidor de visibilidad con el fin de ocultar los elementos hasta un momento posterior.

### <a name="impact"></a>Impacto

Los elementos contraídos se cargan junto con otros elementos y contribuyen a un aumento del tiempo de carga.

### <a name="cause"></a>Causa

Esta regla se ha desencadenado porque un elemento se ha contraído en el tiempo de carga. La contracción de un elemento o el establecimiento de su opacidad en 0 no impide su creación. Esta regla podría deberse a que una aplicación use un valor booleano para el convertidor de visibilidad cuyo valor predeterminado sea "false".

### <a name="solution"></a>Solución

Con [x:Load attribute](../xaml-platform/x-load-attribute.md) o [x:DeferLoadStrategy](https://msdn.microsoft.com/library/windows/apps/Mt204785), puedes retrasar la carga de un fragmento de la interfaz de usuario y cargarlo cuando se necesite. Esto es una buena manera de retrasar el procesamiento de la interfaz de usuario que no es visible en el primer fotograma. Puedes optar por cargar el elemento cuando sea necesario o como parte de un conjunto de lógica retrasada. Para activar la carga, llama a findName en el elemento que quieras cargar. x:Load amplía las capacidades de x:DeferLoadStrategy al permitir la descarga de elementos y que el estado de carga se pueda controlar a través de x:Bind.

En algunos casos, el uso de findName para mostrar un elemento de la interfaz de usuario puede que no sea la respuesta. Esto sucede si esperas que una parte importante de la interfaz de usuario se muestre al hacer clic en un botón con una latencia muy baja. En este caso, puedes compensar la latencia de interfaz de usuario más rápida a costa de memoria adicional. De ser así, deberías usar x:DeferLoadStrategy y establecer los objetos Visibility en Collapsed en el elemento que quieras obtener. Una vez que se haya cargado la página y el subproceso de la interfaz de usuario esté libre, puedes llamar a findName cuando sea necesario para cargar los elementos. Los elementos no será visibles para el usuario hasta que establezcas la propiedad Visibility del elemento en Visible.

## <a name="listview-is-not-virtualized"></a>ListView no se virtualiza

La virtualización de la interfaz de usuario es la mejora más importante que puedes realizar para mejorar el rendimiento de las colecciones. Esto significa que los elementos de la interfaz de usuario que representan los elementos se crean a petición. Para un control de elementos enlazado a una colección de 1000 elementos, sería un desperdicio de recursos crear la interfaz de usuario de todos los elementos al mismo tiempo, porque todos no pueden mostrarse a la vez. ListView y GridView (y otros controles estándar derivados de ItemsControl) realizan la virtualización de la interfaz de usuario por ti. Cuando los elementos están a punto de desplazarse hacia la vista (a una páginas de distancia), el marco de trabajo genera la interfaz de usuario de los elementos y los almacena. Asimismo, cuando sea improbable que los elementos se muestren de nuevo, el marco de trabajo recuperará la memoria.

La virtualización de la interfaz de usuario es solo uno de varios factores clave para mejorar el rendimiento de las colecciones. Reducir la complejidad de los elementos de colección y la virtualización de datos son otros dos aspectos importantes para mejorar el rendimiento de las colecciones. Para obtener más información acerca de cómo mejorar el rendimiento de las colecciones en ListViews y GridViews, consulta los artículos [Optimización de la interfaz de usuario de ListView y GridView](https://msdn.microsoft.com/windows/uwp/debug-test-perf/optimize-gridview-and-listview) y [Virtualización de datos de ListView y GridView](https://msdn.microsoft.com/windows/uwp/debug-test-perf/listview-and-gridview-data-optimization).

### <a name="impact"></a>Impacto

Un ItemsControl no virtualizado aumentará el tiempo de carga y el uso de recursos al cargar más elementos secundarios de los necesarios.

### <a name="cause"></a>Causa

El concepto de una ventanilla es fundamental para la virtualización de la interfaz de usuario, porque el marco debe crear los elementos que es probable que se muestren. En general, la ventanilla de una clase ItemsControl es del tamaño del control lógico. Por ejemplo, la ventanilla de un control ListView es el ancho y el alto del elemento ListView. Algunos paneles permiten que los elementos secundarios tengan un espacio ilimitado, como ScrollViewer y Grid, los cuales cuentan con columnas o filas de tamaño automático. Cuando un ItemsControl virtualizado se coloca en un panel como ese, necesita bastante espacio para mostrar todos sus elementos, lo que inhabilita la virtualización. 

### <a name="solution"></a>Solución

Restaura la virtualización estableciendo un ancho y un alto en el ItemsControl que uses.

## <a name="ui-thread-blocked-or-idle-during-load"></a>Subproceso de la interfaz de usuario bloqueado o inactivo durante la carga

El bloqueo del subproceso de la interfaz de usuario se refiere a las llamadas sincrónicas a funciones que se ejecutan fuera del subproceso y que bloquean el subproceso de la interfaz de usuario.  

Para obtener una lista completa de los procedimientos recomendados para mejorar el rendimiento del inicio de la aplicación, consulta [Procedimientos recomendados para mejorar el rendimiento del inicio de la aplicación](https://msdn.microsoft.com/windows/uwp/debug-test-perf/best-practices-for-your-app-s-startup-performance) y [Mantener la capacidad de respuesta del subproceso de la interfaz de usuario](https://msdn.microsoft.com/windows/uwp/debug-test-perf/keep-the-ui-thread-responsive).

### <a name="impact"></a>Impacto

Un subproceso de la interfaz de usuario bloqueado o inactivo durante el tiempo de carga impedirá el diseño y otras operaciones de la interfaz de usuario, lo que aumentará el tiempo de inicio.

### <a name="cause"></a>Causa

El código de la plataforma para la interfaz de usuario y el código de la aplicación para la interfaz de usuario se ejecutan en el mismo subproceso de la interfaz de usuario. Solo puede ejecutarse una instrucción en dicho subproceso en cada momento, por lo que si el código de la aplicación tarda demasiado en procesar un evento, el marco no podrá ejecutar el diseño ni generar nuevos eventos que representen la interacción del usuario. La capacidad de respuesta de tu aplicación depende de la disponibilidad del subproceso de la interfaz de usuario para procesar trabajo.

### <a name="solution"></a>Solución

La aplicación puede ser interactiva aunque algunas de sus partes no sean totalmente funcionales. Por ejemplo, si la aplicación muestra datos que tardan un poco recuperarse, puedes hacer que ese código se ejecute de forma independiente del código de inicio de la aplicación. Para hacerlo, recupera los datos de forma asincrónica. Cuando los datos estén disponibles, úsalos para rellenar la interfaz de usuario de la aplicación. Para ayudar a mantener la capacidad de respuesta de la aplicación, la plataforma proporciona versiones asincrónicas de muchas de sus API. Una API asincrónica asegura que el subproceso de ejecución activa nunca se bloquee durante un período de tiempo largo. Cuando llames a una API desde el subproceso de la interfaz de usuario, usa la versión asincrónica si está disponible.

## <a name="binding-is-being-used-instead-of-xbind"></a>Se usa {Binding} en lugar de {x: Bind}

Esta regla se activa cuando la aplicación usa una instrucción {Binding}. Para mejorar el rendimiento de la aplicación, las aplicaciones deberían plantearse usar {x: Bind}.

### <a name="impact"></a>Impacto

La ejecución de {Binding} cuesta más tiempo y más memoria que la de {x: Bind}.

### <a name="cause"></a>Causa

La aplicación usa {Binding} en lugar de {x: Bind}. {Binding} aporta un conjunto de trabajo no trivial y la sobrecarga de la CPU. La creación de un elemento {Binding} provoca una serie de asignaciones, y la actualización de un destino de enlace puede provocar reflejos y conversiones boxing.

### <a name="solution"></a>Solución

Usa la extensión de marcado {x: Bind}, que compila los enlaces en el tiempo de compilación. Los enlaces {x:Bind} (a menudo denominados "enlaces compilados") tienen un rendimiento óptimo, proporcionan la validación en tiempo de compilación de las expresiones de enlace y admiten depuración, ya que permiten establecer puntos de interrupción en los archivos de código que se generan como la clase parcial de la página. 

Ten en cuenta que x: Bind no es adecuado en todos los casos, como en escenarios enlazados en tiempo de ejecución. Para obtener una lista completa de los casos que {x: Bind} no abarca, consulta la documentación de {x: Bind}.

## <a name="xname-is-being-used-instead-of-xkey"></a>Se usa x:Name en lugar de x:Key.

Generalmente, el elemento ResourceDictionaries se usa para almacenar los recursos en un nivel más o menos global, es decir, los recursos a los que la aplicación quiere hacer referencia en varios lugares; por ejemplo, estilos, pinceles, plantillas, etc. En general, hemos optimizado el elemento ResourceDictionaries para que no cree instancias de los recursos a menos que se pidan. Sin embargo, hay varios lugares en los que debes tener algo de cuidado.

### <a name="impact"></a>Impacto

Se creará una instancia de cualquier recurso con x: Name tan pronto como se cree el elemento ResourceDictionary. Esto sucede porque x:Name indica a la plataforma que la aplicación necesita acceso de campo a este recurso, por lo que debe crear algo a que hacer referencia.

### <a name="cause"></a>Causa

La aplicación establece x: Name en un recurso.

### <a name="solution"></a>Solución

Usa x: Key en lugar de x: Name cuando no se haga referencia a recursos desde el código subyacente.

## <a name="collections-control-is-using-a-non-virtualizing-panel"></a>El control de colecciones usa un panel sin virtualización

Si proporcionas una plantilla del panel de elementos personalizada (consulta ItemsPanel), asegúrate de usar un panel de virtualización, como ItemsWrapGrid o ItemsStackPanel. Si usas VariableSizedWrapGrid, WrapGrid o StackPanel, no conseguirás virtualización. Además, los siguientes eventos de ListView se generan únicamente cuando se usa una clase ItemsWrapGrid o ItemsStackPanel: ChoosingGroupHeaderContainer, ChoosingItemContainer y ContainerContentChanging.

La virtualización de la interfaz de usuario es la mejora más importante que puedes realizar para mejorar el rendimiento de las colecciones. Esto significa que los elementos de la interfaz de usuario que representan los elementos se crean a petición. Para un control de elementos enlazado a una colección de 1000 elementos, sería un desperdicio de recursos crear la interfaz de usuario de todos los elementos al mismo tiempo, porque todos no pueden mostrarse a la vez. ListView y GridView (y otros controles estándar derivados de ItemsControl) realizan la virtualización de la interfaz de usuario por ti. Cuando los elementos están a punto de desplazarse hacia la vista (a una páginas de distancia), el marco de trabajo genera la interfaz de usuario de los elementos y los almacena. Asimismo, cuando sea improbable que los elementos se muestren de nuevo, el marco de trabajo recuperará la memoria.

La virtualización de la interfaz de usuario es solo uno de varios factores clave para mejorar el rendimiento de las colecciones. Reducir la complejidad de los elementos de colección y la virtualización de datos son otros dos aspectos importantes para mejorar el rendimiento de las colecciones. Para obtener más información acerca de cómo mejorar el rendimiento de las colecciones en ListViews y GridViews, consulta los artículos [Optimización de la interfaz de usuario de ListView y GridView](https://msdn.microsoft.com/windows/uwp/debug-test-perf/optimize-gridview-and-listview) y [Virtualización de datos de ListView y GridView](https://msdn.microsoft.com/windows/uwp/debug-test-perf/listview-and-gridview-data-optimization).

### <a name="impact"></a>Impacto

Un ItemsControl no virtualizado aumentará el tiempo de carga y el uso de recursos al cargar más elementos secundarios de los necesarios.

### <a name="cause"></a>Causa

Usas un panel que no admite la virtualización.

### <a name="solution"></a>Solución

Usa un panel de virtualización como ItemsWrapGrid o ItemsStackPanel.

## <a name="accessibility-uia-elements-with-no-name"></a>Accesibilidad: Elementos UIA sin nombre

En XAML, puedes proporcionar un nombre si estableces AutomationProperties.Name. Muchos elementos de automatización del mismo nivel proporcionan un nombre predeterminado para UIA si no se establece AutomationProperties.Name. 

### <a name="impact"></a>Impacto

Si un usuario alcanza un elemento sin nombre, a menudo no tendrán ninguna manera de saber con qué se relaciona el elemento. 

### <a name="cause"></a>Causa

El nombre de UIA del elemento es nulo o está vacío. Esta regla comprueba qué es lo que ve UIA, no el valor de AutomationProperties.Name.

### <a name="solution"></a>Solución

Establece la propiedad AutomationProperties.Name en el XAML del control a una cadena localizada adecuada.

A veces la corrección de la aplicación adecuada no es proporcionar un nombre, sino quitar el elemento UIA de todos los árboles excepto los árboles sin procesar. Para hacer eso en XAML, establece AutomationProperties.AccessibilityView = "Raw".

## <a name="accessibility-uia-elements-with-the-same-controltype-should-not-have-the-same-name"></a>Accesibilidad: Los elementos UIA con el mismo Controltype no deben tener el mismo nombre

Dos elementos UIA con el mismo elemento primario UIA no deben tener los mismos Name y ControlType. Está bien tener dos controles con el mismo Name si tienen ControlTypes diferentes. 

Esta regla no comprueba nombres duplicados con elementos primarios diferentes. Sin embargo, en la mayoría de los casos, no deben duplicarse Names y ControlTypes dentro de una ventana completa, incluso con diferentes elementos primarios. Los casos en los que resulta aceptable que existan nombres duplicados en una ventana son dos listas con elementos idénticos. En este caso, se espera que los elementos de la lista tengan Names y ControlTypes idénticos.

### <a name="impact"></a>Impacto

Si un usuario alcanza un elemento con el mismo Name y ControlType como otro elemento con el mismo elemento primario UIA, es posible que dicho usuario no pueda distinguir la diferencia entre los elementos.

### <a name="cause"></a>Causa

Los elementos UIA con el mismo elemento primario UIA tienen los mismos Name y ControlType.

### <a name="solution"></a>Solución

Establece un nombre en XAML con AutomationProperties.Name. En las listas donde esto se produce normalmente, usa el enlace para enlazar el valor de AutomationProperties.Name con un origen de datos.


