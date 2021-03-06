---
description: Explica el concepto de propiedad adjunta en XAML y proporciona algunos ejemplos.
title: Introducción a las propiedades adjuntas
ms.assetid: 098C1DE0-D640-48B1-9961-D0ADF33266E2
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cpp
ms.openlocfilehash: cc1a879944054ce8fd2b2ea8a2f53aba4d202358
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155119"
---
# <a name="attached-properties-overview"></a>Introducción a las propiedades adjuntas

Una *propiedad adjunta* es un concepto de XAML. Las propiedades adjuntas permiten establecer en un objeto pares adicionales de propiedad/valor, pero las propiedades no forman parte de la definición original de dicho objeto. Por lo general, las propiedades adjuntas se definen como una forma especializada de propiedad de dependencia que no tiene un contenedor de propiedades convencional en el modelo de objetos del tipo de propietario.

## <a name="prerequisites"></a>Requisitos previos

Se da por hecho que comprendes el concepto básico de las propiedades de dependencia y que has leído [Introducción a las propiedades de dependencia](dependency-properties-overview.md).

## <a name="attached-properties-in-xaml"></a>Propiedades adjuntas en XAML

En XAML, las propiedades adjuntas se establecen mediante la sintaxis _proveedordepropiedadadjunta. PropertyName_. Este es un ejemplo de cómo puedes establecer [**Canvas.Left**](/dotnet/api/system.windows.controls.canvas.left) en XAML.

```xaml
<Canvas>
  <Button Canvas.Left="50">Hello</Button>
</Canvas>
```

> [!NOTE]
> Vamos a usar [**Canvas. Left**](/dotnet/api/system.windows.controls.canvas.left) como una propiedad adjunta de ejemplo sin explicar por completo por qué lo usaría. Si quieres saber más sobre el uso de **Canvas.Left** y el modo en que [**Canvas**](/uwp/api/Windows.UI.Xaml.Controls.Canvas) controla sus elementos secundarios de diseño, consulta el tema de referencia de [**Canvas**](/uwp/api/Windows.UI.Xaml.Controls.Canvas) o [Definir diseños con XAML](../design/layout/layouts-with-xaml.md).

## <a name="why-use-attached-properties"></a>¿Por qué usar propiedades adjuntas?

Las propiedades adjuntas son una manera de evitar las convenciones de código que podrían impedir que los distintos objetos de una relación intercambien información en tiempo de ejecución. En efecto, es posible incluir propiedades en una clase base común para que cada objeto pueda simplemente obtener y establecer esa propiedad. Pero el mero número de escenarios en los que podrías hacerlo llenaría las clases base con propiedades que pueden compartirse. Podría incluso dar lugar a casos en los que podría haber cientos de descendientes intentando usar una propiedad. Eso no es un buen diseño de clases. Para solucionar este problema, el concepto de propiedad adjunta permite a un objeto asignar un valor para una propiedad que no está definida por su propia estructura de clase. La clase definidora puede leer el valor desde los objetos secundarios en tiempo de ejecución después de que se creen los diversos objetos en un árbol de objetos.

Por ejemplo, los elementos secundarios pueden usar las propiedades adjuntas para informar a su elemento primario sobre cómo deben presentarse en la interfaz de usuario. Este es el caso de la propiedad adjunta [**Canvas.Left**](/dotnet/api/system.windows.controls.canvas.left). **Canvas.Left** se crea como una propiedad adjunta porque se establece en elementos contenidos en un elemento [**Canvas**](/uwp/api/Windows.UI.Xaml.Controls.Canvas), en lugar de en el propio **Canvas**. Después, cualquier elemento secundario posible usa **Canvas.Left** y [**Canvas.Top**](/dotnet/api/system.windows.controls.canvas.top) para especificar su desplazamiento dentro del elemento primario contenedor de diseño **Canvas**. Las propiedades adjuntas permiten que este escenario funcione sin abarrotar el modelo de objetos de elementos base con numerosas propiedades que solo se aplican a uno de los muchos contenedores de diseño posibles. En su lugar, muchos de los contenedores de diseño implementan su propio conjunto de propiedades adjuntas.

Para implementar la propiedad adjunta, la clase [**Canvas**](/uwp/api/Windows.UI.Xaml.Controls.Canvas) define un campo [**DependencyProperty**](/uwp/api/Windows.UI.Xaml.DependencyProperty) estático denominado [**Canvas.LeftProperty**](/uwp/api/windows.ui.xaml.controls.canvas.leftproperty). A continuación, **Canvas** proporciona los métodos [**SetLeft**](/uwp/api/windows.ui.xaml.controls.canvas.setleft) y [**GetLeft**](/uwp/api/windows.ui.xaml.controls.canvas.getleft) como descriptores de acceso públicos para la propiedad adjunta, para habilitar tanto la configuración XAML como el acceso a valores en tiempo de ejecución. En XAML y en el sistema de propiedades de dependencia, este conjunto de API sigue un patrón que habilita una sintaxis XAML específica para las propiedades adjuntas, y almacena el valor en el almacén de propiedades de dependencia.

## <a name="how-the-owning-type-uses-attached-properties"></a>Cómo usa el tipo propietario las propiedades adjuntas

Aunque las propiedades adjuntas puedan establecerse en cualquier elemento XAML (o [**DependencyObject**](/uwp/api/Windows.UI.Xaml.DependencyObject) subyacente), eso no significa automáticamente que al establecer la propiedad se produzca un resultado tangible, o que alguna vez se obtenga acceso al valor. El tipo que define la propiedad adjunta generalmente sigue uno de estos escenarios:

- El tipo que define la propiedad adjunta es el elemento primario en una relación de otros objetos. Los objetos secundarios establecerán valores para la propiedad adjunta. El tipo de propietario de la propiedad adjunta tiene algún tipo de comportamiento innato que itera en sus elementos secundarios, obtiene los valores y actúa en esos valores en algún momento de la duración del objeto (una acción de diseño, [**SizeChanged**](/uwp/api/windows.ui.xaml.frameworkelement.sizechanged), etc.).
- El tipo que define la propiedad adjunta se usa como elemento secundario para una variedad de posibles elementos primarios y modelos de contenido, pero la información no es necesariamente información de diseño.
- La propiedad adjunta notifica información a un servicio y no a otro elemento de la interfaz de usuario.

Para obtener más información sobre estos escenarios y tipos propietarios, consulta la sección "Más información sobre Canvas.Left" de [Propiedades adjuntas personalizadas](custom-attached-properties.md).

## <a name="attached-properties-in-code"></a>Propiedades adjuntas en el código 

Las propiedades adjuntas no tienen contenedores de propiedades típicos para obtener o establecer acceso fácilmente como lo hacen otras propiedades de dependencia. Esto se debe a que la propiedad adjunta no forma parte necesariamente del modelo de objetos centrado en el código para las instancias en las que se establece la propiedad. (Aunque no es lo más común, se permite definir una propiedad que sea tanto una propiedad adjunta que otros tipos puedan establecer en sí mismos, como que tenga un uso de propiedad convencional en el tipo propietario).

Hay dos formas de establecer una propiedad adjunta en el código: usar las API del sistema de propiedades o usar los descriptores de acceso del patrón XAML. El resultado de cualquiera de las dos técnicas es casi idéntico, por lo tanto, la decisión acerca de cuál usar es una cuestión de estilo de codificación.

### <a name="using-the-property-system"></a>Uso del sistema de propiedades

Las propiedades adjuntas para Windows Runtime se implementan como propiedades de dependencia, para que el sistema de propiedades pueda almacenar los valores en el almacén de propiedades de dependencia compartido. Por lo tanto, las propiedades adjuntas exponen un identificador de propiedad de dependencia en la clase propietaria.

Para establecer una propiedad adjunta en el código, llama al método [**SetValue**](/uwp/api/windows.ui.xaml.dependencyobject.setvalue) y pasa el campo [**DependencyProperty**](/uwp/api/Windows.UI.Xaml.DependencyProperty) que funciona como identificador de esa propiedad adjunta. (Pasa también el valor que vas a establecer).

Para obtener el valor de una propiedad adjunta en el código, llama al método [**GetValue**](/uwp/api/windows.ui.xaml.dependencyobject.getvalue) y vuelve a pasar el campo [**DependencyProperty**](/uwp/api/Windows.UI.Xaml.DependencyProperty) que funciona como identificador.

### <a name="using-the-xaml-accessor-pattern"></a>Uso del patrón de descriptor de acceso XAML

Un procesador XAML debe poder establecer los valores de las propiedades adjuntas cuando se analiza el código XAML en un árbol de objetos. El tipo de propietario de la propiedad adjunta debe implementar métodos de descriptor de acceso exclusivos con nombre en forma de **Get**_PropertyName_ y **Set**_PropertyName_. Estos descriptores de acceso exclusivos también constituyen una forma de obtener o establecer la propiedad adjunta en el código. Desde el punto de vista del código, una propiedad adjunta es similar a un campo de respaldo en que cuenta con descriptores de acceso del método en lugar de descriptores de acceso de la propiedad, y en que el campo de respaldo puede existir en cualquier objeto sin necesidad de definirlo específicamente.

El siguiente ejemplo muestra cómo se puede establecer una propiedad adjunta en el código a través de la API del descriptor de acceso XAML. En este ejemplo, `myCheckBox` es una instancia de la clase [**CheckBox**](/uwp/api/Windows.UI.Xaml.Controls.CheckBox). La última línea es el código que en realidad establece el valor, las líneas anteriores simplemente establecen las instancias y la relación entre sus elementos primarios y secundarios. La última línea sin comentario es la sintaxis si usas el sistema de propiedades. La última línea comentada es la sintaxis si usas el patrón del descriptor de acceso XAML.

```csharp
    Canvas myC = new Canvas();
    CheckBox myCheckBox = new CheckBox();
    myCheckBox.Content = "Hello";
    myC.Children.Add(myCheckBox);
    myCheckBox.SetValue(Canvas.TopProperty,75);
    //Canvas.SetTop(myCheckBox, 75);
```

```vb
    Dim myC As Canvas = New Canvas()
    Dim myCheckBox As CheckBox= New CheckBox()
    myCheckBox.Content = "Hello"
    myC.Children.Add(myCheckBox)
    myCheckBox.SetValue(Canvas.TopProperty,75)
    ' Canvas.SetTop(myCheckBox, 75)
```

```cppwinrt
Canvas myC;
CheckBox myCheckBox;
myCheckBox.Content(winrt::box_value(L"Hello"));
myC.Children().Append(myCheckBox);
myCheckBox.SetValue(Canvas::TopProperty(), winrt::box_value(75));
// Canvas::SetTop(myCheckBox, 75);
```

```cpp
    Canvas^ myC = ref new Canvas();
    CheckBox^ myCheckBox = ref new CheckBox();
    myCheckBox->Content="Hello";
    myC->Children->Append(myCheckBox);
    myCheckBox->SetValue(Canvas::TopProperty,75);
    // Canvas::SetTop(myCheckBox, 75);
```

## <a name="custom-attached-properties"></a>Propiedades adjuntas personalizadas

Para ver ejemplos de código sobre cómo definir propiedades adjuntas personalizadas y más información acerca de los escenarios donde se usa una propiedad adjunta, consulta [Propiedades adjuntas personalizadas](custom-attached-properties.md).

## <a name="special-syntax-for-attached-property-references"></a>Sintaxis especial para referencias a propiedades adjuntas

El punto en el nombre de la propiedad adjunta es una parte fundamental del patrón de identificación. A veces se producen ambigüedades cuando una sintaxis o una situación trata el punto como si tuviese otro significado. Por ejemplo, el punto se trata como un cruce seguro del modelo de objetos para una ruta de acceso de enlace. En la mayoría de los casos en los que se produce esta ambigüedad, la propiedad adjunta tiene una sintaxis especial que permite que el punto interno se analice como separador _owner_**.**_property_ de una propiedad adjunta.

- Para especificar una propiedad adjunta como parte de una ruta de destino de una animación, encierra el nombre de la propiedad adjunta entre paréntesis ("()"), por ejemplo, "(Canvas.Left)". Para más información, consulta [Sintaxis de property-path](property-path-syntax.md).

> [!WARNING]
> Una limitación existente de la implementación de XAML Windows Runtime es que no se puede animar una propiedad adjunta personalizada.

- Para especificar una propiedad adjunta como la propiedad de destino de una referencia de recurso de un archivo de recursos a **x:UID**, use una sintaxis especial que inserte un estilo de código completo **mediante** la declaración entre corchetes (" \[ \] "), para crear un salto de ámbito deliberado. Por ejemplo, suponiendo que existe un elemento `<TextBlock x:Uid="Title" />` , la clave de recurso en el archivo de recursos que tiene como destino el valor **Canvas. Top** en esa instancia es "title. \[ usar: Windows. UI. Xaml. Controls \] Canvas. Top ". Para obtener más información sobre archivos de recursos y XAML, consulta [Inicio rápido: traducción de recursos de interfaz de usuario](/previous-versions/windows/apps/hh965329(v=win.10)).

## <a name="related-topics"></a>Temas relacionados

- [Propiedades adjuntas personalizadas](custom-attached-properties.md)
- [Introducción a las propiedades de dependencia](dependency-properties-overview.md)
- [Definir diseños con XAML](../design/layout/layouts-with-xaml.md)
- [Inicio rápido: traducción de recursos de interfaz de usuario](/previous-versions/windows/apps/hh943060(v=win.10))
- [**EstablecerValor**](/uwp/api/windows.ui.xaml.dependencyobject.setvalue)
- [**GetValue**](/uwp/api/windows.ui.xaml.dependencyobject.getvalue)