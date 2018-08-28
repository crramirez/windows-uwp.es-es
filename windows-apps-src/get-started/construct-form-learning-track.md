---
author: QuinnRadich
title: Pista de aprendizaje - Construir y configurar un formulario
description: Obtén información sobre lo que tienes que hacer para crear un formulario sólido en tu aplicación.
ms.author: quradic
ms.date: 05/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: introducción, uwp, windows 10, pista de aprendizaje, diseño, formulario
ms.localizationpriority: medium
ms.openlocfilehash: c2a851a442cabca4529cd202c90db692c43adcb5
ms.sourcegitcommit: 9a17266f208ec415fc718e5254d5b4c08835150c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2018
ms.locfileid: "2888384"
---
# <a name="create-and-customize-a-form"></a>Crear y personalizar un formulario

Si vas a crear una aplicación que requiere que los usuarios introduzcan una gran cantidad de información, lo más probable es que desees crear un formulario para que lo cumplimenten. En este artículo mostraremos lo que necesitas saber para crear un formulario que sea útil y eficaz.

Este no es un tutorial. Si quieres uno, consulta nuestro [tutorial de diseño adaptativo](../design/basics/xaml-basics-adaptive-layout.md), que te proporcionará una experiencia paso a paso guiada.

Analizaremos qué **controles XAML** se introducen en un formulario, cómo organizarlos mejor en la página y cómo optimizar tu formulario para cambiar tamaños de pantalla. Sin embargo, dado que un formulario consiste en la posición relativa de elementos visuales, abordaremos primero el diseño de página con XAML.

## <a name="what-do-you-need-to-know"></a>¿Qué debes saber?

UWP no tiene un control de formulario explícito que puedes agregar a tu aplicación y configurar. En su lugar, tendrás que crear un formulario organizando una colección de elementos de interfaz de usuario en una página.

Para ello, tendrás que conocer los **paneles de diseño**. Estos son contenedores que contienen elementos de la interfaz de usuario de la aplicación que permiten organizarlos y agruparlos. Colocar paneles de diseño dentro de otros paneles de diseño ofrece una gran cantidad de control sobre dónde y cómo se mostrarán los elementos entre sí. Esto también facilita en gran medida la adaptación de la aplicación para cambiar tamaños de pantalla.

Lee [esta documentación sobre paneles de diseño](../design/layout/layout-panels.md). Debido a que los formularios normalmente se muestran en una o más columnas verticales, querrás agrupar elementos similares en un **StackPanel**y organizarlos dentro de un **RelativePanel** si es necesario. Empieza a agrupar ahora algunos paneles; si necesitas una referencia, a continuación te mostramos un marco de diseño básico para un formulario de dos columnas:

```xaml
<RelativePanel>
    <StackPanel x:Name="Customer" Margin="20">
        <!--Content-->
    </StackPanel>
    <StackPanel x:Name="Associate" Margin="20" RelativePanel.RightOf="Customer">
        <!--Content-->
    </StackPanel>
    <StackPanel x:Name="Save" Orientation="Horizontal" RelativePanel.Below="Customer">
        <!--Save and Cancel buttons-->
    </StackPanel>
</RelativePanel>
```

## <a name="what-goes-in-a-form"></a>Qué se incluye en un formulario

Tendrás que rellenar tu formulario con una gran variedad de [controles XAML](../design/controls-and-patterns/controls-and-events-intro.md). Probablemente ya estás familiarizado con ellos, pero puedes consultar la información si necesitas ponerte al día. En concreto, querrás controles que permiten al usuario escribir texto o elegir de una lista de valores. Esto es una lista básica de opciones que podría agregar: no es necesario leer todo lo relacionado con ellos, simplemente lo suficiente para poder entender qué aspecto tienen y cómo funcionan.

* [Cuadro de texto](../design/controls-and-patterns/text-box.md) permite a un texto de entrada del usuario en la aplicación.
* [ToggleSwitch](../design/controls-and-patterns/toggles.md) permite al usuario elegir entre dos opciones.
* [DatePicker](../design/controls-and-patterns/date-picker.md) permite al usuario seleccionar un valor de fecha.
* [TimePicker](../design/controls-and-patterns/time-picker.md) permite al usuario seleccionar un valor de hora.
* [ComboBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox) se expande para mostrar una lista de elementos seleccionables. Puedes aprender más sobre ellos [aquí](../design/controls-and-patterns/lists.md#drop-down-lists)

También es posible que desees agregar [botones](../design/controls-and-patterns/buttons.md), para que el usuario pueda guardar o cancelar.

## <a name="format-controls-in-your-layout"></a>Controles de formato en el diseño

Ya sabes cómo organizar los paneles de diseño y tener los elementos que te gustaría agregar, pero ¿cómo se les da formato? La página [formularios](../design/controls-and-patterns/forms.md) ofrece instrucciones de diseño específicas. Lee las secciones en **Tipos de formularios** y **Diseño** para obtener consejos útiles. Analizaremos la accesibilidad y el diseño correspondiente más brevemente.

Con ese consejo en mente, debes empezar a agregar los controles que elijas en el diseño, asegurándote de que reciben etiquetas y que tienen la separación correcta. Como ejemplo, aquí se incluye un esquema básico de un formulario de una página con el diseño, controles e instrucciones de diseño anteriores:

```xaml
<RelativePanel>
    <StackPanel x:Name="Customer" Margin="20">
        <TextBox x:Name="CustomerName" Header= "Customer Name" Margin="0,24,0,0" HorizontalAlignment="Left" />
        <TextBox x:Name="Address" Header="Address" PlaceholderText="Address" Margin="0,24,0,0" HorizontalAlignment="Left" />
        <TextBox x:Name="Address2" Margin="0,24,0,0" PlaceholderText="Address 2" HorizontalAlignment="Left" />
            <RelativePanel>
                <TextBox x:Name="City" PlaceholderText="City" Margin="0,24,0,0" HorizontalAlignment="Left" />
                <ComboBox x:Name="State" PlaceholderText="State" Margin="24,24,0,0" RelativePanel.RightOf="City">
                    <!--List of valid states-->
                </ComboBox>
            </RelativePanel>
    </StackPanel>
    <StackPanel x:Name="Associate" Margin="20" RelativePanel.Below="Customer">
        <TextBox x:Name="AssociateName" Header= "Associate" Margin="0,24,0,0" HorizontalAlignment="Left" />
        <DatePicker x:Name="TargetInstallDate" Header="Target install Date" HorizontalAlignment="Left" Margin="0,24,0,0"></DatePicker>
        <TimePicker x:Name="InstallTime" Header="Install Time" HorizontalAlignment="Left" Margin="0,24,0,0"></TimePicker>
    </StackPanel>
    <StackPanel x:Name="Save" Orientation="Horizontal" RelativePanel.Below="Associate">
        <Button Content="Save" Margin="24" />
        <Button Content="Cancel" Margin="24" />
    </StackPanel>
</RelativePanel>
```

Puede personalizar libremente tus controles con más propiedades para mejorar la experiencia visual.

## <a name="make-your-layout-responsive"></a>Hacer el diseño adaptativo

Los usuarios podrían ver la interfaz de usuario en una variedad de dispositivos con diferentes anchos de pantalla. Para garantizar que tienen una buena experiencia independientemente de la pantalla, debes usar [diseño adaptativo](../design/layout/responsive-design.md). Lee esa página para obtener un buen asesoramiento sobre las filosofías de diseño que hay que tener en cuenta al proceder.

La página [Diseños adaptativos con XAML](../design/layout/layouts-with-xaml.md) ofrece una visión detallada de cómo implementarlo. Por ahora, nos centraremos en **diseños fluidos** y **estados visuales en XAML**.

El esquema del formulario básico que hemos reunido ya es un **diseño fluido**, ya que depende principalmente de la posición relativa de controles con únicamente el uso mínimo de posiciones y tamaños de píxel específicos. Ten estas instrucciones en mente para más interfaces de usuario que puedas crear en el futuro.

Algo más importante para diseños adaptativos son los **estados visuales.** Un estado visual define los valores de propiedad que se aplican a un elemento concreto cuando una condición concreta es true. [Lee cómo hacer esto en xaml](../design/layout/layouts-with-xaml.md#set-visual-states-in-xaml-markup)y luego impleméntalos en el formulario. Aquí mostramos un ejemplo *muy* básico de cómo podría aparecer en nuestro ejemplo anterior:

```xaml
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>
        <VisualState>
            <VisualState.StateTriggers>
                <AdaptiveTrigger MinWindowWidth="640" />
            </VisualState.StateTriggers>

            <VisualState.Setters>
                <Setter Target="Associate.(RelativePanel.RightOf)" Value="Customer"/>
                <Setter Target="Associate.(RelativePanel.Below)" Value=""/>
                <Setter Target="Save.(RelativePanel.Below)" Value="Customer"/>
            </VisualState.Setters>
        </VisualState>
    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>

<RelativePanel>
    <!--Previous 3 stack panels-->
</RelativePanel>
```

No es práctico crear estados visuales para una amplia variedad de tamaños de pantalla, ni hay más de un par de ellos que pueden tener un impacto significativo en la experiencia del usuario de la aplicación. Recomendamos diseñar en su lugar algunos puntos de interrupción clave; puedes [leer más aquí](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md).

## <a name="add-accessibility-support"></a>Añadir compatibilidad para accesibilidad

Ahora que ya tienes un diseño bien construido que responda a cambios en los tamaños de pantalla, una última cosa que puedes hacer para mejorar la experiencia del usuario es [hacer que tu aplicación esté accesible](../design/accessibility/accessibility-overview.md). Hay mucho que se puede incorporar, pero en un formato como este, es más fácil de lo que parece. Céntrate en lo siguiente:

* Compatibilidad con teclado: asegúrate de que el orden de los elementos de los paneles de la interfaz de usuario coincidan en la forma en que se muestran en pantalla, para que un usuario pueda fácilmente alternar entre ellos.
* Compatibilidad para lectores de pantalla: asegúrate de que todos los controles tengan un nombre descriptivo.

Cuando vayas a crear diseños más complejos con más elementos visuales, puedes consultar la [lista de comprobación de accesibilidad](../design/accessibility/accessibility-checklist.md) para obtener más detalles. Después de todo, aunque la accesibilidad no es necesaria para una aplicación, ayuda a llegar y a atraer a un público más amplio.

## <a name="going-further"></a>Ir más allá

Aunque hayas creado un formulario, los conceptos de diseños y controles son aplicables en todas las interfaces de usuario de XAML que puedas crear. No dude en volver a través de los documentos se haya vinculado a y experimentar con el formulario que tiene, agregar nuevas características de la interfaz de usuario y restringir aún más la experiencia del usuario. Si desea instrucciones paso a paso a través de características de diseño más detalladas, vea nuestro [tutorial sobre el diseño adaptable](../design/basics/xaml-basics-adaptive-layout.md)

Los formularios tampoco tienen que existir en vacío: podrías ir un paso más allá e incrustar los tuyos dentro de un [patrón principal/de detalles](../design/controls-and-patterns/master-details.md) o un [control dinámico](../design/controls-and-patterns/tabs-pivot.md). O si quieres ponerte a trabajar en el código subyacente de tu formulario, es posible que quieras empezar con nuestra [introducción a eventos](../xaml-platform/events-and-routed-events-overview.md).

## <a name="useful-apis-and-docs"></a>API y documentos de utilidad

Este es un resumen rápido de las API y otra documentación útiles que te ayudarán a comenzar a trabajar con enlace de datos.

### <a name="useful-apis"></a>API útiles

| API | Descripción |
|------|---------------|
| [Controles útiles para formularios](../design/controls-and-patterns/forms.md#input-controls) | Una lista de controles de entrada útiles para crear formularios y pautas básicas de dónde utilizarlos. |
| [Grid](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid) | Un panel para organizar elementos en diseños de varias filas y varias columnas. |
| [RelativePanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RelativePanel) | Un panel para organizar elementos en relación con otros elementos y los límites del panel. |
| [StackPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel) | Un panel para organizar elementos en una sola línea horizontal o vertical. |
| [VisualState](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualState) | Permite establecer el aspecto de los elementos de la interfaz de usuario cuando están en estados concretos. |

### <a name="useful-docs"></a>Documentos útiles

| Tema | Descripción |
|-------|----------------|
| [Información general sobre la accesibilidad](../design/accessibility/accessibility-overview.md) | Una introducción amplia de las opciones de accesibilidad en aplicaciones. |
| [Lista de comprobación de accesibilidad](../design/accessibility/accessibility-checklist.md) | Una práctica lista de comprobación para asegurarte de que la aplicación cumple con los estándares de accesibilidad. |
| [Introducción a los eventos](../xaml-platform/events-and-routed-events-overview.md) | Detalles sobre cómo agregar y estructurar eventos para controlar acciones de la interfaz de usuario. |
| [Formularios](../design/controls-and-patterns/forms.md) | Instrucciones generales para crear formularios. |
| [Paneles de diseño](../design/layout/layout-panels.md) | Proporciona una descripción general de los tipos de paneles de diseño y dónde usarlos. |
| [Patrón de maestro y detalles](../design/controls-and-patterns/master-details.md) | Un modelo de diseño que se puede implementar alrededor de uno o varios formularios. |
| [Control dinámico](../design/controls-and-patterns/tabs-pivot.md) | Un control que puede contener uno o varios formularios. |
| [Diseño adaptativo](../design/layout/responsive-design.md) | Una descripción general de los principios de diseño adaptativo a gran escala. | 
| [Diseños adaptativos con XAML](../design/layout/layouts-with-xaml.md) | Información específica sobre los estados visuales y otras implementaciones de diseño adaptativo. |
| [Tamaños de pantalla para diseño adaptativo](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md) | Instrucciones sobre qué ámbitos deben cubrirse para los tamaños de pantalla para diseños adaptativos. |

## <a name="useful-code-samples"></a>Muestras de código útiles

| Ejemplo de código | Descripción |
|-----------------|---------------|
| [Tutorial de diseño adaptativo](../design/basics/xaml-basics-adaptive-layout.md) | Una experiencia guiada paso a paso a través de los diseños adaptativos y el diseño con capacidad de respuesta. | 
| [Base de datos de pedidos de clientes](https://github.com/Microsoft/Windows-appsample-customers-orders-database) | Consulta el diseño y los formularios en acción en un ejemplo empresarial de varias páginas. |
| [Galería de controles XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) | Consulta una selección de controles XAML y cómo se implementan. |
| [Ejemplos de código adicionales](https://developer.microsoft.com//windows/samples) | Elige **Controles, diseño y texto** en la lista desplegable de categoría para ver ejemplos de código relacionados. |
