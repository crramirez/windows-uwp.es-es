---
author: jwmsft
description: "Se explica cómo implementar una propiedad adjunta de XAML como una propiedad de dependencia y cómo definir la convención de descriptor de acceso necesaria para que la propiedad adjunta se pueda usar en XAML."
title: Propiedades adjuntas personalizadas
ms.assetid: E9C0C57E-6098-4875-AA3E-9D7B36E160E0
ms.sourcegitcommit: 07058b48a527414b76d55b153359712905aa9786
ms.openlocfilehash: cf6ca169623311e515f02a174224d57652afc753

---

# Propiedades adjuntas personalizadas

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Una *propiedad adjunta* es un concepto de XAML. Las propiedades adjuntas suelen definirse como una forma especializada de propiedad de dependencia. En este tema se explica cómo implementar una propiedad adjunta de XAML como una propiedad de dependencia y cómo definir la convención de descriptor de acceso necesaria para que la propiedad adjunta se pueda usar en XAML.

## Requisitos previos

Damos por hecho que conoces las propiedades de dependencia desde el punto de vista de un usuario de propiedades de dependencia existentes y, asimismo, que has leído la [Introducción a las propiedades de dependencia](dependency-properties-overview.md). También tienes que haber leído la [Introducción a las propiedades adjuntas](attached-properties-overview.md). Para seguir los ejemplos de este tema, también debes conocer XAML y saber cómo escribir una aplicación básica de Windows en tiempo de ejecución con C++, C# o Visual Basic.

## Escenarios para propiedades adjuntas

Puedes crear una propiedad adjunta cuando haya motivos para que las clases que no sean clases definidoras tengan un mecanismo de establecimiento de propiedades. Los escenarios más habituales para esto son diseño y servicios. Algunos ejemplos de propiedades de diseño existentes son [**Canvas.ZIndex**](https://msdn.microsoft.com/library/windows/apps/hh759773) y [**Canvas.Top**](https://msdn.microsoft.com/library/windows/apps/hh759772). En un escenario de diseño, los elementos que existen como elementos secundarios de elementos de control de diseño pueden expresar los requisitos de diseño a sus elementos primarios individualmente, cada uno de los cuales establece un valor de propiedad que el elemento primario define como propiedad adjunta. Un ejemplo de escenario de servicios en la API de Windows Runtime es establecer las propiedades adjuntas de [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/br209527), como [**ScrollViewer.IsZoomChainingEnabled**](https://msdn.microsoft.com/library/windows/apps/br209561).


            **Atención**  Una limitación que presenta la implementación de XAML de Windows Runtime es que las propiedades adjuntas personalizadas no se pueden animar.

## Registro de una propiedad adjunta personalizada

Si vas a definir la propiedad adjunta para usarla exclusivamente en otros tipos, la clase donde se registra la propiedad no tiene que derivar de [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356). No obstante, si sigues el modelo típico para hacer que tu propiedad adjunta sea también una propiedad de dependencia, debes hacer que el parámetro de destino de los descriptores de acceso use **DependencyObject** con el fin de poder usar la memoria auxiliar de propiedades.

Para definir tu propiedad adjunta como una propiedad de dependencia, declara una propiedad **public****static****readonly** del tipo [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362). Esta propiedad se define usando el valor de retorno del método [**RegisterAttached**](https://msdn.microsoft.com/library/windows/apps/hh701833). El nombre de la propiedad debe coincidir con el nombre de la propiedad adjunta que especificaste como parámetro *name* de **RegisterAttached**, con la cadena "Property" anexada al final. Esta es la convención establecida para asignar nombre a los identificadores de las propiedades de dependencia en relación con las propiedades que representan.

La principal diferencia entre una propiedad adjunta personalizada y una propiedad de dependencia personalizada es la manera de definir los descriptores de acceso o contenedores. En lugar de usar la técnica de contenedor descrita en [Propiedades de dependencia personalizadas](custom-dependency-properties.md), también debes proporcionar los métodos **Get***PropertyName* y **Set***PropertyName* estáticos como descriptores de acceso para la propiedad adjunta. Los descriptores de acceso son usados principalmente por el analizador XAML, aunque algunos otros llamadores pueden usarlos para establecer valores en escenarios que no sean de XAML.


            **Importante**  Si no defines correctamente los descriptores de acceso, el procesador XAML no tendrá acceso a la propiedad adjunta y cualquiera que intente usarla obtendrá un error del analizador XAML. Además, las herramientas de diseño y codificación suelen usar las convenciones "\*Property" para asignar un nombre a los identificadores cuando encuentran una propiedad de dependencia personalizada en un ensamblado al que se hace referencia.

## Descriptores de acceso

La firma del descriptor de acceso **Get**_PropertyName_ debe ser esta:

`public static` 
            _valueType_
            **Get**
            _PropertyName_
           `(DependencyObject target)`

En Microsoft Visual Basic, es esta:

` Public Shared Function Get`
            _PropertyName_
            `(ByVal target As DependencyObject) As `
            _valueType_
          `)`

El objeto *target* puede ser de un tipo más específico en tu implementación, pero debe derivar de [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356). El valor de retorno de *valueType* también puede ser de un tipo más específico en tu implementación. El tipo **Object** básico es aceptable, pero con frecuencia querrás que la propiedad adjunta exija seguridad de tipos. El uso de establecimiento de tipos en las firmas getter y setter es una técnica de seguridad de tipos recomendada.

La firma del descriptor de acceso **Set***PropertyName* debe ser esta:

`  public static void Set`
            _PropertyName_
            ` (DependencyObject target , `
            _valueType_
          ` value)`

En Visual Basic, es esta:

`Public Shared Sub Set`
            _PropertyName_
            ` (ByVal target As DependencyObject, ByVal value As `
            _valueType_
          `)`

El objeto *target* puede ser de un tipo más específico en tu implementación, pero debe derivar de [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356). El objeto *value* y su *valueType* pueden ser de un tipo más específico en tu implementación. Recuerda que el valor de este método es la entrada que procede del procesador XAML cuando encuentra tu propiedad adjunta en el marcado. Debe haber conversión de tipos o compatibilidad existente para extensión de marcado para el tipo que uses, de manera que se pueda crear el tipo apropiado a partir del valor de un atributo (que al final es tan solo una cadena). El tipo **Object** básico es aceptable, pero con frecuencia te interesará una mayor seguridad de tipos. Para ello, pon la aplicación de tipos en los accesorios.


            **Nota**  También es posible definir una propiedad adjunta en la que el uso previsto sea mediante sintaxis de elemento de propiedad. En tal caso, no necesita la conversión de tipos para los valores, pero sí que debe asegurarse de que los valores que tiene previstos se puedan construir en XAML. 
            [
              **VisualStateManager.VisualStateGroups**
            ](https://msdn.microsoft.com/library/windows/apps/hh738505) es un ejemplo de una propiedad adjunta existente que solo admite el uso de elementos de propiedad.

## Ejemplo de código

Este ejemplo muestra el registro de la propiedad de dependencia (usando el método [**RegisterAttached**](https://msdn.microsoft.com/library/windows/apps/hh701833)), así como los descriptores de acceso **Get** y **Set** para una propiedad adjunta personalizada. En el ejemplo, el nombre de la propiedad adjunta es `IsMovable`. Por consiguiente, los descriptores de acceso deben denominarse `GetIsMovable` y `SetIsMovable`. El propietario de la propiedad adjunta es una clase de servicio denominada `GameService` que no tiene una interfaz de usuario propia; su objetivo es solo proporcionar los servicios de la propiedad adjunta cuando se use la propiedad adjunta **GameService.IsMovable**.

> [!div class="tabbedCodeSnippets"]
```csharp
    public class GameService : DependencyObject
    {
        public static readonly DependencyProperty IsMovableProperty = 
        DependencyProperty.RegisterAttached(
          "IsMovable",
          typeof(Boolean),
          typeof(GameService),
          new PropertyMetadata(false)
        );
        public static void SetIsMovable(UIElement element, Boolean value)
        {
            element.SetValue(IsMovableProperty, value);
        }
        public static Boolean GetIsMovable(UIElement element)
        {
            return (Boolean)element.GetValue(IsMovableProperty);
        }
    }
```
```vb
Public Class GameService
    Inherits DependencyObject

    Public Shared ReadOnly IsMovableProperty As DependencyProperty = 
        DependencyProperty.RegisterAttached("IsMovable",  
        GetType(Boolean), 
        GetType(GameService), 
        New PropertyMetadata(False))

    Public Shared Sub SetIsMovable(ByRef element As UIElement, value As Boolean)
        element.SetValue(IsMovableProperty, value)
    End Sub

    Public Shared Function GetIsMovable(ByRef element As UIElement) As Boolean
        GetIsMovable = CBool(element.GetValue(IsMovableProperty))
    End Function
End Class
```

[!div class="tabbedCodeSnippets"] Definir la propiedad adjunta en C++ es un poco más complejo. Tienes que decidir cómo repartir entre el archivo de encabezado y de código. Además, debes exponer el identificador como una propiedad con solo un descriptor de acceso **get**, por los motivos tratados en [Propiedades de dependencia personalizadas](custom-dependency-properties.md). En C++, debes definir esta relación propiedad-campo explícitamente en lugar de usar la palabra clave **readonly** de .NET y el respaldo implícito de las propiedades simples. También tienes que registrar la propiedad adjunta dentro de una función auxiliar que solo se ejecute una vez: cuando se inicie la aplicación por primera vez pero antes de que se cargue cualquier página XAML que necesite la propiedad adjunta.

```cpp
//
// GameService.h
// Declaration of the GameService class.
//

#pragma once

#include "pch.h"
//namespace WUX = Windows::UI::Xaml;

namespace UserAndCustomControls {
    public ref class GameService sealed : public WUX::DependencyObject {
    private:
        static WUX::DependencyProperty^ _IsMovableProperty;
    public:
        GameService::GameService();
        void GameService::RegisterDependencyProperties();
        static property WUX::DependencyProperty^ IsMovableProperty
        {
            WUX::DependencyProperty^ get() {
                return _IsMovableProperty;
            }
        };
        static bool GameService::GetIsMovable(WUX::UIElement^ element) {
            return (bool)element->GetValue(_IsMovableProperty);
        };
        static void GameService::SetIsMovable(WUX::UIElement^ element, bool value) {
            element->SetValue(_IsMovableProperty,value);
        }
    };
}
```

```cpp
//
// GameService.cpp
// Implementation of the GameService class.
//

#include "pch.h"
#include "GameService.h"

using namespace UserAndCustomControls;

using namespace Platform;
using namespace Windows::Foundation;
using namespace Windows::Foundation::Collections;
using namespace Windows::UI::Xaml;
using namespace Windows::UI::Xaml::Controls;
using namespace Windows::UI::Xaml::Data;
using namespace Windows::UI::Xaml::Documents;
using namespace Windows::UI::Xaml::Input;
using namespace Windows::UI::Xaml::Interop;
using namespace Windows::UI::Xaml::Media;

GameService::GameService() {

};


GameService::RegisterDependencyProperties() {
    DependencyProperty^ GameService::_IsMovableProperty = DependencyProperty::RegisterAttached(
         "IsMovable", Platform::Boolean::typeid, GameService::typeid, ref new PropertyMetadata(false));
}
```

## La ubicación típica para llamar a las funciones auxiliares de registro de propiedades para cualquier propiedad de dependencia o adjunta es desde el constructor **App** / [**Application**](https://msdn.microsoft.com/library/windows/apps/br242325) en el código del archivo app.xaml.

Uso de la propiedad adjunta personalizada en XAML Después de definir la propiedad adjunta e incluir sus miembros de soporte como parte de un tipo personalizado, debes hacer que las definiciones estén disponibles para el uso de XAML. Para ello, debes asignar un espacio de nombres XAML que hará referencia al espacio de nombres del código que contiene la clase relevante.

Si has definido la propiedad adjunta como parte de una biblioteca, debes incluir dicha biblioteca en el paquete de la aplicación. Normalmente, la asignación de un espacio de nombres XML para XAML se coloca en el elemento raíz de una página XAML.

```XML
<UserControl
  xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
  xmlns:uc="using:UserAndCustomControls"
...
>
```

Por ejemplo, para la clase `GameService` en el espacio de nombres `UserAndCustomControls`, que contiene las definiciones de propiedad adjunta que se muestran en los fragmentos anteriores, la asignación podría tener el siguiente aspecto.

```XML
<Image uc:GameService.IsMovable="true" .../>
```

Mediante la asignación puedes establecer la propiedad adjunta `GameService.IsMovable` en cualquier elemento que coincida con tu definición de destino, incluido un tipo existente definido por Windows Runtime. Si estableces la propiedad en un elemento que también está en el mismo espacio de nombres XML asignado, también debes incluir el prefijo en el nombre de la propiedad adjunta. El motivo es que el prefijo cualifica el tipo de propietario. Aunque, según las reglas normales de XML, los atributos pueden heredar de los elementos el espacio de nombres, no se puede dar por sentado que el atributo de la propiedad adjunta está en el mismo espacio de nombres XML que el elemento donde se incluye el atributo.

```XML
<uc:ImageWithLabelControl uc:GameService.IsMovable="true" .../>
```

Por ejemplo, si estableces `GameService.IsMovable` en un tipo personalizado de `ImageWithLabelControl` (no se muestra la definición) y aunque ambos se definieran en el mismo espacio de nombres de código asignado al mismo prefijo, el XAML seguiría siendo este. 
            **Nota**  Si estás escribiendo una interfaz de usuario de XAML con C++, debes incluir el encabezado para el tipo personalizado que define la propiedad adjunta cada vez que una página XAML use ese tipo. Todas las páginas XAML tienen asociado un encabezado .xaml.h de código subyacente.

## Aquí es donde debes incluir (mediante **\#include**) el encabezado para la definición del tipo de propietario de la propiedad adjunta.

Tipo de valor de una propiedad adjunta personalizada El tipo que se use como tipo de valor de una propiedad adjunta personalizada afecta al uso, a la definición, o a ambos.

El tipo de valor de la propiedad adjunta se declara en varios lugares: en las firmas de ambos métodos de descriptor de acceso **Get** y **Set**; y como parámetro *propertyType* de la llamada a [**RegisterAttached**](https://msdn.microsoft.com/library/windows/apps/hh701833). El tipo de valor más común para las propiedades adjuntas (personalizadas u otras) es una cadena simple. El motivo es que, por lo general, las propiedades adjuntas suelen estar diseñadas para el uso de atributos XAML, y usar una cadena como tipo de valor hace que las propiedades sean ligeras. Otros tipos de valor comunes para las propiedades adjuntas son primitivos que tienen conversión nativa a métodos de cadena, como int, double o un valor de enumeración. También puedes usar otros tipos de valor, que no admitan la conversión nativa a cadena, como valor de propiedad adjunta.

- Sin embargo, esto supone elegir entre uso o implementación: Puedes dejar la propiedad adjunta tal cual, pero solo admitirá el uso si la propiedad adjunta es un elemento de propiedad y el valor se declara como un elemento de objeto. En este caso, el tipo de propiedad no tiene que admitir el uso de XAML como un elemento de objeto.
- Para las clases de referencia existentes de Windows en tiempo de ejecución, comprueba que la sintaxis XAML del tipo admite el uso de elementos de objeto XAML.

## Puedes dejar la propiedad adjunta tal cual, pero úsala únicamente en un uso de atributos mediante una técnica de referencia de XAML, como **Binding** o **StaticResource**, que se pueda expresar como una cadena.

Más información sobre el ejemplo **Canvas.Left** En anteriores ejemplos del uso de propiedades adjuntas, te mostramos distintas formas de establecer la propiedad adjunta [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/hh759771). ¿Pero en qué modo afecta a la interacción de un [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267) con tu objeto y cuándo se produce?

Examinaremos este ejemplo concreto con más detenimiento, ya que si implementas una propiedad adjunta, es interesante ver qué otra cosa intenta realizar una clase de propietario de propiedad adjunta típica con sus valores de propiedad adjunta si los encuentra en otros objetos. La función principal de un [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267) es ser un contenedor de diseño con una posición absoluta en la interfaz de usuario. Los elementos secundarios de un **Canvas** se almacenan en una propiedad [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514) definida por la clase base. De todos los paneles, **Canvas** es el único que usa el posicionamiento absoluto. El modelo de objetos del tipo [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) común se habría expandido para agregar propiedades que podrían ser un problema solamente para **Canvas** y esos casos de **UIElement** concretos en los que son elementos secundarios de **UIElement**.

Definir las propiedades de control de diseño de un **Canvas** para que sean propiedades adjuntas que cualquier **UIElement** pueda usar contribuye a que el modelo de objetos sea más limpio. Para que sea un panel práctico, [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267) tiene un comportamiento que invalida los métodos [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) y [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914) de nivel de marco. Este es el lugar donde **Canvas** realmente comprueba si hay valores de propiedades adjuntas en sus elementos secundarios. Parte de los patrones **Measure** y **Arrange** es un bucle que itera en cualquier contenido, y un panel tiene la propiedad [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514) que convierte en explícito lo que supuestamente debe considerarse el elemento secundario de un panel. De esta forma, el comportamiento de diseño de **Canvas** itera en estos elementos secundarios y realiza llamaras estáticas a [**Canvas.GetLeft**](https://msdn.microsoft.com/library/windows/apps/br209269) y [**Canvas.GetTop**](https://msdn.microsoft.com/library/windows/apps/br209270) en cada elemento secundario para ver si esas propiedades adjuntas contienen un valor no predeterminado (el valor predeterminado es 0).

Estos valores se usan entonces para el posicionamiento absoluto de cada elemento secundario en el espacio de diseño disponible de **Canvas**, según los valores específicos proporcionados por cada elemento secundario y confirmados mediante **Arrange**.

``` syntax
    protected override Size ArrangeOverride(Size finalSize)
    {
        foreach (UIElement child in Children)
        {
            double x = (double) Canvas.GetLeft(child);
            double y = (double) Canvas.GetTop(child);
            child.Arrange(new Rect(new Point(x, y), child.DesiredSize));
        }
        return base.ArrangeOverride(finalSize); 
        // real Canvas has more sophisticated sizing
    }
```

El código tiene un aspecto similar a este seudocódigo:

## 
            **Nota**  Para obtener más información sobre el funcionamiento de los paneles, consulta [Introducción a los paneles personalizados de XAML](https://msdn.microsoft.com/library/windows/apps/mt228351).

* [**Temas relacionados**](https://msdn.microsoft.com/library/windows/apps/hh701833)
* [RegisterAttached](attached-properties-overview.md)
* [Introducción a las propiedades adjuntas](custom-dependency-properties.md)
* [Propiedades de dependencia personalizadas](xaml-overview.md)




<!--HONumber=Jun16_HO4-->


