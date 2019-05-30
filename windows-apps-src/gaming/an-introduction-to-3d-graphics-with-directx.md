---
title: Gráficos 3D básicos para juegos DirectX
description: Se muestra cómo usar la programación DirectX para implementar los conceptos fundamentales de los gráficos 3D.
ms.assetid: 2989c91f-7b45-7377-4e83-9daa0325e92e
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, juegos, games, directx, gráficos, graphics
ms.localizationpriority: medium
ms.openlocfilehash: 556c5c74e5c8284e56047b4b8a9b2c2bf9c0155c
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66369076"
---
# <a name="basic-3d-graphics-for-directx-games"></a>Gráficos 3D básicos para juegos DirectX



Se muestra cómo usar la programación DirectX para implementar los conceptos fundamentales de los gráficos 3D.

**Objetivo:** Aprenda a programar una aplicación de gráficos 3D.

## <a name="prerequisites"></a>Requisitos previos


Suponemos que estás familiarizado con C++. También necesitas tener experiencia básica en los conceptos de programación de elementos gráficos.

**Tiempo total para completar:** 30 minutos.

## <a name="where-to-go-from-here"></a>Dónde ir desde aquí


En este caso, hablamos sobre cómo desarrollar gráficos 3D con DirectX y C++\\Cx. En este tutorial compuesto por cinco partes se presenta la API de [Direct3D](https://docs.microsoft.com/windows/desktop/direct3d), además de conceptos y códigos que también se usan en muchas otras muestras de DirectX. Estas partes se estructuran entre sí, desde la configuración de DirectX de una aplicación para UWP con C++ hasta las texturas de primitivos y adición de efectos.

> **Tenga en cuenta**  este tutorial usa un sistema de coordenadas para diestros con vectores de columna. Muchas aplicaciones y muestras de DirectX usan un sistema de coordenadas a la izquierda con vectores de fila. Si quieres una solución matemática de gráficos completa que admita un sistema de coordenadas a la izquierda con vectores de fila, piensa en usar [DirectXMath](https://docs.microsoft.com/windows/desktop/dxmath/directxmath-portal). Para más información, consulta [Usar DirectXMath con Direct3D](https://docs.microsoft.com/windows/desktop/dxmath/pg-xnamath-migration-d3dx).

 

Te vamos a enseñar lo siguiente:

-   Inicializar interfaces de [Direct3D](https://docs.microsoft.com/windows/desktop/direct3d) con Windows en tiempo de ejecución
-   Aplicar operaciones de sombreador por vértice
-   Configurar la geometría
-   Rasterizar la escena (aplanar la escena en 3D para una proyección en 2D)
-   Eliminar las superficies ocultas

> **Nota:**  

 

A continuación, creamos un dispositivo Direct3D, una cadena de intercambio y una vista de destino de representación, y mostramos la imagen representada en la pantalla.

[Inicio rápido: configuración de los recursos de DirectX y mostrar una imagen](setting-up-directx-resources.md)

## <a name="related-topics"></a>Temas relacionados


* [Direct3D 11 gráficos](https://docs.microsoft.com/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11)
* [DXGI](https://docs.microsoft.com/windows/desktop/direct3ddxgi/dx-graphics-dxgi)
* [HLSL](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl)

 

 




