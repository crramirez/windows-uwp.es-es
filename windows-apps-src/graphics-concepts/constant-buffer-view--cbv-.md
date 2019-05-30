---
title: Vista de búfer de constantes (CBV)
description: Los búferes de constantes contienen datos de constantes de sombreador. Su valor radica en que los datos persisten, y cualquier sombreador de la GPU puede acceder a ellos, hasta que sea necesario cambiar los datos.
ms.assetid: 99AEC6B0-A43B-4B61-8C3A-ECC8DE1B69A7
keywords:
- Vista de búfer de constantes (CBV)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7179f8644970a24a9e7b9ce50a4bcb4d5d225d46
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370454"
---
# <a name="constant-buffer-view-cbv"></a>Vista de búfer de constantes (CBV)


Los búferes de constantes contienen datos de constantes de sombreador. Su valor radica en que los datos persisten, y cualquier sombreador de la GPU puede acceder a ellos, hasta que sea necesario cambiar los datos.

Los datos típicos para un búfer de constantes serían matrices globales, de proyección y de vista, que se mantienen constantes durante el proceso de dibujo de un fotograma.

El diseño del búfer de constantes debe coincidir con el diseño de HLSL (consulta [Reglas de empaquetado para variables constantes](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-packing-rules)).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Vistas](views.md)

 

 




