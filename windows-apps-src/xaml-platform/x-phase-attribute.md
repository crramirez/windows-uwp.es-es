---
author: jwmsft
title: Atributo xPhase
description: Usa xPhase con la extensión de marcado xBind para representar los elementos ListView y GridView de forma incremental y mejorar la experiencia de movimiento panorámica.
ms.assetid: BD17780E-6A34-4A38-8D11-9703107E247E
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 17ee99553b5713acb1917ccb697abb2387d00da2
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2018
ms.locfileid: "6832474"
---
# <a name="xphase-attribute"></a>Atributo x:Phase


Usa **x:Phase** con la [extensión de marcado {x:Bind}](x-bind-markup-extension.md) para representar los elementos [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878) y [**GridView**](https://msdn.microsoft.com/library/windows/apps/br242705) de forma incremental y mejorar la experiencia de movimiento panorámica. **x:Phase** proporciona una manera declarativa de lograr el mismo efecto que si se usase el evento [**ContainerContentChanging**](https://msdn.microsoft.com/library/windows/apps/dn298914) para controlar manualmente la representación de los elementos de lista. Consulta también [Actualizar los elementos ListView and GridView de forma incremental](../debug-test-perf/optimize-gridview-and-listview.md#update-items-incrementally).

## <a name="xaml-attribute-usage"></a>Uso del atributo XAML


``` syntax
<object x:Phase="PhaseValue".../>
```

## <a name="xaml-values"></a>Valores de XAML


| Término | Descripción |
|------|-------------|
| PhaseValue | Un número que indica la fase en la que se procesará el elemento. El predeterminado es 0. | 

## <a name="remarks"></a>Observaciones

Si una lista se mueve panorámicamente con el dedo o la rueda del ratón, tal vez no se puedan representar los elementos a la misma velocidad que el desplazamiento en función de la complejidad de la plantilla de datos. Esto es especialmente cierto para un dispositivo portátil con una CPU eficiente como un teléfono o una tableta.

El escalonamiento habilita la representación incremental de la plantilla de datos para que se pueda priorizar el contenido y que los elementos más importantes se representen primero. Así la lista puede mostrar contenido de cada elemento en caso de desplazamiento rápido y representar más elementos de cada plantilla conforme el tiempo lo vaya permitiendo.

## <a name="example"></a>Ejemplo

```xml
<DataTemplate x:Key="PhasedFileTemplate" x:DataType="model:FileItem">
    <Grid Width="200" Height="80">
        <Grid.ColumnDefinitions>
           <ColumnDefinition Width="75" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>
        <Image Grid.RowSpan="4" Source="{x:Bind ImageData}" MaxWidth="70" MaxHeight="70" x:Phase="3"/>
        <TextBlock Text="{x:Bind DisplayName}" Grid.Column="1" FontSize="12"/>
        <TextBlock Text="{x:Bind prettyDate}"  Grid.Column="1"  Grid.Row="1" FontSize="12" x:Phase="1"/>
        <TextBlock Text="{x:Bind prettyFileSize}"  Grid.Column="1"  Grid.Row="2" FontSize="12" x:Phase="2"/>
        <TextBlock Text="{x:Bind prettyImageSize}"  Grid.Column="1"  Grid.Row="3" FontSize="12" x:Phase="2"/>
    </Grid>
</DataTemplate>
```

La plantilla de datos describe 4 fases:

1.  Presenta el bloque de texto DisplayName. Todos los controles sin fase especificada se considerarán implícitamente como parte de la fase 0.
2.  Muestra el bloque de texto prettyDate.
3.  Muestra los bloques de texto prettyFileSize y prettyImageSize.
4.  Muestra la imagen.

El escalonamiento es una característica de [{x:Bind}](x-bind-markup-extension.md) que funciona con los controles derivados de [**ListViewBase**](https://msdn.microsoft.com/library/windows/apps/br242879) y que procesa incrementalmente la plantilla de elementos para el enlace de datos. Al representar elementos de lista, **ListViewBase** representa una sola fase para todos los elementos en la vista antes de pasar a la fase siguiente. El trabajo de representación se realiza en lotes divididos por tiempo para que durante el desplazamiento por la lista se pueda reevaluar el trabajo necesario y no se realice en el caso de elementos que ya no queden a la vista.

El atributo **x:Phase** se puede especificar en cualquier elemento de una plantilla de datos que use [{x:Bind}](x-bind-markup-extension.md). Cuando un elemento tiene una fase distinta de 0, el elemento se ocultará de la vista (a través de **Opacity**, no **Visibility**) hasta que dicha fase se procese y se actualicen los enlaces. Cuando se desplace un control derivado de [**ListViewBase**](https://msdn.microsoft.com/library/windows/apps/br242879), reciclará las plantillas de elemento de los elementos que ya no están en la pantalla para representar los nuevos elementos visibles. Los elementos de la interfaz de usuario dentro de la plantilla conservan sus valores antiguos hasta que están enlazados a datos nuevamente. El escalonamiento hace que el paso de enlace de datos se retrase y, por tanto, necesite ocultar los elementos de interfaz de usuario en caso de que estén obsoletos.

Cada elemento de interfaz de usuario puede tener solo una fase especificada. Si es así, se aplicará a todos los enlaces en el elemento. Si no se especifica una fase, se supone la fase 0.

Los números de fase no necesitan ser contiguos y son los mismos que el valor de [**ContainerContentChangingEventArgs.Phase**](https://msdn.microsoft.com/library/windows/apps/dn298493). El evento [**ContainerContentChanging**](https://msdn.microsoft.com/library/windows/apps/dn298914) se presentará para cada fase antes de que se procesen los enlaces **x:Phase**.

El escalonamiento solo afecta a los enlaces [{x:Bind}](x-bind-markup-extension.md) , no a los enlaces [{Binding}](binding-markup-extension.md) .

El escalonamiento solo se aplica cuando la plantilla de elementos se representa usando un control que tiene constancia del escalonamiento. Para Windows 10, esto significa [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878) y [**GridView**](https://msdn.microsoft.com/library/windows/apps/br242705). El escalonamiento no se aplicará a las plantillas de datos usadas en otros controles de elementos ni en otros escenarios, como las secciones [**ContentTemplate**](https://msdn.microsoft.com/library/windows/apps/br209369) o [**Hub**](https://msdn.microsoft.com/library/windows/apps/dn251843). En estos casos, todos los elementos de la interfaz de usuario serán datos enlazados a la vez.

