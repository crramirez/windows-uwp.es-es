---
title: Vista ordenada de rasterizador (ROV)
description: Las vistas ordenadas de rasterizador permiten corregir algunas de las limitaciones de un búfer de profundidad, en particular la de tener varias texturas con transparencia que se apliquen a los mismos píxeles.
ms.assetid: BCB1EE0D-4C1D-4E17-BDB7-173F448E0A7B
keywords:
- Vista ordenada de rasterizador (ROV)
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7327304e2b42ff5ff71be136220b58e99c6228d2
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2018
ms.locfileid: "6459100"
---
# <a name="rasterizer-ordered-view-rov"></a>Vista ordenada de rasterizador (ROV)


Las vistas ordenadas de rasterizador permiten corregir algunas de las limitaciones de un búfer de profundidad, en particular la de tener varias texturas con transparencia que se apliquen a los mismos píxeles.

Las vistas ordenadas de rasterizador permiten aplicar los algoritmos de "transparencia independiente del orden" (OIT) a la representación de píxeles. Un búfer de profundidad solo permite dibujar u ocluir un píxel; no hay ningún concepto de oclusión parcial a través de la transparencia. Los algoritmos OIT aplican texturas transparentes en el orden correcto; así, si un objeto de cristal transparente debe aparecer detrás de una ventana que se encuentra detrás de vegetación que usa texturas transparentes, el resultado final se dibuja de forma correcta predecible. Sin las vistas ROV y los algoritmos OIT, el orden en que estos objetos transparentes se dibujarían sería impredecible y la escena representada podría ser simplemente confusa e incorrecta.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Vistas](views.md)

 

 




