---
author: Xansky
Description: Describes the concept of automation peers for Microsoft UI Automation, and how you can provide automation support for your own custom UI class.
ms.assetid: AA8DA53B-FE6E-40AC-9F0A-CB09637C87B4
title: Automatización del mismo nivel personalizada
label: Custom automation peers
template: detail.hbs
ms.author: mhopkins
ms.date: 09/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2bab0ac8b89815a67be2c963979b3712f022248b
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/15/2018
ms.locfileid: "1656570"
---
# <a name="custom-automation-peers"></a>Automatización del mismo nivel personalizada  

Describe el concepto de automatización del mismo nivel para la Automatización de la interfaz de usuario de Microsoft y cómo puedes proporcionar compatibilidad de automatización para tu propia clase de interfaz de usuario personalizada.

La automatización de la interfaz de usuario proporciona un marco que los clientes de automatización pueden usar para examinar o usar las interfaces de usuario de una variedad de marcos y plataformas de interfaz de usuario. Si estás escribiendo una aplicación para la Plataforma universal de Windows (UWP), las clases que usas para la interfaz de usuario ya proporcionan compatibilidad para la automatización de la interfaz de usuario. Puedes derivar de clases no selladas existentes para definir un nuevo tipo de control de la interfaz de usuario o clase de compatibilidad. Durante ese proceso, es posible que tu clase incorpore un comportamiento que incluya compatibilidad para accesibilidad pero que la compatibilidad predeterminada para la automatización de la interfaz de usuario no abarca. En este caso, debes ampliar la compatibilidad para la Automatización de la interfaz de usuario existente. Para ello, debes derivar de la clase [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) usada por la implementación base, agregar la compatibilidad necesaria a tu implementación del mismo nivel e informar a la infraestructura de control de la Plataforma universal de Windows (UWP) que debe crear tu sistema del mismo nivel.

La automatización de la interfaz de usuario no solo permite el uso de aplicaciones de accesibilidad y tecnologías de asistencia, como los lectores de pantalla, sino también el código de control de calidad (prueba). En ambos escenarios, los clientes de automatización de la interfaz de usuario pueden examinar los elementos de la interfaz de usuario y simular la interacción del usuario con tu aplicación desde otro código que no sea el de la aplicación. Para obtener información acerca de la automatización de la interfaz de usuario en todas las plataformas y en su sentido más amplio, consulta la [introducción a la automatización de la interfaz de usuario](https://msdn.microsoft.com/library/windows/desktop/Ee684076).

Existen dos públicos diferentes que usan el marco de automatización de la interfaz de usuario.

* Los **clientes* de automatización de la interfaz de usuario*** llaman a las API de automatización de la interfaz de usuario para obtener información acerca de todas las interfaces de usuario que se muestran actualmente al usuario. Por ejemplo, una tecnología de asistencia como un lector de pantalla actúa como un cliente de automatización de la interfaz de usuario. La interfaz de usuario se presenta como un árbol de elementos de automatización que se relacionan. El cliente de automatización de la interfaz de usuario probablemente esté interesado solamente en una aplicación por vez o en todo el árbol. El cliente de automatización de la interfaz de usuario puede usar API de automatización de la interfaz de usuario para navegar en el árbol y leer o cambiar información en los elementos de automatización.
* Los **proveedores de automatización *de la interfaz de usuario*** proporcionan información al árbol de automatización de la interfaz de usuario implementando API que exponen los elementos en la interfaz de usuario que introdujeron como parte de su aplicación. Cuando crees un nuevo control, ahora debes actuar como partícipe del escenario de proveedor de automatización de la interfaz de usuario. Como proveedor, debes asegurarte de que todos los clientes de automatización de la interfaz de usuario puedan usar el marco de trabajo de automatización de la interfaz de usuario para interactuar con tu control, tanto por motivos de accesibilidad como de pruebas.

Generalmente, existen API paralelas en el marco de trabajo de Automatización de la interfaz de usuario: una API para clientes de Automatización de la interfaz de usuario y otra API con nombre similar para los proveedores de Automatización de la interfaz de usuario. La mayor parte de este tema se centra en las API del proveedor de automatización de la interfaz de usuario, concretamente en las clases e interfaces que permiten la extensibilidad del proveedor en ese marco de trabajo de la interfaz de usuario. En ocasiones, para ofrecer algo de perspectiva, mencionamos las API de automatización de la interfaz de usuario que los clientes de automatización de la interfaz de usuario usan o proporcionamos una tabla de búsqueda que relaciona las API de proveedor y cliente. Para más información sobre la perspectiva del cliente, consulta la [Guía del programador de clientes de automatización de la interfaz de usuario](https://msdn.microsoft.com/library/windows/desktop/Ee684021).

> [!NOTE]
> Normalmente, los clientes de automatización de la interfaz de usuario no usan código administrado y no se implementan como una aplicación para UWP(suelen ser aplicaciones de escritorio). La automatización de la interfaz de usuario se basa en un estándar y no en una implementación o marco específicos. Muchos clientes de automatización de la interfaz de usuario existentes, incluidos los productos de tecnología de asistencia como los lectores de pantalla, usan interfaces del Modelo de objetos componentes (COM) para interactuar con la automatización de la interfaz de usuario, el sistema y las aplicaciones que se ejecutan en ventanas secundarias. Para obtener más información sobre las interfaces COM y cómo escribir un cliente de automatización de la interfaz de usuario mediante COM, consulta los [Conceptos básicos sobre la automatización de la interfaz de usuario](https://msdn.microsoft.com/library/windows/desktop/Ee684007).

<span id="Determining_the_existing_state_of_UI_Automation_support_for_your_custom_UI_class"/>
<span id="determining_the_existing_state_of_ui_automation_support_for_your_custom_ui_class"/>
<span id="DETERMINING_THE_EXISTING_STATE_OF_UI_AUTOMATION_SUPPORT_FOR_YOUR_CUSTOM_UI_CLASS"/>

## <a name="determining-the-existing-state-of-ui-automation-support-for-your-custom-ui-class"></a>Determinar el estado actual de la compatibilidad para automatización de la interfaz de usuario para tu clase de interfaz de usuario personalizada  
Antes de intentar implementar un sistema de automatización del mismo nivel para un control personalizado, debes probar si la clase base y su sistema de automatización del mismo nivel proporcionan la compatibilidad para accesibilidad o automatización que necesitas. En muchos casos, la combinación de las implementaciones [**FrameworkElementAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242472), los pares específicos y los modelos que implementan pueden proporcionar una experiencia de accesibilidad básica y, a la vez, satisfactoria. Que esto sea cierto depende de la cantidad de cambios que hayas implementado en la exposición del modelo de objetos al control en comparación con su clase base. Además, esto depende de si las incorporaciones a la funcionalidad de la clase base se corresponden con los nuevos elementos de la interfaz de usuario en el contrato de la plantilla o con la apariencia visual del control. En algunos casos, tus cambios puede incorporar nuevos aspectos de la experiencia del usuario que requieren una compatibilidad para accesibilidad adicional.

Incluso si al usar la clase base del mismo nivel se proporciona compatibilidad para accesibilidad básica, sigue considerándose una práctica recomendada definir un sistema del mismo nivel para que puedas proporcionar información precisa sobre **ClassName** a Automatización de la interfaz de usuario para escenarios de prueba automatizada. Esta consideración es especialmente importante si estás escribiendo un control que está destinado a su consumo por parte de terceros.

<span id="Automation_peer_classes__"/>
<span id="automation_peer_classes__"/>
<span id="AUTOMATION_PEER_CLASSES__"/>

## <a name="automation-peer-classes"></a>Clases de automatización del mismo nivel  
UWP se basa en convenciones y técnicas de automatización de la interfaz de usuario existentes usadas por marcos de interfaz de usuario de código administrado anteriores, como Windows Forms, Windows Presentation Foundation (WPF) y Microsoft Silverlight. Muchas de las clases de control, y su función y propósito, también derivan de un marco de trabajo de la interfaz de usuario anterior.

Por convención, los nombres de las clases del mismo nivel comienzan con el nombre de la clase de control y terminan con "AutomationPeer". Por ejemplo, [**ButtonAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242458) es la clase del mismo nivel para la clase de control [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265).

> [!NOTE]
> Para los fines de este tema, tratamos las propiedades que se relacionan con la accesibilidad como si fueran más importantes al implementar un sistema del mismo nivel de control. Pero para lograr un concepto más general de la compatibilidad con la Automatización de la interfaz de usuario, debes implementar un sistema del mismo nivel de acuerdo con las recomendaciones documentadas en la [guía del programador de proveedores de Automatización de la interfaz de usuario](https://msdn.microsoft.com/library/windows/desktop/Ee671596) y en los [conceptos básicos de la Automatización de la interfaz de usuario](https://msdn.microsoft.com/library/windows/desktop/Ee684007). Esos temas no abarcan las API [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) específicas que deberías usar para proporcionar la información de marco de UWP para Automatización de la interfaz de usuario, pero describen las propiedades que identifican tu clase o proporcionan otra información o interacción.

<span id="Peers__patterns_and_control_types"/>
<span id="peers__patterns_and_control_types"/>
<span id="PEERS__PATTERNS_AND_CONTROL_TYPES"/>

## <a name="peers-patterns-and-control-types"></a>Sistemas del mismo nivel, patrones y tipos de control  
Un *patrón de control* es una implementación de la interfaz que expone un aspecto específico de una funcionalidad de un control a un cliente de automatización de la interfaz de usuario. Los clientes de automatización de la interfaz de usuario usan las propiedades y los métodos expuestos a través de un patrón de control para recuperar información sobre las funcionalidades del control o manipular el comportamiento en tiempo de ejecución del control.

Los patrones de control ofrecen una manera de categorizar y exponer la funcionalidad de un control independientemente del tipo de control o de su apariencia. Por ejemplo, un control que presenta una interfaz tabular usa el patrón de control **Grid** para exponer el número de filas y columnas en la tabla, y para permitir que un cliente de Automatización de la interfaz de usuario pueda recuperar elementos de la tabla. Como en otros ejemplos, el cliente de Automatización de la interfaz de usuario puede usar el patrón de control **Invoke** para los controles que pueden invocarse (como botones), y el patrón de control **Scroll** para los controles que tienen barras de desplazamiento, (como cuadros de lista, vistas de lista o cuadros combinados). Cada patrón de control representa un tipo de funcionalidad independiente y es posible combinar los patrones de control para describir el conjunto completo de funcionalidades admitidas por un control en particular.

Los patrones de control se relacionan con la interfaz de usuario del mismo modo en que las interfaces se relacionan con los objetos COM. En COM, puedes consultar a un objeto para preguntar qué interfaces admite y después usar esas interfaces para acceder a la funcionalidad. En la automatización de la interfaz de usuario, los clientes pueden preguntar a un elemento de automatización de la interfaz de usuario qué patrones de control admite y, después, interactuar con el elemento y su control del mismo nivel a través de las propiedades, los métodos, los eventos y las estructuras expuestos por los patrones de control admitidos.

Uno de los principales objetivos de un sistema de automatización del mismo nivel es notificar a un cliente de automatización de la interfaz de usuario de cuáles son los patrones de control compatibles con el elemento de la interfaz de usuario mediante el sistema del mismo nivel. Para hacerlo, los proveedores de Automatización de la interfaz de usuario implementan nuevos sistemas del mismo nivel que cambian el comportamiento del método [**GetPattern**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpattern) mediante la anulación del método [**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpatterncore). Los clientes de Automatización de la interfaz de usuario realizan llamadas que el proveedor de Automatización de la interfaz de usuario asigna al **GetPattern** que realiza la llamada. Los clientes de Automatización de la interfaz de usuario consultan cada patrón específico con el que quieren interactuar. Si el elemento del mismo nivel admite el patrón, se devuelve a sí mismo una referencia a un objeto; de lo contrario, devuelve **null**. Si no se devuelve **null**, el cliente de Automatización de la interfaz de usuario espera poder llamar a las API de la interfaz del patrón como un cliente, con el fin de interactuar con ese patrón de control.

Un *tipo de control* es una manera de definir ampliamente la funcionalidad de un control representado por el sistema del mismo nivel. Este es un concepto distinto al del patrón de control, porque mientras que un patrón informa a la Automatización de la interfaz de usuario de la información que puede obtener o de las acciones que puede realizar a través de una interfaz en particular, el tipo de control se encuentra en un nivel superior. Cada tipo de control incluye orientación sobre estos aspectos de la automatización de la interfaz de usuario:

* Patrones de control de automatización de la interfaz de usuario: un tipo de control podría admitir más de un patrón, cada uno de los cuales representa una clasificación distinta de información o interacción. Cada tipo de control tiene un conjunto de patrones de control que el control debe admitir, un conjunto que es opcional, y otro conjunto que el control no debe admitir.
* Valores de propiedad de la automatización de la interfaz de usuario: cada tipo de control tiene un conjunto de propiedades que el control debe admitir. Se trata de propiedades generales, como se describe en [Introducción a las propiedades de automatización de la interfaz de usuario](https://msdn.microsoft.com/library/windows/desktop/Ee671594), no de propiedades que sean específicas del patrón.
* Eventos de automatización de la interfaz de usuario: cada tipo de control tiene un conjunto de eventos que el control debe admitir. Estos también son generales, en lugar de específicos del patrón, tal y como se describe en el tema [Introducción a los eventos de automatización de la interfaz de usuario](https://msdn.microsoft.com/library/windows/desktop/Ee671221).
* Estructura de árbol de automatización de la interfaz de usuario: cada tipo de control define el modo en que debe aparecer el control en la estructura de árbol de automatización de la interfaz de usuario.

Independientemente de cómo se implementen los sistemas del mismo nivel de automatización para el marco, el cliente de Automatización de la interfaz de usuario no está ligada a UWP; de hecho, es probable que los clientes de Automatización de la interfaz de usuario, como las tecnologías de asistencia, usen otros modelos de programación tales como COM. En COM, los clientes pueden usar **QueryInterface** para la interfaz del patrón de control de COM que implementa el patrón solicitado o el marco general de Automatización de la interfaz de usuario para las propiedades, los eventos o el examen del árbol. En el caso de los patrones, el marco de trabajo de automatización de la interfaz de usuario calcula la referencia de ese código de interfaz en todo el código de UWP en el proveedor de automatización de la interfaz de usuario y el sistema del mismo nivel pertinente.

Cuando implementas patrones de control para un marco de código administrado como una aplicación para UWP con C\# o Microsoft Visual Basic, puedes usar interfaces de .NET Framework para representar estos patrones en lugar de usar la representación de interfaces COM. Por ejemplo, la interfaz de un patrón de Automatización de la interfaz de usuario para una implementación de un proveedor de Microsoft .NET del patrón **Invoke** es [**IInvokeProvider**](https://msdn.microsoft.com/library/windows/apps/BR242582).

Para obtener una lista de los patrones de control, interfaces de proveedor y sus finalidades, consulta el tema sobre [interfaces y patrones de control](control-patterns-and-interfaces.md). Para la lista de tipos de control, consulta el tema sobre [introducción a los tipos de control de automatización de la interfaz de usuario](https://msdn.microsoft.com/library/windows/desktop/Ee671197).

<span id="Guidance_for_how_to_implement_control_patterns"/>
<span id="guidance_for_how_to_implement_control_patterns"/>
<span id="GUIDANCE_FOR_HOW_TO_IMPLEMENT_CONTROL_PATTERNS"/>

### <a name="guidance-for-how-to-implement-control-patterns"></a>Instrucciones para implementar patrones de control  
Los patrones de control y las acciones para las que están concebidos forman parte de una definición más amplia del marco de trabajo de Automatización de la interfaz de usuario, y no solo se aplican al soporte de la accesibilidad de una aplicación para UWP. Cuando implementas un patrón de control, deberías asegurarte de implementarlo de forma que coincida con las instrucciones que se ofrecen en MSDN y en la especificación de automatización de la interfaz de usuario. Si necesitas instrucciones, normalmente puedes usar los temas de MSDN y no necesitarás consultar la especificación. Las instrucciones para cada patrón se detallan en el tema sobre la [implementación de patrones de control de automatización de la interfaz de usuario](https://msdn.microsoft.com/library/windows/desktop/Ee671292). Verás que cada tema bajo esta área tiene una sección sobre convenciones e instrucciones de implementación y otra sección sobre los miembros necesarios. Las instrucciones suelen hacer referencia a determinadas API de la interfaz del patrón de control relevante en la referencia sobre [interfaces de patrones de control para proveedores](https://msdn.microsoft.com/library/windows/desktop/Ee671201). Estas interfaces son interfaces nativas o de COM (y sus API usan la sintaxis de estilo COM). Pero todo lo que ves allí tiene un equivalente en el espacio de nombres [**Windows.UI.Xaml.Automation.Provider**](https://msdn.microsoft.com/library/windows/apps/BR209225).

Si estás usando los sistemas del mismo nivel de automatización predeterminados y ampliando su comportamiento, ten en cuenta que estos sistemas del mismo nivel ya han sido escritos de acuerdo con las directrices de Automatización de la interfaz de usuario. Si admiten patrones de control, puedes confiar en la compatibilidad de ese patrón de acuerdo con las instrucciones del tema sobre la [implementación de patrones de control de automatización de la interfaz de usuario](https://msdn.microsoft.com/library/windows/desktop/Ee671292). Si un sistema de control del mismo nivel notifica que es representativo de un tipo de control definido por la automatización de la interfaz de usuario, quiere decir que ha seguido las instrucciones documentadas en el tema sobre [compatibilidad con tipos de control de automatización de la interfaz de usuario](https://msdn.microsoft.com/library/windows/desktop/Ee671633).

Sin embargo, es posible que necesites más instrucciones para que los patrones de control o los tipos de control puedan seguir las recomendaciones de automatización de la interfaz de usuario en tu implementación del mismo nivel. Esto suele suceder particularmente cuando estás implementando compatibilidad con patrones o tipos de control que aún no existe como implementación predeterminada en un control de UWP. Por ejemplo, el patrón para anotaciones no se implementa en ninguno de los controles XAML predeterminados. Pero puede que tengas una aplicación que usa anotaciones de manera extensa y, por tanto, quieras subir a la superficie esa funcionalidad para que esté accesible. Para este escenario, el sistema del mismo nivel debe implementar [**IAnnotationProvider**](https://msdn.microsoft.com/library/windows/apps/Hh738493) y probablemente debería identificarse como tipo de control **Document** con propiedades adecuadas para indicar que los documentos admiten la anotación.

Se recomienda seguir las instrucciones para los patrones de la sección sobre [implementación de patrones de control de automatización del mismo nivel](https://msdn.microsoft.com/library/windows/desktop/Ee671292) o los tipos de control en el tema sobre [compatibilidad con tipos de control de Automatización de la interfaz de usuario](https://msdn.microsoft.com/library/windows/desktop/Ee671633) como orientación e instrucciones generales. Puedes incluso intentar seguir algunos de los vínculos de las API para obtener descripciones y notas con referencia a las API. Pero para la información específica sobre la sintaxis necesaria para la programación de aplicaciones para UWP, busca la API equivalente dentro del espacio de nombres [**Windows.UI.Xaml.Automation.Provider**](https://msdn.microsoft.com/library/windows/apps/BR209225) y usa esas páginas de referencia para obtener más información.

<span id="Built-in_automation_peer_classes"/>
<span id="built-in_automation_peer_classes"/>
<span id="BUILT-IN_AUTOMATION_PEER_CLASSES"/>

## <a name="built-in-automation-peer-classes"></a>Clases integradas de sistemas de automatización del mismo nivel  
En general, los elementos implementan una clase de sistemas de automatización del mismo nivel si aceptan actividad de interfaz de usuario por parte de este o si contienen información que necesitan los usuarios de tecnologías de asistencia que representan la interfaz de usuario interactiva o significativa de las aplicaciones. No todos los elementos visuales de UWP tienen sistemas de automatización del mismo nivel. Algunos ejemplos de las clases que implementan sistemas de automatización del mismo nivel son [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265) y [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683). Algunos ejemplos de clases que no implementan sistemas de automatización del mismo nivel son [**Border**](https://msdn.microsoft.com/library/windows/apps/BR209250) y las clases basadas en [**Panel**](https://msdn.microsoft.com/library/windows/apps/BR227511), como [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704) y [**Canvas**](https://msdn.microsoft.com/library/windows/apps/BR209267). Un **Panel** no tiene un sistema del mismo nivel porque proporciona un comportamiento de diseño que es únicamente visual. No hay ninguna manera accesible de que el usuario interactúe con un **Panel**. En su lugar, todos los elementos secundarios que contenga un **Panel** se notifican a los árboles de Automatización de la interfaz de usuario como elementos secundarios del siguiente elemento primario del árbol que tenga una representación de elemento o de sistema del mismo nivel.

<span id="UI_Automation_and_UWP_process_boundaries"/>
<span id="ui_automation_and_uwp_process_boundaries"/>
<span id="UI_AUTOMATION_AND_UWP_PROCESS_BOUNDARIES"/>

## <a name="ui-automation-and-uwp-process-boundaries"></a>Límites del proceso de UWP y automatización de la interfaz de usuario  
Por lo general, el código de cliente de automatización de la interfaz de usuario que tiene acceso a una aplicación para UWP se ejecuta fuera de proceso. La infraestructura de marco de trabajo de automatización de la interfaz de usuario permite que la información atraviese los límites del proceso. Este concepto se explica con mayor detalle en la [introducción a la Automatización de la interfaz de usuario](https://msdn.microsoft.com/library/windows/desktop/Ee684007).

<span id="OnCreateAutomationPeer"/>
<span id="oncreateautomationpeer"/>
<span id="ONCREATEAUTOMATIONPEER"/>

## <a name="oncreateautomationpeer"></a>OnCreateAutomationPeer  
Todas las clases que derivan del objeto [**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911) contienen el método virtual protegido [**OnCreateAutomationPeer**](https://msdn.microsoft.com/ibrary/windows/apps/windows.ui.xaml.uielement.oncreateautomationpeer). La secuencia de inicialización de objetos para los sistemas de automatización del mismo nivel llama a **OnCreateAutomationPeer** para obtener el objeto de automatización del mismo nivel para cada control y así construir un árbol de Automatización de la interfaz de usuario para uso de tiempo de ejecución. El código de Automatización de la interfaz de usuario puede usar el sistema del mismo nivel para obtener información sobre las características de un control y para simular el uso interactivo por medio de sus patrones de control. Un control personalizado que admite automatización debe invalidar **OnCreateAutomationPeer** y devolver una instancia de una clase que deriva de [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185). Por ejemplo, si un control personalizado deriva de la clase [**ButtonBase**](https://msdn.microsoft.com/library/windows/apps/BR227736), el objeto devuelto por **OnCreateAutomationPeer** debe derivar de [**ButtonBaseAutomationPeer**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.automation.peers.buttonbaseautomationpeer).

Si estás escribiendo una clase de control personalizada y pretendes suministrar también un nuevo sistema del mismo nivel de automatización, debes invalidar el método [**OnCreateAutomationPeer**](https://msdn.microsoft.com/ibrary/windows/apps/windows.ui.xaml.uielement.oncreateautomationpeer) de tu control personalizado para que devuelva una nueva instancia del sistema del mismo nivel. La clase del mismo nivel debe derivar directa o indirectamente de [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185).

Por ejemplo, el siguiente código declara que el control personalizado `NumericUpDown` debe usar `NumericUpDownPeer` del mismo nivel para fines de Automatización de la interfaz de usuario.

C#
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

Visual Basic
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

C++
```cpp
//.h
public ref class NumericUpDown sealed : Windows::UI::Xaml::Controls::Primitives::RangeBase
{
// other initialization not shown
protected:
    virtual AutomationPeer^ OnCreateAutomationPeer() override
    {
         return ref new NumericUpDown(this);
    }
};
```

> [!NOTE]
> La implementación de [**OnCreateAutomationPeer**](https://msdn.microsoft.com/ibrary/windows/apps/windows.ui.xaml.uielement.oncreateautomationpeer) no debe hacer nada más que inicializar una nueva instancia de la automatización del mismo nivel personalizada, pasando el control de llamada como propietario, y devolver esa instancia. No intentes ninguna lógica adicional en este método. En particular, cualquier lógica que pudiera provocar la destrucción del [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) en la misma llamada podría provocar un comportamiento inesperado del tiempo de ejecución.

En las implementaciones típicas de [**OnCreateAutomationPeer**](https://msdn.microsoft.com/ibrary/windows/apps/windows.ui.xaml.uielement.oncreateautomationpeer), se especifica el *propietario* como **this** o **Me** porque la invalidación del método está en el mismo ámbito que el resto de la definición de clase de control.

La definición de clase del mismo nivel se puede hacer en el mismo archivo de código o en un archivo de código independiente. Todas las definiciones de sistemas del mismo nivel existen en el espacio de nombres [**Windows.UI.Xaml.Automation.Peers**](https://msdn.microsoft.com/library/windows/apps/BR242563) independiente de los controles para los que proporcionan sistemas del mismo nivel. También puedes optar por declarar los sistemas del mismo nivel en espacios de nombres diferentes, siempre que hagas referencia a los espacios de nombres necesarios para la llamada al método [**OnCreateAutomationPeer**](https://msdn.microsoft.com/ibrary/windows/apps/windows.ui.xaml.uielement.oncreateautomationpeer).

<span id="Choosing_the_correct_peer_base_class"/>
<span id="choosing_the_correct_peer_base_class"/>
<span id="CHOOSING_THE_CORRECT_PEER_BASE_CLASS"/>

### <a name="choosing-the-correct-peer-base-class"></a>Elección de la clase base del mismo nivel correcta  
Asegúrate de que tu [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) derive de una clase base que sea la mejor opción para la lógica de sistema del mismo nivel existente de la clase de control de la que derivas. En el caso del ejemplo anterior, dado que `NumericUpDown` deriva de [**RangeBase**](https://msdn.microsoft.com/library/windows/apps/BR227863), existe una clase [**RangeBaseAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242506) disponible en la que debes basar tu sistema del mismo nivel. Mediante el uso de la clase del mismo nivel coincidente más cercana en paralelo con el modo en que derivas el control en sí, puedes evitar invalidar al menos parte de la funcionalidad de [**IRangeValueProvider**](https://msdn.microsoft.com/library/windows/apps/BR242590) porque la clase base del mismo nivel ya la implementa.

La clase [**Control**](https://msdn.microsoft.com/library/windows/apps/BR209390) base no tiene una clase correspondiente del mismo nivel. Si necesitas una clase del mismo nivel para que se corresponda con un control personalizado que deriva de **Control**, deriva la clase del mismo nivel personalizada de [**FrameworkElementAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242472).

Si derivas directamente de [**ContentControl**](https://msdn.microsoft.com/library/windows/apps/BR209365), esa clase no tiene comportamiento de automatización del mismo nivel predeterminado porque no existe ninguna implementación [**OnCreateAutomationPeer**](https://msdn.microsoft.com/ibrary/windows/apps/windows.ui.xaml.uielement.oncreateautomationpeer) que haga referencia a una clase del mismo nivel. Por lo tanto, asegúrate de implementar **OnCreateAutomationPeer** para usar tu propio sistema del mismo nivel, o usa [**FrameworkElementAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242472) como el sistema del mismo nivel si ese nivel de compatibilidad de accesibilidad es suficiente para tu control.

> [!NOTE]
> Normalmente no derivas de [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) en lugar de [**FrameworkElementAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242472). Si derivaste directamente de **AutomationPeer**, tendrás que duplicar mucha de la compatibilidad para accesibilidad básica que, de otra forma, provendría de **FrameworkElementAutomationPeer**.

<span id="Initialization_of_a_custom_peer_class"/>
<span id="initialization_of_a_custom_peer_class"/>
<span id="INITIALIZATION_OF_A_CUSTOM_PEER_CLASS"/>

## <a name="initialization-of-a-custom-peer-class"></a>Inicialización de una clase del mismo nivel personalizada  
El sistema de automatización del mismo nivel debe definir un constructor con seguridad de tipos que use una instancia del control propietario para la inicialización base. En el siguiente ejemplo, la implementación pasa el valor *owner* a la base [**RangeBaseAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242506) y, finalmente, [**FrameworkElementAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242472) usa *owner* para establecer [**FrameworkElementAutomationPeer.Owner**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.frameworkelementautomationpeer.owner).

C#
```csharp
public NumericUpDownAutomationPeer(NumericUpDown owner): base(owner)
{}
```

Visual Basic
```vb
Public Sub New(owner As NumericUpDown)
    MyBase.New(owner)
End Sub
```

C++
```cpp
//.h
public ref class NumericUpDownAutomationPeer sealed :  Windows::UI::Xaml::Automation::Peers::RangeBaseAutomationPeer
//.cpp
public:    NumericUpDownAutomationPeer(NumericUpDown^ owner);
```

<span id="Core_methods_of_AutomationPeer"/>
<span id="core_methods_of_automationpeer"/>
<span id="CORE_METHODS_OF_AUTOMATIONPEER"/>

## <a name="core-methods-of-automationpeer"></a>Métodos Core de AutomationPeer  
Por cuestiones de infraestructura de UWP, los métodos que se pueden invalidar de un sistema de automatización del mismo nivel forman parte de un par de métodos: el método de acceso público que el proveedor de Automatización de la interfaz de usuario usa como punto de reenvío para los clientes de Automatización de la interfaz de usuario y el método de personalización "Core" protegido que una clase de UWP puede invalidar para influir en el comportamiento. Estos dos métodos se conectan entre sí de manera predeterminada de tal manera que la llamada al método de acceso siempre invoca el método "Core" paralelo que tiene la implementación del proveedor o, como reserva, invoca una implementación predeterminada de las clases base.

Cuando se implemente un sistema del mismo nivel para un control personalizado, invalida cualquiera de los métodos "Core" desde la clase base de sistemas de automatización del mismo nivel donde quieres exponer comportamiento exclusivo para tu control personalizado. Para obtener información sobre tu control, el código de automatización de la interfaz de usuario llama a los métodos públicos de la clase del mismo nivel. Para proporcionar información acerca de tu control, invalida cada método con un nombre que termine con "Core" cuando el diseño y la implementación de tu control cree escenarios de accesibilidad u otros escenarios de Automatización de la interfaz de usuario que difieran de lo que admite la clase base de sistemas de automatización del mismo nivel.

Como mínimo, siempre que definas una nueva clase del mismo nivel, implementa el método [**GetClassNameCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getclassnamecore), como se muestra en el ejemplo siguiente.

C#
```csharp
protected override string GetClassNameCore()
{
    return "NumericUpDown";
}
```

> [!NOTE]
> Quizás quieras almacenar las cadenas como constantes en lugar de directamente en el cuerpo del método, pero eso depende de ti. En el caso de [**GetClassNameCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getclassnamecore), no necesitarás localizar esta cadena. La propiedad **LocalizedControlType** se usa siempre que un cliente de Automatización de la interfaz de usuario necesita una cadena localizada, no **ClassName**.

### <span id="GetAutomationControlType"/>
<span id="getautomationcontroltype"/>
<span id="GETAUTOMATIONCONTROLTYPE"/>GetAutomationControlType

Algunas tecnologías de asistencia usan el valor [**GetAutomationControlType**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getautomationcontroltype) directamente cuando notifican características de los elementos en un árbol de Automatización de la interfaz de usuario, como información adicional del valor **Name** de Automatización de la interfaz de usuario. Si tu control es muy diferente del control del que derivas y quieres notificar un tipo de control diferente del notificado por la clase base del mismo nivel que usa el control, debes implementar un sistema del mismo nivel e invalidar [**GetAutomationControlTypeCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getautomationcontroltypecore) en tu implementación del mismo nivel. Esto es particularmente importante si derivas desde una clase base generalizada como [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/BR242803) o [**ContentControl**](https://msdn.microsoft.com/library/windows/apps/BR209365), donde el sistema del mismo nivel base no proporciona información precisa acerca del tipo de control.

La implementación de [**GetAutomationControlTypeCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getautomationcontroltypecore) describe el control con el valor [**AutomationControlType**](https://msdn.microsoft.com/library/windows/apps/BR209182). Si bien puedes devolver **AutomationControlType.Custom**, deberías devolver uno de los tipos de control más específicos si alguno de ellos describe con mayor precisión los escenarios principales de tu control. Aquí tienes un ejemplo.

C#
```csharp
protected override AutomationControlType GetAutomationControlTypeCore()
{
    return AutomationControlType.Spinner;
}
```

> [!NOTE]
> A menos que especifiques [**AutomationControlType.Custom**](https://msdn.microsoft.com/library/windows/apps/BR209182), no tienes que implementar [**GetLocalizedControlTypeCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getlocalizedcontroltypecore) para proporcionar una propiedad **LocalizedControlType** a los clientes. La estructura común de Automatización de la interfaz de usuario proporciona cadenas traducidas para cada valor **AutomationControlType** posible que no sea **AutomationControlType.Custom**.

<span id="GetPattern_and_GetPatternCore"/>
<span id="getpattern_and_getpatterncore"/>
<span id="GETPATTERN_AND_GETPATTERNCORE"/>

### <a name="getpattern-and-getpatterncore"></a>GetPattern y GetPatternCore  
Una implementación de [**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) de un sistema del mismo nivel devuelve el objeto que admite el patrón solicitado en el parámetro de entrada. Específicamente, un cliente de Automatización de la interfaz de usuario llama a un método que se reenvía al método [**GetPattern**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpattern) del proveedor y especifica un valor de enumeración [**PatternInterface**](https://msdn.microsoft.com/library/windows/apps/BR242496) que asigna nombre al patrón solicitado. La invalidación de **GetPatternCore** debería devolver el objeto que implementa el modelo especificado. Ese objeto es el propio sistema del mismo nivel, porque el sistema del mismo nivel debe implementar la interfaz de patrón correspondiente siempre que notifica que admite un patrón. Si tu sistema del mismo nivel no tiene una implementación personalizada de un patrón, pero sabes que la base de este implementa el patrón, puedes llamar a la implementación de **GetPatternCore** del tipo base desde tu **GetPatternCore**. Un **GetPatternCore** de un sistema del mismo nivel devolverá **null** si este no admite un patrón. Sin embargo, en lugar de devolver **null** directamente desde tu implementación, normalmente usarás la llamada a la implementación base para devolver **null** para cualquier patrón no admitido.

Cuando se admite un patrón, la implementación de [**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) implementación puede devolver **this** o **Me**. Lo que se espera es que el cliente de Automatización de la interfaz de usuario convierta el valor [**GetPattern**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpattern) devuelto a la interfaz de patrón solicitada siempre que no sea un valor **null**.

Si una clase de sistemas del mismo nivel hereda otro sistema del mismo nivel, y la generación de informes de patrones y compatibilidad necesaria ya se controla a través de la clase base, no es necesario implementar [**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpatterncore). Por ejemplo, si estás implementando un control de intervalo que deriva de [**RangeBase**](https://msdn.microsoft.com/library/windows/apps/BR227863), y tu sistema del mismo nivel deriva de [**RangeBaseAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242506), ese sistema del mismo nivel se devuelve a sí mismo para [**PatternInterface.RangeValue**](https://msdn.microsoft.com/library/windows/apps/BR242496) y tiene implementaciones de trabajo de la interfaz [**IRangeValueProvider**](https://msdn.microsoft.com/library/windows/apps/BR242590) que admite el patrón.

Aunque no es el código literal, este ejemplo se acerca a la implementación de [**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) ya presente en [**RangeBaseAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242506).

C#
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

Si estás implementando un sistema del mismo nivel donde no tienes toda la compatibilidad que necesitas de una clase base del mismo nivel, o si quieres cambiar o incorporar al conjunto de modelos heredados de la base que tu sistema del mismo nivel puede admitir, debes invalidar [**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) para permitir que los clientes de Automatización de la interfaz de usuario usen los modelos.

Para obtener una lista de los modelos de proveedores que están disponibles en la implementación UWP de la compatibilidad para la Automatización de la interfaz de usuario, consulta [**Windows.UI.Xaml.Automation.Provider**](https://msdn.microsoft.com/library/windows/apps/BR209225). Cada uno de esos modelos tiene un valor correspondiente de la enumeración [**PatternInterface**](https://msdn.microsoft.com/library/windows/apps/BR242496), que es cómo los clientes de Automatización de la interfaz de usuario solicitan el modelo en una llamada [**GetPattern**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpattern).

Un sistema del mismo nivel puede informar que admite más de un patrón. En ese caso, la invalidación debería incluir lógica de ruta de acceso devuelta para cada valor [**PatternInterface**](https://msdn.microsoft.com/library/windows/apps/BR242496) admitido y devolver el sistema del mismo nivel en cada caso coincidente. Se espera que el autor de la llamada solicite solo una interfaz por vez, y depende de él realizar la conversión a la interfaz esperada.

Este es un ejemplo de una invalidación [**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) para un sistema del mismo nivel personalizado. Notifica la compatibilidad de dos patrones: [**IRangeValueProvider**](https://msdn.microsoft.com/library/windows/apps/BR242590) y [**IToggleProvider**](https://msdn.microsoft.com/library/windows/apps/BR242653). Este control es un control de visualización de medios que se puede mostrar en pantalla completa (modo de conmutación) y que tiene una barra de progreso en la que los usuarios pueden seleccionar una posición (el control de intervalo). Este código procedía de la [muestra de accesibilidad XAML](http://go.microsoft.com/fwlink/p/?linkid=238570).

C#
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

### <a name="forwarding-patterns-from-sub-elements"></a>Reenvío de patrones de subelementos  
Una implementación del método [**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) también puede especificar un subelemento o una parte como proveedor de patrones para su host. Este ejemplo imita el modo en que [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/BR242803) transfiere el control del patrón de desplazamiento al sistema del mismo nivel de su control [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/BR209527) interno. Para especificar un subelemento para el control de patrones, este código obtiene el objeto del subelemento, crea un sistema del mismo nivel para el subelemento con el método [**FrameworkElementAutomationPeer.CreatePeerForElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.frameworkelementautomationpeer.createpeerforelement) y devuelve el nuevo sistema del mismo nivel.

C#
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

### <a name="other-core-methods"></a>Otros métodos Core  
Tu control probablemente necesite admitir equivalentes de teclado para escenarios primarios. Para obtener más información acerca de por qué esto puede ser necesario, consulta [Accesibilidad de teclado](keyboard-accessibility.md). La implementación de compatibilidad de teclado es necesariamente parte del código de control y no el código de sistemas del mismo nivel, porque eso es parte de la lógica de un control, pero tu clase de sistemas del mismo nivel debe invalidar los métodos [**GetAcceleratorKeyCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getacceleratorkeycore) y [**GetAccessKeyCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getaccesskeycore) para notificar a los clientes de Automatización de la interfaz de usuario qué claves se usan. Ten en cuenta que es posible que las cadenas que proporcionan información clave deban localizarse y, por consiguiente, deben provenir de recursos, en lugar de cadenas codificadas de forma rígida.

Si estás proporcionando un sistema del mismo nivel para una clase que admite una colección, es mejor derivar de clases funcionales y clases del mismo nivel que ya cuenten con ese tipo de compatibilidad para colecciones. Si no puedes hacerlo, los sistemas del mismo nivel para controles que mantienen colecciones secundarias pueden tener que invalidar el método [**GetChildrenCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getchildrencore) de los sistemas del mismo nivel relacionado con la colección para notificar correctamente las relaciones entre los elementos principales y los secundarios al árbol de Automatización de la interfaz de usuario.

Implementa los métodos [**IsContentElementCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.iscontentelementcore) y [**IsControlElementCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.iscontrolelementcore) para indicar si tu control incluye contenido de datos o cumple un rol interactivo en la interfaz de usuario (o ambas cosas). De manera predeterminada, ambos métodos devuelven el valor **true**. Estas opciones de configuración mejoran la facilidad de uso de las tecnologías de asistencia como los lectores de pantalla, que pueden usar estos métodos para filtrar el árbol de automatización. Si tu método [**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) transfiere el control de patrones al sistema del mismo nivel de un subelemento, el método **IsControlElementCore** de dicho sistema del mismo nivel puede devolver **false** para ocultar el sistema del mismo nivel del subelemento del árbol de automatización.

Algunos controles probablemente admitan escenarios de etiquetado, donde una parte de la etiqueta de texto proporciona información para una parte que no es de texto, o un control está destinado a formar parte de una relación de etiquetado conocida con otro control de la interfaz de usuario. Si es posible proporcionar un comportamiento basado en clases útil, puedes invalidar [**GetLabeledByCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getlabeledbycore) para hacerlo.

[**GetBoundingRectangleCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getboundingrectanglecore) y [**GetClickablePointCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getclickablepointcore) se usan principalmente para escenarios de pruebas automatizadas. Si quieres admitir pruebas automatizadas para tu control, quizás quieras invalidar estos métodos. Esto podría ser deseable para controles del tipo de intervalo, en los que no puedes sugerir un único punto porque el lugar donde el usuario hace clic en el espacio de coordenadas tiene un efecto diferente en un intervalo. Por ejemplo, el sistema de automatización del mismo nivel [**ScrollBar**](https://msdn.microsoft.com/library/windows/apps/BR209745) predeterminado invalida **GetClickablePointCore** para devolver un valor [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) que "no es un número".

[**GetLiveSettingCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getlivesettingcore) influye en el valor predeterminado del control de **LiveSetting** para la Automatización de la interfaz de usuario. Quizás quieras invalidarlo si quieres que el control devuelva un valor diferente de [**AutomationLiveSetting.Off**](https://msdn.microsoft.com/library/windows/apps/JJ191519). Para obtener más información sobre lo que **LiveSetting** representa, consulta [**AutomationProperties.LiveSetting**](https://msdn.microsoft.com/library/windows/apps/JJ191516).

Podrías invalidar [**GetOrientationCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getorientationcore) si tu control tiene una propiedad de orientación que pueda establecerse y que pueda asignarse a [**AutomationOrientation**](https://msdn.microsoft.com/library/windows/apps/BR209184). Las clases [**ScrollBarAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242522) y [**SliderAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242546) hacen esto.

<span id="Base_implementation_in_FrameworkElementAutomationPeer"/>
<span id="base_implementation_in_frameworkelementautomationpeer"/>
<span id="BASE_IMPLEMENTATION_IN_FRAMEWORKELEMENTAUTOMATIONPEER"/>

### <a name="base-implementation-in-frameworkelementautomationpeer"></a>Implementación base en FrameworkElementAutomationPeer  
La implementación base de [**FrameworkElementAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242472) proporciona cierta información de Automatización de la interfaz de usuario que puede ser interpretada desde diversas propiedades de comportamiento y diseño que se definen en el nivel del marco.

* [**GetBoundingRectangleCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getboundingrectanglecore): devuelve una estructura [**Rect**](https://msdn.microsoft.com/library/windows/apps/BR225994) basada en las características de diseño conocidas. Devuelve un valor de 0 **Rect** si [**IsOffscreen**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.isoffscreen) es **true**.
* [**GetClickablePointCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getclickablepointcore): devuelve la estructura [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) basada en las características de diseño conocidas, siempre que exista un **BoundingRectangle** no nulo.
* [**GetNameCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getnamecore): para conocer un comportamiento más amplio que el que puede resumirse aquí, consulta [**GetNameCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getnamecore). Básicamente, intenta una conversión de cadena en cualquier contenido conocido de un [**ContentControl**](https://msdn.microsoft.com/library/windows/apps/BR209365) o clases relacionadas que tienen contenido. Además, si existe un valor para [**LabeledBy**](https://msdn.microsoft.com/library/windows/apps/Hh759769), ese valor **Name** del elemento se usa como **Nombre**
* [**HasKeyboardFocusCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.haskeyboardfocuscore): se evalúa en función de las propiedades [**FocusState**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.focusstate) e [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.isenabled) del propietario. Los elementos que no son controles siempre devuelven **false**.
* [**IsEnabledCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.isenabledcore): se evalúa en función de la propiedad [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.isenabled) del propietario si es un [**Control**](https://msdn.microsoft.com/library/windows/apps/BR209390). Los elementos que no son controles siempre devuelven **true**. Esto no significa que el propietario esté habilitado en el sentido convencional de interacción; significa que el sistema del mismo nivel está habilitado a pesar de que el propietario no tiene una propiedad **IsEnabled**.
* [**IsKeyboardFocusableCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.iskeyboardfocusablecore): devuelve **true** si el propietario es [**Control**](https://msdn.microsoft.com/library/windows/apps/BR209390); de lo contrario, devuelve **false**.
* [**IsOffscreenCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.isoffscreencore): [**Visibility**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.visibility) de [**Collapsed**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.visibility) en el elemento propietario o en cualquiera de sus elementos principales equivale a un valor **true** de [**IsOffscreen**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.isoffscreen). Excepción: un objeto [**Popup**](https://msdn.microsoft.com/library/windows/apps/BR227842) puede ser visible incluso si los elementos principales de su propietario no lo son.
* [**SetFocusCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.setfocuscore): llama a [**Focus**](https://msdn.microsoft.com/library/windows/apps/hh702161).
* [**GetParent**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getparent): llama a [**FrameworkElement.Parent**](https://msdn.microsoft.com/library/windows/apps/BR208739) desde el propietario y busca el sistema del mismo nivel apropiado. Este no es un par de invalidación con un método "Core", por lo que no puedes cambiar este comportamiento.

> [!NOTE]
> Los sistemas del mismo nivel de UWP predeterminados implementan un comportamiento mediante el uso de código nativo interno que implementa UWP, no necesariamente mediante el uso del código real de UWP. No podrás ver el código ni la lógica de la implementación a través de la reflexión de common language runtime (CLR) ni de otras técnicas. Tampoco verás páginas de referencia diferentes en la referencia para invalidaciones específicas de la subclase del comportamiento de sistemas del mismo nivel base. Por ejemplo, probablemente exista comportamiento adicional para [**GetNameCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getnamecore) de [**TextBoxAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242550), que no se describe en la página de referencia **AutomationPeer.GetNameCore**, pero no hay ninguna página de referencia para **TextBoxAutomationPeer.GetNameCore**. Ni siquiera existe una página de referencia de **TextBoxAutomationPeer.GetNameCore**. En cambio, lee el tema de referencia sobre la clase de sistemas del mismo nivel más inmediata y busca notas de implementación en la sección de observaciones.

<span id="Peers_and_AutomationProperties"/>
<span id="peers_and_automationproperties"/>
<span id="PEERS_AND_AUTOMATIONPROPERTIES"/>

## <a name="peers-and-automationproperties"></a>Sistemas del mismo nivel y AutomationProperties  
Tu sistema de automatización del mismo nivel debe proporcionar valores predeterminados adecuados para la información relativa a la accesibilidad de tu control. Ten en cuenta que cualquier código de aplicación que use el control puede invalidar parte de ese comportamiento incluyendo los valores de propiedad adjunta [**AutomationProperties**](https://msdn.microsoft.com/library/windows/apps/BR209081) en instancias de control. Los autores de llamadas pueden hacer esto para los controles predeterminados o para los controles personalizados. Por ejemplo, el siguiente XAML crea un botón que tiene dos propiedades de Automatización de la interfaz de usuario personalizadas:  `<Button AutomationProperties.Name="Special"      AutomationProperties.HelpText="This is a special button."/>`

Para obtener más información acerca de las propiedades adjuntas [**AutomationProperties**](https://msdn.microsoft.com/library/windows/apps/BR209081), consulta [Información básica de accesibilidad](basic-accessibility-information.md).

Algunos de los métodos [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) existen debido al contrato general relativo a cómo se espera que los proveedores de Automatización de la interfaz de usuario proporcionen información, pero estos métodos generalmente no se implementan en sistemas del mismo nivel del control. Esto se debe a que se espera que los valores [**AutomationProperties**](https://msdn.microsoft.com/library/windows/apps/BR209081) aplicados al código de la aplicación que usa los controles en una interfaz de usuario específica proporcionen esa información. Por ejemplo, la mayoría de las aplicaciones definirían la relación de etiquetado entre dos controles diferentes de la interfaz de usuario aplicando un valor de [**AutomationProperties.LabeledBy**](https://msdn.microsoft.com/library/windows/apps/Hh759769). No obstante, [**LabeledByCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getlabeledbycore) se implementa en ciertos sistemas del mismo nivel que representan relaciones de elementos o datos de un control, como el uso de una parte de encabezado para etiquetar una parte de campo de datos, el etiquetado de los elementos con sus contenedores o escenarios similares.

<span id="Implementing_patterns"/>
<span id="implementing_patterns"/>
<span id="IMPLEMENTING_PATTERNS"/>

## <a name="implementing-patterns"></a>Implementación de patrones  
Veamos cómo escribir un sistema del mismo nivel para un control que implementa un comportamiento de expansión y contracción implementando la interfaz del patrón de control para expansión y contracción. El sistema del mismo nivel debería permitir la accesibilidad para el comportamiento de expansión y contracción devolviéndose a sí mismo siempre que se llame a [**GetPattern**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpattern) con un valor [**PatternInterface.ExpandCollapse**](https://msdn.microsoft.com/library/windows/apps/BR242496). El sistema del mismo nivel después hereda la interfaz de proveedor para ese patrón ([**IExpandCollapseProvider**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.provider.iexpandcollapseprovider)) y proporciona implementaciones para cada uno de los miembros de esa interfaz de proveedor. En este caso, la interfaz tiene que invalidar tres miembros: [**Expand**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.provider.iexpandcollapseprovider.expand), [**Collapse**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.provider.iexpandcollapseprovider.collapse) y [**ExpandCollapseState**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.provider.iexpandcollapseprovider.expandcollapsestate).

Es útil planear con tiempo la accesibilidad en el diseño de la API de la propia clase. Siempre que tengas un comportamiento que pueda llegar a ser solicitado por las interacciones típicas con un usuario que trabaja en la interfaz de usuario y también por medio de un patrón de proveedores de automatización, proporciona un solo método que pueda llamar el patrón de automatización o la respuesta de la interfaz de usuario. Por ejemplo, si tu control tiene partes de botón que tienen controladores de eventos conectados que pueden expandir o contraer el control, y tiene equivalentes de teclado para esas acciones, haz que estos controladores de eventos llamen al mismo método al que llamas desde dentro del cuerpo de las implementaciones de [**Expand**](https://msdn.microsoft.com/library/windows/apps/BR242570) o [**Collapse**](https://msdn.microsoft.com/library/windows/apps/BR242569) para [**IExpandCollapseProvider**](https://msdn.microsoft.com/library/windows/desktop/Ee671242) en el sistema del mismo nivel. Usar un método de lógica común también puede resultar útil para garantizar la actualización de los estados visuales de tu control a fin de mostrar estados lógicos de forma uniforme independientemente de cómo se haya invocado el comportamiento.

Una implementación típica es aquella en las que las API de proveedor primero llaman a [**Owner**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.frameworkelementautomationpeer.owner) para acceder a la instancia de control en tiempo de ejecución. Después, pueden llamarse a los métodos de comportamiento necesarios en ese objeto.

C#
```csharp
public class IndexCardAutomationPeer : FrameworkElementAutomationPeer, IExpandCollapseProvider {
    private IndexCard ownerIndexCard;
    public IndexCardAutomationPeer(IndexCard owner) : base(owner)
    {
         ownerIndexCard = owner;
    }
}
```

Una implementación alternativa es que el propio control pueda hacer referencia a su sistema del mismo nivel. Este es un patrón común si generas eventos de automatización desde el control, porque el método [**RaiseAutomationEvent**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.raiseautomationevent) es un método del sistema del mismo nivel.

<span id="UI_Automation_events"/>
<span id="ui_automation_events"/>
<span id="UI_AUTOMATION_EVENTS"/>

## <a name="ui-automation-events"></a>Eventos de automatización de la interfaz de usuario  

Los eventos de Automatización de la interfaz de usuario pueden clasificarse de la siguiente manera.

| Evento | Descripción |
|-------|-------------|
| Cambio de propiedad | Se desencadena cuando cambia una propiedad de un patrón de control o un elemento de automatización de la interfaz de usuario. Por ejemplo, si un cliente necesita supervisar el control de casilla de una aplicación, puede registrarse para escuchar un evento de cambio de propiedades en la propiedad [**ToggleState**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.provider.itoggleprovider.togglestate). Cuando el control de casilla se activa o desactiva, el proveedor inicia el evento y el cliente puede actuar de la manera que sea necesaria. |
| Acción de elemento | Se desencadena cuando un cambio en la interfaz de usuario se origina en la actividad programática o del usuario. Por ejemplo, cuando se hace clic en un botón o se invoca a través del patrón **Invoke**. |
| Cambio de estructura | Se desencadena cuando cambia la estructura del árbol de Automatización de la interfaz de usuario. La estructura cambia cuando se hacen visibles, ocultan o quitan elementos nuevos de la interfaz de usuario en el escritorio. |
| Cambio global | Se desencadena cuando se producen acciones de interés global para el cliente, como cuando el foco cambia desde un elemento a otro o cuando se cierra una ventana secundaria. Algunos eventos no necesariamente significan que el estado de la interfaz de usuario ha cambiado. Por ejemplo, si el usuario presiona un campo de entrada de texto y después hace clic en un botón para actualizar el campo, se desencadena un evento [**TextChanged**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textbox.textchanged) si el usuario no cambió el texto. Cuando se procesa un evento, probablemente sea necesario para una aplicación cliente comprobar si se produjo algún cambio antes de realizar alguna acción. |

<span id="AutomationEvents_identifiers"/>
<span id="automationevents_identifiers"/>
<span id="AUTOMATIONEVENTS_IDENTIFIERS"/>

### <a name="automationevents-identifiers"></a>Identificadores de AutomationEvents  
Los eventos de Automatización de la interfaz de usuario están definidos por los valores [**AutomationEvents**](https://msdn.microsoft.com/library/windows/apps/BR209183). Los valores de la enumeración identifican unívocamente el tipo de evento.

<span id="Raising_events"/>
<span id="raising_events"/>
<span id="RAISING_EVENTS"/>

### <a name="raising-events"></a>Generación de eventos  
Los clientes de automatización de la interfaz de usuario pueden suscribirse a eventos de automatización. En el modelo de sistemas de automatización del mismo nivel, los sistemas de automatización del mismo nivel para los controles personalizados deben informar de los cambios en el estado del control que sean pertinentes a la accesibilidad llamando al método [**RaiseAutomationEvent**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.raiseautomationevent). De manera similar, cuando cambia un valor clave de una propiedad de Automatización de la interfaz de usuario, los sistemas de control personalizado del mismo nivel deben llamar al método [**RaisePropertyChangedEvent**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.raisepropertychangedevent).

El siguiente ejemplo de código muestra cómo obtener el objeto del sistema del mismo nivel del código de definición de control y llamar a un método para desencadenar un evento desde ese sistema del mismo nivel. Con fines de optimización, el código determina si hay escuchas para este tipo de evento. Desencadenar el evento y crear el objeto del mismo nivel solo cuando hay escuchas evita sobrecargas innecesarias y ayuda al control a mantener su capacidad de respuesta.

C#
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

## <a name="peer-navigation"></a>Navegación de sistemas del mismo nivel  
Después de localizar un sistema de automatización del mismo nivel, un cliente de Automatización de la interfaz de usuario puede navegar por la estructura del mismo nivel de una aplicación llamando a los métodos [**GetChildren**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getchildren) y [**GetParent**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getparent) del objeto del sistema de mismo nivel. La implementación del método [**GetChildrenCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getchildrencore) del sistema del mismo nivel admite la navegación entre elementos de la interfaz de usuario dentro de un control. El sistema de Automatización de la interfaz de usuario llama a este método para crear un árbol de los subelementos incluidos dentro de un control, por ejemplo, elementos de lista en un cuadro de lista. El método **GetChildrenCore** en [**FrameworkElementAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242472) recorre el árbol visual de elementos para crear el árbol de sistemas de automatización del mismo nivel. Los controles personalizados pueden invalidar este método para exponer una representación diferente de los elementos secundarios a los clientes de automatización, lo que devuelve los sistemas de automatización del mismo nivel de los elementos que transmiten información o permiten la interacción del usuario.

<span id="Native_automation_support_for_text_patterns"/>
<span id="native_automation_support_for_text_patterns"/>
<span id="NATIVE_AUTOMATION_SUPPORT_FOR_TEXT_PATTERNS"/>

## <a name="native-automation-support-for-text-patterns"></a>Compatibilidad de automatización nativa para patrones de texto  
Algunos sistemas de automatización del mismo nivel predeterminados de las aplicaciones para UWP admiten patrones de control para el patrón de texto ([**PatternInterface.Text**](https://msdn.microsoft.com/library/windows/apps/BR242496)). Sin embargo, esta compatibilidad se ofrece mediante métodos nativos y los sistemas de automatización del mismo nivel no anotarán la interfaz [**ITextProvider**](https://msdn.microsoft.com/library/windows/apps/BR242627) en la herencia (administrada). Aun así, si un cliente de Automatización de la interfaz de usuario administrado o no administrado solicita patrones al sistema de automatización del mismo nivel, notificará la compatibilidad con el patrón de texto y proporcionará el comportamiento de partes del patrón cuando se llame a las API del cliente.

Si tienes intención de derivar desde uno de los controles de texto de la aplicación para UWP y crear también un sistema del mismo nivel personalizado que derive de uno de los sistemas del mismo nivel relacionados con el texto, comprueba las secciones Observaciones del sistema del mismo nivel para informarte sobre la compatibilidad de nivel nativo con los patrones. Puedes acceder al comportamiento de base nativa en el sistema del mismo nivel personalizado si llamas a la implementación base desde las implementaciones de la interfaz del proveedor administrado, pero es difícil modificar lo que hace la implementación base porque las interfaces nativas en el sistema del mismo nivel y su control propietario no están expuestas. Generalmente usarás las implementaciones base tal cual (solo llamada a la base) o reemplazarás por completo la funcionalidad con tu propio código administrado y no llamarás a la implementación base. Este es un escenario avanzado. Necesitarás estar muy familiarizado con el marco de servicios de texto que usa tu control para poder admitir los requisitos de accesibilidad al usar ese marco.

<span id="AutomationProperties.AccessibilityView"/>
<span id="automationproperties.accessibilityview"/>
<span id="AUTOMATIONPROPERTIES.ACCESSIBILITYVIEW"/>

## <a name="automationpropertiesaccessibilityview"></a>AutomationProperties.AccessibilityView  
Además de proporcionar un sistema del mismo nivel personalizado, también puedes ajustar la representación de la vista de árbol para cualquier instancia de control, estableciendo [**AutomationProperties.AccessibilityView**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.automationproperties.accessibilityview) en XAML. Esto no se implementa como parte de una clase del mismo nivel, pero lo mencionamos aquí porque está relacionado con la compatibilidad para accesibilidad general ya sea para controles personalizados o para las plantillas que personalices.

El principal escenario de uso de **AutomationProperties.AccessibilityView** es omitir deliberadamente determinados controles de una plantilla de las vistas de Automatización de la interfaz de usuario, porque no contribuyen de forma significativa a la vista de accesibilidad de todo el control. Para evitar esto, establece **AutomationProperties.AccessibilityView** en "Raw".

<span id="Throwing_exceptions_from_automation_peers"/>
<span id="throwing_exceptions_from_automation_peers"/>
<span id="THROWING_EXCEPTIONS_FROM_AUTOMATION_PEERS"/>

## <a name="throwing-exceptions-from-automation-peers"></a>Iniciar excepciones desde sistemas de automatización del mismo nivel  
Las API que implementas para la compatibilidad del sistema de automatización del mismo nivel pueden generar excepciones. Se espera que cualquier cliente de automatización de la interfaz de usuario que está escuchando sea lo suficientemente sólido como para continuar después de que se inicien la mayoría de las excepciones. Con toda probabilidad, el agente de escucha está mirando hacia un árbol de automatización vertical que incluye aplicaciones distintas de la tuya y es un diseño de cliente inaceptable para traer el cliente completo, solo porque un área del árbol inició una excepción basada en el sistema del mismo nivel cuando el cliente llamó a sus API.

Para los parámetros que se pasan en tu sistema del mismo nivel, se puede validar la entrada y, por ejemplo, iniciar [**ArgumentNullException**](https://msdn.microsoft.com/library/windows/apps/system.argumentnullexception) si se pasó **null** y eso no es un valor válido para tu implementación. Sin embargo, si el sistema del mismo nivel realiza operaciones posteriores, recuerda que las interacciones del sistema del mismo nivel con el control de hospedaje tienen cierto carácter asíncrono. Cualquier cosa que haga un sistema del mismo nivel no bloqueará necesariamente el umbral de la interfaz de usuario en el control (y probablemente no debería). Así que podrías tener situaciones en las que un objeto estaba disponible o tenía ciertas propiedades cuando se creó el sistema del mismo nivel o cuando se llamó por primera vez a un método de sistema de automatización del mismo nivel, pero mientras tanto, el estado del control ha cambiado. Para estos casos, hay dos excepciones dedicadas que un proveedor puede iniciar:

* Inicia [**ElementNotAvailableException**](https://msdn.microsoft.com/library/system.windows.automation.elementnotavailableexception) si no puedes acceder al propietario del sistema del mismo nivel o a un elemento relacionado del sistema del mismo nivel, basado en la información original que pasó tu API. Por ejemplo, puede que tengas un sistema del mismo nivel que está intentando ejecutar sus métodos pero el propietario se ha eliminado de la interfaz de usuario, como un cuadro de diálogo modal que se ha cerrado. Para un cliente que no sea de .NET, esto se asigna a [**UIA\_E\_ELEMENTNOTAVAILABLE**](https://msdn.microsoft.com/library/windows/desktop/Ee671218).
* Inicia [**ElementNotEnabledException**](https://msdn.microsoft.com/library/system.windows.automation.elementnotenabledexception) si todavía hay un propietario, pero se encuentra en un modo como [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.isenabled)`=`**false**, que está bloqueando algunos de los cambios de programación específicos que el sistema del mismo nivel está intentando cumplir. Para un cliente que no sea de .NET, esto se asigna a [**UIA\_E\_ELEMENTNOTENABLED**](https://msdn.microsoft.com/library/windows/desktop/Ee671218).

Más allá de esto, los sistemas del mismo nivel deberían ser relativamente conservadores en cuanto a las excepciones que inician desde su compatibilidad con el sistema del mismo nivel. La mayoría de los clientes no podrá controlar excepciones procedentes de sistemas del mismo nivel y convertirlas en opciones que requieren acción y que puedan ser elegidas por los usuarios cuando interaccionen con el cliente. Así que, a veces, capturar excepciones sin volver a iniciarlas dentro de las implementaciones del mismo nivel puede suponer una estrategia más acertada que iniciar excepciones cada vez que no funciona algo que el sistema del mismo nivel intenta hacer. Ten en cuenta también que la mayoría de los clientes de Automatización de la interfaz de usuario no se escriben en código administrado. La mayoría están escritas en COM, y solo buscan **S\_OK** en **HRESULT** cuando llaman a un método de cliente de Automatización de la interfaz de usuario que termina accediendo al sistema del mismo nivel.

<span id="related_topics"/>

## <a name="related-topics"></a>Temas relacionados  
* [Accesibilidad](accessibility.md)
* [Muestra de accesibilidad en XAML](http://go.microsoft.com/fwlink/p/?linkid=238570)
* [**FrameworkElementAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242472)
* [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185)
* [**OnCreateAutomationPeer**](https://msdn.microsoft.com/ibrary/windows/apps/windows.ui.xaml.uielement.oncreateautomationpeer)
* [Interfaces y patrones de control](control-patterns-and-interfaces.md)
