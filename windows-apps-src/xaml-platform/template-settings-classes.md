---
description: Clases de configuración de plantillas
title: Clases de configuración de plantillas
ms.assetid: CAE933C6-EF13-465A-9831-AB003AF23907
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b0d68b31a1574d23dd66de37e2f708d8d77fc052
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371119"
---
# <a name="template-settings-classes"></a>Clases de configuración de plantillas


## <a name="prerequisites"></a>Requisitos previos

Damos por hecho que sabes agregar controles a la interfaz de usuario, establecer sus propiedades y adjuntar controladores de eventos. Para obtener instrucciones para agregar controles a la aplicación, consulta [Agregar controles y administrar eventos](https://docs.microsoft.com/windows/uwp/controls-and-patterns/controls-and-events-intro). También damos por hecho que conoces los conceptos básicos de cómo definir una plantilla personalizada para un control al realizar una copia de la plantilla predeterminada y modificarla. Para obtener más información al respecto, consulte [inicio rápido: Plantillas de control](https://docs.microsoft.com/previous-versions/windows/apps/hh465374(v=win.10)).

## <a name="the-scenario-for-templatesettings-classes"></a>Escenario para las clases **TemplateSettings**

Las clases **TemplateSettings** proporcionan un conjunto de propiedades que se usan al definir una nueva plantilla de control para un control. Las propiedades tienen valores como medidas de píxel para el tamaño de determinadas partes de los elementos de la interfaz de usuario. A veces, los valores son valores calculados que provienen de la lógica de control que normalmente no es de fácil reemplazo ni acceso. Algunas de las propiedades están pensadas como valores **De origen** y **De destino** que controlan las transiciones y animaciones de los elementos y, por ende, las propiedades **TemplateSettings** correspondientes vienen en pares.

Existen varias clases **TemplateSettings**. Todas ellas están en el espacio de nombres [**Windows.UI.Xaml.Controls.Primitives**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives). A continuación, se ofrece una lista de las clases y un vínculo a la propiedad **TemplateSettings** del control correspondiente. Esta propiedad **TemplateSettings** define el modo de acceso a los valores **TemplateSettings** para el control y puede establecer enlaces de plantilla a sus propiedades:

-   [**ComboBoxTemplateSettings**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ComboBoxTemplateSettings): valor de [ **ComboBox.TemplateSettings**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.combobox.templatesettings)
-   [**GridViewItemTemplateSettings**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.GridViewItemTemplateSettings): valor de [ **GridViewItem.TemplateSettings**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.gridviewitem.templatesettings)
-   [**ListViewItemTemplateSettings**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ListViewItemTemplateSettings): valor de [ **ListViewItem.TemplateSettings**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewitem.templatesettings)
-   [**ProgressBarTemplateSettings**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ProgressBarTemplateSettings): valor de [ **ProgressBar.TemplateSettings**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.progressbar.templatesettings)
-   [**ProgressRingTemplateSettings**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ProgressRingTemplateSettings): valor de [ **ProgressRing.TemplateSettings**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.progressring.templatesettings)
-   [**SettingsFlyoutTemplateSettings**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.SettingsFlyoutTemplateSettings): valor de [ **SettingsFlyout.TemplateSettings**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.settingsflyout.templatesettings)
-   [**ToggleSwitchTemplateSettings**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleSwitchTemplateSettings): valor de [ **ToggleSwitch.TemplateSettings**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.templatesettings)
-   [**ToolTipTemplateSettings**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToolTipTemplateSettings): valor de [ **ToolTip.TemplateSettings**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.tooltip.templatesettings)

Las propiedades **TemplateSettings** están pensadas para usarlas siempre en XAML, no para el código. Se trata de subpropiedades de solo lectura de una propiedad **TemplateSettings** de solo lectura de un control principal. Para un escenario avanzado de control personalizado, en el que creas una nueva clase basada en [**Control**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control) que, por consiguiente, puede influir en la lógica de control, considera la posibilidad de definir una propiedad **TemplateSettings** personalizada en el control para comunicar información que podría resultar útil a cualquier persona que esté volviendo a generar plantillas del control. Dado que esa propiedad es un valor de solo lectura, define una nueva clase **TemplateSettings** relacionada con tu control que tiene propiedades de solo lectura para cada uno de los elementos de información que son relevantes para las medidas de plantilla, posicionamiento de animación, etc. y asigna a los llamadores la instancia en tiempo de ejecución de la clase que se inicializa con la lógica de control. Las clases **TemplateSettings** se derivan de [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject), de modo que las propiedades pueden usar el sistema de propiedades de dependencia de devoluciones de llamada de cambio de propiedad. Pero los identificadores de propiedades de dependencia para las propiedades no se exponen como API pública, porque las propiedades **TemplateSettings** deben ser de solo lectura para los llamadores.

## <a name="how-to-use-templatesettings-in-a-control-template"></a>Cómo usar **TemplateSettings** en una plantilla de control

Este es un ejemplo proveniente de las plantillas predeterminadas iniciales de control XAML. Este ejemplo en particular proviene de la plantilla predeterminada de [**ProgressRing**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressRing):

```xml
<Ellipse
    x:Name="E1"
    Style="{StaticResource ProgressRingEllipseStyle}"
    Width="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseDiameter}"
    Height="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseDiameter}"
    Margin="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseOffset}"
    Fill="{TemplateBinding Foreground}"/>
```

El código XAML completo para la plantilla [**ProgressRing**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressRing) contiene cientos de líneas, por lo que aquí se muestra un pequeño fragmento. Este código XAML define un elemento de control que es uno de los seis elementos [**Elipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) que revelan la animación de giro de progreso indeterminado. Como desarrollador, quizá no quieras los círculos y podrías usar otra primitiva gráfica u otra una forma básica para el modo de avance de la animación. Por ejemplo, puedes componer un **ProgressRing** que use un conjunto de elementos [**Rectángulo**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) organizados en un cuadrado. Si así fuera, cada componente de **Rectángulo** de la nueva plantilla podrían tener el aspecto siguiente:

```xml
<Rectangle
    x:Name="R1"
    Width="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseDiameter}"
    Height="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseDiameter}"
    Margin="{Binding RelativeSource={RelativeSource TemplatedParent}, 
        Path=TemplateSettings.EllipseOffset}"
    Fill="{TemplateBinding Foreground}"/>
```

El motivo por el cual las propiedades **TemplateSettings** son útiles aquí es porque son valores calculados procedentes de la lógica de control básico de [**ProgressRing**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressRing). El cálculo se realiza al dividir los valores [**ActualWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualwidth) y [**ActualHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.actualheight) generales de **ProgressRing** y al asignar una medida calculada a cada uno de los elementos de movimiento en sus plantillas para que los elementos de plantilla puedan cambiar el tamaño según el contenido.

Este es otro ejemplo de uso de plantillas predeterminadas de control XAML. Esta vez el ejemplo muestra uno de los conjuntos de propiedades que corresponden a los valores **De origen** y **De destino** de una animación. Se trata de la plantilla predeterminada [**ComboBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox):

```xml
<VisualStateGroup x:Name="DropDownStates">
    <VisualState x:Name="Opened">
        <Storyboard>
            <SplitOpenThemeAnimation
               OpenedTargetName="PopupBorder"
               ContentTargetName="ScrollViewer"
               ClosedTargetName="ContentPresenter"
               ContentTranslationOffset="0"
               OffsetFromCenter="{Binding RelativeSource={RelativeSource TemplatedParent}, 
                 Path=TemplateSettings.DropDownOffset}"
               OpenedLength="{Binding RelativeSource={RelativeSource TemplatedParent}, 
                 Path=TemplateSettings.DropDownOpenedHeight}"
               ClosedLength="{Binding RelativeSource={RelativeSource TemplatedParent},
                 Path=TemplateSettings.DropDownClosedHeight}" />
        </Storyboard>
   </VisualState>
...
</VisualStateGroup>
```

Una vez más, hay gran cantidad de XAML en la plantilla, por lo que solo mostramos un fragmento. Además, este es solo uno de los diversos estados y animaciones de tema que usan las mismas propiedades [**ComboBoxTemplateSettings**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ComboBoxTemplateSettings). En el caso de [**ComboBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox), el uso de los valores **ComboBoxTemplateSettings** mediante enlaces exige que las animaciones relacionadas de la plantilla se detengan y se inicien en posiciones basadas en valores compartidos y, por consiguiente, la transición se realiza sin problemas.

**Tenga en cuenta**    cuando usas **TemplateSettings** valores como parte de la plantilla de control, asegúrese de que está configurando las propiedades que coinciden con el tipo del valor. De lo contrario, tendrías que crear un convertidor de valores para el enlace para poder convertir el tipo de destino del enlace a partir de un tipo de origen diferente del valor **TemplateSettings**. Para obtener más información, consulta [**IValueConverter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.IValueConverter).

## <a name="related-topics"></a>Temas relacionados

* [Inicio rápido: Plantillas de control](https://docs.microsoft.com/previous-versions/windows/apps/hh465374(v=win.10))

