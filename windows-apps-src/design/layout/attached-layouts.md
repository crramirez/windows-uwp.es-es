---
Description: Puede definir un adjunto diseños para su uso con contenedores, como el control ItemsRepeater.
title: AttachedLayout
label: AttachedLayout
template: detail.hbs
ms.date: 03/13/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6ff73b13acb5f5970bb79755b0bf5706fb12545a
ms.sourcegitcommit: c10d7843ccacb8529cb1f53948ee0077298a886d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/04/2019
ms.locfileid: "58913995"
---
# <a name="attached-layouts"></a>Diseños adjuntos

Un contenedor (por ejemplo, el Panel) que delega su lógica de diseño a otro objeto se basa en el objeto de diseño conectados para proporcionar el comportamiento de diseño para su elemento secundario de los elementos.  Un modelo de diseño adjunta proporciona flexibilidad para una aplicación modificar el diseño de elementos en tiempo de ejecución, o compartir más fácilmente los aspectos de diseño entre las distintas partes de la interfaz de usuario (por ejemplo, los elementos de las filas de una tabla que parecen estar alineado dentro de una columna).

En este tema, se explica lo que implica la creación de un diseño adjunto (virtualización y sin virtualizar), los conceptos y las clases, deberá entender, y las ventajas y desventajas deberá tener en cuenta al decidir entre ellos.

| **Obtener la biblioteca de interfaz de usuario de Windows** |
| - |
| Este control se incluye como parte de la biblioteca de interfaz de usuario de Windows, un paquete de NuGet que contiene los nuevos controles y características de interfaz de usuario para aplicaciones UWP. Para obtener más información, incluidas las instrucciones de instalación, consulte el [Introducción a la biblioteca de interfaz de usuario de Windows](https://docs.microsoft.com/uwp/toolkits/winui/). |

> **API importantes**:

> * [ScrollViewer](/uwp/api/windows.ui.xaml.controls.scrollviewer)
> * [ItemsRepeater](/windows/uwp/design/controls-and-patterns/items-repeater)
> * [Diseño](/uwp/api/microsoft.ui.xaml.controls.layout)
>     * [NonVirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout)
>     * [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)
> * [LayoutContext](/uwp/api/microsoft.ui.xaml.controls.layoutcontext)
>     * [NonVirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext)
>     * [VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext)
> * [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel) (versión preliminar)

## <a name="key-concepts"></a>Conceptos clave

Realiza el diseño requiere que para cada elemento responderse dos preguntas:

1. ¿Qué ***tamaño*** será este elemento?

2. ¿Qué le el ***posición*** de este elemento ser?

Sistema de diseño de XAML, que responde a estas preguntas, se trata brevemente como parte de la discusión de [paneles personalizados](/windows/uwp/design/layout/custom-panels-overview).

### <a name="containers-and-context"></a>Contenedores y contexto

Del conceptualmente, XAML [Panel](/uwp/api/windows.ui.xaml.controls.panel) rellena dos funciones importantes en el marco de trabajo:

1. Puede contener elementos secundarios y presenta la bifurcación en el árbol de elementos.
2. Una estrategia de diseño específico se aplica a los elementos secundarios.

Por este motivo, un Panel en XAML a veces ha sido un sinónimo de diseño, aunque técnicamente hablando, hace algo más que el diseño.

El [ItemsRepeater](/windows/uwp/design/controls-and-patterns/items-repeater) también se comporta igual que el Panel, pero, a diferencia del Panel, no expone una propiedad Children que permitiría mediante programación, agregue o quite elementos secundarios de UIElement.  En su lugar, la duración de sus elementos secundarios se administran automáticamente por el marco de trabajo para que coincida con una colección de elementos de datos.  Aunque no se deriva de Panel, se comporta y se trata como un Panel en el marco de trabajo.

> [!NOTE]
> El [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel) es un contenedor, derivado de Panel, que delega su lógica para el archivo adjunto [diseño](/uwp/api/microsoft.ui.xaml.controls.layoutpanel.layout) objeto.  LayoutPanel está en *Preview* y solo está disponible actualmente en el *Prerelease* quita del paquete WinUI.

#### <a name="containers"></a>Contenedores

Conceptualmente, [Panel](/uwp/api/windows.ui.xaml.controls.panel) es un contenedor de elementos que también tiene la capacidad de representar píxeles para un [en segundo plano](/uwp/api/windows.ui.xaml.controls.panel.background).  Los paneles proporcionan una manera de encapsular la lógica de diseño común en un paquete fácil de usar.

El concepto de **adjunta diseño** hace que la distinción entre las dos funciones de contenedor y el diseño más clara.  Si el contenedor delega su lógica de diseño a otro objeto se llamaría a ese objeto el diseño adjuntado tal como se muestra en el siguiente fragmento. Los contenedores que heredan de [FrameworkElement](/uwp/api/windows.ui.xaml.frameworkelement), como el LayoutPanel, expone automáticamente las propiedades comunes que proporcionan la entrada proceso de diseño del XAML (por ejemplo, alto y ancho).

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

Durante el proceso de diseño del contenedor se basa en el archivo adjunto *UniformGridLayout* para medir y organizar sus elementos secundarios.

#### <a name="per-container-state"></a>Estado de cada contenedor

Con un diseño adjuntado, una única instancia del objeto de diseño se puede asociar *muchos* contenedores, como en el siguiente fragmento; por lo tanto, no debe depender o hacer referencia directamente el contenedor del host.  Por ejemplo:

```xaml
<!-- ... --->
<Page.Resources>
    <ExampleLayout x:Name="exampleLayout"/>
<Page.Resources>

<LayoutPanel x:Name="example1" Layout="{StaticResource exampleLayout}"/>
<LayoutPanel x:Name="example2" Layout="{StaticResource exampleLayout}"/>
<!-- ... --->
```

En esta situación *ExampleLayout* deben considerar detenidamente el estado que se usa en sus cálculos de diseño y se guarda en ese estado para evitar el impacto en el diseño de elementos en un panel con las demás.  Es análogo a un Panel personalizado cuya lógica MeasureOverride y ArrangeOverride depende de los valores de sus *estático* propiedades.

#### <a name="layoutcontext"></a>LayoutContext

El propósito de la [LayoutContext](/uwp/api/microsoft.ui.xaml.controls.layoutcontext) es afrontar estos desafíos.  Proporciona el diseño adjuntado la capacidad de interactuar con el contenedor del host, como la recuperación de elementos secundarios, sin introducir una dependencia directa entre los dos. El contexto también permite que el diseño almacenar cualquier estado requiere que puede estar relacionados con los elementos secundarios del contenedor.

Diseños simples, no virtualización a menudo es necesario mantener cualquier estado, lo que un problema. Sin embargo, un diseño más complejo, por ejemplo, la cuadrícula, puede elegir mantener el estado entre la medida y organizar la llamada para evitar volver a calcular un valor.

Virtualizar distribuciones *a menudo* debe mantener algún estado entre la medida y organizar, así como entre las fases de diseño iterativo.

#### <a name="initializing-and-uninitializing-per-container-state"></a>Estado de cada contenedor inicializando y cancelar la inicialización

Cuando se adjunta un diseño en un contenedor, su [InitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore) método se llama y proporciona una oportunidad para inicializar un objeto para almacenar el estado.

De forma similar, cuando el diseño se está quitando de un contenedor, el [UninitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore) se llamará al método.  Esto proporciona el diseño de una oportunidad para limpiar cualquier estado que tenía asociada con ese contenedor.

Se puede almacenar con objeto de estado del diseño y recuperar desde el contenedor con el [LayoutState](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate) propiedad del contexto.

### <a name="ui-virtualization"></a>Virtualización de interfaz de usuario

Virtualización de interfaz de usuario significa retrasar la creación de un objeto de interfaz de usuario hasta que _cuando sea necesario_.  Es una optimización del rendimiento.  Para determinar los escenarios sin desplazamiento _cuando sea necesario_ puede basarse en cualquier número de cosas que son específicos de la aplicación.  En esos casos, las aplicaciones deben considerar el uso del [x: Load](../../xaml-platform/x-load-attribute.md). No se requiere ningún control especial en su diseño.

En escenarios basados en el desplazamiento como una lista, determinar _cuando sea necesario_ a menudo se basa en "será visible para el usuario" que depende en gran medida en donde se colocó durante el proceso de diseño y requiere consideraciones especiales.  Este escenario es un enfoque de este documento.

> [!NOTE]
> Aunque no se tratan en este documento, se pueden aplicar las mismas funcionalidades que permiten la virtualización de interfaz de usuario en el desplazamiento de escenarios en los escenarios no se pueden desplazar.  Por ejemplo, un controladas por datos control ToolBar que administra la duración de los comandos presenta y responde a los cambios en el espacio disponible por reciclaje o mover elementos entre un área visible y un menú de desbordamiento.

## <a name="getting-started"></a>Introducción

En primer lugar, decida si el diseño, deberá crear debe admitir la virtualización de interfaz de usuario.

**Algunas cosas a tener en cuenta...**

1. Los diseños de virtualización de no son más fáciles de autor. A continuación, crear un diseño que no sea virtualización se recomienda si el número de elementos siempre será pequeño.
2. La plataforma proporciona un conjunto de diseños adjuntos que funcionan con el [ItemsRepeater](/windows/uwp/design/controls-and-patterns/items-repeater#change-the-layout-of-items) y [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel) para cubrir necesidades comunes.  Familiarícese con los que antes de decidir si es necesario definir un diseño personalizado.
3. Los diseños de virtualización siempre tienen algunas CPU adicional y el costo o complejidad y sobrecarga de memoria en comparación con un diseño que no sea virtualización.  Como una regla general si los elementos secundarios el diseño necesitará administrar es probable que quepan en un área que es 3 veces el tamaño de la ventanilla, que no haya ganancia desde un diseño de virtualización. El tamaño 3 x se describe con más detalle más adelante en este documento, pero es debido a la naturaleza asincrónica de desplazamiento en Windows y su impacto en la virtualización.

> [!TIP]
> Como un punto de referencia, la configuración predeterminada para el [ListView](/uwp/api/windows.ui.xaml.controls.listview) (y [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)) son que el reciclaje no comienza hasta que el número de elementos es suficientes para llenar 3 veces el tamaño de la ventanilla actual.

**Elija el tipo base**

![jerarquía de diseño adjunto](images/xaml-attached-layout-hierarchy.png)

La base de [diseño](/uwp/api/microsoft.ui.xaml.controls.layout) tipo tiene dos tipos derivados que sirven como punto de inicio para la creación de un diseño adjunto:

1. [NonVirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout)
2. [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout)

## <a name="non-virtualizing-layout"></a>Diseño no virtualización

El enfoque para crear un diseño que no sea virtualización debería resultarle familiar a cualquier persona que ha creado un [Panel personalizado](/windows/uwp/design/layout/custom-panels-overview).  Se aplican los mismos conceptos.  La principal diferencia es que un [NonVirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext) se usa para tener acceso a la [elementos secundarios](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayoutcontext.children) colección y el diseño pueden optar por almacenar el estado.

1. Derivar del tipo base [NonVirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout) (en lugar del Panel).
2. *(Opcional)*  Definir las propiedades de dependencia que, cuando cambia will invalidar el diseño.
3. _(**New**/opcional)_ inicializar cualquier objeto de estado requerido por el diseño como parte de la [InitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore). Guardar provisionalmente con el contenedor del host mediante el uso de la [LayoutState](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate) proporciona el contexto.
4. Invalidar el [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout.measureoverride) y llamar a la [medida](/uwp/api/windows.ui.xaml.uielement.measure) método en todos los elementos secundarios.
5. Invalidar el [ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.nonvirtualizinglayout.arrangeoverride) y llamar a la [organizar](/uwp/api/windows.ui.xaml.uielement.arrange) método en todos los elementos secundarios.
6. *(**New**/opcional)* limpiar cualquier estado guardado como parte de la [UninitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore).

### <a name="example-a-simple-stack-layout-varying-sized-items"></a>Por ejemplo: Un diseño de pila Simple (elementos de diferentes tamaños)

![MyStackLayout](images/xaml-attached-layout-mystacklayout.png)

Este es un diseño de pila que no sea virtualización muy básica de los elementos de tamaño variables. Carece de las propiedades para ajustar el comportamiento de diseño. La implementación siguiente muestra cómo el diseño se basa en el objeto de contexto proporcionado por el contenedor para:

1. Obtener el recuento de elementos secundarios, y
2. Acceso a cada elemento secundario por índice.

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

## <a name="virtualizing-layouts"></a>Diseños de virtualización

Los pasos generales para un diseño de virtualización similar a un diseño sin virtualización, son el mismo.  La complejidad es en gran medida de determinar qué elementos se encuentran dentro de la ventanilla y deben llevarse a cabo.

1. Derivar del tipo base [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout).
2. (Opcional) Definir sus propiedades de dependencia que, cuando cambia will invalidar el diseño.
3. Inicializar cualquier objeto de estado que tendrá el diseño como parte de la [InitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.initializeforcontextcore). Guardar provisionalmente con el contenedor del host mediante el uso de la [LayoutState](/uwp/api/microsoft.ui.xaml.controls.layoutcontext.layoutstate) proporciona el contexto.
4. Invalidar el [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride) y llamar a la [medida](/uwp/api/windows.ui.xaml.uielement.measure) método para cada elemento secundario que debe llevarse a cabo.
   1. El [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) método se usa para recuperar un elemento de IU que se ha preparado el marco de trabajo (por ejemplo, los enlaces datos aplicados).
5. Invalidar el [ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride) y llamar a la [organizar](/uwp/api/windows.ui.xaml.uielement.arrange) método para cada elemento secundario realizada.
6. (Opcional) Limpiar cualquier estado guardado como parte de la [UninitializeForContextCore](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.uninitializeforcontextcore).

> [!TIP]
> El valor devuelto por la [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout) se utiliza como el tamaño del contenido virtualizado.

Hay dos enfoques generales para tener en cuenta al crear un diseño de virtualización.  Si se selecciona en gran medida de una u otra depende de "¿cómo se determinará el tamaño de un elemento".  Si su suficiente para conocer el índice de un elemento en el conjunto de datos o los propios datos dicta su tamaño final, a continuación, se consideraría **dependiente de los datos**.  Estos son más sencillos de crear.  Si, sin embargo, es la única manera de determinar el tamaño de un elemento consiste en crear y medir la interfaz de usuario, a continuación, nos diría **contenido dependiente**.  Estos son más complejos.

### <a name="the-layout-process"></a>El proceso de diseño

Si va a crear un diseño de contenido dependiente o de datos, es importante comprender el proceso de diseño y el impacto de desplazamiento de asincrónica de Windows.

(Sobre) es una vista simplificada de los pasos realizados por el marco de trabajo de inicio para mostrar la interfaz de usuario en la pantalla que:

1. Analiza el marcado.

2. Genera un árbol de elementos.

3. Realiza una fase de diseño.

4. Realiza una fase de representación.

Con la virtualización de interfaz de usuario, creación de los elementos que normalmente se realizarían en el paso 2 se retrase o terminó al principio de una vez su ha determinado que el contenido suficiente se ha creado para rellenar el área de visualización. Un contenedor de virtualización (por ejemplo, ItemsRepeater) aplaza a su diseño adjunto para controlar este proceso. Proporciona el diseño adjunto con un [VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext) que presenta la información adicional que necesita un diseño de virtualización.

**El RealizationRect (es decir, la ventanilla)**

Desplazamiento en Windows ocurre asincrónica al subproceso de interfaz de usuario. No se controla mediante el diseño de .NET framework.  En su lugar, la interacción y el movimiento se produce en el compositor del sistema. La ventaja de este enfoque es que el movimiento panorámico contenido siempre se realice a 60fps.  El reto, sin embargo, es que la "ventanilla", tal como lo ve el diseño, podría estar ligeramente obsoleta en relación con lo que es visible en pantalla. Si un usuario se desplaza rápidamente, que es posible que superan la velocidad del subproceso de interfaz de usuario para generar nuevos contenidos y "realizar una panorámica en negro". Por este motivo, a menudo es necesario para un diseño de virtualización generar un búfer adicional del preparada elementos suficientes para rellenar un área mayor que la ventanilla. Cuando más pesada carga durante el desplazamiento del usuario seguirá encontrando con contenido.

![Rect realización](images/xaml-attached-layout-realizationrect.png)

Puesto que la creación del elemento es costosa, virtualizar contenedores (por ejemplo, [ItemsRepeater](/windows/uwp/design/controls-and-patterns/items-repeater)) proporcionará inicialmente el diseño adjunto con un [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) que coincida con la ventanilla. En tiempo de inactividad, el contenedor puede alcanzar el búfer de contenido preparada mediante la realización de llamadas repetidas al diseño del uso de un objeto Rect. cada vez mayores de realización Este comportamiento es una optimización del rendimiento que intenta lograr un equilibrio entre la hora de inicio rápido y una buena experiencia panorámica. El tamaño máximo del búfer que generará el ItemsRepeater se controla mediante su [VerticalCacheLength](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.verticalcachelength) y [HorizontalCacheLength](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.verticalcachelength) propiedades.

**Volver a usar elementos (reciclar)**

Se espera que el diseño del tamaño y posición de los elementos que se va a rellenar la [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) cada vez que se ejecute. De forma predeterminada el [VirtualizingLayout](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout) reciclará todos los elementos no utilizados al final de cada fase de diseño.

El [VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext) que se pasa al diseño como parte de la [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride) y [ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride) proporciona la información adicional un se necesita el diseño de virtualización. Algunas de las cosas más usadas proporciona son la capacidad de:

1. El número de elementos de los datos de consulta ([ItemCount](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.itemcount)).
2. Recuperar un determinado elemento mediante el [GetItemAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getitemat) método.
3. Recuperar un [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) que representa el área de visualización y producidas de búfer que el diseño debe rellenar con los elementos.
4. Solicitar el UIElement para un elemento específico con el [GetOrCreateElement](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) método.

Solicitar un elemento de un determinado índice hará que ese elemento se marquen como "en uso" para esa fase del diseño. Si el elemento no existe ya, a continuación, se dio cuenta y se preparan automáticamente para su uso (por ejemplo, lo que infla el árbol de la interfaz de usuario definido en una plantilla de datos, procesamiento de cualquier enlace de datos, etcetera.).  En caso contrario, se obtendrá de un grupo de instancias existentes.

Al final de cada paso de medida existente, dimos cuenta de elemento que no se marcó como "en uso" automáticamente se considera disponible para su reutilización, a menos que la opción de [SuppressAutoRecycle](/uwp/api/microsoft.ui.xaml.controls.elementrealizationoptions) se usó cuando el elemento se ha recuperado a través de la [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) método. El marco de trabajo mueve a un grupo de reciclaje y hace que esté disponible automáticamente. Posteriormente puede extraerse para su uso por un contenedor diferente. El marco de trabajo intenta evitar esta situación si es posible que haya algún coste asociado puede volver a ser el primario de un elemento.

Si sabe que un diseño de virtualización al principio de cada medida que los elementos ya no se encuentran dentro del rectángulo de realización puede optimizar su reutilización. En lugar de confiar en el comportamiento del marco de forma predeterminada. El diseño de forma preventiva puede mover elementos para el grupo de reciclaje mediante el [RecycleElement](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recycleelement) método.  Llamar a este método antes de solicitar nuevos elementos hace que los elementos existentes que estén disponibles cuando el diseño más adelante se emite un [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) solicitud para un índice que ya no está asociado con un elemento.

El VirtualizingLayoutContext proporciona dos propiedades adicionales que se ha diseñado para que los autores de diseño para crear un diseño de contenido dependiente. Se tratan con más detalle más adelante.

1. Un [RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex) que proporciona un elemento opcional _entrada_ al diseño.
2. Un [LayoutOrigin](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.layoutorigin) que es opcional _salida_ del diseño.

## <a name="data-dependent-virtualizing-layouts"></a>Diseños de virtualización dependiente de datos

Un diseño de virtualización es más fácil si sabe cuál debe ser el tamaño de todos los elementos sin necesidad de medir el contenido para mostrar.  En este documento simplemente nos referiremos a esta categoría de virtualizar distribuciones como **diseños datos** ya que normalmente implican inspeccionar los datos.  En función de los datos de una aplicación puede seleccionar una representación visual con un tamaño conocido: quizás porque su parte de los datos o se ha determinado previamente por diseño.

Es el enfoque general para el diseño para:

1. Calcule un tamaño y posición de cada elemento.
2. Como parte de la [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride):
   1. Use la [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect) para determinar qué elementos deben aparecer dentro de la ventanilla.
   2. Recuperar el UIElement que debe representar el elemento con el [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) método.
   3. [Medida](/uwp/api/windows.ui.xaml.uielement.measure) el UIElement con el tamaño calculado previamente.
3. Como parte de la [ArrangeOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.arrangeoverride), [organizar](/uwp/api/windows.ui.xaml.uielement.arrange) cada producidas UIElement con la posición precalculada.

> [!NOTE]
> A menudo no es compatible con un enfoque de diseño de datos _virtualización_.  En concreto, donde los datos solo se cargan en memoria es que los datos necesarios para rellenar lo que es visible para el usuario.  Virtualización de datos no es que hace referencia a la carga diferida o incremental de datos como un usuario se desplaza hacia abajo donde los datos quedan residentes.  En su lugar, se refiere a cuando los elementos se liberan de la memoria a medida que van apareciendo por desplazamiento fuera de la vista.  Tener un diseño de datos que inspecciona cada elemento de datos como parte de un diseño de datos podría impedir la virtualización de datos funcionen según lo previsto.  Una excepción es un diseño similar a la UniformGridLayout que supone que todo lo que tiene el mismo tamaño.

> [!TIP]
> Si va a crear un control personalizado para una biblioteca de controles que se utilizará por otros usuarios en una amplia variedad de situaciones un diseño de datos no puede ser una buena opción.

### <a name="example-xbox-activity-feed-layout"></a>Por ejemplo: Diseño de la fuente de actividades de Xbox

La interfaz de usuario para la fuente de actividades de Xbox usa un patrón de repetición donde cada línea tiene un icono amplio, seguido de dos iconos estrechos que se invierte en la línea siguiente. En este diseño, el tamaño de cada elemento es una función de la posición del elemento en el conjunto de datos y el tamaño conocido de los iconos (vs amplias restringir).

![Fuente de actividades de Xbox](images/xaml-attached-layout-activityfeedscreenshot.png)

El código siguiente le guía a través de qué un personalizado virtualizar podría ser la interfaz de usuario para la fuente de actividades mostrar general enfoque que puede tardar una **diseño datos**.

<table>
<td>
    <p>Si tiene la <strong style="font-weight: semi-bold">Galería de controles XAML</strong> aplicación instalada, haga clic aquí para abrir la aplicación y ver el <a href="xamlcontrolsgallery:/item/ItemsRepeater">ItemsRepeater</a> en acción con este diseño de ejemplo.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación Galería de controles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

#### <a name="implementation"></a>Implementación

```csharp
/// <summary>
///  This is a custom layout that displays elements in two different sizes
///  wide (w) and narrow (n). There are two types of rows 
///  odd rows - narrow narrow wide
///  even rows - wide narrow narrow
///  This pattern repeats.
/// </summary>

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

### <a name="optional-managing-the-item-to-uielement-mapping"></a>(Opcional) Administrar el elemento de asignación de UIElement

De forma predeterminada, el [VirtualizingLayoutContext](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext) mantiene una asignación entre los elementos realizados y el índice del origen de datos que representan.  Puede elegir un diseño administrar esta asignación de la propia solicitando siempre la opción de [SuppressAutoRecycle](/uwp/api/microsoft.ui.xaml.controls.elementrealizationoptions) al recuperar un elemento a través de la [GetOrCreateElementAt](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.getorcreateelementat) método lo que impide que el valor predeterminado comportamiento de reciclaje automático.  Puede elegir un diseño hacer esto, por ejemplo, si solo se usará cuando el desplazamiento está restringido a una dirección y los elementos que se considera que siempre sean contiguos (es decir, sabiendo que el índice del primer y último elemento es suficiente para conocer todos los elementos que se deben leer lized).

#### <a name="example-xbox-activity-feed-measure"></a>Por ejemplo: Medida de la fuente de actividades de Xbox

El siguiente fragmento muestra la lógica adicional que se puede agregar a la MeasureOverride, en el ejemplo anterior, para administrar la asignación.

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

## <a name="content-dependent-virtualizing-layouts"></a>Diseños de virtualización contenido dependiente

Si primero se debe medir el contenido de la interfaz de usuario de un elemento para averiguar su tamaño exacto, entonces es un **contenido dependiente diseño**.  También puede considerar del mismo como un diseño donde cada elemento debe dimensionar en lugar del diseño que le indica el elemento de su tamaño. Diseños de virtualización que entran en esta categoría son más complejas.

> [!NOTE]
> Diseños de contenido dependiente no (no debería) interrumpir la virtualización de datos.

### <a name="estimations"></a>Estimaciones

Diseños de contenido dependiente dependen de estimación que adivinar el tamaño del contenido no realizado y la posición del contenido realizada. A medida que cambian las estimaciones provocará el contenido producido se desplacen con regularidad las posiciones dentro del área desplazable. Esto puede dar lugar a una experiencia de usuario muy frustrante y discordante si no mitiga. Los posibles problemas y las mitigaciones se tratan aquí.

> [!NOTE]
> Los diseños de datos que tenga en cuenta todos los elementos y conocer el tamaño exacto de todos los elementos, o no, realizados y sus posiciones pueden evitar estos problemas por completo.

**Delimitación de desplazamiento**

XAML proporciona un mecanismo para mitigar los turnos de ventanilla repentino manteniendo la compatibilidad con controles el desplazamiento [desplazamiento delimitación](/uwp/api/windows.ui.xaml.controls.iscrollanchorprovider) implementando la [IScrollAnchorPovider](/uwp/api/windows.ui.xaml.controls.iscrollanchorprovider) interfaz. Si el usuario manipula el contenido, el control de desplazamiento continuamente selecciona un elemento del conjunto de candidatos que estaban suscritas en realizar un seguimiento. Si desplaza la posición del elemento delimitador durante el diseño, a continuación, el control de desplazamiento cambiará automáticamente su ventanilla para mantener la ventanilla.

El valor de la [RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex) proporcionado para el diseño puede reflejar ese elemento seleccionado actualmente delimitador elegido por el control de desplazamiento. Como alternativa, si un desarrollador solicita explícitamente que llevarse a cabo un elemento de un índice con el [GetOrCreateElement](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater.getorcreateelement) método en el [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater), a continuación, se indica que el índice como el [ RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex) en la siguiente fase de diseño. Esto permite que el diseño estar preparado para el escenario probable que un desarrollador de un elemento de realización y posteriormente solicita poner en la vista a través de la [StartBringIntoView](/uwp/api/windows.ui.xaml.uielement.startbringintoview) método.

El [RecommendedAnchorIndex](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.recommendedanchorindex) es el índice del elemento en el origen de datos que un diseño de contenido dependiente debe colocar en primer lugar al estimar la posición de sus elementos. Debería servir como punto de partida para el posicionamiento de otros elementos realizados.

**Impacto en las barras de desplazamiento**

Incluso con delimitación de desplazamiento, si las estimaciones del diseño variarán considerablemente, quizás debido a las variaciones importantes en el tamaño del contenido, a continuación, en la posición del control de posición para la barra de desplazamiento puede parecer que cambian de posición.  Esto puede ser discordante para un usuario si no aparece el cuadro de desplazamiento realizar un seguimiento de la posición del puntero del mouse cuando está arrastrándola.

A continuación, más precisos que puede ser el diseño en sus estimaciones el menos probable que un usuario vea saltar de control de posición de la barra de desplazamiento.

### <a name="layout-corrections"></a>Correcciones de diseño

Un diseño de contenido dependiente debe estar preparado para racionalizar su estimación con la realidad.  Por ejemplo, cuando el usuario se desplaza a la parte superior del contenido y el diseño de realización del primer elemento, quizás encuentre que la posición del elemento prevista en relación con el elemento desde el que inició provocaría que aparezca en algún lugar que no sea el origen de (x: 0 y:0). Cuando esto ocurre, puede usar el diseño de la [LayoutOrigin](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.layoutorigin) propiedad para establecer la posición calcula como el nuevo origen de diseño.  El resultado neto es similar para desplazarse de delimitación en el que se ajusta automáticamente ventanilla del control de desplazamiento a la cuenta para la posición del contenido devuelto por el diseño.

![Corregir el LayoutOrigin](images/xaml-attached-layout-origincorrection.png)

### <a name="disconnected-viewports"></a>Ventanillas desconectado

Devuelve el tamaño del diseño [MeasureOverride](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayout.measureoverride) método representa la mejor aproximación con el tamaño del contenido que puede cambiar con cada diseño sucesivo.  Cuando un usuario se desplaza el diseño estará continuamente vuelve a evaluar con un controlador actualizado [RealizationRect](/uwp/api/microsoft.ui.xaml.controls.virtualizinglayoutcontext.realizationrect).

Si un usuario arrastra el control de posición muy rápidamente, a continuación, es posible que para la ventanilla, desde la perspectiva del diseño, parece que grande salta donde la posición anterior no superponga a la posición actual.  Esto es debido a la naturaleza asincrónica de desplazamiento. También es posible que una aplicación que consume el diseño para solicitar que se puede poner un elemento en la vista para un elemento que no se realiza actualmente y se calcula que para diseñar fuera del intervalo actual que hace un seguimiento mediante el diseño.

Cuando el diseño descubre su suposición es correcta o ve un desplazamiento de la ventanilla inesperado, debe volver a orientar su posición inicial.  Los diseños de virtualización que se incluyen como parte de los controles XAML se desarrollan como diseños de contenido dependiente como colocan menos restricciones en la naturaleza del contenido que se mostrará.


### <a name="example-simple-virtualizing-stack-layout-for-variable-sized-items"></a>Por ejemplo: Diseño de pila de virtualización simple para los elementos de tamaño Variable

El ejemplo siguiente muestra un diseño de pila simple para elementos de tamaño variable:

* admite la virtualización de interfaz de usuario,
* usa las estimaciones para estimar el tamaño de los elementos no realizados,
* sea consciente de los turnos de ventanilla discontinuos posibles, y
* aplica las correcciones de diseño para tener en cuenta los turnos.

**Uso: Marcado**

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
