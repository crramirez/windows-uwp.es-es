---
author: mtoepke
title: Herramientas de Visual Studio para programación de juegos
description: Aquí encontrarás información general acerca de las herramientas específicas de DirectX disponibles en Visual Studio.
ms.assetid: 43137bfc-7876-70e0-515c-4722f68bd064
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, juegos, visual studio, herramientas, directx
ms.localizationpriority: medium
ms.openlocfilehash: eec406fd317abbd0034ba573cc0e791f9e32ba98
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2018
ms.locfileid: "5563685"
---
# <a name="visual-studio-tools-for-game-programming"></a>Herramientas de Visual Studio para la programación de juegos



**Resumen**

-   [Crear un proyecto de juego DirectX con una plantilla](user-interface.md)
-   Visual Studio tools para programación de juegos DirectX


Si usas Visual Studio Ultimate para desarrollar aplicaciones de DirectX, hay herramientas adicionales disponibles para crear, editar, obtener una vista previa y exportar una imagen, modelo y recursos de sombreador. También puedes usar herramientas para convertir recursos durante la compilación y depurar el código de gráficos de DirectX.

En este tema encontrarás una introducción a estas herramientas de gráficos.

## <a name="image-editor"></a>Editor de imágenes


Usa el Editor de imágenes para trabajar con los tipos de textura enriquecida y formatos de imagen que DirectX usa. El Editor de imágenes admite los siguientes formatos:

-   .png
-   .jpg, .jpeg, .jpe, .jfif
-   .dds
-   .gif
-   .bmp
-   .dib
-   .tif, .tiff
-   .tga

Crea [archivos de personalizaciones de compilación](#build-customizations-for-3d-assets) para convertir estos formatos en archivos .dds durante la compilación.

Para obtener más información, consulta [Trabajar con texturas e imágenes](https://msdn.microsoft.com/library/windows/apps/hh873119.aspx).

> **Nota**el Editor de imágenes no está pensado para reemplazar una aplicación de edición de imágenes completo de características, pero es apropiado para escenarios de edición y visualización simple muchos.

 

## <a name="model-editor"></a>Editor de modelos


Puedes usar el Editor de modelos para crear modelos básicos 3D desde cero, o para ver y modificar modelos 3D más complejos en herramientas de modelos 3D completas. El Editor de modelos admite varios formatos de modelos 3D que se usan en el desarrollo de aplicaciones DirectX. Puedes crear [archivos de personalizaciones de compilación](#build-customizations-for-3d-assets) para convertir estos formatos en archivos .cmo durante la compilación.

-   .fbx
-   .dae
-   .obj

Esta es la captura de pantalla de un modelo en el editor con iluminación aplicada.

![tetera](images/modeleditor.png)

Para obtener más información, consulta [Trabajar con modelos 3D](https://msdn.microsoft.com/library/windows/apps/hh873114.aspx).

> **Nota**el Editor de modelos no está pensado para reemplazar una aplicación de edición de modelos completo de características, pero es apropiado para escenarios de edición y visualización simple muchos.

 

## <a name="shader-designer"></a>Diseñador de sombras


Usa el Diseñador de sombras para crear efectos visuales personalizados para tu juego o aplicación, aun cuando no sepas programar en HLSL.

Creas un sombreador visualmente como un gráfico. Cada nodo muestra una vista previa del resultado de esa operación. Este es un ejemplo que aplica la iluminación Lambert con una vista previa de esfera.

![gráfico de sombreador visual](images/shaderdesigner.png)

Usa el Editor de sombras para diseñar, editar y guardar sombreadores en el formato .dgsl. También exporta los siguientes formatos.

-   .hlsl (código fuente)
-   .cso (código de bytes)
-   .h (matriz de código de bytes HLSL)

Crea [archivos de personalizaciones de compilación](#build-customizations-for-3d-assets) para convertir estos formatos en archivos .cso durante la compilación.

Esta es una parte del código HLSL que el Editor de sombras exporta. Este solo es el código para el nodo de iluminación Lambert.

```hlsl
//
// Lambert lighting function
//
float3 LambertLighting(
    float3 lightNormal,
    float3 surfaceNormal,
    float3 materialAmbient,
    float3 lightAmbient,
    float3 lightColor,
    float3 pixelColor
    )
{
    // Compute the amount of contribution per light.
    float diffuseAmount = saturate(dot(lightNormal, surfaceNormal));
    float3 diffuse = diffuseAmount * lightColor * pixelColor;

    // Combine ambient with diffuse.
    return saturate((materialAmbient * lightAmbient) + diffuse);
}
```

Para obtener más información, consulta [Trabajar con sombreadores](https://msdn.microsoft.com/library/windows/apps/hh873117.aspx).

## <a name="build-customizations-for-3d-assets"></a>Personalizaciones de compilación para activos 3D


Puedes agregar personalizaciones de compilación a tu proyecto para que Visual Studio convierta recursos en formatos utilizables. A continuación, puedes cargar los activos en tu aplicación y usarlos al crear y llenar recursos de DirectX tal como lo harías en cualquier otra aplicación de DirectX.

Para agregar una personalización de compilación, haz clic en el proyecto en el **Explorador de soluciones** y selecciona la **Compilación personalizaciones**. Puedes agregar los siguientes tipos de personalizaciones de compilación al proyecto.

-   La canalización de contenido de imagen toma archivos de imagen como entrada y muestra archivos de DirectDraw Surface (.dds) como salida.
-   La canalización de contenido de malla toma archivos de malla (como .fbx) y muestra archivos de malla .cmo como salida.
-   La canalización de contenido de sombreador toma un gráfico de sombreador visual (.dgsl) del Editor de sombras de Visual Studio y muestra un archivo de Compiled Shader Output (.cso).

Para obtener más información, consulta [Usar activos 3D en el juego o aplicación](https://msdn.microsoft.com/library/windows/apps/hh972446.aspx).

## <a name="debugging-directx-graphics"></a>Depurar gráficos DirectX 


Visual Studio proporciona herramientas de depuración específicas para gráficos. Usa estas herramientas para depurar cosas como estas:

-   La canalización de gráficos.
-   La pila de llamadas de eventos.
-   La tabla de objetos.
-   El estado del dispositivo.
-   Errores de sombreador.
-   Parámetros y búferes de constantes incorrectos o no inicializados.
-   Compatibilidad de la versión de DirectX.
-   Compatibilidad limitada con Direct2D.
-   Requisitos de SDK y sistema operativo

Para obtener más información, consulta [Depurar gráficos DirectX](https://msdn.microsoft.com/library/windows/apps/hh315751.aspx).


 

 

 




