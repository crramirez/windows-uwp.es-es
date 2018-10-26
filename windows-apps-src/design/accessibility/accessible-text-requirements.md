---
author: Xansky
Description: This topic describes best practices for accessibility of text in an app, by assuring that colors and backgrounds satisfy the necessary contrast ratio.
ms.assetid: BA689C76-FE68-4B5B-9E8D-1E7697F737E6
title: Requisitos de texto accesible
label: Accessible text requirements
template: detail.hbs
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 39979ed3fa2fcd85cbf1f1b73d7c37b2dce38f20
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2018
ms.locfileid: "5552353"
---
# <a name="accessible-text-requirements"></a>Requisitos de texto accesible  




En este tema se describen los procedimientos recomendados sobre accesibilidad de texto en una aplicación mediante la configuración de los colores de texto y fondo, de forma que cumplan con la relación de contraste necesaria. También se analizan los roles de automatización de la interfaz de usuario de Microsoft que pueden tener los elementos de texto en una aplicación para la Plataforma universal de Windows (UWP) con C++, C# o Visual Basic y los procedimientos recomendados para texto en gráficos.

<span id="contrast_rations"/>
<span id="CONTRAST_RATIONS"/>

## <a name="contrast-ratios"></a>Relaciones de contraste  
Aunque los usuarios siempre tienen la opción de cambiar a un modo de contraste alto, el diseño de la aplicación para el texto debe considerar esa opción como último recurso. Un procedimiento mucho más recomendable es asegurarse de que el texto de la aplicación cumpla con ciertos criterios establecidos para el nivel de contraste entre el texto y su fondo. La evaluación del nivel de contraste se basa en técnicas deterministas que no tienen en cuenta el matiz del color. Por ejemplo, si hay texto rojo sobre un fondo verde, es posible que un usuario daltónico no pueda leer el texto. Si se comprueba y corrige la relación de contraste, se puede evitar este tipo de problemas de accesibilidad.

Las recomendaciones de contraste de texto aquí documentadas se basan en un estándar de accesibilidad web, [G18: asegurarse de que exista una relación de contraste de al menos 4.5:1 entre el texto (e imágenes de texto) y el fondo que se encuentra detrás de él](http://go.microsoft.com/fwlink/p/?linkid=221823). Este criterio se encuentra en la especificación sobre las *técnicas de W3C para WCAG 2.0*.

Para considerarse accesible, el texto visible debe tener una relación de contraste de luminosidad mínima de 4.5:1 si se lo compara con el fondo. Algunas excepciones son los logotipos y el texto ocasional, como el texto que forma parte de un componente inactivo de la interfaz de usuario.

Se excluye el texto que es decorativo y no transmite ninguna información. Por ejemplo, si se usan palabras aleatorias para crear un fondo, y las palabras pueden reordenarse o substituirse sin cambiar el significado, se consideran decorativas y no necesitan cumplir con este criterio.

Usa herramientas de contraste de color para comprobar que la relación de contraste del texto visible sea aceptable. Consulta [Técnicas de WCAG 2.0 G18 (Sección recursos)](http://www.w3.org/TR/WCAG20-TECHS/G18.html#G18-resources) para ver las herramientas que pueden probar las relaciones de contraste.

> [!NOTE]
> Algunas de las herramientas indicadas en el tema sobre las técnicas de WCAG 2.0 G18 no se pueden usar de manera interactiva con una aplicación para UWP. Es posible que tengas que especificar valores de colores para el primer plano y el fondo manualmente en la herramienta, o realizar capturas de pantalla de la interfaz de usuario de la aplicación y después ejecutar la herramienta de relación de contraste en la imagen capturada.

<span id="Text_element_roles"/>
<span id="text_element_roles"/>
<span id="TEXT_ELEMENT_ROLES"/>

## <a name="text-element-roles"></a>Roles de elementos de texto  
Una aplicación para UWP puede usar estos elementos predeterminados (comúnmente denominados *elementos de texto* o *controles de textedit*):

* [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652): El rol es [**Text**](https://msdn.microsoft.com/library/windows/apps/BR209182).
* [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683): El rol es [**Edit**](https://msdn.microsoft.com/library/windows/apps/BR209182).
* [**RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/BR227565) (y la clase de desbordamiento [**RichTextBlockOverflow**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.richtextblockoverflow)): El rol es [**Text**](https://msdn.microsoft.com/library/windows/apps/BR209182),
* [**RichEditBox**](https://msdn.microsoft.com/library/windows/apps/BR227548): El rol es [**Edit**](https://msdn.microsoft.com/library/windows/apps/BR209182).

Cuando un control notifica que tiene un rol [**Edit**](https://msdn.microsoft.com/library/windows/apps/BR209182), las tecnologías de asistencia suponen que los usuarios tienen mecanismos para cambiar los valores. Por tanto, si pones texto estático en un elemento [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683), provocarás que se malinterprete el rol y, por consiguiente, la estructura de la aplicación para el usuario de accesibilidad.

En los modelos de texto de XAML, hay dos elementos que se usan principalmente para texto estático, [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) y [**RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/BR227565). Ninguno de ellos son una subclase de [**Control**](https://msdn.microsoft.com/library/windows/apps/BR209390) y, por lo tanto, ninguno es activable mediante teclado ni puede aparecer en el orden de tabulación. Pero eso no significa que las tecnologías de asistencia no puedan leerlos. Los lectores de pantalla suelen estar diseñados para admitir varios modos de lectura del contenido de una aplicación, incluido un modo de lectura dedicado o patrones de navegación que van más allá del foco y el orden de tabulación, como un "cursor virtual". Por lo tanto, no pongas texto estático en contenedores activables para que el orden de tabulación lleve allí al usuario. Los usuarios de tecnologías de asistencia esperan que lo que hay en el orden de tabulación sea interactivo y si encuentran texto estático, resultará más confuso que útil. Debes probarlo con Narrador para hacerte una idea de cuál será la experiencia del usuario con tu aplicación cuando use un lector de pantalla para examinar el texto estático de dicha aplicación.

<span id="Auto-suggest_accessibility"/>
<span id="auto-suggest_accessibility"/>
<span id="AUTO-SUGGEST_ACCESSIBILITY"/>

## <a name="auto-suggest-accessibility"></a>Accesibilidad de las sugerencias automáticas  
Cuando un usuario escribe en un campo de entrada y aparece una lista de sugerencias posibles, este tipo de escenario se conoce como sugerencia automática. Esto es habitual en la línea **A:** de un campo de correo, el cuadro de búsqueda de Cortana en Windows, el campo de entrada de la dirección URL en Microsoft Edge, el campo de entrada de la ubicación en la aplicación El Tiempo, etc. Si usas un [**AutosuggestBox**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.autosuggestbox) de XAML o los controles HTML intrínsecos, esta experiencia ya está enlazada automáticamente de manera predeterminada. Para que esta experiencia sea accesible, deben asociarse el campo de entrada y la lista. Esto se explica en la sección [Implementación de las sugerencias automáticas](#implementing_auto-suggest).

Narrador se ha actualizado para que este tipo de experiencia sea accesible con un modo especial de sugerencias. En un nivel alto, si el campo de edición y la lista se conectan correctamente, el usuario podrá:

* Saber que la lista está presente y cuándo se cierra.
* Saber cuántas sugerencias hay disponibles.
* Conocer el elemento seleccionado, si lo hay.
* Mover el foco de Narrador a la lista.
* Navegar por una sugerencia con todos los demás modos de lectura.

![Lista de sugerencias](images/autosuggest-list.png)<br/>
_Ejemplo de una lista de sugerencias_

<span id="Implementing_auto-suggest"/>
<span id="implementing_auto-suggest"/>
<span id="IMPLEMENTING_AUTO-SUGGEST"/>

### <a name="implementing-auto-suggest"></a>Implementación de las sugerencias automáticas  
Para que esta experiencia sea accesible, el campo de entrada y la lista deben asociarse en el árbol de UIA. Esta asociación se realiza con la propiedad [UIA_ControllerForPropertyId](https://msdn.microsoft.com/windows/desktop/ee684017) en las aplicaciones de escritorio o con la propiedad [ControlledPeers](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.automationproperties.getcontrolledpeers) en las aplicaciones para UWP.

En un nivel alto, existen 2 tipos de experiencias de sugerencias automáticas.

**Selección predeterminada**  
Si se realiza una selección predeterminada en la lista, Narrador busca un evento [**UIA_SelectionItem_ElementSelectedEventId**](https://msdn.microsoft.com/library/windows/desktop/ee671223) en una aplicación de escritorio o que se genere el evento [**AutomationEvents.SelectionItemPatternOnElementSelected**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationevents) en una aplicación para UWP. Cada vez que cambia la selección, cuando el usuario escribe otra letra y las sugerencias se actualizan o cuando un usuario navega por la lista, debería activarse el evento **ElementSelected**.

![Lista con una selección predeterminada](images/autosuggest-default-selection.png)<br/>
_Ejemplo en el que existe una selección predeterminada_

**Ninguna selección predeterminada**  
Si no hay ninguna selección predeterminada, como en el cuadro de ubicación de la aplicación El Tiempo, Narrador busca que cada vez que se actualice la lista, en dicha lista se genere el evento [**UIA_LayoutInvalidatedEventId**](https://msdn.microsoft.com/library/windows/desktop/ee671223 ) en una aplicación de escritorio o el evento [**LayoutInvalidated**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationevents) en una aplicación para UWP.

![Lista sin ninguna selección predeterminada](images/autosuggest-no-default-selection.png)<br/>
_Ejemplo en el que no existe ninguna selección predeterminada_

### <a name="xaml-implementation"></a>Implementación de XAML  
Si se utiliza la clase XAML [**AutosuggestBox**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.autosuggestbox) predeterminada, todo está ya enlazado de forma automática. Si vas a crear tu propia experiencia de sugerencias automáticas con una clase [**TextBox**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textbox) y una lista, tendrás que establecer la lista como [**AutomationProperties.ControlledPeers**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.automationproperties.getcontrolledpeers) en la clase **TextBox**. Debes activar el evento **AutomationPropertyChanged** para la propiedad [**ControlledPeers**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.automationproperties.getcontrolledpeers) cada vez que agregues o quites esta propiedad y también activar tus propios eventos [**SelectionItemPatternOnElementSelected**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationevents) o [**LayoutInvalidated**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationevents) según el tipo de escenario, algo que se explica anteriormente en este mismo artículo.

### <a name="html-implementation"></a>Implementación de HTML  
Si usas los controles intrínsecos de HTML, la implementación de UIA ya se habrá asignado automáticamente. A continuación hay un ejemplo de una implementación que ya está enlazada automáticamente:

``` HTML
<label>Sites <input id="input1" type="text" list="datalist1" /></label>
<datalist id="datalist1">
        <option value="http://www.google.com/" label="Google"></option>
        <option value="http://www.reddit.com/" label="Reddit"></option>
</datalist>
```

 Si vas a crear tus propios controles, debes configurar tus propios controles ARIA, que se explican en los estándares de W3C.

<span id="Text_in_graphics"/>
<span id="text_in_graphics"/>
<span id="TEXT_IN_GRAPHICS"/>

## <a name="text-in-graphics"></a>Texto en gráficos  
Siempre que sea posible, evita incluir texto en un gráfico. Por ejemplo, las tecnologías de asistencia no podrán leer automáticamente los textos que incluyas en el archivo de origen de imagen que se muestre en la aplicación como un elemento [**Image**](https://msdn.microsoft.com/library/windows/apps/BR242752) ni tampoco acceder a ellos. Si tienes que usar texto en los gráficos, asegúrate de que el valor [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770) que proporcionas como equivalente del texto alternativo incluya el texto o un resumen del significado del texto. Se aplican consideraciones similares si estás creando caracteres de texto de vectores como parte de una clase [**Path**](/uwp/api/Windows.UI.Xaml.Shapes.Path) o mediante la clase [**Glyphs**](https://msdn.microsoft.com/library/windows/apps/BR209921).

<span id="Text_font_size"/>
<span id="text_font_size"/>
<span id="TEXT_FONT_SIZE"/>

## <a name="text-font-size"></a>Tamaño de fuente del texto  
Muchos lectores tienen problemas para leer el texto que hay en una aplicación cuando se usa un tamaño de fuente de texto que les resulta demasiado pequeño para leerlo. En primer lugar, puedes evitar este problema si creas un tamaño de texto lo suficientemente grande para la interfaz de usuario de la aplicación. También hay tecnologías de asistencia que forman parten de Windows y que permiten a los usuarios cambiar los tamaños de presentación de las aplicaciones o la visualización en general.

* Algunos usuarios cambian los valores de puntos por pulgada (ppp) de su pantalla principal como opción de accesibilidad. Esta opción está disponible en **Aumentar el tamaño de los objetos en pantalla** de **Accesibilidad**, que lleva a la interfaz de usuario del **Panel de control** para la **Apariencia y personalización** / **Pantalla**. Las opciones exactas de tamaño que están disponibles pueden variar según las funciones del dispositivo de presentación.
* La herramienta de lupa puede aumentar el tamaño en un área seleccionada de la interfaz de usuario. Pero es difícil usarla para leer texto.

<span id="Text_scale_factor"/>
<span id="text_scale_factor"/>
<span id="TEXT_SCALE_FACTOR"/>

## <a name="text-scale-factor"></a>Factor de escala de texto  
Varios controles y elementos de texto tienen una propiedad [**IsTextScaleFactorEnabled**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.istextscalefactorenabled). Esta propiedad tiene el valor **true** de forma predeterminada. Cuando su valor es **true**, la opción **Ajuste de escala de texto** del teléfono (**Configuración &gt; Accesibilidad**) hace que el tamaño del texto de ese elemento se agrande. El texto que tiene un tamaño de **FontSize** pequeño se verá más afectado que el texto que tiene un tamaño de **FontSize** grande. De todos modos, puedes deshabilitar el aumento de tamaño automático si estableces la propiedad **IsTextScaleFactorEnabled** del elemento en **false**. Prueba con este marcado, ajusta la opción **Tamaño del texto** en el teléfono y mira a ver qué ocurre con los elementos **TextBlock**:

XAML
```xml
<TextBlock Text="In this case, IsTextScaleFactorEnabled has been left set to its default value of true."
    Style="{StaticResource BodyTextBlockStyle}"/>

<TextBlock Text="In this case, IsTextScaleFactorEnabled has been set to false."
    Style="{StaticResource BodyTextBlockStyle}" IsTextScaleFactorEnabled="False"/>
```  

No deshabilites el aumento de tamaño automático de forma habitual, porque el escalado global de la interfaz de usuario en todas las aplicaciones es una experiencia de accesibilidad importante para los usuarios y estos esperan que funcione también en tu aplicación.

También puedes usar el evento [**TextScaleFactorChanged**](https://msdn.microsoft.com/library/windows/apps/Dn633867) y la propiedad [**TextScaleFactor**](https://msdn.microsoft.com/library/windows/apps/Dn633866) para averiguar sobre los cambios realizados en la opción **Tamaño del texto** del teléfono. Aquí te mostramos cómo:

C#
```csharp
{
    ...
    var uiSettings = new Windows.UI.ViewManagement.UISettings();
    uiSettings.TextScaleFactorChanged += UISettings_TextScaleFactorChanged;
    ...
}

private async void UISettings_TextScaleFactorChanged(Windows.UI.ViewManagement.UISettings sender, object args)
{
    var messageDialog = new Windows.UI.Popups.MessageDialog(string.Format("It's now {0}", sender.TextScaleFactor), "The text scale factor has changed");
    await messageDialog.ShowAsync();
}
```

El valor de **TextScaleFactor** es doble en el intervalo \[1,2\]. El texto más pequeño se aumenta según esta cantidad. El valor se podría usar para, por ejemplo, escalar elementos gráficos para que vayan a tono con el texto. Recuerda, de todas formas, que no todo el texto escala mediante el mismo factor. En términos generales, el texto más grande es el que menos se ve afectado por el ajuste de escala.

Estos tipos tienen una propiedad **IsTextScaleFactorEnabled**:  
* [**ContentPresenter**](https://msdn.microsoft.com/library/windows/apps/BR209378)
* [**Control**](https://msdn.microsoft.com/library/windows/apps/BR209390) y clases derivadas
* [**FontIcon**](https://msdn.microsoft.com/library/windows/apps/Dn279514)
* [**RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/BR227565)
* [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652)
* [**TextElement**](https://msdn.microsoft.com/library/windows/apps/BR209967) y clases derivadas

<span id="related_topics"/>

## <a name="related-topics"></a>Temas relacionados  
* [Accesibilidad](accessibility.md)
* [Información básica de accesibilidad](basic-accessibility-information.md)
* [Muestra de visualización de texto en XAML](http://go.microsoft.com/fwlink/p/?linkid=238579)
* [Muestra de edición de texto en XAML](http://go.microsoft.com/fwlink/p/?linkid=251417)
* [Muestra de accesibilidad XAML](http://go.microsoft.com/fwlink/p/?linkid=238570) 
