---
author: jwmsft
description: Se explica cómo definir e implementar las propiedades de dependencia personalizadas para una aplicación de Windows Runtime con C++, C# o Visual Basic.
title: Propiedades de dependencia personalizadas
ms.assetid: 5ADF7935-F2CF-4BB6-B1A5-F535C2ED8EF8
ms.author: jimwalk
ms.date: 07/12/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cppwinrt
- cpp
ms.openlocfilehash: 3b52c801ffcf1cfe916e41cfd102a37397fadfee
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/19/2018
ms.locfileid: "7279980"
---
# <a name="custom-dependency-properties"></a>Propiedades de dependencia personalizadas

Aquí te explicamos la manera de definir e implementar tus propias propiedades de dependencia para una aplicación de Windows Runtime con C++, C# o Visual Basic. Enumeramos los motivos por los que los desarrolladores de aplicaciones y los creadores de componentes podrían querer crear propiedades personalizadas. También describimos los pasos para implementar una propiedad de dependencia personalizada, así como los procedimientos recomendados que pueden mejorar el rendimiento, la capacidad de uso o la versatilidad de la propiedad de dependencia.

## <a name="prerequisites"></a>Requisitos previos

Damos por sentado que has leído la [introducción a las propiedades de dependencia](dependency-properties-overview.md) y que conoces las propiedades de dependencia desde la perspectiva de un usuario de propiedades de dependencia existentes. Para seguir los ejemplos de este tema, debes conocer XAML y saber cómo crear una aplicación básica de Windows Runtime con C++, C# o Visual Basic.

## <a name="what-is-a-dependency-property"></a>¿Qué es una propiedad de dependencia?

Para admitir estilos, enlaces de datos, animaciones y valores predeterminados para una propiedad, se deberían implementar como propiedad de dependencia. Los valores de propiedad de dependencia no se almacenan como campos en la clase, los almacena el marco xaml y se hace referencia a ellos con una clave que se recupera cuando la propiedad se registra con el sistema de propiedades de Windows Runtime mediante una llamada al método [**DependencyProperty.Register**](https://msdn.microsoft.com/library/windows/apps/hh701829).   Solo pueden usar las propiedades de dependencia los tipos que deriven de [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356), aunque **DependencyObject** está en una posición bastante alta de la jerarquía de clases, por lo que la mayoría de las clases diseñadas para soporte de interfaz de usuario y de presentación pueden admitir las propiedades de dependencia. Para obtener más información acerca de las propiedades de dependencia y alguna de la terminología y las convenciones usadas para describirlas en esta documentación, consulta [Introducción a las propiedades de dependencia](dependency-properties-overview.md).

Algunos ejemplos de las propiedades de dependencia en Windows Runtime son [**Control.Background**](https://msdn.microsoft.com/library/windows/apps/br209395), [**FrameworkElement.Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) y [**TextBox.Text**](https://msdn.microsoft.com/library/windows/apps/br209702), entre otros muchos.

La convención es que cada propiedad de dependencia expuesta por una clase tiene una propiedad **public static readonly** correspondiente del tipo [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) que se expone en esa misma clase que proporciona el identificador de la propiedad de dependencia. El nombre del identificador sigue esta convención: el nombre de la propiedad de dependencia, con la cadena "Property" agregada al final del nombre. Por ejemplo, el identificador **DependencyProperty** correspondiente de la propiedad **Control.Background** es [**Control.BackgroundProperty**](https://msdn.microsoft.com/library/windows/apps/br209396). El identificador almacena la información acerca de la propiedad de dependencia con la que se registró, y se puede usar para otras operaciones que implican a la propiedad de dependencia, como llamar a [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361).

## <a name="property-wrappers"></a>Contenedores de propiedades

Normalmente, las propiedades de dependencia tienen una implementación de contenedor. Sin el contenedor, la única manera de obtener o establecer las propiedades sería usar los métodos de utilidad de propiedades de dependencia [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359) y [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361), y pasar el identificador como un parámetro. Este es un uso bastante forzado para algo que evidentemente es una propiedad. Pero con el contenedor, tu código y cualquier otro código que haga referencia a la propiedad de dependencia puede usar una sintaxis objeto-propiedad directa que resulte natural para el lenguaje que estés usando.

Si implementas tú mismo una propiedad de dependencia personalizada y quieres que sea pública y fácil de llamar, define también los contenedores de la propiedad. Los contenedores también resultan útiles para comunicar información básica sobre la propiedad de dependencia a procesos de análisis estáticos o de reflexión. Específicamente, el contenedor es el lugar donde se colocan atributos como [**ContentPropertyAttribute**](https://msdn.microsoft.com/library/windows/apps/br228011).

## <a name="when-to-implement-a-property-as-a-dependency-property"></a>Cuándo implementar una propiedad como propiedad de dependencia

Siempre que implementes una propiedad de lectura y escritura en una clase, si esta deriva de [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356), tienes la opción de hacer que tu propiedad funcione como propiedad de dependencia. Algunas veces, la técnica habitual de respaldar la propiedad con un campo privado es adecuada. No siempre es necesario o apropiado definir tu propiedad personalizada como propiedad de dependencia. La elección dependerá de los escenarios que quieras que admita tu propiedad.

Podrías considerar implementarla como propiedad de dependencia cuando quieras que admita una o varias de estas características de Windows Runtime o de las aplicaciones de Windows Runtime:

- Establecer la propiedad mediante [**Style**](https://msdn.microsoft.com/library/windows/apps/br208849)
- Actuar como propiedad de destino válida para enlace de datos con [**{Binding}**](binding-markup-extension.md)
- Admitir valores animados mediante [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/br210490)
- Notificar el momento en que el valor de la propiedad ha sido modificado por:
  - Acciones llevadas a cabo por el propio sistema de propiedades
  - El entorno
  - Acciones del usuario
  - Estilos de lectura y escritura

## <a name="checklist-for-defining-a-dependency-property"></a>Lista de comprobación para definir una propiedad de dependencia

La definición de una propiedad de dependencia se puede considerar un conjunto de conceptos. Estos conceptos no son necesariamente pasos de procedimientos, porque varios de ellos pueden abordarse en una única línea de código en la implementación. Esta lista ofrece solo una introducción rápida. Explicaremos cada concepto con más detalle más adelante en este tema y mostraremos ejemplos de código en varios lenguajes.

- Registra el nombre de la propiedad en el sistema de la propiedad (llama a [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829)), especificando un tipo propietario y el tipo del valor de la propiedad.
  - Un parámetro requerido por [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) espera metadatos de propiedad. Especifica **null** para ello o, si quieres un comportamiento modificado por la propiedad o un valor predeterminado basado en metadatos que pueda restaurarse mediante una llamada a [**ClearValue**](https://msdn.microsoft.com/library/windows/apps/br242357), especifica una instancia de [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.propertymetadata).
- Definir un identificador [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) como una propiedad **public static readonly** en el tipo propietario.
- Definir una propiedad de contenedor, siguiendo el modelo de descriptor de acceso de propiedad usado en el lenguaje que estés implementando. El nombre de la propiedad de contenedor debe coincidir con la cadena *name* que usaste en [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829). Implementa los descriptores de acceso **get** y **set** para conectar el contenedor con la propiedad de dependencia que contiene; para ello, llama a [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359) y [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361) y pasa tu propio identificador de propiedad como un parámetro.
- (Opcional) Colocar atributos como [**ContentPropertyAttribute**](https://msdn.microsoft.com/library/windows/apps/br228011) en el contenedor.

> [!NOTE]
> Si vas a definir una propiedad adjunta personalizada, normalmente omites el contenedor. En su lugar, escribes un estilo diferente de descriptor de acceso que un procesador XAML puede usar. Consulta [Propiedades adjuntas personalizadas](custom-attached-properties.md). 

## <a name="registering-the-property"></a>Registro de la propiedad

Para que tu propiedad sea una propiedad de dependencia, debes registrarla en un almacén de propiedades mantenido por el sistema de propiedades de Windows Runtime.  Para registrar la propiedad, llama al método [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829).

En los lenguajes de Microsoft .NET (C# y Microsoft Visual Basic), llama a [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) en el cuerpo de la clase (dentro de la clase, pero fuera de las definiciones de miembro). La llamada al método [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) proporciona este identificador como valor de retorno. La llamada a [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) se realiza normalmente como un constructor estático o como parte de la inicialización de una propiedad **public static readonly** de tipo [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) como parte de la clase. Esta propiedad expone el identificador de tu propiedad de dependencia. Estos son algunos ejemplos de la llamada a [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829).

> [!NOTE]
> Registrar la propiedad de dependencia como parte del identificador de la definición de la propiedad es la implementación típica, pero también puede registrar una propiedad de dependencia en el constructor estático de la clase. Este enfoque tiene sentido si necesitas más de una línea de código para inicializar la propiedad de dependencia.

Para C++ / CX, tienes opciones para dividir la implementación entre el encabezado y el archivo de código. La división típica consiste en declarar el propio identificador como propiedad **public static** en el encabezado con una implementación de **get**, pero sin **set**. La implementación **get** hace referencia a un campo privado, que es una instancia de [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) sin inicializar. También puedes declarar los contenedores y las implementaciones **get** y **set** del contenedor. En este caso, el encabezado incluye una implementación mínima. Si el contenedor necesita atribución de Windows Runtime, inclúyela también en el encabezado. Pon la llamada de [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) en el archivo de código, dentro de la función auxiliar que solo se ejecuta cuando la aplicación se inicializa por primera vez. Usa el valor devuelto de **Register** para rellenar los identificadores estáticos pero sin inicializar que has declarado en el encabezado, que estableciste inicialmente en **nullptr**, en el ámbito raíz del archivo de implementación.

```csharp
public static readonly DependencyProperty LabelProperty = DependencyProperty.Register(
  "Label",
  typeof(String),
  typeof(ImageWithLabelControl),
  new PropertyMetadata(null)
);
```

```vb
Public Shared ReadOnly LabelProperty As DependencyProperty = 
    DependencyProperty.Register("Label", 
      GetType(String), 
      GetType(ImageWithLabelControl), 
      New PropertyMetadata(Nothing))
```

```cppwinrt
// ImageWithLabelControl.idl
namespace ImageWithLabelControlApp
{
    runtimeclass ImageWithLabelControl : Windows.UI.Xaml.Controls.Control
    {
        ImageWithLabelControl();
        static Windows.UI.Xaml.DependencyProperty LabelProperty{ get; };
        String Label;
    }
}

// ImageWithLabelControl.h
...
private:
    static Windows::UI::Xaml::DependencyProperty m_labelProperty;
...

// ImageWithLabelControl.cpp
...
Windows::UI::Xaml::DependencyProperty ImageWithLabelControl::m_labelProperty =
    Windows::UI::Xaml::DependencyProperty::Register(
        L"Label",
        winrt::xaml_typename<winrt::hstring>(),
        winrt::xaml_typename<ImageWithLabelControlApp::ImageWithLabelControl>(),
        Windows::UI::Xaml::PropertyMetadata{ nullptr }
);
...
```

```cpp
//.h file
//using namespace Windows::UI::Xaml::Controls;
//using namespace Windows::UI::Xaml::Interop;
//using namespace Windows::UI::Xaml;
//using namespace Platform;

public ref class ImageWithLabelControl sealed : public Control
{
private:
    static DependencyProperty^ _LabelProperty;
...
public:
    static void RegisterDependencyProperties();
    static property DependencyProperty^ LabelProperty
    {
        DependencyProperty^ get() {return _LabelProperty;}
    }
...
};

//.cpp file
using namespace Windows::UI::Xaml;
using namespace Windows::UI::Xaml.Interop;

DependencyProperty^ ImageWithLabelControl::_LabelProperty = nullptr;

// This function is called from the App constructor in App.xaml.cpp
// to register the properties
void ImageWithLabelControl::RegisterDependencyProperties()
{ 
    if (_LabelProperty == nullptr)
    { 
        _LabelProperty = DependencyProperty::Register(
          "Label", Platform::String::typeid, ImageWithLabelControl::typeid, nullptr);
    } 
}
```

> [!NOTE]
> Para C++ / CX en el código, la razón para tener un campo privado y una propiedad pública de solo lectura que superficies [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) es para que otros llamadores que usan tu propiedad de dependencia también pueden usar la utilidad del sistema de propiedades API que requieren la identificador sea público. Si haces que el identificador sea privado, los usuarios no podrán usar estas API de utilidad. Algunos ejemplos de estas API y escenarios son [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359) o [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361) a elegir, [**ClearValue**](https://msdn.microsoft.com/library/windows/apps/br242357), [**GetAnimationBaseValue**](https://msdn.microsoft.com/library/windows/apps/br242358), [**SetBinding**](https://msdn.microsoft.com/library/windows/apps/br244257) y [**Setter.Property**](https://msdn.microsoft.com/library/windows/apps/br208836). No puedes usar un campo público para esto porque las reglas de metadatos de Windows Runtime no admiten campos públicos.

## <a name="dependency-property-name-conventions"></a>Convenciones de nomenclatura para propiedades de dependencia

Existen convenciones de nomenclatura para las propiedades de dependencia; síguelas siempre, salvo en circunstancias excepcionales. La propiedad de dependencia en sí tiene un nombre básico ("Label" en el ejemplo anterior) que se proporciona como el primer parámetro de [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829). El nombre debe ser único en cada tipo de registro, y este requisito también se aplica a todos los miembros heredados. Las propiedades de dependencia heredadas mediante tipos base ya se consideran parte del tipo de registro; los nombres de las propiedades heredadas no se pueden volver a registrar.

> [!WARNING]
> Aunque el nombre que proporciones que aquí puede ser cualquier identificador de cadena que es válido en la programación del lenguaje que uses, normalmente quieres poder establecer tu propiedad de dependencia en XAML. Para poder establecerse en XAML, el nombre de propiedad que elijas debe ser un nombre XAML válido. Para obtener más información, consulta [Introducción a XAML](xaml-overview.md).

Cuando crees la propiedad de identificador, combina el nombre de la propiedad tal y como lo registraste con el sufijo "Property" ("LabelProperty", por ejemplo). Esta propiedad es el identificador de la propiedad de dependencia y se usa como entrada para las llamadas a [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361) y [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359) que hagas en tus propios contenedores de propiedad. También lo usa el sistema de propiedades y los procesadores XAML como [**{x:Bind}**](x-bind-markup-extension.md)

## <a name="implementing-the-wrapper"></a>Implementación del contenedor

Tu contenedor de propiedad debe llamar a [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359) en la implementación **get** y a [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361) en la implementación **set**.

> [!WARNING]
> En circunstancias excepcionales, las implementaciones del contenedor deben realizar las operaciones [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359) y [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361) . De lo contrario, el comportamiento cuando la propiedad se establece mediante XAML será diferente de cuando se establece mediante código. Por motivos de eficacia, el analizador XAML omite los contenedores al establecer las propiedades de dependencia; se comunica con la memoria auxiliar a través de **SetValue**.

```csharp
public String Label
{
    get { return (String)GetValue(LabelProperty); }
    set { SetValue(LabelProperty, value); }
}
```

```vb
Public Property Label() As String
    Get
        Return DirectCast(GetValue(LabelProperty), String) 
    End Get 
    Set(ByVal value As String)
        SetValue(LabelProperty, value)
    End Set
End Property
```

```cppwinrt
// ImageWithLabelControl.h
...
winrt::hstring Label()
{
    return winrt::unbox_value<winrt::hstring>(GetValue(m_labelProperty));
}

void Label(winrt::hstring const& value)
{
    SetValue(m_labelProperty, winrt::box_value(value));
}
...
```

```cpp
//using namespace Platform;
public:
...
  property String^ Label
  {
    String^ get() {
      return (String^)GetValue(LabelProperty);
    }
    void set(String^ value) {
      SetValue(LabelProperty, value);
    }
  }
```

## <a name="property-metadata-for-a-custom-dependency-property"></a>Metadatos de propiedad para una propiedad de dependencia personalizada

Cuando se asignan metadatos de propiedad a una propiedad de dependencia, se aplican los mismos metadatos a dicha propiedad en cada instancia del tipo propietario de la propiedad o sus subclases. En los metadatos de propiedad, puedes especificar dos comportamientos:

- Un valor predeterminado que el sistema de propiedades asigna a todas las clases de la propiedad.
- Un método de devolución de llamada estático que se invoca automáticamente en el sistema de propiedades siempre que se detecta un cambio de valor de propiedad.

### <a name="calling-register-with-property-metadata"></a>Llamada al Registro con metadatos de propiedad

En los anteriores ejemplos de llamada a [**DependencyProperty.Register**](https://msdn.microsoft.com/library/windows/apps/hh701829), pasamos un valor nulo para el parámetro *propertyMetadata*. Para habilitar una propiedad de dependencia para que proporcione un valor predeterminado o use una devolución de llamada modificada por propiedades, deberás definir una instancia de [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771) que proporcione una de estas funcionalidades o las dos.

Normalmente deberás proporcionar [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771) como instancia creada en línea, dentro de los parámetros de [**DependencyProperty.Register**](https://msdn.microsoft.com/library/windows/apps/hh701829).

> [!NOTE]
> Si vas a definir una implementación [**CreateDefaultValueCallback**](https://msdn.microsoft.com/library/windows/apps/hh701812) , debes usar el método de utilidad [**PropertyMetadata.Create**](https://msdn.microsoft.com/library/windows/apps/hh702099) en lugar de llamar a un constructor [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771) para definir la instancia **PropertyMetadata** .

El siguiente ejemplo modifica los ejemplos mostrados anteriormente [**DependencyProperty.Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) al hacer referencia a una instancia de [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771) con un valor de [**PropertyChangedCallback**](https://msdn.microsoft.com/library/windows/apps/br208770). La implementación de la devolución de llamada "OnLabelChanged" se mostrará más adelante en esta sección.

```csharp
public static readonly DependencyProperty LabelProperty = DependencyProperty.Register(
  "Label",
  typeof(String),
  typeof(ImageWithLabelControl),
  new PropertyMetadata(null,new PropertyChangedCallback(OnLabelChanged))
);
```

```vb
Public Shared ReadOnly LabelProperty As DependencyProperty =
    DependencyProperty.Register("Label",
      GetType(String),
      GetType(ImageWithLabelControl),
      New PropertyMetadata(
        Nothing, new PropertyChangedCallback(AddressOf OnLabelChanged)))
```

```cppwinrt
// ImageWithLabelControl.cpp
...
Windows::UI::Xaml::DependencyProperty ImageWithLabelControl::m_labelProperty =
    Windows::UI::Xaml::DependencyProperty::Register(
        L"Label",
        winrt::xaml_typename<winrt::hstring>(),
        winrt::xaml_typename<ImageWithLabelControlApp::ImageWithLabelControl>(),
        Windows::UI::Xaml::PropertyMetadata{ nullptr, Windows::UI::Xaml::PropertyChangedCallback{ &ImageWithLabelControl::OnLabelChanged } }
);
...
```

```cpp
DependencyProperty^ ImageWithLabelControl::_LabelProperty =
    DependencyProperty::Register("Label",
    Platform::String::typeid,
    ImageWithLabelControl::typeid,
    ref new PropertyMetadata(nullptr,
      ref new PropertyChangedCallback(&ImageWithLabelControl::OnLabelChanged))
    );
```

### <a name="default-value"></a>Valor predeterminado

Puedes especificar un valor predeterminado para una propiedad de dependencia de tal forma que la propiedad siempre devuelva un valor predeterminado particular cuando esté sin establecer. Este valor puede ser distinto que el valor predeterminado inherente del tipo de esa propiedad.

Si no se especifica un valor predeterminado, el valor predeterminado de una propiedad de dependencia es null para un tipo de referencia, o el valor predeterminado del tipo para un tipo de valor o tipo primitivo del lenguaje (por ejemplo, 0 para un entero o una cadena vacía para una cadena). El principal motivo para establecer un valor predeterminado es que este valor se restablece al llamar a [**ClearValue**](https://msdn.microsoft.com/library/windows/apps/br242357) en la propiedad. Quizás sea más conveniente establecer un valor predeterminado para cada propiedad que establecer valores predeterminados en los constructores, especialmente para los tipos de valor. Sin embargo, para los tipos de referencia, asegúrate de que al establecer un valor predeterminado no se crea un patrón de singleton no intencionado. Para más información, consulta los [Procedimientos recomendados](#best-practices) más adelante en este tema

```cppwinrt
// ImageWithLabelControl.cpp
...
Windows::UI::Xaml::DependencyProperty ImageWithLabelControl::m_labelProperty =
    Windows::UI::Xaml::DependencyProperty::Register(
        L"Label",
        winrt::xaml_typename<winrt::hstring>(),
        winrt::xaml_typename<ImageWithLabelControlApp::ImageWithLabelControl>(),
        Windows::UI::Xaml::PropertyMetadata{ winrt::box_value(L"default label"), Windows::UI::Xaml::PropertyChangedCallback{ &ImageWithLabelControl::OnLabelChanged } }
);
...
```

> [!NOTE]
> No realices el registro con un valor predeterminado de [**UnsetValue**](https://msdn.microsoft.com/library/windows/apps/br242371). Esto confundirá a los usuarios de la propiedad y tendrá consecuencias imprevistas en el sistema de propiedades.

### <a name="createdefaultvaluecallback"></a>CreateDefaultValueCallback

En algunos escenarios, definirás propiedades de dependencias para objetos que se usan en más de un subproceso de interfaz de usuario. Este podría ser el caso si vas a definir un objeto de datos que usen varias aplicaciones, o un control que uses en más de una aplicación. Puedes habilitar el intercambio del objeto entre distintos subprocesos de interfaz de usuario proporcionando una implementación [**CreateDefaultValueCallback**](https://msdn.microsoft.com/library/windows/apps/hh701812) en vez de una instancia de valor predeterminado, que está vinculada al subproceso que registró la propiedad. Básicamente, [**CreateDefaultValueCallback**](https://msdn.microsoft.com/library/windows/apps/hh701812) define una fábrica de valores predeterminados. El valor devuelto por **CreateDefaultValueCallback** siempre está asociado al actual subproceso de interfaz de usuario **CreateDefaultValueCallback** que usa el objeto.

Para definir metadatos que especifiquen [**CreateDefaultValueCallback**](https://msdn.microsoft.com/library/windows/apps/hh701812), deberás llamar a [**PropertyMetadata.Create**](https://msdn.microsoft.com/library/windows/apps/hh702115) para volver a una instancia de metadatos; los constructores [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771) no tienen una firma que incluya un parámetro **CreateDefaultValueCallback**.

El patrón de implementación típico de [**CreateDefaultValueCallback**](https://msdn.microsoft.com/library/windows/apps/hh701812) consiste en crear una nueva clase [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356), establecer el valor de propiedad específico de cada propiedad de **DependencyObject** en el valor predeterminado previsto y devolver la clase nueva como una referencia de **Object** mediante el valor devuelto del método **CreateDefaultValueCallback**.

### <a name="property-changed-callback-method"></a>Método de devolución de llamada modificado por propiedades

Puedes definir un método de devolución de llamada modificado por propiedades para definir las interacciones de tu propiedad con las demás propiedades de dependencia, o bien para actualizar una propiedad o un estado interno del objeto siempre que la propiedad cambie. Si se invoca tu devolución de llamada, el sistema de propiedades ha determinado que hay un cambio eficaz en el valor de propiedad. Como el método de devolución de llamada es estático, el parámetro *d* de la devolución de llamada es importante porque te dice qué instancia de la clase ha notificado el cambio. Una implementación típica usa la propiedad [**NewValue**](https://msdn.microsoft.com/library/windows/apps/br242364) de los datos del evento y realiza algún procesamiento en ese valor, normalmente realizando otros cambios en el objeto pasado como *d*. Otras respuestas al cambio de una propiedad son rechazar el valor notificado en **NewValue**, para restablecer [**OldValue**](https://msdn.microsoft.com/library/windows/apps/br242365) o aplicar a **NewValue** un valor restringido mediante programación.

El siguiente ejemplo muestra una implementación de [**PropertyChangedCallback**](https://msdn.microsoft.com/library/windows/apps/br208770). Implementa el método al que se hizo referencia en los ejemplos [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) anteriores, como parte de los argumentos de construcción de [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771). En el escenario al que se dirige esta devolución de llamada, la clase también tiene una propiedad calculada de solo lectura denominada "HasLabelValue" (no se muestra la implementación). Siempre que se vuelve a evaluar la propiedad "Label", se invoca este método de devolución de llamada, así mismo, la devolución de llamada permite que el valor calculado dependiente permanezca sincronizado con los cambios de la propiedad de dependencia.

```csharp
private static void OnLabelChanged(DependencyObject d, DependencyPropertyChangedEventArgs e) {
    ImageWithLabelControl iwlc = d as ImageWithLabelControl; //null checks omitted
    String s = e.NewValue as String; //null checks omitted
    if (s == String.Empty)
    {
        iwlc.HasLabelValue = false;
    } else {
        iwlc.HasLabelValue = true;
    }
}
```

```vb
    Private Shared Sub OnLabelChanged(d As DependencyObject, e As DependencyPropertyChangedEventArgs)
        Dim iwlc As ImageWithLabelControl = CType(d, ImageWithLabelControl) ' null checks omitted
        Dim s As String = CType(e.NewValue,String) ' null checks omitted
        If s Is String.Empty Then
            iwlc.HasLabelValue = False
        Else
            iwlc.HasLabelValue = True
        End If
    End Sub
```

```cppwinrt
void ImageWithLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& e)
{
    auto iwlc{ d.as<ImageWithLabelControlApp::ImageWithLabelControl>() };
    auto s{ winrt::unbox_value<winrt::hstring>(e.NewValue()) };
    iwlc.HasLabelValue(s.size() != 0);
}
```

```cpp
static void OnLabelChanged(DependencyObject^ d, DependencyPropertyChangedEventArgs^ e)
{
    ImageWithLabelControl^ iwlc = (ImageWithLabelControl^)d;
    Platform::String^ s = (Platform::String^)(e->NewValue);
    if (s->IsEmpty()) {
        iwlc->HasLabelValue=false;
    }
}
```

### <a name="property-changed-behavior-for-structures-and-enumerations"></a>Comportamiento modificado por propiedades para estructuras y enumeraciones

Si una [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/br242362) es una enumeración o una estructura, la devolución de llamada podría invocarse incluso si los valores internos de la estructura o el valor de la enumeración no cambiaron. Esto es distinto de un primitivo del sistema, como una cadena donde solo se invoca si el cambia el valor. Esto es un efecto secundario de las operaciones de conversiones boxing y unboxing en estos valores que se realizan internamente. Si tienes un método [**PropertyChangedCallback**](https://msdn.microsoft.com/library/windows/apps/br208770) para una propiedad en la que el valor es una enumeración o una estructura, deberás compara los valores [**OldValue**](https://msdn.microsoft.com/library/windows/apps/br242365) y [**NewValue**](https://msdn.microsoft.com/library/windows/apps/br242364) difundiendo tú mismo los valores y usando los operadores de comparación sobrecargados disponibles para los valores difundidos ahora. O, si ninguno de estos operadores está disponible (lo que podría ocurrir en caso de una estructura personalizada), es posible que tengas que comparar los valores individuales. Normalmente no tendrías que hacer nada si el resultado es que los valores no han cambiado.

```csharp
private static void OnVisibilityValueChanged(DependencyObject d, DependencyPropertyChangedEventArgs e) {
    if ((Visibility)e.NewValue != (Visibility)e.OldValue)
    {
        //value really changed, invoke your changed logic here
    } // else this was invoked because of boxing, do nothing
}
```

```vb
Private Shared Sub OnVisibilityValueChanged(d As DependencyObject, e As DependencyPropertyChangedEventArgs)
    If CType(e.NewValue,Visibility) != CType(e.OldValue,Visibility) Then
        '  value really changed, invoke your changed logic here
    End If
    '  else this was invoked because of boxing, do nothing
End Sub
```

```cppwinrt
static void OnVisibilityValueChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& e)
{
    auto oldVisibility{ winrt::unbox_value<Windows::UI::Xaml::Visibility>(e.OldValue()) };
    auto newVisibility{ winrt::unbox_value<Windows::UI::Xaml::Visibility>(e.NewValue()) };

    if (newVisibility != oldVisibility)
    {
        // The value really changed; invoke your property-changed logic here.
    }
    // Otherwise, OnVisibilityValueChanged was invoked because of boxing; do nothing.
}
```

```cpp
static void OnVisibilityValueChanged(DependencyObject^ d, DependencyPropertyChangedEventArgs^ e)
{
    if ((Visibility)e->NewValue != (Visibility)e->OldValue)
    {
        //value really changed, invoke your changed logic here
    } 
    // else this was invoked because of boxing, do nothing
    }
}
```

## <a name="best-practices"></a>Procedimientos recomendados

Las siguientes consideraciones son los procedimientos recomendados cuando definas tu propiedad de dependencia personalizada.

### <a name="dependencyobject-and-threading"></a>DependencyObject y subprocesos

Todas las instancias de [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356) deben crearse en el subproceso de interfaz de usuario asociado a [**Window**](https://msdn.microsoft.com/library/windows/apps/br209041) actual que muestra la aplicación de Windows Runtime. Aunque cada **DependencyObject** debe crearse en el subproceso de interfaz de usuario, se puede tener acceso a los objetos mediante una referencia de distribuidor desde otros subprocesos llamando a [**Dispatcher**](https://msdn.microsoft.com/library/windows/apps/br230616).

Los aspectos de subprocesos de [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356) son relevantes porque, por lo general, significa que solo el código que se ejecuta en el subproceso de interfaz de usuario puede cambiar o incluso leer el valor de una propiedad de dependencias. Los problemas de subprocesos normalmente se pueden evitar en el código típico de la interfaz de usuario que haga un uso correcto de los patrones **async** y de los subprocesos de trabajo en segundo plano. Normalmente, solo te encontrarás con problemas de subprocesos relacionados con **DependencyObject** si vas a definir tipos de **DependencyObject** e intentas usarlos para los orígenes de datos u otros escenarios donde un **DependencyObject** no es necesariamente lo adecuado.

### <a name="avoiding-unintentional-singletons"></a>Evitar singleton no intencionados

Se puede producir un singleton no intencionado si declaras una propiedad de dependencia que toma un tipo de referencia y llamas a un constructor para este tipo de referencia como parte del código que establece tu [**PropertyMetadata**](https://msdn.microsoft.com/library/windows/apps/br208771). Lo que sucede es que todos los usos de la propiedad de dependencia comparten tan solo una instancia de **PropertyMetadata** y, por lo tanto, intentan compartir el único tipo de referencia que construiste. Después, las subpropiedades de ese tipo de valor que estableces mediante la propiedad de dependencia se propagan a los demás objetos de maneras que, probablemente, no pretendías.

Puedes usar constructores de clases para establecer los valores iniciales de una propiedad de dependencia de tipo de referencia si deseas un valor no null, pero ten en cuenta que este debe considerarse un valor local en términos de [Introducción a las propiedades de dependencia](dependency-properties-overview.md). Sería más apropiado usar una plantilla para este propósito, si tu clase admite plantillas. Otra manera de evitar un patrón de singleton y seguir proporcionando un valor predeterminado útil, es exponer una propiedad estática en el tipo de referencia que proporcione valores predeterminados apropiados para dicha clase.

### <a name="collection-type-dependency-properties"></a>Propiedades de dependencia de tipo de colección

Las propiedades de dependencia de tipo de colección presentan algunas dificultades de implementación adicionales que debes tener en cuenta.

Las propiedades de dependencia de tipo de colección son relativamente raras en la API de Windows Runtime. En la mayoría de los casos, puedes usar colecciones en las que los elementos sean una subclase [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356), pero la propiedad de colección en sí se implementa como una propiedad convencional de CLR o C++. El motivo es que las colecciones no son necesariamente adecuadas para algunos escenarios habituales en los que hay implicadas propiedades de dependencia. Por ejemplo:

- Normalmente no animas una colección.
- No sueles rellenar previamente los elementos de una colección con estilos o una plantilla.
- Aunque el enlace a colecciones es un escenario importante, una colección no necesita ser una propiedad de dependencia para ser un origen de enlace. En el caso de los destinos de enlace, es más habitual usar subclases de [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/br242803) o [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/br242348) para admitir elementos de colección o para usar patrones de modelo de vista. Para obtener más información sobre el enlace hacia o desde colecciones, consulta [Enlace de datos en profundidad](https://msdn.microsoft.com/library/windows/apps/mt210946).
- Las notificaciones de los cambios en una colección se controlan mejor mediante interfaces como **INotifyPropertyChanged** o **INotifyCollectionChanged**, o bien derivando el tipo de colección de [**ObservableCollection&lt;T&gt;**](https://msdn.microsoft.com/library/windows/apps/ms668604.aspx).

En cualquier caso, existen escenarios para propiedades de dependencia de tipo de colección. Las siguientes tres secciones proporcionan una orientación sobre cómo implementar una propiedad de dependencia de tipo de colección.

### <a name="initializing-the-collection"></a>Inicialización de la colección

Al crear una propiedad de dependencia, puedes establecer un valor predeterminado mediante metadatos de propiedad de dependencia. Ten cuidado de no usar una colección estática de singleton como valor predeterminado. En su lugar, debes establecer deliberadamente el valor de la colección en una colección única (instancia) como parte de la lógica del constructor de la clase propietaria de la propiedad de colección.

### <a name="change-notifications"></a>Notificación de cambios

Definir la colección como una propiedad de dependencia no proporciona automáticamente notificaciones para los elementos de la colección cuando el sistema de propiedades invoca el método de devolución de llamadas "PropertyChanged". Si deseas notificaciones para colecciones o elementos de colección, por ejemplo, un escenario de enlace de datos, implementa la interfaz **INotifyPropertyChanged** o **INotifyCollectionChanged**. Para obtener más información, consulta el tema sobre el [Enlace de datos en profundidad](https://msdn.microsoft.com/library/windows/apps/mt210946).

### <a name="dependency-property-security-considerations"></a>Consideraciones de seguridad de las propiedades de dependencia

Declara las propiedades de dependencia como propiedades públicas. Declara los identificadores de las propiedades de dependencia como miembros de **solo lectura, estáticos y públicos**. Aunque intentes declarar otros niveles de acceso permitidos por un lenguaje (como **protected**), una propiedad de dependencia siempre es accesible mediante el identificador en combinación con las API del sistema de propiedades. Declarar el identificador de la propiedad de dependencia como interno o privado no funcionará porque el sistema de propiedades no podría funcionar correctamente.

En realidad, las propiedades de contenedor se usan por conveniencia. Los mecanismos de seguridad que se aplican a los contenedores se pueden omitir con llamadas a [**GetValue**](https://msdn.microsoft.com/library/windows/apps/br242359) o [**SetValue**](https://msdn.microsoft.com/library/windows/apps/br242361). Por lo tanto, haz que las propiedades de contenedor sean públicas; de lo contrario, solo conseguirás que la propiedad sea más difícil de usar para los usuarios legítimos sin proporcionar a cambio ninguna ventaja en cuanto a seguridad.

Windows Runtime no ofrece ninguna manera de registrar una propiedad de dependencia personalizada como de solo lectura.

### <a name="dependency-properties-and-class-constructors"></a>Propiedades de dependencia y constructores de clases

Existe un principio general por el que los constructores de clases no deben llamar a métodos virtuales. Esto se debe a que se puede llamar a los constructores para realizar la inicialización de base de un constructor de clases derivado y la entrada en el método virtual a través del constructor podría ocurrir cuando la instancia del objeto que se está construyendo aún no se ha inicializado por completo. Al derivar de una clase que ya deriva de [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356), recuerda que el propio sistema de propiedades llama y expone los métodos virtuales internamente como parte de sus servicios. Para evitar posibles problemas con la inicialización en tiempo de ejecución, establece los valores de las propiedades de dependencia en los constructores de clases.

### <a name="registering-the-dependency-properties-for-ccx-apps"></a>Registro de las propiedades de dependencia de las aplicaciones C++/CX

La implementación para registrar una propiedad en C++/CX es más complicada que en C#, tanto debido a la separación en encabezado y archivo de implementación, como debido a la inicialización en el ámbito raíz del archivo de implementación como una práctica errónea. (Las extensiones de componentes de VisualC ++ (C++ / CX) ponen el código del inicializador estático desde el ámbito raíz directamente en **DllMain**, mientras que los compiladores C# asignan los inicializadores estáticos a clases y, por consiguiente, evitar problemas de bloqueo de carga de **DllMain** .). En este caso, la mejor práctica consiste en declarar una función auxiliar que se encargue de todo el registro de la propiedad de dependencia de una clase, una función por clase. Luego, por cada clase personalizada que la aplicación consuma, tendrás que hacer referencia a la función de registro del auxiliar que todas las clases personalizadas que quieres usar exponen. Llama una vez a cada función de registro del auxiliar como parte de [**Application constructor**](https://msdn.microsoft.com/library/windows/apps/br242325) (`App::App()`), antes de `InitializeComponent`. Ese constructor solo se ejecuta cuando se hace realmente referencia a la aplicación por primera vez. No se volverá a ejecutar si, por ejemplo, se reanuda una aplicación suspendida. Además, tal como se ha visto en el ejemplo de registro de C++ anterior, la comprobación de **nullptr** en torno a cada llamada de [**Register**](https://msdn.microsoft.com/library/windows/apps/hh701829) es importante: garantiza que ningún llamador de la función pueda registrar dos veces la propiedad. Una segunda llamada de registro probablemente bloquearía la aplicación sin esa comprobación, ya que el nombre de la propiedad sería un duplicado. Puedes ver este patrón de implementación en el [ejemplo de controles personalizados y de usuario XAML](http://go.microsoft.com/fwlink/p/?linkid=238581) si buscas en el código la versión C++/CX de la muestra.

## <a name="related-topics"></a>Temas relacionados

- [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356)
- [**DependencyProperty.Register**](https://msdn.microsoft.com/library/windows/apps/hh701829)
- [Introducción a las propiedades de dependencia](dependency-properties-overview.md)
- [Muestra de controles de usuario y controles personalizados de XAML](http://go.microsoft.com/fwlink/p/?linkid=238581)
 