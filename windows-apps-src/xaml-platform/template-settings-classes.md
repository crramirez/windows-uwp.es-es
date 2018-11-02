---
author: jwmsft
description: Clases de configuración de plantillas
title: Clases de configuración de plantillas
ms.assetid: CAE933C6-EF13-465A-9831-AB003AF23907
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4d7b08138ab22d4cf2cbf4fb5273759f000a7c94
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2018
ms.locfileid: "5968835"
---
# <a name="template-settings-classes"></a>Clases de configuración de plantillas


## <a name="prerequisites"></a>Requisitos previos

Damos por hecho que sabes agregar controles a la interfaz de usuario, establecer sus propiedades y adjuntar controladores de eventos. Para obtener instrucciones para agregar controles a la aplicación, consulta [Agregar controles y administrar eventos](https://msdn.microsoft.com/library/windows/apps/mt228345). También damos por hecho que conoces los conceptos básicos de cómo definir una plantilla personalizada para un control al realizar una copia de la plantilla predeterminada y modificarla. Para obtener más información al respecto, consulta [Inicio rápido: plantillas de control](https://msdn.microsoft.com/library/windows/apps/xaml/hh465374).

## <a name="the-scenario-for-templatesettings-classes"></a>Escenario para las clases **TemplateSettings**

Las clases **TemplateSettings** proporcionan un conjunto de propiedades que se usan al definir una nueva plantilla de control para un control. Las propiedades tienen valores como medidas de píxel para el tamaño de determinadas partes de los elementos de la interfaz de usuario. A veces, los valores son valores calculados que provienen de la lógica de control que normalmente no es de fácil reemplazo ni acceso. Algunas de las propiedades están pensadas como valores **De origen** y **De destino** que controlan las transiciones y animaciones de los elementos y, por ende, las propiedades **TemplateSettings** correspondientes vienen en pares.

Existen varias clases **TemplateSettings**. Todas ellas están en el espacio de nombres [**Windows.UI.Xaml.Controls.Primitives**](https://msdn.microsoft.com/library/windows/apps/br209818). A continuación, se ofrece una lista de las clases y un vínculo a la propiedad **TemplateSettings** del control correspondiente. Esta propiedad **TemplateSettings** define el modo de acceso a los valores **TemplateSettings** para el control y puede establecer enlaces de plantilla a sus propiedades:

-   [**ComboBoxTemplateSettings**](https://msdn.microsoft.com/library/windows/apps/br227752): valor de [**ComboBox.TemplateSettings**](https://msdn.microsoft.com/library/windows/apps/br209364)
-   [**GridViewItemTemplateSettings**](https://msdn.microsoft.com/library/windows/apps/hh738499): valor de [**GridViewItem.TemplateSettings**](https://msdn.microsoft.com/library/windows/apps/hh738503)
-   [**ListViewItemTemplateSettings**](https://msdn.microsoft.com/library/windows/apps/hh701948): valor de [**ListViewItem.TemplateSettings**](https://msdn.microsoft.com/library/windows/apps/br242923)
-   [**ProgressBarTemplateSettings**](https://msdn.microsoft.com/library/windows/apps/br227856): valor de [**ProgressBar.TemplateSettings**](https://msdn.microsoft.com/library/windows/apps/br227537)
-   [**ProgressRingTemplateSettings**](https://msdn.microsoft.com/library/windows/apps/hh702248): valor de [**ProgressRing.TemplateSettings**](https://msdn.microsoft.com/library/windows/apps/hh702581)
-   [**SettingsFlyoutTemplateSettings**](https://msdn.microsoft.com/library/windows/apps/dn298721): valor de [**SettingsFlyout.TemplateSettings**](https://msdn.microsoft.com/library/windows/apps/dn252826)
-   [**ToggleSwitchTemplateSettings**](https://msdn.microsoft.com/library/windows/apps/br209804): valor de [**ToggleSwitch.TemplateSettings**](https://msdn.microsoft.com/library/windows/apps/br209731)
-   [**ToolTipTemplateSettings**](https://msdn.microsoft.com/library/windows/apps/br209813): valor de [**ToolTip.TemplateSettings**](https://msdn.microsoft.com/library/windows/apps/br227629)

Las propiedades **TemplateSettings** están pensadas para usarlas siempre en XAML, no para el código. Se trata de subpropiedades de solo lectura de una propiedad **TemplateSettings** de solo lectura de un control principal. Para un escenario avanzado de control personalizado, en el que creas una nueva clase basada en [**Control**](https://msdn.microsoft.com/library/windows/apps/br209390) que, por consiguiente, puede influir en la lógica de control, considera la posibilidad de definir una propiedad **TemplateSettings** personalizada en el control para comunicar información que podría resultar útil a cualquier persona que esté volviendo a generar plantillas del control. Dado que esa propiedad es un valor de solo lectura, define una nueva clase **TemplateSettings** relacionada con tu control que tiene propiedades de solo lectura para cada uno de los elementos de información que son relevantes para las medidas de plantilla, posicionamiento de animación, etc. y asigna a los llamadores la instancia en tiempo de ejecución de la clase que se inicializa con la lógica de control. Las clases **TemplateSettings** se derivan de [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/br242356), de modo que las propiedades pueden usar el sistema de propiedades de dependencia de devoluciones de llamada de cambio de propiedad. Pero los identificadores de propiedades de dependencia para las propiedades no se exponen como API pública, porque las propiedades **TemplateSettings** deben ser de solo lectura para los llamadores.

## <a name="how-to-use-templatesettings-in-a-control-template"></a>Cómo usar **TemplateSettings** en una plantilla de control

Este es un ejemplo proveniente de las plantillas predeterminadas iniciales de control XAML. Este ejemplo en particular proviene de la plantilla predeterminada de [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/br227538):

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

El código XAML completo para la plantilla [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/br227538) contiene cientos de líneas, por lo que aquí se muestra un pequeño fragmento. Este código XAML define un elemento de control que es uno de los seis elementos [**Elipse**](/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) que revelan la animación de giro de progreso indeterminado. Como desarrollador, quizá no quieras los círculos y podrías usar otra primitiva gráfica u otra una forma básica para el modo de avance de la animación. Por ejemplo, puedes componer un **ProgressRing** que use un conjunto de elementos [**Rectángulo**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) organizados en un cuadrado. Si así fuera, cada componente de **Rectángulo** de la nueva plantilla podrían tener el aspecto siguiente:

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

El motivo por el cual las propiedades **TemplateSettings** son útiles aquí es porque son valores calculados procedentes de la lógica de control básico de [**ProgressRing**](https://msdn.microsoft.com/library/windows/apps/br227538). El cálculo se realiza al dividir los valores [**ActualWidth**](https://msdn.microsoft.com/library/windows/apps/br208709) y [**ActualHeight**](https://msdn.microsoft.com/library/windows/apps/br208707) generales de **ProgressRing** y al asignar una medida calculada a cada uno de los elementos de movimiento en sus plantillas para que los elementos de plantilla puedan cambiar el tamaño según el contenido.

Este es otro ejemplo de uso de plantillas predeterminadas de control XAML. Esta vez el ejemplo muestra uno de los conjuntos de propiedades que corresponden a los valores **De origen** y **De destino** de una animación. Se trata de la plantilla predeterminada [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/br209348):

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

Una vez más, hay gran cantidad de XAML en la plantilla, por lo que solo mostramos un fragmento. Además, este es solo uno de los diversos estados y animaciones de tema que usan las mismas propiedades [**ComboBoxTemplateSettings**](https://msdn.microsoft.com/library/windows/apps/br227752). En el caso de [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/br209348), el uso de los valores **ComboBoxTemplateSettings** mediante enlaces exige que las animaciones relacionadas de la plantilla se detengan y se inicien en posiciones basadas en valores compartidos y, por consiguiente, la transición se realiza sin problemas.

**Nota**  al usar los valores de **TemplateSettings** como parte de la plantilla de control, asegúrate de que establecer propiedades que coincidan con el tipo de valor. De lo contrario, tendrías que crear un convertidor de valores para el enlace para poder convertir el tipo de destino del enlace a partir de un tipo de origen diferente del valor **TemplateSettings**. Para obtener más información, consulta [**IValueConverter**](https://msdn.microsoft.com/library/windows/apps/br209903).

## <a name="related-topics"></a>Temas relacionados

* [Inicio rápido: Plantillas de control](https://msdn.microsoft.com/library/windows/apps/xaml/hh465374)

