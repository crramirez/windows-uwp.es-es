---
Description: En este tema se describe el uso de la geometría de contacto para la selección táctil del destino y se proporcionan procedimientos recomendados para la selección del destino en aplicaciones de Windows Runtime.
title: Establecer destinos
ms.assetid: 93ad2232-97f3-42f5-9e45-3fc2143ac4d2
label: Targeting
template: detail.hbs
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 199c120dcc85e5c113d6d4d529699a3f2fb28aa1
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970090"
---
# <a name="guidelines-for-touch-targets"></a>Directrices para destinos táctiles

Todos los elementos de interfaz de usuario interactivos de la aplicación de aplicación de Windows deben ser lo suficientemente grandes como para que los usuarios accedan y utilicen con precisión, independientemente del tipo de dispositivo o del método de entrada.

La compatibilidad con la entrada táctil (y la naturaleza relativamente poco precisa del área de contacto táctil) requiere una optimización adicional con respecto al tamaño de destino y al diseño del control, ya que el número más grande y más complejo de datos de entrada que se comunica mediante el digitalizador táctil se usa para determinar el destino previsto (o el más probable) del usuario.

Todos los controles de UWP se han diseñado con diseños y tamaños de destino táctil predeterminados que le permiten crear aplicaciones visualmente equilibradas y atractivas, fáciles de usar e inspirar confianza.

En este tema, se describen estos comportamientos predeterminados para que pueda diseñar su aplicación para obtener el máximo uso mediante controles de plataforma y controles personalizados (si la aplicación los necesita).

> **API importantes**: [**Windows. UI. Core**](https://docs.microsoft.com/uwp/api/Windows.UI.Core), [**Windows. UI. Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input), [**Windows. UI. Xaml. Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input)

## <a name="fluent-standard-sizing"></a>Tamaño Fluent Standard

El *tamaño Fluent Standard* se ha creado para proporcionar un equilibrio entre la densidad de la información y la comodidad del usuario. De hecho, todos los elementos de la pantalla se alinean con un destino de 40 x 40 píxeles efectivos (epx), que permite que los elementos de la interfaz de usuario se alineen con una cuadrícula y se escalen de forma adecuada en función del escalado de nivel de sistema.

> [!NOTE]
> Para obtener más información sobre los píxeles efectivos y el escalado, consulte [Introducción al diseño de aplicaciones de Windows](../basics/design-and-ui-intro.md#effective-pixels-and-scaling) .
>
> Para obtener más información sobre el escalado de nivel de sistema, consulta [Alineación, margen, espaciado interno](../layout/alignment-margin-padding.md).

## <a name="fluent-compact-sizing"></a>Tamaño Fluent Compact

Las aplicaciones pueden mostrar un nivel más alto de densidad de información con *ajuste de tamaño compacto fluida*. El ajuste de tamaño compacto alinea los elementos de la interfaz de usuario con un destino de 32x32 EPX, lo que permite que los elementos de la interfaz de usuario se alineen con una cuadrícula más estrecha y se escalen adecuadamente según el escalado del nivel

### <a name="examples"></a>Ejemplos

El ajuste de tamaño compacto se puede aplicar en el nivel de página o de cuadrícula.

### <a name="page-level"></a>De página

```xaml
<Page.Resources>
    <ResourceDictionary Source="ms-appx:///Microsoft.UI.Xaml/DensityStyles/Compact.xaml" />
</Page.Resources>
```

### <a name="grid-level"></a>Nivel de cuadrícula

```xaml
<Grid>
    <Grid.Resources>
        <ResourceDictionary Source="ms-appx:///Microsoft.UI.Xaml/DensityStyles/Compact.xaml" />
    </Grid.Resources>
</Grid>
```

## <a name="target-size"></a>Tamaño de destino

En general, establezca el tamaño del destino táctil en un intervalo de cuadrados de 7,5 mm (40 x 40 píxeles en una pantalla de 135 PPP con un nivel de ajuste de escala de 1,0 x). Normalmente, los controles de UWP se alinean con un destino táctil de 7,5 mm (esto puede variar en función del control específico y de cualquier patrón de uso común). Consulte [control de tamaño y densidad](../style/spacing.md) para obtener más detalles.

Estas recomendaciones del tamaño de destino pueden ajustarse según sea necesario para un escenario en particular. Estos son algunos aspectos que hay que tener en cuenta:

- Frecuencia de los toques: considere la posibilidad de hacer que los destinos se presionen de forma repetida o más grande que el tamaño mínimo.
- Consecuencias de error: los destinos que tienen consecuencias graves si se tocan en un error deben tener un relleno mayor y colocarse más lejos del borde del área de contenido. Esto se aplica especialmente a destinos que se tocan con frecuencia.
- Posición en el área de contenido.
- Factor de forma y tamaño de la pantalla.
- Postura de dedo.
- Visualizaciones táctiles.

## <a name="related-articles"></a>Artículos relacionados

- [Introducción al diseño de aplicaciones de Windows](../basics/design-and-ui-intro.md)
- [Tamaño de control y densidad](../style/spacing.md)
- [Alineación, margen, espaciado interno](../layout/alignment-margin-padding.md)

### <a name="samples"></a>Ejemplos

- [Ejemplo de entrada básica](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [Ejemplo de entrada de latencia baja](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [Ejemplo de modo de interacción del usuario](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [Ejemplo de elementos visuales de foco](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

### <a name="archive-samples"></a>Ejemplos de archivo

- [Entrada: muestra de eventos de entrada de usuario de XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20XAML%20user%20input%20events%20sample)
- [Entrada: muestra de funcionalidades del dispositivo](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
- [Entrada: muestra de prueba de acceso táctil](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20desktop%20samples/%5BC%2B%2B%5D-Windows%208%20desktop%20samples/C%2B%2B/Windows%208%20desktop%20samples/Input%20Touch%20hit%20testing%20sample)
- [Ejemplo de desplazamiento, panorámica y zoom de XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Universal%20Windows%20app%20samples/111487-Universal%20Windows%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample)
- [Entrada: ejemplo de entrada de lápiz simplificada](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Simplified%20ink%20sample)
- [Entrada: muestra de gestos de Windows 8](https://docs.microsoft.com/samples/browse/?redirectedfrom=MSDN-samples)
* [Entrada: ejemplo de manipulaciones y gestos](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Gestures%20and%20manipulations%20with%20GestureRecognizer)
- [Muestra de entrada táctil de DirectX](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%2B%2B%5D-Windows%208%20app%20samples/C%2B%2B/Windows%208%20app%20samples/DirectX%20touch%20input%20sample%20(Windows%208))
