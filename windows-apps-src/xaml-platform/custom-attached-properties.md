---
author: jwmsft
description: Se explica cómo implementar una propiedad adjunta de XAML como una propiedad de dependencia y cómo definir la convención de descriptor de acceso necesaria para que la propiedad adjunta se pueda usar en XAML.
title: Propiedades adjuntas personalizadas
ms.assetid: E9C0C57E-6098-4875-AA3E-9D7B36E160E0
ms.author: jimwalk
ms.date: 07/18/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cppwinrt
- cpp
ms.openlocfilehash: de2d319f536b2c9d25d53e0b1ec2886831c66e12
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2018
ms.locfileid: "5937608"
---
# <a name="custom-attached-properties"></a>Propiedades adjuntas personalizadas

Una *propiedad adjunta* es un concepto de XAML. Las propiedades adjuntas suelen definirse como una forma especializada de propiedad de dependencia. En este tema se explica cómo implementar una propiedad adjunta de XAML como una propiedad de dependencia y cómo definir la convención de descriptor de acceso necesaria para que la propiedad adjunta se pueda usar en XAML.

## <a name="prerequisites"></a>Requisitos previos

Damos por hecho que conoces las propiedades de dependencia desde el punto de vista de un usuario de propiedades de dependencia existentes y, asimismo, que has leído la [Introducción a las propiedades de dependencia](dependency-properties-overview.md). También tienes que haber leído la [Introducción a las propiedades adjuntas](attached-properties-overview.md). Para seguir los ejemplos de este tema, también debes conocer XAML y saber cómo escribir una aplicación básica de Windows en tiempo de ejecución con C++, C# o Visual Basic.

## <a name="scenarios-for-attached-properties"></a>Escenarios para propiedades adjuntas

Puedes crear una propiedad adjunta cuando haya motivos para que las clases que no sean clases definidoras tengan un mecanismo de establecimiento de propiedades. Los escenarios más habituales para esto son diseño y servicios. Algunos ejemplos de propiedades de diseño existentes son [**Canvas.ZIndex**](https://msdn.microsoft.com/library/windows/apps/hh759773) y [**Canvas.Top**](https://msdn.microsoft.com/library/windows/apps/hh759772). En un escenario de diseño, los elementos que existen como elementos secundarios de elementos de control de diseño pueden expresar los requisitos de diseño a sus elementos primarios individualmente, cada uno de los cuales establece un valor de propiedad que el elemento primario define como propiedad adjunta. Un ejemplo de escenario de servicios en la API de Windows Runtime es establecer las propiedades adjuntas de [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/br209527), como [**ScrollViewer.IsZoomChainingEnabled**](https://msdn.microsoft.com/library/windows/apps/br209561).

> [!WARNING]
> Una limitación de la implementación de XAML de Windows Runtime es que no puedes animar las propiedades adjuntas personalizadas.

## <a name="registering-a-custom-attached-property"></a>Registro de una propiedad adjunta personalizada

Si vas a definir la propiedad adjunta para usarla exclusivamente en otros tipos, la clase donde se registra la propiedad no tiene que derivar de [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356). No obstante, si sigues el modelo típico para hacer que tu propiedad adjunta sea también una propiedad de dependencia, debes hacer que el parámetro de destino de los descriptores de acceso use **DependencyObject** con el fin de poder usar la memoria auxiliar de propiedades.

Para definir tu propiedad adjunta como una propiedad de dependencia, declara una propiedad **public** **static** **readonly** del tipo [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362). Esta propiedad se define usando el valor de retorno del método [**RegisterAttached**](https://msdn.microsoft.com/library/windows/apps/hh701833). El nombre de la propiedad debe coincidir con el nombre de la propiedad adjunta que especificaste como parámetro *name* de **RegisterAttached**, con la cadena "Property" anexada al final. Esta es la convención establecida para asignar nombre a los identificadores de las propiedades de dependencia en relación con las propiedades que representan.

La principal diferencia entre una propiedad adjunta personalizada y una propiedad de dependencia personalizada es la manera de definir los descriptores de acceso o contenedores. En lugar de usar la técnica de contenedor que se describe en las [Propiedades de dependencia personalizadas](custom-dependency-properties.md), también debes proporcionar estático **obtener *** PropertyName* y **establecer *** PropertyName* estáticos como descriptores de acceso para la propiedad adjunta. Los descriptores de acceso son usados principalmente por el analizador XAML, aunque algunos otros llamadores pueden usarlos para establecer valores en escenarios que no sean de XAML.

> [!IMPORTANT]
> Si no defines correctamente los descriptores de acceso, el procesador XAML no puede acceder a la propiedad adjunta y cualquier persona que intente usarla obtendrá un error del analizador XAML. Además, las herramientas de diseño y codificación suelen usar las convenciones "\*Property" para asignar un nombre a los identificadores cuando encuentran una propiedad de dependencia personalizada en un ensamblado al que se hace referencia.

## <a name="accessors"></a>Descriptores de acceso

La firma del descriptor de acceso **Get**_PropertyName_ debe ser esta:

`public static` _valueType_ **Get**_PropertyName_ `(DependencyObject target)`

En Microsoft Visual Basic, es esta:

`Public Shared Function Get`_PropertyName_`(ByVal target As DependencyObject) As `_valueType_`)`

El objeto *target* puede ser de un tipo más específico en tu implementación, pero debe derivar de [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356). El valor de retorno de *valueType* también puede ser de un tipo más específico en tu implementación. El tipo **Object** básico es aceptable, pero con frecuencia querrás que la propiedad adjunta exija seguridad de tipos. El uso de establecimiento de tipos en las firmas getter y setter es una técnica de seguridad de tipos recomendada.

La firma para la **establecer *** PropertyName* descriptor de acceso debe ser esta.

`public static void Set`_PropertyName_` (DependencyObject target , `_valueType_` value)`

En Visual Basic, es esta:

`Public Shared Sub Set`_PropertyName_` (ByVal target As DependencyObject, ByVal value As `_valueType_`)`

El objeto *target* puede ser de un tipo más específico en tu implementación, pero debe derivar de [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356). El objeto *value* y su *valueType* pueden ser de un tipo más específico en tu implementación. Recuerda que el valor de este método es la entrada que procede del procesador XAML cuando encuentra tu propiedad adjunta en el marcado. Debe haber conversión de tipos o compatibilidad existente para extensión de marcado para el tipo que uses, de manera que se pueda crear el tipo apropiado a partir del valor de un atributo (que al final es tan solo una cadena). El tipo **Object** básico es aceptable, pero con frecuencia te interesará una mayor seguridad de tipos. Para ello, pon la aplicación de tipos en los accesorios.

> [!NOTE]
> También es posible definir una propiedad adjunta que el uso previsto sea mediante sintaxis de elemento de propiedad. En tal caso, no necesita la conversión de tipos para los valores, pero sí que debe asegurarse de que los valores que tiene previstos se puedan construir en XAML. [**VisualStateManager.VisualStateGroups**](https://msdn.microsoft.com/library/windows/apps/hh738505) es un ejemplo de una propiedad adjunta existente que solo admite el uso de elementos de propiedad.

## <a name="code-example"></a>Ejemplo de código

Este ejemplo muestra el registro de la propiedad de dependencia (usando el método [**RegisterAttached**](https://msdn.microsoft.com/library/windows/apps/hh701833)), así como los descriptores de acceso **Get** y **Set** para una propiedad adjunta personalizada. En el ejemplo, el nombre de la propiedad adjunta es `IsMovable`. Por consiguiente, los descriptores de acceso deben denominarse `GetIsMovable` y `SetIsMovable`. El propietario de la propiedad adjunta es una clase de servicio denominada `GameService` que no tiene una interfaz de usuario propia; su objetivo es solo proporcionar los servicios de la propiedad adjunta cuando se use la propiedad adjunta **GameService.IsMovable**.

Definir la propiedad adjunta en C++ / CX es un poco más complejo. Tienes que decidir cómo repartir entre el archivo de encabezado y de código. Además, debes exponer el identificador como una propiedad con solo un descriptor de acceso **get**, por los motivos tratados en [Propiedades de dependencia personalizadas](custom-dependency-properties.md). En C++ / CX, debes definir esta relación de campo de la propiedad explícitamente en lugar de confiar en las palabras clave de .NET **readonly** e implícito de respaldo de propiedades simple. También tienes que registrar la propiedad adjunta dentro de una función auxiliar que solo se ejecute una vez: cuando se inicie la aplicación por primera vez pero antes de que se cargue cualquier página XAML que necesite la propiedad adjunta. La ubicación típica para llamar a las funciones auxiliares de registro de propiedades para cualquier propiedad de dependencia o adjunta es desde el constructor **App** / [**Application**](https://msdn.microsoft.com/library/windows/apps/br242325) en el código del archivo app.xaml.

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

```cppwinrt
// GameService.idl
namespace UserAndCustomControls
{
    runtimeclass GameService : Windows.UI.Xaml.DependencyObject
    {
        GameService();
        static Windows.UI.Xaml.DependencyProperty IsMovableProperty{ get; };
        Boolean IsMovable;
    }
}

// GameService.h
...
    bool IsMovable(){ return winrt::unbox_value<bool>(GetValue(m_IsMovableProperty)); }
    void IsMovable(bool value){ SetValue(m_IsMovableProperty, winrt::box_value(value)); }
    Windows::UI::Xaml::DependencyProperty IsMovableProperty(){ return m_IsMovableProperty; }

private:
    static Windows::UI::Xaml::DependencyProperty m_IsMovableProperty;
...

// GameService.cpp
...
Windows::UI::Xaml::DependencyProperty GameService::m_IsMovableProperty =
    Windows::UI::Xaml::DependencyProperty::RegisterAttached(
        L"IsMovable",
        winrt::xaml_typename<bool>(),
        winrt::xaml_typename<UserAndCustomControls::GameService>(),
        Windows::UI::Xaml::PropertyMetadata{ winrt::box_value(false) }
);
...
```

```cpp
// GameService.h
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

// GameService.cpp
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

GameService::GameService() {};

GameService::RegisterDependencyProperties() {
    DependencyProperty^ GameService::_IsMovableProperty = DependencyProperty::RegisterAttached(
         "IsMovable", Platform::Boolean::typeid, GameService::typeid, ref new PropertyMetadata(false));
}
```

## <a name="using-your-custom-attached-property-in-xaml"></a>Uso de la propiedad adjunta personalizada en XAML

Después de definir la propiedad adjunta e incluir sus miembros de soporte como parte de un tipo personalizado, debes hacer que las definiciones estén disponibles para el uso de XAML. Para ello, debes asignar un espacio de nombres XAML que hará referencia al espacio de nombres del código que contiene la clase relevante. Si has definido la propiedad adjunta como parte de una biblioteca, debes incluir dicha biblioteca en el paquete de la aplicación.

Normalmente, la asignación de un espacio de nombres XML para XAML se coloca en el elemento raíz de una página XAML. Por ejemplo, para la clase `GameService` en el espacio de nombres `UserAndCustomControls`, que contiene las definiciones de propiedad adjunta que se muestran en los fragmentos anteriores, la asignación podría tener el siguiente aspecto.

```xaml
<UserControl
  xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
  xmlns:uc="using:UserAndCustomControls"
  ... >
```

Mediante la asignación puedes establecer la propiedad adjunta `GameService.IsMovable` en cualquier elemento que coincida con tu definición de destino, incluido un tipo existente definido por Windows Runtime.

```xaml
<Image uc:GameService.IsMovable="True" .../>
```

Si estableces la propiedad en un elemento que también está en el mismo espacio de nombres XML asignado, también debes incluir el prefijo en el nombre de la propiedad adjunta. El motivo es que el prefijo cualifica el tipo de propietario. Aunque, según las reglas normales de XML, los atributos pueden heredar de los elementos el espacio de nombres, no se puede dar por sentado que el atributo de la propiedad adjunta está en el mismo espacio de nombres XML que el elemento donde se incluye el atributo. Por ejemplo, si estableces `GameService.IsMovable` en un tipo personalizado de `ImageWithLabelControl` (no se muestra la definición) y aunque ambos se definieran en el mismo espacio de nombres de código asignado al mismo prefijo, el XAML seguiría siendo este.

```xaml
<uc:ImageWithLabelControl uc:GameService.IsMovable="True" .../>
```

> [!NOTE]
> Si estás escribiendo una UI de XAML con C++, debes incluir el encabezado para el tipo personalizado que define la propiedad adjunta, cada vez que una página XAML use ese tipo. Todas las páginas XAML tienen asociado un encabezado .xaml.h de código subyacente. Aquí es donde debes incluir (mediante **\#include**) el encabezado para la definición del tipo de propietario de la propiedad adjunta.

## <a name="value-type-of-a-custom-attached-property"></a>Tipo de valor de una propiedad adjunta personalizada

El tipo que se use como tipo de valor de una propiedad adjunta personalizada afecta al uso, a la definición, o a ambos. El tipo de valor de la propiedad adjunta se declara en varios lugares: en las firmas de ambos métodos de descriptor de acceso **Get** y **Set**; y como parámetro *propertyType* de la llamada a [**RegisterAttached**](https://msdn.microsoft.com/library/windows/apps/hh701833).

El tipo de valor más común para las propiedades adjuntas (personalizadas u otras) es una cadena simple. El motivo es que, por lo general, las propiedades adjuntas suelen estar diseñadas para el uso de atributos XAML, y usar una cadena como tipo de valor hace que las propiedades sean ligeras. Otros tipos de valor comunes para las propiedades adjuntas son primitivos que tienen conversión nativa a métodos de cadena, como int, double o un valor de enumeración. También puedes usar otros tipos de valor, que no admitan la conversión nativa a cadena, como valor de propiedad adjunta. Sin embargo, esto supone elegir entre uso o implementación:

- Puedes dejar la propiedad adjunta tal cual, pero solo admitirá el uso si la propiedad adjunta es un elemento de propiedad y el valor se declara como un elemento de objeto. En este caso, el tipo de propiedad no tiene que admitir el uso de XAML como un elemento de objeto. Para las clases de referencia existentes de Windows en tiempo de ejecución, comprueba que la sintaxis XAML del tipo admite el uso de elementos de objeto XAML.
- Puedes dejar la propiedad adjunta tal cual, pero úsala únicamente en un uso de atributos mediante una técnica de referencia de XAML, como **Binding** o **StaticResource**, que se pueda expresar como una cadena.

## <a name="more-about-the-canvasleft-example"></a>Más información sobre el ejemplo **Canvas.Left**

En anteriores ejemplos del uso de propiedades adjuntas, te mostramos distintas formas de establecer la propiedad adjunta [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/hh759771). ¿Pero en qué modo afecta a la interacción de un [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267) con tu objeto y cuándo se produce? Examinaremos este ejemplo concreto con más detenimiento, ya que si implementas una propiedad adjunta, es interesante ver qué otra cosa intenta realizar una clase de propietario de propiedad adjunta típica con sus valores de propiedad adjunta si los encuentra en otros objetos.

La función principal de un [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267) es ser un contenedor de diseño con una posición absoluta en la interfaz de usuario. Los elementos secundarios de un **Canvas** se almacenan en una propiedad [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514) definida por la clase base. De todos los paneles, **Canvas** es el único que usa el posicionamiento absoluto. El modelo de objetos del tipo [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) común se habría expandido para agregar propiedades que podrían ser un problema solamente para **Canvas** y esos casos de **UIElement** concretos en los que son elementos secundarios de **UIElement**. Definir las propiedades de control de diseño de un **Canvas** para que sean propiedades adjuntas que cualquier **UIElement** pueda usar contribuye a que el modelo de objetos sea más limpio.

Para que sea un panel práctico, [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267) tiene un comportamiento que invalida los métodos [**Measure**](https://msdn.microsoft.com/library/windows/apps/br208952) y [**Arrange**](https://msdn.microsoft.com/library/windows/apps/br208914) de nivel de marco. Este es el lugar donde **Canvas** realmente comprueba si hay valores de propiedades adjuntas en sus elementos secundarios. Parte de los patrones **Measure** y **Arrange** es un bucle que itera en cualquier contenido, y un panel tiene la propiedad [**Children**](https://msdn.microsoft.com/library/windows/apps/br227514) que convierte en explícito lo que supuestamente debe considerarse el elemento secundario de un panel. De esta forma, el comportamiento de diseño de **Canvas** itera en estos elementos secundarios y realiza llamaras estáticas a [**Canvas.GetLeft**](https://msdn.microsoft.com/library/windows/apps/br209269) y [**Canvas.GetTop**](https://msdn.microsoft.com/library/windows/apps/br209270) en cada elemento secundario para ver si esas propiedades adjuntas contienen un valor no predeterminado (el valor predeterminado es 0). Estos valores se usan entonces para el posicionamiento absoluto de cada elemento secundario en el espacio de diseño disponible de **Canvas**, según los valores específicos proporcionados por cada elemento secundario y confirmados mediante **Arrange**.

El código tiene el aspecto similar a este seudocódigo.

```syntax
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

> [!NOTE]
> Para obtener más información sobre el funcionan de los paneles, consulta la [información general de los paneles personalizados de XAML](https://msdn.microsoft.com/library/windows/apps/mt228351).

## <a name="related-topics"></a>Temas relacionados

* [**RegisterAttached**](https://msdn.microsoft.com/library/windows/apps/hh701833)
* [Introducción a las propiedades adjuntas](attached-properties-overview.md)
* [Propiedades de dependencia personalizadas](custom-dependency-properties.md)
* [Introducción a XAML](xaml-overview.md)
