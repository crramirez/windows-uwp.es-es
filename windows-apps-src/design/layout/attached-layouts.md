---
description: Puedes definir diseños adjunto para su uso con contenedores como el control ItemsRepeater.
title: AttachedLayout
label: AttachedLayout
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 62ecc21d3ed9835ae7360d0c0dfdfa0b09cbdced
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034868"
---
# <a name="attached-layouts"></a>Diseños adjuntos

Un contenedor (por ejemplo, Panel) que delega su lógica de diseño en otro objeto se basa en el objeto de diseño adjunto para establecer el comportamiento de diseño de sus elementos secundarios.  Un modelo de diseño adjunto proporciona flexibilidad a una aplicación para modificar el diseño de los elementos de un entorno de ejecución, o compartir más fácilmente aspectos del diseño entre diferentes partes de la interfaz de usuario (por ejemplo, los elementos de las filas de una tabla que parecen estar alineados dentro de una columna).

En este tema, trataremos lo que implica la creación de un diseño adjunto (con virtualización y sin virtualización), los conceptos y las clases que debe comprender y las ventajas y desventajas que debe tener en cuenta a la hora de decidir entre ellos.

| **Obtención de la biblioteca de la interfaz de usuario de Windows** |
| - |
| Este control se incluye como parte de la biblioteca de interfaz de usuario de Windows, un paquete NuGet que contiene nuevos controles y características de interfaz de usuario destinados a aplicaciones de Windows. Para obtener más información, incluidas instrucciones sobre la instalación, consulta la [introducción a la biblioteca de la interfaz de usuario de Windows](/uwp/toolkits/winui/). |

> **API importantes** :

> * [ScrollViewer](/uwp/api/windows.ui.xaml.controls.scrollviewer)
> * [ItemsRepeater](../controls-and-patterns/items-repeater.md)
> * [Diseño](/uwp/api/microsoft.ui.xaml.controls.layout)
>     * [NonVirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout)
>     * [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)
> * [LayoutContext](/uwp/api/microsoft.ui.xaml.controls.layoutcontext)
>     * [NonVirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext)
>     * [VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext)
> * [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel) (versión preliminar)

## <a name="key-concepts"></a>Conceptos clave

Para disponer un diseño es necesario responder a dos preguntas para cada elemento:

1. ¿Qué * **tamaño** _ tendrá este elemento?

2. ¿Cuál será la _*_posición_*_ de este elemento?

El sistema de diseño de XAML, que responde a estas preguntas, se trata brevemente en el marco de los [paneles personalizados](./custom-panels-overview.md).

### <a name="containers-and-context"></a>Contenedores y contexto

Conceptualmente, el panel de [XAML](/uwp/api/windows.ui.xaml.controls.panel) completa dos roles importantes en el marco:

1. Puede contener elementos secundarios y presenta la bifurcación en el árbol de elementos.
2. Aplica una estrategia de diseño específica a esos elementos secundarios.

Por esta razón, un panel de XAML se ha asociado tradicionalmente al diseño, pero desde un punto de vista técnico va un paso más allá.

El elemento [ItemsRepeater](../controls-and-patterns/items-repeater.md) también se comporta como el panel pero, a diferencia de este, no expone una propiedad secundaria que permita agregar o quitar mediante programación elementos secundarios de UIElement.  En su lugar, el marco administra automáticamente la vigencia de sus elementos secundarios para que se correspondan con una colección de elementos de datos.  Aunque no se deriva del panel, se comporta como un panel y así lo trata el marco.

> [!NOTE]
> El elemento [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel) es un contenedor, derivado del panel, que delega su lógica en el objeto [Layout](/uwp/api/microsoft.ui.xaml.controls.layoutpanel.layout) adjunto.  LayoutPanel se encuentra en _versión preliminar* y actualmente solo está disponible en la *versión preliminar* del paquete WinUI.

#### <a name="containers"></a>Contenedores

Conceptualmente, el [panel](/uwp/api/windows.ui.xaml.controls.panel) es un contenedor de elementos que también tiene la capacidad de representar píxeles para un [fondo](/uwp/api/windows.ui.xaml.controls.panel.background).  Los paneles proporcionan una manera de encapsular la lógica de diseño común en un paquete fácil de usar.

El concepto de **diseño adjunto** hace que la distinción entre los dos roles de contenedor y diseño sea más clara.  Si el contenedor delega su lógica de diseño en otro objeto, ese sería el objeto al que llamaríamos diseño adjunto (como se ve en el fragmento de código a continuación). Los contenedores que heredan de [FrameworkElement](/uwp/api/windows.ui.xaml.frameworkelement), como LayoutPanel, exponen automáticamente las propiedades comunes que proporcionan la entrada al proceso de diseño de XAML (por ejemplo, Height y Width).

```xaml
<LayoutPanel>
    <LayoutPanel.Layout>
        <UniformGridLayout/>
    </LayoutPanel.Layout>
    <Button Content="1"/>
    <Button Content="2"/>
    <Button Content="3"/>
</LayoutPanel>
```

Durante el proceso de diseño, el contenedor se basa en el elemento *UniformGridLayout* adjunto para medir y organizar sus elementos secundarios.

#### <a name="per-container-state"></a>Estado por contenedor

Con un diseño adjunto, una única instancia del objeto de diseño se puede asociar a *muchos* contenedores como en el siguiente fragmento de código; por lo tanto, no debe depender del contenedor host ni hacer referencia directamente a él.  Por ejemplo:

```xaml
<!-- ... --->
<Page.Resources>
    <ExampleLayout x:Name="exampleLayout"/>
<Page.Resources>

<LayoutPanel x:Name="example1" Layout="{StaticResource exampleLayout}"/>
<LayoutPanel x:Name="example2" Layout="{StaticResource exampleLayout}"/>
<!-- ... --->
```

En esta situación, *ExampleLayout* debe considerar detenidamente el estado que usa en el cálculo del diseño y dónde se almacena el estado para evitar que afecte al diseño de los elementos de un panel con el otro.  Sería análogo a un panel personalizado cuya lógica de MeasureOverride y ArrangeOverride dependiera de los valores de sus propiedades *estáticas*.

#### <a name="layoutcontext"></a>LayoutContext

El propósito de [LayoutContext](/uwp/api/microsoft.ui.xaml.controls.layoutcontext) es hacer frente a estos desafíos.  Proporciona al diseño adjunto la capacidad de interactuar con el contenedor host, como recuperar elementos secundarios, sin añadir una dependencia directa entre ambos. El contexto también permite que el diseño almacene cualquier estado que requiera y que pueda estar relacionado con los elementos secundarios del contenedor.

Los diseños simples y sin virtualización no suelen tener la necesidad de mantener un estado, por lo que esto no supone un problema. Sin embargo, un diseño más complejo, como Grid, puede optar por mantener el estado entre la llamada de medida y organización para evitar volver a calcular un valor.

Los diseños con virtualización *a menudo* necesitan mantener un estado entre la medida y la organización, así como entre las fases de diseño iterativo.

#### <a name="initializing-and-uninitializing-per-container-state"></a>Inicialización y anulación de la inicialización de un estado por contenedor

Cuando se adjunta un diseño a un contenedor, se llama a su método [InitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore) y se brinda la oportunidad de inicializar un objeto para almacenar el estado.

Del mismo modo, cuando se quita el diseño de un contenedor, se llamará al método [UninitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore).  Esto brinda al diseño una oportunidad para limpiar cualquier estado asociado a ese contenedor.

El objeto de estado del diseño se puede almacenar y recuperar del contenedor con la propiedad [LayoutState](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate) en el contexto.

### <a name="ui-virtualization"></a>Virtualización de interfaz de usuario

La virtualización de la interfaz de usuario implica retrasar la creación de un objeto de interfaz de usuario hasta _cuando sea necesario_.  Es una optimización del rendimiento.  Los escenarios sin desplazamiento que determinan _cuando sea necesario_ pueden basarse en cualquier número de elementos que sean específicos de la aplicación.  En estos casos, las aplicaciones deben considerar el uso de [x:Load](../../xaml-platform/x-load-attribute.md). No requiere ningún control especial en el diseño.

En escenarios basados en desplazamiento, como una lista, la determinación de _cuando sea necesario_ a menudo se basa en "¿será visible para un usuario?", que depende en gran medida de dónde se colocó durante el proceso de diseño y requiere consideraciones especiales.  Este escenario es un tema central de este documento.

> [!NOTE]
> Aunque no se trata en este documento, las mismas capacidades que permiten la virtualización de la interfaz de usuario en escenarios de desplazamiento se pueden aplicar en escenarios que no son de desplazamiento.  Por ejemplo, un control de barra de herramientas controlado por datos que administra la vigencia de los comandos que presenta y responde a los cambios en el espacio disponible al reciclar o mover elementos entre un área visible y un menú de desbordamiento.

## <a name="getting-started"></a>Introducción

En primer lugar, decide si el diseño que necesitas crear debe admitir la virtualización de la interfaz de usuario.

**Algunos aspectos que hay que tener en cuenta...**

1. Los diseños sin virtualización son más fáciles de crear. Si el número de elementos siempre será pequeño, se recomienda crear un diseño sin virtualización.
2. La plataforma proporciona un conjunto de diseños adjuntos que funcionan con [ItemsRepeater](../controls-and-patterns/items-repeater.md#change-the-layout-of-items) y [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel) para cubrir las necesidades comunes.  Familiarízate con ellos antes de decidirte por la definición de un diseño personalizado.
3. Los diseños con virtualización siempre tienen un costo adicional de CPU y memoria, complejidad y sobrecarga en comparación con un diseño sin virtualización.  Como norma general, si los elementos secundarios que el diseño debe administrar probablemente quepan en un área que sea el triple del tamaño de la ventanilla, puede que no compense el diseño con virtualización. El tamaño triple se describe con más detalle más adelante en este documento, pero se debe a la naturaleza asincrónica del desplazamiento en Windows y su repercusión en la virtualización.

> [!TIP]
> Como punto de referencia, los valores predeterminados para [ListView](/uwp/api/windows.ui.xaml.controls.listview) (and [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)) indican que el reciclaje no comienza hasta que el número de elementos sea suficiente para llenar el triple del tamaño de la ventanilla actual.

**Elección del tipo de base**

![jerarquía de diseño adjunto](images/xaml-attached-layout-hierarchy.png)

El tipo [Layout](/uwp/api/microsoft.ui.xaml.controls.layout) base tiene dos tipos derivados que sirven como punto de partida para crear un diseño adjunto:

1. [NonVirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout)
2. [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)

## <a name="non-virtualizing-layout"></a>Diseño sin virtualización

El enfoque para crear un diseño sin virtualización debe ser familiar para cualquier usuario que haya creado un [panel personalizado](./custom-panels-overview.md).  Se aplican los mismos conceptos.  La principal diferencia es que se usa una clase [NonVirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext) para tener acceso a la colección de [elementos secundarios](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext.children), y el diseño puede optar por almacenar el estado.

1. Deriva del tipo base [NonVirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout) (en lugar del panel).
2. *(Opcional)* Define las propiedades de dependencia que cuando cambien invalidarán el diseño.
3. _( **Nuevo** /Opcional)_ Inicializa cualquier objeto de estado requerido por el diseño como parte de [InitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore). Guárdalo con el contenedor host mediante la clase [LayoutState](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate) proporcionada con el contexto.
4. Invalida el valor [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout.measureoverride) y llama al método [Measure](/uwp/api/windows.ui.xaml.uielement.measure) en todos los elementos secundarios.
5. Invalida el valor [ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout.arrangeoverride) y llama al método [Arrange](/uwp/api/windows.ui.xaml.uielement.arrange) en todos los elementos secundarios.
6. *( **Nuevo** /Opcional)* Limpia cualquier estado guardado como parte de [UninitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore).

### <a name="example-a-simple-stack-layout-varying-sized-items"></a>Ejemplo: Un diseño de pila simple (elementos de tamaño variable)

![MyStackLayout](images/xaml-attached-layout-mystacklayout.png)

Este es un diseño de pila sin virtualización muy básico de elementos de tamaño variable. Faltan propiedades para ajustar el comportamiento del diseño. En la implementación siguiente se muestra cómo el diseño se basa en el objeto de contexto proporcionado por el contenedor para:

1. Obtener el recuento de elementos secundarios
2. Acceder a cada elemento secundario por índice

```csharp
public class MyStackLayout : NonVirtualizingLayout
{
    protected override Size MeasureOverride(NonVirtualizingLayoutContext context, Size availableSize)
    {
        double extentHeight = 0.0;
        foreach (var element in context.Children)
        {
            element.Measure(availableSize);
            extentHeight += element.DesiredSize.Height;
        }

        return new Size(availableSize.Width, extentHeight);
    }

    protected override Size ArrangeOverride(NonVirtualizingLayoutContext context, Size finalSize)
    {
        double offset = 0.0;
        foreach (var element in context.Children)
        {
            element.Arrange(
                new Rect(0, offset, finalSize.Width, element.DesiredSize.Height));
            offset += element.DesiredSize.Height;
        }

        return finalSize;
    }
}
```

```xaml
 <LayoutPanel MaxWidth="196">
    <LayoutPanel.Layout>
        <local:MyStackLayout/>
    </LayoutPanel.Layout>

    <Button HorizontalAlignment="Stretch">1</Button>
    <Button HorizontalAlignment="Right">2</Button>
    <Button HorizontalAlignment="Center">3</Button>
    <Button>4</Button>

</LayoutPanel>
```

## <a name="virtualizing-layouts"></a>Diseños con virtualización

De forma similar a un diseño sin virtualización, los pasos de alto nivel para un diseño con virtualización son los mismos.  La complejidad radica en gran medida en determinar qué elementos se incluirán en la ventanilla y se deben realizar.

1. Deriva del tipo base [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout).
2. (Opcional) Define sus propiedades de dependencia que cuando cambien invalidarán el diseño.
3. Inicializa cualquier objeto de estado que se requerirá por el diseño como parte de [InitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore). Guárdalo con el contenedor host mediante la clase [LayoutState](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate) proporcionada con el contexto.
4. Invalida [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride) y llama al método [Measure](/uwp/api/windows.ui.xaml.uielement.measure) para cada elemento secundario que se deba realizar.
   1. El método [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) se usa para recuperar un elemento UIElement preparado por el marco (por ejemplo, los enlaces de datos aplicados).
5. Invalida el valor [ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride) y llama al método [Arrange](/uwp/api/windows.ui.xaml.uielement.arrange) para cada elemento secundario realizado.
6. Limpia cualquier estado guardado como parte de [UninitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore).

> [!TIP]
> El valor devuelto por [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout) se utiliza como tamaño del contenido virtualizado.

Hay dos enfoques generales que se deben tener en cuenta al crear un diseño con virtualización.  El hecho de elegir uno u otro depende en gran medida de "¿cómo se determinará el tamaño de un elemento".  Si basta conocer el índice de un elemento en el conjunto de datos o los propios datos indican su tamaño final, se consideraría **dependiente de los datos**.  Son más fáciles de crear.  Sin embargo, si la única manera de determinar el tamaño de un elemento es crear y medir la interfaz de usuario, se diría que es **dependiente del contenido**.  Estos son más complejos.

### <a name="the-layout-process"></a>El proceso de diseño

Tanto si va a crear un diseño dependiente de contenido como de los datos, es importante comprender el proceso de diseño y el impacto del desplazamiento asincrónico de Windows.

Esta sería una vista muy simplificada de los pasos realizados por el marco desde el principio para mostrar la interfaz de usuario en la pantalla:

1. Analiza el marcado.

2. Genera un árbol de elementos.

3. Realiza una fase de diseño.

4. Realiza una fase de representación.

Con la virtualización de la interfaz de usuario, la creación de los elementos que se produciría normalmente en el paso 2 se retrasa o finaliza con antelación una vez que se determine que se ha creado contenido suficiente para rellenar la ventanilla. Un contenedor de virtualización (por ejemplo, ItemsRepeater) se aplaza a su diseño adjunto para impulsar este proceso. Proporciona un elemento [VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext) al diseño adjunto que muestra la información adicional que necesita un diseño de virtualización.

**RealizationRect (es decir, ventanilla)**

El desplazamiento en Windows es un proceso asincrónico al subproceso de interfaz de usuario. No se controla mediante el diseño del marco.  En su lugar, la interacción y el movimiento se producen en el compositor del sistema. La ventaja de este enfoque es que el desplazamiento de contenido siempre se puede realizar a 60 fps.  La dificultad, sin embargo, radica en que la "ventanilla", tal como la ve el diseño, podría estar ligeramente desactualizada respecto a lo que realmente está visible en la pantalla. Si un usuario se desplaza rápidamente, puede sobrepasar la velocidad del subproceso de la interfaz de usuario para generar contenido nuevo y "desplazarse a negro". Por esta razón, a menudo es necesario que un diseño de virtualización genere un búfer adicional de elementos preparados suficiente para rellenar un área más grande que la ventanilla. Cuando se mantiene una carga más pesada durante el desplazamiento, al usuario se le sigue presentando el contenido.

![Rectángulo de realización](images/xaml-attached-layout-realizationrect.png)

Dado que la creación de elementos es costosa, la virtualización de contenedores (por ejemplo, [ItemsRepeater](../controls-and-patterns/items-repeater.md)) proporcionará inicialmente al diseño adjunto un objeto [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) que coincida con la ventanilla. En tiempo de inactividad, el contenedor puede aumentar el tamaño del búfer de contenido preparado realizando llamadas repetidas al diseño mediante un rectángulo de realización cada vez mayor. Este comportamiento es una optimización del rendimiento que intenta lograr un equilibrio entre la rapidez del tiempo de inicio y una buena experiencia de desplazamiento. El tamaño de búfer máximo que generará el objeto ItemsRepeater se controla mediante las propiedades [VerticalCacheLength](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.verticalcachelength) y [HorizontalCacheLength](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.verticalcachelength).

**Reutilización de elementos (reciclaje)**

Se espera que el diseño disponga el tamaño de los elementos y los coloque para rellenar el elemento [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) cada vez que se ejecute. De forma predeterminada, [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout) reciclará los elementos no usados al final de cada fase de diseño.

El elemento [VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext) que se pasa al diseño como parte de [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride) y [ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride) proporciona la información adicional que necesita un diseño con virtualización. Entre las acciones que proporciona, algunas de las que se usan con más frecuencia son las siguientes:

1. Consultar el número de elementos de los datos ([ItemCount](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.itemcount)).
2. Recuperar un elemento específico mediante el método [GetItemAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getitemat).
3. Recuperar un objeto [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) que representa la ventanilla y el búfer que el diseño debe rellenar con los elementos realizados.
4. Solicitar el elemento UIElement para un elemento específico con el método [GetOrCreateElement](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat).

La solicitud de un elemento para un índice determinado hará que ese elemento se marque como "en uso" para esa fase del diseño. Si el elemento todavía no existe, se realizará y se preparará automáticamente para su uso (por ejemplo, al aumentar el árbol de la interfaz de usuario definido en un objeto DataTemplate, procesar cualquier enlace de datos, etc.).  De lo contrario, se recuperará de un grupo de instancias existentes.

Al final de cada fase de medida, cualquier elemento existente realizado que no se haya marcado como "en uso" se considerará automáticamente disponible para su reutilización, a menos que se usara la opción para [SuppressAutoRecycle](/uwp/api/microsoft.ui.xaml.controls.elementrealizationoptions) al recuperar el elemento mediante el método [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat). El marco lo mueve automáticamente a un grupo de reciclaje y hace que esté disponible. Posteriormente, se puede extraer para que lo use un contenedor diferente. El marco intenta evitar esto cuando es posible, ya que la reasignación como primario de un elemento puede implicar costos.

Si un diseño de virtualización conoce al principio de cada medida qué elementos dejarán de estar dentro del rectángulo de realización, puede optimizar su reutilización. En lugar de confiar en el comportamiento predeterminado del marco. El diseño puede trasladar los elementos de forma preferente al grupo de reciclaje mediante el método [RecycleElement](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recycleelement).  La llamada a este método antes de solicitar nuevos elementos hace que estos elementos existentes estén disponibles cuando el diseño emite posteriormente una solicitud [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) para un índice que no está asociado a un elemento.

VirtualizingLayoutContext proporciona dos propiedades adicionales diseñadas para los autores de diseño que crean un diseño dependiente del contenido. Se tratan más adelante con más detalle.

1. Un elemento [RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex) que proporciona una _entrada_ opcional al diseño.
2. Un elemento [LayoutOrigin](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.layoutorigin) que es una _salida_ opcional del diseño.

## <a name="data-dependent-virtualizing-layouts"></a>Diseños con virtualización dependientes de datos

Un diseño con virtualización resulta más fácil si sabes cuál debe ser el tamaño de cada elemento sin necesidad de medir el contenido que se va a mostrar.  En este documento, nos referiremos simplemente a esta categoría de diseños con virtualización como **diseños de datos** , ya que normalmente implican inspeccionar los datos.  En función de los datos, una aplicación puede elegir una representación visual con un tamaño conocido, quizás por su parte de los datos o porque se determinó previamente por diseño.

El enfoque general es para que el diseño:

1. Calcule el tamaño y la posición de cada elemento.
2. Como parte de [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride):
   1. Use [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) para determinar qué elementos deben aparecer dentro de la ventanilla.
   2. Recupere el objeto UIElement que debe representar el elemento con el método [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat).
   3. [Mida](/uwp/api/windows.ui.xaml.uielement.measure) el objeto UIElement con el tamaño precalculado.
3. Como parte de [ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride), use [Arrange](/uwp/api/windows.ui.xaml.uielement.arrange) para organizar cada objeto UIElement realizado con la posición precalculada.

> [!NOTE]
> A menudo, un enfoque de diseño de datos es incompatible con la _virtualización de datos_.  En concreto, cuando los únicos datos cargados en la memoria son los datos necesarios para rellenar lo que está visible para el usuario.  La virtualización de datos no hace referencia a la carga diferida o incremental de los datos a medida que un usuario se desplaza hacia abajo donde esos datos permanecen residentes.  En su lugar, se refiere a cuando los elementos se liberan de la memoria a medida que se desplazan fuera de la vista.  Disponer de un diseño de datos que inspeccione todos los elementos de datos como parte de un diseño de datos impide que la virtualización de datos funcione según lo previsto.  Una excepción es un diseño como UniformGridLayout, que presupone que todo tiene el mismo tamaño.

> [!TIP]
> Si vas a crear un control personalizado para una biblioteca de controles que utilizarán otros en una amplia variedad de situaciones, es posible que el diseño de datos no sea una opción.

### <a name="example-xbox-activity-feed-layout"></a>Ejemplo: Diseño de fuente de actividades de Xbox

La interfaz de usuario de la fuente de actividades de Xbox usa un patrón de repetición donde cada línea tiene un mosaico ancho, seguido de dos mosaicos estrechos que se invierten en la línea siguiente. En este diseño, el tamaño de cada elemento es una función de la posición del elemento en el conjunto de datos y el tamaño conocido para los mosaicos (ancho o estrecho).

![Fuente de actividades de Xbox](images/xaml-attached-layout-activityfeedscreenshot.png)

El código siguiente te guía por lo que podría ser una interfaz de usuario con virtualización personalizada para la fuente de actividades para ilustrar el enfoque general que deberías adoptar para un **diseño de datos**.

<table>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">XAML Controls Gallery</strong>, haz clic aquí para abrir la aplicación y ver <a href="xamlcontrolsgallery:/item/ItemsRepeater">ItemsRepeater</a> en acción con este diseño de ejemplo.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

#### <a name="implementation"></a>Implementación

```csharp
/// <summary>
///  This is a custom layout that displays elements in two different sizes
///  wide (w) and narrow (n). There are two types of rows 
///  odd rows - narrow narrow wide
///  even rows - wide narrow narrow
///  This pattern repeats.
/// </summary>

public class ActivityFeedLayout : VirtualizingLayout // STEP #1 Inherit from base attached layout
{
    // STEP #2 - Parameterize the layout
    #region Layout parameters

    // We'll cache copies of the dependency properties to avoid calling GetValue during layout since that
    // can be quite expensive due to the number of times we'd end up calling these.
    private double _rowSpacing;
    private double _colSpacing;
    private Size _minItemSize = Size.Empty;

    /// <summary>
    /// Gets or sets the size of the whitespace gutter to include between rows
    /// </summary>
    public double RowSpacing
    {
        get { return _rowSpacing; }
        set { SetValue(RowSpacingProperty, value); }
    }

    /// <summary>
    /// Gets or sets the size of the whitespace gutter to include between items on the same row
    /// </summary>
    public double ColumnSpacing
    {
        get { return _colSpacing; }
        set { SetValue(ColumnSpacingProperty, value); }
    }

    public Size MinItemSize
    {
        get { return _minItemSize; }
        set { SetValue(MinItemSizeProperty, value); }
    }

    public static readonly DependencyProperty RowSpacingProperty =
        DependencyProperty.Register(
            nameof(RowSpacing),
            typeof(double),
            typeof(ActivityFeedLayout),
            new PropertyMetadata(0, OnPropertyChanged));

    public static readonly DependencyProperty ColumnSpacingProperty =
        DependencyProperty.Register(
            nameof(ColumnSpacing),
            typeof(double),
            typeof(ActivityFeedLayout),
            new PropertyMetadata(0, OnPropertyChanged));

    public static readonly DependencyProperty MinItemSizeProperty =
        DependencyProperty.Register(
            nameof(MinItemSize),
            typeof(Size),
            typeof(ActivityFeedLayout),
            new PropertyMetadata(Size.Empty, OnPropertyChanged));

    private static void OnPropertyChanged(DependencyObject obj, DependencyPropertyChangedEventArgs args)
    {
        var layout = obj as ActivityFeedLayout;
        if (args.Property == RowSpacingProperty)
        {
            layout._rowSpacing = (double)args.NewValue;
        }
        else if (args.Property == ColumnSpacingProperty)
        {
            layout._colSpacing = (double)args.NewValue;
        }
        else if (args.Property == MinItemSizeProperty)
        {
            layout._minItemSize = (Size)args.NewValue;
        }
        else
        {
            throw new InvalidOperationException("Don't know what you are talking about!");
        }

        layout.InvalidateMeasure();
    }

    #endregion

    #region Setup / teardown // STEP #3: Initialize state

    protected override void InitializeForContextCore(VirtualizingLayoutContext context)
    {
        base.InitializeForContextCore(context);

        var state = context.LayoutState as ActivityFeedLayoutState;
        if (state == null)
        {
            // Store any state we might need since (in theory) the layout could be in use by multiple
            // elements simultaneously
            // In reality for the Xbox Activity Feed there's probably only a single instance.
            context.LayoutState = new ActivityFeedLayoutState();
        }
    }

    protected override void UninitializeForContextCore(VirtualizingLayoutContext context)
    {
        base.UninitializeForContextCore(context);

        // clear any state
        context.LayoutState = null;
    }

    #endregion

    #region Layout // STEP #4,5 - Measure and Arrange

    protected override Size MeasureOverride(VirtualizingLayoutContext context, Size availableSize)
    {
        if (this.MinItemSize == Size.Empty)
        {
            var firstElement = context.GetOrCreateElementAt(0);
            firstElement.Measure(new Size(double.PositiveInfinity, double.PositiveInfinity));

            // setting the member value directly to skip invalidating layout
            this._minItemSize = firstElement.DesiredSize;
        }

        // Determine which rows need to be realized.  We know every row will have the same height and
        // only contain 3 items.  Use that to determine the index for the first and last item that
        // will be within that realization rect.
        var firstRowIndex = Math.Max(
            (int)(context.RealizationRect.Y / (this.MinItemSize.Height + this.RowSpacing)) - 1,
            0);
        var lastRowIndex = Math.Min(
            (int)(context.RealizationRect.Bottom / (this.MinItemSize.Height + this.RowSpacing)) + 1,
            (int)(context.ItemCount / 3));

        // Determine which items will appear on those rows and what the rect will be for each item
        var state = context.LayoutState as ActivityFeedLayoutState;
        state.LayoutRects.Clear();

        // Save the index of the first realized item.  We'll use it as a starting point during arrange.
        state.FirstRealizedIndex = firstRowIndex * 3;

        // ideal item width that will expand/shrink to fill available space
        double desiredItemWidth = Math.Max(this.MinItemSize.Width, (availableSize.Width - this.ColumnSpacing * 3) / 4);

        // Foreach item between the first and last index,
        //     Call GetElementOrCreateElementAt which causes an element to either be realized or retrieved
        //       from a recycle pool
        //     Measure the element using an appropriate size
        //
        // Any element that was previously realized which we don't retrieve in this pass (via a call to
        // GetElementOrCreateAt) will be automatically cleared and set aside for later re-use.
        // Note: While this work fine, it does mean that more elements than are required may be
        // created because it isn't until after our MeasureOverride completes that the unused elements
        // will be recycled and available to use.  We could avoid this by choosing to track the first/last
        // index from the previous layout pass.  The diff between the previous range and current range
        // would represent the elements that we can pre-emptively make available for re-use by calling
        // context.RecycleElement(element).
        for (int rowIndex = firstRowIndex; rowIndex < lastRowIndex; rowIndex++)
        {
            int firstItemIndex = rowIndex * 3;
            var boundsForCurrentRow = CalculateLayoutBoundsForRow(rowIndex, desiredItemWidth);

            for (int columnIndex = 0; columnIndex < 3; columnIndex++)
            {
                var index = firstItemIndex + columnIndex;
                var rect = boundsForCurrentRow[index % 3];
                var container = context.GetOrCreateElementAt(index);

                container.Measure(
                    new Size(boundsForCurrentRow[columnIndex].Width, boundsForCurrentRow[columnIndex].Height));

                state.LayoutRects.Add(boundsForCurrentRow[columnIndex]);
            }
        }

        // Calculate and return the size of all the content (realized or not) by figuring out
        // what the bottom/right position of the last item would be.
        var extentHeight = ((int)(context.ItemCount / 3) - 1) * (this.MinItemSize.Height + this.RowSpacing) + this.MinItemSize.Height;

        // Report this as the desired size for the layout
        return new Size(desiredItemWidth * 4 + this.ColumnSpacing * 2, extentHeight);
    }

    protected override Size ArrangeOverride(VirtualizingLayoutContext context, Size finalSize)
    {
        // walk through the cache of containers and arrange
        var state = context.LayoutState as ActivityFeedLayoutState;
        var virtualContext = context as VirtualizingLayoutContext;
        int currentIndex = state.FirstRealizedIndex;

        foreach (var arrangeRect in state.LayoutRects)
        {
            var container = virtualContext.GetOrCreateElementAt(currentIndex);
            container.Arrange(arrangeRect);
            currentIndex++;
        }

        return finalSize;
    }

    #endregion
    #region Helper methods

    private Rect[] CalculateLayoutBoundsForRow(int rowIndex, double desiredItemWidth)
    {
        var boundsForRow = new Rect[3];

        var yoffset = rowIndex * (this.MinItemSize.Height + this.RowSpacing);
        boundsForRow[0].Y = boundsForRow[1].Y = boundsForRow[2].Y = yoffset;
        boundsForRow[0].Height = boundsForRow[1].Height = boundsForRow[2].Height = this.MinItemSize.Height;

        if (rowIndex % 2 == 0)
        {
            // Left tile (narrow)
            boundsForRow[0].X = 0;
            boundsForRow[0].Width = desiredItemWidth;
            // Middle tile (narrow)
            boundsForRow[1].X = boundsForRow[0].Right + this.ColumnSpacing;
            boundsForRow[1].Width = desiredItemWidth;
            // Right tile (wide)
            boundsForRow[2].X = boundsForRow[1].Right + this.ColumnSpacing;
            boundsForRow[2].Width = desiredItemWidth * 2 + this.ColumnSpacing;
        }
        else
        {
            // Left tile (wide)
            boundsForRow[0].X = 0;
            boundsForRow[0].Width = (desiredItemWidth * 2 + this.ColumnSpacing);
            // Middle tile (narrow)
            boundsForRow[1].X = boundsForRow[0].Right + this.ColumnSpacing;
            boundsForRow[1].Width = desiredItemWidth;
            // Right tile (narrow)
            boundsForRow[2].X = boundsForRow[1].Right + this.ColumnSpacing;
            boundsForRow[2].Width = desiredItemWidth;
        }

        return boundsForRow;
    }

    #endregion
}

internal class ActivityFeedLayoutState
{
    public int FirstRealizedIndex { get; set; }

    /// <summary>
    /// List of layout bounds for items starting with the
    /// FirstRealizedIndex.
    /// </summary>
    public List<Rect> LayoutRects
    {
        get
        {
            if (_layoutRects == null)
            {
                _layoutRects = new List<Rect>();
            }

            return _layoutRects;
        }
    }

    private List<Rect> _layoutRects;
}
```

### <a name="optional-managing-the-item-to-uielement-mapping"></a>(Opcional) Administración del elemento para la asignación de UIElement

De forma predeterminada, [VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext) mantiene una asignación entre los elementos realizados y el índice del origen de datos que representan.  Un diseño puede optar por administrar esta propia asignación solicitando siempre la opción de [SuppressAutoRecycle](/uwp/api/microsoft.ui.xaml.controls.elementrealizationoptions) al recuperar un elemento a través del método [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat), lo que evita el comportamiento predeterminado de reciclaje automático.  Un diseño puede decidir hacerlo, por ejemplo, si solo se va a usar cuando el desplazamiento está restringido a una dirección y los elementos que considera siempre serán contiguos (es decir, conocer el índice del primer y el último elemento es suficiente para conocer todos los elementos que se deben realizar).

#### <a name="example-xbox-activity-feed-measure"></a>Ejemplo: Medida de fuente de actividades de Xbox

En el fragmento de código siguiente se muestra la lógica adicional que se podría agregar a MeasureOverride en el ejemplo anterior para administrar la asignación.

```csharp
    protected override Size MeasureOverride(VirtualizingLayoutContext context, Size availableSize)
    {
        //...

        // Determine which items will appear on those rows and what the rect will be for each item
        var state = context.LayoutState as ActivityFeedLayoutState;
        state.LayoutRects.Clear();

         // Recycle previously realized elements that we know we won't need so that they can be used to
        // fill in gaps without requiring us to realize additional elements.
        var newFirstRealizedIndex = firstRowIndex * 3;
        var newLastRealizedIndex = lastRowIndex * 3 + 3;
        for (int i = state.FirstRealizedIndex; i < newFirstRealizedIndex; i++)
        {
            context.RecycleElement(state.IndexToElementMap.Get(i));
            state.IndexToElementMap.Clear(i);
        }

        for (int i = state.LastRealizedIndex; i < newLastRealizedIndex; i++)
        {
            context.RecycleElement(context.IndexElementMap.Get(i));
            state.IndexToElementMap.Clear(i);
        }

        // ...

        // Foreach item between the first and last index,
        //     Call GetElementOrCreateElementAt which causes an element to either be realized or retrieved
        //       from a recycle pool
        //     Measure the element using an appropriate size
        //
        for (int rowIndex = firstRowIndex; rowIndex < lastRowIndex; rowIndex++)
        {
            int firstItemIndex = rowIndex * 3;
            var boundsForCurrentRow = CalculateLayoutBoundsForRow(rowIndex, desiredItemWidth);

            for (int columnIndex = 0; columnIndex < 3; columnIndex++)
            {
                var index = firstItemIndex + columnIndex;
                var rect = boundsForCurrentRow[index % 3];
                UIElement container = null;
                if (state.IndexToElementMap.Contains(index))
                {
                    container = state.IndexToElementMap.Get(index);
                }
                else
                {
                    container = context = context.GetOrCreateElementAt(index, ElementRealizationOptions.ForceCreate | ElementRealizationOptions.SuppressAutoRecycle);
                    state.IndexToElementMap.Add(index, container);
                }

                container.Measure(
                    new Size(boundsForCurrentRow[columnIndex].Width, boundsForCurrentRow[columnIndex].Height));

                state.LayoutRects.Add(boundsForCurrentRow[columnIndex]);
            }
        }

        // ...
   }

internal class ActivityFeedLayoutState
{
    // ...
    Dictionary<int, UIElement> IndexToElementMap { get; set; }
    // ...
}
```

## <a name="content-dependent-virtualizing-layouts"></a>Diseños con virtualización dependientes de contenido

Si primero debe medir el contenido de la interfaz de usuario de un elemento para averiguar su tamaño exacto, se trata de un **diseño dependiente del contenido**.  También puede considerarse como un diseño en el que cada elemento debe calcular su propio tamaño en lugar de que sea el diseño quien indique al elemento su tamaño. Los diseños con virtualización que entran en esta categoría son más complicados.

> [!NOTE]
> Los diseños dependientes del contenido no deben (no deberían) interrumpir la virtualización de datos.

### <a name="estimations"></a>Estimaciones

Los diseños dependientes del contenido dependen de la estimación para adivinar el tamaño del contenido no realizado y la posición del contenido realizado. A medida que cambien estas estimaciones, el contenido realizado cambiará regularmente las posiciones dentro del área con desplazamiento permitido. Esto puede dar lugar a una experiencia de usuario muy frustrante y discordante si no se soluciona. Los posibles problemas y mitigaciones se tratan aquí.

> [!NOTE]
> Los diseños de datos que tienen en cuenta cada elemento y saben el tamaño exacto de todos los elementos, realizados o no, y sus posiciones pueden evitar estos problemas por completo.

**Delimitación de desplazamiento**

XAML proporciona un mecanismo para mitigar los cambios bruscos en la ventanilla haciendo que los controles de desplazamiento admitan la [delimitación de desplazamiento](/uwp/api/windows.ui.xaml.controls.iscrollanchorprovider) implementando la interfaz [IScrollAnchorPovider](/uwp/api/windows.ui.xaml.controls.iscrollanchorprovider). A medida que el usuario manipula el contenido, el control de desplazamiento selecciona continuamente un elemento del conjunto de candidatos seleccionados para seguimiento. Si la posición del elemento delimitador cambia durante el diseño, el control de desplazamiento cambia automáticamente su ventanilla para mantenerla.

El valor del objeto [RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex) proporcionado al diseño puede reflejar ese elemento delimitador seleccionado actualmente elegido por el control de desplazamiento. Como alternativa, si un desarrollador solicita explícitamente que se realice un elemento para un índice con el método [GetOrCreateElement](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.getorcreateelement) en [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater), ese índice se proporciona como [RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex) en la siguiente fase de diseño. Esto permite preparar el diseño para el escenario probable en el que un desarrollador realiza un elemento y, posteriormente, solicita que se visualice a través del método [StartBringIntoView](/uwp/api/windows.ui.xaml.uielement.startbringintoview).

[RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex) es el índice del elemento del origen de datos que un diseño dependiente del contenido debe colocar primero al estimar la posición de sus elementos. Debe actuar como punto de partida para colocar otros elementos realizados.

**Impacto en las barras de desplazamiento**

Incluso con la delimitación de desplazamiento, si las estimaciones del diseño varían mucho, quizás debido a variaciones significativas en el tamaño del contenido, es posible que parezca que la posición del control de la barra de desplazamiento salta.  Esto puede ser molesto para un usuario si parece que el control de posición no sigue la posición del puntero del mouse cuando lo arrastra.

Cuanto más preciso sea el diseño en sus estimaciones, menor será la probabilidad de que un usuario vea el salto del control de la barra de desplazamiento.

### <a name="layout-corrections"></a>Correcciones de diseño

Un diseño dependiente del contenido debe estar preparado para racionalizar su estimación con realidad.  Por ejemplo, a medida que el usuario se desplaza a la parte superior del contenido y el diseño realiza el primer elemento, puede que la posición prevista del elemento en relación con el elemento desde el que se inició haga que aparezca en otro lugar distinto del origen de (x:0, y:0). Cuando esto ocurre, el diseño puede utilizar la propiedad [LayoutOrigin](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.layoutorigin) para establecer la posición calculada como el nuevo origen de diseño.  El resultado neto es similar a la delimitación de desplazamiento en la que la ventanilla del control de desplazamiento se ajusta automáticamente para tener en cuenta la posición del contenido según lo indicado por el diseño.

![Corrección de LayoutOrigin](images/xaml-attached-layout-origincorrection.png)

### <a name="disconnected-viewports"></a>Ventanillas desconectadas

El tamaño devuelto desde el método [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride) del diseño representa la mejor aproximación al tamaño del contenido que puede cambiar con cada diseño sucesivo.  A medida que un usuario se desplaza, el diseño se volverá a evaluar continuamente con un elemento [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) actualizado.

Si un usuario arrastra el control de posición muy rápidamente, es posible que la ventanilla, desde la perspectiva del diseño, parezca hacer grandes saltos en los que la posición anterior no se superpone a la posición actual.  Esto se debe a la naturaleza asincrónica del desplazamiento. También es posible que una aplicación que consume el diseño solicite que un elemento se presente en la vista de un elemento que no se haya realizado actualmente y que se estime que está fuera del intervalo actual del que el diseño realiza el seguimiento.

Cuando el diseño detecta que su estimación es incorrecta o ve un cambio de ventanilla inesperado, debe reorientar su posición inicial.  Los diseños con virtualización que se incluyen como parte de los controles de XAML se desarrollan como diseños dependientes del contenido, ya que imponen menos restricciones en la naturaleza del contenido que se va a mostrar.


### <a name="example-simple-virtualizing-stack-layout-for-variable-sized-items"></a>Ejemplo: Diseño de pila con virtualización simple para elementos de tamaño variable

En el ejemplo siguiente se muestra un diseño de pila simple para elementos de tamaño variable que:

* admite la virtualización de interfaz de usuario,
* utiliza estimaciones para suponer el tamaño de los elementos no realizados,
* es consciente de los posibles cambios interrumpidos de la ventanilla y
* aplica correcciones de diseño para tener en cuenta esos cambios.

**Uso: marcado**

```xaml
<ScrollViewer>

  <ItemsRepeater x:Name="repeater" >
    <ItemsRepeater.Layout>

      <local:VirtualizingStackLayout />

    </ItemsRepeater.Layout>
    <ItemsRepeater.ItemTemplate>
      <DataTemplate x:Key="item">
        <UserControl IsTabStop="True" UseSystemFocusVisuals="True" Margin="5">
          <StackPanel BorderThickness="1" Background="LightGray" Margin="5">
            <Image x:Name="recipeImage" Source="{Binding ImageUri}"  Width="100" Height="100"/>
              <TextBlock x:Name="recipeDescription"
                         Text="{Binding Description}"
                         TextWrapping="Wrap"
                         Margin="10" />
          </StackPanel>
        </UserControl>
      </DataTemplate>
    </ItemsRepeater.ItemTemplate>
  </ItemsRepeater>

</ScrollViewer>
```

**Código subyacente: Main.cs**

```csharp
string _lorem = @"Lorem ipsum dolor sit amet, consectetur adipiscing elit. Etiam laoreet erat vel massa rutrum, eget mollis massa vulputate. Vivamus semper augue leo, eget faucibus nulla mattis nec. Donec scelerisque lacus at dui ultricies, eget auctor ipsum placerat. Integer aliquet libero sed nisi eleifend, nec rutrum arcu lacinia. Sed a sem et ante gravida congue sit amet ut augue. Donec quis pellentesque urna, non finibus metus. Proin sed ornare tellus. Lorem ipsum dolor sit amet, consectetur adipiscing elit. Etiam laoreet erat vel massa rutrum, eget mollis massa vulputate. Vivamus semper augue leo, eget faucibus nulla mattis nec. Donec scelerisque lacus at dui ultricies, eget auctor ipsum placerat. Integer aliquet libero sed nisi eleifend, nec rutrum arcu lacinia. Sed a sem et ante gravida congue sit amet ut augue. Donec quis pellentesque urna, non finibus metus. Proin sed ornare tellus.";

var rnd = new Random();
var data = new ObservableCollection<Recipe>(Enumerable.Range(0, 300).Select(k =>
               new Recipe
               {
                   ImageUri = new Uri(string.Format("ms-appx:///Images/recipe{0}.png", k % 8 + 1)),
                   Description = k + " - " + _lorem.Substring(0, rnd.Next(50, 350))
               }));

repeater.ItemsSource = data;
```

**Código: VirtualizingStackLayout.cs**

```csharp
// This is a sample layout that stacks elements one after
// the other where each item can be of variable height. This is
// also a virtualizing layout - we measure and arrange only elements
// that are in the viewport. Not measuring/arranging all elements means
// that we do not have the complete picture and need to estimate sometimes.
// For example the size of the layout (extent) is an estimation based on the
// average heights we have seen so far. Also, if you drag the mouse thumb
// and yank it quickly, then we estimate what goes in the new viewport.

// The layout caches the bounds of everything that are in the current viewport.
// During measure, we might get a suggested anchor (or start index), we use that
// index to start and layout the rest of the items in the viewport relative to that
// index. Note that since we are estimating, we can end up with negative origin when
// the viewport is somewhere in the middle of the extent. This is achieved by setting the
// LayoutOrigin property on the context. Once this is set, future viewport will account
// for the origin.
public class VirtualizingStackLayout : VirtualizingLayout
{
    // Estimation state
    List<double> m_estimationBuffer = Enumerable.Repeat(0d, 100).ToList();
    int m_numItemsUsedForEstimation = 0;
    double m_totalHeightForEstimation = 0;

    // State to keep track of realized bounds
    int m_firstRealizedDataIndex = 0;
    List<Rect> m_realizedElementBounds = new List<Rect>();

    Rect m_lastExtent = new Rect();

    protected override Size MeasureOverride(VirtualizingLayoutContext context, Size availableSize)
    {
        var viewport = context.RealizationRect;
        DebugTrace("MeasureOverride: Viewport " + viewport);

        // Remove bounds for elements that are now outside the viewport.
        // Proactive recycling elements means we can reuse it during this measure pass again.
        RemoveCachedBoundsOutsideViewport(viewport);

        // Find the index of the element to start laying out from - the anchor
        int startIndex = GetStartIndex(context, availableSize);

        // Measure and layout elements starting from the start index, forward and backward.
        Generate(context, availableSize, startIndex, forward:true);
        Generate(context, availableSize, startIndex, forward:false);

        // Estimate the extent size. Note that this can have a non 0 origin.
        m_lastExtent = EstimateExtent(context, availableSize);
        context.LayoutOrigin = new Point(m_lastExtent.X, m_lastExtent.Y);
        return new Size(m_lastExtent.Width, m_lastExtent.Height);
    }

    protected override Size ArrangeOverride(VirtualizingLayoutContext context, Size finalSize)
    {
        DebugTrace("ArrangeOverride: Viewport" + context.RealizationRect);
        for (int realizationIndex = 0; realizationIndex < m_realizedElementBounds.Count; realizationIndex++)
        {
            int currentDataIndex = m_firstRealizedDataIndex + realizationIndex;
            DebugTrace("Arranging " + currentDataIndex);

            // Arrange the child. If any alignment needs to be done, it
            // can be done here.
            var child = context.GetOrCreateElementAt(currentDataIndex);
            var arrangeBounds = m_realizedElementBounds[realizationIndex];
            arrangeBounds.X -= m_lastExtent.X;
            arrangeBounds.Y -= m_lastExtent.Y;
            child.Arrange(arrangeBounds);
        }

        return finalSize;
    }

    // The data collection has changed, since we are maintaining the bounds of elements
    // in the viewport, we will update the list to account for the collection change.
    protected override void OnItemsChangedCore(VirtualizingLayoutContext context, object source, NotifyCollectionChangedEventArgs args)
    {
        InvalidateMeasure();
        if (m_realizedElementBounds.Count > 0)
        {
            switch (args.Action)
            {
                case NotifyCollectionChangedAction.Add:
                    OnItemsAdded(args.NewStartingIndex, args.NewItems.Count);
                    break;
                case NotifyCollectionChangedAction.Replace:
                    OnItemsRemoved(args.OldStartingIndex, args.OldItems.Count);
                    OnItemsAdded(args.NewStartingIndex, args.NewItems.Count);
                    break;
                case NotifyCollectionChangedAction.Remove:
                    OnItemsRemoved(args.OldStartingIndex, args.OldItems.Count);
                    break;
                case NotifyCollectionChangedAction.Reset:
                    m_realizedElementBounds.Clear();
                    m_firstRealizedDataIndex = 0;
                    break;
                default:
                    throw new NotImplementedException();
            }
        }
    }

    // Figure out which index to use as the anchor and start laying out around it.
    private int GetStartIndex(VirtualizingLayoutContext context, Size availableSize)
    {
        int startDataIndex = -1;
        var recommendedAnchorIndex = context.RecommendedAnchorIndex;
        bool isSuggestedAnchorValid = recommendedAnchorIndex != -1;

        if (isSuggestedAnchorValid)
        {
            if (IsRealized(recommendedAnchorIndex))
            {
                startDataIndex = recommendedAnchorIndex;
            }
            else
            {
                ClearRealizedRange();
                startDataIndex = recommendedAnchorIndex;
            }
        }
        else
        {
            // Find the first realized element that is visible in the viewport.
            startDataIndex = GetFirstRealizedDataIndexInViewport(context.RealizationRect);
            if (startDataIndex < 0)
            {
                startDataIndex = EstimateIndexForViewport(context.RealizationRect, context.ItemCount);
                ClearRealizedRange();
            }
        }

        // We have an anchorIndex, realize and measure it and
        // figure out its bounds.
        if (startDataIndex != -1 & context.ItemCount > 0)
        {
            if (m_realizedElementBounds.Count == 0)
            {
                m_firstRealizedDataIndex = startDataIndex;
            }

            var newAnchor = EnsureRealized(startDataIndex);
            DebugTrace("Measuring start index " + startDataIndex);
            var desiredSize = MeasureElement(context, startDataIndex, availableSize);

            var bounds = new Rect(
                0,
                newAnchor ?
                    (m_totalHeightForEstimation / m_numItemsUsedForEstimation) * startDataIndex : GetCachedBoundsForDataIndex(startDataIndex).Y,
                availableSize.Width,
                desiredSize.Height);
            SetCachedBoundsForDataIndex(startDataIndex, bounds);
        }

        return startDataIndex;
    }


    private void Generate(VirtualizingLayoutContext context, Size availableSize, int anchorDataIndex, bool forward)
    {
        // Generate forward or backward from anchorIndex until we hit the end of the viewport
        int step = forward ? 1 : -1;
        int previousDataIndex = anchorDataIndex;
        int currentDataIndex = previousDataIndex + step;
        var viewport = context.RealizationRect;
        while (IsDataIndexValid(currentDataIndex, context.ItemCount) &&
            ShouldContinueFillingUpSpace(previousDataIndex, forward, viewport))
        {
            EnsureRealized(currentDataIndex);
            DebugTrace("Measuring " + currentDataIndex);
            var desiredSize = MeasureElement(context, currentDataIndex, availableSize);
            var previousBounds = GetCachedBoundsForDataIndex(previousDataIndex);
            Rect currentBounds = new Rect(0,
                                          forward ? previousBounds.Y + previousBounds.Height : previousBounds.Y - desiredSize.Height,
                                          availableSize.Width,
                                          desiredSize.Height);
            SetCachedBoundsForDataIndex(currentDataIndex, currentBounds);
            previousDataIndex = currentDataIndex;
            currentDataIndex += step;
        }
    }

    // Remove bounds that are outside the viewport, leaving one extra since our
    // generate stops after generating one extra to know that we are outside the
    // viewport.
    private void RemoveCachedBoundsOutsideViewport(Rect viewport)
    {
        int firstRealizedIndexInViewport = 0;
        while (firstRealizedIndexInViewport < m_realizedElementBounds.Count &&
               !Intersects(m_realizedElementBounds[firstRealizedIndexInViewport], viewport))
        {
            firstRealizedIndexInViewport++;
        }

        int lastRealizedIndexInViewport = m_realizedElementBounds.Count - 1;
        while (lastRealizedIndexInViewport >= 0 &&
            !Intersects(m_realizedElementBounds[lastRealizedIndexInViewport], viewport))
        {
            lastRealizedIndexInViewport--;
        }

        if (firstRealizedIndexInViewport > 0)
        {
            m_firstRealizedDataIndex += firstRealizedIndexInViewport;
            m_realizedElementBounds.RemoveRange(0, firstRealizedIndexInViewport);
        }

        if (lastRealizedIndexInViewport >= 0 && lastRealizedIndexInViewport < m_realizedElementBounds.Count - 2)
        {
            m_realizedElementBounds.RemoveRange(lastRealizedIndexInViewport + 2, m_realizedElementBounds.Count - lastRealizedIndexInViewport - 3);
        }
    }

    private bool Intersects(Rect bounds, Rect viewport)
    {
        return !(bounds.Bottom < viewport.Top ||
            bounds.Top > viewport.Bottom);
    }

    private bool ShouldContinueFillingUpSpace(int dataIndex, bool forward, Rect viewport)
    {
        var bounds = GetCachedBoundsForDataIndex(dataIndex);
        return forward ?
            bounds.Y < viewport.Bottom :
            bounds.Y > viewport.Top;
    }

    private bool IsDataIndexValid(int currentDataIndex, int itemCount)
    {
        return currentDataIndex >= 0 && currentDataIndex < itemCount;
    }

    private int EstimateIndexForViewport(Rect viewport, int dataCount)
    {
        double averageHeight = m_totalHeightForEstimation / m_numItemsUsedForEstimation;
        int estimatedIndex = (int)(viewport.Top / averageHeight);
        // clamp to an index within the collection
        estimatedIndex = Math.Max(0, Math.Min(estimatedIndex, dataCount));
        return estimatedIndex;
    }

    private int GetFirstRealizedDataIndexInViewport(Rect viewport)
    {
        int index = -1;
        if (m_realizedElementBounds.Count > 0)
        {
            for (int i = 0; i < m_realizedElementBounds.Count; i++)
            {
                if (m_realizedElementBounds[i].Y < viewport.Bottom &&
                   m_realizedElementBounds[i].Bottom > viewport.Top)
                {
                    index = m_firstRealizedDataIndex + i;
                    break;
                }
            }
        }

        return index;
    }

    private Size MeasureElement(VirtualizingLayoutContext context, int index, Size availableSize)
    {
        var child = context.GetOrCreateElementAt(index);
        child.Measure(availableSize);

        int estimationBufferIndex = index % m_estimationBuffer.Count;
        bool alreadyMeasured = m_estimationBuffer[estimationBufferIndex] != 0;
        if (!alreadyMeasured)
        {
            m_numItemsUsedForEstimation++;
        }

        m_totalHeightForEstimation -= m_estimationBuffer[estimationBufferIndex];
        m_totalHeightForEstimation += child.DesiredSize.Height;
        m_estimationBuffer[estimationBufferIndex] = child.DesiredSize.Height;

        return child.DesiredSize;
    }

    private bool EnsureRealized(int dataIndex)
    {
        if (!IsRealized(dataIndex))
        {
            int realizationIndex = RealizationIndex(dataIndex);
            Debug.Assert(dataIndex == m_firstRealizedDataIndex - 1 ||
                dataIndex == m_firstRealizedDataIndex + m_realizedElementBounds.Count ||
                m_realizedElementBounds.Count == 0);

            if (realizationIndex == -1)
            {
                m_realizedElementBounds.Insert(0, new Rect());
            }
            else
            {
                m_realizedElementBounds.Add(new Rect());
            }

            if (m_firstRealizedDataIndex > dataIndex)
            {
                m_firstRealizedDataIndex = dataIndex;
            }

            return true;
        }

        return false;
    }

    // Figure out the extent of the layout by getting the number of items remaining
    // above and below the realized elements and getting an estimation based on
    // average item heights seen so far.
    private Rect EstimateExtent(VirtualizingLayoutContext context, Size availableSize)
    {
        double averageHeight = m_totalHeightForEstimation / m_numItemsUsedForEstimation;

        Rect extent = new Rect(0, 0, availableSize.Width, context.ItemCount * averageHeight);

        if (context.ItemCount > 0 && m_realizedElementBounds.Count > 0)
        {
            extent.Y = m_firstRealizedDataIndex == 0 ?
                            m_realizedElementBounds[0].Y :
                            m_realizedElementBounds[0].Y - (m_firstRealizedDataIndex - 1) * averageHeight;

            int lastRealizedIndex = m_firstRealizedDataIndex + m_realizedElementBounds.Count;
            if (lastRealizedIndex == context.ItemCount - 1)
            {
                var lastBounds = m_realizedElementBounds[m_realizedElementBounds.Count - 1];
                extent.Y = lastBounds.Bottom;
            }
            else
            {
                var lastBounds = m_realizedElementBounds[m_realizedElementBounds.Count - 1];
                int lastRealizedDataIndex = m_firstRealizedDataIndex + m_realizedElementBounds.Count;
                int numItemsAfterLastRealizedIndex = context.ItemCount - lastRealizedDataIndex;
                extent.Height = lastBounds.Bottom + numItemsAfterLastRealizedIndex * averageHeight - extent.Y;
            }
        }

        DebugTrace("Extent " + extent + " with average height " + averageHeight);
        return extent;
    }

    private bool IsRealized(int dataIndex)
    {
        int realizationIndex = dataIndex - m_firstRealizedDataIndex;
        return realizationIndex >= 0 && realizationIndex < m_realizedElementBounds.Count;
    }

    // Index in the m_realizedElementBounds collection
    private int RealizationIndex(int dataIndex)
    {
        return dataIndex - m_firstRealizedDataIndex;
    }

    private void OnItemsAdded(int index, int count)
    {
        // Using the old indexes here (before it was updated by the collection change)
        // if the insert data index is between the first and last realized data index, we need
        // to insert items.
        int lastRealizedDataIndex = m_firstRealizedDataIndex + m_realizedElementBounds.Count - 1;
        int newStartingIndex = index;
        if (newStartingIndex > m_firstRealizedDataIndex &&
            newStartingIndex <= lastRealizedDataIndex)
        {
            // Inserted within the realized range
            int insertRangeStartIndex = newStartingIndex - m_firstRealizedDataIndex;
            for (int i = 0; i < count; i++)
            {
                // Insert null (sentinel) here instead of an element, that way we do not
                // end up creating a lot of elements only to be thrown out in the next layout.
                int insertRangeIndex = insertRangeStartIndex + i;
                int dataIndex = newStartingIndex + i;
                // This is to keep the contiguousness of the mapping
                m_realizedElementBounds.Insert(insertRangeIndex, new Rect());
            }
        }
        else if (index <= m_firstRealizedDataIndex)
        {
            // Items were inserted before the realized range.
            // We need to update m_firstRealizedDataIndex;
            m_firstRealizedDataIndex += count;
        }
    }

    private void OnItemsRemoved(int index, int count)
    {
        int lastRealizedDataIndex = m_firstRealizedDataIndex + m_realizedElementBounds.Count - 1;
        int startIndex = Math.Max(m_firstRealizedDataIndex, index);
        int endIndex = Math.Min(lastRealizedDataIndex, index + count - 1);
        bool removeAffectsFirstRealizedDataIndex = (index <= m_firstRealizedDataIndex);

        if (endIndex >= startIndex)
        {
            ClearRealizedRange(RealizationIndex(startIndex), endIndex - startIndex + 1);
        }

        if (removeAffectsFirstRealizedDataIndex &&
            m_firstRealizedDataIndex != -1)
        {
            m_firstRealizedDataIndex -= count;
        }
    }

    private void ClearRealizedRange(int startRealizedIndex, int count)
    {
        m_realizedElementBounds.RemoveRange(startRealizedIndex, count);
        if (startRealizedIndex == 0)
        {
            m_firstRealizedDataIndex = m_realizedElementBounds.Count == 0 ? 0 : m_firstRealizedDataIndex + count;
        }
    }

    private void ClearRealizedRange()
    {
        m_realizedElementBounds.Clear();
        m_firstRealizedDataIndex = 0;
    }

    private Rect GetCachedBoundsForDataIndex(int dataIndex)
    {
        return m_realizedElementBounds[RealizationIndex(dataIndex)];
    }

    private void SetCachedBoundsForDataIndex(int dataIndex, Rect bounds)
    {
        m_realizedElementBounds[RealizationIndex(dataIndex)] = bounds;
    }

    private Rect GetCachedBoundsForRealizationIndex(int relativeIndex)
    {
        return m_realizedElementBounds[relativeIndex];
    }

    void DebugTrace(string message, params object[] args)
    {
        Debug.WriteLine(message, args);
    }
}
```

## <a name="related-articles"></a>Artículos relacionados

- [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)
- [ScrollViewer](/uwp/api/windows.ui.xaml.controls.scrollviewer)
