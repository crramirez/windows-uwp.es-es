---
title: Pista de aprendizaje - Construir y configurar un formulario
description: Aprenda lo que necesita hacer para crear un formulario sólido en su aplicación.
ms.date: 05/07/2018
ms.topic: article
keywords: introducción, uwp, windows 10, pista de aprendizaje, diseño, formulario
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 2e64392e1de5f6061b802acc0a2eed81c3e750fb
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/21/2019
ms.locfileid: "67318804"
---
# <a name="create-and-customize-a-form"></a>Crear y personalizar un formulario

Si está creando una aplicación que requiere que los usuarios ingresen una cantidad significativa de información, es probable que desee crear un formulario para que rellenen. En este artículo le mostrará lo que necesita saber para crear un formulario que sea útil y sólido.

Este no es un tutorial. Si desea uno, consulte nuestro [ tutorial de diseño adaptable ](../design/basics/xaml-basics-adaptive-layout.md), que le proporcionará una experiencia guiada paso a paso.

Analizaremos qué  **controles XAML** entran en un formulario, cómo organizarlos mejor en su página y cómo optimizar su formulario para cambiar el tamaño de la pantalla. Pero debido a que un formulario trata sobre la posición relativa de los elementos visuales, primero analicemos el diseño de la página con XAML.

## <a name="what-do-you-need-to-know"></a>¿Qué debes saber?

UWP no tiene un control de formulario explícito que puede agregar a su aplicación y configurar. En su lugar, tendrá que crear un formulario organizando una colección de elementos de interfaz de usuario en una página.

Para ello, tendrá que conocer los **paneles de diseño**. Estos son contenedores que contienen los elementos de la interfaz de usuario de su aplicación, lo que le permite organizarlos y agruparlos. La colocación de paneles de diseño dentro de otros paneles de diseño le da un gran control sobre dónde y cómo se muestran sus elementos entre sí. Esto también hace que sea mucho más fácil adaptar su aplicación a los cambiantes tamaños de pantalla.

Lea [ esta documentación sobre los paneles de diseño ](../design/layout/layout-panels.md). Debido a que los formularios generalmente se muestran en una o más columnas verticales, querrá agrupar elementos similares en un  **StackPanel**, y organizarlos dentro de un  **RelativePanel** si es necesario. Comience a armar algunos paneles ahora; si necesita una referencia, a continuación se muestra un marco de diseño básico para un formulario de dos columnas:

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

## <a name="what-goes-in-a-form"></a>¿Qué se incluye en un formulario?

Deberá rellenar su formulario con una variedad de [Controles de XAML](../design/controls-and-patterns/controls-and-events-intro.md). Probablemente esté familiarizado con eso, pero siéntase libre de leer si necesita un repaso. En concreto, querrá controles que le permitan al usuario escribir texto o elegir de una lista de valores. Esta es una lista básica de opciones que podría agregar: no necesita leer todo sobre ellas, solo lo suficiente para comprender su aspecto y funcionamiento.

* [TextBox](../design/controls-and-patterns/text-box.md) permite al usuario escribir texto en su aplicación.
* [ToggleSwitch](../design/controls-and-patterns/toggles.md) permite al usuario elegir entre dos opciones.
* [DatePicker](../design/controls-and-patterns/date-picker.md) permite al usuario seleccionar un valor de fecha.
* [TimePicker](../design/controls-and-patterns/time-picker.md) permite al usuario seleccionar un valor de hora.
* [ComboBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox) se expande para mostrar una lista de elementos seleccionables. Puede aprender más sobre ellos [aquí](../design/controls-and-patterns/lists.md#drop-down-lists)

También es posible que desee agregar [ botones ](../design/controls-and-patterns/buttons.md), para que el usuario pueda guardar o cancelar.

## <a name="format-controls-in-your-layout"></a>Controles de formato en el diseño

Sabe cómo organizar paneles de diseño y tiene los elementos que le gustaría agregar, pero ¿cómo se les da formato? La página [formularios](../design/controls-and-patterns/forms.md) ofrece instrucciones de diseño específicas. Lea las secciones **Tipos de formularios** y **diseño**  para obtener consejos útiles. Analizaremos la accesibilidad y el diseño correspondiente más brevemente.

Con ese consejo en mente, debe comenzar a agregar sus controles de elección a su diseño, asegurándose de que se les asignen las etiquetas y el espacio adecuado. A modo de ejemplo, aquí está el esquema básico para un formulario de una sola página utilizando el diseño, los controles y la guía de diseño anteriores:

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

Puede personalizar libremente sus controles con más propiedades para mejorar la experiencia visual.

## <a name="make-your-layout-responsive"></a>Haga su diseño dinámico

Los usuarios podrían ver su interfaz de usuario en una variedad de dispositivos con diferentes anchos de pantalla. Para asegurarse de que tengan una buena experiencia independientemente de su pantalla, debe usar [ diseño dinámico ](../design/layout/responsive-design.md). Lee esa página para obtener un buen asesoramiento sobre las filosofías de diseño que hay que tener en cuenta a medida que avanza.

La página [ Diseños dinámicos con XAML ](../design/layout/layouts-with-xaml.md) ofrece una descripción detallada de cómo implementar esto. Por ahora, nos centraremos en **diseños fluidos** y **estados visuales en XAML**.

El esquema del formulario básico que hemos reunido ya es un **diseño fluido**, ya que depende principalmente de la posición relativa de controles con únicamente el uso mínimo de posiciones y tamaños de píxel específicos. Sin embargo, tenga en cuenta estas instrucciones para más interfaces de usuario que pueda crear en el futuro.

Algo más importante para diseños dinámicos son los **estados visuales.** Un estado visual define valores de propiedad que se aplican a un elemento dado cuando una condición dada es verdadera. [Lee cómo hacer esto en xaml](../design/layout/layouts-with-xaml.md#set-visual-states-in-xaml-markup)y luego impleméntelo en su formulario. Esto es lo que podría parecer un *muy* básico en nuestra muestra anterior:

```xaml
<Page ...>
    <Grid>
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
            <!-- Customer StackPanel -->
            <!-- Associate StackPanel -->
            <!-- Save StackPanel -->
        </RelativePanel>
    </Grid>
</Page>
```

> [!IMPORTANT]
> Cuando use StateTriggers, siempre asegúrese de que VisualStateGroups se adjunte al menor elemento secundario de la raíz. Aquí, **Grid** es el primer elemento secundario del elemento **Page** raíz.

No es práctico crear estados visuales para una amplia variedad de tamaños de pantalla, ni hay más de un par de ellos que pueden tener un impacto significativo en la experiencia del usuario de su aplicación. Recomendamos diseñar en su lugar algunos puntos de interrupción clave; puede [leer más aquí](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md).

## <a name="add-accessibility-support"></a>Agregar compatibilidad para accesibilidad

Ahora que ya tiene un diseño bien construido que responde a cambios en los tamaños de pantalla, una última cosa que puede hacer para mejorar la experiencia del usuario es [hacer que su aplicación esté accesible](../design/accessibility/accessibility-overview.md). Hay muchas cosas que pueden incluirse en esto, pero en un formulario como este es más fácil de lo que parece. Céntrese en lo siguiente:

* Compatibilidad con el teclado: asegúrese de que el orden de los elementos de sus paneles de la interfaz de usuario coincidan en la forma en que se muestran en pantalla, de modo que el usuario pueda verlos fácilmente.
* Compatibilidad con lector de pantalla: asegúrese de que todos sus controles tengan un nombre descriptivo.

Cuando cree diseños más complejos con más elementos visuales, querrá consultar la [ lista de comprobación de accesibilidad ](../design/accessibility/accessibility-checklist.md) para obtener más detalles. Después de todo, si bien la accesibilidad no es necesaria para una aplicación, ayuda a que llegue a una audiencia más amplia.

## <a name="going-further"></a>Ir más allá

Aunque ha creado un formulario aquí, los conceptos de diseño y control son aplicables en todas las interfaces de usuario de XAML que pueda crear. Siéntase libre de volver a los documentos a los que lo hemos enlazado y experimente con el formulario que tiene, agregando nuevas funciones de interfaz de usuario y refinando aún más la experiencia del usuario. Si desea una guía paso a paso a través de funciones de diseño más detalladas, consulte nuestro [ tutorial de diseño adaptable ](../design/basics/xaml-basics-adaptive-layout.md)

Los formularios tampoco tienen que existir en un vacío: puede ir un paso más allá e insertar los suyos dentro de un [trama principal/de detalles](../design/controls-and-patterns/master-details.md) o un [control dinámico](../design/controls-and-patterns/pivot.md). O si desea comenzar a trabajar en el código subyacente para su formulario, puede comenzar con nuestra [introducción de eventos ](../xaml-platform/events-and-routed-events-overview.md).

## <a name="useful-apis-and-docs"></a>API y documentos útiles

Este es un resumen rápido de las API y otra documentación útiles que te ayudarán a comenzar a trabajar con los enlaces de datos.

### <a name="useful-apis"></a>API útiles

| API | Descripción |
|------|---------------|
| [Controles útiles para formularios](../design/controls-and-patterns/forms.md#input-controls) | Una lista de controles de entrada útiles para crear formularios, e instrucciones básicas de dónde usarlos. |
| [Grid](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid) | Un panel para organizar elementos en diseños de varias filas y varias columnas. |
| [RelativePanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RelativePanel) | Un panel para organizar elementos en relación con otros elementos y los límites del panel. |
| [StackPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel) | Un panel para organizar elementos en una sola línea horizontal o vertical. |
| [VisualState](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualState) | Permite establecer el aspecto de los elementos de la interfaz de usuario cuando están en estados concretos. |

### <a name="useful-docs"></a>Documentos útiles

| Tema | Descripción |
|-------|----------------|
| [Información general sobre accesibilidad](../design/accessibility/accessibility-overview.md) | Una visión general a gran escala de las opciones de accesibilidad en las aplicaciones. |
| [Lista de comprobación de accesibilidad](../design/accessibility/accessibility-checklist.md) | Una lista de verificación práctica para asegurar que su aplicación cumpla con los estándares de accesibilidad. |
| [Introducción a los eventos](../xaml-platform/events-and-routed-events-overview.md) | Detalles sobre cómo agregar y estructurar eventos para controlar acciones de la interfaz de usuario. |
| [Formularios](../design/controls-and-patterns/forms.md) | Instrucciones generales para crear formularios. |
| [Paneles de diseño](../design/layout/layout-panels.md) | Proporciona una introducción de los tipos de paneles de diseño y dónde usarlos. |
| [Trama master y detalles](../design/controls-and-patterns/master-details.md) | Un modelo de trama que se puede implementar alrededor de uno o varios formularios. |
| [Control Pivot](../design/controls-and-patterns/pivot.md) | Un control que puede contener uno o varios formularios. |
| [Diseño dinámico](../design/layout/responsive-design.md) | Una introducción de los principios de diseño adaptativo a gran escala. |
| [Diseños dinámicos con XAML](../design/layout/layouts-with-xaml.md) | Información específica sobre los estados visuales y otras implementaciones de diseño dinámico. |
| [Tamaños de pantalla para diseño dinámico](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md) | Instrucciones sobre los tamaños de pantalla a los que deben aplicarse los diseños de respuesta. |

## <a name="useful-code-samples"></a>Ejemplos de código útiles

| Ejemplo de código | Descripción |
|-----------------|---------------|
| [Tutorial de diseño adaptativo](../design/basics/xaml-basics-adaptive-layout.md) | Una experiencia guiada paso a paso a través de diseños adaptativos y diseño dinámico. |
| [Base de datos de pedidos de clientes](https://github.com/Microsoft/Windows-appsample-customers-orders-database) | Consulte el diseño y los formularios en acción en un ejemplo empresarial de varias páginas. |
| [XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery) | Consulte una selección de controles XAML y cómo se implementan. |
| [Ejemplos de código adicionales](https://developer.microsoft.com/windows/samples) | Elija **Controles, diseño y texto** en la lista desplegable de categorías para ver ejemplos de códigos relacionados. |
