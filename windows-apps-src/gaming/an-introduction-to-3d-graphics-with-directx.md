---
title: Gráficos 3D básicos para juegos DirectX
description: Se muestra cómo usar la programación DirectX para implementar los conceptos fundamentales de los gráficos 3D.
ms.assetid: 2989c91f-7b45-7377-4e83-9daa0325e92e
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, juegos, DirectX, gráficos
ms.localizationpriority: medium
ms.openlocfilehash: 68ee694c036d4bc92f76ea75f7c5e5e530e26334
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173199"
---
# <a name="basic-3d-graphics-for-directx-games"></a>Gráficos 3D básicos para juegos DirectX



Se muestra cómo usar la programación DirectX para implementar los conceptos fundamentales de los gráficos 3D.

**Objetivo:** Aprender a programar una aplicación de gráficos 3D.

## <a name="prerequisites"></a>Requisitos previos


Suponemos que estás familiarizado con C++. También necesitas tener experiencia básica en los conceptos de programación de elementos gráficos.

**Tiempo total para completarse:** 30 minutos.

## <a name="where-to-go-from-here"></a>Cómo continuar a partir de aquí


Aquí, hablaremos sobre cómo desarrollar gráficos 3D con DirectX y C++ \\ CX. En este tutorial compuesto por cinco partes se presenta la API de [Direct3D](/windows/desktop/direct3d), además de conceptos y códigos que también se usan en muchas otras muestras de DirectX. Estas partes se estructuran entre sí, desde la configuración de DirectX de una aplicación para UWP con C++ hasta las texturas de primitivos y adición de efectos.

> **Nota:**    En este tutorial se usa un sistema de coordenadas de la mano derecha con vectores de columna. Muchas aplicaciones y muestras de DirectX usan un sistema de coordenadas a la izquierda con vectores de fila. Si quieres una solución matemática de gráficos completa que admita un sistema de coordenadas a la izquierda con vectores de fila, piensa en usar [DirectXMath](/windows/desktop/dxmath/directxmath-portal). Para más información, consulta [Usar DirectXMath con Direct3D](/windows/desktop/dxmath/pg-xnamath-migration-d3dx).

 

Te vamos a enseñar lo siguiente:

-   Inicializar interfaces de [Direct3D](/windows/desktop/direct3d) con Windows en tiempo de ejecución
-   Aplicar operaciones de sombreador por vértice
-   Configurar la geometría
-   Rasterizar la escena (aplanar la escena en 3D para una proyección en 2D)
-   Eliminar las superficies ocultas

> **Nota**  

 

A continuación, creamos un dispositivo Direct3D, una cadena de intercambio y una vista de destino de representación, y mostramos la imagen representada en la pantalla.

[Inicio rápido: configurar recursos de DirectX y mostrar una imagen](setting-up-directx-resources.md)

## <a name="related-topics"></a>Temas relacionados


* [Gráficos de Direct3D 11](/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11)
* [DXGI](/windows/desktop/direct3ddxgi/dx-graphics-dxgi)
* [HLSL](/windows/desktop/direct3dhlsl/dx-graphics-hlsl)

 

 