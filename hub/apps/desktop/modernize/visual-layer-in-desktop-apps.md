---
title: Modernice sus aplicaciones de escritorio mediante la capa Visual
description: Utilice la capa Visual para mejorar la interfaz de usuario de la aplicación de escritorio de .NET o Win32.
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 042e34b8d09c2bae5cefb4227ef2a104a43861a6
ms.sourcegitcommit: f0f933d5cf0be734373a7b03e338e65000cc3d80
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65984975"
---
# <a name="using-the-visual-layer-in-desktop-apps"></a>Uso de la capa Visual en aplicaciones de escritorio

Ahora puede usar las API de UWP en aplicaciones de escritorio no de UWP para mejorar la apariencia, sensación y funcionalidad de su WPF, Windows Forms, y C++ aplicaciones Win32 y aprovechar las ventajas de las características más recientes de la interfaz de usuario de Windows 10 que solo están disponibles a través de UWP.

Para muchos escenarios, puede usar [Islas XAML](xaml-islands.md) para agregar controles XAML modernos a la aplicación. Sin embargo, cuando necesite crear experiencias personalizadas que van más allá de los controles integrados, puede tener acceso a la API de la capa Visual.

La capa Visual proporciona un alto rendimiento, la API de modo retenido para gráficos, efectos y animaciones. Es la base para la interfaz de usuario en dispositivos Windows 10. Controles de UWP XAML se basan en la capa Visual, y permite muchos aspectos de la [sistema de diseño Fluent](/windows/uwp/design/fluent-design-system/index), como la luz, profundidad, movimiento, Material y escala.

![Interfaz de usuario creada con la capa visual](images/visual-layer-interop/pull-to-animate.gif)

> _Interfaz de usuario creada con la capa visual_

## <a name="create-a-visually-engaging-user-interface-in-any-windows-app"></a>Crear una interfaz de usuario visualmente atractivas en cualquier aplicación de Windows

La capa Visual le permite crear atractivas experiencias mediante composición ligera de contenido dibujado personalizado (objetos visuales) y aplicar animaciones eficaces, efectos y manipulaciones en dichos objetos en la aplicación. La capa Visual no reemplaza cualquier marco de interfaz de usuario existente; en su lugar, es un complemento valioso para esos marcos.

Puede usar la capa Visual para dar una apariencia única a la aplicación y establecer una identidad que lo distingue de otras aplicaciones. También permite a los principios de diseño Fluent, que están diseñados para que sus aplicaciones más fáciles de usar, interacción de dibujo de los usuarios. Por ejemplo, puede usar para crear las indicaciones visuales y transiciones de pantalla animadas que muestran relaciones entre elementos en la pantalla.

## <a name="visual-layer-features"></a>Características de la capa Visual

### <a name="brushes"></a>Pinceles

[Pinceles de composición](/windows/uwp/composition/composition-brushes) permiten pintar objetos de interfaz de usuario con colores sólidos, degradados, imágenes, vídeos, efectos complejos y mucho más.

![Un huevo creado con el creador de Material](images/visual-layer-interop/egg.gif)

> _Un huevo creado con el [aplicación de demostración de creador de Material](https://github.com/Microsoft/WindowsCompositionSamples/tree/master/Demos/MaterialCreator)._

### <a name="effects"></a>Efectos

[Efectos de composición](/windows/uwp/composition/composition-effects) incluyen luz, instantáneas y una lista de los efectos de filtro. Puede ser animados, personalizados y encadenadas y luego aplicar directamente a los objetos visuales. El SceneLightingEffect puede combinarse con la iluminación de composición para crear atmósfera, profundidad y materiales.

![Las luces y material](images/visual-layer-interop/light-interop.gif)

> _Las luces y material que se muestran en el [Galería de ejemplos de composición de interfaz de usuario de Windows](https://github.com/Microsoft/WindowsCompositionSamples/tree/master/SampleGallery)._

### <a name="animations"></a>Animaciones

[Animaciones de composición](/windows/uwp/composition/composition-animation) ejecutar directamente en el proceso de compositor, independiente del subproceso de interfaz de usuario. Esto garantiza la suavidad y la escala para que pueda ejecutar grandes cantidades de animaciones simultáneas, explícitas. Además de animaciones de fotogramas clave familiares a los cambios de propiedad de unidad con el tiempo, puede utilizar expresiones para establecer relaciones matemáticas entre las diferentes propiedades, proporcionados por el usuario. Animaciones controlado por entrada le permiten crear la interfaz de usuario de forma dinámica y fluidez responde a la entrada del usuario, que puede dar lugar a mayor participación de los usuarios.

![Interfaz de usuario creada con la capa visual](images/visual-layer-interop/swipe-scroller.gif)

> _Movimiento se muestra en el [Galería de ejemplos de composición de interfaz de usuario de Windows](https://github.com/Microsoft/WindowsCompositionSamples/tree/master/SampleGallery)._

## <a name="keep-your-existing-codebase-and-adopt-incrementally"></a>Mantener el código base existente y adoptar de forma incremental

El código en las aplicaciones existentes representa una inversión importante que no desea perder. Puede migrar _Islas_ de contenido para utilizar la capa Visual y mantener el resto de la interfaz de usuario en su marco de trabajo existente. Esto significa que puede realizar actualizaciones importantes y mejoras en la interfaz de usuario de la aplicación sin necesidad de realizar grandes cambios en el código existente base.

## <a name="samples-and-tutorials"></a>Ejemplos y tutoriales

Obtenga información sobre cómo usar la capa Visual en sus aplicaciones experimentando con nuestros ejemplos. Estos ejemplos y tutoriales le ayudan a empezar a usar la capa Visual y mostrarle cómo funcionan las características.

### <a name="win32"></a>Win32

- [Uso de la capa Visual con Win32](using-the-visual-layer-with-win32.md) tutorial
  - [Ejemplo de composición de Hello](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloComposition)
- [Ejemplo de Hello vectores](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloVectors)
- [Ejemplo de superficies virtual](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/VirtualSurfaces)
- [Ejemplo de captura de pantalla](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/ScreenCaptureforHWND)

### <a name="windows-forms"></a>Windows Forms

- [Uso de la capa Visual con Windows Forms](using-the-visual-layer-with-windows-forms.md) tutorial
  - [Ejemplo de composición de Hello](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/HelloComposition)
- [Ejemplo de integración de la capa Visual](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WinForms/VisualLayerIntegration)

### <a name="wpf"></a>WPF

- [Uso de la capa Visual con WPF](using-the-visual-layer-with-wpf.md) tutorial
  - [Ejemplo de composición de Hello](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/HelloComposition)
- [Ejemplo de integración de la capa Visual](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/VisualLayerIntegration)
- [Ejemplo de captura de pantalla](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/dotnet/WPF/ScreenCapture)

## <a name="limitations"></a>Limitaciones

Mientras que muchas características de la capa Visual funcionan igual cuando se hospeda en una aplicación de escritorio como lo hacen en una aplicación para UWP, algunas características tienen limitaciones. Estas son algunas de las limitaciones a tener en cuenta:

- Dependen de las cadenas de efecto [Win2D](http://microsoft.github.io/Win2D/html/Introduction.htm) para las descripciones del efecto. El [paquete Win2D NuGet](https://www.nuget.org/packages/Win2D.uwp) no se admite en aplicaciones de escritorio, por lo que tendría que volver a compilarlo desde la [código fuente](https://github.com/Microsoft/Win2D).
- Para la prueba de posicionamiento, deberá realizar los cálculos de límites recorriendo el árbol visual usted mismo. Esto es igual que la capa Visual en UWP, excepto que en este caso, no hay ningún elemento XAML que puede enlazar fácilmente a prueba de posicionamiento.
- La capa Visual no tiene un tipo primitivo para representar texto.
- Cuando se utilizan dos tecnologías diferentes de interfaz de usuario, como WPF y la capa Visual, son cada uno de ellos responsable de dibujar sus propios píxeles en la pantalla y no pueden compartir píxeles. Como resultado, el contenido de la capa Visual siempre se representa por encima de otro contenido de la interfaz de usuario. (Esto se conoce como el _espacio aéreo_ problema.) Es posible que deba escribir código adicional y pruebas para garantizar el contenido de la capa Visual cambia el tamaño con el host de la interfaz de usuario y no occlude otro contenido.
- Contenido hospedado en una aplicación de escritorio automáticamente no cambiar el tamaño o ajustar la escala de PPP. Es posible que requieren pasos adicionales para asegurarse de que el contenido controla los cambios de PPP. (Vea los tutoriales específicos de plataforma para obtener más información).

## <a name="additional-resources"></a>Recursos adicionales

- [Capa visual](/windows/uwp/composition/visual-layer)
- [Composición visual](/windows/uwp/composition/composition-visual-tree)
- [Pinceles de composición](/windows/uwp/composition/composition-brushes)
- [Efectos de composición](/windows/uwp/composition/composition-effects)
- [Animaciones de composición](/windows/uwp/composition/composition-animation)

Referencia de la API

- [Windows.UI.Composition](/uwp/api/Windows.UI.Composition)