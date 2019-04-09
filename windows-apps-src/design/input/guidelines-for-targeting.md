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
ms.openlocfilehash: 5c05b6686d31606a9510b1433339dc8829a52893
ms.sourcegitcommit: 7a1d5198345d114c58287d8a047eadc4fe10f012
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59247183"
---
# <a name="guidelines-for-touch-targets"></a>Directrices para los destinos de toque

Todos los elementos de interfaz de usuario interactivos en la aplicación de plataforma Universal de Windows (UWP) deben ser lo suficientemente grandes como para los usuarios acceso con precisión y utilizar, independientemente del método de entrada o el tipo de dispositivo.

Compatibilidad con entrada táctil (y la naturaleza relativamente imprecisa del área de contacto táctil) requiere una optimización adicional con respecto al diseño de tamaño y el control de destino como el conjunto más grande y complejo de datos de entrada notificados por el digitalizador táctil se usa para determinar el destino de previsto (o el más probable) del usuario.

Todos los controles UWP se han diseñado con tamaños de destino táctil predeterminados y diseños que permiten crear aplicaciones visualmente atractivas y equilibradas que son fáciles de usar, cómodo e inspiran confianza.

En este tema, se describen estos comportamientos predeterminados, por lo que puede diseñar la aplicación para la máxima facilidad de uso mediante los controles de plataforma y los controles personalizados (la aplicación, es necesario ellos).

> **API importantes**: [**Windows.UI.Core**](https://msdn.microsoft.com/library/windows/apps/br208383), [**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084), [**Windows.UI.Xaml.Input**](https://msdn.microsoft.com/library/windows/apps/br227994)

## <a name="fluent-standard-sizing"></a>Ajuste de tamaño estándar Fluent

*Ajuste de tamaño estándar Fluent* fue creado para proporcionar un equilibrio entre la comodidad de usuario y la densidad de la información. De hecho, todos los elementos en la pantalla se alinean a un destino de 40 x 40 píxeles efectivos (epx), que permite elementos de interfaz de usuario se alineen con una cuadrícula y escalar de forma adecuada en función de escalado del nivel de sistema.

> [!NOTE]
>Para obtener más información sobre los píxeles efectivos y el escalado, consulte [Introducción al diseño de aplicaciones para UWP](../basics/design-and-ui-intro.md#effective-pixels-and-scaling)
>
> Para obtener más información sobre el escalado del nivel de sistema, consulte [alineación, márgenes y relleno](../layout/alignment-margin-padding.md).

## <a name="fluent-compact-sizing"></a>Ajuste de tamaño compacto Fluent

Las aplicaciones pueden mostrar un mayor nivel de densidad de la información con *sizing Fluent Compact*. Ajuste de tamaño compacto alinea los elementos de interfaz de usuario a un destino de 32 x 32 epx, que permite a los elementos de interfaz de usuario para alinearse con una cuadrícula más estricto y una escala adecuadamente en función de escalado del nivel de sistema.

### <a name="examples"></a>Ejemplos

Ajuste de tamaño compacto puede aplicarse en el nivel de página o una cuadrícula.

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

En general, establezca el tamaño de destino táctil al intervalo de 7,5 mm cuadrado (40 x 40 píxeles en una pantalla PPI 135 en un 1,0 x escalado meseta). Normalmente, los controles UWP se alinean con destino táctil para 7,5 mm (Esto puede variar según el control específico y los patrones de uso común). Consulte [controlar el tamaño y la densidad](../style/spacing.md) para obtener más detalles.

Estas recomendaciones del tamaño de destino pueden ajustarse según sea necesario para un escenario en particular. Estos son algunos aspectos a tener en cuenta:

- Frecuencia de toques - considere convertir los destinos que están varias veces con frecuencia presionados o mayores que el tamaño mínimo.
- Error consecuencia - destinos que tiene consecuencias graves si tocadas error debe tener mayor relleno y colocarse más lejos del borde del área de contenido. Esto se aplica especialmente a destinos que se tocan con frecuencia.
- Posición en el área de contenido.
- Tamaño de pantalla y el factor de formulario.
- Posición del dedo.
- Pulse en visualizaciones.

## <a name="related-articles"></a>Artículos relacionados

- [Introducción al diseño de aplicaciones para UWP](../basics/design-and-ui-intro.md)
- [Tamaño del control y la densidad](../style/spacing.md)
- [Alineación, margen, espaciado interno](../layout/alignment-margin-padding.md)

### <a name="samples"></a>Muestras

- [Ejemplo de entrada básica](https://go.microsoft.com/fwlink/p/?LinkID=620302)
- [Ejemplo de entrada de latencia baja](https://go.microsoft.com/fwlink/p/?LinkID=620304)
- [Ejemplo de modo de interacción del usuario](https://go.microsoft.com/fwlink/p/?LinkID=619894)
- [Muestra de elementos visuales de foco](https://go.microsoft.com/fwlink/p/?LinkID=619895)

### <a name="archive-samples"></a>Ejemplos de archivo

- [Entrada: Ejemplo de eventos de entrada de usuario XAML](https://go.microsoft.com/fwlink/p/?linkid=226855)
- [Entrada: Ejemplo de las capacidades de dispositivo](https://go.microsoft.com/fwlink/p/?linkid=231530)
- [Entrada: Ejemplo de pruebas de posicionamiento táctil](https://go.microsoft.com/fwlink/p/?linkid=231590)
- [Muestra de desplazamiento, movimiento panorámico y zoom XAML](https://go.microsoft.com/fwlink/p/?linkid=251717)
- [Entrada: Ejemplo de tinta simplificada](https://go.microsoft.com/fwlink/p/?linkid=246570)
- [Entrada: Ejemplo de gestos de Windows 8](https://go.microsoft.com/fwlink/p/?LinkId=264995)
- [Entrada: Las manipulaciones y ejemplo de gestos (C++)](https://go.microsoft.com/fwlink/p/?linkid=231605)
- [Muestra de entrada táctil de DirectX](https://go.microsoft.com/fwlink/p/?LinkID=231627)
