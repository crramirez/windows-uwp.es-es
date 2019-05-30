---
ms.assetid: a2751e22-6842-073a-daec-425fb981bafe
title: Capa visual
description: La API de Windows.UI.Composition proporciona acceso a la capa de composición entre la capa de marco (XAML) y la capa de elementos gráficos (DirectX).
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7c61f6580039b9fe3da915491acd84c939088370
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66361386"
---
# <a name="visual-layer"></a>Capa visual

La capa visual ofrece una API de modo retenido y alto rendimiento para gráficos, efectos y animaciones, y es la base de todas las interfaces de usuario en todos los dispositivos Windows. La interfaz de usuario la defines de forma declarativa, y la capa visual depende de la aceleración de hardware, para garantizar que el contenido, los efectos y las animaciones se representan de manera suave y no entrecortada, independiente del subproceso de interfaz de usuario de la aplicación.

Información destacada:

* API de WinRT familiares
* Una arquitectura definida para una interfaz de usuario e interacciones más dinámicas
* Conceptos alineados con herramientas de diseño
* Escalabilidad lineal sin huecos repentinos de rendimiento

Las aplicaciones para UWP de Windows ya usan la capa visual a través de uno de los marcos de interfaz de usuario. También puedes sacar provecho de la capa visual directamente para una representación personalizada, efectos y animaciones con muy poco esfuerzo.

![Disposición del marco de trabajo de la interfaz de usuario: la capa de marco (Windows.UI. XAML) está integrada en la capa visual (Windows.UI. Composición) que está integrada en la capa de gráficos (DirectX).](images/layers-win-ui-composition.png)

## <a name="whats-in-the-visual-layer"></a>¿Qué es la capa visual?

Las principales funciones de la capa visual son las siguientes:

1. **Contenido**: Composición ligera de contenido dibujado personalizado
1. **Efectos**: Sistema cuyos efectos se pueden animar, encadenar y personalizar los efectos de la interfaz de usuario en tiempo real
1. **Las animaciones**: Animaciones expresivas, independiente del marco de trabajo ejecuta independientemente del subproceso de interfaz de usuario

### <a name="content"></a>Contenido

El contenido es hospedado, transformado y disponible para su uso por el sistema de animación y efectos mediante elementos visuales. En la base de la jerarquía de clase es la clase [**Visual**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.Visual), un proxy ligero de subprocesos ágiles en el proceso de la aplicación para el estado visual del compositor de objetos. Incluyen subclases de Visual  [**ContainerVisual** ](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.ContainerVisual) para permitir que los elementos secundarios crear árboles de objetos visuales y [ **SpriteVisual** ](https://docs.microsoft.com/uwp/api/Windows.UI.Composition.SpriteVisual) que contiene el contenido y se puede pintar con cualquier colores sólidos, efectos personalizados de contenido o visuales dibujados. En conjunto, estos tipos de elementos visuales conforman la estructura del árbol visual para la interfaz de usuario 2D y respaldan a los objetos FrameworkElements de XAML más visibles.

Para más información, consulta la información general sobre los [objetos visuales de composición](composition-visual-tree.md).

### <a name="effects"></a>Efectos

El sistema de efectos de la capa visual te permite aplicar una cadena de efectos de filtro y transparencia para un elemento visual o un árbol de elementos visuales. Se trata de un sistema de efectos de la interfaz de usuario, que no debe confundirse los con efectos de imagen y contenido multimedia. Los efectos funcionan en junto con el sistema de animación, lo que permite a los usuarios lograr animaciones suaves y dinámicas de propiedades de efectos, representados independientemente del subproceso de interfaz de usuario. Los efectos de la capa visual proporcionan los bloques base creativos que se pueden combinar y animar para crear experiencias interactivas y adaptadas.

Además de las cadenas de efectos que se pueden animar, la capa visual también admite un modelo de iluminación que permite a los elementos visuales imitar propiedades materiales respondiendo a luces que se pueden animar. Los elementos visuales también pueden modelar sombras. La iluminación y las sombras se pueden combinar para crear una sensación de profundidad y realismo.

Para más información, consulta la información general sobre los [efectos de composición](composition-effects.md).

### <a name="animations"></a>Animaciones

El sistema de animación de la capa visual te permite mover elementos visuales, animar efectos e impulsar transformaciones, clips y otras propiedades.  Se trata de un sistema independiente de marco que se diseñó desde el principio para mejorar el rendimiento.  Se ejecuta de forma independiente desde el subproceso de interfaz de usuario para garantizar suavidad y escalabilidad.  Si bien te permite usar animaciones de fotograma clave familiares para impulsar los cambios de propiedades a medida que pasa el tiempo, también te permite establecer relaciones matemáticas entre propiedades diferentes, como por ejemplo, la entrada del usuario, lo que te permite crear directamente experiencias coreografiadas ágiles.

Para más información, consulta la información general sobre las [animaciones de composición](composition-animation.md).

### <a name="working-with-your-xaml-uwp-app"></a>Trabajar con la aplicación para UWP de XAML

Puedes acceder a un elemento visual creado por el marco XAML y respaldando un objeto FrameworkElement visible, mediante la clase [**ElementCompositionPreview**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Hosting.ElementCompositionPreview) en [**Windows.UI.Xaml.Hosting**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Hosting). Ten en cuenta que los elementos visuales que el marco crea para ti vienen con algunas limitaciones en cuanto a personalización. Esto se debe a que el marco administra los desplazamientos, las transformaciones y los ciclos de vida. Sin embargo, puedes crear tus propios elementos visuales y adjuntarlos a un elemento XAML existente a través de ElementCompositionPreview o agregándolo a un objeto ContainerVisual existente en algún lugar de la estructura del árbol visual.

Para más información, consulta la información general de [Uso de la capa visual con XAML](using-the-visual-layer-with-xaml.md).

### <a name="working-with-your-desktop-app"></a>Trabajar con su aplicación de escritorio

Puede usar la capa Visual para mejorar la apariencia, sensación y funcionalidad de su WPF, Windows Forms, y C++ aplicaciones de escritorio de Win32. Puede migrar islas de contenido para utilizar la capa Visual y mantener el resto de la interfaz de usuario en su marco de trabajo existente. Esto significa que puede realizar actualizaciones importantes y mejoras en la interfaz de usuario de la aplicación sin necesidad de realizar grandes cambios en el código existente base.

Para más información, consulta [Modernización de una aplicación de escritorio mediante la capa visual](/windows/apps/desktop/modernize/visual-layer-in-desktop-apps).

## <a name="additional-resources"></a>Recursos adicionales

* [**Documentación de referencia completa de la API**](https://docs.microsoft.com/uwp/api/Windows.UI.Composition)
* Muestras avanzadas de la interfaz de usuario y la composición en [WindowsUIDevLabs GitHub](https://github.com/microsoft/windowsuidevlabs).
* [Galería de ejemplos de Windows.UI.Composition](https://aka.ms/winuiapp)
* [@windowsui Fuente de Twitter ](https://twitter.com/windowsui)
* Lea el artículo de MSDN de Kenny en esta API: [Gráficos y animación: la nueva composición para Windows 10](https://msdn.microsoft.com/magazine/mt590968)
