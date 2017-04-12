---
title: Vista ordenada de rasterizador (ROV)
description: "Las vistas ordenadas de rasterizador permiten corregir algunas de las limitaciones de un búfer de profundidad, en particular la de tener varias texturas con transparencia que se apliquen a los mismos píxeles."
ms.assetid: BCB1EE0D-4C1D-4E17-BDB7-173F448E0A7B
keywords: Vista ordenada de rasterizador (ROV)
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 313b599a402ba00e220aca649834a217daf3eaaa
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="rasterizer-ordered-view-rov"></a>Vista ordenada de rasterizador (ROV)


Las vistas ordenadas de rasterizador permiten corregir algunas de las limitaciones de un búfer de profundidad, en particular la de tener varias texturas con transparencia que se apliquen a los mismos píxeles.

Las vistas ordenadas de rasterizador permiten aplicar los algoritmos de "transparencia independiente del orden" (OIT) a la representación de píxeles. Un búfer de profundidad solo permite dibujar u ocluir un píxel; no hay ningún concepto de oclusión parcial a través de la transparencia. Los algoritmos OIT aplican texturas transparentes en el orden correcto; así, si un objeto de cristal transparente debe aparecer detrás de una ventana que se encuentra detrás de vegetación que usa texturas transparentes, el resultado final se dibuja de forma correcta predecible. Sin las vistas ROV y los algoritmos OIT, el orden en que estos objetos transparentes se dibujarían sería impredecible y la escena representada podría ser simplemente confusa e incorrecta.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Vistas](views.md)

 

 




