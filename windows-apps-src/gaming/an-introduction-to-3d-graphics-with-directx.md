---
title: Gráficos 3D básicos para juegos DirectX
description: Se muestra cómo usar la programación en DirectX para implementar los conceptos fundamentales de los gráficos 3D.
ms.assetid: 2989c91f-7b45-7377-4e83-9daa0325e92e
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, juegos, DirectX, gráficos
ms.localizationpriority: medium
ms.openlocfilehash: 5dbdf6072f57d12d424f0787cfa2e8993a1624af
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2018
ms.locfileid: "8483379"
---
# <a name="basic-3d-graphics-for-directx-games"></a>Gráficos 3D básicos para juegos DirectX



Te vamos a enseñar cómo usar la programación DirectX para implementar los conceptos fundamentales de los gráficos 3D.

**Objetivo:** Aprender a programar una aplicación de gráficos 3D.

## <a name="prerequisites"></a>Requisitos previos


Suponemos que estás familiarizado con C++. También necesitas tener experiencia básica en los conceptos de programación de gráficos.

**Tiempo total para completarse:** 30minutos.

## <a name="where-to-go-from-here"></a>Dónde ir desde aquí


Vamos a explicar cómo desarrollar gráficos 3D con DirectX y C++\\Cx. En este tutorial compuesto por cinco partes se presenta la API de [Direct3D](https://msdn.microsoft.com/library/windows/desktop/hh309466), además de conceptos y códigos que también se usan en muchas otras muestras de DirectX. Estas partes se estructuran entre sí, desde la configuración de DirectX de una aplicación para UWP con C++ hasta las texturas de primitivos y adición de efectos.

> **Nota**este tutorial usa un sistema de coordenadas a la derecha con vectores de columna. Muchas aplicaciones y muestras de DirectX usan un sistema de coordenadas a la izquierda con vectores de fila. Si quieres una solución matemática de gráficos completa que admita un sistema de coordenadas a la izquierda con vectores de fila, piensa en usar [DirectXMath](https://msdn.microsoft.com/library/windows/desktop/hh437833). Para más información, consulta [Usar DirectXMath con Direct3D](https://msdn.microsoft.com/library/windows/desktop/ff729728#Use_DXMath_with_D3D).

 

Te vamos a enseñar lo siguiente:

-   Inicializar interfaces de [Direct3D](https://msdn.microsoft.com/library/windows/desktop/hh309466) con Windows en tiempo de ejecución
-   Aplicar operaciones de sombreador por vértice
-   Configurar la geometría
-   Rasterizar la escena (aplanar la escena en 3D para una proyección en 2D)
-   Eliminar las superficies ocultas

> **Nota**  

 

A continuación, creamos un dispositivo Direct3D, una cadena de intercambio y una vista de destino de representación, y mostramos la imagen representada en la pantalla.

[Inicio rápido: configurar recursos de DirectX y mostrar una imagen](setting-up-directx-resources.md)

## <a name="related-topics"></a>Temas relacionados


* [Gráficos de Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476080)
* [DXGI](https://msdn.microsoft.com/library/windows/desktop/hh404534)
* [HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509561)

 

 




