---
author: scottmill
ms.assetid: a2751e22-6842-073a-daec-425fb981bafe
title: Capa visual
description: "La API de Windows.UI.Composition proporciona acceso a la capa de composición entre la capa de marco (XAML) y la capa de elementos gráficos (DirectX)."
translationtype: Human Translation
ms.sourcegitcommit: 4a00847f0559d93eea199d7ddca0844b5ccaa5aa
ms.openlocfilehash: 3a3dbf7b529d5d2848b161869d2f77fef3651488

---
# Capa visual

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

En Windows 10, se ha dedicado mucho trabajo a la creación de un nuevo compositor unificado y motor de representación para todas las aplicaciones Windows, ya sean de escritorio o para móviles. Un resultado de ese trabajo fue la API de composición de WinRT unificada denominada Windows.UI.Composition que ofrece acceso a los nuevos objetos de composición ligeros, junto con nuevas animaciones y efectos impulsadas por Compositor.

Windows.UI.Composition es una API declarativa de [modo retenido](https://msdn.microsoft.com/library/windows/desktop/ff684178.aspx) que se puede llamar desde cualquier aplicación para la Plataforma universal de Windows (UWP) con el fin de crear objetos de composición, animaciones y efectos directamente en una aplicación. La API es un eficaz complemento para los marcos existentes, como XAML, que ofrece a los desarrolladores de aplicaciones para UWP una superficie C# familiar para agregar a su aplicación. Estas API también se pueden usar para crear aplicaciones sin marco de estilo DX.

Un desarrollador de XAML puede "bajar" a la capa de composición en C# para realizar trabajos personalizados en la capa de composición con WinRT, en lugar de descender hasta el fondo, a la capa de elementos gráficos, y usar DirectX y C++ para el trabajo personalizado de la interfaz de usuario. Esta técnica puede usarse para animar un elemento existente con las API de composición o para aumentar una interfaz de usuario mediante la creación de una "isla visual" de contenido de Windows.UI.Composition dentro del árbol de elementos de XAML.

![](images/layers-win-ui-composition.png)
## <span id="Composition_Objects_and_The_Compositor"></span><span id="composition_objects_and_the_compositor"></span><span id="COMPOSITION_OBJECTS_AND_THE_COMPOSITOR"></span>Objetos de composición y el objeto Compositor

Los objetos de composición los crea el objeto [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Dn706789), que actúa como una fábrica de objetos de composición. El compositor puede crear objetos [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858), que permiten la creación de una estructura de árbol visual que todas las demás funciones y objetos de la API usan y en la que se basan.

La API permite a los desarrolladores definir y crear uno o varios objetos [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) que representan un nodo único en un árbol visual.

Los elementos visuales pueden ser contenedores de otros elementos visuales o pueden hospedar elementos visuales de contenido. La API facilita el uso al ofrecer un conjunto claro de objetos [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) para tareas específicas que existen en una jerarquía de objetos:

-   [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858): El objeto base. La mayoría de las propiedades están aquí y las heredan los otros elementos visuales.
-   [**ContainerVisual**](https://msdn.microsoft.com/library/windows/apps/Dn706810): Se deriva del objeto [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) y agrega la capacidad de insertar elementos visuales secundarios.
-   [**SpriteVisual**](https://msdn.microsoft.com/library/windows/apps/Mt589433): Se deriva del objeto [**ContainerVisual**](https://msdn.microsoft.com/library/windows/apps/Dn706810) e incluye contenido en forma de imágenes, efectos y cadenas de intercambio.
-   [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Dn706789): Es la fábrica de objetos que administra la relación entre una aplicación y el proceso del compositor del sistema.

El compositor también es una fábrica de varios otros objetos de composición que se usan para recortar o transformar elementos visuales en el árbol, así como un amplio conjunto de animaciones y efectos de transformación.

## <span id="Effects_System"></span><span id="effects_system"></span><span id="EFFECTS_SYSTEM"></span>Sistema de efectos

Windows.UI.Composition admite efectos en tiempo real que se pueden animar, personalizar y encadenar. Entre los efectos se incluyen transformaciones afines 2D, composiciones aritméticas, combinaciones, origen del color, compuesto, contraste, exposición, escala de grises, transferencia gamma, girar matiz, invertir, saturar, sepia, temperatura y tono.

Para más información, consulta la información general sobre los [efectos de composición](composition-effects.md).

## <span id="Animation_System"></span><span id="animation_system"></span><span id="ANIMATION_SYSTEM"></span>Sistema de animación

Windows.UI.Composition contiene un sistema de animación expresivo independiente de marcos que te permite configurar dos tipos de animaciones: animaciones de fotograma clave y animaciones de expresión. Estos se usan para mover objetos visuales, impulsar una transformación o recorte, o animar un efecto. Al ejecutarlo directamente en el proceso de compositor, esto garantiza un procesamiento suave y escalabilidad, lo que permite ejecutar grandes cantidades de animaciones simultáneas y únicas.

Para más información, consulta la información general sobre las [animaciones de composición](composition-animation.md).

## <span id="XAML_Interoperation"></span><span id="xaml_interoperation"></span><span id="XAML_INTEROPERATION"></span>Interoperación XAML

Además de crear un árbol visual desde cero, la API de composición puede interactuar con una interfaz de usuario XAML existente mediante la clase [**ElementCompositionPreview**](https://msdn.microsoft.com/library/windows/apps/Mt608976) de [**Windows.UI.Xaml.Hosting**](https://msdn.microsoft.com/library/windows/apps/Hh701908).

- [**ElementCompositionPreview.GetElementVisual()**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual): Obtén el elemento visual de respaldo de un elemento para animarlo con las API de composición.
- [**ElementCompositionPreview.SetChildVisual()**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.hosting.elementcompositionpreview.setelementchildvisual): Agrega una "isla visual" de contenido de composición a un árbol XAML.
- [**ElementCompositionPreview.GetScrollViewerManipulationPropertySet()**](https://msdn.microsoft.com/library/windows/apps/mt608980.aspx): Usa la manipulación de una clase [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx) como entrada para una animación de composición.


**Nota**  
Este artículo está orientado a desarrolladores de Windows10 que programen aplicaciones para la Plataforma universal de Windows (UWP). Si estás desarrollando para Windows8.x o Windows Phone8.x, consulta la [documentación archivada](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

## <span id="Additional_Resources_"></span><span id="additional_resources_"></span><span id="ADDITIONAL_RESOURCES_"></span>Recursos adicionales:

-   Artículo de MSDN de Kenny Kerr sobre esta API: [Gráficos y animación: La nueva composición para Windows10](https://msdn.microsoft.com/magazine/mt590968)
-   Muestras avanzadas de la interfaz de usuario y la composición en [WindowsUIDevLabs GitHub](https://github.com/microsoft/windowsuidevlabs).
-   [**Documentación de referencia completa de la API**](https://msdn.microsoft.com/library/windows/apps/Dn706878).
-   Problemas conocidos: [Problemas conocidos](http://go.microsoft.com/fwlink/?LinkId=823237).

 

 







<!--HONumber=Aug16_HO3-->


