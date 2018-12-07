---
title: Vista de búfer de constantes (CBV)
description: Los búferes de constantes contienen datos de constantes de sombreador. Su valor radica en que los datos persisten, y cualquier sombreador de la GPU puede acceder a ellos, hasta que sea necesario cambiar los datos.
ms.assetid: 99AEC6B0-A43B-4B61-8C3A-ECC8DE1B69A7
keywords:
- Vista de búfer de constantes (CBV)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 33e850ba16be7a8d2621f061015d39c8b334cab2
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/07/2018
ms.locfileid: "8807047"
---
# <a name="constant-buffer-view-cbv"></a>Vista de búfer de constantes (CBV)


Los búferes de constantes contienen datos de constantes de sombreador. Su valor radica en que los datos persisten, y cualquier sombreador de la GPU puede acceder a ellos, hasta que sea necesario cambiar los datos.

Los datos típicos para un búfer de constantes serían matrices globales, de proyección y de vista, que se mantienen constantes durante el proceso de dibujo de un fotograma.

El diseño del búfer de constantes debe coincidir con el diseño de HLSL (consulta [Reglas de empaquetado para variables constantes](https://msdn.microsoft.com/library/windows/desktop/bb509632.aspx)).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Vistas](views.md)

 

 




