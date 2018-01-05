---
title: Operaciones disponibles en recursos de streaming
description: "En esta sección, se enumeran las operaciones que puedes realizar en recursos de streaming."
ms.assetid: 700D8C54-0E20-4B2B-BEA3-20F6F72B8E24
keywords: Operaciones disponibles en recursos de streaming
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: ee9f6b3744b038d9f4ae0f5b490cbd763eda08ee
ms.sourcegitcommit: c80b9e6589a1ee29c5032a0b942e6a024c224ea7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/22/2017
---
# <a name="operations-available-on-streaming-resources"></a>Operaciones disponibles en recursos de streaming


En esta sección, se enumeran las operaciones que puedes realizar en recursos de streaming.

-   Operaciones de actualización de asignaciones de iconos y operaciones de copia de asignaciones de iconos que devuelvan un valor void: estas operaciones apuntan ubicaciones de iconos en un recurso de streaming a ubicaciones en grupos de iconos, nulas o a ambas. Estas operaciones pueden actualizar un subconjunto no contiguo de punteros de iconos.
-   Operaciones de copia y actualización: todas las API que pueden copiar datos a una superficie de grupos por defecto o desde ella funcionan para recursos de streaming. La lectura de iconos sin asignar produce 0 y se anulan las escrituras en iconos sin asignar.
-   Operaciones de copia y actualización de iconos: estas operaciones existen para copiar iconos a una granularidad de 64KB a cualquier recurso de streaming y desde él y un recurso de búfer en un diseño de memoria canónico. El hardware y el controlador de pantalla realizan cualquier "permutación" de memoria necesaria para el recurso de streaming.
-   Los enlaces de canalizaciones y los enlaces o creaciones de vista de Direct3D que funcionan en recursos de no streaming funcionan también en recursos de streaming.

Los controles de iconos están disponibles en contextos inmediatos o diferidos (igual que las actualizaciones de los recursos típicos) y después de ejecutar accesos posteriores de impacto en los iconos (operaciones que no se enviaron anteriormente).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Creación de recursos de streaming](creating-streaming-resources.md)

 

 




