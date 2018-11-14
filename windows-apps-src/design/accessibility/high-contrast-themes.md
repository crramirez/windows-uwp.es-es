---
author: Xansky
description: Describe los pasos necesarios para garantizar que la aplicación para la Plataforma universal de Windows (UWP) pueda usarse con un tema de contraste alto activo.
ms.assetid: FD7CA6F6-A8F1-47D8-AA6C-3F2EC3168C45
title: Temas de contraste alto
template: detail.hbs
ms.author: mhopkins
ms.date: 09/28/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7cf8b634cfc7ba66cde107150b54ecec76b2861d
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2018
ms.locfileid: "6263613"
---
# <a name="high-contrast-themes"></a>Temas de contraste alto  

Windows admite temas de contraste alto para el sistema operativo y las aplicaciones que los usuarios pueden elegir habilitar. Los temas de contraste alto usan una pequeña paleta de colores de contraste que facilita la visualización de la interfaz.

![Calculadora con el tema claro y el tema Negro en contraste alto.](images/high-contrast-calculators.png)

*Calculadora con el tema claro y el tema Negro en contraste alto.*

Puedes cambiar a un tema de contraste alto desde *Configuración > Accesibilidad > Contraste alto*.

> [!NOTE]
> No confundas los temas de contraste alto con temas claros y oscuros, que permiten una paleta de colores mucho más amplia que no se considera de contraste alto. Para obtener más temas claros y oscuros, consulta el artículo sobre el [color](../style/color.md).

Aunque los controles comunes ofrecen total compatibilidad con el contraste alto de forma gratuita, es necesario prestar atención al personalizar la interfaz de usuario. El error más común en relación con el contraste alto lo causa la codificación de forma rígida de un color en un control alineado.

```xaml
<!-- Don't do this! -->
<Grid Background="#E6E6E6">

<!-- Instead, create BrandedPageBackgroundBrush and do this. -->
<Grid Background="{ThemeResource BrandedPageBackgroundBrush}">
```

Si el color `#E6E6E6` se establece alineado en el primer ejemplo, el objeto Grid conservará ese color de fondo en todos los temas. Si el usuario cambia al tema Negro en contraste alto, se esperará que tu aplicación tenga un fondo negro. Dado que `#E6E6E6` es casi blanco, es posible que algunos usuarios no puedan interactuar con la aplicación.

En el segundo ejemplo, la [**extensión de marcado {ThemeResource}**](../../xaml-platform/themeresource-markup-extension.md) se usa para hacer referencia a un color de la colección [**ThemeDictionaries**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.resourcedictionary.themedictionaries.aspx), una propiedad dedicada de un elemento [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/BR208794). **ThemeDictionaries** permite a XAML intercambiar automáticamente los colores en función del tema actual del usuario.

## <a name="theme-dictionaries"></a>Diccionarios de temas

Si necesitas cambiar un color de su valor predeterminado del sistema, crea una colección ThemeDictionaries para tu aplicación.

1. Comienza con la creación de las asociaciones adecuadas, si aún no existen. En App.xaml, crea una colección **ThemeDictionaries** que incluya, como mínimo, los temas **Default** y **HighContrast**.
2. En **Default**, crea el tipo de [Brush](http://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.media.brush.aspx) que necesites, normalmente un **SolidColorBrush**. Dale un nombre de *x: Key* que resulte específico para su uso.
3. Asígnale el **Color** que desees.
4. Copia ese **Brush** en **HighContrast**.

``` xaml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <!-- Default is a fallback if a more precise theme isn't called
            out below -->
            <ResourceDictionary x:Key="Default">
                <SolidColorBrush x:Key="BrandedPageBackgroundBrush" Color="#E6E6E6" />
            </ResourceDictionary>

            <!-- Optional, Light is used in light theme.
            If included, Default will be used for Dark theme -->
            <ResourceDictionary x:Key="Light">
                <SolidColorBrush x:Key="BrandedPageBackgroundBrush" Color="#E6E6E6" />
            </ResourceDictionary>

            <!-- HighContrast is used in all high contrast themes -->
            <ResourceDictionary x:Key="HighContrast">
                <SolidColorBrush x:Key="BrandedPageBackgroundBrush" Color="#E6E6E6" />
            </ResourceDictionary>
        </ResourceDictionary.ThemeDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

El último paso es determinar qué color se usará en el contraste alto y se trata en la siguiente sección.

> [!NOTE]
> **HighContrast** no es el único nombre de clave disponible. También están **HighContrastBlack**, **HighContrastWhite** y **HighContrastCustom**. En la mayoría de los casos, solo necesitarás **HighContrast**.

## <a name="high-contrast-colors"></a>Colores de contraste alto

En la página *Configuración > Accesibilidad > Contraste alto*, hay 4 temas de contraste alto de manera predeterminada. 


![Configuración de contraste alto](images/high-contrast-settings.png)  

*Cuando el usuario selecciona una opción, la página muestra una vista previa.*  

![Recursos de contraste alto](images/high-contrast-resources.png)  

*Puedes hacer clic en todas las muestras de color de la vista previa para cambiar su valor. Asimismo, cada muestra se asigna directamente a un recurso de color XAML.*  

Cada recurso **SystemColor*Color** es una variable que actualiza automáticamente el color cuando el usuario cambia los temas de contraste alto. A continuación se muestran directrices sobre dónde y cuándo usar cada recurso.

Recurso | Uso |
|--------|-------|
**SystemColorWindowTextColor** | Copia del cuerpo, encabezados, listas; cualquier texto con el que no se pueda interactuar |
| **SystemColorHotlightColor** | Hipervínculos |
| **SystemColorGrayTextColor** | Interfaz de usuario deshabilitada |
| **SystemColorHighlightTextColor** | Color de primer plano de la interfaz de usuario o el texto en curso o seleccionado, o con el que se esté interactuando actualmente |
| **SystemColorHighlightColor** | Color de fondo de la interfaz de usuario o el texto en curso o seleccionado, o con el que se esté interactuando actualmente |
| **SystemColorButtonTextColor** | Color de primer plano de los botones; cualquier interfaz de usuario con la que se pueda interactuar |
| **SystemColorButtonFaceColor** | Color de fondo de los botones; cualquier interfaz de usuario con la que se pueda interactuar |
| **SystemColorWindowColor** | Fondo de páginas, paneles, elementos emergentes y barras |

A menudo resulta útil observar las aplicaciones existentes, Inicio o los controles comunes para ver cómo han resuelto otras personas problemas de diseño de contraste alto similares a los tuyos.

**Cosas que hacer**

* Respetar los pares de fondo/primer plano siempre que sea posible.
* Probar los 4 temas de contraste alto mientras se ejecuta la aplicación. El usuario no debería tener que reiniciar la aplicación al cambiar los temas.
* Ser coherente.

**Cosas que evitar**

* Codificar de forma rígida un color en el tema **HighContrast**; usar los recursos **SystemColor*Color**.
* Elegir un recurso de color con fines estéticos. Recuerda que cambian con el tema.
* No uses **SystemColorGrayTextColor** para una copia del cuerpo que sea secundaria o actúe como una sugerencia.


Para continuar con el ejemplo anterior, deberás elegir un recurso de **BrandedPageBackgroundBrush**. Dado que el nombre indica que se usará para un fondo, **SystemColorWindowColor** es una buena elección.

``` xaml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.ThemeDictionaries>
            <!-- Default is a fallback if a more precise theme isn't called
            out below -->
            <ResourceDictionary x:Key="Default">
                <SolidColorBrush x:Key="BrandedPageBackgroundBrush" Color="#E6E6E6" />
            </ResourceDictionary>

            <!-- Optional, Light is used in light theme.
            If included, Default will be used for Dark theme -->
            <ResourceDictionary x:Key="Light">
                <SolidColorBrush x:Key="BrandedPageBackgroundBrush" Color="#E6E6E6" />
            </ResourceDictionary>

            <!-- HighContrast is used in all high contrast themes -->
            <ResourceDictionary x:Key="HighContrast">
                <SolidColorBrush x:Key="BrandedPageBackgroundBrush" Color="{ThemeResource SystemColorWindowColor}" />
            </ResourceDictionary>
        </ResourceDictionary.ThemeDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

Más adelante, en la aplicación, puedes establecer el fondo.

```xaml
<Grid Background="{ThemeResource BrandedPageBackgroundBrush}">
```

Ten en cuenta cómo **\{ThemeResource\}** se usa dos veces, una vez para hacer referencia a **SystemColorWindowColor** y otra vez para hacer referencia a **BrandedPageBackgroundBrush**. Ambos son necesarios para que la aplicación use el tema correctamente en tiempo de ejecución. Este es un buen momento para probar la funcionalidad en la aplicación. El fondo de Grid se actualizará automáticamente al cambiar a un tema de contraste alto. También se actualizará al cambiar entre los distintos temas de contraste alto.

## <a name="when-to-use-borders"></a>Cuándo usar bordes

Las páginas, los paneles, los elementos emergentes y las barras deberían usar **SystemColorWindowColor** para su fondo en contraste alto. Agrega un borde solo de contraste alto cuando sea necesario para conservar los límites importantes en la interfaz de usuario.

![Un panel de navegación separado del resto de la página](images/high-contrast-actions-content.png)  

*El panel de navegación y la página comparten el mismo color de fondo en contraste alto. Es fundamental un borde solo de contraste alto para dividirlos.*


## <a name="list-items"></a>Elementos de lista

En contraste alto, los elementos de una clase [ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx) tienen el fondo establecido en **SystemColorHighlightColor** al mantener el mouse encima, pulsarlos o seleccionarlos. Los elementos de lista complejos suelen presentar un error en que el contenido del elemento de lista no puede invertir el color al mantener el mouse sobre el elemento, pulsarlo o seleccionarlo. Esto hace que el elemento no se pueda leer.

![Lista simple con el tema claro y el tema Negro en contraste alto](images/high-contrast-list1.png)

*Una lista sencilla con el tema claro (izquierda) y el tema Negro en contraste alto (derecha). El segundo elemento está seleccionado; observa cómo se invierte su color de texto en contraste alto.*


### <a name="list-items-with-colored-text"></a>Elementos de lista con texto de color

Una causa es establecer TextBlock.Foreground en la clase [DataTemplate](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemscontrol.itemtemplate.aspx) de ListView. Normalmente, esto se hace para establecer una jerarquía visual. La propiedad Foreground se establece en la clase [ListViewItem](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewitem.aspx) y la propiedad TextBlocks de DataTemplate hereda el color de Foreground correcto al mantener el mouse sobre el elemento, pulsarlo o seleccionarlo. Sin embargo, al establecer Foreground, se interrumpe la herencia.

![Lista compleja con el tema claro y el tema Negro en contraste alto](images/high-contrast-list2.png)

*Una lista compleja con el tema claro (izquierda) y el tema Negro en contraste alto (derecha). En contraste alto, la segunda línea del elemento seleccionado no se puede invertir.*  

Para resolverlo establece Foreground de manera condicional a través de un objeto Style de una colección de **ThemeDictionaries**. Dado que **SecondaryBodyTextBlockStyle** no ha establecido **Foreground** en **HighContrast**, su color se invertirá correctamente.

```xaml
<!-- In App.xaml... -->
<ResourceDictionary.ThemeDictionaries>
    <ResourceDictionary x:Key="Default">
        <Style
            x:Key="SecondaryBodyTextBlockStyle"
            TargetType="TextBlock"
            BasedOn="{StaticResource BodyTextBlockStyle}">
            <Setter Property="Foreground" Value="{StaticResource SystemControlForegroundBaseMediumBrush}" />
        </Style>
    </ResourceDictionary>

    <ResourceDictionary x:Key="Light">
        <Style
            x:Key="SecondaryBodyTextBlockStyle"
            TargetType="TextBlock"
            BasedOn="{StaticResource BodyTextBlockStyle}">
            <Setter Property="Foreground" Value="{StaticResource SystemControlForegroundBaseMediumBrush}" />
        </Style>
    </ResourceDictionary>

    <ResourceDictionary x:Key="HighContrast">
        <!-- The Foreground Setter is omitted in HighContrast -->
        <Style
            x:Key="SecondaryBodyTextBlockStyle"
            TargetType="TextBlock"
            BasedOn="{StaticResource BodyTextBlockStyle}" />
    </ResourceDictionary>
</ResourceDictionary.ThemeDictionaries>

<!-- Usage in your DataTemplate... -->
<DataTemplate>
    <StackPanel>
        <TextBlock Style="{StaticResource BodyTextBlockStyle}" Text="Double line list item" />

        <!-- Note how ThemeResource is used to reference the Style -->
        <TextBlock Style="{ThemeResource SecondaryBodyTextBlockStyle}" Text="Second line of text" />
    </StackPanel>
</DataTemplate>
```


## <a name="detecting-high-contrast"></a>Detección de contraste alto

Puedes comprobar mediante programación si el tema actual es un tema de contraste alto usando los miembros de la clase [**AccessibilitySettings**](https://msdn.microsoft.com/library/windows/apps/BR242237).

> [!NOTE]
> Asegúrate de llamar al constructor **AccessibilitySettings** desde un ámbito donde la aplicación esté inicializada y mostrando contenido.

## <a name="related-topics"></a>Temas relacionados  
* [Accesibilidad](accessibility.md)
* [Ejemplo de configuración y contraste de la interfaz de usuario](http://go.microsoft.com/fwlink/p/?linkid=231539)
* [Ejemplo de accesibilidad XAML](http://go.microsoft.com/fwlink/p/?linkid=238570)
* [Ejemplo de contraste alto XAML](http://go.microsoft.com/fwlink/p/?linkid=254993)
* [**AccessibilitySettings**](https://msdn.microsoft.com/library/windows/apps/BR242237)
