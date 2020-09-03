---
title: Espaciado y tamaño
description: Los nuevos estilos de control Fluent Standard y Fluent Compact garantizan una cómoda experiencia del usuario independientemente del dispositivo y del método de entrada.
keywords: UWP, Windows 10, controles, tamaño, densidad, Standard, Compact
ms.date: 04/19/2019
ms.topic: article
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 37bc5688321249428405e730d366eb987a81bd66
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159849"
---
# <a name="control-size-and-density"></a>Tamaño de control y densidad

Usa una combinación de tamaño de control y densidad con el fin de optimizar la aplicación de Windows y proporcionar una experiencia de usuario más adecuada para las funcionalidades y los requisitos de interacción de la aplicación.

De forma predeterminada, las aplicaciones para UWP se representan con un diseño de baja densidad (o `Standard`). Aun así, a partir WinUI 2.1, también se admite la opción de diseño de alta densidad (o `Compact`) para interfaces de usuario con mucha información y escenarios especializados similares. Esto se puede especificar mediante un recurso de estilo básico (consulta los ejemplos siguientes).

Aunque las funcionalidades y el comportamiento no se han cambiado y se mantienen uniformes en las dos opciones de tamaño y la densidad, el tamaño de fuente predeterminado del cuerpo se ha actualizado a 14 px para todos los controles con el fin de admitir estas dos opciones de densidad. Este tamaño de fuente funciona en diferentes regiones y dispositivos y garantiza que la aplicación esté equilibrada y sea cómoda para los usuarios.

## <a name="examples"></a>Ejemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">XAML Controls Gallery</strong>, haz clic aquí para <a href="xamlcontrolsgallery:/item/Compact Sizing">abrirla y ver el tamaño compacto en acción</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="fluent-standard-sizing"></a>Tamaño Fluent Standard

El *tamaño Fluent Standard* se ha creado para proporcionar un equilibrio entre la densidad de la información y la comodidad del usuario. De hecho, todos los elementos de la pantalla se alinean con un destino de 40 x 40 píxeles efectivos (epx), que permite que los elementos de la interfaz de usuario se alineen con una cuadrícula y se escalen de forma adecuada en función del escalado de nivel de sistema.

**El tamaño Standard está diseñado para integrar la función táctil y la entrada de puntero.**

> [!NOTE]
>Para obtener más información sobre los píxeles efectivos y el escalado, consulta [Introducción al diseño de aplicaciones de Windows](../basics/design-and-ui-intro.md#effective-pixels-and-scaling).
>
> Para obtener más información sobre el escalado de nivel de sistema, consulta [Alineación, margen, espaciado interno](../layout/alignment-margin-padding.md).

En la actualización de octubre de 2018 de Windows 10 (versión 1809), el tamaño predeterminado estándar de todos los controles de UWP se redujo para aumentar la usabilidad en todos los escenarios de uso.

En la imagen siguiente se muestran algunos de los cambios en el diseño de los controles que se introdujeron con la actualización de octubre de 2018 de Windows 10. En concreto, el margen entre un encabezado y la parte superior de un control se ha reducido de 8 epx a 4 epx, mientras que la cuadrícula de 44 epx se ha cambiado a una cuadrícula de 40 epx.

![Ejemplo del diseño de control Standard](images/standarddensity.png)

*Ejemplo del diseño de control Standard*

En la imagen siguiente se muestran los cambios realizados en los tamaños de los controles en la actualización de octubre de 2018 de Windows 10. En concreto, la alineación con la cuadrícula de 40 epx.

![Ejemplo de comandos estándar](images/standarddensitycommanding.png)

## <a name="fluent-compact-sizing"></a>Tamaño Fluent Compact

El tamaño Compact permite grupos de controles densos y con mucha de la información. Además, puede ayudar a:

- Explorar grandes cantidades de contenido.
- Maximizar el contenido visible de una página.
- Navegar e interactuar con los controles y el contenido.

**El tamaño Compact está diseñado principalmente para integrar la entrada de puntero.**

### <a name="examples"></a>Ejemplos

El tamaño Compact se implementa a través de un diccionario de recursos especial que se puede especificar en la aplicación en el nivel de página o en un diseño específico. El diccionario de recursos está disponible en el paquete NuGet [WinUI](/uwp/toolkits/winui/).

En los ejemplos siguientes se muestra cómo se puede aplicar el estilo `Compact` a la página y un control Grid individual.

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

## <a name="get-the-sample-code"></a>Obtener el código de ejemplo

- [Muestra de XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery): Vea todos los controles XAML en un formato interactivo.

## <a name="related-articles"></a>Artículos relacionados

- [Directrices para destinos táctiles](../input/guidelines-for-targeting.md)
- [Referencias a ResourceDictionary y a los recursos XAML](../controls-and-patterns/resourcedictionary-and-xaml-resource-references.md)
- [Diccionario de recursos](/uwp/api/windows.ui.xaml.resourcedictionary)
- [Estilos XAML](../controls-and-patterns/xaml-styles.md)