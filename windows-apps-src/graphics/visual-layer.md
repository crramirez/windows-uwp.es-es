---
author: scottmill
ms.assetid: a2751e22-6842-073a-daec-425fb981bafe
title: Capa visual
description: La API de Windows.UI.Composition proporciona acceso a la capa de composición entre la capa de marco (XAML) y la capa de elementos gráficos (DirectX).
---
# Capa visual

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

En Windows 10, se ha dedicado mucho trabajo a la creación de un nuevo compositor unificado y motor de representación para todas las aplicaciones Windows, ya sean de escritorio o para móviles. Un resultado de ese trabajo fue la API de composición de WinRT unificada denominada Windows.UI.Composition que ofrece acceso a los nuevos objetos de composición ligeros, junto con nuevas animaciones y efectos impulsadas por Compositor.

Windows.UI.Composition es una API declarativa de [modo retenido](https://msdn.microsoft.com/library/windows/desktop/ff684178.aspx) que se puede llamar desde cualquier aplicación para la Plataforma universal de Windows (UWP) con el fin de crear objetos de composición, animaciones y efectos directamente en una aplicación. La API es un eficaz complemento de marcos existentes, como XAML, que ofrece a los desarrolladores de aplicaciones para UWP una superficie C# familiar para agregar a su aplicación. Estas API se pueden usar para crear aplicaciones sin marco de estilo DX.

Un desarrollador de XAML puede "desplegar" a la capa de composición en C# para realizar trabajos personalizados en el nivel de composición con WinRT y crear una "isla de composición" de objetos en su aplicación XAML en lugar de bajar hasta la capa de elementos gráficos y usar DirectX y C++ para el trabajo de interfaz de usuario personalizada.

![](images/layers-win-ui-composition.png)
## <span id="Composition_Objects_and_The_Compositor"></span><span id="composition_objects_and_the_compositor"></span><span id="COMPOSITION_OBJECTS_AND_THE_COMPOSITOR"></span>Objetos de composición y el objeto Compositor

Los objetos de composición los crea el objeto [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Dn706789), que actúa como una fábrica de objetos de composición. El compositor puede crear objetos [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858), que permiten la creación de una estructura de árbol visual que todas las demás funciones y objetos de la API usan y en la que se basan.

La API permite a los desarrolladores definir y crear uno o varios objetos [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) que representan un nodo único en un árbol visual.

Los elementos visuales pueden ser contenedores de otros elementos visuales o pueden hospedar elementos visuales de contenido. La API facilita el uso al ofrecer un conjunto claro de objetos [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) para tareas específicas que existen en una jerarquía de objetos:

-   [
              **Visual**
            ](https://msdn.microsoft.com/library/windows/apps/Dn706858): el objeto base. La mayoría de las propiedades están aquí y las heredan los otros elementos visuales.
-   [
              **ContainerVisual**
            ](https://msdn.microsoft.com/library/windows/apps/Dn706810): se deriva del objeto [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) y agrega la capacidad de insertar elementos visuales secundarios.
-   [
              **SpriteVisual**
            ](https://msdn.microsoft.com/library/windows/apps/Mt589433): se deriva del objeto [**ContainerVisual**](https://msdn.microsoft.com/library/windows/apps/Dn706810) e incluye contenido en forma de imágenes, efectos y cadenas de intercambio.
-   [
              **Compositor**
            ](https://msdn.microsoft.com/library/windows/apps/Dn706789): es la fábrica de objetos que administra la relación entre una aplicación y el proceso de compositor del sistema.

El compositor también es una fábrica de varios otros objetos de composición que se usan para recortar o transformar elementos visuales en el árbol, así como un amplio conjunto de animaciones y efectos de transformación.

## <span id="Effects_System"></span><span id="effects_system"></span><span id="EFFECTS_SYSTEM"></span>Sistema de efectos

Windows.UI.Composition admite efectos en tiempo real que se pueden animar, personalizar y encadenar. Entre los efectos se incluyen transformaciones afines 2D, composiciones aritméticas, combinaciones, origen del color, compuesto, contraste, exposición, escala de grises, transferencia gamma, girar matiz, invertir, saturar, sepia, temperatura y tono.

Para más información, consulta la información general sobre los [efectos de composición](composition-effects.md).

## <span id="Animation_System"></span><span id="animation_system"></span><span id="ANIMATION_SYSTEM"></span>Sistema de animación

Windows.UI.Composition contiene un sistema de animación expresivo independiente de marcos que te permite configurar dos tipos de animaciones: animaciones de fotograma clave y animaciones de expresión. Estos se usan para mover objetos visuales, impulsar una transformación o recorte, o animar un efecto. Al ejecutarlo directamente en el proceso de compositor, esto garantiza un procesamiento suave y escalabilidad, lo que permite ejecutar grandes cantidades de animaciones simultáneas y únicas.

Para más información, consulta la información general sobre las [animaciones de composición](composition-animation.md).

## <span id="XAML_Interoperation"></span><span id="xaml_interoperation"></span><span id="XAML_INTEROPERATION"></span>Interoperación XAML

Además de crear un árbol visual desde cero, la API de composición puede interactuar con una interfaz de usuario XAML existente mediante la clase [**ElementCompositionPreview**](https://msdn.microsoft.com/library/windows/apps/Mt608976) de [**Windows.UI.Xaml.Hosting**](https://msdn.microsoft.com/library/windows/apps/Hh701908).


**Nota**  
Este artículo está orientado a desarrolladores de Windows 10 que programan aplicaciones para la Plataforma universal de Windows (UWP). Si desarrollas para Windows 8.x o Windows Phone 8.x, consulta la [documentación archivada](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

## <span id="Additional_Resources_"></span><span id="additional_resources_"></span><span id="ADDITIONAL_RESOURCES_"></span>Recursos adicionales:

-   Artículo de MSDN de Kenny Kerr sobre esta API, [Graphics and Animation - Windows Composition Turns 10 (Gráficos y animación: La nueva composición para Windows 10)](https://msdn.microsoft.com/magazine/mt590968)
-   Muestras de composición en [Composition GitHub](https://github.com/Microsoft/composition).
-   Documentación de referencia completa de la API
-   Problemas conocidos: [Problemas conocidos](https://social.msdn.microsoft.com/Forums/en-US/home?forum=Win10SDKToolsIssues).

 

 






<!--HONumber=May16_HO2-->


