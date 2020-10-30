---
description: En este tema se describen los procedimientos recomendados sobre accesibilidad de texto en una aplicación mediante la configuración de los colores de texto y fondo, de forma que cumplan con la relación de contraste necesaria.
ms.assetid: BA689C76-FE68-4B5B-9E8D-1E7697F737E6
title: Requisitos de texto accesible
label: Accessible text requirements
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: aefc53f6d28d2c30566680ac985a4712040ea8e0
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93032618"
---
# <a name="accessible-text-requirements"></a>Requisitos de texto accesible  




En este tema se describen los procedimientos recomendados sobre accesibilidad de texto en una aplicación mediante la configuración de los colores de texto y fondo, de forma que cumplan con la relación de contraste necesaria. También se analizan los roles de automatización de la interfaz de usuario de Microsoft que pueden tener los elementos de texto en una aplicación para la Plataforma universal de Windows (UWP) con C++, C# o Visual Basic y los procedimientos recomendados para texto en gráficos.

<span id="contrast_rations"/>
<span id="CONTRAST_RATIONS"/>

## <a name="contrast-ratios"></a>Relaciones de contraste  
Aunque los usuarios siempre tienen la opción de cambiar a un modo de contraste alto, el diseño de la aplicación para el texto debe considerar esa opción como último recurso. Un procedimiento mucho más recomendable es asegurarse de que el texto de la aplicación cumpla con ciertos criterios establecidos para el nivel de contraste entre el texto y su fondo. La evaluación del nivel de contraste se basa en técnicas deterministas que no tienen en cuenta el matiz del color. Por ejemplo, si hay texto rojo sobre un fondo verde, es posible que un usuario daltónico no pueda leer el texto. Si se comprueba y corrige la relación de contraste, se puede evitar este tipo de problemas de accesibilidad.

Las recomendaciones de contraste de texto aquí documentadas se basan en un estándar de accesibilidad web, [G18: asegurarse de que exista una relación de contraste de al menos 4.5:1 entre el texto (e imágenes de texto) y el fondo que se encuentra detrás de él](https://www.w3.org/TR/WCAG20-TECHS/G18.html). Este criterio se encuentra en la especificación sobre las *técnicas de W3C para WCAG 2.0* .

Para considerarse accesible, el texto visible debe tener una relación de contraste de luminosidad mínima de 4.5:1 si se lo compara con el fondo. Algunas excepciones son los logotipos y el texto ocasional, como el texto que forma parte de un componente inactivo de la interfaz de usuario.

Se excluye el texto que es decorativo y no transmite ninguna información. Por ejemplo, si se usan palabras aleatorias para crear un fondo, y las palabras pueden reordenarse o substituirse sin cambiar el significado, se consideran decorativas y no necesitan cumplir con este criterio.

Usa herramientas de contraste de color para comprobar que la relación de contraste del texto visible sea aceptable. Consulta [Técnicas de WCAG 2.0 G18 (Sección recursos)](https://www.w3.org/TR/WCAG20-TECHS/G18.html#G18-resources) para ver las herramientas que pueden probar las relaciones de contraste.

> [!NOTE]
> Algunas de las herramientas indicadas en el tema sobre las técnicas de WCAG 2.0 G18 no se pueden usar de manera interactiva con una aplicación para UWP. Es posible que tengas que especificar valores de colores para el primer plano y el fondo manualmente en la herramienta, o realizar capturas de pantalla de la interfaz de usuario de la aplicación y después ejecutar la herramienta de relación de contraste en la imagen capturada.

<span id="Text_element_roles"/>
<span id="text_element_roles"/>
<span id="TEXT_ELEMENT_ROLES"/>

## <a name="text-element-roles"></a>Roles de elementos de texto  
Una aplicación para UWP puede usar estos elementos predeterminados (comúnmente denominados *elementos de texto* o *controles de textedit* ):

* [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock): El rol es [**Text**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType).
* [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox): El rol es [**Edit**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType).
* [**RichTextBlock**](/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock) (y la clase de desbordamiento [**RichTextBlockOverflow**](/uwp/api/windows.ui.xaml.controls.richtextblockoverflow)): el rol es [**Text**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType)
* [**RichEditBox**](/uwp/api/Windows.UI.Xaml.Controls.RichEditBox): El rol es [**Edit**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType).

Cuando un control notifica que tiene un rol [**Edit**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType), las tecnologías de asistencia suponen que los usuarios tienen mecanismos para cambiar los valores. Por tanto, si pones texto estático en un elemento [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox), provocarás que se malinterprete el rol y, por consiguiente, la estructura de la aplicación para el usuario de accesibilidad.

En los modelos de texto de XAML, hay dos elementos que se usan principalmente para texto estático, [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) y [**RichTextBlock**](/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock). Ninguno de ellos son una subclase de [**Control**](/uwp/api/Windows.UI.Xaml.Controls.Control) y, por lo tanto, ninguno es activable mediante teclado ni puede aparecer en el orden de tabulación. Pero eso no significa que las tecnologías de asistencia no puedan leerlos. Los lectores de pantalla suelen estar diseñados para admitir varios modos de lectura del contenido de una aplicación, incluido un modo de lectura dedicado o patrones de navegación que van más allá del foco y el orden de tabulación, como un "cursor virtual". Por lo tanto, no pongas texto estático en contenedores activables para que el orden de tabulación lleve allí al usuario. Los usuarios de tecnologías de asistencia esperan que lo que hay en el orden de tabulación sea interactivo y si encuentran texto estático, resultará más confuso que útil. Debes probarlo con Narrador para hacerte una idea de cuál será la experiencia del usuario con tu aplicación cuando use un lector de pantalla para examinar el texto estático de dicha aplicación.

<span id="Auto-suggest_accessibility"/>
<span id="auto-suggest_accessibility"/>
<span id="AUTO-SUGGEST_ACCESSIBILITY"/>

## <a name="auto-suggest-accessibility"></a>Accesibilidad de las sugerencias automáticas  
Cuando un usuario escribe en un campo de entrada y aparece una lista de sugerencias posibles, este tipo de escenario se conoce como sugerencia automática. Esto es habitual en la línea **A:** de un campo de correo, el cuadro de búsqueda de Cortana en Windows, el campo de entrada de la dirección URL en Microsoft Edge, el campo de entrada de la ubicación en la aplicación El Tiempo, etc. Si usas un [**AutosuggestBox**](/uwp/api/windows.ui.xaml.controls.autosuggestbox) de XAML o los controles HTML intrínsecos, esta experiencia ya está enlazada automáticamente de manera predeterminada. Para que esta experiencia sea accesible, deben asociarse el campo de entrada y la lista. Esto se explica en la sección [Implementación de las sugerencias automáticas](#implementing_auto-suggest).

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
Para que esta experiencia sea accesible, el campo de entrada y la lista deben asociarse en el árbol de UIA. Esta asociación se realiza con la propiedad [UIA_ControllerForPropertyId](/windows/win32/winauto/uiauto-automation-element-propids) en las aplicaciones de escritorio o con la propiedad [ControlledPeers](/uwp/api/windows.ui.xaml.automation.automationproperties.getcontrolledpeers) en las aplicaciones para UWP.

En un nivel alto, existen 2 tipos de experiencias de sugerencias automáticas.

**Selección predeterminada**  
Si se realiza una selección predeterminada en la lista, Narrador busca un evento [**UIA_SelectionItem_ElementSelectedEventId**](/windows/desktop/WinAuto/uiauto-event-ids) en una aplicación de escritorio o que se genere el evento [**AutomationEvents.SelectionItemPatternOnElementSelected**](/uwp/api/windows.ui.xaml.automation.peers.automationevents) en una aplicación para UWP. Cada vez que cambia la selección, cuando el usuario escribe otra letra y las sugerencias se actualizan o cuando un usuario navega por la lista, debería activarse el evento **ElementSelected** .

![Lista con una selección predeterminada](images/autosuggest-default-selection.png)<br/>
_Ejemplo en el que existe una selección predeterminada_

**Ninguna selección predeterminada**  
Si no hay ninguna selección predeterminada, como en el cuadro de ubicación de la aplicación El Tiempo, Narrador busca que cada vez que se actualice la lista, en dicha lista se genere el evento [**UIA_LayoutInvalidatedEventId**](/windows/desktop/WinAuto/uiauto-event-ids) en una aplicación de escritorio o el evento [**LayoutInvalidated**](/uwp/api/windows.ui.xaml.automation.peers.automationevents) en una aplicación para UWP.

![Lista sin ninguna selección predeterminada](images/autosuggest-no-default-selection.png)<br/>
_Ejemplo en el que no existe ninguna selección predeterminada_

### <a name="xaml-implementation"></a>Implementación de XAML  
Si se utiliza la clase XAML [**AutosuggestBox**](/uwp/api/windows.ui.xaml.controls.autosuggestbox) predeterminada, todo está ya enlazado de forma automática. Si vas a crear tu propia experiencia de sugerencias automáticas con una clase [**TextBox**](/uwp/api/windows.ui.xaml.controls.textbox) y una lista, tendrás que establecer la lista como [**AutomationProperties.ControlledPeers**](/uwp/api/windows.ui.xaml.automation.automationproperties.getcontrolledpeers) en la clase **TextBox** . Debes activar el evento **AutomationPropertyChanged** para la propiedad [**ControlledPeers**](/uwp/api/windows.ui.xaml.automation.automationproperties.getcontrolledpeers) cada vez que agregues o quites esta propiedad y también activar tus propios eventos [**SelectionItemPatternOnElementSelected**](/uwp/api/windows.ui.xaml.automation.peers.automationevents) o [**LayoutInvalidated**](/uwp/api/windows.ui.xaml.automation.peers.automationevents) según el tipo de escenario, algo que se explica anteriormente en este mismo artículo.

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

Siempre que sea posible, evita incluir texto en un gráfico. Por ejemplo, las tecnologías de asistencia no podrán leer automáticamente los textos que incluyas en el archivo de origen de imagen que se muestre en la aplicación como un elemento [**Image**](/uwp/api/Windows.UI.Xaml.Controls.Image) ni tampoco acceder a ellos. Si tienes que usar texto en los gráficos, asegúrate de que el valor [**AutomationProperties.Name**](/dotnet/api/system.windows.automation.automationproperties.name) que proporcionas como equivalente del texto alternativo incluya el texto o un resumen del significado del texto. Se aplican consideraciones similares si va a crear caracteres de texto a partir de vectores como parte de una [**ruta de acceso**](/uwp/api/Windows.UI.Xaml.Shapes.Path)o mediante [**glifos**](/uwp/api/Windows.UI.Xaml.Documents.Glyphs).

<span id="Text_font_size"/>
<span id="text_font_size"/>
<span id="TEXT_FONT_SIZE"/>

## <a name="text-font-size-and-scale"></a>Tamaño y escala de fuente de texto

Los usuarios pueden tener dificultades para leer texto en una aplicación cuando los usos de las fuentes son simplemente demasiado pequeños, por lo que debe asegurarse de que todo el texto de la aplicación tenga un tamaño razonable en primer lugar.

Una vez que haya hecho lo obvio, Windows incluye varias herramientas de accesibilidad y configuraciones en las que los usuarios pueden aprovechar las ventajas y adaptarse a sus propias necesidades y preferencias para leer texto. Se incluyen los siguientes:

* La herramienta ampliador, que amplía un área seleccionada de la interfaz de usuario. Debe asegurarse de que el diseño de texto de la aplicación no dificulta el uso de la lupa para la lectura.
* Configuración de escala y resolución global en **configuración->>de pantalla->de la escala y el diseño** . Exactamente las opciones de ajuste de tamaño disponibles pueden variar, ya que depende de las capacidades del dispositivo de pantalla.
* Configuración de tamaño de texto en **Configuración: >la facilidad de acceso >pantalla** . Ajuste la configuración de **hacer que el texto sea más grande** para especificar solo el tamaño del texto en los controles auxiliares en todas las aplicaciones y pantallas (todos los controles de texto de UWP admiten la experiencia de escalado de texto sin ninguna personalización o plantilla). 
> [!NOTE]
> La configuración **hacer todo es mayor permite que** un usuario especifique su tamaño preferido para el texto y las aplicaciones en general solo en su pantalla principal.

Varios controles y elementos de texto tienen una propiedad [**IsTextScaleFactorEnabled**](/uwp/api/windows.ui.xaml.controls.textblock.istextscalefactorenabled). Esta propiedad tiene el valor **true** de forma predeterminada. Si **es true** , se puede escalar el tamaño del texto en ese elemento. El ajuste de escala afecta al texto que tiene una pequeña **FontSize** en un grado mayor que el texto que tiene una gran **FontSize** . Puede deshabilitar el cambio de tamaño automático Si establece la propiedad **IsTextScaleFactorEnabled** de un elemento en **false** . 

Vea [escalado de texto](../input/text-scaling.md) para obtener más detalles.

Agregue el marcado siguiente a una aplicación y ejecútelo. Ajuste la configuración de **tamaño de texto** y vea lo que sucede con cada **TextBlock** .

XAML
```xml
<TextBlock Text="In this case, IsTextScaleFactorEnabled has been left set to its default value of true."
    Style="{StaticResource BodyTextBlockStyle}"/>

<TextBlock Text="In this case, IsTextScaleFactorEnabled has been set to false."
    Style="{StaticResource BodyTextBlockStyle}" IsTextScaleFactorEnabled="False"/>
```  

No se recomienda deshabilitar el ajuste de escala de texto al escalar el texto de la interfaz de usuario universalmente en todas las aplicaciones es una experiencia de accesibilidad importante para los usuarios.

También puedes usar el evento [**TextScaleFactorChanged**](/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged) y la propiedad [**TextScaleFactor**](/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactor) para averiguar sobre los cambios realizados en la opción **Tamaño del texto** del teléfono. Este es el procedimiento:

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

El valor de **TextScaleFactor** es un valor Double en el intervalo de \[ 1 a 2,25 \] . El texto más pequeño se aumenta según esta cantidad. El valor se podría usar para, por ejemplo, escalar elementos gráficos para que vayan a tono con el texto. Recuerda, de todas formas, que no todo el texto escala mediante el mismo factor. En términos generales, el texto más grande es el que menos se ve afectado por el ajuste de escala.

Estos tipos tienen una propiedad **IsTextScaleFactorEnabled** :  
* [**ContentPresenter**](/uwp/api/Windows.UI.Xaml.Controls.ContentPresenter)
* [**Control**](/uwp/api/Windows.UI.Xaml.Controls.Control) y clases derivadas
* [**FontIcon**](/uwp/api/Windows.UI.Xaml.Controls.FontIcon)
* [**RichTextBlock**](/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock)
* [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock)
* [**TextElement**](/uwp/api/Windows.UI.Xaml.Documents.TextElement) y clases derivadas

<span id="related_topics"/>

## <a name="related-topics"></a>Temas relacionados  

* [Ajuste de escala de texto](../input/text-scaling.md)
* [Accesibilidad](accessibility.md)
* [Información básica de accesibilidad](basic-accessibility-information.md)
* [XAML text display sample (Muestra de visualización de texto XAML)](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/XAML%20text%20display%20sample%20(Windows%208))
* [Ejemplo de edición de texto XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/XAML%20text%20editing%20sample%20(Windows%208))
* [Ejemplo de accesibilidad XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/XAML%20accessibility%20sample)
