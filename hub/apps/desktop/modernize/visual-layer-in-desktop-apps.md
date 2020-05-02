---
title: Modernizar una aplicación de escritorio mediante la capa visual
description: Usa la capa visual para mejorar la interfaz de usuario de tu aplicación de escritorio de .NET o Win32.
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: jimwalk
author: jwmsft
ms.localizationpriority: medium
ms.openlocfilehash: 249291c59a31036fa967ac338209404557b57503
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "66215176"
---
# <a name="using-the-visual-layer-in-desktop-apps"></a>Usar la capa visual en aplicaciones de escritorio

Ahora puedes usar las API de UWP en aplicaciones de escritorio que no son de UWP para mejorar la apariencia y funcionalidad de las aplicaciones de WPF, Windows Forms y C++ Win32, y aprovechar las ventajas de las características más recientes de la interfaz de usuario de Windows 10 que solo están disponibles a través de UWP.

En muchos casos, puedes usar [islas XAML](xaml-islands.md) para agregar controles XAML modernos a la aplicación. Sin embargo, cuando necesites crear experiencias personalizadas que vayan más allá de los controles integrados de UWP, puedes acceder mediante las API de capa visual.

La capa visual proporciona una API de modo retenido y alto rendimiento para gráficos, efectos y animaciones. Es la base de la interfaz de usuario de todos los dispositivos Windows 10. Los controles XAML de UWP se basan en la capa visual y habilitan muchos aspectos del [sistema Fluent Design](/windows/uwp/design/fluent-design-system/index), tales como luz, profundidad, movimiento, material y escala.

![Interfaz de usuario creada con la capa visual](images/visual-layer-interop/pull-to-animate.gif)

> _Interfaz de usuario creada con la capa visual_

## <a name="create-a-visually-engaging-user-interface-in-any-windows-app"></a>Crear una interfaz de usuario visualmente atractiva en cualquier aplicación de Windows

La capa visual permite crear experiencias atractivas mediante una composición ligera de contenido dibujado personalizado (objetos visuales) y la aplicación de animaciones, efectos y manipulaciones en esos objetos de la aplicación. La capa visual no reemplaza ningún marco de trabajo de la interfaz de usuario actual; es un complemento valioso para esos marcos de trabajo.

Puedes usar la capa visual para dar a la aplicación una apariencia única y establecer una identidad que la diferencie de otras aplicaciones. También habilita los principios de Fluent Design, ideados para facilitar el uso de tus aplicaciones y atraer más a los usuarios. Por ejemplo, puedes utilizarlo para crear indicaciones visuales y transiciones de pantalla animadas que muestren las relaciones entre los elementos de la pantalla.

## <a name="visual-layer-features"></a>Características de la capa visual

### <a name="brushes"></a>Pinceles

Los [pinceles de Composition](/windows/uwp/composition/composition-brushes) permiten pintar objetos de interfaz de usuario con colores sólidos, degradados, imágenes, vídeos, efectos complejos, etc.

![Un huevo creado con Material Creator](images/visual-layer-interop/egg.gif)

> _Un huevo creado con la [aplicación de demostración Material Creator](https://github.com/Microsoft/WindowsCompositionSamples/tree/master/Demos/MaterialCreator)._

### <a name="effects"></a>Efectos

Los [efectos de Composition](/windows/uwp/composition/composition-effects) incluyen luz, sombra y una lista de efectos de filtro. Se pueden animar, personalizar y encadenar y, después, aplicar directamente a los objetos visuales. SceneLightingEffect se puede combinar con la iluminación de la composición para crear una atmósfera, profundidad y materiales.

![Luces y materiales](images/visual-layer-interop/light-interop.gif)

> _Luces y materiales que se demuestran en la [galería de ejemplos de Windows.UI.Composition](https://github.com/Microsoft/WindowsCompositionSamples/tree/master/SampleGallery)._

### <a name="animations"></a>Animaciones

Las [animaciones de Composition](/windows/uwp/composition/composition-animation) se ejecutan directamente en el proceso del compositor, independientemente del subproceso de la interfaz de usuario. Esto garantiza fluidez y escalado, para que puedas ejecutar un gran número de animaciones explícitas simultáneas. Además de las animaciones KeyFrame conocidas para controlar los cambios de propiedad a lo largo del tiempo, puedes usar expresiones para configurar las relaciones matemáticas entre las distintas propiedades, incluida la entrada del usuario. Las animaciones controladas por la entrada permiten crear una interfaz de usuario que responde de forma dinámica y fluida a la entrada del usuario, lo que mejora la interacción con el usuario.

![Interfaz de usuario creada con la capa visual](images/visual-layer-interop/swipe-scroller.gif)

> _Movimiento que se demuestra en la [galería de ejemplos de Windows.UI.Composition](https://github.com/Microsoft/WindowsCompositionSamples/tree/master/SampleGallery)._

## <a name="keep-your-existing-codebase-and-adopt-incrementally"></a>Conserva tu código base actual y haz la adopción de forma incremental

El código de las aplicaciones existentes es una inversión que ya has hecho y que no quieres perder. Puedes migrar _islas_ de contenido para usar la capa visual y mantener el resto de la interfaz de usuario en tu marco de trabajo actual. De esta manera, puedes realizar actualizaciones y mejoras importantes en la interfaz de usuario de la aplicación sin necesidad de hacer cambios significativos en el código base que ya tienes.

## <a name="samples-and-tutorials"></a>Ejemplos y tutoriales

Practica con nuestros ejemplos para aprender a usar la capa visual en tus aplicaciones. Estos ejemplos y tutoriales te ayudarán a empezar a usar la capa visual y te mostrarán cómo funcionan las características.

### <a name="win32"></a>Win32

- Tutorial [Usar la capa visual con Win32](using-the-visual-layer-with-win32.md)
  - [Ejemplo HelloComposition](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloComposition)
- [Ejemplo HelloVectors](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloVectors)
- [Ejemplo de superficies virtuales](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/VirtualSurfaces)
- [Ejemplo de captura de pantalla](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/ScreenCaptureforHWND)

### <a name="windows-forms"></a>Windows Forms

- Tutorial [Usar la capa visual con Windows Forms](using-the-visual-layer-with-windows-forms.md)
  - [Ejemplo HelloComposition](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/HelloComposition)
- [Ejemplo de integración de la capa visual](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/VisualLayerIntegration)

### <a name="wpf"></a>WPF

- Tutorial [Usar la capa visual con WPF](using-the-visual-layer-with-wpf.md)
  - [Ejemplo HelloComposition](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/HelloComposition)
- [Ejemplo de integración de la capa visual](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/VisualLayerIntegration)
- [Ejemplo de captura de pantalla](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/ScreenCapture)

## <a name="limitations"></a>Limitaciones

Aunque muchas características de la capa visual funcionan igual cuando se hospedan en una aplicación de escritorio que en una aplicación de UWP, algunas características tienen limitaciones. Existen algunas limitaciones que hay que tener en cuenta, por ejemplo:

- Las cadenas de efectos se basan en [Win2D](http://microsoft.github.io/Win2D/html/Introduction.htm) para las descripciones de los efectos. El [paquete NuGet de Win2D](https://www.nuget.org/packages/Win2D.uwp) no se admite en las aplicaciones de escritorio, por lo que tendrás que volver a compilarlo a partir del [código fuente](https://github.com/Microsoft/Win2D).
- Para la prueba de aciertos, tienes que calcular los límites recorriendo el árbol de objetos visuales. Esto es lo mismo que la capa visual en UWP, excepto que, en este caso, no hay ningún elemento XAML con el que puedas enlazar fácilmente para la prueba de aciertos.
- La capa visual no tiene una primitiva para representar texto.
- Cuando se usan juntas dos tecnologías de interfaz de usuario diferentes, como WPF y la capa visual, cada una es responsable de dibujar sus propios píxeles en la pantalla y no pueden compartir píxeles. Como resultado, el contenido de la capa visual siempre se representa encima de otro contenido de la interfaz de usuario. (Esto se conoce como el problema de _espacio aéreo_). Puede que tengas que hacer pruebas y crear código adicional para asegurarte de que el contenido de la capa visual cambie de tamaño con la interfaz de usuario del host y no tape otro contenido.
- El contenido hospedado en una aplicación de escritorio no cambia de tamaño y el valor de ppp no se escala automáticamente. Puede que tengas que dar algunos pasos adicionales para asegurarte de que el contenido controle los cambios de ppp. (Consulta los tutoriales específicos de la plataforma para más información).

## <a name="additional-resources"></a>Recursos adicionales

- [Capa visual](/windows/uwp/composition/visual-layer)
- [Objeto visual de Composition](/windows/uwp/composition/composition-visual-tree)
- [Pinceles de Composition](/windows/uwp/composition/composition-brushes)
- [Efectos de Composition](/windows/uwp/composition/composition-effects)
- [Animaciones de Composition](/windows/uwp/composition/composition-animation)

Referencia de la API

- [Windows.UI.Composition](/uwp/api/Windows.UI.Composition)