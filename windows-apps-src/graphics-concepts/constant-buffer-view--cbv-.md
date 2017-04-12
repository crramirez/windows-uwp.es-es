---
title: "Vista de búfer de constantes (CBV)"
description: "Los búferes de constantes contienen datos de constantes de sombreador. Su valor radica en que los datos persisten, y cualquier sombreador de la GPU puede acceder a ellos, hasta que sea necesario cambiar los datos."
ms.assetid: 99AEC6B0-A43B-4B61-8C3A-ECC8DE1B69A7
keywords: "Vista de búfer de constantes (CBV)"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 755c80e895281b93e9e37144526ed1790c63199f
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="constant-buffer-view-cbv"></a>Vista de búfer de constantes (CBV)


Los búferes de constantes contienen datos de constantes de sombreador. Su valor radica en que los datos persisten, y cualquier sombreador de la GPU puede acceder a ellos, hasta que sea necesario cambiar los datos.

Los datos típicos para un búfer de constantes serían matrices globales, de proyección y de vista, que se mantienen constantes durante el proceso de dibujo de un fotograma.

El diseño del búfer de constantes debe coincidir con el diseño de HLSL (consulta [Reglas de empaquetado para variables constantes](https://msdn.microsoft.com/library/windows/desktop/bb509632.aspx)).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Vistas](views.md)

 

 




