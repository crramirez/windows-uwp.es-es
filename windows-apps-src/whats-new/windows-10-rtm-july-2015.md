---
author: QuinnRadich
Description: Windows 10 y las nuevas herramientas de desarrollador proporcionan las herramientas, características y experiencias con tecnología de la nueva Plataforma universal de Windows (UWP).
title: Novedades para desarrolladores en Windows 10, RTM, julio de 2015
---

# Novedades para desarrolladores en Windows 10, RTM: julio de 2015


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Windows 10 y las nuevas herramientas de desarrollador proporcionan las herramientas, características y experiencias con tecnología de la nueva Plataforma universal de Windows (UWP). Después de [instalar las herramientas y el SDK](https://dev.windows.com/downloads) en Windows 10, estarás listo para [crear una nueva aplicación universal de Windows](https://msdn.microsoft.com/library/windows/apps/bg124288) o para explorar cómo puedes usar tu [código de aplicación existente en Windows](https://msdn.microsoft.com/library/windows/apps/mt238321).

Este es un vistazo característica por característica de las novedades de Windows 10, RTM.

## Diseños adaptativos


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Varias vistas de contenido personalizado</td>
<td align="left">XAML ofrece nueva compatibilidad para definir vistas adaptadas (archivos .xaml) que comparten el mismo archivo de código. Esto facilita la creación y el mantenimiento de diferentes vistas adaptadas a un escenario o una familia de dispositivos específicos. Si la aplicación tiene distintos modelos de navegación, diseño o contenido de interfaz de usuario que son drásticamente diferentes para diversos escenarios, crea varias vistas. Por ejemplo, podrías usar un control [<strong>Pivot</strong>](https://msdn.microsoft.com/library/windows/apps/dn608241) para la aplicación móvil, con navegación optimizada para su uso con una sola mano, y el control [<strong>SplitView</strong>](https://msdn.microsoft.com/library/windows/apps/dn864360) para la aplicación de escritorio, con navegación optimizada para el uso del mouse.</td>
</tr>
<tr class="even">
<td align="left">StateTriggers</td>
<td align="left">La nueva característica [<strong>VisualState.StateTriggers</strong>](https://msdn.microsoft.com/library/windows/apps/dn890396) permite establecer condicionalmente propiedades basadas en el alto o ancho de la ventana, o en un desencadenador personalizado. Antes había que gestionar los eventos [<strong>SizeChanged</strong>] (https://msdn.microsoft.com/library/windows/apps/br209055) de ventanas en el código y llamar a [<strong>VisualStateManager.GoToState</strong>](https://msdn.microsoft.com/library/windows/apps/br209025).</td>
</tr>
<tr class="odd">
<td align="left">Establecedores</td>
<td align="left"><p>La nueva sintaxis de [<strong>VisualState.Setters</strong>](https://msdn.microsoft.com/library/windows/apps/dn890395) permite usar marcado simplificado para definir cambios de propiedades en [<strong>VisualStateManager</strong>](https://msdn.microsoft.com/library/windows/apps/br209021). Antes, tenías que usar un guion gráfico y crear animaciones para aplicar cambios de propiedad, como el cambio de la orientación de un [<strong>StackPanel</strong>](https://msdn.microsoft.com/library/windows/apps/br209635) de Horizontal a Vertical. En las aplicaciones universales de Windows, puedes usar esta sintaxis de Setter más sencilla:</p>
<p><code>&lt;setter target=&quot;stackPanel1.Orientation&quot; value=&quot;Vertical&quot; /&gt;</code></p></td>
</tr>
</tbody>
</table>

 

## Características XAML


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Enlaces de datos compilados (x:Bind)</td>
<td align="left"><p>En las aplicaciones universales de Windows, puedes usar el nuevo mecanismo de enlace basado en el compilador habilitado por la propiedad x:Bind. Los enlaces basados en el compilador están fuertemente tipados y se procesan en tiempo de compilación, lo que resulta más rápido y ofrece errores en tiempo de compilación cuando no coinciden los tipos de enlace. Y dado que los enlaces se traducen al código de la aplicación compilada, ahora puedes depurar enlaces recorriendo el código de Visual Studio para diagnosticar problemas de enlace específicos. También puedes usar x:Bind para enlazar a un método, como este:</p>
<p><code>&lt;textblock text=&quot;{x:Bind Customer.Address.ToString()}&quot; /&gt;</code></p>
<p>Para escenarios de enlace típicos, puedes usar x:Bind en lugar de Binding y obtener un mejor rendimiento y mantenimiento.</p></td>
</tr>
<tr class="even">
<td align="left">Representación incremental declarativa de listas (x:Phase)</td>
<td align="left"><p>En las aplicaciones universales de Windows, el nuevo atributo x:Phase permite realizar la representación incremental, o en fases, de listas con XAML en lugar de código. Al moverse en el modo panorámico en largas listas con elementos complejos, es posible que tu aplicación no pueda representar elementos lo suficientemente rápido como para igualar la velocidad de movimiento panorámico, lo que hará que la experiencia de los usuarios sea mala. La representación por fases permite especificar la prioridad de representación de los elementos individuales en un elemento de lista, por lo que únicamente las partes más importantes del elemento de lista se representan en escenarios de movimiento panorámico rápido. Esto produce una experiencia más uniforme de movimiento panorámico para el usuario.</p>
<p>En Windows 8.1, podrías controlar el evento [<strong>ContainerContentChanging</strong>](https://msdn.microsoft.com/library/windows/apps/dn298914) y escribir código para representar elementos de lista en fases. En las aplicaciones para UWP, puedes realizar la representación en fases mediante declaración con el atributo x:Phase. Usado junto con x:Bind de enlaces compilados, x:Phase permite especificar con facilidad una prioridad de representación para cada elemento enlazado de una plantilla de datos. Al desplazarse en movimiento panorámico, el trabajo para representar elementos se divide por tiempo en función de la fase, lo que permite la representación de elementos incremental.</p></td>
</tr>
<tr class="odd">
<td align="left">Carga aplazada de elementos de la interfaz de usuario (x:DeferLoadingStrategy)</td>
<td align="left">En las aplicaciones universales de Windows, la nueva directiva x:DeferLoadingStrategy te permite especificar la carga retrasada de partes de la interfaz de usuario, lo que mejora el rendimiento de inicio y reduce el uso de memoria de la aplicación. Por ejemplo, si la interfaz de usuario de la aplicación tiene un elemento para la validación de datos que se muestra únicamente cuando se escriben datos incorrectos, puede retrasar la carga de ese elemento hasta que se necesite. A continuación, los objetos del elemento no se crean cuando se carga la página; en su lugar, se crean solo cuando hay un error de datos y es necesario agregarlos al árbol visual de la página.</td>
</tr>
<tr class="even">
<td align="left">SplitView</td>
<td align="left">El nuevo control [<strong>SplitView</strong>](https://msdn.microsoft.com/library/windows/apps/dn864360) proporciona una forma de mostrar y ocultar fácilmente contenido transitorio. Normalmente se usa para escenarios de navegación de nivel superior como el &quot;menú hamburguesa&quot;, donde el contenido de la navegación se oculta y se desliza cuando es necesario como resultado de una acción del usuario.</td>
</tr>
<tr class="odd">
<td align="left">RelativePanel</td>
<td align="left">[<strong>RelativePanel</strong>](https://msdn.microsoft.com/library/windows/apps/dn879546) es un nuevo panel de diseño que te permite colocar y alinear objetos secundarios relacionados entre sí o el panel primario. Por ejemplo, puedes especificar que siempre se debe colocar texto en el lado izquierdo del panel y que un botón siempre se debe alinear debajo del texto. Usa <strong>RelativePanel</strong> para crear interfaces de usuario sin un patrón lineal claro que empleen objetos [<strong>StackPanel</strong>](https://msdn.microsoft.com/library/windows/apps/br209635) o [<strong>Grid</strong>](https://msdn.microsoft.com/library/windows/apps/br242704).</td>
</tr>
<tr class="even">
<td align="left">CalendarView</td>
<td align="left">El control [<strong>CalendarView</strong>](https://msdn.microsoft.com/library/windows/apps/dn890052) facilita el proceso de ver y seleccionar fechas e intervalos de fechas mediante una vista mensual personalizable. <strong>CalendarView</strong> admite funciones como las fechas mínima, máxima y sin disponibilidad para limitar qué fechas se pueden seleccionar. También puedes configurar barras de densidad personalizadas que se pueden usar para mostrar lo que se ha &quot;completado&quot; de manera general de la programación en un día concreto.</td>
</tr>
<tr class="odd">
<td align="left">CalendarDatePicker</td>
<td align="left">[<strong>CalendarDatePicker</strong>](https://msdn.microsoft.com/library/windows/apps/dn950083) es un control desplegable que está optimizado para seleccionar una fecha determinada en un control [<strong>CalendarView</strong>](https://msdn.microsoft.com/library/windows/apps/dn890052) cuando es importante la información contextual, como el día de la semana o el nivel de ocupación del calendario. Es similar al control [<strong>DatePicker</strong>](https://msdn.microsoft.com/library/windows/apps/dn298584), pero <strong>DatePicker</strong> está optimizado para seleccionar una fecha conocida, como un cumpleaños.</td>
</tr>
<tr class="even">
<td align="left">MediaTransportControls</td>
<td align="left">La nueva clase [<strong>MediaTransportControls</strong>](https://msdn.microsoft.com/library/windows/apps/dn831962) facilita la personalización de los controles de transporte de un [<strong>MediaElement</strong>](https://msdn.microsoft.com/library/windows/apps/br242926). En Windows 8.1, podrías habilitar los controles de transporte integrados de <strong>MediaElement</strong> o crear controles de transporte propios que llamen a métodos de <strong>MediaElement</strong>. Ahora puedes usar la funcionalidad integrada de <strong>MediaTransportControls</strong> y seguir personalizando con facilidad el aspecto para adaptar tu aplicación.</td>
</tr>
<tr class="odd">
<td align="left">Notificaciones de cambio de propiedad</td>
<td align="left"><p>En las aplicaciones universales de Windows es posible escuchar cambios de propiedades en [<strong>DependencyObject</strong>](https://msdn.microsoft.com/library/windows/apps/br242356), incluso en el caso de propiedades sin eventos de cambio correspondientes.</p>
<p>Las notificaciones actúan como un evento, pero realmente se exponen como una devolución de llamada. La devolución de llamada toma un argumento de remitente como un controlador de eventos, pero no toma un argumento de evento. En su lugar, solamente se pasa el identificador de la propiedad para indicar la propiedad. Con esta información la aplicación puede definir un solo controlador para las notificaciones de varias propiedades. Para obtener más información, consulta [<strong>RegisterPropertyChangedCallback</strong>](https://msdn.microsoft.com/library/windows/apps/dn831902) y [<strong>UnregisterPropertyChangedCallback</strong>](https://msdn.microsoft.com/library/windows/apps/dn831910).</p></td>
</tr>
<tr class="even">
<td align="left">Mapas</td>
<td align="left"><p>La clase [<strong>MapControl</strong>](https://msdn.microsoft.com/library/windows/apps/dn637004) se ha actualizado para proporcionar imágenes aéreas 3D y vistas de calle. Estas nuevas características y las funcionalidades de mapas anteriores ahora están disponibles para las aplicaciones universales de Windows. Agrega mapas a la aplicación con las siguientes API:</p>
<ul>
<li>Espacio de nombres [<strong>Windows.UI.Xaml.Controls.Maps</strong>](https://msdn.microsoft.com/library/windows/apps/dn610751): mostrar mapas.</li>
<li>Espacio de nombres [<strong>Windows.Services.Maps</strong>](https://msdn.microsoft.com/library/windows/apps/dn636979): buscar ubicaciones y rutas.</li>
</ul>
<p>Para empezar a usar ya estas API en una aplicación universal de Windows, solicita una clave desde el [Centro para desarrolladores de Mapas de Bing](https://www.bingmapsportal.com/). Para obtener más información, consulta [Cómo autenticar una aplicación de Mapas](https://msdn.microsoft.com/library/windows/apps/xaml/dn741528). Otra novedad de Windows 10 es que los usuarios de PC y teléfonos pueden descargar mapas sin conexión desde la aplicación Configuración. Cuando están disponibles, [<strong>MapControl</strong>](https://msdn.microsoft.com/library/windows/apps/dn637004) usa los mapas sin conexión para mostrarlos cuando no hay disponible acceso a Internet.</p></td>
</tr>
<tr class="odd">
<td align="left">Asignación de botón de entrada</td>
<td align="left">La clase [<strong>Windows.UI.Xaml.Input.KeyRoutedEventArgs</strong>](https://msdn.microsoft.com/library/windows/apps/hh943072) tiene una nueva propiedad [<strong>OriginalKey</strong>](https://msdn.microsoft.com/library/windows/apps/dn960168) que, junto con la actualización correspondiente de [<strong>Windows.System.VirtualKey</strong>](https://msdn.microsoft.com/library/windows/apps/br241812), te permite obtener el botón de entrada original y sin asignar asociado al evento de entrada de teclado.</td>
</tr>
<tr class="even">
<td align="left">Entrada manuscrita</td>
<td align="left"><p>Ahora es más fácil usar la robusta funcionalidad de entrada de lápiz en las aplicaciones de Windows Runtime con C++, C# o Visual Basic, gracias al control [<strong>InkCanvas</strong>](https://msdn.microsoft.com/library/windows/apps/dn858535) y a las clases subyacentes [<strong>InkPresenter</strong>](https://msdn.microsoft.com/library/windows/apps/dn922011).</p>
<p>El control [<strong>InkCanvas</strong>](https://msdn.microsoft.com/library/windows/apps/dn858535) define un área de superposición para dibujar y representar los trazos de lápiz. La funcionalidad de este control (entrada, procesamiento y representación) proviene de las clases [<strong>InkPresenter</strong>](https://msdn.microsoft.com/library/windows/apps/dn922011), [<strong>InkStroke</strong>](https://msdn.microsoft.com/library/windows/apps/br208485), [<strong>InkRecognizer</strong>](https://msdn.microsoft.com/library/windows/apps/br208478) y [<strong>InkSynchronizer</strong>](https://msdn.microsoft.com/library/windows/apps/dn903979).</p>
<p></p>
<div class="alert">
<strong>Importante</strong> Estas clases no se admiten en aplicaciones de Windows que usan JavaScript.
</div>
<div>
 
</div></td>
</tr>
</tbody>
</table>

 

## Funciones XAML actualizadas


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Actualizaciones de CommandBar y AppBar</td>
<td align="left"><p>Los controles [<strong>CommandBar</strong>](https://msdn.microsoft.com/library/windows/apps/dn279427) y [<strong>AppBar</strong>](https://msdn.microsoft.com/library/windows/apps/hh701927) se han actualizado para ofrecer una API, un comportamiento y una experiencia de usuario coherentes en todas las familias de dispositivos en las aplicaciones para UWP.</p>
<p>El control [<strong>CommandBar</strong>](https://msdn.microsoft.com/library/windows/apps/dn279427) para aplicaciones universales de Windows se ha mejorado para proporcionar un superconjunto de funcionalidad [<strong>AppBar</strong>](https://msdn.microsoft.com/library/windows/apps/hh701927) y una mayor flexibilidad en el uso de la aplicación. Deberías usar <strong>CommandBar</strong> para todas las nuevas aplicaciones universales de Windows en Windows 10. En un <strong>CommandBar</strong> de Windows 8.1 solo puedes usar controles que implementen [<strong>ICommandBarElement</strong>](https://msdn.microsoft.com/library/windows/apps/dn251969), como [<strong>AppBarButton</strong>](https://msdn.microsoft.com/library/windows/apps/dn279244). En las aplicaciones universales de Windows, ahora puedes incluir contenido personalizado en un control <strong>CommandBar</strong> además de <strong>AppBarButton</strong>.</p>
<p>El control [<strong>AppBar</strong>](https://msdn.microsoft.com/library/windows/apps/hh701927) se ha actualizado para que puedas trasladar con más facilidad las aplicaciones de Windows 8.1 que usen <strong>AppBar</strong> a la plataforma universal de Windows. <strong>AppBar</strong> se diseñó para su uso con aplicaciones de pantalla completa y para invocarse con gestos de borde. Se ha actualizado la cuenta de controles para problemas como las aplicaciones con ventana y la falta de gestos de borde en Windows 10.</p>
<p>El [<strong>AppBar.ClosedDisplayMode</strong>](https://msdn.microsoft.com/library/windows/apps/dn633872) oculto, anteriormente solo en Windows Phone, se admite ahora en todas las familias de dispositivos, lo que te permite elegir entre diferentes niveles de sugerencias para comandos. [<strong>AppBar</strong>](https://msdn.microsoft.com/library/windows/apps/hh701927) muestra una sugerencia mínima de forma predeterminada para ofrecerte coherencia al actualizar las aplicaciones de Windows 8.1 a aplicaciones universales de Windows en las que ya no puedes confiar en la compatibilidad de gestos de borde de la plataforma.</p>
<p><strong>Nueva </strong>API de [<strong>AppBar</strong>](https://msdn.microsoft.com/library/windows/apps/hh701927) <strong>:</strong>[<strong>Closing</strong>](https://msdn.microsoft.com/library/windows/apps/dn996483), [<strong>OnClosing</strong>](https://msdn.microsoft.com/library/windows/apps/dn996484), [<strong>Opening</strong>](https://msdn.microsoft.com/library/windows/apps/dn996486), [<strong>OnOpening</strong>](https://msdn.microsoft.com/library/windows/apps/dn996485), [<strong>TemplateSettings</strong>](https://msdn.microsoft.com/library/windows/apps/dn996487).</p>
<p><strong>Nueva </strong>API de [<strong>CommandBar</strong>](https://msdn.microsoft.com/library/windows/apps/dn279427) <strong>:</strong>[<strong>CommandBarOverflowPresenterStyle</strong>](https://msdn.microsoft.com/library/windows/apps/dn975227) y [<strong>CommandBarOverflowPresenter</strong>](https://msdn.microsoft.com/library/windows/apps/dn975225).</p></td>
</tr>
<tr class="even">
<td align="left">Actualizaciones de GridView</td>
<td align="left">Antes de Windows 10, la orientación predeterminada de [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/br242705) era horizontal en Windows y vertical en Windows Phone. En las aplicaciones para UWP, <strong>GridView</strong> usa un diseño vertical de manera predeterminada para todas las familias de dispositivos a fin de garantizar que tengas una experiencia predeterminada coherente.</td>
</tr>
<tr class="odd">
<td align="left">Propiedad AreStickyGroupHeadersEnabled</td>
<td align="left">Cuando muestras datos agrupados en [<strong>ListView</strong>](https://msdn.microsoft.com/library/windows/apps/br242878) o [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/br242705), los encabezados de grupo ahora permanecen visibles cuando te desplazas por la lista. Esto es importante en grandes conjuntos de datos donde el encabezado ofrece contexto para los datos que el usuario está viendo. Sin embargo, en casos donde solo hay algunos elementos en cada grupo, es posible que quieras que los encabezados se desplacen fuera de la pantalla con los elementos. Puedes establecer la propiedad [<strong>AreStickyGroupHeadersEnabled</strong>](https://msdn.microsoft.com/library/windows/apps/dn932028) en [<strong>ItemsStackPanel</strong>](https://msdn.microsoft.com/library/windows/apps/dn298795) y [<strong>ItemsWrapGrid</strong>](https://msdn.microsoft.com/library/windows/apps/dn298849) para controlar este comportamiento.</td>
</tr>
<tr class="even">
<td align="left">Método GroupHeaderContainerFromItemContainer</td>
<td align="left">Cuando se muestran datos agrupados en [<strong>ItemsControl</strong>](https://msdn.microsoft.com/library/windows/apps/br242803), puedes llamar al método [<strong>GroupHeaderContainerFromItemContainer</strong>](https://msdn.microsoft.com/library/windows/apps/dn903984) para obtener una referencia al encabezado primario para el grupo. Por ejemplo, si un usuario elimina el último elemento de un grupo, puedes obtener una referencia al encabezado del grupo y quitar tanto el elemento como el encabezado del grupo a la vez.</td>
</tr>
<tr class="odd">
<td align="left">Evento ChoosingGroupHeaderContainer</td>
<td align="left">El nuevo evento [<strong>ChoosingGroupHeaderContainer</strong>](https://msdn.microsoft.com/library/windows/apps/dn899082) en [<strong>ListViewBase</strong>](https://msdn.microsoft.com/library/windows/apps/br242879) te permite establecer el estado en los encabezados de grupo en [<strong>ListView</strong>](https://msdn.microsoft.com/library/windows/apps/br242878) o [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/br242705). Por ejemplo, puedes controlar este evento para establecer [<strong>AutomationProperties.NameProperty</strong>](https://msdn.microsoft.com/library/windows/apps/br209099) en el encabezado del grupo para que represente el grupo en las tecnologías de ayuda.</td>
</tr>
<tr class="even">
<td align="left">Evento ChoosingItemContainer</td>
<td align="left">El nuevo evento [<strong>ChoosingItemContainer</strong>](https://msdn.microsoft.com/library/windows/apps/dn903989) en [<strong>ListViewBase</strong>](https://msdn.microsoft.com/library/windows/apps/br242879) te da mayor control de la virtualización de la interfaz de usuario en [<strong>ListView</strong>](https://msdn.microsoft.com/library/windows/apps/br242878) o [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/br242705). Usa este evento junto con el evento [<strong>ContainerContentChanging</strong>](https://msdn.microsoft.com/library/windows/apps/dn298914) para mantener tu propia cola de contenedores reciclados a la que recurrir según sea necesario. Por ejemplo, si el origen de datos se ha restablecido debido al filtrado, puedes hacer coincidir rápidamente un conjunto de elementos visuales (ItemContainers) ya creados con sus datos para lograr un rendimiento óptimo.</td>
</tr>
<tr class="odd">
<td align="left">Virtualización de desplazamiento de la lista</td>
<td align="left"><p>Los controles XAML [<strong>ListView</strong>](https://msdn.microsoft.com/library/windows/apps/br242878) y [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/br242705) tienen un nuevo evento [<strong>ListViewBase.ChoosingItemContainer</strong>](https://msdn.microsoft.com/library/windows/apps/dn903989) que mejora el rendimiento del control cuando se produce un cambio en la colección de datos.</p>
<p>En lugar de realizar un restablecimiento completo de la lista, en el que se reproduce la animación de entrada, el sistema ahora mantiene los elementos actuales de la vista, junto con el estado de selección y enfoque; los elementos nuevos y quitados de la ventanilla aparecen y desaparecen sin problemas. Después de realizar un cambio en la colección de datos en la que los contenedores no se destruyen, una aplicación puede relacionar rápidamente cualquier elemento &quot;antiguo&quot; con su contenedor previo y omitir el procesamiento posterior de los métodos de invalidación del ciclo de vida del contenedor. Solo los elementos &quot;nuevos&quot; se procesan y se asocian a los contenedores reciclados o nuevos.</p></td>
</tr>
<tr class="even">
<td align="left">Método SelectRange y propiedad SelectedRanges</td>
<td align="left">En las aplicaciones universales de Windows, los controles [<strong>ListView</strong>](https://msdn.microsoft.com/library/windows/apps/br242878) y [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/br242705) te permiten ahora seleccionar elementos mediante intervalos de índices de elemento, en lugar de como referencias a objetos de elemento. Esta es una manera más eficaz de describir la selección de elementos porque los objetos de elementos no se tienen que crear para cada elemento seleccionado. Para obtener más información, consulta [<strong>ListViewBase.SelectedRanges</strong>](https://msdn.microsoft.com/library/windows/apps/dn904244), [<strong>ListViewBase.SelectRange</strong>](https://msdn.microsoft.com/library/windows/apps/dn904245) y [<strong>ListViewBase.DeselectRange</strong>](https://msdn.microsoft.com/library/windows/apps/dn904241).</td>
</tr>
<tr class="odd">
<td align="left">Nuevas API de ListViewItemPresenter</td>
<td align="left">[<strong>ListView</strong>](https://msdn.microsoft.com/library/windows/apps/br242878) y [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/br242705) usan presentadores de elementos para proporcionar los elementos visuales predeterminados para la selección y el foco. En las aplicaciones para UWP, [<strong>ListViewItemPresenter</strong>](https://msdn.microsoft.com/library/windows/apps/dn298500) y [<strong>GridViewItemPresenter</strong>](https://msdn.microsoft.com/library/windows/apps/dn279298) tienen nuevas propiedades que te permiten personalizar aún más los efectos visuales para los elementos de lista. Las nuevas propiedades son [<strong>CheckBoxBrush</strong>](https://msdn.microsoft.com/library/windows/apps/dn913905), [<strong>CheckMode</strong>](https://msdn.microsoft.com/library/windows/apps/dn913923), [<strong>FocusSecondaryBorderBrush</strong>](https://msdn.microsoft.com/library/windows/apps/dn898370), [<strong>PointerOverForeground</strong>](https://msdn.microsoft.com/library/windows/apps/dn898372), [<strong>PressedBackground</strong>](https://msdn.microsoft.com/library/windows/apps/dn913931) y [<strong>SelectedPressedBackground</strong>](https://msdn.microsoft.com/library/windows/apps/dn913937).</td>
</tr>
<tr class="even">
<td align="left">Actualizaciones de SemanticZoom</td>
<td align="left"><p>El control [<strong>SemanticZoom</strong>](https://msdn.microsoft.com/library/windows/apps/hh702601) tiene ahora un comportamiento coherente en las aplicaciones para UWP en todas las familias de dispositivos.</p>
<p>La acción predeterminada para cambiar entre la vista acercada y la vista alejada es pulsar en un encabezado de grupo en la vista acercada. Esto es lo mismo que el comportamiento en Windows Phone 8.1, pero supone un cambio respecto a Windows 8.1, que usaba el gesto de reducir para hacer zoom. Para cambiar las vistas con reducir para hacer zoom, establece [<strong>ScrollViewer.ZoomMode</strong>](https://msdn.microsoft.com/library/windows/apps/br209601)=&quot;Enabled&quot; en el [<strong>ScrollViewer</strong>](https://msdn.microsoft.com/library/windows/apps/br209527) interno de [<strong>SemanticZoom</strong>](https://msdn.microsoft.com/library/windows/apps/hh702601).</p>
<p>En las aplicaciones universales de Windows, la vista alejada reemplaza la vista acercada y tiene el mismo tamaño que la vista que reemplazó. Este es el mismo comportamiento que en Windows 8.1, pero supone un cambio respecto a Windows Phone 8.1, donde la vista alejada ocupaba todo el tamaño de la pantalla y se representaba encima de todo el resto del contenido.</p></td>
</tr>
<tr class="odd">
<td align="left">Actualizaciones de DatePicker y TimePicker</td>
<td align="left">Los controles [<strong>DatePicker</strong>](https://msdn.microsoft.com/library/windows/apps/dn298584) y [<strong>TimePicker</strong>](https://msdn.microsoft.com/library/windows/apps/dn299280) tienen ahora una implementación coherente para las aplicaciones universales de Windows en todas las familias de dispositivos. También tienen un nuevo aspecto para Windows 10. La parte emergente usa ahora controles [<strong>DatePickerFlyout</strong>](https://msdn.microsoft.com/library/windows/apps/dn625013) y [<strong>TimePickerFlyout</strong>](https://msdn.microsoft.com/library/windows/apps/dn608313) en todos los dispositivos. Es el mismo comportamiento que en Windows Phone 8.1, pero supone un cambio respecto a Windows 8.1, que usaba controles [<strong>ComboBox</strong>](https://msdn.microsoft.com/library/windows/apps/br209348). El uso de los controles flotantes te permite crear selectores de hora y fecha personalizados con mayor facilidad.</td>
</tr>
<tr class="even">
<td align="left">Nuevas API de ScrollViewer</td>
<td align="left">[<strong>ScrollViewer</strong>](https://msdn.microsoft.com/library/windows/apps/br209527) tiene los nuevos eventos [<strong>DirectManipulationStarted</strong>](https://msdn.microsoft.com/library/windows/apps/dn858544) y [<strong>DirectManipulationCompleted</strong>](https://msdn.microsoft.com/library/windows/apps/dn858543) para notificar a la aplicación el inicio y el fin del movimiento panorámico táctil. Puedes controlar estos eventos para coordinar tu interfaz de usuario con estas acciones de usuario.</td>
</tr>
<tr class="odd">
<td align="left">Actualizaciones de MenuFlyout</td>
<td align="left">En las aplicaciones universales de Windows, hay nuevas API que te permiten compilar mejores menús contextuales con mayor facilidad. El nuevo método [<strong>MenuFlyout.ShowAt</strong>](https://msdn.microsoft.com/library/windows/apps/dn913174) te permite especificar dónde quieres que aparezca el control flotante en relación con otro elemento. (y [<strong>MenuFlyout</strong>](https://msdn.microsoft.com/library/windows/apps/dn299030) puede incluso superponerse a los límites de la ventana de la aplicación). Usa la nueva clase [<strong>MenuFlyoutSubItem</strong>](https://msdn.microsoft.com/library/windows/apps/dn913170) para crear menús en cascada.</td>
</tr>
<tr class="even">
<td align="left">Nuevas propiedades de borde para ContentPresenter, Grid y StackPanel</td>
<td align="left">Los controles de contenedor comunes tienen nuevas propiedades de borde que te permiten dibujar un borde alrededor de ellos sin agregar un elemento de borde adicional en el XAML. [<strong>ContentPresenter</strong>](https://msdn.microsoft.com/library/windows/apps/br209378), [<strong>Grid</strong>](https://msdn.microsoft.com/library/windows/apps/br242704) y [<strong>StackPanel</strong>](https://msdn.microsoft.com/library/windows/apps/br209635) tienen estas nuevas propiedades: [<strong>BorderBrush</strong>](https://msdn.microsoft.com/library/windows/apps/dn960179), [<strong>BorderThickness</strong>](https://msdn.microsoft.com/library/windows/apps/dn960181), [<strong>CornerRadius</strong>](https://msdn.microsoft.com/library/windows/apps/dn960183) y [<strong>Padding</strong>](https://msdn.microsoft.com/library/windows/apps/dn960187).</td>
</tr>
<tr class="odd">
<td align="left">Nuevas API de texto en ContentPresenter</td>
<td align="left">[<strong>ContentPresenter</strong>](https://msdn.microsoft.com/library/windows/apps/br209378) tiene nuevas API que ofrecen más control sobre la presentación del texto: [<strong>LineHeight</strong>](https://msdn.microsoft.com/library/windows/apps/dn903982), [<strong>LineStackingStrategy</strong>](https://msdn.microsoft.com/library/windows/apps/dn831933), [<strong>MaxLines</strong>](https://msdn.microsoft.com/library/windows/apps/dn831935) y [<strong>TextWrapping</strong>](https://msdn.microsoft.com/library/windows/apps/dn831937).</td>
</tr>
<tr class="even">
<td align="left">Elementos visuales de foco del sistema</td>
<td align="left">Ahora el sistema crea elementos visuales de foco para controles XAML, en lugar de declararse como elementos XAML en la plantilla de control. Por lo general, los elementos visuales de foco no suelen ser necesarios en los dispositivos móviles, y se mejora el rendimiento de la aplicación al permitir que el sistema los cree y administre según sea necesario. Si necesita un mayor control sobre los elementos visuales de foco, puedes invalidar el comportamiento del sistema y ofrecer una plantilla de control personalizado que defina los elementos visuales de foco. Consulta [<strong>UseSystemFocusVisuals</strong>](https://msdn.microsoft.com/library/windows/apps/dn899076) y [<strong>IsTemplateFocusTarget</strong>](https://msdn.microsoft.com/library/windows/apps/dn899074) para obtener más información.</td>
</tr>
<tr class="odd">
<td align="left">PasswordBox.PasswordRevealMode</td>
<td align="left"><p>En las aplicaciones universales de Windows, la propiedad [<strong>PasswordRevealMode</strong>](https://msdn.microsoft.com/library/windows/apps/dn890867) reemplaza a [<strong>IsPasswordRevealButtonEnabled</strong>](https://msdn.microsoft.com/library/windows/apps/hh702579) para proporcionar un comportamiento coherente en todas las familias de dispositivos.</p>
<p></p>
<div class="alert">
<strong>Precaución</strong> Antes de Windows 10, el botón para mostrar la contraseña no se mostraba de forma predeterminada; en cambio, en las aplicaciones universales de Windows se muestra de forma predeterminada. Si la seguridad de la aplicación requiere que la contraseña se oculte siempre, asegúrate de establecer [<strong>PasswordRevealMode</strong>](https://msdn.microsoft.com/library/windows/apps/dn890867) en Oculto.
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left">Control.IsTextScaleFactorEnabled</td>
<td align="left">La propiedad [<strong>IsTextScaleFactorEnabled</strong>](https://msdn.microsoft.com/library/windows/apps/dn624997) de Windows Phone 8.1 ahora está disponible para aplicaciones universales de Windows en todas las familias de dispositivos.</td>
</tr>
<tr class="odd">
<td align="left">AutoSuggestBox</td>
<td align="left">El control [<strong>AutoSuggestBox</strong>](https://msdn.microsoft.com/library/windows/apps/dn633874) de Windows Phone 8.1 ahora está disponible para aplicaciones universales de Windows en todas las familias de dispositivos y se debe usar en lugar de [<strong>SearchBox</strong>](https://msdn.microsoft.com/library/windows/apps/dn252771). <strong>AutoSuggestBox</strong> proporciona sugerencias mientras el usuario escribe y funciona bien con diversos tipos de entrada, como la entrada táctil, el teclado y los editores de métodos de entrada. También dispone de algunos miembros nuevos para que funcione mejor como cuadro de búsqueda: la propiedad [<strong>QueryIcon</strong>](https://msdn.microsoft.com/library/windows/apps/dn996494) y el evento [<strong>QuerySubmitted</strong>](https://msdn.microsoft.com/library/windows/apps/dn996497).</td>
</tr>
<tr class="even">
<td align="left">ContentDialog</td>
<td align="left">El control [<strong>ContentDialog</strong>](https://msdn.microsoft.com/library/windows/apps/dn633972) de Windows Phone 8.1 ahora está disponible para aplicaciones universales de Windows en todas las familias de dispositivos. <strong>ContentDialog</strong> permite mostrar un cuadro de diálogo modal personalizable que funciona perfectamente en toda la gama de dispositivos.</td>
</tr>
<tr class="odd">
<td align="left">Pivot</td>
<td align="left">El control [<strong>Pivot</strong>](https://msdn.microsoft.com/library/windows/apps/dn608241) de Windows Phone 8.1 ahora está disponible para aplicaciones universales de Windows en todas las familias de dispositivos. Ahora puedes usar el mismo control <strong>Pivot</strong> en tu aplicación para dispositivos móviles y de escritorio. <strong>Pivot</strong> ofrece un comportamiento adaptativo en función del tamaño de la pantalla y el tipo de entrada. Puedes aplicar estilo a un control <strong>Pivot</strong> para ofrecer un comportamiento similar al de las pestañas, con diferentes vistas de información en cada elemento dinámico.</td>
</tr>
</tbody>
</table>

 

## Texto


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">API de texto principales de Windows</td>
<td align="left"><p>El nuevo espacio de nombres [<strong>Windows.UI.Text.Core</strong>](https://msdn.microsoft.com/library/windows/apps/dn958238) ofrece un sistema de cliente/servidor que centraliza el procesamiento de entrada de teclado en un solo servidor.</p>
<p>Puedes usarlo para manipular el búfer de edición del control de entrada de texto personalizado. El servidor de entrada de texto se asegura de que el contenido del control de entrada de texto y el contenido de su búfer de edición estén siempre sincronizados a través de un canal de comunicación asincrónica entre la aplicación y el servidor.</p></td>
</tr>
<tr class="even">
<td align="left">Iconos de vector</td>
<td align="left">El elemento [<strong>Glyphs</strong>](https://msdn.microsoft.com/library/windows/apps/br209921) tiene las nuevas propiedades [<strong>IsColorFontEnabled</strong>](https://msdn.microsoft.com/library/windows/apps/dn900468) y [<strong>ColorFontPalleteIndex</strong>](https://msdn.microsoft.com/library/windows/apps/dn900466) para admitir las fuentes de color; ahora puedes usar un archivo de fuente para representar iconos basados en fuentes. Cuando usas <strong>ColorFontPalleteIndex</strong> para cambiar la paleta de colores, se puede representar un solo icono con conjuntos de colores diferentes; por ejemplo, para mostrar una versión habilitada y deshabilitada del icono.</td>
</tr>
<tr class="odd">
<td align="left">Eventos de ventana del Editor de métodos de entrada</td>
<td align="left">A veces, los usuarios escriben texto mediante un Editor de métodos de entrada que se muestra en una ventana justo debajo de un cuadro de entrada de texto (normalmente para idiomas de Asia Oriental). Puedes usar el evento [<strong>CandidateWindowBoundsChanged</strong>](https://msdn.microsoft.com/library/windows/apps/dn955144) y la propiedad [<strong>DesiredCandidateWindowAlignment</strong>](https://msdn.microsoft.com/library/windows/apps/dn955145) de [<strong>TextBox</strong>](https://msdn.microsoft.com/library/windows/apps/br209683) y [<strong>RichEditBox</strong>] (https://msdn.microsoft.com/library/windows/apps/br227548) para que la interfaz de usuario de la aplicación funcione mejor con la ventana del IME.</td>
</tr>
<tr class="even">
<td align="left">Eventos de composición de texto</td>
<td align="left">[<strong>TextBox</strong>](https://msdn.microsoft.com/library/windows/apps/br209683) y [<strong>RichEditBox</strong>](https://msdn.microsoft.com/library/windows/apps/br227548) tienen nuevos eventos para informar a la aplicación cuando se compone texto con un editor de métodos de entrada: [<strong>TextCompositionStarted</strong>](https://msdn.microsoft.com/library/windows/apps/dn891458), [<strong>TextCompositionEnded</strong>](https://msdn.microsoft.com/library/windows/apps/dn891457) y [<strong>TextCompositionChanged</strong>](https://msdn.microsoft.com/library/windows/apps/dn891456). Puedes controlar estos eventos para coordinar el código de la aplicación con el proceso de composición del texto del IME. Por ejemplo, podrías implementar la funcionalidad de finalización automática en línea para idiomas de Asia oriental.</td>
</tr>
<tr class="odd">
<td align="left">Control mejorado del texto bidireccional</td>
<td align="left"><p>Los controles de texto XAML tienen una nueva API para mejorar el control del texto bidireccional, lo que da lugar a una mejor direccionalidad de párrafo y alineación de texto en una variedad de idiomas de entrada.</p>
<p>El valor predeterminado de la propiedad [<strong>TextReadingOrder</strong>](https://msdn.microsoft.com/library/windows/apps/dn949133) se ha cambiado a <strong>DetectFromContent</strong> para admitir que la detección del orden de lectura esté habilitada de manera predeterminada. La propiedad <strong>TextReadingOrder</strong> también se ha agregado a [<strong>PasswordBox</strong>](https://msdn.microsoft.com/library/windows/apps/br227519), [<strong>RichEditBox</strong>](https://msdn.microsoft.com/library/windows/apps/br227548) y [<strong>TextBox</strong>](https://msdn.microsoft.com/library/windows/apps/br209683).</p>
<p>Puedes establecer la propiedad [<strong>TextAlignment</strong>](https://msdn.microsoft.com/library/windows/apps/br209703) de los controles de texto en el nuevo valor <strong>DetectFromContent</strong> para que sea posible detectar automáticamente la alineación a partir del contenido.</p></td>
</tr>
<tr class="even">
<td align="left">Representación de texto</td>
<td align="left">En Windows 10, el texto en aplicaciones XAML representa ahora, en la mayoría de las situaciones, a casi dos veces la velocidad de Windows 8.1. En la mayoría de los casos, las aplicaciones se beneficiarán de esta mejora sin necesidad de cambios. Además de una representación más rápida, estas mejoras también reducen el consumo de memoria típico de las aplicaciones XAML en un 5 %.</td>
</tr>
</tbody>
</table>

 

## Modelo de aplicación


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Cortana</td>
<td align="left"><p>Amplía la funcionalidad básica de <strong>Cortana</strong> con comandos de voz que inician y ejecutan una acción única en una aplicación externa.</p>
<p>Al integrar la funcionalidad básica de tu aplicación y ofrecer un punto de entrada central para que el usuario realice la mayoría de las tareas sin tener que abrir la aplicación directamente, <strong>Cortana</strong> puede ser un enlace entre tu aplicación y el usuario. En muchos casos, esto puede ahorrarle al usuario un tiempo y esfuerzo considerables.</p>
<p>Aprende cómo [integrar tu aplicación en el lienzo de Cortana](https://msdn.microsoft.com/library/windows/apps/xaml/dn974230). Si necesitas ideas, puedes consultar las recomendaciones de diseño y las directrices de experiencia del usuario específicas para <strong>Cortana</strong> en [Conceptos básicos de diseño de aplicaciones universales de Windows](https://dev.windows.com/design/design-basics).</p></td>
</tr>
<tr class="even">
<td align="left">Explorador de archivos</td>
<td align="left">Los nuevos métodos [<strong>Windows.System.Launcher.LaunchFolderAsync</strong>](https://msdn.microsoft.com/library/windows/apps/dn889616) te permiten iniciar el Explorador de archivos y mostrar el contenido de una carpeta que especifiques.</td>
</tr>
<tr class="odd">
<td align="left">Almacenamiento compartido</td>
<td align="left">La nueva clase [<strong>Windows.ApplicationModel.DataTransfer.SharedStorageAccessManager</strong>](https://msdn.microsoft.com/library/windows/apps/dn889985) y sus métodos te permiten compartir un archivo con otra aplicación al pasar un token de uso compartido cuando inicias la otra aplicación mediante la activación de URI. La aplicación de destino canjea el token para conseguir el archivo compartido por la aplicación de origen.</td>
</tr>
<tr class="even">
<td align="left">Configuración</td>
<td align="left"><p>Muestra páginas de configuración integradas mediante el protocolo ms-settings con el método [<strong>LaunchUriAsync</strong>](https://msdn.microsoft.com/library/windows/apps/hh701476). El siguiente código, por ejemplo, muestra la página de configuración de Wi-Fi.</p>
<p><code>bool result = await Launcher.LaunchUriAsync(new Uri(&quot;ms-settings://network/wifi&quot;));</code></p>
<p>Para obtener una lista de las páginas de configuración que puedes mostrar, consulta [Cómo mostrar páginas de configuración integradas con el protocolo ms-settings](https://msdn.microsoft.com/library/windows/apps/jj207014.aspx).</p></td>
</tr>
<tr class="odd">
<td align="left">Comunicación de una aplicación a otra</td>
<td align="left"><p>Las nuevas API de [comunicación de una aplicación a otra](https://msdn.microsoft.com/library/windows/apps/xaml/dn997827) en Windows 10 permiten que las aplicaciones Windows (así como las aplicaciones web de Windows) se inicien entre sí e intercambien archivos y datos.</p>
<p>Con estas nuevas API, las tareas complejas que hubieran requerido que el usuario usara varias aplicaciones se pueden controlar ahora sin problemas. Por ejemplo, la aplicación podría iniciar una aplicación de redes sociales para elegir un contacto o iniciar una aplicación de confirmación de compra para completar un proceso de pago.</p></td>
</tr>
<tr class="even">
<td align="left">Servicios de aplicaciones</td>
<td align="left">Un servicio de aplicaciones es un modo de que una aplicación proporcione servicios a otras aplicaciones en Windows 10. Un servicio de aplicaciones tiene la forma de una tarea en segundo plano. Las aplicaciones en primer plano pueden llamar a un servicio de aplicación en otra aplicación para realizar tareas en segundo plano. Para obtener información de referencia sobre la API del servicio de aplicaciones, consulta [<strong>Windows.ApplicationModel.AppService</strong>](https://msdn.microsoft.com/library/windows/apps/dn921731).</td>
</tr>
<tr class="odd">
<td align="left">Manifiesto del paquete de la aplicación</td>
<td align="left"><p>Las actualizaciones de la referencia del [esquema de manifiesto del paquete](https://msdn.microsoft.com/library/windows/apps/br211474) para Windows 10 incluyen elementos que se han agregado, quitado y cambiado.</p>
<p>Consulta [Jerarquía de elemento](https://msdn.microsoft.com/library/windows/apps/dn934819) para obtener información de referencia sobre todos los elementos, atributos y tipos del esquema.</p></td>
</tr>
</tbody>
</table>

 

## Dispositivos


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Microsoft Surface Hub</td>
<td align="left"><p>Microsoft Surface Hub es un dispositivo de colaboración de equipo potente y una plataforma de pantalla grande para Universal Windows apps que se ejecutan de forma nativa desde Surface Hub o el dispositivo conectado.</p>
<p>Crea tus propias aplicaciones, diseñadas específicamente para tu negocio, que aprovechen las ventajas de la pantalla grande, la entrada táctil y manuscrita, y una gran cantidad de hardware incorporado como cámaras y sensores.</p>
<p>Echa un vistazo a las recomendaciones de diseño y las directrices de experiencia del usuario específicas de Surface Hub en [Conceptos básicos de diseño de aplicaciones universales de Windows](https://dev.windows.com/design/design-basics). Estos documentos explican técnicas de diseño con capacidad de respuesta para aplicaciones universales de Windows.</p>
<p>Para obtener información sobre cómo admitir aplicaciones compartidas comunes, consulta [<strong>SharedModeSettings</strong>](https://msdn.microsoft.com/library/windows/apps/dn949019).</p>
<p>Para obtener información sobre la entrada de lápiz y detalles sobre la entrada de lápiz multipunto en el nuevo control [<strong>InkCanvas</strong>](https://msdn.microsoft.com/library/windows/apps/dn858535), consulta [<strong>Windows.UI.Input.Inking</strong>](https://msdn.microsoft.com/library/windows/apps/br208524) y [<strong>Windows.UI.Input.Inking.Core</strong>](https://msdn.microsoft.com/library/windows/apps/dn958452).</p>
<p>Para controlar la entrada del sensor, consulta [Integración de dispositivos, impresoras y sensores](https://msdn.microsoft.com/library/windows/apps/br229563).</p></td>
</tr>
<tr class="even">
<td align="left">Ubicación</td>
<td align="left"><p>Windows 10 introduce un nuevo método para pedir al usuario permiso para obtener acceso a su ubicación, [<strong>RequestAccessAsync</strong>](https://msdn.microsoft.com/library/windows/apps/dn859152).</p>
<p>El usuario establece la privacidad de sus datos de ubicación con la <strong>configuración de privacidad de ubicación</strong> en la aplicación <strong>Configuración</strong>. La aplicación puede acceder a la ubicación del usuario solo cuando:</p>
<ul>
<li><strong>La ubicación para este dispositivo</strong> está activada (no es aplicable a Windows 10 para teléfonos).</li>
<li>La configuración de servicios de ubicación “<strong>Ubicación"</strong>” está <strong>activada</strong>.</li>
<li>En <strong>Elegir las aplicaciones que pueden usar tu ubicación</strong>, la aplicación está establecida en el valor <strong>activado</strong>.</li>
</ul>
<p>Es importante llamar a [<strong>RequestAccessAsync</strong>](https://msdn.microsoft.com/library/windows/apps/dn859152) antes de obtener acceso a la ubicación del usuario. En ese momento, la aplicación debe estar en primer plano y se debe llamar a <strong>RequestAccessAsync</strong> desde el subproceso de la interfaz de usuario. La aplicación no puede tener acceso a los datos de ubicación hasta que el usuario conceda permiso.</p></td>
</tr>
<tr class="odd">
<td align="left">AllJoyn</td>
<td align="left"><p>El espacio de nombres [<strong>Windows.Devices.AllJoyn</strong>](https://msdn.microsoft.com/library/windows/apps/dn894971) de Windows Runtime incorpora la implementación de los servicios y el marco de software de código abierto de AllJoyn de Microsoft. Estas API permiten que tu aplicación universal para dispositivos de Windows participe con otros dispositivos de escenarios de Internet de las cosas (IoT) controlados por AllJoyn. Para obtener más detalles sobre las API C de AllJoyn, descarga la documentación en [The AllSeen Alliance](https://allseenalliance.org/).</p>
<p>Usa la [herramienta AllJoynCodeGen](https://msdn.microsoft.com/library/windows/apps/dn913809) que se incluye en esta versión para generar un componente de Windows que puedas usar para habilitar escenarios de AllJoyn en tu aplicación de dispositivo.</p>
<div class="alert">
<strong>Nota</strong> Windows 10 IoT Core está ahora disponible para una nueva clase de dispositivos pequeños, lo que te permite crear dispositivos del "Internet de las cosas" (IoT) con Windows y Visual Studio. Obtén más información sobre Windows IoT en [WindowsOnDevices.com](http://www.windowsondevices.com/).
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left">API de impresión en móviles (XAML)</td>
<td align="left">Hay un conjunto único y unificado de API que te permite imprimir desde tus aplicaciones para UWP basadas en XAML entre las familias de dispositivos, incluidos los dispositivos móviles. Ahora puedes agregar impresión a tu aplicación móvil usando API de impresión familiares desde los espacios de nombres [<strong>Windows.Graphics.Printing</strong>](https://msdn.microsoft.com/library/windows/apps/br226489) y [<strong>Windows.UI.Xaml.Printing</strong>](https://msdn.microsoft.com/library/windows/apps/br243325).</td>
</tr>
<tr class="odd">
<td align="left">Batería</td>
<td align="left"><p>Con las API de batería del espacio de nombres [<strong>Windows.Devices.Power</strong>](https://msdn.microsoft.com/library/windows/apps/dn895017), tu aplicación puede obtener más información sobre las baterías que están conectadas al dispositivo que ejecuta la aplicación.</p>
<p>Crea un objeto [<strong>Battery</strong>](https://msdn.microsoft.com/library/windows/apps/dn895004) para representar un controlador de batería individual o un agregado de todos los controladores de la batería (según lo cree [<strong>FromIdAsync</strong>](https://msdn.microsoft.com/library/windows/apps/dn895013) o [<strong>AggregateBattery</strong>](https://msdn.microsoft.com/library/windows/apps/dn895011), respectivamente).</p>
<p>Usa el método [<strong>GetReport</strong>](https://msdn.microsoft.com/library/windows/apps/dn895015) para devolver un objeto [<strong>BatteryReport</strong>](https://msdn.microsoft.com/library/windows/apps/dn895005) que indica la carga, capacidad y estado de las baterías correspondientes.</p></td>
</tr>
<tr class="even">
<td align="left">Dispositivos MIDI</td>
<td align="left"><p>El nuevo espacio de nombres [<strong>Windows.Devices.Midi</strong>](https://msdn.microsoft.com/library/windows/apps/dn895002) te permite crear:</p>
<ul>
<li>Aplicaciones que se pueden comunicar con dispositivos MIDI externos.</li>
<li>Aplicaciones y dispositivos externos que se comunican directamente con el sintetizador de software de Microsoft GS MIDI.</li>
<li>Escenarios en los que varios clientes tienen acceso simultáneamente a un solo puerto MIDI.</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">Compatibilidad del sensor personalizado</td>
<td align="left">El espacio de nombres [<strong>Windows.Devices.Sensors.Custom</strong>](https://msdn.microsoft.com/library/windows/apps/dn895032) permite a los desarrolladores de hardware definir nuevos tipos de sensor personalizados, como un sensor de CO2.</td>
</tr>
<tr class="even">
<td align="left">Emulación de tarjeta basada en host (HCE)</td>
<td align="left"><p>La emulación de tarjeta de host le permite implementar servicios de emulación de tarjeta NFC hospedados en el sistema operativo y aún así poder comunicarse todavía con el terminal del lector externo mediante la radio NFC.</p>
<p>Implementar una tarea en segundo plano para emular una tarjeta inteligente mediante NFC. Para activar la tarea en segundo plano, usa la clase [<strong>SmartCardTrigger</strong>](https://msdn.microsoft.com/library/windows/apps/dn624853).</p>
<p>El valor <strong>EmulatorHostApplicationActivated</strong> en la enumeración [<strong>SmartCardTriggerType</strong>](https://msdn.microsoft.com/library/windows/apps/dn608017) permite a tu aplicación saber que se ha producido un evento HCE.</p></td>
</tr>
</tbody>
</table>

 

## Elementos gráficos


|                      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
|----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| DirectX              | DirectX 12 en Windows 10 incorpora la nueva versión de Microsoft Direct3D, la API de gráficos 3D en el corazón de DirectX. [Direct3D 12 Graphics (Gráficos de Direct3D 12)](https://msdn.microsoft.com/library/windows/desktop/dn903821) ofrece la eficacia y el rendimiento de una API de tipo consola de bajo nivel. Direct3D 12 es más rápido y más eficiente que nunca. Permite crear escenas más vivaces, más objetos, efectos más complejos, así como mejorar el uso del hardware gráfico moderno.                                                                                                                                                                                                                                                                                     |
| SoftwareBitmapSource | En las aplicaciones universales de Windows, puedes usar el nuevo tipo [**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/dn997854) como origen de imagen XAML. Esto te permite pasar imágenes sin codificación al marco XAML para que se muestren inmediatamente en pantalla, omitiendo la descodificación de imágenes por el marco XAML. Puedes lograr una representación de imágenes mucho más rápida, como la representación de fotos de retardo bajo directamente desde la cámara, usando descodificadores de imagen personalizados, capturando fotogramas desde superficies de DirectX, o incluso creando imágenes en memoria desde cero y representándolas todas directamente en XAML con latencia baja y baja sobrecarga de memoria.                                                                                                     |
| Cámara perspectiva   | En aplicaciones universales de Windows, XAML tiene una nueva API de Transform3D que te permite aplicar transformaciones de perspectiva a un árbol XAML (o escena), que transforma todos los elementos secundarios XAML en función de esa transformación única de toda la escena (o cámara). Podrías hacer esto anteriormente con MatrixTransform y matemáticas complejas, pero Transform3D simplifica en gran medida este efecto y también permite la animación del efecto. Para obtener más información, consulta la propiedad [**UIElement.Transform3D**](https://msdn.microsoft.com/library/windows/apps/dn906919), [**Transform3D**](https://msdn.microsoft.com/library/windows/apps/dn914748), [**CompositeTransform3D**](https://msdn.microsoft.com/library/windows/apps/dn914714) y [**PerspectiveTransform3D**](https://msdn.microsoft.com/library/windows/apps/dn914740). |

 

## Multimedia


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">HTTP Live Streaming</td>
<td align="left">Puedes usar la nueva clase [<strong>AdaptiveMediaSource</strong>](https://msdn.microsoft.com/library/windows/apps/dn946912) para agregar a tus aplicaciones capacidades adaptables de streaming de vídeo. Para inicializar el objeto, debes apuntarlo a un archivo de manifiesto de streaming. Los formatos de manifiesto compatibles incluyen Http Live Streaming (HLS) y Dynamic Adaptive Streaming over HTTP (DASH). Una vez el objeto está enlazado a un elemento multimedia XAML, se inicia la reproducción adaptable. Las propiedades del flujo como, por ejemplo, la velocidad de bits disponible, mínima y máxima, se pueden consultar y establecer según sea necesario.</td>
</tr>
<tr class="even">
<td align="left">Compatibilidad del procesador Transcode Video Processor (XVP) de Media Foundation con las Transformaciones de Media Foundation (MFT)</td>
<td align="left"><p>Las aplicaciones de Windows que usan Transformaciones de Media Foundation (MFT) ahora pueden usar el procesador <strong>Transcode Video Processor (XVP) de Media Foundation</strong> para convertir, escalar y transformar datos de vídeo sin procesar:</p>
<p>El nuevo atributo [MF_XVP_CALLER_ALLOCATES_OUTPUT](https://msdn.microsoft.com/library/windows/desktop/dn803919) permite la salida a texturas asignadas por el llamador incluso en el modo Microsoft DirectX Video Acceleration (DXVA)</p>
<p>La nueva interfaz [<strong>IMFVideoProcessorControl2</strong>](https://msdn.microsoft.com/library/windows/desktop/dn800741) permite a la aplicación habilitar efectos de hardware, consultar efectos de hardware admitidos y omitir la operación de rotación realizada por el procesador de vídeo.</p></td>
</tr>
<tr class="odd">
<td align="left">Transcodificación</td>
<td align="left">La nueva API [<strong>MediaProcessingTrigger</strong>](https://msdn.microsoft.com/library/windows/apps/dn806005) permite que la aplicación realice la transcodificación multimedia en una tarea en segundo plano, de modo que las operaciones de transcodificación continúen incluso cuando la aplicación en primer plano haya terminado.</td>
</tr>
<tr class="even">
<td align="left">Eventos de error de medios de MediaElement</td>
<td align="left">En las aplicaciones universales de Windows, [<strong>MediaElement</strong>](https://msdn.microsoft.com/library/windows/apps/br242926) reproducirá contenido que abarque varios flujos, incluso si hay un error en la descodificación de uno de ellos, siempre y cuando el contenido multimedia incluya al menos un flujo válido. Por ejemplo, si se produce un error en la secuencia de vídeo en un contenido con secuencia de vídeo y audio, <strong>MediaElement</strong> seguirá reproduciendo la secuencia de audio. El evento [<strong>PartialMediaFailureDetected</strong>](https://msdn.microsoft.com/library/windows/apps/dn889635) le notifica que una de las secuencias dentro de una secuencia no se pudo descodificar. También permite saber qué tipo de secuencia dio error para poder reflejar esa información en la interfaz de usuario. Si todas las secuencias dentro de una secuencia de medios falla, se genera el evento [<strong>MediaFailed</strong>](https://msdn.microsoft.com/library/windows/apps/br227393).</td>
</tr>
<tr class="odd">
<td align="left">Compatibilidad con la emisión de vídeo adaptativa con MediaElement</td>
<td align="left">[<strong>MediaElement</strong>](https://msdn.microsoft.com/library/windows/apps/br242926) tiene el nuevo método [<strong>SetPlaybackSource</strong>](https://msdn.microsoft.com/library/windows/apps/dn899085) para admitir la transmisión por secuencias de vídeo adaptativa. Usa este método para establecer como origen multimedia [<strong>AdaptiveMediaSource</strong>](https://msdn.microsoft.com/library/windows/apps/dn946912).</td>
</tr>
<tr class="even">
<td align="left">Conversión con MediaElement e Image</td>
<td align="left">Los controles [<strong>MediaElement</strong>](https://msdn.microsoft.com/library/windows/apps/br242926) y [<strong>Image</strong>](https://msdn.microsoft.com/library/windows/apps/br242752) tienen el nuevo método [<strong>GetAsCastingSource</strong>](https://msdn.microsoft.com/library/windows/apps/dn920012). Puedes usar este método para enviar contenido mediante programación desde cualquier elemento de imagen o multimedia a una gama más amplia de dispositivos remotos, como Miracast, Bluetooth y DLNA.Esta funcionalidad se habilita automáticamente cuando se establece [<strong>AreTransportControlsEnabled</strong>](https://msdn.microsoft.com/library/windows/apps/dn298977) en true en un <strong>MediaElement</strong>.</td>
</tr>
<tr class="odd">
<td align="left">Controles de transporte multimedia para aplicaciones de escritorio</td>
<td align="left">La interfaz [<strong>ISystemMediaTransportControls</strong>](https://msdn.microsoft.com/library/windows/desktop/dn892299) y las API relacionadas permiten a las aplicaciones de escritorio interactuar con los controles de transporte del sistema multimedia integrado. Esto incluye responder a las interacciones del usuario con los botones de los controles de transporte y actualizar la visualización de los controles de transporte para mostrar metadatos sobre el contenido multimedia que se está reproduciendo actualmente.</td>
</tr>
<tr class="even">
<td align="left">Codificación y descodificación de JPEG de acceso aleatorio</td>
<td align="left">Lo nuevos métodos WIC [<strong>IWICJpegFrameEncode</strong>](https://msdn.microsoft.com/library/windows/desktop/dn903864) y [<strong>IWICJpegFrameDecode</strong>](https://msdn.microsoft.com/library/windows/desktop/dn903834) permiten la codificación y descodificación de imágenes JPEG. Ahora también puedes habilitar la indización de los datos de imagen, que proporciona acceso aleatorio eficaz para imágenes grandes a costa de una superficie de memoria mayor.</td>
</tr>
<tr class="odd">
<td align="left">Superposiciones para composiciones multimedia</td>
<td align="left">Las nuevas API [<strong>MediaOverlay</strong>](https://msdn.microsoft.com/library/windows/apps/dn764793) y [<strong>MediaOverlayLayer</strong>](https://msdn.microsoft.com/library/windows/apps/dn764795) hacen muy sencillo agregar varias capas de contenido multimedia estático o dinámico a una composición multimedia. Se puede ajustar la opacidad, la posición y el tiempo para cada capa e incluso puede implementar su propio compositor personalizado para las capas de entrada.</td>
</tr>
<tr class="even">
<td align="left">Nuevo marco de efectos</td>
<td align="left">El espacio de nombres [<strong>Windows.Media.Effects</strong>](https://msdn.microsoft.com/library/windows/apps/dn278802) proporciona un marco sencillo e intuitivo para agregar efectos en secuencias de audio y vídeos. El marco incluye interfaces básicas que se pueden implementar para crear efectos de vídeo y audio personalizados e insertarlos en la canalización multimedia.</td>
</tr>
</tbody>
</table>

 

## Redes


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Sockets</td>
<td align="left"><p>Entre las actualizaciones de sockets se incluyen:</p>
<ul>
<li><strong>Agente de socket.</strong> El agente de socket puede establecer y cerrar las conexiones de socket en nombre de una aplicación en cualquier estado del ciclo de vida de la aplicación. Esto permite detectar más fácilmente las aplicaciones y los servicios estas que proporcionan. Por ejemplo, mediante el agente de socket, un servicio de Win32 puede aceptar conexiones entrantes de socket aunque no se esté ejecutando.</li>
<li><strong>Mejoras de rendimiento.</strong> El rendimiento de socket se ha optimizado para las aplicaciones que usan el espacio de nombres [<strong>Windows.Networking.Sockets</strong>](https://msdn.microsoft.com/library/windows/apps/br226960).</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">Tareas de posprocesamiento de la transferencia en segundo plano</td>
<td align="left">Nuevas API en el espacio de nombres [<strong>Windows.Networking.BackgroundTransfer</strong>](https://msdn.microsoft.com/library/windows/apps/br207242) te permiten registrar grupos de tareas de posprocesamiento. La aplicación puede actuar inmediatamente tanto si las transferencias en segundo plano se realizan de forma correcta o incorrecta y aunque no esté en primer plano, en lugar de esperar a que el usuario reanude la aplicación la próxima vez.</td>
</tr>
<tr class="odd">
<td align="left">Compatibilidad con Bluetooth para los anuncios</td>
<td align="left">Con el espacio de nombres [<strong>Windows.Devices.Bluetooth.Advertisement</strong>](https://msdn.microsoft.com/library/windows/apps/dn894325), tus aplicaciones pueden enviar, recibir y filtrar los anuncios de Bluetooth LE.</td>
</tr>
<tr class="even">
<td align="left">Actualización de la API de Wi-Fi Direct</td>
<td align="left"><p>El agente del dispositivo se actualiza para permitir el emparejamiento con dispositivos sin tener que dejar la aplicación. Las adiciones al espacio de nombres [<strong>Windows.Devices.WiFiDirect</strong>](https://msdn.microsoft.com/library/windows/apps/dn297687) también permiten que otros dispositivos puedan detectar un dispositivo y que este escuche las notificaciones de conexión entrante.</p>
<div class="alert">
<strong>Nota</strong> En esta versión, las mejoras de la función Wi-Fi Direct no se integran en la experiencia del usuario y solo admiten el emparejamiento con el botón de comando. Asimismo, esta versión solo admite una conexión activa.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left">Mejoras de compatibilidad con JSON</td>
<td align="left">El espacio de nombres [<strong>Windows.Data.Json</strong>](https://msdn.microsoft.com/library/windows/apps/br240639) ahora ofrece mayor compatibilidad con las definiciones estándar existentes y la experiencia del desarrollador al convertir los objetos JSON durante las sesiones de depuración.</td>
</tr>
</tbody>
</table>

 

## Seguridad


|                             |                                                                      |
|-----------------------------|----------------------------------------------------------------------|
| Cifrado ECC              | Las nuevas API del espacio de nombres [**Windows.Security.Cryptography**](https://msdn.microsoft.com/library/windows/apps/br241404) ofrecen compatibilidad con la criptografía de curva elíptica (ECC), una implementación de criptografía de clave pública basada en curvas elípticas sobre campos limitados. ECC es matemáticamente más complejo que RSA, proporciona tamaños más pequeños de claves, reduce el consumo de la memoria y mejora el rendimiento. Esto ofrece una alternativa a las claves de RSA y a los parámetros de la curva aprobados por NIST para los servicios y los clientes de Microsoft. |
| Microsoft Passport          | Microsoft Passport es un método alternativo de autenticación que reemplaza las contraseñas con criptografía asimétrica y un gesto. Las clases del espacio de nombres [**Credentials**](https://msdn.microsoft.com/library/windows/apps/br227089), como [**KeyCredentialManager**](https://msdn.microsoft.com/library/windows/apps/dn973043), facilitan a los desarrolladores la creación de aplicaciones con Microsoft Passport sin la complejidad de la criptografía o la biométrica.  |
| Microsoft Passport para el trabajo | Microsoft Passport para el trabajo es un método alternativo para iniciar sesión en Windows con tu cuenta de Azure Active Directory que no use contraseñas, tarjetas inteligentes y tarjetas inteligentes virtuales. Puede optar por deshabilitar o habilitar esta configuración de directiva. |
| Agente de token                | El Agente de token es un nuevo marco de autenticación que facilita que las aplicaciones se conecten a los proveedores de identidad en línea (como Facebook). Características como la administración de nombres de usuario y contraseñas de cuenta y una interfaz de usuario optimizada proporcionan una experiencia de autenticación mejorada para los usuarios. |

 

## Servicios del sistema


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Energía</td>
<td align="left"><p>La aplicación de escritorio de Windows ahora puede recibir notificaciones cuando se active o desactive el ahorro de batería. Al responder a cambios en las condiciones de energía, la aplicación tiene la oportunidad de ayudar a prolongar la duración de la batería.</p>
<p>[<strong>GUID_POWER_SAVING_STATUS</strong>](https://msdn.microsoft.com/library/windows/desktop/hh448380): usa este nuevo GUID con la función [<strong>PowerSettingRegisterNotification</strong>](https://msdn.microsoft.com/library/windows/desktop/hh769082) para que se te notifique cuando el ahorro de batería esté activado o desactivado.</p>
<p>[<strong>SYSTEM_POWER_STATUS</strong>](https://msdn.microsoft.com/library/windows/desktop/aa373232): esta estructura se ha actualizado para admitir el ahorro de batería. El cuarto miembro, <strong>SystemStatusFlag</strong> (denominado anteriormente Reserved1), indica si el ahorro de batería está activado o no. Usa la función [<strong>GetSystemPowerStatus</strong>](https://msdn.microsoft.com/library/windows/desktop/aa372693) para recuperar un puntero a esta estructura.</p></td>
</tr>
<tr class="even">
<td align="left">Versión</td>
<td align="left"><p>Puedes usar las [funciones de la aplicación auxiliar de versiones](https://msdn.microsoft.com/library/windows/desktop/dn424972) para determinar la versión del sistema operativo. Para Windows 10, estas funciones auxiliares incluyen una nueva función, [<strong>IsWindows10OrGreater</strong>](https://msdn.microsoft.com/library/windows/desktop/dn905474). Debes usar las funciones auxiliares en lugar de las funciones en desuso [<strong>GetVersionEx</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724451) y [<strong>GetVersion</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724439) para determinar la versión del sistema. Para obtener más información acerca de cómo obtener la versión del sistema, consulta [Obtener la versión del sistema](https://msdn.microsoft.com/library/windows/desktop/ms724429).</p>
<p>Si usas las funciones en desuso [<strong>GetVersionEx</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724451) o [<strong>GetVersion</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724439) para obtener información de la versión en una estructura [<strong>OSVERSIONINFOEX</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724833) o [<strong>OSVERSIONINFO</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724834), ten en cuenta que el número de versión que estas estructuras contienen aumenta de 6.3 para Windows 8.1 y Windows Server 2012 R2 a 10.0 para Windows 10. Para obtener más información sobre los números de versión del sistema operativo, consulta [Versión del sistema operativo](https://msdn.microsoft.com/library/windows/desktop/ms724832).</p>
<p>También debes seleccionar específicamente Windows 8.1 o Windows 10 como destino de la aplicación para obtener la información de versión correcta de estas versiones con las funciones [<strong>GetVersionEx</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724451) o [<strong>GetVersion</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724439). Para obtener información acerca de cómo seleccionar tu aplicación como destino para estas versiones de Windows, consulta [Targeting your application for Windows (Orientar tu aplicación a Windows)](https://msdn.microsoft.com/library/windows/desktop/dn481241).</p></td>
</tr>
<tr class="odd">
<td align="left">Información de usuario</td>
<td align="left">Con las nuevas API en el espacio de nombres [<strong>Windows.System</strong>](https://msdn.microsoft.com/library/windows/apps/br241814) es muy fácil obtener acceso a información sobre un usuario, como su nombre de usuario o la imagen de cuenta. También proporciona la capacidad de responder a eventos de usuario, como el inicio y el cierre de sesión.</td>
</tr>
<tr class="even">
<td align="left">Administración de memoria y creación de perfiles</td>
<td align="left">La compatibilidad con la API de generación de perfiles de memoria de [<strong>Windows.System</strong>](https://msdn.microsoft.com/library/windows/apps/br241814) se ha ampliado a todas las plataformas y su funcionalidad general se ha mejorado con nuevas clases y funciones.</td>
</tr>
</tbody>
</table>

 

## Almacenamiento


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">API de búsqueda de archivos para Windows Phone</td>
<td align="left"><p>Como editor de aplicaciones, puedes registrar la aplicación para que comparta una carpeta del almacenamiento con otras aplicaciones que publiques agregando extensiones al manifiesto de la aplicación. A continuación, llama al método [<strong>Windows.Storage.ApplicationData.GetPublisherCacheFolder</strong>](https://msdn.microsoft.com/library/windows/apps/dn889607) para obtener la ubicación de almacenamiento compartido.</p>
<p>El modelo de seguridad sólida de las aplicaciones de Windows Runtime evita que las aplicaciones compartan datos entre ellas. No obstante, esto puede resultar útil para que las aplicaciones del mismo editor compartan archivos y valores de configuración de forma individual para cada usuario.</p></td>
</tr>
</tbody>
</table>

 

## Herramientas


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Árbol visual dinámico en Visual Studio</td>
<td align="left">Visual Studio tiene una nueva función de árbol visual dinámico. Puedes usarla durante la depuración para comprender con rapidez el estado del árbol visual de la aplicación y descubrir cómo se establecieron las propiedades del elemento. También te permite cambiar los valores de propiedad mientras se ejecuta la aplicación, para que puedas retocar y experimentar sin tener que reiniciar.</td>
</tr>
<tr class="even">
<td align="left">Registro de seguimiento</td>
<td align="left"><p>[TraceLogging](https://msdn.microsoft.com/library/windows/desktop/dn904636) es una nueva API de seguimiento de eventos para las aplicaciones de modo usuario y los controladores de modo kernel; se basa en el [Seguimiento de eventos para Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803) (ETW). Esta API ofrece una manera simplificada de instrumentar código e incluir datos estructurados con eventos sin necesitar un archivo XML de manifiesto de instrumentación independiente.</p>
<p>Las API TraceLogging de WinRT, .NET y C/C++ están disponibles para los distintos tipos de desarrolladores.</p></td>
</tr>
</tbody>
</table>

 

## Experiencia del usuario


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Reconocimiento de voz</td>
<td align="left">El reconocimiento de voz continuo para escenarios de dictado de formato largo ahora es compatible con la Plataforma universal de Windows. Consulta cómo habilitar el dictado continuo en los Documentos de interacción de voz.</td>
</tr>
<tr class="even">
<td align="left">Capacidades de arrastrar y colocar entre distintas plataformas de aplicaciones</td>
<td align="left"><p>Los nuevos espacios de nombres [<strong>Windows.ApplicationModel.DataTransfer.DragDrop</strong>](https://msdn.microsoft.com/library/windows/apps/dn894216) llevan la funcionalidad de arrastrar y colocar a las aplicaciones universales de Windows. Los escenarios comunes de arrastrar y colocar que anteriormente se permitían en los programas de escritorio (por ejemplo, arrastrar un documento de una carpeta a un mensaje de correo de Outlook para adjuntarlo) no eran posibles con las aplicaciones universales de Windows. Con estas nuevas API, la aplicación puede permitir que los usuarios muevan datos fácilmente entre diferentes aplicaciones universales de Windows y el escritorio.</p>
<p>Para admitir la acción de arrastrar y colocar entre aplicaciones, se han agregado las siguientes API nuevas a XAML:</p>
<ul>
<li>[<strong>ListViewBase.DragItemsCompleted</strong>](https://msdn.microsoft.com/library/windows/apps/dn831959)</li>
<li>[<strong>UIElement</strong>](https://msdn.microsoft.com/library/windows/apps/br208911): [<strong>CanDrag</strong>](https://msdn.microsoft.com/library/windows/apps/dn903558), [<strong>DragStarting</strong>](https://msdn.microsoft.com/library/windows/apps/dn903560), [<strong>StartDragAsync</strong>](https://msdn.microsoft.com/library/windows/apps/dn903562), [<strong>DropCompleted</strong>](https://msdn.microsoft.com/library/windows/apps/dn903561)</li>
<li>[<strong>DragOperationDeferral</strong>](https://msdn.microsoft.com/library/windows/apps/dn831917), [<strong>DragUI</strong>](https://msdn.microsoft.com/library/windows/apps/dn985714), [<strong>DragUIOverride</strong>](https://msdn.microsoft.com/library/windows/apps/dn985715)</li>
<li>[<strong>DragEventArgs</strong>](https://msdn.microsoft.com/library/windows/apps/br242372): [<strong>AcceptedOperation</strong>](https://msdn.microsoft.com/library/windows/apps/dn831912), [<strong>DataView</strong>](https://msdn.microsoft.com/library/windows/apps/dn831913), [<strong>DragUIOverride</strong>](https://msdn.microsoft.com/library/windows/apps/dn985710), [<strong>GetDeferral</strong>](https://msdn.microsoft.com/library/windows/apps/dn831914), [<strong>Modifiers</strong>](https://msdn.microsoft.com/library/windows/apps/dn831915)</li>
<li>[<strong>DragItemsCompletedEventArgs</strong>](https://msdn.microsoft.com/library/windows/apps/dn831953), [<strong>DropCompletedEventArgs</strong>](https://msdn.microsoft.com/library/windows/apps/dn903549), [<strong>DragStartingEventArgs</strong>](https://msdn.microsoft.com/library/windows/apps/dn903540)</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">Barras de título de ventanas personalizadas</td>
<td align="left">En las aplicaciones para UWP para la familia de dispositivos de escritorio, ahora puedes usar la clase [<strong>ApplicationViewTitleBar</strong>](https://msdn.microsoft.com/library/windows/apps/dn906115) con la propiedad [<strong>ApplicationView.TitleBar</strong>](https://msdn.microsoft.com/library/windows/apps/dn906131) y el método [<strong>Window.SetTitleBar</strong>](https://msdn.microsoft.com/library/windows/apps/dn965560) para reemplazar el contenido predeterminado de la barra de título de Windows por tu propio contenido XAML personalizado. El código XAML se trata como &quot;cromo del sistema&quot;, por lo que Windows administrará los eventos de entrada en lugar de la aplicación. Esto significa que el usuario puede seguir arrastrando y cambiando el tamaño de la ventana, incluso al hacer clic en el contenido de la barra de título personalizado.</td>
</tr>
</tbody>
</table>

 

## Web


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Internet Explorer</td>
<td align="left"><p>Internet Explorer presenta el modo Edge: un nuevo modo de documento &quot;vivo&quot; diseñado para lograr una máxima interoperabilidad con otros exploradores modernos y contenidos web contemporáneos. Este modo experimental se está distribuyendo de forma progresiva a un conjunto de usuarios de Windows 10 elegido de forma aleatoria. Puedes habilitar o inhabilitar manualmente el modo de borde mediante el nuevo mecanismo <strong>about:flags</strong> de IE. Para obtener más información, consulta:</p>
<ul>
<li>[Living on the Edge – our next step in helping the web just work (Vivir al límite: nuestro próximo paso para contribuir al funcionamiento de la Web)](http://blogs.msdn.com/b/ie/archive/2014/11/11/living-on-the-edge-our-next-step-in-interoperability.aspx)</li>
<li>[Guía para desarrolladores de Internet Explorer para Windows 10](https://dev.windows.com/microsoft-edge/)</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">Exploración del modo Edge de WebView</td>
<td align="left">El control [<strong>WebView</strong>](https://msdn.microsoft.com/library/windows/apps/br227702) usa el mismo motor de representación que el nuevo explorador Edge. Esto ofrece el modo más preciso y conforme a los estándares de la representación HTML.</td>
</tr>
<tr class="odd">
<td align="left">WebView fuera de subproceso</td>
<td align="left">Puedes especificar [<strong>WebView.ExecutionMode</strong>](https://msdn.microsoft.com/library/windows/apps/dn932034) para habilitar el procesamiento y la presentación de contenido web en un subproceso independiente en segundo plano. Esto puede mejorar el rendimiento en determinados escenarios específicos.</td>
</tr>
<tr class="even">
<td align="left">Evento WebView.UnsupportedUriSchemeIdentified</td>
<td align="left"><p>El nuevo evento [<strong>WebView.UnsupportedUriSchemeIdentified</strong>](https://msdn.microsoft.com/library/windows/apps/dn974400) te permite decidir cómo debe tratar tu aplicación un esquema de URI no admitido. Puedes controlar este evento para que la aplicación ofrezca la administración personalizada de los esquemas URI no admitidos.</p>
<p>Para el control HTML WebView, consulta el evento [<strong>MSWebViewUnsupportedUriSchemeIdentified</strong>](https://msdn.microsoft.com/library/windows/apps/dn803906.aspx).</p></td>
</tr>
<tr class="odd">
<td align="left">Evento WebView.NewWindowRequested</td>
<td align="left"><p>El nuevo evento [<strong>WebView.NewWindowRequested</strong>](https://msdn.microsoft.com/library/windows/apps/dn974397) te permite responder cuando un script en un control de WebView solicita una nueva ventana del explorador.</p>
<p>Para el control HTML WebView, consulta el evento [<strong>MSWebViewNewWindowRequested</strong>](https://msdn.microsoft.com/library/windows/apps/dn806030).</p></td>
</tr>
<tr class="even">
<td align="left">Evento WebView.PermissionRequested</td>
<td align="left"><p>El nuevo evento [<strong>WebView.PermissionRequested</strong>](https://msdn.microsoft.com/library/windows/apps/dn974398) permite que el contenido de WebView aproveche las nuevas y potentes API de HTML5 que requieren permiso especial del usuario, como la geolocalización.</p>
<p>Para el control HTML WebView, consulta el evento [<strong>MSWebViewPermissionRequested</strong>](https://msdn.microsoft.com/library/windows/apps/dn806030.aspx).</p></td>
</tr>
<tr class="odd">
<td align="left">Evento WebView.UnviewableContentIdentified</td>
<td align="left"><p>El nuevo evento [<strong>WebView.UnviewableContentIdentified</strong>](https://msdn.microsoft.com/library/windows/apps/dn299351) te permite responder cuando te desplazas de [<strong>WebView</strong>](https://msdn.microsoft.com/library/windows/apps/br227702) a contenido no de web, como un archivo PDF o un documento de Office.</p>
<p>Para los controles HTML WebView, consulta el evento [<strong>MSWebViewUnviewableContentIdentified</strong>](https://msdn.microsoft.com/library/windows/apps/dn609716).</p></td>
</tr>
<tr class="even">
<td align="left">Método WebView.AddWebAllowedObject</td>
<td align="left"><p>Puedes llamar al nuevo método [<strong>WebView.AddWebAllowedObject</strong>](https://msdn.microsoft.com/library/windows/apps/dn903993) para insertar un objeto de WinRT en un [<strong>WebView</strong>](https://msdn.microsoft.com/library/windows/apps/br227702) XAML y después llamar a sus funciones desde un JavaScript de confianza hospedado en dicho <strong>WebView</strong>. Por ejemplo, el contenido web puede mostrar notificaciones del sistema solicitando que su aplicación principal realice una llamada a la API [<strong>ToastNotificationManager</strong>](https://msdn.microsoft.com/library/windows/apps/br208642) de WinRT.</p>
<p>Para el control HTML [<strong>WebView</strong>](https://msdn.microsoft.com/library/windows/apps/br227702), consulta el método [<strong>addWebAllowedObject</strong>](https://msdn.microsoft.com/library/windows/apps/dn926632).</p></td>
</tr>
<tr class="odd">
<td align="left">Método WebView.ClearTemporaryWebDataAync</td>
<td align="left">Cuando un usuario interactúa con el contenido web dentro de un [<strong>WebView</strong>](https://msdn.microsoft.com/library/windows/apps/br227702) XAML, el control <strong>WebView</strong> almacena en caché los datos basados en la sesión del usuario. Puedes llamar al nuevo método [<strong>ClearTemporaryWebDataAsync</strong>](https://msdn.microsoft.com/library/windows/apps/dn974394) para borrar esta caché. Por ejemplo, puedes borrar la memoria caché cuando un usuario cierre sesión en la aplicación para que otro usuario no pueda obtener acceso a los datos desde la sesión anterior.</td>
</tr>
</tbody>
</table>



<!--HONumber=May16_HO2-->


