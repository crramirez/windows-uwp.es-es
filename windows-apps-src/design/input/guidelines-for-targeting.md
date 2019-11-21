---
Description: En este tema se describe el uso de la geometría de contacto para la selección táctil del destino y se proporcionan procedimientos recomendados para la selección del destino en aplicaciones de Windows Runtime.
title: Selección de destino
ms.assetid: 93ad2232-97f3-42f5-9e45-3fc2143ac4d2
label: Targeting
template: detail.hbs
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9b1cac04405f18aaf3c8f39f9bfce2b965577807
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2019
ms.locfileid: "74257938"
---
# <a name="guidelines-for-touch-targets"></a>Directrices para los destinos táctiles

Todos los elementos interactivos de la interfaz de usuario de la aplicación Plataforma universal de Windows (UWP) deben ser lo suficientemente grandes como para que los usuarios accedan y utilicen con precisión, independientemente del tipo de dispositivo o del método de entrada.

La compatibilidad con la entrada táctil (y la naturaleza relativamente poco precisa del área de contacto táctil) requiere una optimización adicional con respecto al tamaño de destino y al diseño del control, ya que el modo más grande y más complejo de datos de entrada que se comunica mediante el digitalizador táctil se usa para determinar el destino previsto (o más probable) del usuario.

Todos los controles de UWP se han diseñado con diseños y tamaños de destino táctil predeterminados que le permiten crear aplicaciones visualmente equilibradas y atractivas, fáciles de usar e inspirar confianza.

En este tema, se describen estos comportamientos predeterminados para que pueda diseñar su aplicación para obtener el máximo uso mediante controles de plataforma y controles personalizados (si la aplicación los necesita).

> **API importantes**: [**Windows.UI.Core**](https://docs.microsoft.com/uwp/api/Windows.UI.Core), [**Windows.UI.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input), [**Windows.UI.Xaml.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Input)

## <a name="fluent-standard-sizing"></a>Tamaño Fluent Standard

El *tamaño Fluent Standard* se ha creado para proporcionar un equilibrio entre la densidad de la información y la comodidad del usuario. De hecho, todos los elementos de la pantalla se alinean con un destino de 40 x 40 píxeles efectivos (epx), que permite que los elementos de la interfaz de usuario se alineen con una cuadrícula y se escalen de forma adecuada en función del escalado de nivel de sistema.

> [!NOTE]
>Para obtener más información sobre los píxeles efectivos y el escalado, consulta [Introducción al diseño de aplicaciones para UWP](../basics/design-and-ui-intro.md#effective-pixels-and-scaling).
>
> Para obtener más información sobre el escalado de nivel de sistema, consulta [Alineación, margen, espaciado interno](../layout/alignment-margin-padding.md).

## <a name="fluent-compact-sizing"></a>Tamaño Fluent Compact

Las aplicaciones pueden mostrar un nivel más alto de densidad de información con *ajuste de tamaño compacto fluida*. El ajuste de tamaño compacto alinea los elementos de la interfaz de usuario con un destino de 32x32 EPX, lo que permite que los elementos de la interfaz de usuario se alineen con una cuadrícula más estrecha y se escalen adecuadamente según el escalado del nivel

### <a name="examples"></a>Ejemplos

El ajuste de tamaño compacto se puede aplicar en el nivel de página o de cuadrícula.

### <a name="page-level"></a>Nivel de página

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

Estas recomendaciones del tamaño de destino pueden ajustarse según sea necesario para un escenario en particular. Estos son algunos aspectos que se deben tener en cuenta:

- Frecuencia de los toques: considere la posibilidad de hacer que los destinos se presionen de forma repetida o más grande que el tamaño mínimo.
- Consecuencias de error: los destinos que tienen consecuencias graves si se tocan en un error deben tener un relleno mayor y colocarse más lejos del borde del área de contenido. Esto se aplica especialmente a destinos que se tocan con frecuencia.
- Posición en el área de contenido.
- Factor de forma y tamaño de la pantalla.
- Postura de dedo.
- Visualizaciones táctiles.

## <a name="related-articles"></a>Artículos relacionados

- [Introducción al diseño de aplicaciones para UWP](../basics/design-and-ui-intro.md)
- [Tamaño y densidad del control](../style/spacing.md)
- [Alineación, margen, espaciado interno](../layout/alignment-margin-padding.md)

### <a name="samples"></a>Muestras

- [Ejemplo de entrada básica](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [Ejemplo de entrada de baja latencia](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [Ejemplo de modo de interacción del usuario](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [Ejemplo de elementos visuales de foco](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

### <a name="archive-samples"></a>Muestras de archivo

- [Entrada: ejemplo de eventos de entrada de usuario de XAML](https://code.msdn.microsoft.com/windowsapps/Input-3dff271b)
- [Entrada: ejemplo de funcionalidades del dispositivo](https://code.msdn.microsoft.com/windowsapps/Input-device-capabilities-31b67745)
- [Entrada: ejemplo de prueba de posicionamiento táctil](https://code.msdn.microsoft.com/windowsapps/Touch-Hit-Testing-sample-5e35c690)
- [Ejemplo de desplazamiento, panorámica y zoom de XAML](https://code.msdn.microsoft.com/windowsapps/xaml-scrollviewer-pan-and-949d29e9)
- [Entrada: ejemplo de entrada de lápiz simplificada](https://code.msdn.microsoft.com/windowsapps/Input-simplified-ink-sample-11614bbf)
- [Entrada: ejemplo de gestos de Windows 8](https://docs.microsoft.com/samples/browse/?redirectedfrom=MSDN-samples)
- [Entrada: ejemplo de manipulaciones y gestos (C++)](https://code.msdn.microsoft.com/windowsapps/Manipulations-and-gestures-362b6b59)
- [Ejemplo de entrada táctil de DirectX](https://code.msdn.microsoft.com/windowsapps/Simple-Direct3D-Touch-f98db97e)
