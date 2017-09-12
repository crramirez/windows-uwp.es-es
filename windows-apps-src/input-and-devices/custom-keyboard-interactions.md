---
author: Karl-Bridge-Microsoft
pm-contact: miguelrb
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.openlocfilehash: 010663320b4011d853c53bc4121f86a14bfbfe0c
ms.sourcegitcommit: a2908889b3566882c7494dc81fa9ece7d1d19580
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2017
---
# <a name="custom-keyboard-interactions"></a>Interacciones de teclado personalizadas

Proporciona experiencias de interacción de teclado integrales y coherentes en tus aplicaciones para UWP y controles personalizados para usuarios avanzados de teclado y para aquellos que tienen discapacidades y otros requisitos de accesibilidad.

En este tema, nuestro objetivo principal trata sobre cómo admitir la entrada de teclado con controles personalizados para aplicaciones para UWP en equipos. Sin embargo, una experiencia de teclado bien diseñada es importante para admitir las herramientas de accesibilidad, como el Narrador de Windows, con teclados de software, como el teclado táctil y el teclado en pantalla.

## <a name="providing-2d-directional-inner-navigation-a-namexyfocuskeyboardnavigation"></a>Proporcionar una navegación interna direccional 2D <a name="xyfocuskeyboardnavigation">

Usa la propiedad **XYFocusKeyboardNavigation** para admitir la navegación interna direccional 2D de controles personalizados y de grupos de controles con el teclado (teclas de dirección), el controlador para juegos de Xbox (la cruceta y los botones del stick izquierdo) y el control remoto de Xbox (la cruceta).

**NOTA** A la región de navegación interna de un control o un grupo de controles la llamamos el *área direccional*.

**XYFocusKeyboardNavigation** tiene un valor de tipo **XYFocusKeyboardNavigationMode** con valores posibles de **Auto** (predeterminado), **Enabled** o **Disabled**.

Esta propiedad no afecta a la navegación mediante tabulación, solo afecta a la navegación interna de los elementos secundarios dentro del control o del grupo de controles. Los elementos secundarios de un área direccional no deben incluirse en la navegación mediante tabulación.

### <a name="default-behavior"></a>Comportamiento predeterminado

El comportamiento de navegación direccional se basa en la ascendencia del elemento o la jerarquía de herencia. Si todos los antecesores se encuentran en el modo predeterminado o están establecidos en **Auto**, no se admite el comportamiento de navegación direccional para teclado (el controlador para juegos y el control remoto admiten siempre la navegación direccional, a menos que se establezcan explícitamente en **Disabled**).

### <a name="custom-behavior"></a>Comportamiento personalizado

Establecer esta propiedad en **Enabled** te permite controlar la navegación interna 2D (con las teclas de dirección del teclado) por cada UIElement que controles.

Al usar las teclas de dirección del teclado, la navegación queda restringida dentro del área direccional (al presionar la tecla TAB, el foco se establece en el siguiente elemento activable fuera del área direccional).

**NOTA** Esto no es así cuando se usa un controlador para juegos o control remoto, donde la navegación continúa fuera del área direccional al siguiente control activable.

Esta propiedad afecta a la navegación interna únicamente con las teclas de dirección, no afecta a la navegación mediante la tecla TAB. Todos los controles mantienen su jerarquía de orden de tabulación esperada.

La siguiente imagen muestra tres botones (A, B y C) dentro de un área direccional y un cuarto botón (D) fuera del área direccional.

![área direccional](images/keyboard/directional-area.png)

*Las teclas de dirección del teclado pueden mover el foco entre los botones A, B y C, pero no D*

El siguiente ejemplo de código muestra cómo se ve afectada la navegación cuando **XYFocusKeyboardNavigation** está habilitado. Con la imagen anterior, A tiene el foco inicial y la tecla tab recorre cíclicamente todos los controles (A -&gt; B -&gt; C -&gt; D y vuelve a A) mientras que las teclas de dirección están restringidas al área direccional.

```XAML
<StackPanel Orientation="Horizontal">
      <StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation="Enabled">
            <Button Content="A" />
            <Button Content="B" />
            <Button Content="C" />
      </StackPanel>
      <Button Content="D" />
</StackPanel>
```

#### <a name="override-directional-navigation"></a>Invalidar la navegación direccional

Usa las propiedades XYFocusRight/XYFocusLeft/XYFocusTop/XYFocusDown para invalidar los comportamientos de navegación predeterminados.

Aquí se muestra la misma imagen que el ejemplo anterior, donde aparecen tres botones (A, B y C) dentro de un área direccional y un cuarto botón (D) fuera del área direccional.

![área direccional](images/keyboard/directional-area.png)

*Las teclas de dirección del teclado pueden mover el foco entre los botones A, B, C y fuera hasta D*

Este ejemplo de código muestra cómo invalidar el comportamiento de navegación predeterminado para la tecla de flecha derecha, al permitir que navegue a un control fuera del área direccional. Ten en cuenta que no es posible volver a entrar en el área direccional usando la tecla de dirección izquierda.

```XAML
<StackPanel Orientation="Horizontal">
      <StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation="Enabled">
            <Button Content="A" />
            <Button Content="B" />
            <Button Content="C" XYFocusRight="{x:Bind ButtonD}" />
      </StackPanel>
      <Button Content="D" x:Name="ButtonD"/>
</StackPanel>
```

Para obtener más información, consulta [Estrategias de navegación XYFocus](#set-the-tab-navigation-behavior) más adelante en este tema.

#### <a name="restrict-navigation-with-disabled"></a>Restringir la navegación con Disabled

Establece **XYFocusKeyboardNavigation** en **Disabled** para restringir la navegación mediante teclas de dirección dentro de un área direccional.

**NOTA** Establecer esta propiedad en Disabled no afecta a la navegación con teclado para el control en sí, solo a los elementos secundarios del control.

En el siguiente ejemplo de código, el elemento primario StackPanel tiene **XYFocusKeyboardNavigation** establecido en **Enabled** y el elemento secundario, C, tiene **XYFocusKeyboardNavigation** establecido en **Disabled**. Solo los elementos secundarios de C tienen deshabilitada la navegación mediante teclas de dirección.

```XAML
<StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation="Enabled">
        <Button Content="A" />
        <Button Content="B" />
        <Button Content="C" XYFocusKeyboardNavigation="Disabled" >
            ...
        </Button>
</StackPanel>
```

#### <a name="use-nested-directional-areas"></a>Usar áreas direccionales anidadas

Puedes tener varios niveles de áreas direccionales anidadas. Si todos los elementos primarios tienen **XYFocusKeyboardNavigation** establecido en **Enabled**, la navegación mediante teclas de dirección omite los límites de la región.

Aquí hay una imagen que muestra tres botones (A, B y C) dentro de un área direccional anidada y un cuarto botón (D) fuera del área direccional.

![área direccional anidada](images/keyboard/nested-directional-area.png)

*Las teclas de dirección del teclado pueden mover el foco entre los botones A, B y C, pero no D*

En este ejemplo de código se muestra cómo especificar que las áreas direccionales anidadas admitan la navegación mediante teclas de dirección más allá de los límites de la región.

```XAML
<StackPanel Orientation="Horizontal">
        <StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation ="Enabled">
            <StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation ="Enabled">
                <Button Content="A" />
                <Button Content="B" />
            </StackPanel>
            <Button Content="C" />
        </StackPanel>
        <Button Content="D" />
 </StackPanel>
```

He aquí un imagen que muestra cuatro botones (A, B, C y D), donde A y B se encuentran dentro de un área direccional anidada, y C y D se encuentran dentro de otra área. Dado que el elemento primario tiene **XYFocusKeyboardNavigation** establecido en **Disabled**, no es posible cruzar los límites de cada área anidada con las teclas de dirección.

![área no direccional](images/keyboard/non-directional-area.png)

*Las teclas de dirección del teclado pueden mover el foco entre los botones A y B y entre los botones C y D, pero no entre las regiones*

En este ejemplo de código se muestra cómo especificar áreas direccionales anidadas que no admitan la navegación mediante teclas de dirección más allá de los límites de la región.

```XAML
<StackPanel Orientation="Horizontal">
  <StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation="Enabled">
    <Button Content="A" />
    <Button Content="B" />
  </StackPanel>
  <StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation="Enabled">
    <Button Content="C" />
    <Button Content="D" />
  </StackPanel>
</StackPanel>
```

Este es un ejemplo más complejo de áreas direccionales anidadas donde:

-   Si A tiene el foco, solo se puede navegar a E (y viceversa) porque hay un límite de área no direccional que hace que no se pueda acceder a B, C y D con las teclas de dirección.
-   Si B tiene el foco, solo se puede navegar a C (y viceversa) porque D está fuera del área direccional y el límite de área no direccional bloquea el acceso a A y E.
-   Si D tiene el foco, debe usarse la tecla Tab para navegar entre los controles, ya que la navegación mediante las teclas de dirección no es posible.

![área no direccional anidada](images/keyboard/nested-non-directional-area.png)

*Las teclas de dirección del teclado pueden mover el foco entre los botones A y E y entre los botones B y C, pero no entre otras regiones*

En este ejemplo de código se muestra cómo especificar áreas direccionales anidadas que admiten la navegación compleja mediante teclas de dirección más allá de los límites de la región.

```XAML
<StackPanel  Orientation="Horizontal" XYFocusKeyboardNavigation ="Enabled">
  <Button Content="A" />
    <StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation ="Disabled">
      <StackPanel Orientation="Horizontal" XYFocusKeyboardNavigation ="Enabled">
        <Button Content="B" />
        <Button Content="C" />
      </StackPanel>
        <Button Content="D" />
    </StackPanel>
  <Button Content="E" />
</StackPanel>
```

## <a name="set-the-tab-navigation-behavior-a-nametab-navigation"></a>Establecer el comportamiento de navegación mediante tabulación <a name="tab-navigation">

La propiedad UIElement.[TabFocusNavigation](http://msdn.microsoft.com/en-us/library/windows/apps/xaml/Windows.UI.Xaml.Controls.Control.TabNavigation) especifica el comportamiento de navegación mediante tabulación de todo su árbol de objetos (o área direccional).

[TabFocusNavigation](http://msdn.microsoft.com/en-us/library/windows/apps/xaml/Windows.UI.Xaml.Controls.Control.TabNavigation) tiene un valor de tipo **TabFocusNavigationMode** con valores posibles de **Once**, **Cycle**, o **Local** (predeterminado).

En la siguiente imagen, dependiendo de la navegación mediante tabulación del área direccional, el foco se mueve de las siguientes maneras:

-   Once: A, B1, C, A
-   Local: A, B1, B2, B3, B4, B5, C, A
-   Cycle: A, B1, B2, B3, B4, B5, B1, B2, B3, B4, B5, (bucle en B)

![navegación mediante tabulación](images/keyboard/tab-navigation.png)

*Comportamiento del foco basado en el modo de navegación mediante tabulación*

En el siguiente ejemplo de código se muestra el uso de TabFocusNavigation con un modo de Once.

```XAML
<Button Content="X" Click="OnAClick"/>
<StackPanel Orientation="Horizontal" XYFocusNavigation ="Local"
   TabFocusNavigation ="Local">
   <Button Content="A" Click="OnAClick"/>
   <StackPanel Orientation="Horizontal" TabFocusNavigation ="Once">
        <Button Content="B" Click="OnBClick"/>
        <Button Content="C" Click="OnCClick"/>
        <Button Content="D" Click="OnDClick"/>
   </StackPanel>
   <Button Content="E" Click="OnBClick"/>
</StackPanel>
```

*La navegación mediante tabulación cuando el foco está en X es: A, B, E, X*

#### <a name="about-tabfocusnavigation-and-tabindex"></a>Acerca de TabFocusNavigation y TabIndex

La propiedad UIElement.TabFocusNavigation tiene el mismo comportamiento que la propiedad Control.TabNavigation, incluso cómo funciona con TabIndex.

Cuando un control no tiene especificado ningún atributo TabIndex, el marco le asigna un valor de índice mayor que el valor de índice mayor actual (y la prioridad más baja). Para resolver la ambigüedad, selecciona el primer elemento en el árbol visual. El marco resuelve los índices de tabulación por ámbito. Los elementos secundarios de un control se consideran un ámbito y, si uno de estos elementos secundarios tiene elementos secundarios, estos son parte de otro ámbito.

En la siguiente imagen, dependiendo de la navegación mediante tabulación del área direccional y el índice de tabulación de los elementos, el foco se mueve de las siguientes maneras:

-   Once: A, B3, C, A.
-   Local: A, B3, B4, B5, B1, B2, C, A.
-   Cycle: A, B3, B4, B5, B1, B2, B3, B4, B5, B1, B2, (bucle en B)

![índice de tabulación](images/keyboard/tab-index.png)

*Comportamiento del foco basado en el modo de navegación mediante tabulación y los índices de tabulación*

Ten en cuenta la manera en que el área direccional se considera un ámbito y cómo la navegación por el foco se mueve al control con la prioridad más alta en primer lugar: B3. De hecho, hay dos ámbitos: uno para A, área direccional y C; y otro para el área direccional. Dado que el área direccional no es una propiedad TabStop, el marco cambia el ámbito para buscar el mejor candidato y luego a través de los elementos secundarios de forma recursiva.
