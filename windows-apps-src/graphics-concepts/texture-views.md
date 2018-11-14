---
title: Vistas de texturas
description: En Direct3D, se accede a los recursos de textura con una vista, que es un mecanismo para la interpretación de hardware de un recurso en la memoria.
ms.assetid: 18DABFCE-8A36-4C4E-B08E-10428B05D701
keywords:
- Vistas de texturas
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9506b86fc16861984e539c52bdd92eed544079a8
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2018
ms.locfileid: "6472046"
---
# <a name="texture-views"></a>Vistas de texturas


En Direct3D, se accede a los recursos de texturas con una vista, que es un mecanismo para la interpretación de hardware de un recurso en memoria. Una vista permite a una fase de canalización concreta acceder solo a los [subrecursos](resource-types.md) que necesita, en la representación deseada por la aplicación.

Una vista admite la noción de un recurso sin tipo. Un recurso sin tipo es un recurso creado con un tamaño específico pero sin un tipo de datos específico. Los datos se interpretan de forma dinámica cuando se enlazan a la canalización.

La siguiente ilustración muestra un ejemplo de enlace de una matriz de texturas 2D con 6 texturas como recurso de sombreador, mediante la creación de una vista de recursos de sombreador para ella. A continuación, el recurso se trata como una matriz de texturas. (Nota: No se pueden enlazar un subrecurso a la canalización como entrada y salida simultáneamente).

![Ilustración de una matriz de texturas con 6 texturas](images/d3d10-cube-texture-faces.png)

Cuando se usa una matriz de texturas 2D como destino de representación, el recurso puede verse como una matriz de texturas 2D (6 en este ejemplo) con niveles de mapas MIP (3 en este ejemplo).

Crea un objeto de vista para un destino de representación mediante una llamada a CreateRenderTargetView. Luego llama a OMSetRenderTargets para establecer la vista del destino de representación para la canalización. Representa en los destinos de representación mediante una llamada a Draw y con RenderTargetArrayIndex para indexar en la textura adecuada de la matriz. Puedes usar un subrecurso (un nivel de mapa MIP, una combinación de índices de matriz) para enlazar a cualquier matriz de subrecursos. Así podrías enlazar al segundo nivel de mapa MIP y actualizar únicamente este nivel de mapa MIP concreto si lo desearas, como se muestra en la siguiente ilustración.

![Ilustración de enlace únicamente con el segundo nivel de mapa MIP de una matriz de texturas](images/d3d10-cube-texture-faces-subresource.png)

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Recursos](resources.md)

 

 




