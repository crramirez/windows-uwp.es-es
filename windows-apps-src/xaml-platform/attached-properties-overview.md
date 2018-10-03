---
author: jwmsft
description: Explica el concepto de propiedad adjunta en XAML y proporciona algunos ejemplos.
title: Introducción a las propiedades adjuntas
ms.assetid: 098C1DE0-D640-48B1-9961-D0ADF33266E2
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cpp
ms.openlocfilehash: 7f92b12ab9c8962fe98d8eed22b21e7d10330c99
ms.sourcegitcommit: 1938851dc132c60348f9722daf994b86f2ead09e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2018
ms.locfileid: "4263312"
---
# <a name="attached-properties-overview"></a>Introducción a las propiedades adjuntas

Una *propiedad adjunta* es un concepto de XAML. Las propiedades adjuntas permiten establecer en un objeto pares adicionales de propiedad/valor, pero las propiedades no forman parte de la definición original de dicho objeto. Por lo general, las propiedades adjuntas se definen como una forma especializada de propiedad de dependencia que no tiene un contenedor de propiedades convencional en el modelo de objetos del tipo de propietario.

## <a name="prerequisites"></a>Requisitos previos

Se da por hecho que comprendes el concepto básico de las propiedades de dependencia y que has leído [Introducción a las propiedades de dependencia](dependency-properties-overview.md).

## <a name="attached-properties-in-xaml"></a>Propiedades adjuntas en XAML

En XAML, las propiedades adjuntas se establecen mediante la sintaxis _AttachedPropertyProvider.PropertyName_. Este es un ejemplo de cómo puedes establecer [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/hh759771) en XAML.

```xaml
<Canvas>
  <Button Canvas.Left="50">Hello</Button>
</Canvas>
```

> [!NOTE]
> Vamos a usar [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/hh759771) como una propiedad adjuntado de ejemplo sin explicar por qué la usarías. Si quieres saber más sobre el uso de **Canvas.Left** y el modo en que [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267) controla sus elementos secundarios de diseño, consulta el tema de referencia de [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267) o [Definir diseños con XAML](https://msdn.microsoft.com/library/windows/apps/mt228350).

## <a name="why-use-attached-properties"></a>¿Por qué usar propiedades adjuntas?

Las propiedades adjuntas son una manera de evitar las convenciones de código que podrían impedir que los distintos objetos de una relación intercambien información en tiempo de ejecución. En efecto, es posible incluir propiedades en una clase base común para que cada objeto pueda simplemente obtener y establecer esa propiedad. Pero el mero número de escenarios en los que podrías hacerlo llenaría las clases base con propiedades que pueden compartirse. Podría incluso dar lugar a casos en los que podría haber cientos de descendientes intentando usar una propiedad. Eso no es un buen diseño de clases. Para solucionar este problema, el concepto de propiedad adjunta permite a un objeto asignar un valor para una propiedad que no está definida por su propia estructura de clase. La clase definidora puede leer el valor desde los objetos secundarios en tiempo de ejecución después de que se creen los diversos objetos en un árbol de objetos.

Por ejemplo, los elementos secundarios pueden usar las propiedades adjuntas para informar a su elemento primario sobre cómo deben presentarse en la interfaz de usuario. Este es el caso de la propiedad adjunta [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/hh759771). **Canvas.Left** se crea como una propiedad adjunta porque se establece en elementos contenidos en un elemento [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267), en lugar de en el propio **Canvas**. Después, cualquier elemento secundario posible usa **Canvas.Left** y [**Canvas.Top**](https://msdn.microsoft.com/library/windows/apps/hh759772) para especificar su desplazamiento dentro del elemento primario contenedor de diseño **Canvas**. Las propiedades adjuntas permiten que este escenario funcione sin abarrotar el modelo de objetos de elementos base con numerosas propiedades que solo se aplican a uno de los muchos contenedores de diseño posibles. En su lugar, muchos de los contenedores de diseño implementan su propio conjunto de propiedades adjuntas.

Para implementar la propiedad adjunta, la clase [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267) define un campo [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) estático denominado [**Canvas.LeftProperty**](https://msdn.microsoft.com/library/windows/apps/br209272). A continuación, **Canvas** proporciona los métodos [**SetLeft**](https://msdn.microsoft.com/library/windows/apps/br209273) y [**GetLeft**](https://msdn.microsoft.com/library/windows/apps/br209269) como descriptores de acceso públicos para la propiedad adjunta, para habilitar tanto la configuración XAML como el acceso a valores en tiempo de ejecución. En XAML y en el sistema de propiedades de dependencia, este conjunto de API sigue un patrón que habilita una sintaxis XAML específica para las propiedades adjuntas, y almacena el valor en el almacén de propiedades de dependencia.

## <a name="how-the-owning-type-uses-attached-properties"></a>Cómo usa el tipo propietario las propiedades adjuntas

Aunque las propiedades adjuntas puedan establecerse en cualquier elemento XAML (o [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356) subyacente), eso no significa automáticamente que al establecer la propiedad se produzca un resultado tangible, o que alguna vez se obtenga acceso al valor. El tipo que define la propiedad adjunta generalmente sigue uno de estos escenarios:

- El tipo que define la propiedad adjunta es el elemento primario en una relación de otros objetos. Los objetos secundarios establecerán valores para la propiedad adjunta. El tipo de propietario de la propiedad adjunta tiene algún tipo de comportamiento innato que itera en sus elementos secundarios, obtiene los valores y actúa en esos valores en algún momento de la duración del objeto (una acción de diseño, [**SizeChanged**](https://msdn.microsoft.com/library/windows/apps/br208742), etc.).
- El tipo que define la propiedad adjunta se usa como elemento secundario para una variedad de posibles elementos primarios y modelos de contenido, pero la información no es necesariamente información de diseño.
- La propiedad adjunta notifica información a un servicio y no a otro elemento de la interfaz de usuario.

Para obtener más información sobre estos escenarios y tipos propietarios, consulta la sección "Más información sobre Canvas.Left" de [Propiedades adjuntas personalizadas](custom-attached-properties.md).

## <a name="attached-properties-in-code"></a>Propiedades adjuntas en el código 


Las propiedades adjuntas no tienen contenedores de propiedades típicos para obtener o establecer acceso fácilmente como lo hacen otras propiedades de dependencia. Esto se debe a que la propiedad adjunta no forma parte necesariamente del modelo de objetos centrado en el código para las instancias en las que se establece la propiedad. (Aunque no es lo más común, se permite definir una propiedad que sea tanto una propiedad adjunta que otros tipos puedan establecer en sí mismos, como que tenga un uso de propiedad convencional en el tipo propietario).

Hay dos formas de establecer una propiedad adjunta en el código: usar las API del sistema de propiedades o usar los descriptores de acceso del patrón XAML. El resultado de cualquiera de las dos técnicas es casi idéntico, por lo tanto, la decisión acerca de cuál usar es una cuestión de estilo de codificación.

### <a name="using-the-property-system"></a>Uso del sistema de propiedades

Las propiedades adjuntas para Windows Runtime se implementan como propiedades de dependencia, para que el sistema de propiedades pueda almacenar los valores en el almacén de propiedades de dependencia compartido. Por lo tanto, las propiedades adjuntas exponen un identificador de propiedad de dependencia en la clase propietaria.

Para establecer una propiedad adjunta en el código, llama al método [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361) y pasa el campo [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) que funciona como identificador de esa propiedad adjunta. (Pasa también el valor que vas a establecer).

Para obtener el valor de una propiedad adjunta en el código, llama al método [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359) y vuelve a pasar el campo [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) que funciona como identificador.

### <a name="using-the-xaml-accessor-pattern"></a>Uso del patrón de descriptor de acceso XAML

Un procesador XAML debe poder establecer los valores de las propiedades adjuntas cuando se analiza el código XAML en un árbol de objetos. El tipo de propietario de la propiedad adjunta debe implementar métodos de descriptor de acceso exclusivos con nombre en el formulario **obtener *** PropertyName* y **establecer *** PropertyName*. Estos descriptores de acceso exclusivos también constituyen una forma de obtener o establecer la propiedad adjunta en el código. Desde el punto de vista del código, una propiedad adjunta es similar a un campo de respaldo en que cuenta con descriptores de acceso del método en lugar de descriptores de acceso de la propiedad, y en que el campo de respaldo puede existir en cualquier objeto sin necesidad de definirlo específicamente.

El siguiente ejemplo muestra cómo se puede establecer una propiedad adjunta en el código a través de la API del descriptor de acceso XAML. En este ejemplo, `myCheckBox` es una instancia de la clase [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/br209316). La última línea es el código que en realidad establece el valor, las líneas anteriores simplemente establecen las instancias y la relación entre sus elementos primarios y secundarios. La última línea sin comentario es la sintaxis si usas el sistema de propiedades. La última línea comentada es la sintaxis si usas el patrón del descriptor de acceso XAML.

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

- Para especificar una propiedad adjunta como parte de una ruta de destino de una animación, encierra el nombre de la propiedad adjunta entre paréntesis ("()"), por ejemplo, "(Canvas.Left)". Para obtener más información, consulta [Sintaxis de property-path](property-path-syntax.md).

> [!WARNING]
> Una limitación de la implementación de XAML de Windows Runtime es que no puedes animar una propiedad adjunta personalizada.

- Para especificar una propiedad adjunta como propiedad de destino para una referencia a un recurso desde un archivo de recursos a **x:Uid**, usa una sintaxis especial que inyecte una declaración **using:** completa con estilo de código entre corchetes ("\[\]") para crear un salto de ámbito deliberado. Por ejemplo, si suponemos que existe un elemento `<TextBlock x:Uid="Title" />`, la clave de recurso en el archivo de recursos que tiene como destino el valor **Canvas.Top** en esa instancia es "Title.\[using:Windows.UI.Xaml.Controls\]Canvas.Top". Para obtener más información sobre archivos de recursos y XAML, consulta [Inicio rápido: traducción de recursos de interfaz de usuario](https://msdn.microsoft.com/library/windows/apps/xaml/hh965329).

## <a name="related-topics"></a>Temas relacionados

- [Propiedades adjuntas personalizadas](custom-attached-properties.md)
- [Introducción a las propiedades de dependencia](dependency-properties-overview.md)
- [Definir diseños con XAML](https://msdn.microsoft.com/library/windows/apps/mt228350)
- [Inicio rápido: traducción de recursos de interfaz de usuario](https://msdn.microsoft.com/library/windows/apps/hh943060)
- [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361)
- [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359)
