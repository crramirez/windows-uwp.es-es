---
Description: La información de accesibilidad básica se suele clasificar en nombre, rol y valor. En este tema se describe el código para ayudar a que tu aplicación exponga la información básica requerida por las tecnologías de asistencia.
title: Exponer información básica de accesibilidad
ms.assetid: 9641C926-68C9-4842-8B55-C38C39A9E5C5
label: Expose basic accessibility information
template: detail.hbs
---

Exponer información básica de accesibilidad
==========================================================================================================

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


La información de accesibilidad básica se suele clasificar en nombre, rol y valor. En este tema se describe el código para ayudar a que tu aplicación exponga la información básica requerida por las tecnologías de asistencia.

<span id="accessible_name"></span><span id="ACCESSIBLE_NAME"></span>Nombre accesible
-----------------------------------------------------------------------------------

Un nombre accesible es una cadena de texto descriptiva y corta que un lector de pantalla usa para anunciar un elemento de la interfaz de usuario. Establece el nombre accesible de los elementos de la interfaz de usuario para que tengan un significado que es importante para comprender el contenido o interactuar con la interfaz de usuario. Generalmente, esos elementos incluyen imágenes, campos de entrada, botones y controles.

En esta tabla, se describe cómo definir un nombre accesible para varios tipos de elementos en una interfaz de usuario XAML.

| Tipo de elemento      | Descripción                                                                                                                                                                                                                                                                                                                                                            |
|-------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Texto estático       | En los elementos [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) y [**RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/BR227565), se determina automáticamente un nombre accesible del texto visible (interno). Todo el texto incluido en ese elemento se usa como nombre. Consulta [Nombre del texto interno](#name_from_inner_text).                                                                    |
| Imágenes            | El elemento XAML [**Image**](https://msdn.microsoft.com/library/windows/apps/BR242752) no tiene un equivalente directo con el atributo **alt** de HTML de **img** y elementos similares. Usa [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770) para proporcionar un nombre o usa la técnica de subtítulos. Consulta [Nombres accesibles para imágenes](#images).                                   |
| Elementos de formulario     | El nombre accesible para un elemento de formulario debe ser el mismo que la etiqueta que se muestra para ese elemento. Consulta [Etiquetas y LabeledBy](#labels).                                                                                                                                                                                                                        |
| Botones y vínculos | De manera predeterminada, el nombre accesible de un botón o vínculo se basa en el texto visible, usando las mismas reglas descritas en [Nombre del texto interno](#name_from_inner_text). En los casos en los que un botón incluye solo una imagen, usa [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770) para proporcionar un equivalente de solo texto de la acción prevista del botón. |

 

Las mayoría de los elementos contenedores, como paneles, no promueven su contenido como nombre accesible. Esto se debe a que el contenido del elemento debe notificar un nombre y su respectivo rol, no su contenedor. El elemento contenedor puede notificar que se trata de un elemento que tiene elementos secundarios en una representación de automatización de la interfaz de usuario de Microsoft para que la lógica de las tecnologías de asistencia pueda recorrerlo. Pero los usuarios de tecnologías de asistencia, por lo general, no necesitan saber sobre los contenedores y, por consiguiente, la mayoría de los contenedores no llevan nombre.

<span id="role_value"></span><span id="ROLE_VALUE"></span>Rol y valor
------------------------------------------------------------------------

Los controles y otros elementos de la interfaz de usuario que forman parte del vocabulario XAML de Windows implementan compatibilidad para la automatización de la interfaz de usuario para notificar el rol y el valor como parte de sus definiciones. Puedes usar las herramientas de automatización de la interfaz de usuario para los controles o puedes leer la documentación para las implementaciones de [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) de cada control. Los roles disponibles en un marco de trabajo de automatización de la interfaz de usuario están definidos en la enumeración [**AutomationControlType**](https://msdn.microsoft.com/library/windows/apps/BR209182). Los clientes de automatización de la interfaz de usuario, como las tecnologías de asistencia pueden obtener información de roles mediante llamadas a los métodos que el marco de trabajo de automatización de la interfaz de usuario expone con **AutomationPeer** del control.

No todos los controles tienen un valor. Los controles que tienen un valor proporcionan esta información a la automatización de la interfaz de usuario mediante los modelos y sistemas del mismo nivel admitidos por ese control. Por ejemplo, un elemento de formulario [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) tiene un valor. Una tecnología de asistencia puede ser un cliente de automatización de la interfaz de usuario y puede descubrir que un valor existe y cuál es su valor. En este caso concreto, **TextBox** es compatible con el patrón [**IValueProvider**](https://msdn.microsoft.com/library/windows/apps/BR242663) mediante las definiciones [**TextBoxAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242550).

**Nota**  En los casos en los que usas [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770) u otras técnicas para proporcionar el nombre accesible de forma explícita, no incluyas el mismo texto que usa el rol de control ni escribas información en el nombre accesible. Por ejemplo, no incluyas cadenas como "botón" o "lista" en el nombre. La información sobre el rol y el tipo proviene de otra propiedad de automatización de la interfaz de usuario (**LocalizedControlType**) que es proporcionada por la compatibilidad predeterminada del control para la automatización de la interfaz de usuario. Muchas tecnologías de asistencia anexan **LocalizedControlType** al nombre accesible, por lo que duplican el rol en el nombre accesible que puede provocar la repetición innecesaria de palabras. Por ejemplo, si asignas a un control [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265) un nombre accesible de "botón" o incluyes "botón" como la parte final del nombre, los lectores de pantalla podrían leer esto como "botón botón". Debes probar este aspecto de tu información de accesibilidad usando el Narrador.

 

<span id="Influencing_the_UI_Automation_tree_views"></span><span id="influencing_the_ui_automation_tree_views"></span><span id="INFLUENCING_THE_UI_AUTOMATION_TREE_VIEWS"></span>Influencia en las vistas de árbol de automatización de la interfaz de usuario
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

El marco de trabajo de automatización de la interfaz de usuario tiene un concepto de vistas de árbol, en el que los clientes de automatización de la interfaz de usuario pueden recuperar las relaciones entre los elementos en una interfaz de usuario usando tres posibles vistas: raw, control y content. La vista control es la que suelen usar los clientes de automatización de la interfaz de usuario porque proporciona una buena representación y organización de los elementos en una interfaz de usuario que es interactiva. Las herramientas de pruebas normalmente permiten elegir qué vista de árbol se usará cuando la herramienta presente la organización de los elementos.

De manera predeterminada, cualquier clase derivada de [**Control**](https://msdn.microsoft.com/library/windows/apps/BR209390) y algunos otros elementos aparecerán en la vista de control cuando el marco de trabajo de automatización de la interfaz de usuario represente la interfaz de usuario para una aplicación para la Plataforma universal de Windows (UWP) con C++, C# o Visual Basic. Pero a veces no quieres que un elemento aparezca en la vista control debido a la composición de la interfaz de usuario, en la que el elemento duplica información o presenta información que no es importante para los escenarios de accesibilidad. Usa la propiedad adjunta [**AutomationProperties.AccessibilityView**](https://msdn.microsoft.com/library/windows/apps/Dn251788) para cambiar la manera en que los elementos se exponen en las vistas de árbol. Si colocas un elemento en el árbol **Raw**, la mayoría de las tecnologías de asistencia no notificarán ese elemento como parte de sus vistas. Para ver algunos ejemplos de cómo funciona esto en los controles existentes, abre el archivo XAML de referencia de diseño generic.xaml en un editor de texto y busca **AutomationProperties.AccessibilityView** en las plantillas.

<span id="name_from_inner_text"></span><span id="NAME_FROM_INNER_TEXT"></span>Nombre del texto interno
--------------------------------------------------------------------------------------------------

Para simplificar el uso de cadenas que ya existen en la interfaz de usuario visible para los valores de nombres accesibles, muchos de los controles y otros elementos de la interfaz de usuario permiten determinar automáticamente un nombre accesible predeterminado en función del texto interno del elemento o a partir de los valores de cadena de las propiedades de contenido.

-   [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652), [**RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/BR227565), [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) y **RichTextBlock** promueven el valor de la propiedad **Text** como nombre accesible predeterminado.
-   Todas las subclases [**ContentControl**](https://msdn.microsoft.com/library/windows/apps/BR209365) usan una técnica iterativa "ToString" para encontrar cadenas en su valor [**Content**](https://msdn.microsoft.com/library/windows/apps/BR209365_content) y promueven estas cadenas como nombre accesible predeterminado.

**Nota**   Tal y como impone la automatización de la interfaz de usuario, la longitud del nombre accesible no puede ser superior a 2048 caracteres. Si una cadena usada para determinar el nombre accesible de manera automática excede ese límite, el nombre accesible se truncará en ese punto.

 

<span id="images"></span><span id="IMAGES"></span>Nombres accesibles para imágenes
-----------------------------------------------------------------------------

Para admitir lectores de pantalla y proporcionar información de identificación básica de cada elemento de la interfaz de usuario, en ocasiones debes proporcionar alternativas de texto a información no textual, como imágenes o gráficos (y excluir todo elemento meramente decorativo o estructural). Estos elementos no tienen texto interno, por lo que el nombre accesible no tendrá un valor calculado. Puedes definir el nombre accesible configurando la propiedad adjunta [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770) como se muestra en este ejemplo.

<span codelanguage="XAML"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">XAML</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code>&lt;Image Source=&quot;product.png&quot;
  AutomationProperties.Name=&quot;An image of a customer using the product.&quot;/&gt;</code></pre></td>
</tr>
</tbody>
</table>

De manera alternativa, considera incluir un subtítulo que aparezca en la interfaz de usuario visible y que también funcione como información de accesibilidad asociada con la etiqueta para el contenido de imagen. A continuación, se incluye un ejemplo:

<span codelanguage="XAML"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">XAML</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code>&lt;Image HorizontalAlignment=&quot;Left&quot; Width=&quot;480&quot; x:Name=&quot;img_MyPix&quot;
  Source=&quot;snoqualmie-NF.jpg&quot;
  AutomationProperties.LabeledBy=&quot;{Binding ElementName=caption_MyPix}&quot;/&gt;
&lt;TextBlock x:Name=&quot;caption_MyPix&quot;&gt;
Mount Snoqualmie Skiing
&lt;/TextBlock&gt;</code></pre></td>
</tr>
</tbody>
</table>

<span id="labels"></span><span id="LABELS"></span>Etiquetas y LabeledBy
----------------------------------------------------------------------

La manera preferida de asociar una etiqueta con un elemento de formulario es usar [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) con **x:Name** para el texto de la etiqueta y después definir la propiedad adjunta [**AutomationProperties.LabeledBy**](https://msdn.microsoft.com/library/windows/apps/Hh759769) en el elemento de formulario para hacer referencia a la etiqueta **TextBlock** por su nombre XAML. Si usas este modelo, cuando el usuario hace clic en la etiqueta, el foco se mueve al control asociado y las tecnologías de asistencia pueden usar el texto de la etiqueta como nombre accesible del campo del formulario. A continuación, se incluye un ejemplo que muestra esta técnica.

<span codelanguage="XAML"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">XAML</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code> &lt;StackPanel x:Name=&quot;LayoutRoot&quot; Background=&quot;White&quot;&gt;
   &lt;StackPanel Orientation=&quot;Horizontal&quot;&gt;
     &lt;TextBlock Name=&quot;lbl_FirstName&quot;&gt;First name&lt;/TextBlock&gt;
     &lt;TextBox
      AutomationProperties.LabeledBy=&quot;{Binding ElementName=lbl_FirstName}&quot;
      Name=&quot;tbFirstName&quot; Width=&quot;100&quot;/&gt;
   &lt;/StackPanel&gt;
   &lt;StackPanel Orientation=&quot;Horizontal&quot;&gt;
     &lt;TextBlock Name=&quot;lbl_LastName&quot;&gt;Last name&lt;/TextBlock&gt;
     &lt;TextBox
      AutomationProperties.LabeledBy=&quot;{Binding ElementName=lbl_LastName}&quot;
      Name=&quot;tbLastName&quot; Width=&quot;100&quot;/&gt;
   &lt;/StackPanel&gt;
 &lt;/StackPanel&gt;</code></pre></td>
</tr>
</tbody>
</table>

<span id="accessible_description"></span><span id="ACCESSIBLE_DESCRIPTION"></span>Descripción accesible (opcional)
-------------------------------------------------------------------------------------------------------------------

Una descripción accesible proporciona información de accesibilidad adicional acerca de un elemento de la interfaz de usuario particular. Generalmente proporcionas una descripción accesible cuando un nombre accesible solo no transmite suficientemente el propósito de un elemento.

El lector de pantalla Narrador lee la descripción accesible de un elemento solamente cuando el usuario solicita más información acerca del elemento presionando Bloq Mayús+F.

El nombre accesible está destinado a identificar el control, en lugar de documentar su comportamiento. Si una pequeña descripción no es suficiente para explicar el control, podrás establecer la propiedad adjunta [**AutomationProperties.HelpText**](https://msdn.microsoft.com/library/windows/apps/Hh759765) además de [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770).

<span id="Testing_accessibility_early_and_often"></span><span id="testing_accessibility_early_and_often"></span><span id="TESTING_ACCESSIBILITY_EARLY_AND_OFTEN"></span>Probar la accesibilidad desde el principio y con frecuencia
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

En última instancia, el mejor enfoque para admitir lectores de pantalla es probar tu aplicación usando uno tú mismo. Eso mostrará cómo se comporta el lector de pantalla y qué información de accesibilidad básica puede faltar en la aplicación. Después, puedes ajustar los valores de la interfaz de usuario o propiedad de automatización de la interfaz de usuario en consecuencia. Para más información, consulta [Pruebas de accesibilidad](accessibility-testing.md).

Una de las herramientas que puedes usar para probar la accesibilidad se llama **AccScope**. La herramienta **AccScope** es especialmente útil porque puedes ver representaciones visuales de la interfaz de usuario que muestran cómo verían tu aplicación las tecnologías de asistencia como un árbol de automatización. En particular, hay un modo de Narrador que muestra cómo el Narrador obtiene el texto de la aplicación y cómo organiza los elementos de la interfaz de usuario. AccScope está diseñado para que pueda usarse y resulte útil durante todo el ciclo de desarrollo de una aplicación, incluso en la fase de diseño preliminar. Para más información, consulta el tema sobre [AccScope](https://msdn.microsoft.com/library/windows/desktop/Dn433239).

<span id="Accessible_names_from_dynamic_data"></span><span id="accessible_names_from_dynamic_data"></span><span id="ACCESSIBLE_NAMES_FROM_DYNAMIC_DATA"></span>Nombres accesibles de datos dinámicos
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Windows admite muchos controles que se pueden usar para mostrar valores que provienen de un origen de datos asociado, a través de una función conocida como *enlace de datos*. Cuando rellenas listas con elementos de datos, puedes tener que usar una técnica que establezca nombres accesibles para elementos de lista enlazados a datos después de que se haya rellenado la lista inicial. Para más información, consulta el "Escenario 4" en la [muestra de accesibilidad XAML](http://go.microsoft.com/fwlink/p/?linkid=238570).

<span id="Accessible_names_and_localization"></span><span id="accessible_names_and_localization"></span><span id="ACCESSIBLE_NAMES_AND_LOCALIZATION"></span>Nombres accesibles y localización
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Para asegurarte de que el nombre accesible también sea un elemento que está localizado, deberías usar técnicas adecuadas para almacenar las cadenas localizables como recursos y después hacer referencia a las conexiones de los recursos con valores [directiva x:Uid](https://msdn.microsoft.com/library/windows/apps/Mt204791). Si el nombre accesible proviene de un uso de [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770) establecido de manera explícita, asegúrate de que esa cadena también sea localizable.

Ten en cuenta que las propiedades adjuntas, como las propiedades [**AutomationProperties**](https://msdn.microsoft.com/library/windows/apps/BR209081), usan una sintaxis de calificación especial para el nombre del recurso, de manera que el recurso hace referencia a la propiedad adjunta como si se aplicara a un elemento específico. Por ejemplo, el nombre de recurso [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770) tal y como se aplica a un elemento de la interfaz de usuario denominado `MediumButton` es: `MediumButton.[using:Windows.UI.Xaml.Automation]AutomationProperties.Name`.

<span id="related_topics"></span>Temas relacionados
-----------------------------------------------

* [Accesibilidad](accessibility.md)
* [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770)
* [Muestra de accesibilidad XAML](http://go.microsoft.com/fwlink/p/?linkid=238570)
* [Pruebas de accesibilidad](accessibility-testing.md)
 

 





<!--HONumber=Mar16_HO3-->


