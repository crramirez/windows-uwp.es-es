---
title: Tamaños y espaciado
description: Los nuevos estilos de control estándar Fluent y compacto garantizar una experiencia de usuario cómodo independientemente del método de dispositivo y la entrada.
keywords: UWP, Windows 10, controles, tamaño, densidad, estándar, compact
ms.date: 4/4/2019
ms.topic: article
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 7b74e3dc2ad047d9e52509b71ef00b829ad63a0d
ms.sourcegitcommit: 7a1d5198345d114c58287d8a047eadc4fe10f012
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59249457"
---
# <a name="control-size-and-density"></a>Tamaño del control y la densidad

Usar una combinación de tamaño de control y la densidad para optimizar la aplicación de plataforma Universal de Windows (UWP) y proporcionar una experiencia de usuario que sea más adecuada para los requisitos de funcionalidad y la interacción de la aplicación.

De forma predeterminada, las aplicaciones UWP se representan con una baja densidad (o `Standard`) diseño. Sin embargo, a partir WinUI 2.1, una alta densidad (o `Compact`) opción de diseño, para obtener información completa interfaz de usuario y otros escenarios similares especializados, también se admite. Esto se puede especificar a través de un recurso de estilo básica (vea los ejemplos siguientes).

Mientras la funcionalidad y el comportamiento no ha cambiado y se mantiene coherente entre las dos opciones de tamaño y la densidad, el tamaño de fuente de cuerpo predeterminado se ha actualizado a 14px para todos los controles admitir estas opciones de densidad de dos. Este tamaño de fuente funciona en diferentes regiones y los dispositivos y garantiza que la aplicación siga estando equilibrado y cómodo para los usuarios.

## <a name="fluent-standard-sizing"></a>Ajuste de tamaño estándar Fluent

*Ajuste de tamaño estándar Fluent* fue creado para proporcionar un equilibrio entre la comodidad de usuario y la densidad de la información. De hecho, todos los elementos en la pantalla se alinean a un destino de 40 x 40 píxeles efectivos (epx), que permite elementos de interfaz de usuario se alineen con una cuadrícula y escalar de forma adecuada en función de escalado del nivel de sistema.

**Ajuste de tamaño estándar está diseñado para dar cabida a la función táctil y puntero de entrada.**

> [!NOTE]
>Para obtener más información sobre los píxeles efectivos y el escalado, consulte [Introducción al diseño de aplicaciones para UWP](../basics/design-and-ui-intro.md#effective-pixels-and-scaling)
>
> Para obtener más información sobre el escalado del nivel de sistema, consulte [alineación, márgenes y relleno](../layout/alignment-margin-padding.md).

Para Windows 10 de octubre de 2018 Update (versión 1809), el estándar, el tamaño predeterminado para todos los controles UWP se redujo para aumentar la usabilidad en todos los escenarios de uso.

La siguiente imagen muestra algunos del control de cambios de diseño que se introdujeron con Windows Update 10 de octubre de 2018. En concreto, el margen entre un encabezado y la parte superior de un control se redujo de 8epx a 4epx y la cuadrícula 44epx se cambió a una cuadrícula 40epx.

![Ejemplo de diseño de control estándar](images/standarddensity.png)

*Ejemplo de diseño de control estándar*

Esta imagen siguiente muestra los cambios realizados para controlar los tamaños de las ventanas de actualizar el 10 de octubre de 2018. En concreto, la alineación a la cuadrícula 40epx.

![Ejemplo de comandos estándar](images/standarddensitycommanding.png)

## <a name="fluent-compact-sizing"></a>Ajuste de tamaño compacto Fluent

Ajuste de tamaño compacto permite densos, mucha de la información de grupos de controles y puede ayudarle con lo siguiente:

- Exploración de grandes cantidades de contenido.
- Maximización de contenido visible en una página.
- Navegar e interactuar con los controles y contenido

**Ajuste de tamaño compacto está diseñado principalmente para dar cabida a la entrada de puntero.**

### <a name="examples"></a>Ejemplos

Ajuste de tamaño compacto se implementa a través de un diccionario de recursos especiales que se puede especificar en la aplicación en el nivel de página o en un diseño específico. El diccionario de recursos está disponible en el [WinUI](https://docs.microsoft.com/en-us/uwp/toolkits/winui/) paquete Nuget.

Los ejemplos siguientes muestran cómo el `Compact` se puede aplicar el estilo para la página y un control individual de la cuadrícula.

#### <a name="page-level"></a>Nivel de página

```xaml
<Page.Resources>
    <ResourceDictionary Source="ms-appx:///Microsoft.UI.Xaml/DensityStyles/Compact.xaml" />
</Page.Resources>
```

#### <a name="grid-level"></a>Nivel de cuadrícula

```xaml
<Grid>
    <Grid.Resources>
        <ResourceDictionary Source="ms-appx:///Microsoft.UI.Xaml/DensityStyles/Compact.xaml" />
    </Grid.Resources>
</Grid>
```

## <a name="related-articles"></a>Artículos relacionados

- [Directrices para los destinos de toque](../input/guidelines-for-targeting.md)
- [Referencias a ResourceDictionary y a los recursos XAML](https://docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/resourcedictionary-and-xaml-resource-references)
- [Diccionario de recursos](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.resourcedictionary)
- [Estilos XAML](https://docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/xaml-styles) 
