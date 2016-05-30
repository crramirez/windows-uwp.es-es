---
author: Jwmsft
ms.assetid: 54CC0BD4-1961-44D7-AB40-6E8B58E42D65
title: Dibujar formas
description: Aprende a dibujar formas como elipses, rectángulos, polígonos y trayectorias. La clase Path te permite visualizar un lenguaje de dibujo basado en vectores, relativamente complejo, en una interfaz de usuario XAML; por ejemplo, lo puedes visualizar para dibujar curvas Bézier.
---
# Dibujar formas

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


** API importantes **

-   [**Ruta de acceso**](https://msdn.microsoft.com/library/windows/apps/BR243355)
-   [**Espacio de nombres Windows.UI.Xaml.Shapes**](https://msdn.microsoft.com/library/windows/apps/BR243401)
-   [**Espacio de nombres Windows.UI.Xaml.Media**](https://msdn.microsoft.com/library/windows/apps/BR243045)

Aprende a dibujar formas como elipses, rectángulos, polígonos y trayectorias. La clase [**Path**](https://msdn.microsoft.com/library/windows/apps/BR243355) te permite visualizar un lenguaje de dibujo basado en vectores, relativamente complejo, en una interfaz de usuario XAML; por ejemplo, lo puedes visualizar para dibujar curvas Bézier.

## Introducción

Son dos los conjuntos de clases que definen una región del espacio en la interfaz de usuario XAML: las clases [**Shape**](https://msdn.microsoft.com/library/windows/apps/BR243377) y las clases [**Geometry**](https://msdn.microsoft.com/library/windows/apps/BR210041). La principal diferencia entre estas clases es que **Shape** tiene asociado un pincel y se puede representar en la pantalla, mientras que **Geometry** simplemente define una región del espacio y no se representa, salvo que ayude a aportar información a otra propiedad de interfaz de usuario. Puedes pensar en **Shape** como una clase [**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911), la cual tiene su límite definido por **Geometry**. Este tema se centra principalmente en las clases **Shape**.

Las clases [**Shape**](https://msdn.microsoft.com/library/windows/apps/BR243377) son: [**Line**](https://msdn.microsoft.com/library/windows/apps/BR243345), [**Ellipse**](https://msdn.microsoft.com/library/windows/apps/BR243343), [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371), [**Polygon**](https://msdn.microsoft.com/library/windows/apps/BR243359), [**Polyline**](https://msdn.microsoft.com/library/windows/apps/BR243365) y [**Path**](https://msdn.microsoft.com/library/windows/apps/BR243355). La clase **Path** es interesante porque puede definir una geometría arbitraria; asimismo, la clase [**Geometry**](https://msdn.microsoft.com/library/windows/apps/BR210041) también tiene su función aquí, dado que es una forma de definir las partes de una clase **Path**.

## Relleno y trazo para formas

Para representar una clase [**Shape**](https://msdn.microsoft.com/library/windows/apps/BR243377) en el lienzo de la aplicación, debes asociarla a una clase [**Brush**](https://msdn.microsoft.com/library/windows/apps/BR228076). Establece la propiedad [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill) de la clase **Shape** en la clase **Brush** que quieras. Para obtener más información sobre los pinceles, consulta [Usar pinceles](using-brushes.md).

Igualmente, una clase [**Shape**](https://msdn.microsoft.com/library/windows/apps/BR243377) también puede tener una propiedad [**Stroke**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.stroke), que es una línea dibujada en torno al perímetro de la forma. Además, la propiedad **Stroke** no solo necesita tener una clase [**Brush**](https://msdn.microsoft.com/library/windows/apps/BR228076) que defina su apariencia, también debe tener un valor distinto de cero para la propiedad [**StrokeThickness**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.strokethickness). **StrokeThickness** es una propiedad que define el grosor del perímetro en torno al borde de la forma. Si no especificas un valor **Brush** de la propiedad **Stroke** o si estableces **StrokeThickness** en 0, no se dibujará el borde alrededor de la forma.

## Elipse

Una [**Elipse**](https://msdn.microsoft.com/library/windows/apps/BR243343) es una forma con un perímetro curvo. Para crear una **elipse** básica, especifica las propiedades [**Width**](https://msdn.microsoft.com/library/windows/apps/BR208751), [**Height**](https://msdn.microsoft.com/library/windows/apps/BR208718) y el objeto [**Brush**](https://msdn.microsoft.com/library/windows/apps/BR228076) de la propiedad [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill).

En el siguiente ejemplo crearemos una [**elipse**](https://msdn.microsoft.com/library/windows/apps/BR243343) cuya propiedad [**Width**](https://msdn.microsoft.com/library/windows/apps/BR208751) tenga un valor de 200, la propiedad [**Height**](https://msdn.microsoft.com/library/windows/apps/BR208718) tenga un valor de 200 y que use una clase [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) con un valor de color [**SteelBlue**](https://msdn.microsoft.com/library/windows/apps/Hh748056) en la propiedad [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill).

```xml
<Ellipse Fill="SteelBlue" Height="200" Width="200" />
```

Esta es la [**elipse**](https://msdn.microsoft.com/library/windows/apps/BR243343) representada.

![Elipse representada.](images/shapes-ellipse.jpg)

En este caso, la [**elipse**](https://msdn.microsoft.com/library/windows/apps/BR243343) parece un círculo, pero así es como se declara un círculo en XAML: debes usar una **elipse** que tenga los mismos valores en las propiedades [**Width**](https://msdn.microsoft.com/library/windows/apps/BR208751) y [**Height**](https://msdn.microsoft.com/library/windows/apps/BR208718).

Cuando se coloca una [**elipse**](https://msdn.microsoft.com/library/windows/apps/BR243343) en un diseño de interfaz de usuario, se supone que su tamaño es el mismo que el de un rectángulo que tenga los mismos valores en sus propiedades [**Width**](https://msdn.microsoft.com/library/windows/apps/BR208751) y [**Height**](https://msdn.microsoft.com/library/windows/apps/BR208718); el área que quede fuera del perímetro no tiene representación, pero sigue formando parte del tamaño de diseño asignado.

Un grupo de seis elementos [**Ellipse**](https://msdn.microsoft.com/library/windows/apps/BR243343) forman parte de la plantilla de control para el control [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/BR227538) y dos elementos **Ellipse** concéntricos forman parte de un elemento [**RadioButton**](https://msdn.microsoft.com/library/windows/apps/BR227544).

## <span id="Rectangle"></span><span id="rectangle"></span><span id="RECTANGLE"></span>Rectángulo

Un [**Rectángulo**](https://msdn.microsoft.com/library/windows/apps/BR243371) es una forma de cuatro lados cuyos lados opuestos son iguales. Para crear un **rectángulo** básico, debes especificar las propiedades [**Width**](https://msdn.microsoft.com/library/windows/apps/BR208751), [**Height**](https://msdn.microsoft.com/library/windows/apps/BR208718) y [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill).

Puedes redondear las esquinas de un [**rectángulo**](https://msdn.microsoft.com/library/windows/apps/BR243371). Para ello, especifica un valor para las propiedades [**RadiusX**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.shapes.rectangle.radiusx.aspx) y [**RadiusY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.rectangle.radiusy). Estas propiedades especifican los ejes x e y de una elipse que define la curva de las esquinas. El valor máximo permitido de **RadiusX** es el valor de la propiedad [**Width**](https://msdn.microsoft.com/library/windows/apps/BR208751) dividido entre dos; asimismo, el valor máximo permitido de **RadiusY** es el valor de la propiedad [**Height**](https://msdn.microsoft.com/library/windows/apps/BR208718) dividido entre dos.

En el ejemplo siguiente, crearemos un [**rectángulo**](https://msdn.microsoft.com/library/windows/apps/BR243371) cuya propiedad [**Width**](https://msdn.microsoft.com/library/windows/apps/BR208751) tenga un valor de 200 y la propiedad [**Height**](https://msdn.microsoft.com/library/windows/apps/BR208718) un valor de 100. Asimismo, usa la clase [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) con un valor de color [**Blue**](https://msdn.microsoft.com/library/windows/apps/Hh747837) en la propiedad [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill) y otra clase **SolidColorBrush** con valor de color [**Black**](https://msdn.microsoft.com/library/windows/apps/Hh747833) en la propiedad [**Stroke**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.stroke). A continuación, establecemos el valor de la propiedad [**StrokeThickness**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.strokethickness) en 3. Igualmente, establecemos la propiedad [**RadiusX**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.shapes.rectangle.radiusx.aspx) en 50 y la propiedad [**RadiusY**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.rectangle.radiusy) en 10 para dar al **rectángulo** las esquinas redondeadas.

```xml
<Rectangle Fill="Blue"
           Width="200"
           Height="100"
           Stroke="Black"
           StrokeThickness="3"
           RadiusX="50"
           RadiusY="10" />
           ```

Here's the rendered [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371).

![A rendered Rectangle.](images/shapes-rectangle.jpg)

**Tip**  There are some scenarios for UI definitions where instead of using a [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371), a [**Border**](https://msdn.microsoft.com/library/windows/apps/BR209250) might be more appropriate. If your intention is to create a rectangle shape around other content, it might be better to use **Border** because it can have child content and will automatically size around that content, rather than using the fixed dimensions for height and width like **Rectangle** does. A **Border** also has the option of having rounded corners if you set the [**CornerRadius**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.border.cornerradius) property.

 

On the other hand, a [**Rectangle**](https://msdn.microsoft.com/library/windows/apps/BR243371) is probably a better choice for control composition. A **Rectangle** shape is seen in many control templates because it's used as a "FocusVisual" part for focusable controls. Whenever the control is in a "Focused" visual state, this rectangle is made visible, in other states it's hidden.

## Polygon

A [**Polygon**](https://msdn.microsoft.com/library/windows/apps/BR243359) is a shape with a boundary defined by an arbitrary number of points. The boundary is created by connecting a line from one point to the next, with the last point connected to the first point. The [**Points**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.shapes.polygon.points.aspx) property defines the collection of points that make up the boundary. In XAML, you define the points with a comma-separated list. In code-behind you use a [**PointCollection**](https://msdn.microsoft.com/library/windows/apps/BR210220) to define the points and you add each individual point as a [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) value to the collection.

You don't need to explicitly declare the points such that the start point and end point are both specified as the same [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) value. The rendering logic for a [**Polygon**](https://msdn.microsoft.com/library/windows/apps/BR243359) assumes that you are defining a closed shape and will connect the end point to the start point implicitly.

The next example creates a [**Polygon**](https://msdn.microsoft.com/library/windows/apps/BR243359) with 4 points set to `(10,200)`, `(60,140)`, `(130,140)`, and `(180,200)`. It uses a [**LightBlue**](https://msdn.microsoft.com/library/windows/apps/Hh747960) value of [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) for its [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill), and has no value for [**Stroke**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.stroke) so it has no perimeter outline.

```xml
<Polygon Fill="LightBlue"
         Points="10,200,60,140,130,140,180,200" />
```

Y aquí tienes el [**polígono**](https://msdn.microsoft.com/library/windows/apps/BR243359) representado.

![Polígono representado.](images/shapes-polygon.jpg)

**Sugerencia**  El valor [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) se usa a menudo como tipo en XAML para escenarios en los que no se declaran los vértices de las formas. Por ejemplo, un valor **Point** forma parte de los datos del evento referentes a los eventos de entrada táctil; de esta manera, podrás saber con exactitud en qué punto de un espacio de coordenadas se produjo la acción táctil. Para obtener más información sobre el valor **Point** y cómo usarlo en XAML o en código, consulta el tema de referencia de la API para [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870).

 

## Línea

Una [**Línea**](https://msdn.microsoft.com/library/windows/apps/BR243345) es simplemente una línea dibujada entre dos puntos en un espacio de coordenadas. Una **línea** omite cualquier valor proporcionado para [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill), ya que no tiene espacio interior. Si vas a realizar una **línea**, asegúrate de especificar los valores de las propiedades [**Stroke**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.stroke) y [**StrokeThickness**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.strokethickness), ya que de lo contrario no se podrá representar esa **línea**.

No uses valores de la estructura [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) para especificar una [**línea**](https://msdn.microsoft.com/library/windows/apps/BR243345); en vez de eso, puedes usar valores discretos de la estructura [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx) para las propiedades [**X1**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.shapes.line.x1.aspx), [**Y1**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.shapes.line.y1.aspx), [**X2**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.shapes.line.x2.aspx) y [**Y2**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.shapes.line.y2.aspx). Esto permite usar un número mínimo de marcas para líneas horizontales o verticales. Por ejemplo, `<Line Stroke="Red" X2="400"/>` define una línea horizontal de 400 píxeles de largo. Las otras propiedades X e Y tienen el valor 0 de manera predeterminada, por lo que, en términos de puntos, este XAML dibujaría una línea de `(0,0)` a `(400,0)`. A continuación, puedes usar la clase [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027) para mover toda la **línea**, si quieres que se inicie en un punto que no sea (0,0).

## <span id="_Polyline"></span><span id="_polyline"></span><span id="_POLYLINE"></span> Polilínea

Una [**Polilínea**](https://msdn.microsoft.com/library/windows/apps/BR243365) es similar a un [**polígono**](https://msdn.microsoft.com/library/windows/apps/BR243359) ya que el límite de la forma está definido por un conjunto de puntos, pero hay que tener en cuenta que el último punto de la **polilínea** no está conectado al primero.

**Nota**   Puedes tener un punto inicial y un punto final explícitamente idénticos en la propiedad [**Points**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.shapes.polyline.points.aspx) establecida para la [**polilínea**](https://msdn.microsoft.com/library/windows/apps/BR243365), pero en ese caso te recomendamos que uses un [**polígono**](https://msdn.microsoft.com/library/windows/apps/BR243359).

 

Si especificas la propiedad [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill) de una [**polilínea**](https://msdn.microsoft.com/library/windows/apps/BR243365), la propiedad **Fill** pintará el espacio interior de la forma, incluso si el punto inicial y el punto final de la propiedad [**Points**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.shapes.polyline.points.aspx) establecidos en la **polilínea** no se cruzan. Si no especificas la propiedad **Fill**, la **polilínea** será similar a lo que se habría representado si hubieras especificado varios elementos [**Line**](https://msdn.microsoft.com/library/windows/apps/BR243345) individuales en los que se cruzan los puntos iniciales y finales de líneas consecutivas.

Al igual que en un [**polígono**](https://msdn.microsoft.com/library/windows/apps/BR243359), la propiedad [**Points**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.shapes.polyline.points.aspx) define la colección de puntos que conforman el límite. En XAML, los puntos se definen con una lista separada por comas. En el código subyacente, se usa una clase [**PointCollection**](https://msdn.microsoft.com/library/windows/apps/BR210220) para definir los puntos; una vez hecho esto, cada punto individual se agrega como una estructura [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) a la colección.

En este ejemplo crearemos una [**polilínea**](https://msdn.microsoft.com/library/windows/apps/BR243365) con cuatro puntos establecidos en `(10,200)`, `(60,140)`, `(130,140)` y `(180,200)`. Se define la propiedad [**Stroke**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.stroke), pero no la propiedad [**Fill**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.fill).

```xml
<Polyline Stroke="Black"
        StrokeThickness="4"
        Points="10,200,60,140,130,140,180,200" />
```

Y aquí tienes la [**polilínea**](https://msdn.microsoft.com/library/windows/apps/BR243365) representada. Observa que el primer y el último punto no están conectados por el contorno establecido por la propiedad [**Stroke**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.shape.stroke), como sucedería con un [**polígono**](https://msdn.microsoft.com/library/windows/apps/BR243359).

![Polilínea representada.](images/shapes-polyline.jpg)

## Trazados

Un [**Trazado**](https://msdn.microsoft.com/library/windows/apps/BR243355) es el objeto más versátil de la clase [**Shape**](https://msdn.microsoft.com/library/windows/apps/BR243377), ya que se puede usar para definir una geometría arbitraria. Pero esta versatilidad implica complejidad. Veamos cómo crear un **trazado** básico en XAML.

Primero, debes definir la geometría de un trazado con la propiedad [**Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data). Existen dos técnicas para establecer la propiedad **Data**:

-   Puedes establecer un valor de cadena para la propiedad [**Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data) en XAML. Con este formato, el valor **Path.Data** usará un formato de serialización para elementos gráficos. Normalmente no se edita el texto de este valor en forma de cadena una vez establecido. En lugar de ello puedes usar herramientas de diseño para trabajar en una metáfora de diseño o dibujo sobre una superficie. A continuación, guardas o exportas la salida para generar un archivo XAML o un fragmento de cadena XAML con información de la propiedad **Path.Data**.
-   Puedes establecer la propiedad [**Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data) en un solo objeto [**Geometry**](https://msdn.microsoft.com/library/windows/apps/BR210041). Esto puede hacerse mediante programación o en XAML. Ese simple objeto **Geometry** suele ser una clase [**GeometryGroup**](https://msdn.microsoft.com/library/windows/apps/BR210041group), que actúa como un contenedor que puede componer varias definiciones de geometría en un solo objeto, para satisfacer los fines del modelo de objetos. La razón más común para hacer esto, es que quieras usar una o más de las curvas y formas complejas que se pueden definir como valores de la propiedad [**Segments**](https://msdn.microsoft.com/library/windows/apps/BR210164) de la clase [**PathFigure**](https://msdn.microsoft.com/library/windows/apps/BR210143) como, por ejemplo, [**BezierSegment**](https://msdn.microsoft.com/library/windows/apps/BR228068).

En este ejemplo se muestra un [**trazado**](https://msdn.microsoft.com/library/windows/apps/BR243355) que puede ser perfectamente el resultado de haber usado Blend para Visual Studio para obtener unas pocas formas vectoriales y guardarlas como un archivo XAML. El **trazado** total consiste en un segmento de curva Bézier y un segmento de línea. Este ejemplo está pensado principalmente para señalar los elementos que existen en el formato de serialización [**Path.Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data) y para indicar qué representan los números.

Esta propiedad [**Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data) comienza con el comando Move, indicado con una "M", que establece el punto de inicio de la trayectoria.

El primer segmento es una curva Bézier cúbica que comienza en `(100,200)` y termina en `(400,175)`, y que se dibuja con los dos puntos de control `(100,25)` y `(400,350)`. Este segmento se indica con el comando "C" de la cadena de atributo [**Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data).

El segundo segmento comienza con un comando de línea horizontal absoluto "H", que especifica una línea dibujada desde el punto de conexión de la subtrayectoria anterior `(400,175)` hasta un nuevo punto de conexión `(280,175)`. Como es un comando de línea horizontal, el valor especificado es una coordenada x.

```xml
<Path Stroke="DarkGoldenRod" 
      StrokeThickness="3"
      Data="M 100,200 C 100,25 400,350 400,175 H 280" />
      ```

Here's the rendered [**Path**](https://msdn.microsoft.com/library/windows/apps/BR243355).

![A rendered Path.](images/shapes-path.jpg)

The next example shows a usage of the other technique we discussed: a [**GeometryGroup**](https://msdn.microsoft.com/library/windows/apps/BR210041group) with a [**PathGeometry**](https://msdn.microsoft.com/library/windows/apps/BR210168). This example exercises some of the contributing geometry types that can be used as part of a **PathGeometry**: [**PathFigure**](https://msdn.microsoft.com/library/windows/apps/BR210143) and the various elements that can be a segment in [**PathFigure.Segments**](https://msdn.microsoft.com/library/windows/apps/BR210164).

```xml
<Path Stroke="Black" StrokeThickness="1" Fill="#CCCCFF">
            <Path.Data>
              <GeometryGroup>
                  <RectangleGeometry Rect="50,5 100,10" />
                  <RectangleGeometry Rect="5,5 95,180" />
                  <EllipseGeometry Center="100, 100" RadiusX="20" RadiusY="30"/>
                  <RectangleGeometry Rect="50,175 100,10" />
                  <PathGeometry>
                    <PathGeometry.Figures>
                      <PathFigureCollection>
                        <PathFigure IsClosed="true" StartPoint="50,50">
                          <PathFigure.Segments>
                            <PathSegmentCollection>
                              <BezierSegment Point1="75,300" Point2="125,100" Point3="150,50"/>
                              <BezierSegment Point1="125,300" Point2="75,100"  Point3="50,50"/>
                            </PathSegmentCollection>
                          </PathFigure.Segments>
                        </PathFigure>
                      </PathFigureCollection>
                    </PathGeometry.Figures>
                  </PathGeometry>               
              </GeometryGroup>
            </Path.Data>
          </Path>
```

Una de las razones por las que querrías usar la clase [**PathGeometry**](https://msdn.microsoft.com/library/windows/apps/BR210168) con las diversas partes, es que cada una de ellas tiene propiedades **Double** y **Point** que podrías usar como destino para realizar una animación de interfaz de usuario. Es más, no puedes hacer esto con el formato de serialización de la propiedad [**Path.Data**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.shapes.path.data). Para obtener más información, consulta [Animaciones con guion gráfico](storyboarded-animations.md).

 

 






<!--HONumber=May16_HO2-->


