---
Description: Este artículo es una introducción a los conceptos y tecnologías relacionados con escenarios de accesibilidad para aplicaciones de Windows.
ms.assetid: AA053196-F331-4CBE-B032-4E9CBEAC699C
title: Información general sobre accesibilidad
label: Accessibility overview
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 386ea9a5ea9b66b0756963da10f72c3dbed53ff9
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2020
ms.locfileid: "82969660"
---
# <a name="accessibility-overview"></a>Información general sobre accesibilidad

Este artículo es una introducción a los conceptos y tecnologías relacionados con escenarios de accesibilidad para aplicaciones de Windows.

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Developing-Apps-for-Accessibility/player]

<span id="Accessibility_and_your_app"/>
<span id="accessibility_and_your_app"/>
<span id="ACCESSIBILITY_AND_YOUR_APP"/>

## <a name="accessibility-and-your-app"></a>Accesibilidad y la aplicación

Existe una gran variedad de discapacidades, entre ellas, limitaciones en la movilidad, vista, percepción de los colores, audición, habla, cognición y alfabetización. No obstante, puedes dar respuesta a la mayoría de los requisitos si sigues las instrucciones incluidas en este documento. Esto significa que debes proporcionar lo siguiente:

* Soporte para lectores de pantalla e interacciones de teclado.
* Soporte para la personalización de usuario, como configuraciones de fuente, configuración de zoom (ampliación), color y contraste alto.
* Alternativas o complementos para partes de la interfaz de usuario.

Los controles para XAML proporcionan compatibilidad de teclado integrada y compatibilidad de tecnologías de asistencia, como lectores de pantalla, que sacan provecho de los marcos de accesibilidad que ya admiten aplicaciones para UWP, HTML y otras tecnologías de interfaz de usuario. Esta compatibilidad integrada permite un nivel básico de accesibilidad que puedes personalizar con muy poco esfuerzo mediante la configuración de unas pocas propiedades. Si estás creando tus propios componentes y controles de XAML personalizados, también puedes agregar una compatibilidad similar usando el concepto de *sistemas de automatización del mismo nivel*.

Además, las funciones de plantilla, estilo y enlace de datos simplifican la implementación de la compatibilidad de cambios dinámicos para mostrar la configuración y el texto para interfaces de usuario alternativas.

<span id="UI_Automation"/>
<span id="ui_automation"/>
<span id="UI_AUTOMATION"/>

## <a name="ui-automation"></a>Automatización de la interfaz de usuario

Compatibilidad de la accesibilidad viene principalmente de la compatibilidad integrada con el marco de trabajo de automatización de la interfaz de usuario de Microsoft. Esa compatibilidad se proporciona mediante clases base y el comportamiento integrado de la implementación de clases para los tipos de control, y una representación de la interfaz de la API del proveedor de la automatización de la interfaz de usuario. Cada clase de control usa los conceptos de automatización de la interfaz de usuario de automatización del mismo nivel y patrones de automatización que notifican del contenido y rol del control a los clientes de automatización de la interfaz de usuario. La automatización de la interfaz de usuario considera que la aplicación es una ventana de nivel superior y todo el contenido de esa ventana de aplicación se pone a disposición de un cliente de automatización de la interfaz de usuario a través del marco de trabajo de automatización de la interfaz de usuario. Para obtener más información sobre la automatización de la interfaz de usuario, consulta el tema [UI Automation Overview](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-uiautomationoverview) (Introducción a la automatización de la interfaz de usuario).

<span id="Assistive_technology"/>
<span id="assistive_technology"/>
<span id="ASSISTIVE_TECHNOLOGY"/>

## <a name="assistive-technology"></a>Tecnología de asistencia

Muchas de las necesidades de accesibilidad del usuario se satisfacen a través de productos de tecnología de asistencia instalados por este o por herramientas y de la configuración proporcionada por el sistema operativo. Esto incluye funciones como lectores de pantalla, ampliación de pantalla y configuración de contraste alto.

Los productos de tecnología de asistencia incluyen una gran variedad de software y hardware. Estos productos funcionan a través de los marcos de accesibilidad y la interfaz de teclado estándar que proporcionan información acerca del contenido y la estructura de una interfaz de usuario a los lectores de pantalla y otras tecnologías de asistencia. A continuación, se incluyen algunos ejemplos de productos de tecnología de asistencia:

* El teclado en pantalla, que permite a las personas usar un puntero en lugar de un teclado para escribir texto.
* Software de reconocimiento de voz, que convierte las palabras habladas en texto escrito.
* Lectores de pantalla, que convierten el texto en palabras habladas u otros formatos como el Braille.
* El lector de pantalla Narrador, que forma parte específica de Windows. Narrador tiene un modo táctil que puede llevar a cabo tareas de lectura con gestos táctiles cuando no hay un teclado disponible.
* Los programas o configuraciones que ajustan la pantalla o áreas de la misma, por ejemplo, temas de alto contraste, la configuración de puntos por pulgada (ppp) de la pantalla o la herramienta Lupa.

Las aplicaciones que tienen buena compatibilidad para lectores de pantalla y teclados generalmente funcionan correctamente con diversos productos de tecnología de asistencia. En muchos casos, una aplicación para UWP funciona con estos productos sin necesidad de modificar la información o la estructura. No obstante, es posible que quieras modificar algunas configuraciones para lograr una experiencia de accesibilidad óptima o para implementar compatibilidad adicional.

Algunas de las opciones que puedes usar para probar los escenarios de accesibilidad básicos con tecnologías de asistencia se enumeran en [Pruebas de accesibilidad](accessibility-testing.md).

<span id="Screen_reader_support_and_basic_accessibility_information"/>
<span id="screen_reader_support_and_basic_accessibility_information"/>
<span id="SCREEN_READER_SUPPORT_AND_BASIC_ACCESSIBILITY_INFORMATION"/>

## <a name="screen-reader-support-and-basic-accessibility-information"></a>Información de accesibilidad básica y compatibilidad para lectores de pantalla

Los lectores de pantalla proporcionan acceso al texto de una aplicación representándolo en algún otro formato, como Braille o idioma hablado. El comportamiento exacto de un lector de pantalla depende del software y de la configuración de usuario de este.

Por ejemplo, algunos lectores de pantalla leen toda la interfaz de usuario de una aplicación cuando el usuario la inicia o cambia a ella, lo que permite que el usuario reciba todo el contenido informativo disponible antes de desplazarse por ella. Algunos lectores de pantalla también leen el texto asociado con un control individual cuando recibe el foco durante la navegación por tabulación. Esto permite a los usuarios orientarse mientras navegan entre los controles de entrada de una aplicación. Narrador es un ejemplo de lector de pantalla que proporciona ambos comportamientos, en función de la elección del usuario.

La información más importante que necesita un lector de pantalla o cualquier otra tecnología de asistencia para poder ayudar a los usuarios a comprender una aplicación o a navegar por ella es un *nombre accesible* para los elementos de la aplicación. En muchos casos, un control o elemento ya tiene un nombre accesible calculado a partir de otros valores de propiedad que hayas proporcionado. El caso más común en el que se puede usar un nombre calculado previamente es un elemento que admita y muestre texto interno. Para otros elementos, a veces debes considerar otras maneras de proporcionar un nombre accesible siguiendo los procedimientos recomendados para la estructura de elementos. Y otras veces debes proporcionar un nombre que esté destinado explícitamente a ser el nombre accesible para la accesibilidad de la aplicación. Para obtener una lista de cuántos de estos valores calculados funcionan en elementos comunes de la interfaz de usuario y para obtener información general sobre los nombres accesibles, consulta [Información de accesibilidad básica](basic-accessibility-information.md).

Existen muchas otras propiedades de automatización disponibles (incluidas las propiedades de teclado descritas en la siguiente sección). Pero no todos los lectores de pantalla admiten todas las propiedades de automatización. En general, debes establecer todas las propiedades de automatización correspondientes y hacer una prueba para proporcionar la mayor compatibilidad posible para los lectores de pantalla.

<span id="Keyboard_support"/>
<span id="keyboard_support"/>
<span id="KEYBOARD_SUPPORT"/>

## <a name="keyboard-support"></a>Compatibilidad con el teclado

Para proporcionar una buena compatibilidad de teclado, debes asegurarte de que cada parte de tu aplicación pueda usarse con un teclado. Si tu aplicación usa principalmente los controles estándar y no usa ningún control personalizado, ya tienes la mayor parte del camino recorrido. El modelo de control básico de XAML proporciona compatibilidad de teclado integrada que incluye navegación mediante tabulación, entrada de texto y compatibilidad específica del control. Los elementos que funcionan como contenedores de diseño (como los paneles) usan el orden de diseño para establecer un orden de tabulación predeterminado. Ese orden suele ser el orden de tabulación correcto que se debe usar para una representación accesible de la interfaz de usuario. Si usas los controles [**ListBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListBox) y [**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) para mostrar los datos, estos proporcionan navegación integrada mediante teclas de dirección. O bien, si usas un control [**Button**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button), este ya controla la tecla Entrar o la barra espaciadora para la activación de botones.

Para más información acerca de todos los aspectos de la compatibilidad de teclado, incluido el orden de tabulación y la activación o la navegación basadas en teclas, consulta [Accesibilidad de teclado](keyboard-accessibility.md).

<span id="Media_and_captioning"/>
<span id="media_and_captioning"/>
<span id="MEDIA_AND_CAPTIONING"/>

## <a name="media-and-captioning"></a>Contenido multimedia y subtítulos

Normalmente muestras los medios audiovisuales a través de un objeto [**MediaElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement). Puedes usar las API **MediaElement** para controlar la reproducción multimedia. Con fines de accesibilidad, proporciona controles que permitan que los usuarios reproduzcan, pongan en pausa y detengan los medios según sea necesario. En ocasiones, los medios incluyen componentes adicionales que tienen como propósito ofrecer accesibilidad, como los subtítulos o las pistas de audio alternativas que incluyen descripciones narrativas.

<span id="Accessible_text"/>
<span id="accessible_text"/>
<span id="ACCESSIBLE_TEXT"/>

## <a name="accessible-text"></a>Texto accesible

Tres aspectos del texto son pertinentes para la accesibilidad:

* Las herramientas deben determinar si se debe leer el texto como parte de un recorrido de secuencia de tabulación o solo como una representación general del documento. Para ayudar a determinar este aspecto, puedes elegir el elemento adecuado para mostrar el texto o ajustar las propiedades de esos elementos de texto. Cada elemento de texto disponible tiene un propósito específico que, a su vez, suele tener un rol de automatización de la interfaz de usuario correspondiente. Si se usa el elemento incorrecto, es posible que se notifique el rol incorrecto a la automatización de la interfaz de usuario y que se genere confusión en la experiencia de un usuario de tecnología de asistencia.
* Muchos usuarios tienen problemas de vista que les dificulta la lectura de texto, a menos de que tenga el contraste adecuado con respecto al color de fondo. La manera en que esto afecta al usuario no es algo que los diseñadores de aplicaciones que no padecen esos problemas de vista puedan reconocer de manera intuitiva. Por ejemplo, una mala elección del color en el diseño puede generar que algunos usuarios daltónicos no puedan leer el texto. Las recomendaciones de accesibilidad que se realizaron originalmente para contenido web definen las normas sobre contraste que permiten evitar estos problemas también en las aplicaciones. Para más información, consulta [Requisitos para texto accesible](accessible-text-requirements.md).
* Muchos usuarios tienen dificultades en leer texto que es demasiado pequeño. En primer lugar, puedes evitar este problema si creas un tamaño de texto lo suficientemente grande para la interfaz de usuario de la aplicación. Pero eso resulta complicado en las aplicaciones que muestran grandes cantidades de texto o texto intercalado con otros elementos visuales. En estos casos, asegúrate de que la aplicación interactúe correctamente con las funciones del sistema que pueden aumentar el tamaño de la pantalla para que el texto de las aplicaciones aumente de tamaño con estas. (Algunos usuarios cambian los valores de ppp como opción de accesibilidad. Esta opción está disponible desde **Aumentar el tamaño de los objetos en pantalla**, en la sección **Accesibilidad**, que redirige a la interfaz de usuario del **Panel de control**, que incluye **Apariencia y personalización** / **Pantalla**).

<span id="Supporting_high-contrast_themes"/>
<span id="supporting_high-contrast_themes"/>
<span id="SUPPORTING_HIGH-CONTRAST_THEMES"/>

## <a name="supporting-high-contrast-themes"></a>Compatibilidad con temas de contraste alto

Los controles de la interfaz de usuario usan una representación visual que se define como parte de un diccionario de temas de recursos XAML. Uno o varios de estos temas se usan específicamente cuando el sistema se establece en contraste alto. Cuando el usuario cambia a contraste alto, al buscar el tema adecuado de forma dinámica en un diccionario de recursos, todos tus controles de la interfaz de usuario también usarán un tema de contraste alto adecuado. Debes asegurarte de no haber deshabilitado los temas mediante la especificación de un estilo explícito o el uso de otra técnica de aplicación de estilo que impida la carga de temas de contraste alto e invalide los cambios de estilo. Para más información, consulta [Temas de contraste alto](high-contrast-themes.md).

<span id="Design_for_alternative_UI"/>
<span id="design_for_alternative_ui"/>
<span id="DESIGN_FOR_ALTERNATIVE_UI"/>

## <a name="design-for-alternative-ui"></a>Diseño para interfaces de usuario alternativas

Cuando diseñes tus aplicaciones, ten en cuenta su uso por parte de personas con movilidad, vista y audición limitadas. Dado que los productos de tecnología de asistencia hacen gran uso de las interfaces de usuario estándares, es particularmente importante proporcionar buena compatibilidad para teclado y lectores de pantalla aunque no realices ningún otro ajuste para la accesibilidad.

En muchos casos, puedes transmitir información esencial mediante el uso de múltiples técnicas a fin de ampliar tu público. Por ejemplo, puedes resaltar información usando tanto información de color como iconos para ayudar a los usuarios con daltonismo; además, puedes mostrar alertas visuales junto con efectos de sonido para ayudar a los usuarios con discapacidades auditivas.

Si fuera necesario, puedes proporcionar elementos de interfaz de usuario alternativos y accesibles que quitan completamente las animaciones y los elementos que no son esenciales, y proporcionar otras simplificaciones para hacer que la experiencia del usuario sea más sencilla. El siguiente ejemplo de código muestra cómo presentar una instancia de [**UserControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.UserControl) en lugar de otra según una configuración de usuario.

XAML

```xml
<StackPanel x:Name="LayoutRoot" Background="White">

  <CheckBox x:Name="ShowAccessibleUICheckBox" Click="ShowAccessibleUICheckBox_Click">
    Show Accessible UI
  </CheckBox>

  <UserControl x:Name="ContentBlock">
    <local:ContentPage/>
  </UserControl>

</StackPanel>
```

Visual Basic

```vb
Private Sub ShowAccessibleUICheckBox_Click(ByVal sender As Object,
    ByVal e As RoutedEventArgs)

    If (ShowAccessibleUICheckBox.IsChecked.Value) Then
        ContentBlock.Content = New AccessibleContentPage()
    Else
        ContentBlock.Content = New ContentPage()
    End If
End Sub
```

C#

```csharp
private void ShowAccessibleUICheckBox_Click(object sender, RoutedEventArgs e)
{
    if ((sender as CheckBox).IsChecked.Value)
    {
        ContentBlock.Content = new AccessibleContentPage();
    }
    else
    {
        ContentBlock.Content = new ContentPage();
    }
}
```

<span id="Verification_and_publishing"/>
<span id="verification_and_publishing"/>
<span id="VERIFICATION_AND_PUBLISHING"/>

## <a name="verification-and-publishing"></a>Verificación y publicación

Para obtener más información sobre las declaraciones de accesibilidad y la publicación de la aplicación, consulta [Accesibilidad en la Tienda](accessibility-in-the-store.md).

> [!NOTE]
> Declarar la aplicación como accesible solo es pertinente para el Microsoft Store.

<span id="Assistive_technology_support_in_custom_controls"/>
<span id="assistive_technology_support_in_custom_controls"/>
<span id="ASSISTIVE_TECHNOLOGY_SUPPORT_IN_CUSTOM_CONTROLS"/>

## <a name="assistive-technology-support-in-custom-controls"></a>Compatibilidad con tecnologías de asistencia en controles personalizados

Cuando crees un control personalizado, te recomendamos que también implementes o extiendas una o varias subclases de [**AutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer) para proporcionar compatibilidad con accesibilidad. En algunos casos, siempre y cuando uses la misma clase del mismo nivel que usó la clase de control base, la compatibilidad de la automatización con tu clase derivada es adecuada en un nivel básico. De todos modos, debes probarlo. Y recuerda que la implementación de una clase del mismo nivel es un procedimiento recomendado para que esta pueda notificar correctamente el nombre de clase de tu nueva clase de control. La implementación de la automatización del mismo nivel personalizada conlleva algunos pasos. Para obtener más información, vea [elementos de automatización del mismo nivel](custom-automation-peers.md).

<span id="Assistive_technology_support_in_apps_that_support_XAML___Microsoft_DirectX_interop"/>
<span id="assistive_technology_support_in_apps_that_support_xaml___microsoft_directx_interop"/>
<span id="ASSISTIVE_TECHNOLOGY_SUPPORT_IN_APPS_THAT_SUPPORT_XAML___MICROSOFT_DIRECTX_INTEROP"/>

## <a name="assistive-technology-support-in-apps-that-support-xaml--microsoft-directx-interop"></a>Compatibilidad para tecnología de asistencia en aplicaciones que admiten la interoperabilidad XAML/Microsoft DirectX

No se puede acceder de manera predeterminada al contenido de Microsoft DirectX hospedado en una interfaz de usuario XAML (mediante [**SwapChainPanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) o [**SurfaceImageSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SurfaceImageSource)). El [ejemplo de interoperabilidad XAML SwapChainPanel DirectX](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208.1%20Store%20app%20samples/%5BC%23%5D-Windows%208.1%20Store%20app%20samples/XAML%20SwapChainPanel%20DirectX%20interop%20sample) muestra cómo crear sistemas del mismo nivel de automatización de la interfaz de usuario para el contenido de DirectX hospedado. Esta técnica permite que se pueda acceder al contenido hospedado a través de la automatización de la interfaz de usuario.

## <a name="related-topics"></a>Temas relacionados

* [**Windows. UI. Xaml. Automation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation)
* [Diseño de accesibilidad](https://docs.microsoft.com/windows/uwp/accessibility/accessibility-overview)
* [Ejemplo de accesibilidad XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/master/Official%20Windows%20Platform%20Sample/XAML%20accessibility%20sample)
* [Accesibilidad](accessibility.md)
* [Introducción a narrador](https://support.microsoft.com/help/22798/windows-10-complete-guide-to-narrator)
