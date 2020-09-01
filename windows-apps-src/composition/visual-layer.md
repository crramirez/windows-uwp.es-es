---
ms.assetid: a2751e22-6842-073a-daec-425fb981bafe
title: Capa visual
description: La API de Windows.UI.Composition proporciona acceso a la capa de composición entre la capa de marco (XAML) y la capa de elementos gráficos (DirectX).
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3cdd7c17bc0d419a7449b366b620a4d5654cccc4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89160729"
---
# <a name="visual-layer"></a>Capa visual

La capa visual proporciona una API de modo retenido de alto rendimiento para gráficos, efectos y animaciones, y es la base para todas las interfaces de usuario en todos los dispositivos de Windows.La interfaz de usuario se define de forma declarativa y el nivel visual se basa en la aceleración de hardware de gráficos para garantizar que el contenido, los efectos y las animaciones se representan de forma fluida y sin problemas con independencia del subproceso de la interfaz de usuario de la aplicación.

Sugerencias destacadas:

* API de WinRT conocidas
* Diseñado para la interfaz de usuario y las interacciones más dinámicas
* Conceptos alineados con las herramientas de diseño
* Escalabilidad lineal sin acantilados de rendimiento repentinos

Las aplicaciones para UWP de Windows ya usan la capa visual a través de uno de los marcos de interfaz de usuario. También puede aprovechar el nivel visual directamente para la representación, los efectos y las animaciones personalizadas con muy poco esfuerzo.

![Disposición en capas del marco de interfaz de usuario: la capa de marco de trabajo (Windows. UI. XAML) se basa en la capa visual (Windows. UI. Composition) que se basa en la capa de gráficos (DirectX)](images/layers-win-ui-composition.png)

## <a name="whats-in-the-visual-layer"></a>¿Qué hay en la capa visual?

Las funciones principales de la capa visual son:

1. **Contenido**: composición ligera de contenido dibujado personalizado
1. **Efectos**: sistema de efectos de la interfaz de usuario en tiempo real cuyos efectos se pueden animar, encadenar y personalizar
1. **Animaciones**: animaciones expresivas independientes de la plataforma que se ejecutan de manera independiente del subproceso de interfaz de usuario

### <a name="content"></a>Contenido

El contenido se hospeda, transforma y se pone a disposición para que lo use el sistema de animación y efectos con objetos visuales. En la base de la jerarquía de clases está la clase [**Visual**](/uwp/api/Windows.UI.Composition.Visual) , un proxy ligero de subprocesos y ágil en el proceso de la aplicación para el estado visual en el compositor. Las subclases de Visual incluyen  [**ContainerVisual**](/uwp/api/Windows.UI.Composition.ContainerVisual) para permitir a los elementos secundarios crear árboles de objetos visuales y [**SpriteVisual**](/uwp/api/Windows.UI.Composition.SpriteVisual) que contienen contenido y se pueden pintar con colores sólidos, contenido dibujado personalizado o efectos visuales. Juntos, estos tipos de objetos visuales componen la estructura de árbol visual para la interfaz de usuario 2D y la mayoría de los FrameworkElements XAML visibles.

Para obtener más información, vea la información general del [visual de composición](composition-visual-tree.md) .

### <a name="effects"></a>Efectos

El sistema de efectos de la capa visual le permite aplicar una cadena de efectos de filtro y transparencia a un gráfico visual o a un árbol de objetos visuales. Se trata de un sistema de efectos de la interfaz de usuario, que no se debe confundir con efectos multimedia y de imagen. Los efectos funcionan junto con el sistema de animación, lo que permite a los usuarios obtener animaciones suaves y dinámicas de propiedades de efectos, que se representan de forma independiente del subproceso de interfaz de usuario. Los efectos en el nivel visual proporcionan los bloques de creación creativos que se pueden combinar y animar para construir experiencias personalizadas e interactivas.

Además de las cadenas de efectos que se pueden animar, la capa visual también admite un modelo de iluminación que permite a los objetos visuales imitar propiedades de material al responder a las luces animadas. Los objetos visuales también pueden proyectar sombras. La iluminación y las sombras se pueden combinar para crear una percepción de profundidad y realismo.

Para más información, consulta la información general sobre los [efectos de composición](composition-effects.md).

### <a name="animations"></a>Animaciones

El sistema de animación de la capa visual le permite trasladar objetos visuales, animar efectos y impulsar transformaciones, clips y otras propiedades.Es un sistema independiente del marco de trabajo que se ha diseñado desde cero teniendo en cuenta el rendimiento.Se ejecuta independientemente del subproceso de interfaz de usuario para garantizar la suavidad y la escalabilidad.Aunque permite usar animaciones de fotogramas clave conocidas para controlar los cambios de propiedad con el tiempo, también permite configurar relaciones matemáticas entre diferentes propiedades, incluidos los datos proporcionados por el usuario, lo que permite crear directamente experiencias de sencillamente sin problemas.

Para más información, consulta la información general sobre las [animaciones de composición](composition-animation.md).

### <a name="working-with-your-xaml-uwp-app"></a>Trabajar con la aplicación para UWP de XAML

Puede obtener acceso a un visual creado por el marco XAML y hacer una copia de seguridad de un FrameworkElement visible mediante la clase [**ElementCompositionPreview**](/uwp/api/Windows.UI.Xaml.Hosting.ElementCompositionPreview) de [**Windows. UI. Xaml. Hosting**](/uwp/api/Windows.UI.Xaml.Hosting). Tenga en cuenta que los objetos visuales creados por el marco de trabajo tienen algunos límites en la personalización. Esto se debe a que el marco de trabajo administra desplazamientos, transformaciones y duraciones. Sin embargo, puede crear sus propios objetos visuales y asociarlos a un elemento XAML existente a través de ElementCompositionPreview, o bien agregarlos a un ContainerVisual existente en alguna parte de la estructura de árbol visual.

Para obtener más información, vea la información general sobre el [uso de la capa visual con XAML](using-the-visual-layer-with-xaml.md) .

### <a name="working-with-your-desktop-app"></a>Trabajar con la aplicación de escritorio

Puede usar el nivel visual para mejorar la apariencia, el funcionamiento y la funcionalidad de las aplicaciones de escritorio de Win32, Windows Forms y C++. Puede migrar islas de contenido para usar la capa visual y mantener el resto de la interfaz de usuario en su marco de trabajo existente. De esta manera, puedes realizar actualizaciones y mejoras importantes en la interfaz de usuario de la aplicación sin necesidad de hacer cambios significativos en el código base que ya tienes.

Para más información, consulta [Modernización de una aplicación de escritorio mediante la capa visual](/windows/apps/desktop/modernize/visual-layer-in-desktop-apps).

## <a name="additional-resources"></a>Recursos adicionales

* [**Documentación de referencia completa de la API**](/uwp/api/Windows.UI.Composition)
* Muestras avanzadas de la interfaz de usuario y la composición en [WindowsUIDevLabs GitHub](https://github.com/microsoft/WindowsCompositionSamples).
* [Galería de ejemplos de Windows. UI. Composition](https://www.microsoft.com/store/apps/9pp1sb5wgnww)
* [@windowsui Fuente de Twitter ](https://twitter.com/windowsui)
* Artículo de MSDN de Kenny Kerr sobre esta API: [Gráficos y animación: La nueva composición para Windows 10](/archive/msdn-magazine/2015/windows-10-special-issue/graphics-and-animation-windows-composition-turns-10)