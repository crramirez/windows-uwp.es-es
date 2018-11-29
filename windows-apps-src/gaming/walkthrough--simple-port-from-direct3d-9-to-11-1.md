---
title: 'Tutorial: Migrar de Direct3D 9 a DirectX 11 y UWP'
description: En este ejercicio de migración se muestra cómo llevar un marco de representación sencillo de Direct3D 9 a Direct3D 11 y la Plataforma universal de Windows (UWP).
ms.assetid: d4467e1f-929b-a4b8-b233-e142a8714c96
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, juegos, directx, puerto, direct3d 9, direct3d 11
ms.localizationpriority: medium
ms.openlocfilehash: c7569c6b2f041f5535e0eabe934a91da86b60b9a
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/29/2018
ms.locfileid: "8188980"
---
# <a name="walkthrough-port-a-simple-direct3d-9-app-to-directx-11-and-universal-windows-platform-uwp"></a>Tutorial Portar una aplicación de Direct3D 9 sencilla a DirectX 11 y la Plataforma universal de Windows (UWP)



En este ejercicio de migración se muestra cómo llevar un marco de representación sencillo de Direct3D 9 a Direct3D 11 y la Plataforma universal de Windows (UWP).
## 
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Tema</th>
<th align="left">Descripción</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="simple-port-from-direct3d-9-to-11-1-part-1--initializing-direct3d.md">Inicializar Direct3D11</a></p></td>
<td align="left"><p>Aprende a convertir el código de inicialización de Direct3D 9 a Direct3D 11, a obtener identificadores para el dispositivo Direct3D y el contexto de dispositivo, y a usar DXGI para configurar una cadena de intercambio.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="simple-port-from-direct3d-9-to-11-1-part-2--rendering.md">Convertir el marco de representación</a></p></td>
<td align="left"><p>Aprende a convertir un marco de representación simple de Direct3D 9 a Direct3D 11 y a realizar algunas acciones, como portar búferes de geometría, compilar y cargar programas sombreadores HLSL e implementar la cadena de representación en Direct3D 11.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="simple-port-from-direct3d-9-to-11-1-part-3--viewport-and-game-loop.md">Migrar el bucle del juego</a></p></td>
<td align="left"><p>Aprende a implementar una ventana para un juego de UWP y a traer el bucle de la repetición, incluso cómo crear una <a href="https://msdn.microsoft.com/library/windows/apps/hh700478"><strong>IFrameworkView</strong></a> para controlar una <a href="https://msdn.microsoft.com/library/windows/apps/br208225"><strong>CoreWindow</strong></a> de pantalla completa.</p></td>
</tr>
</tbody>
</table>

 

En este tema se analizan dos rutas de código que realizan las mismas tareas gráficas básicas: mostrar un cubo de vértice sombreado giratorio. En ambos casos, el código cubre el siguiente proceso:

1.  Crear un dispositivo Direct3D y una cadena de intercambio.
2.  Crear un búfer de vértices y un búfer de índices para representar una malla de cubo de colores.
3.  Crear un sombreador de vértices que transforma los vértices en espacio de pantalla y un sombreador de píxeles que combina los valores de color, compilar los sombreadores y cargar los sombreadores como recursos Direct3D.
4.  Implementar la cadena de representación y presentar el cubo dibujado en la pantalla.
5.  Crear una ventana, iniciar un bucle principal y encargarse del procesamiento de los mensajes de ventana.

Al finalizar con este tutorial, deberías estar familiarizado con las siguientes diferencias básicas entre Direct3D 9 y Direct3D 11:

-   La separación del dispositivo, el contexto del dispositivo y la infraestructura de gráficos.
-   El proceso de compilar sombreadores y cargar el código de bytes del sombreador en tiempo de ejecución.
-   Procedimientos para configurar los datos por vértice para la fase de ensamblador de entrada (IA).
-   Procedimientos para usar [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478) para crear una vista de CoreWindow.

Ten en cuenta que este tutorial usa [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) por simplicidad y no cubre la interoperabilidad de XAML.

## <a name="prerequisites"></a>Requisitos previos


Debes [preparar tu entorno de desarrollo para el desarrollo de juegos de DirectX de UWP](prepare-your-dev-environment-for-windows-store-directx-game-development.md). Todavía no necesitas una plantilla, pero tendrás que Microsoft Studio2015 Visual para cargar las muestras de código de este tutorial.

Consulta [Conceptos y consideraciones de migración](porting-considerations.md) para comprender mejor los conceptos de programación de DirectX11 y UWP que se muestran en este tutorial.

## <a name="related-topics"></a>Temas relacionados

**Direct3D**

* [Escribir sombreadores HLSL en Direct3D9](https://msdn.microsoft.com/library/windows/desktop/bb944006)
* [Plantillas de proyectos de juegos DirectX](user-interface.md)

**Microsoft Store**

* [**Microsoft::WRL::ComPtr**](https://msdn.microsoft.com/library/windows/apps/br244983.aspx)
* [**Identificador para Objeto operador (^)**](https://msdn.microsoft.com/library/windows/apps/yk97tc08.aspx)

