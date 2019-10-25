---
Description: Describe el concepto de automatización del mismo nivel para la automatización de la interfaz de usuario de Microsoft y cómo puede proporcionar compatibilidad con automatización para su propia clase de interfaz de usuario personalizada.
ms.assetid: AA8DA53B-FE6E-40AC-9F0A-CB09637C87B4
title: Elementos de automatización del mismo nivel personalizados
label: Custom automation peers
template: detail.hbs
ms.date: 07/13/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: c6bc6996e80aacbd9eec0f37127a1dd24ec0e24e
ms.sourcegitcommit: f561efbda5c1d47b85601d91d70d86c5332bbf8c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2019
ms.locfileid: "72690406"
---
# <a name="custom-automation-peers"></a>Elementos de automatización del mismo nivel personalizados  

Describe el concepto de automatización del mismo nivel para la automatización de la interfaz de usuario de Microsoft y cómo puede proporcionar compatibilidad con automatización para su propia clase de interfaz de usuario personalizada.

La automatización de la interfaz de usuario proporciona un marco de trabajo que los clientes de automatización pueden utilizar para examinar o utilizar las interfaces de usuario de una variedad de plataformas y plataformas de interfaz de usuario. Si está escribiendo una aplicación Plataforma universal de Windows (UWP), las clases que usa para la interfaz de usuario ya proporcionan compatibilidad con la automatización de la interfaz de usuario. Puede derivar de clases existentes y no selladas para definir un nuevo tipo de control de interfaz de usuario o clase de compatibilidad. En el proceso de hacerlo, la clase podría agregar un comportamiento que debería tener compatibilidad con accesibilidad pero que la compatibilidad con la automatización de la interfaz de usuario predeterminada no abarca. En este caso, debe extender la compatibilidad con la automatización de la interfaz de usuario existente derivando de la clase [**AutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer) que usó la implementación base, agregando cualquier compatibilidad necesaria a la implementación del mismo nivel e informando del plataforma universal de Windows (UWP) Controle la infraestructura que debe crear el nuevo elemento del mismo nivel.

La automatización de la interfaz de usuario no solo permite aplicaciones de accesibilidad y tecnologías de asistencia, como lectores de pantalla, sino también código de garantía de calidad (prueba). En cualquier escenario, los clientes de automatización de la interfaz de usuario pueden examinar los elementos de la interfaz de usuario y simular la interacción del usuario con la aplicación desde otro código fuera de la aplicación. Para obtener información sobre la automatización de la interfaz de usuario en todas las plataformas y en su significado más amplio, consulte [información general sobre UI Automation](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-uiautomationoverview).

Hay dos audiencias distintas que usan el marco de automatización de la interfaz de usuario.

* ***Los clientes* de UI Automation** llaman a las API de automatización de la interfaz de usuario para obtener información sobre toda la interfaz de usuario que se muestra actualmente al usuario. Por ejemplo, una tecnología de asistencia, como un lector de pantalla, actúa como un cliente de automatización de la interfaz de usuario. La interfaz de usuario se presenta como un árbol de elementos de automatización relacionados. El cliente de automatización de la interfaz de usuario puede estar interesado en una sola aplicación a la vez o en todo el árbol. El cliente de automatización de la interfaz de usuario puede usar las API de automatización de la interfaz de usuario para navegar por el árbol y leer o cambiar la información de los elementos de automatización.
* Los  ***proveedores* de automatización** de la interfaz de usuario aportan información al árbol de automatización de la interfaz de usuario mediante la implementación de las API que exponen los elementos de la interfaz de usuario que introdujeron como parte de su aplicación. Al crear un nuevo control, ahora debe actuar como participante en el escenario del proveedor de automatización de la interfaz de usuario. Como proveedor, debe asegurarse de que todos los clientes de automatización de la interfaz de usuario puedan usar el marco de automatización de la interfaz de usuario para interactuar con el control con fines de prueba y accesibilidad.

Normalmente, hay API paralelas en el marco de automatización de la interfaz de usuario: una API para clientes de automatización de la interfaz de usuario y otra API con el nombre similar para proveedores de automatización de la interfaz de usuario. En su mayor parte, en este tema se tratan las API del proveedor de automatización de la interfaz de usuario y, en concreto, las clases e interfaces que habilitan la extensibilidad del proveedor en ese marco de interfaz de usuario. En ocasiones, se mencionan las API de automatización de la interfaz de usuario que usan los clientes de automatización de la interfaz de usuario para proporcionar alguna perspectiva o proporcionar una tabla de búsqueda que correlaciona las API del cliente y del proveedor. Para obtener más información sobre la perspectiva del cliente, vea la [Guía del programador del cliente de UI Automation](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-clientportal).

> [!NOTE]
> Los clientes de automatización de la interfaz de usuario no suelen usar código administrado y no se implementan normalmente como una aplicación de UWP (suelen ser aplicaciones de escritorio). La automatización de la interfaz de usuario se basa en un estándar, no en una implementación o marco de trabajo específicos. Muchos clientes de automatización de la interfaz de usuario existentes, incluidos los productos de tecnología de asistencia, como los lectores de pantalla, utilizan las interfaces del modelo de objetos componentes (COM) para interactuar con la automatización de la interfaz de usuario, el sistema y las aplicaciones que se ejecutan en ventanas secundarias. Para obtener más información sobre las interfaces COM y cómo escribir un cliente de UI Automation mediante COM, consulte [aspectos básicos](https://docs.microsoft.com/windows/desktop/WinAuto/entry-uiautocore-overview)de la automatización de la interfaz de usuario.

<span id="Determining_the_existing_state_of_UI_Automation_support_for_your_custom_UI_class"/>
<span id="determining_the_existing_state_of_ui_automation_support_for_your_custom_ui_class"/>
<span id="DETERMINING_THE_EXISTING_STATE_OF_UI_AUTOMATION_SUPPORT_FOR_YOUR_CUSTOM_UI_CLASS"/>

## <a name="determining-the-existing-state-of-ui-automation-support-for-your-custom-ui-class"></a>Determinar el estado existente de la compatibilidad de automatización de la interfaz de usuario para la clase de interfaz de usuario personalizada  
Antes de intentar implementar una automatización del mismo nivel para un control personalizado, debe probar si la clase base y su automatización del mismo nivel ya proporcionan la compatibilidad con la accesibilidad o la automatización que necesita. En muchos casos, la combinación de las implementaciones de [**FrameworkElementAutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer) , los pares específicos y los patrones que implementan pueden proporcionar una experiencia de accesibilidad básica pero satisfactoria. El hecho de que esto sea cierto depende del número de cambios realizados en el modelo de objetos que se exponen al control frente a su clase base. Además, esto depende de si las adiciones a la funcionalidad de la clase base se correlacionan con los nuevos elementos de la interfaz de usuario en el contrato de plantilla o con la apariencia visual del control. En algunos casos, los cambios pueden presentar nuevos aspectos de la experiencia del usuario que requieren compatibilidad adicional con la accesibilidad.

Aunque el uso de la clase base existente del mismo nivel proporciona la compatibilidad básica con la accesibilidad, sigue siendo un procedimiento recomendado definir un elemento del mismo nivel para que pueda notificar información de **classnames** precisa a la automatización de la interfaz de usuario para escenarios de pruebas automatizadas. Esta consideración es especialmente importante si está escribiendo un control destinado a un consumo de terceros.

<span id="Automation_peer_classes__"/>
<span id="automation_peer_classes__"/>
<span id="AUTOMATION_PEER_CLASSES__"/>

## <a name="automation-peer-classes"></a>Clases de automatización del mismo nivel  
UWP se basa en las convenciones y técnicas de automatización de la interfaz de usuario existentes usadas por los marcos de interfaz de usuario de código administrado anteriores, como Windows Forms, Windows Presentation Foundation (WPF) y Microsoft Silverlight. Muchas de las clases de control y su función y propósito también tienen su origen en un marco de trabajo de interfaz de usuario anterior.

Por Convención, los nombres de clase del mismo nivel comienzan por el nombre de la clase de control y terminan por "AutomationPeer". Por ejemplo, [**ButtonAutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.ButtonAutomationPeer) es la clase del mismo nivel para la clase de control de [**botón**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) .

> [!NOTE]
> Para los fines de este tema, trataremos las propiedades que están relacionadas con la accesibilidad como más importantes al implementar un control del mismo nivel. Pero para un concepto más general de la compatibilidad con la automatización de la interfaz de usuario, debe implementar un elemento del mismo nivel de acuerdo con las recomendaciones que se documentan en la [Guía del programador del proveedor de automatización](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-providerportal) de la interfaz de usuario y los [fundamentos de UI Automation](https://docs.microsoft.com/windows/desktop/WinAuto/entry-uiautocore-overview). Estos temas no cubren las API de [**AutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer) específicas que usaría para proporcionar la información en el marco UWP para la automatización de la interfaz de usuario, pero describen las propiedades que identifican la clase o proporcionan otra información o interacción.

<span id="Peers__patterns_and_control_types"/>
<span id="peers__patterns_and_control_types"/>
<span id="PEERS__PATTERNS_AND_CONTROL_TYPES"/>

## <a name="peers-patterns-and-control-types"></a>Elementos del mismo nivel, modelos y tipos de control  
Un *patrón de control* es una implementación de interfaz que expone un aspecto concreto de la funcionalidad de un control a un cliente de automatización de la interfaz de usuario. Los clientes de automatización de la interfaz de usuario usan las propiedades y los métodos expuestos a través de un patrón de control para recuperar información sobre las capacidades del control o para manipular el comportamiento del control en tiempo de ejecución.

Los patrones de control proporcionan una manera de categorizar y exponer la funcionalidad de un control independientemente del tipo de control o de la apariencia del control. Por ejemplo, un control que presenta una interfaz tabular usa el patrón de control **Grid** para exponer el número de filas y columnas de la tabla, y para permitir que un cliente de automatización de la interfaz de usuario recupere elementos de la tabla. Como ejemplo, el cliente de automatización de la interfaz de usuario puede usar el patrón de control **Invoke** para los controles que se pueden invocar, como los botones, y el patrón de control **Scroll** para los controles que tienen barras de desplazamiento, como cuadros de lista, vistas de lista o cuadros combinados. Cada patrón de control representa un tipo de funcionalidad independiente y los patrones de control se pueden combinar para describir el conjunto completo de funcionalidades admitidas por un control determinado.

Los patrones de control se relacionan con la interfaz de usuario cuando las interfaces se relacionan con objetos COM. En COM, puede consultar un objeto para preguntar qué interfaces admite y, a continuación, usar esas interfaces para tener acceso a la funcionalidad. En la automatización de la interfaz de usuario, los clientes de automatización de la interfaz de usuario pueden consultar un elemento de automatización de la interfaz de usuario para averiguar qué patrones de control admite y, después, interactuar con el elemento y su control emparejado a través de las propiedades, métodos, eventos y estructuras que expone el patrones de control.

Uno de los objetivos principales de una automatización del mismo nivel es informar a un cliente de automatización de la interfaz de usuario de qué patrones de control puede admitir el elemento de la interfaz de usuario a través de su elemento del mismo nivel. Para ello, los proveedores de automatización de la interfaz de usuario implementan nuevos elementos del mismo nivel que cambian el comportamiento del método [**GetPattern**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpattern) invalidando el método [**GetPatternCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) . Los clientes de UI Automation realizan llamadas que el proveedor de automatización de la interfaz de usuario asigna a llamar a **GetPattern**. Los clientes de automatización de la interfaz de usuario consultan cada patrón específico con el que desean interactuar. Si el elemento del mismo nivel admite el patrón, devuelve una referencia de objeto a sí misma. en caso contrario, devuelve **null**. Si el valor devuelto no es **null**, el cliente de automatización de la interfaz de usuario espera que pueda llamar a las API de la interfaz de patrón como un cliente, con el fin de interactuar con ese patrón de control.

Un *tipo de control* es una manera de definir de forma amplia la funcionalidad de un control que representa el elemento del mismo nivel. Este es un concepto diferente que un patrón de control porque mientras que un patrón informa a la automatización de la interfaz de usuario de la información que puede obtener o de las acciones que puede realizar a través de una interfaz determinada, el tipo de control existe un nivel por encima de ese. Cada tipo de control tiene orientación sobre estos aspectos de la automatización de la interfaz de usuario:

* Patrones de control de UI Automation: un tipo de control puede admitir más de un patrón, cada uno de los cuales representa una clasificación diferente de información o interacción. Cada tipo de control tiene un conjunto de patrones de control que el control debe admitir, un conjunto que es opcional y un conjunto que el control no debe admitir.
* Valores de propiedad de automatización de la interfaz de usuario: cada tipo de control tiene un conjunto de propiedades que el control debe admitir. Estas son las propiedades generales, como se describe en [información general sobre las propiedades de automatización](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-propertiesoverview)de la interfaz de usuario, no las que son específicas del patrón.
* Eventos de automatización de la interfaz de usuario: cada tipo de control tiene un conjunto de eventos que el control debe admitir. De nuevo, son generales, no específicas del patrón, como se describe en [información general sobre eventos de automatización](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-eventsoverview)de la interfaz de usuario.
* Estructura de árbol de automatización de la interfaz de usuario: cada tipo de control define cómo debe aparecer el control en la estructura de árbol de automatización de la interfaz de usuario.

Independientemente de cómo se implementen la automatización del mismo nivel para el marco de trabajo, la funcionalidad de cliente de automatización de la interfaz de usuario no está asociada a UWP y, de hecho, es probable que los clientes de automatización de la interfaz de usuario existentes, como las tecnologías de asistencia, usen otros modelos de programación, como COM. En COM, los clientes pueden ejecutar **QueryInterface** para la interfaz de patrón de control com que implementa el patrón solicitado o el marco de automatización de la interfaz de usuario general para propiedades, eventos o examen de árbol. En el caso de los patrones, el marco de automatización de la interfaz de usuario calcula las referencias de ese código de interfaz en el código UWP que se ejecuta en el proveedor de automatización de la interfaz de usuario de la aplicación y el elemento

Cuando se implementan patrones de control para un marco de código administrado, como una aplicación de UWP que usa C\# o Microsoft Visual Basic, puede usar interfaces de .NET Framework para representar estos patrones en lugar de usar la representación de la interfaz COM. Por ejemplo, la interfaz de patrón de automatización de la interfaz de usuario para una implementación de proveedor Microsoft .NET del patrón **Invoke** es [**IInvokeProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IInvokeProvider).

Para obtener una lista de patrones de control, interfaces de proveedor y su propósito, vea [patrones de control e interfaces](control-patterns-and-interfaces.md). Para obtener la lista de los tipos de control, vea [UI Automation Control Types Overview](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-controltypesoverview).

<span id="Guidance_for_how_to_implement_control_patterns"/>
<span id="guidance_for_how_to_implement_control_patterns"/>
<span id="GUIDANCE_FOR_HOW_TO_IMPLEMENT_CONTROL_PATTERNS"/>

### <a name="guidance-for-how-to-implement-control-patterns"></a>Instrucciones para implementar patrones de control  
Los patrones de control y su finalidad son parte de una definición más grande del marco de automatización de la interfaz de usuario y no solo se aplican a la compatibilidad con accesibilidad para una aplicación para UWP. Cuando implemente un patrón de control, debe asegurarse de que lo está implementando de forma que coincida con las instrucciones que se documentan en estos documentos y también en la especificación de automatización de la interfaz de usuario. Si busca instrucciones, en general puede usar la documentación de Microsoft y no necesitará hacer referencia a la especificación. Las instrucciones para cada patrón se documentan aquí: [implementar patrones de control de UI Automation](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-implementinguiautocontrolpatterns). Observará que cada tema de esta área tiene una sección "instrucciones y convenciones de implementación" y la sección "miembros necesarios". La guía normalmente hace referencia a las API específicas de la interfaz de patrón de control pertinente en el [patrón de control de referencia de proveedores](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-cpinterfaces) . Estas interfaces son las interfaces nativas/COM (y sus API usan la sintaxis de estilo COM). Pero todo lo que ve tiene un equivalente en el espacio de nombres [**Windows. UI. Xaml. Automation. Provider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider) .

Si utiliza los elementos del mismo nivel de automatización predeterminados y expande su comportamiento, esos elementos del mismo nivel ya se han escrito de acuerdo con las directrices de automatización de la interfaz de usuario. Si admiten patrones de control, puede confiar en esa compatibilidad de patrón conforme a las instrucciones de [implementación](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-implementinguiautocontrolpatterns)de los patrones de control de automatización de la interfaz de usuario. Si un control del mismo nivel indica que es representativo de un tipo de control definido por la automatización de la interfaz de usuario, la guía documentada en compatibilidad con los [tipos de control de automatización](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-supportinguiautocontroltypes) de la interfaz de usuario ha sido seguida por ese mismo nivel.

No obstante, es posible que necesite orientación adicional para patrones de control o tipos de control con el fin de seguir las recomendaciones de automatización de la interfaz de usuario en su implementación del mismo nivel. Esto sería especialmente cierto si está implementando compatibilidad con el tipo de control o patrón que todavía no existe como implementación predeterminada en un control de UWP. Por ejemplo, el patrón de las anotaciones no se implementa en ninguno de los controles XAML predeterminados. Pero es posible que tenga una aplicación que use anotaciones exhaustivamente y, por tanto, desee exponer esa funcionalidad para que sea accesible. En este escenario, el equipo del mismo nivel debe implementar [**IAnnotationProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IAnnotationProvider) y probablemente se informe a sí mismo como el tipo de control **Document** con las propiedades adecuadas para indicar que los documentos admiten la anotación.

Se recomienda usar las instrucciones que se ven en los patrones de implementación de [patrones de control de automatización](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-implementinguiautocontrolpatterns) de la interfaz de usuario o tipos de control en [compatibilidad con tipos de control de automatización](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-supportinguiautocontroltypes) de la interfaz de usuario como orientación e instrucciones generales. Incluso puede intentar seguir algunos de los vínculos de API para obtener descripciones y comentarios sobre el propósito de las API. Pero para ver los detalles de la sintaxis que se necesitan para la programación de aplicaciones para UWP, busque la API equivalente en el espacio de nombres [**Windows. UI. Xaml. Automation. Provider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider) y use esas páginas de referencia para obtener más información.

<span id="Built-in_automation_peer_classes"/>
<span id="built-in_automation_peer_classes"/>
<span id="BUILT-IN_AUTOMATION_PEER_CLASSES"/>

## <a name="built-in-automation-peer-classes"></a>Clases integradas de automatización del mismo nivel  
En general, los elementos implementan una clase de automatización del mismo nivel si aceptan actividad de la interfaz de usuario del usuario, o si contienen información necesaria para los usuarios de tecnologías de asistencia que representan la interfaz de usuario interactiva o significativa de las aplicaciones. No todos los elementos visuales de UWP tienen pares de automatización. Ejemplos de clases que implementan elementos de automatización del mismo nivel son [**Button**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) y [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox). Ejemplos de clases que no implementan elementos de automatización del mismo nivel son [**Border**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Border) y clases basadas en el [**Panel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Panel), como [**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid) y [**Canvas**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas). Un **Panel** no tiene un elemento del mismo nivel porque está proporcionando un comportamiento de diseño que solo es visual. No hay ninguna manera relevante de accesibilidad para que el usuario interactúe con un **Panel**. Los elementos secundarios que contiene un **Panel** se informan en los árboles de automatización de la interfaz de usuario como elementos secundarios del siguiente elemento primario disponible en el árbol que tiene una representación del mismo nivel o elemento.

<span id="UI_Automation_and_UWP_process_boundaries"/>
<span id="ui_automation_and_uwp_process_boundaries"/>
<span id="UI_AUTOMATION_AND_UWP_PROCESS_BOUNDARIES"/>

## <a name="ui-automation-and-uwp-process-boundaries"></a>Automatización de la interfaz de usuario y límites del proceso de UWP  
Normalmente, el código de cliente de automatización de la interfaz de usuario que tiene acceso a una aplicación de UWP se ejecuta fuera de proceso. La infraestructura del marco de automatización de la interfaz de usuario permite a la información llegar a través del límite del proceso. Este concepto se explica con más detalle en [aspectos básicos](https://docs.microsoft.com/windows/desktop/WinAuto/entry-uiautocore-overview)de la automatización de la interfaz de usuario.

<span id="OnCreateAutomationPeer"/>
<span id="oncreateautomationpeer"/>
<span id="ONCREATEAUTOMATIONPEER"/>

## <a name="oncreateautomationpeer"></a>OnCreateAutomationPeer  
Todas las clases que derivan de [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) contienen el método virtual protegido [**OnCreateAutomationPeer**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.oncreateautomationpeer). La secuencia de inicialización de objetos para la automatización del mismo nivel llama a **OnCreateAutomationPeer** para obtener el objeto de automatización del mismo nivel para cada control y, por tanto, para construir un árbol de UI Automation para el uso en tiempo de ejecución. El código de automatización de la interfaz de usuario puede utilizar el elemento del mismo nivel para obtener información sobre las características y características de un control, y para simular el uso interactivo por medio de sus patrones de control. Un control personalizado que admite Automation debe invalidar **OnCreateAutomationPeer** y devolver una instancia de una clase que derive de [**AutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer). Por ejemplo, si un control personalizado se deriva de la clase [**ButtonBase**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ButtonBase) , el objeto devuelto por **OnCreateAutomationPeer** debe derivar de [**ButtonBaseAutomationPeer**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.automation.peers.buttonbaseautomationpeer).

Si está escribiendo una clase de control personalizada y pretende proporcionar también un nuevo elemento de automatización del mismo nivel, debe invalidar el método [**OnCreateAutomationPeer**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.oncreateautomationpeer) para el control personalizado de modo que devuelva una nueva instancia de su elemento del mismo nivel. La clase del mismo nivel debe derivar directa o indirectamente de [**AutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer).

Por ejemplo, el código siguiente declara que el control personalizado `NumericUpDown` debe usar el `NumericUpDownPeer` del mismo nivel para la automatización de la interfaz de usuario.

```csharp
using Windows.UI.Xaml.Automation.Peers;
...
public class NumericUpDown : RangeBase {
    public NumericUpDown() {
    // other initialization; DefaultStyleKey etc.
    }
    ...
    protected override AutomationPeer OnCreateAutomationPeer()
    {
        return new NumericUpDownAutomationPeer(this);
    }
}
```

```vb
Public Class NumericUpDown
    Inherits RangeBase
    ' other initialization; DefaultStyleKey etc.
       Public Sub New()
       End Sub
       Protected Overrides Function OnCreateAutomationPeer() As AutomationPeer
              Return New NumericUpDownAutomationPeer(Me)
       End Function
End Class
```

```cppwinrt
// NumericUpDown.idl
namespace MyNamespace
{
    runtimeclass NumericUpDown : Windows.UI.Xaml.Controls.Primitives.RangeBase
    {
        NumericUpDown();
        Int32 MyProperty;
    }
}

// NumericUpDown.h
...
struct NumericUpDown : NumericUpDownT<NumericUpDown>
{
    ...
    Windows::UI::Xaml::Automation::Peers::AutomationPeer OnCreateAutomationPeer()
    {
        return winrt::make<MyNamespace::implementation::NumericUpDownAutomationPeer>(*this);
    }
};
```

```cpp
//.h
public ref class NumericUpDown sealed : Windows::UI::Xaml::Controls::Primitives::RangeBase
{
// other initialization not shown
protected:
    virtual AutomationPeer^ OnCreateAutomationPeer() override
    {
         return ref new NumericUpDownAutomationPeer(this);
    }
};
```

> [!NOTE]
> La implementación de [**OnCreateAutomationPeer**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.oncreateautomationpeer) no debe hacer nada más que inicializar una nueva instancia de la automatización del mismo nivel personalizada, pasar el control de llamada como propietario y devolver esa instancia. No intente la lógica adicional en este método. En concreto, cualquier lógica que podría provocar la destrucción de [**AutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer) dentro de la misma llamada puede dar lugar a un comportamiento en tiempo de ejecución inesperado.

En las implementaciones típicas de [**OnCreateAutomationPeer**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.oncreateautomationpeer), el *propietario* se especifica como **this** o **me** porque la invalidación del método está en el mismo ámbito que el resto de la definición de clase del control.

La definición de clase del mismo nivel real se puede realizar en el mismo archivo de código que el control o en un archivo de código independiente. Todas las definiciones del mismo nivel existen en el espacio de nombres [**Windows. UI. Xaml. Automation. Peers**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers) , que es un espacio de nombres independiente de los controles para los que proporcionan elementos del mismo nivel. También puede optar por declarar los elementos del mismo nivel en espacios de nombres independientes, siempre y cuando haga referencia a los espacios de nombres necesarios para la llamada al método [**OnCreateAutomationPeer**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.oncreateautomationpeer) .

<span id="Choosing_the_correct_peer_base_class"/>
<span id="choosing_the_correct_peer_base_class"/>
<span id="CHOOSING_THE_CORRECT_PEER_BASE_CLASS"/>

### <a name="choosing-the-correct-peer-base-class"></a>Elección de la clase base del mismo nivel correcta  
Asegúrese de que el [**AutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer) se derive de una clase base que le proporcione la mejor coincidencia para la lógica del mismo nivel existente de la clase de control de la que se va a derivar. En el caso del ejemplo anterior, dado que `NumericUpDown` se deriva de [**RangeBase**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.RangeBase), hay una clase [**RangeBaseAutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.RangeBaseAutomationPeer) disponible en la que se debe basar el elemento del mismo nivel. Al utilizar la clase del mismo nivel coincidente más cercana en paralelo a cómo deriva el control en sí, puede evitar invalidar al menos parte de la funcionalidad de [**IRangeValueProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IRangeValueProvider) porque la clase base del mismo nivel ya la implementa.

La clase de [**control**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control) base no tiene una clase del mismo nivel correspondiente. Si necesita que una clase del mismo nivel se corresponda con un control personalizado que deriva de **control**, derive la clase personalizada del mismo nivel de [**FrameworkElementAutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer).

Si deriva directamente de [**ContentControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentControl) , esa clase no tiene ningún comportamiento predeterminado del mismo nivel de automatización porque no hay ninguna implementación de [**OnCreateAutomationPeer**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.oncreateautomationpeer) que haga referencia a una clase del mismo nivel. Por lo tanto, asegúrese de implementar **OnCreateAutomationPeer** para usar su propio equipo del mismo nivel o para usar [**FrameworkElementAutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer) como el elemento del mismo nivel si el nivel de compatibilidad de accesibilidad es adecuado para el control.

> [!NOTE]
> Normalmente no se deriva de [**AutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer) en lugar de [**FrameworkElementAutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer). Si ha derivado directamente de **AutomationPeer** , deberá duplicar una gran cantidad de compatibilidad básica de accesibilidad que, de otro modo, proviene de **FrameworkElementAutomationPeer**.

<span id="Initialization_of_a_custom_peer_class"/>
<span id="initialization_of_a_custom_peer_class"/>
<span id="INITIALIZATION_OF_A_CUSTOM_PEER_CLASS"/>

## <a name="initialization-of-a-custom-peer-class"></a>Inicialización de una clase personalizada del mismo nivel  
La automatización del mismo nivel debe definir un constructor con seguridad de tipos que use una instancia del control propietario para la inicialización base. En el ejemplo siguiente, la implementación pasa el valor *Owner* a la base [**RangeBaseAutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.RangeBaseAutomationPeer) y, en última instancia, es el [**FrameworkElementAutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer) que usa realmente *Owner* para establecer [ **FrameworkElementAutomationPeer. Owner**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.frameworkelementautomationpeer.owner).

```csharp
public NumericUpDownAutomationPeer(NumericUpDown owner): base(owner)
{}
```

```vb
Public Sub New(owner As NumericUpDown)
    MyBase.New(owner)
End Sub
```

```cppwinrt
// NumericUpDownAutomationPeer.idl
import "NumericUpDown.idl";
namespace MyNamespace
{
    runtimeclass NumericUpDownAutomationPeer : Windows.UI.Xaml.Automation.Peers.AutomationPeer
    {
        NumericUpDownAutomationPeer(NumericUpDown owner);
        Int32 MyProperty;
    }
}

// NumericUpDownAutomationPeer.h
...
struct NumericUpDownAutomationPeer : NumericUpDownAutomationPeerT<NumericUpDownAutomationPeer>
{
    ...
    NumericUpDownAutomationPeer(MyNamespace::NumericUpDown const& owner);
};
```

```cpp
//.h
public ref class NumericUpDownAutomationPeer sealed :  Windows::UI::Xaml::Automation::Peers::RangeBaseAutomationPeer
//.cpp
public:    NumericUpDownAutomationPeer(NumericUpDown^ owner);
```

<span id="Core_methods_of_AutomationPeer"/>
<span id="core_methods_of_automationpeer"/>
<span id="CORE_METHODS_OF_AUTOMATIONPEER"/>

## <a name="core-methods-of-automationpeer"></a>Métodos principales de AutomationPeer  
Para la infraestructura de UWP, los métodos reemplazables de un elemento de automatización del mismo nivel forman parte de un par de métodos: el método de acceso público que el proveedor de automatización de la interfaz de usuario usa como punto de reenvío para los clientes de automatización de la interfaz de usuario y el método de personalización "principal" protegido que una clase UWP puede invalidar para influir en el comportamiento. El par de métodos se conecta de forma predeterminada de modo que la llamada al método de acceso siempre invoca el método Parallel "Core" que tiene la implementación del proveedor, o como una reserva, invoca una implementación predeterminada de las clases base.

Al implementar un elemento del mismo nivel para un control personalizado, invalide cualquiera de los métodos "Core" de la clase base de automatización del mismo nivel en la que desea exponer el comportamiento que es único para el control personalizado. El código de automatización de la interfaz de usuario obtiene información sobre el control llamando a métodos públicos de la clase del mismo nivel. Para proporcionar información sobre el control, invalide cada método con un nombre que termine en "Core" cuando la implementación y el diseño del control crean escenarios de accesibilidad u otros escenarios de automatización de la interfaz de usuario que difieren de lo que admite la automatización base clase del mismo nivel.

Como mínimo, siempre que defina una nueva clase del mismo nivel, implemente el método [**GetClassNameCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getclassnamecore) , como se muestra en el ejemplo siguiente.

```csharp
protected override string GetClassNameCore()
{
    return "NumericUpDown";
}
```

> [!NOTE]
> Es posible que desee almacenar las cadenas como constantes en lugar de directamente en el cuerpo del método, pero eso depende de usted. En el caso de [**GetClassNameCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getclassnamecore), no necesitará localizar esta cadena. La propiedad **LocalizedControlType** se usa cada vez que un cliente de automatización de la interfaz de usuario necesita una cadena localizada, no **className**.

### <span id="GetAutomationControlType"/>
<span id="getautomationcontroltype"/>
<span id="GETAUTOMATIONCONTROLTYPE"/>GetAutomationControlType

Algunas tecnologías de asistencia usan el valor [**GetAutomationControlType**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getautomationcontroltype) directamente al notificar las características de los elementos en un árbol de automatización de la interfaz de usuario, como información adicional más allá del **nombre**de la automatización de la interfaz de usuario. Si el control es significativamente diferente del control del que se va a derivar y desea notificar a un tipo de control diferente de lo que notifica la clase base del mismo nivel utilizada por el control, debe implementar un elemento del mismo nivel e invalidar [**GetAutomationControlTypeCore** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getautomationcontroltypecore)en la implementación del mismo nivel. Esto es especialmente importante si se deriva de una clase base generalizada como [**ItemsControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsControl) o [**ContentControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentControl), donde el elemento base del mismo nivel no proporciona información precisa sobre el tipo de control.

La implementación de [**GetAutomationControlTypeCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getautomationcontroltypecore) describe el control devolviendo un valor de [**AutomationControlType**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType) . Aunque puede devolver **AutomationControlType. Custom**, debe devolver uno de los tipos de control más específicos si describe con precisión los escenarios principales del control. Este es un ejemplo.

```csharp
protected override AutomationControlType GetAutomationControlTypeCore()
{
    return AutomationControlType.Spinner;
}
```

> [!NOTE]
> A menos que especifique [**AutomationControlType. Custom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType), no tiene que implementar [**GetLocalizedControlTypeCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getlocalizedcontroltypecore) para proporcionar un valor de la propiedad **LocalizedControlType** a los clientes. La infraestructura común de UI Automation proporciona cadenas traducidas para cada valor de **AutomationControlType** posible que no sea **AutomationControlType. Custom**.

<span id="GetPattern_and_GetPatternCore"/>
<span id="getpattern_and_getpatterncore"/>
<span id="GETPATTERN_AND_GETPATTERNCORE"/>

### <a name="getpattern-and-getpatterncore"></a>GetPattern y GetPatternCore  
La implementación de [**GetPatternCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) de un elemento del mismo nivel devuelve el objeto que admite el patrón que se solicita en el parámetro de entrada. En concreto, un cliente de automatización de la interfaz de usuario llama a un método que se reenvía al método [**GetPattern**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpattern) del proveedor y especifica un valor de enumeración [**PatternInterface**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.PatternInterface) que nombra el patrón solicitado. La invalidación de **GetPatternCore** debe devolver el objeto que implementa el patrón especificado. Ese objeto es el propio elemento del mismo nivel, porque el elemento del mismo nivel debe implementar la interfaz de patrón correspondiente cada vez que informa de que admite un patrón. Si el elemento del mismo nivel no tiene una implementación personalizada de un patrón, pero sabe que la base del mismo nivel implementa el patrón, puede llamar a la implementación del tipo base de **getpatterncore** desde el **getpatterncore**. El valor de **GetPatternCore** de un elemento de mismo nivel debe devolver **null** si el elemento del mismo nivel no admite un patrón. Sin embargo, en lugar de devolver **null** directamente desde su implementación, normalmente se basaría en la llamada a la implementación base para devolver **null** para cualquier patrón no compatible.

Cuando se admite un patrón, la implementación de [**GetPatternCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) puede devolver **this** o **me**. La expectativa es que el cliente de automatización de la interfaz de usuario convierta el valor devuelto de [**GetPattern**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpattern) a la interfaz de patrón solicitada siempre que no sea **null**.

Si una clase del mismo nivel hereda de otro elemento del mismo nivel, y todos los informes de compatibilidad y patrón necesarios ya están controlados por la clase base, no es necesario implementar [**GetPatternCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) . Por ejemplo, si está implementando un control de intervalo que se deriva de [**RangeBase**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.RangeBase)y el elemento del mismo nivel deriva de [**RangeBaseAutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.RangeBaseAutomationPeer), ese elemento del mismo nivel se devuelve para [**PatternInterface. RangeValue**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.PatternInterface) y tiene implementaciones de trabajo de la interfaz [**IRangeValueProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IRangeValueProvider) que admite el patrón.

Aunque no es el código literal, este ejemplo se aproxima a la implementación de [**GetPatternCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) que ya está presente en [**RangeBaseAutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.RangeBaseAutomationPeer).


```csharp
protected override object GetPatternCore(PatternInterface patternInterface)
{
    if (patternInterface == PatternInterface.RangeValue)
    {
        return this;
    }
    return base.GetPattern(patternInterface);
}
```

Si está implementando un elemento del mismo nivel en el que no tiene toda la compatibilidad que necesita de una clase base del mismo nivel o desea cambiar o agregar al conjunto de patrones heredados de base que el mismo nivel admite, debe invalidar [**GetPatternCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) para habilitar la automatización de la interfaz de usuario. clientes para usar los patrones.

Para obtener una lista de los patrones de proveedor que están disponibles en la implementación de UWP de la compatibilidad de UI Automation, vea [**Windows. UI. Xaml. Automation. Provider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider). Cada patrón de este tipo tiene un valor correspondiente de la enumeración [**PatternInterface**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.PatternInterface) , que es cómo los clientes de automatización de la interfaz de usuario solicitan el patrón en una llamada [**GetPattern**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpattern) .

Un elemento del mismo nivel puede informar de que admite más de un patrón. Si es así, la invalidación debe incluir la lógica de la ruta de acceso devuelta para cada valor de [**PatternInterface**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.PatternInterface) compatible y devolver el elemento del mismo nivel en cada caso coincidente. Se espera que el autor de la llamada solicite solo una interfaz cada vez, y depende del llamador convertirse en la interfaz esperada.

Este es un ejemplo de una invalidación de [**GetPatternCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) para un elemento del mismo nivel personalizado. Informa de la compatibilidad con dos patrones, [**IRangeValueProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IRangeValueProvider) y [**IToggleProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IToggleProvider). Este control es un control de pantalla multimedia que se puede mostrar como pantalla completa (modo de alternancia) y que tiene una barra de progreso en la que los usuarios pueden seleccionar una posición (el control de intervalo). Este código proviene del [ejemplo de accesibilidad de XAML](https://go.microsoft.com/fwlink/p/?linkid=238570).


```csharp
protected override object GetPatternCore(PatternInterface patternInterface)
{
    if (patternInterface == PatternInterface.RangeValue)
    {
        return this;
    }
    else if (patternInterface == PatternInterface.Toggle)
    {
        return this;
    }
    return null;
}
```

<span id="Forwarding_patterns_from_sub-elements"/>
<span id="forwarding_patterns_from_sub-elements"/>
<span id="FORWARDING_PATTERNS_FROM_sub-elementS"/>

### <a name="forwarding-patterns-from-sub-elements"></a>Reenviar patrones de subelementos  
Una implementación de método [**GetPatternCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) también puede especificar un subelemento o una parte como un proveedor de patrones para su host. En este ejemplo se imita el modo en que [**ItemsControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsControl) transfiere el control del patrón de desplazamiento al elemento del mismo nivel de su control [**ScrollViewer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) interno. Para especificar un subelemento para el control de patrones, este código obtiene el objeto de subelemento, crea un elemento del mismo nivel para el subelemento mediante el método [**FrameworkElementAutomationPeer. CreatePeerForElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.frameworkelementautomationpeer.createpeerforelement) y devuelve el nuevo elemento del mismo nivel.


```csharp
protected override object GetPatternCore(PatternInterface patternInterface)
{
    if (patternInterface == PatternInterface.Scroll)
    {
        ItemsControl owner = (ItemsControl) base.Owner;
        UIElement itemsHost = owner.ItemsHost;
        ScrollViewer element = null;
        while (itemsHost != owner)
        {
            itemsHost = VisualTreeHelper.GetParent(itemsHost) as UIElement;
            element = itemsHost as ScrollViewer;
            if (element != null)
            {
                break;
            }
        }
        if (element != null)
        {
            AutomationPeer peer = FrameworkElementAutomationPeer.CreatePeerForElement(element);
            if ((peer != null) && (peer is IScrollProvider))
            {
                return (IScrollProvider) peer;
            }
        }
    }
    return base.GetPatternCore(patternInterface);
}
```

<span id="Other_Core_methods"/>
<span id="other_core_methods"/>
<span id="OTHER_CORE_METHODS"/>

### <a name="other-core-methods"></a>Otros métodos principales  
Es posible que el control necesite admitir equivalentes de teclado en escenarios principales; para obtener más información sobre por qué esto podría ser necesario, vea [accesibilidad del teclado](keyboard-accessibility.md). Implementar la compatibilidad con claves es necesariamente parte del código de control y no del código del mismo nivel porque forma parte de la lógica de un control, pero la clase del mismo nivel debe reemplazar los métodos [**GetAcceleratorKeyCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getacceleratorkeycore) y [**GetAccessKeyCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getaccesskeycore) para notificar a la interfaz de usuario. Clientes de Automation que usan claves. Tenga en cuenta que es posible que las cadenas que notifican información de clave deban estar localizadas y, por tanto, provienen de recursos, no de cadenas codificadas de forma rígida.

Si proporciona un elemento del mismo nivel para una clase que admite una colección, es mejor derivar de las clases funcionales y de las clases del mismo nivel que ya tienen ese tipo de compatibilidad con colecciones. Si no puede hacerlo, los elementos del mismo nivel para los controles que mantienen colecciones secundarias pueden tener que invalidar el método del mismo nivel relacionado con la colección [**GetChildrenCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getchildrencore) para notificar correctamente las relaciones de elementos primarios y secundarios en el árbol de automatización de la interfaz de usuario.

Implemente los métodos [**IsContentElementCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.iscontentelementcore) y [**IsControlElementCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.iscontrolelementcore) para indicar si el control contiene contenido de datos o si cumple un rol interactivo en la interfaz de usuario (o ambos). De forma predeterminada, ambos métodos devuelven **true**. Estas opciones mejoran la facilidad de uso de las tecnologías de asistencia, como los lectores de pantalla, que pueden utilizar estos métodos para filtrar el árbol de automatización. Si el método [**GetPatternCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) transfiere el control del patrón a un subelemento del mismo nivel, el método **IsControlElementCore** del subelemento del mismo nivel puede devolver **false** para ocultar el subelemento del mismo nivel del árbol de automatización.

Algunos controles pueden admitir escenarios de etiquetado, donde una parte de la etiqueta de texto proporciona información para una parte que no es de texto, o un control está pensado para estar en una relación de etiqueta conocida con otro control en la interfaz de usuario. Si es posible proporcionar un comportamiento práctico basado en clases, puede invalidar [**GetLabeledByCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getlabeledbycore) para proporcionar este comportamiento.

[**GetBoundingRectangleCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getboundingrectanglecore) y [**GetClickablePointCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getclickablepointcore) se usan principalmente para escenarios de pruebas automatizadas. Si desea admitir las pruebas automatizadas para el control, puede que desee invalidar estos métodos. Esto podría ser deseable para los controles de tipo de intervalo, donde no se puede sugerir un solo punto, ya que el lugar en el que el usuario hace clic en el espacio de coordenadas tiene un efecto diferente en un intervalo. Por ejemplo, el elemento de automatización de [**ScrollBar**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ScrollBar) predeterminado invalida **GetClickablePointCore** para devolver un valor de [**punto**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point) "no es un número".

[**GetLiveSettingCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getlivesettingcore) influye en el valor predeterminado del control para el valor de **LiveSetting** para la automatización de la interfaz de usuario. Es posible que desee invalidar esta opción si desea que el control devuelva un valor distinto de [**AutomationLiveSetting. OFF**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationLiveSetting). Para obtener más información sobre qué representa **LiveSetting** , consulte [**AutomationProperties. LiveSetting**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.livesettingproperty).

Podría invalidar [**GetOrientationCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getorientationcore) si el control tiene una propiedad de orientación configurable que se puede asignar a [**AutomationOrientation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationOrientation). Las clases [**ScrollBarAutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.ScrollBarAutomationPeer) y [**SliderAutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.SliderAutomationPeer) lo hacen.

<span id="Base_implementation_in_FrameworkElementAutomationPeer"/>
<span id="base_implementation_in_frameworkelementautomationpeer"/>
<span id="BASE_IMPLEMENTATION_IN_FRAMEWORKELEMENTAUTOMATIONPEER"/>

### <a name="base-implementation-in-frameworkelementautomationpeer"></a>Implementación base en FrameworkElementAutomationPeer  
La implementación base de [**FrameworkElementAutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer) proporciona cierta información de automatización de la interfaz de usuario que se puede interpretar desde diversas propiedades de diseño y comportamiento que se definen en el nivel de marco.

* [**GetBoundingRectangleCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getboundingrectanglecore): devuelve una estructura [**Rect**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Rect) basada en las características de diseño conocidas. Devuelve un **Rect** de valor 0 si [**IsOffscreen**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.isoffscreen) es **true**.
* [**GetClickablePointCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getclickablepointcore): devuelve una estructura de [**punto**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point) en función de las características de diseño conocidas, siempre y cuando haya un **BoundingRectangle**distinto de cero.
* [**GetNameCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getnamecore): un comportamiento más amplio del que se puede resumir aquí; vea [**GetNameCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getnamecore). Básicamente, intenta realizar una conversión de cadena en cualquier contenido conocido de un [**ContentControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentControl) o en clases relacionadas que tienen contenido. Además, si hay un valor para [**LabeledBy**](https://docs.microsoft.com/previous-versions/windows/silverlight/dotnet-windows-silverlight/ms591292(v=vs.95)), el valor del **nombre** de ese elemento se usa como **nombre**.
* [**HasKeyboardFocusCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.haskeyboardfocuscore): se evalúa en función de las propiedades [**FocusState**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.focusstate) y [**IsEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.isenabled) del propietario. Los elementos que no son controles siempre devuelven **false**.
* [**IsEnabledCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.isenabledcore): se evalúa según la propiedad [**IsEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.isenabled) del propietario si es un [**control**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control). Los elementos que no son controles siempre devuelven **true**. Esto no significa que el propietario esté habilitado en el sentido de interacción convencional; significa que el elemento del mismo nivel está habilitado a pesar de que el propietario no tiene una propiedad **IsEnabled** .
* [**IsKeyboardFocusableCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.iskeyboardfocusablecore): devuelve **true** si el propietario es un [**control**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control); en caso contrario, es **false**.
* [**IsOffscreenCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.isoffscreencore): la [**visibilidad**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.visibility) [**contraída**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.visibility) en el elemento propietario o cualquiera de sus elementos primarios equivale a un valor **true** para [**IsOffscreen**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.isoffscreen). Excepción: un objeto [**emergente**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.Popup) puede estar visible incluso si no lo son los elementos primarios de su propietario.
* [**SetFocusCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.setfocuscore): llama al [**foco**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.focus).
* [**GetParent**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getparent): llama a [**FrameworkElement. Parent**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.parent) desde el propietario y busca el elemento del mismo nivel adecuado. No se trata de un par de invalidación con un método "Core", por lo que no puede cambiar este comportamiento.

> [!NOTE]
> Los pares de UWP predeterminados implementan un comportamiento mediante el uso de código nativo interno que implementa la UWP, no necesariamente con el código UWP real. No podrá ver el código o la lógica de la implementación a través de la reflexión Common Language Runtime (CLR) u otras técnicas. Tampoco verá páginas de referencia distintas para invalidaciones específicas de subclases del comportamiento del mismo nivel base. Por ejemplo, puede haber un comportamiento adicional para [**GetNameCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getnamecore) de un [**TextBoxAutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.TextBoxAutomationPeer), que no se describe en la página de referencia de **AutomationPeer. GetNameCore** , y no hay ninguna página de referencia para  **TextBoxAutomationPeer. GetNameCore**. No hay ninguna página de referencia de **TextBoxAutomationPeer. GetNameCore** . En su lugar, lea el tema de referencia de la clase del mismo nivel más inmediata y busque las notas de implementación en la sección Comentarios.

<span id="Peers_and_AutomationProperties"/>
<span id="peers_and_automationproperties"/>
<span id="PEERS_AND_AUTOMATIONPROPERTIES"/>

## <a name="peers-and-automationproperties"></a>Pares y AutomationProperties  
La automatización del mismo nivel debe proporcionar los valores predeterminados adecuados para la información relacionada con la accesibilidad del control. Tenga en cuenta que cualquier código de aplicación que use el control puede invalidar parte de ese comportamiento incluyendo los valores de propiedad asociados de [**AutomationProperties**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.AutomationProperties) en instancias de control. Los llamadores pueden hacer esto para los controles predeterminados o para los controles personalizados. Por ejemplo, el siguiente código XAML crea un botón que tiene dos propiedades de automatización de la interfaz de usuario personalizadas: `<Button AutomationProperties.Name="Special"      AutomationProperties.HelpText="This is a special button."/>`

Para obtener más información sobre las propiedades adjuntas de [**AutomationProperties**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.AutomationProperties) , consulte [información básica de accesibilidad](basic-accessibility-information.md).

Algunos de los métodos de [**AutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer) existen debido al contrato general de cómo se espera que los proveedores de automatización de la interfaz de usuario informen de la información, pero estos métodos no se implementan normalmente en el control del mismo nivel. Esto se debe a que se espera que la información la proporcionen los valores [**AutomationProperties**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.AutomationProperties) aplicados al código de la aplicación que usa los controles en una interfaz de usuario específica. Por ejemplo, la mayoría de las aplicaciones definirían la relación de etiquetado entre dos controles diferentes en la interfaz de usuario aplicando un valor [**AutomationProperties. LabeledBy**](https://docs.microsoft.com/previous-versions/windows/silverlight/dotnet-windows-silverlight/ms591292(v=vs.95)) . Sin embargo, [**LabeledByCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getlabeledbycore) se implementa en ciertos elementos del mismo nivel que representan las relaciones de datos o elementos de un control, como el uso de una parte de encabezado para etiquetar una parte de campo de datos, etiquetar elementos con sus contenedores o escenarios similares.

<span id="Implementing_patterns"/>
<span id="implementing_patterns"/>
<span id="IMPLEMENTING_PATTERNS"/>

## <a name="implementing-patterns"></a>Implementar patrones  
Echemos un vistazo a cómo escribir un elemento del mismo nivel para un control que implementa un comportamiento de expansión y contracción implementando la interfaz de patrón de control para expandir y contraer. El elemento del mismo nivel debe habilitar la accesibilidad para el comportamiento de expansión y contracción devolviendo cada vez que se llama a [**GetPattern**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getpattern) con un valor de [**PatternInterface. ExpandCollapse**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.PatternInterface). A continuación, el elemento del mismo nivel debe heredar la interfaz del proveedor para ese patrón ([**IExpandCollapseProvider**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.provider.iexpandcollapseprovider)) y proporcionar implementaciones para cada uno de los miembros de la interfaz del proveedor. En este caso, la interfaz tiene tres miembros para invalidar: [**Expand**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.provider.iexpandcollapseprovider.expand), [**Collapse**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.provider.iexpandcollapseprovider.collapse), [**ExpandCollapseState**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.provider.iexpandcollapseprovider.expandcollapsestate).

Resulta útil planear de antemano la accesibilidad en el diseño de la API de la propia clase. Siempre que tenga un comportamiento que puede solicitarse como resultado de las interacciones típicas con un usuario que esté trabajando en la interfaz de usuario o a través de un patrón de proveedor de automatización, proporcione un método único que pueda llamar a la respuesta de la interfaz de usuario o al patrón de automatización. Por ejemplo, si el control tiene partes de botón que tienen controladores de eventos con cable que pueden expandir o contraer el control y tienen equivalentes de teclado para esas acciones, haga que estos controladores de eventos llamen al mismo método al que se llama desde dentro del cuerpo [**de la** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.provider.iexpandcollapseprovider.expand)o [**contraiga**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.provider.iexpandcollapseprovider.collapse) las implementaciones para [**IExpandCollapseProvider**](https://docs.microsoft.com/windows/desktop/api/uiautomationcore/nn-uiautomationcore-iexpandcollapseprovider) en el elemento del mismo nivel. El uso de un método lógico común también puede ser una forma útil de asegurarse de que los Estados visuales del control se actualizan para mostrar el estado lógico de forma uniforme, independientemente de cómo se invoque el comportamiento.

Una implementación típica es que las API de proveedor llaman primero a [**Owner**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.frameworkelementautomationpeer.owner) para el acceso a la instancia del control en tiempo de ejecución. A continuación, se puede llamar a los métodos de comportamiento necesarios en ese objeto.


```csharp
public class IndexCardAutomationPeer : FrameworkElementAutomationPeer, IExpandCollapseProvider {
    private IndexCard ownerIndexCard;
    public IndexCardAutomationPeer(IndexCard owner) : base(owner)
    {
         ownerIndexCard = owner;
    }
}
```

Una implementación alternativa es que el propio control puede hacer referencia a su elemento del mismo nivel. Se trata de un patrón común si se están generando eventos de automatización desde el control, porque el método [**RaiseAutomationEvent**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.raiseautomationevent) es un método del mismo nivel.

<span id="UI_Automation_events"/>
<span id="ui_automation_events"/>
<span id="UI_AUTOMATION_EVENTS"/>

## <a name="ui-automation-events"></a>Eventos de UI Automation  

Los eventos de automatización de la interfaz de usuario se dividen en las siguientes categorías.

| Evento | Description |
|-------|-------------|
| Cambio de propiedad | Se desencadena cuando cambia una propiedad en un patrón de control o elemento de automatización de la interfaz de usuario. Por ejemplo, si un cliente necesita supervisar un control de casilla de la aplicación, se puede registrar para escuchar un evento de cambio de propiedad en la propiedad [**ToggleState**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.provider.itoggleprovider.togglestate) . Cuando el control de casilla está activado o desactivado, el proveedor activa el evento y el cliente puede actuar según sea necesario. |
| Acción de elemento | Se desencadena cuando se produce un cambio en la interfaz de usuario de la actividad de usuario o mediante programación; por ejemplo, cuando se hace clic en un botón o se invoca mediante el patrón de **invocación** . |
| Cambio de estructura | Se desencadena cuando cambia la estructura del árbol de automatización de la interfaz de usuario. La estructura cambia cuando se muestran, se ocultan o se quitan nuevos elementos de la interfaz de usuario en el escritorio. |
| Cambio global | Se desencadena cuando se producen acciones de interés global en el cliente, por ejemplo, cuando el foco cambia de un elemento a otro o cuando se cierra una ventana secundaria. Algunos eventos no implican necesariamente que el estado de la interfaz de usuario haya cambiado. Por ejemplo, si el usuario realiza una pestaña en un campo de entrada de texto y, a continuación, hace clic en un botón para actualizar el campo, se desencadena un evento de [**TextChanged**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.textchanged) incluso si el usuario no cambió realmente el texto. Al procesar un evento, puede ser necesario que una aplicación cliente Compruebe si realmente ha cambiado antes de realizar alguna acción. |

<span id="AutomationEvents_identifiers"/>
<span id="automationevents_identifiers"/>
<span id="AUTOMATIONEVENTS_IDENTIFIERS"/>

### <a name="automationevents-identifiers"></a>Identificadores de AutomationEvents  
Los eventos de automatización de la interfaz de usuario se identifican mediante valores [**AutomationEvents**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationEvents) . Los valores de la enumeración identifican de forma única el tipo de evento.

<span id="Raising_events"/>
<span id="raising_events"/>
<span id="RAISING_EVENTS"/>

### <a name="raising-events"></a>Provocar eventos  
Los clientes de automatización de la interfaz de usuario pueden suscribirse a eventos de automatización. En el modelo de automatización del mismo nivel, los elementos del mismo nivel para controles personalizados deben notificar los cambios en el estado del control que son relevantes para la accesibilidad llamando al método [**RaiseAutomationEvent**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.raiseautomationevent) . Del mismo modo, cuando cambia el valor de propiedad de automatización de la interfaz de usuario clave, los pares de controles personalizados deben llamar al método [**RaisePropertyChangedEvent**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.raisepropertychangedevent) .

En el ejemplo de código siguiente se muestra cómo obtener el objeto del mismo nivel desde el código de definición del control y llamar a un método para desencadenar un evento desde ese mismo nivel. Como medida de optimización, el código determina si hay agentes de escucha para este tipo de evento. La activación del evento y la creación del objeto del mismo nivel solo cuando hay agentes de escucha evita la sobrecarga innecesaria y ayuda a que el control siga respondiendo.


```csharp
if (AutomationPeer.ListenerExists(AutomationEvents.PropertyChanged))
{
    NumericUpDownAutomationPeer peer =
        FrameworkElementAutomationPeer.FromElement(nudCtrl) as NumericUpDownAutomationPeer;
    if (peer != null)
    {
        peer.RaisePropertyChangedEvent(
            RangeValuePatternIdentifiers.ValueProperty,
            (double)oldValue,
            (double)newValue);
    }
}
```

<span id="Peer_navigation"/>
<span id="peer_navigation"/>
<span id="PEER_NAVIGATION"/>

## <a name="peer-navigation"></a>Navegación del mismo nivel  
Después de encontrar un elemento de automatización del mismo nivel, un cliente de automatización de la interfaz de usuario puede navegar por la estructura del mismo nivel de una aplicación mediante una llamada a los métodos [**GetChildren**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getchildren) y [**GetParent**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getparent) del objeto del mismo nivel. La navegación entre los elementos de la interfaz de usuario dentro de un control es compatible con la implementación del mismo nivel del método [**GetChildrenCore**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.automationpeer.getchildrencore) . El sistema de automatización de la interfaz de usuario llama a este método para crear un árbol de subelementos incluidos dentro de un control. por ejemplo, los elementos de lista de un cuadro de lista. El método **GetChildrenCore** predeterminado en [**FrameworkElementAutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer) atraviesa el árbol visual de elementos para generar el árbol de elementos de automatización del mismo nivel. Los controles personalizados pueden invalidar este método para exponer una representación diferente de los elementos secundarios a los clientes de automatización, devolviendo los elementos del mismo nivel de automatización de los elementos que transmiten información o permiten la interacción del usuario.

<span id="Native_automation_support_for_text_patterns"/>
<span id="native_automation_support_for_text_patterns"/>
<span id="NATIVE_AUTOMATION_SUPPORT_FOR_TEXT_PATTERNS"/>

## <a name="native-automation-support-for-text-patterns"></a>Compatibilidad de automatización nativa con patrones de texto  
Algunos de los pares de automatización de aplicaciones de UWP predeterminados proporcionan compatibilidad con el patrón de control para el patrón de texto ([**PatternInterface. Text**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.PatternInterface)). Pero proporcionan esta compatibilidad a través de métodos nativos y los elementos del mismo nivel implicados no notarán la interfaz [**ITextProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.ITextProvider) en la herencia (administrada). Aun así, si un cliente de automatización de la interfaz de usuario administrado o no administrado consulta los patrones del mismo nivel, notificará la compatibilidad con el patrón de texto y proporcionará un comportamiento para las partes del patrón cuando se llame a las API de cliente.

Si piensa derivar de uno de los controles de texto de la aplicación para UWP y también crea un elemento del mismo nivel personalizado que deriva de uno de los elementos del mismo nivel relacionados con el texto, consulte las secciones de comentarios del elemento del mismo nivel para obtener más información sobre cualquier compatibilidad de nivel nativo con patrones. Puede tener acceso al comportamiento base nativo en el elemento personalizado del mismo nivel si llama a la implementación base desde las implementaciones de la interfaz del proveedor administrado, pero es difícil modificar lo que hace la implementación base porque las interfaces nativas en el mismo nivel y en su el control propietario no está expuesto. En general, debe usar las implementaciones base tal cual (solo para la base de llamada) o reemplazar por completo la funcionalidad por su propio código administrado y no llamar a la implementación base. Este último es un escenario avanzado, por lo que deberá estar familiarizado con el marco de trabajo de servicios de texto que utiliza el control para admitir los requisitos de accesibilidad al usar ese marco.

<span id="AutomationProperties.AccessibilityView"/>
<span id="automationproperties.accessibilityview"/>
<span id="AUTOMATIONPROPERTIES.ACCESSIBILITYVIEW"/>

## <a name="automationpropertiesaccessibilityview"></a>AutomationProperties. AccessibilityView  
Además de proporcionar un elemento del mismo nivel personalizado, también puede ajustar la representación de la vista de árbol para cualquier instancia del control estableciendo [**AutomationProperties. AccessibilityView**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.accessibilityview) en XAML. Esto no se implementa como parte de una clase del mismo nivel, pero lo mencionaremos aquí porque es de alemán a soporte general de accesibilidad para controles personalizados o para plantillas que se personalizan.

El escenario principal para usar **AutomationProperties. AccessibilityView** es omitir deliberadamente determinados controles de una plantilla de las vistas de automatización de la interfaz de usuario, ya que no contribuyen de forma significativa a la vista de accesibilidad de todo el control. Para evitarlo, establezca **AutomationProperties. AccessibilityView** en "RAW".

<span id="Throwing_exceptions_from_automation_peers"/>
<span id="throwing_exceptions_from_automation_peers"/>
<span id="THROWING_EXCEPTIONS_FROM_AUTOMATION_PEERS"/>

## <a name="throwing-exceptions-from-automation-peers"></a>Producir excepciones desde elementos de automatización del mismo nivel  
Las API que está implementando para la compatibilidad con la automatización del mismo nivel pueden producir excepciones. Se espera que los clientes de automatización de la interfaz de usuario que están escuchando sean lo suficientemente sólidos como para continuar después de que se produzca la mayoría de las excepciones. En todo, es probable que el agente de escucha esté examinando un árbol de automatización de todo el mundo que incluye aplicaciones distintas de las suyas, y es un diseño de cliente inaceptable para deshacer todo el cliente simplemente porque un área del árbol produjo una excepción basada en el mismo nivel cuando el cliente llamó a sus API.

En el caso de los parámetros que se pasan a su elemento del mismo nivel, es aceptable validar la entrada y, por ejemplo, Throw [**ArgumentNullException**](https://docs.microsoft.com/dotnet/api/system.argumentnullexception) si se pasó como **null** y no es un valor válido para la implementación. Sin embargo, si hay operaciones posteriores realizadas por el elemento del mismo nivel, recuerde que las interacciones del mismo nivel con el control de hospedaje tienen algo de un carácter asincrónico. Cualquier elemento del mismo nivel no bloquea necesariamente el subproceso de la interfaz de usuario en el control (y probablemente no debería). Por lo tanto, puede tener situaciones en las que un objeto estaba disponible o tenía determinadas propiedades cuando se creó el elemento del mismo nivel o cuando se llamó por primera vez a un método de automatización del mismo nivel, pero mientras tanto el estado del control ha cambiado. En estos casos, hay dos excepciones dedicadas que un proveedor puede iniciar:

* Inicie [**ElementNotAvailableException**](https://docs.microsoft.com/dotnet/api/system.windows.automation.elementnotavailableexception) si no puede tener acceso al propietario del mismo nivel o a un elemento relacionado del mismo nivel en función de la información original que se ha pasado a la API. Por ejemplo, podría tener un elemento del mismo nivel que está intentando ejecutar sus métodos pero el propietario ya se ha quitado de la interfaz de usuario, como un cuadro de diálogo modal que se ha cerrado. Para un cliente de non-.NET, se asigna a [**UIA\_E\_ELEMENTNOTAVAILABLE**](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-error-codes).
* Inicie [**ElementNotEnabledException**](https://docs.microsoft.com/dotnet/api/system.windows.automation.elementnotenabledexception) si todavía existe un propietario, pero ese propietario está en un modo como [**IsEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.isenabled)`=`**false** que bloquea algunos de los cambios de programación específicos que el elemento del mismo nivel está intentando realizar. Para un cliente de non-.NET, se asigna a [**UIA\_E\_ELEMENTNOTENABLED**](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-error-codes).

Más allá de esto, los pares deben ser relativamente conservadores en lo que respecta a las excepciones que inician desde su compatibilidad del mismo nivel. La mayoría de los clientes no podrán controlar las excepciones de los elementos del mismo nivel y convertirlas en opciones procesables que los usuarios pueden realizar al interactuar con el cliente. A veces, un valor no operativo y la detección de excepciones sin volver a producir en las implementaciones del mismo nivel es una estrategia mejor que producir excepciones cada vez que algo que el elemento del mismo nivel intenta no funcionar. También tenga en cuenta que la mayoría de los clientes de automatización de la interfaz de usuario no están escritos en código administrado. La mayoría están escritas en COM y simplemente están comprobando si hay **\_aceptar** en un **valor HRESULT** cada vez que llaman a un método de cliente de automatización de la interfaz de usuario que termina de obtener acceso al mismo nivel.

<span id="related_topics"/>

## <a name="related-topics"></a>Temas relacionados  
* [Consejos](accessibility.md)
* [Ejemplo de accesibilidad XAML](https://go.microsoft.com/fwlink/p/?linkid=238570)
* [**FrameworkElementAutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.FrameworkElementAutomationPeer)
* [**AutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer)
* [**OnCreateAutomationPeer**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.oncreateautomationpeer)
* [Patrones e interfaces de control](control-patterns-and-interfaces.md)
