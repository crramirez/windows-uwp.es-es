---
description: Conoce los comandos de movimiento y dibujo (un minilenguaje) que podrás usar para especificar geometrías de rutas de acceso como un valor de atributo XAML.
title: Sintaxis de comandos de movimiento y dibujo
ms.assetid: 7772BC3E-A631-46FF-9940-3DD5B9D0E0D9
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a54f87de49e9d43bfa423b0c01b086de1f89426f
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71340484"
---
# <a name="move-and-draw-commands-syntax"></a>Sintaxis de comandos de movimiento y dibujo


Conoce los comandos de movimiento y dibujo (un minilenguaje) que podrás usar para especificar geometrías de rutas de acceso como un valor de atributo XAML. Los comandos de movimiento y dibujo se usan en muchas herramientas de diseño y de gráficos capaces de producir un gráfico de vector o una forma, como un formato de serialización e intercambio.

## <a name="properties-that-use-move-and-draw-command-strings"></a>Propiedades que usan cadenas de comandos de movimiento y dibujo

La sintaxis de comandos de movimiento y dibujo es compatible con un convertidor de tipos interno para XAML, que analiza los comandos y genera una representación de gráficos en tiempo de ejecución. Esta representación es básicamente un conjunto acabado de vectores que están listos para su presentación. Los propios vectores no definen por completo los detalles de la presentación, sino que es necesario definir otros valores en los elementos. En cuanto al objeto [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path), necesitas valores para las propiedades [**Fill**](/uwp/api/Windows.UI.Xaml.Shapes.Shape.Fill) y [**Stroke**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes.shape.stroke) entre otras y, a continuación, debes conectar ese objeto **Path** al árbol visual. En cambio, para el objeto [**PathIcon**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.PathIcon), debes establecer la propiedad [**Foreground**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.iconelement.foreground).

Hay dos propiedades en el Windows Runtime que pueden usar una cadena que representa los comandos de movimiento y dibujo: [**Path. Data**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes.path.data) y [**PathIcon. Data**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.pathicon.data). Si especificas comandos de movimiento y dibujo para establecer una de estas propiedades, normalmente la especificarás como un valor de atributo XAML junto con otros atributos obligatorios de ese elemento. Sin entrar en más detalles, este es el aspecto que tiene:

```xml
<Path x:Name="Arrow" Fill="White" Height="11" Width="9.67"
  Data="M4.12,0 L9.67,5.47 L4.12,10.94 L0,10.88 L5.56,5.47 L0,0.06" />
```

[**PathGeometry. figures**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.pathgeometry.figures) también puede usar comandos de movimiento y dibujo. Asimismo, puedes combinar un objeto [**PathGeometry**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.PathGeometry) que use comandos de movimiento y dibujo con otros tipos de [**Geometry**](/uwp/api/Windows.UI.Xaml.Media.Geometry) en un objeto [**GeometryGroup**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.GeometryGroup), que luego puedes usar como valor de [**Path.Data**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes.path.data). De todos modos, esto no es tan común como usar comandos de movimiento y dibujo para los datos definidos del atributo.

## <a name="using-move-and-draw-commands-versus-using-a-pathgeometry"></a>Usar comandos de movimiento y dibujo y usar la clase **PathGeometry**

Para XAML en Windows Runtime, los comandos de movimiento y dibujo crean una clase [**PathGeometry**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.PathGeometry) que consta de un solo objeto [**PathFigure**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.PathFigure) con un valor de propiedad [**Figures**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.pathgeometry.figures). Cada comando de dibujo crea una clase derivada [**PathSegment**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.PathSegment) en cada colección de [**segmentos**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.pathfigure.segments) del objeto **PathFigure**, el comando de movimiento cambia la propiedad [**StartPoint**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.pathfigure.startpoint) y la existencia de un comando próximo establece [**IsClosed**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.pathfigure.isclosed) en **true**. Puedes examinar esta estructura como si fuera un modelo de objetos si examinas los valores de **Data** en tiempo de ejecución.

## <a name="the-basic-syntax"></a>Sintaxis básica

La sintaxis para mover y dibujar comandos puede resumirse del modo siguiente:

1.  Comienza con una regla de relleno opcional. Esto se suele especificar únicamente si no quieres usar el elemento **EvenOdd** predeterminado. (Encontrarás más información acerca de **EvenOdd** un poco más adelante).
2.  Especifica exactamente un comando de movimiento.
3.  Especifica uno o más comandos de dibujo.
4.  Especifica un comando de cierre. Puedes omitir un comando de cierre, pero esto dejaría tu figura abierta (una situación poco habitual).

Estas son las reglas generales de esta sintaxis:

-   Cada comando viene representado exactamente por una letra+.
-   Esta letra puede ser mayúscula o minúscula. La distinción entre mayúsculas y minúsculas es importante, como ya se sabe.
-   Cada comando, excepto el de cierre, suele ir seguido de uno o varios números.
-   Si un comando tiene varios números, sepáralos con comas o espacios.

**\[** _fillRule_ **\]** _moveCommand_ _drawCommand_ **\[** _drawCommand_**1 @ no__t-12** **4**_closeCommand_**7**

Muchos de los comandos de dibujo usan puntos en los que puedes proporcionar un valor _x,y_. Siempre que vea un marcador de posición \*_puntos_ , puede suponer que está ofreciendo dos valores decimales para el valor _x, y_ de un punto.

Con frecuencia se pueden omitir los espacios en blanco, siempre que el resultado no sea ambiguo. De hecho, puedes omitir todos los espacios en blanco si usas comas como separador para todos los conjuntos de números (puntos y tamaño). Por ejemplo, este uso es aceptable: `F1M0,58L2,56L6,60L13,51L15,53L6,64z`. Pero es más habitual incluir espacios en blanco entre los comandos por motivos de claridad.

No uses la coma como separador decimal. Recuerda que la cadena de comandos se interpreta en lenguaje XAML y que no se tienen en cuenta las convenciones de formato numérico específicas de otras culturas que puedan ser distintas de las usadas en la configuración regional **en-us**.

## <a name="syntax-specifics"></a>Peculiaridades de la sintaxis

**Regla de relleno**

Hay dos valores posibles para la regla de relleno opcional: **F0** o **F1**. (La **F** siempre está en mayúsculas). **F0** es el valor predeterminado; genera un comportamiento de relleno **EvenOdd** , por lo que normalmente no se especifica. Usa **F1** para obtener el comportamiento de relleno **Nonzero**. Estos valores de relleno se alinean con los valores de la enumeración [**FillRule**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.FillRule).

**Comando de movimiento**

Especifica el punto de inicio de una nueva figura.

| Sintaxis |
|--------|
| `M ` _startPoint_ <br/>O bien<br/>`m` _startPoint_|

| Término | Descripción |
|------|-------------|
| _startPoint_ | [**Elija**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point) <br/>Es el punto de inicio de una nueva figura.|

Una **M** mayúscula indica que *startPoint* es una coordenada absoluta; en cambio, una **m** minúscula indica que *startPoint* es el desplazamiento de un punto anterior o (0,0) si no había punto anterior.

**Tenga en cuenta**  It's legal para especificar varios puntos después del comando de movimiento. Se dibuja una línea por esos puntos como si especificaras el comando de línea. Pero no te recomendamos seguir este estilo, mejor usa el comando de línea específico para ello.

**Dibujar comandos**

Un comando de dibujo puede contener varios comandos de forma: línea, línea horizontal, línea vertical, curva Bézier cúbica, curva Bézier cuadrática, curva Bézier cúbica suave, curva Bézier cuadrática suave y arco elíptico.

Para todos los comandos de dibujo, se distingue entre mayúsculas y minúsculas. Las letras mayúsculas indican coordenadas absolutas y las minúsculas indican coordenadas relativas al comando anterior.

Los puntos de control para un segmento son relativos al extremo del segmento anterior. Cuando accedes a más de un comando del mismo tipo de forma secuencial, puedes omitir la entrada de comando duplicada. Por ejemplo, `L 100,200 300,400` equivale a `L 100,200 L 300,400`.

**Línea (comando)**

Crea una línea directa entre el punto actual y el extremo especificado. `l 20 30` y `L 20,30` son ejemplos de comandos de línea válidos. Define el equivalente de un objeto [**LineGeometry**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.LineGeometry).

| Sintaxis |
|--------|
| _punto de conexión_ `L` <br/>O bien<br/>_punto de conexión_ `l` |

| Término | Descripción |
|------|-------------|
| endPoint | [**Elija**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point)<br/>Es el extremo de la línea.|

**Comando de línea horizontal**

Crea una línea horizontal entre el punto actual y la coordenada x especificada. `H 90` es un ejemplo de un comando de línea horizontal válido.

| Sintaxis |
|--------|
| `H ` _x_ <br/> O bien <br/>`h ` _x_ |

| Término | Descripción |
|------|-------------|
| x | [**Hace**](https://docs.microsoft.com/dotnet/api/system.double) <br/> Es la coordenada x del extremo de la línea. |

**Comando de línea vertical**

Crea una línea vertical entre el punto actual y la coordenada y especificada. `v 90` es un ejemplo de un comando de línea vertical válido.

| Sintaxis |
|--------|
| `V ` _y_ <br/> O bien <br/> `v ` _y_ |

| Término | Descripción |
|------|-------------|
| *sí* | [**Hace**](https://docs.microsoft.com/dotnet/api/system.double) <br/> Es la coordenada y del extremo de la línea. |

**Comando de curva Bézier cúbica**

Crea una curva Bézier cúbica entre el punto actual y el extremo especificado, mediante los dos puntos de control especificados (*controlPoint1* y *controlPoint2*). `C 100,200 200,400 300,200` es un ejemplo de un comando de curva válido. Define el equivalente de un objeto [**PathGeometry**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.PathGeometry) con un objeto [**BezierSegment**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.BezierSegment).

| Sintaxis |
|--------|
| `C ` *controlPoint1* *controlPoint2* <br/> O bien <br/> `c ` *controlPoint1* *controlPoint2* |

| Término | Descripción |
|------|-------------|
| *controlPoint1* | [**Elija**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point) <br/> Es el primer punto de control de la curva, que determina la tangente del inicio de la curva. |
| *controlPoint2* | [**Elija**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point) <br/> Es el segundo punto de control de la curva, que determina la tangente del final de la curva. |
| *Finales* | [**Elija**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point) <br/> Es el punto en el que se dibuja la curva. | 

**Comando de curva Bézier cuadrática**

Crea una curva Bézier cuadrática entre el punto actual y el extremo especificado, mediante el punto de control especificado (*controlPoint*). `q 100,200 300,200` es un ejemplo de un comando válido de curva Bézier cuadrática. Define el equivalente de una clase [**PathGeometry**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.PathGeometry) con una clase [**QuadraticBezierSegment**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.QuadraticBezierSegment).

| Sintaxis |
|--------|
| `Q ` *punto de conexión controlPoint* <br/> O bien <br/> `q ` *punto de conexión controlPoint* |

| Término | Descripción |
|------|-------------|
| *controlPoint* | [**Elija**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point) <br/> Es el punto de control de la curva, que determina las tangentes de inicio y final de la curva. |
| *Finales* | [**Elija**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point)<br/> Es el punto en el que se dibuja la curva. |

**Comando de curva Bézier cúbica suavizada**

Crea una curva Bézier cúbica entre el punto actual y el extremo especificado. Se supone que el primer punto de control es el reflejo del segundo punto de control del comando anterior, relativo al punto actual. Si no hay ningún comando anterior o si el comando anterior no era un comando de curva Bézier cúbica o un comando de curva Bézier cúbica suave, se supone que el primer punto de control coincide con el punto actual. El segundo punto de control (el punto de control del extremo de la curva) viene especificado por *controlPoint2*. Por ejemplo, `S 100,200 200,300` es un comando válido de curva Bézier cúbica suave. Este comando define el equivalente de una clase [**PathGeometry**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.PathGeometry) con una clase [**BezierSegment**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.BezierSegment), en la cual había un segmento de curva anterior.

| Sintaxis |
|--------|
| `S` *punto de conexión* controlPoint2 <br/> O bien <br/>`s` *punto de conexión controlPoint2* |

| Término | Descripción |
|------|-------------|
| *controlPoint2* | [**Elija**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point) <br/> Es el punto de control de la curva, que determina la tangente del final de la curva. |
| *Finales* | [**Elija**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point)<br/> Es el punto en el que se dibuja la curva. |

**Comando de curva Bézier cuadrática suavizada**

Crea una curva Bézier cuadrática entre el punto actual y el extremo especificado. Se supone que el punto de control es el reflejo del punto de control del comando anterior, relativo al punto actual. Si no hay ningún comando anterior o si el comando anterior no era un comando de curva Bézier cuadrática o un comando de curva Bézier cuadrática suave, entonces el punto de control coincide con el punto actual. Este comando define el equivalente de una clase [**PathGeometry**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.PathGeometry) con una clase [**QuadraticBezierSegment**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.QuadraticBezierSegment), donde había un segmento de curva anterior.

| Sintaxis |
|--------|
| `T` *punto de conexión* controlPoint <br/> O bien <br/> `t` *punto de conexión* controlPoint |

| Término | Descripción |
|------|-------------|
| *controlPoint* | [**Elija**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point)<br/> Es el punto de control de la curva, que determina las tangentes de inicio y final de la curva. |
| *Finales* | [**Elija**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point)<br/> Es el punto en el que se dibuja la curva. |

**Comando de arco elíptico**

Crea un arco elíptico entre el punto actual y el extremo especificado. Define el equivalente de una clase [**PathGeometry**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.PathGeometry) con una clase [**ArcSegment**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ArcSegment).

| Sintaxis |
|--------|
| `A ` *tamaño* *rotationAngle* *isLargeArcFlag* *sweepDirectionFlag* <br/> O bien <br/>`a ` *sizerotationAngleisLargeArcFlagsweepDirectionFlagendPoint* |

| Término | Descripción |
|------|-------------|
| *size* | [**Ajusta**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Size)<br/>Es el radio x y el radio y del arco. |
| *rotationAngle* | [**Hace**](https://docs.microsoft.com/dotnet/api/system.double) <br/> Es la rotación de la elipse, en grados. |
| *isLargeArcFlag* | Se establece en 1 si el arco debe ser de 180 grados o mayor; de lo contrario, se establece en 0. |
| *sweepDirectionFlag* | Se establece en 1 si el arco se dibuja en una dirección de ángulo positivo; de lo contrario, se establece en 0. |
| *Finales* | [**Elija**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point) <br/> Es el punto en el que se dibuja el arco.|
 
**Close (comando)**

Termina la figura actual y crea una línea que conecta el punto actual con el punto de inicio de la figura. Este comando crea una unión de líneas (esquina) entre el último segmento y el primer segmento de la figura.

| Sintaxis |
|--------|
| `Z` <br/> O bien <br/> `z ` |

**Sintaxis de punto**

Describe las coordenadas x e y de un punto. Consulta también [**Point**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point).

| Sintaxis |
|--------|
| *x*,*y*<br/> O bien <br/>*x* *y* |

| Término | Descripción |
|------|-------------|
| *x* | [**Hace**](https://docs.microsoft.com/dotnet/api/system.double) <br/> Es la coordenada x del punto. |
| *sí* | [**Hace**](https://docs.microsoft.com/dotnet/api/system.double) <br/> Es la coordenada y del punto. |

**Notas adicionales**

En lugar de usar un valor numérico estándar, puedes usar estos valores especiales. Estos valores distinguen entre mayúsculas y minúsculas.

-   **Infinito**: Representa **PositiveInfinity**.
-   **@no__t 1Infinity**: Representa **NegativeInfinity**.
-   **Nan**: Representa **Nan**.

En lugar de usar valores decimales o enteros, puedes usar la notación científica. Por ejemplo, `+1.e17` es un valor válido.

## <a name="design-tools-that-produce-move-and-draw-commands"></a>Herramientas de diseño que producen comandos de movimiento y dibujo

El uso de la herramienta **pluma** y otras herramientas de dibujo en Blend para Microsoft Visual Studio 2015 normalmente producirá un objeto de [**trazado**](/uwp/api/Windows.UI.Xaml.Shapes.Path) , con comandos de movimiento y dibujo.

Es posible que encuentres datos de comandos de movimiento y dibujo existentes en algunas partes del control definidas en las plantillas XAML predeterminadas para los controles en Windows Runtime. Por ejemplo, algunos controles usan una clase [**PathIcon**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.PathIcon) que tiene los datos definidos como comandos de movimiento y dibujo.

Asimismo, hay disponibles complementos o exportadores de otras herramientas que normalmente se usan para el diseño con gráficos de vector y que pueden generar el vector en formato XAML. Normalmente crean objetos [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path) en un contenedor de diseño junto con comandos de movimiento y dibujo de la propiedad [**Path.Data**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes.path.data). Es posible contar con varios elementos **Path** en el XAML para poder aplicar distintos pinceles. Muchos de estos exportadores o complementos se escribieron originalmente para XAML de Windows Presentation Foundation (WPF), pero la sintaxis de la ruta de acceso XAML es idéntica a la de Windows Runtime XAML. Normalmente, es posible usar trozos de XAML de un exportador y pegarlos directamente en una página XAML de Windows Runtime. (No obstante, no podrás usar el pincel **RadialGradientBrush** si este formaba parte del XAML convertido, ya que no es compatible con el lenguaje XAML de Windows Runtime).

## <a name="related-topics"></a>Temas relacionados

* [Dibujar formas](https://docs.microsoft.com/windows/uwp/graphics/drawing-shapes)
* [Usar pinceles](https://docs.microsoft.com/windows/uwp/graphics/using-brushes)
* [**Path. Data**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes.path.data)
* [**PathIcon**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.PathIcon)

