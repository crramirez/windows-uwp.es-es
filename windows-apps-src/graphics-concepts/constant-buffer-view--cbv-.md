---
title: Vista de búfer de constantes (CBV)
description: Los búferes de constantes contienen datos de constantes de sombreador. El valor de ellos es que los datos persisten y se puede tener acceso a ellos mediante cualquier sombreador de GPU hasta que sea necesario cambiar los datos.
ms.assetid: 99AEC6B0-A43B-4B61-8C3A-ECC8DE1B69A7
keywords:
- Vista de búfer de constantes (CBV)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 50b590ab931f60b67ecd527629b681c9ffe63f71
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162749"
---
# <a name="constant-buffer-view-cbv"></a>Vista de búfer de constantes (CBV)


Los búferes de constantes contienen datos de constantes de sombreador. El valor de ellos es que los datos persisten y se puede tener acceso a ellos mediante cualquier sombreador de GPU hasta que sea necesario cambiar los datos.

Los datos típicos de un búfer de constantes serían matrices de vistas, de proyección y de vista mundial, que permanecen constantes a lo largo del dibujo de un fotograma.

El diseño de búfer de constantes debe coincidir con el diseño de HLSL (consulte [reglas de empaquetado para variables constantes](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-packing-rules)).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Vistas](views.md)

 

 