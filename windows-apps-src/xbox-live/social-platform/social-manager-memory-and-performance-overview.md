---
title: Memoria y rendimiento de administrador social
description: Describe las consideraciones de rendimiento y la memoria al usar el Administrador de redes sociales de la API del Administrador de Xbox Live.
ms.assetid: 2540145e-b8e2-4ab5-9390-65e2c9b19792
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, el Administrador de redes sociales, las personas
ms.localizationpriority: medium
ms.openlocfilehash: 7c6a0a31c8fe82faa644a59147060f9323d42eb0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618460"
---
# <a name="social-manager-memory-and-performance-overview"></a>Información general de rendimiento y memoria del Administrador de redes sociales

## <a name="memory"></a>Memoria
Social Manager permite ahora ha asignado memoria persistente que va a contener espacio del título. Se puede especificar un asignador de memoria personalizado para su uso por el Administrador de redes sociales mediante una llamada a `xbox_live_services_settings::set_memory_allocation_hooks()`. Este enlace de asignador de memoria es posible que también se usará para asignaciones futuras gran cantidad de memoria que usa Xbox Live API (XSAPI).

El peor caso para la asignación de memoria por el Administrador de redes sociales de forma predeterminada debe ser aproximadamente 4,75 mb (1). Hay más sobrecarga que el Administrador de redes sociales puede asignar según las actualizaciones de ATR y HTTP. Si crea un `xbox_social_user_group` de lista, agrega cada miembro ocupará kb 2,43 adicionales. Si se agrega un usuario a través de la `create_social_social_user_group_from_list`, `update_social_user_group`, o al usuario agregar a un amigo fuera el título, esto puede provocar un realloc buscar el espacio de memoria contigua.

(1) los mb 4,75 procede = 1000 `xbox_social_user` en 2,43 kb cada * 2. El 2 es que el Administrador de redes sociales mantiene un búfer doble de memoria.

## <a name="additional-user-limits"></a>Límites de usuarios adicionales
El Administrador de redes sociales actualmente tiene una restricción de 100 usuarios agregados al gráfico. Esto es debido a dos problemas:

1. El número máximo de suscripciones de ATR que un usuario puede tener es 1100. Si un usuario local tiene 1000 amigos, deja sólo 100 para las suscripciones adicionales.
2. La cantidad máxima de tamaño de lote de usuarios que pueden enviarse a PeopleHub está alrededor de 100.

El Administrador de redes sociales se comunica al no permitir que un grupo de usuarios de redes sociales de lista para que contenga más de 100 usuarios. Hay un límite de 100 usuarios total adicionales que puede estar en el sistema en cualquier momento a través de global `create_social_user_group_from_list`.

## <a name="processing-time"></a>medio de procesamiento
Social Manager va a tener en el peor caso 1100 usuarios. Las características de rendimiento del Administrador de redes sociales en Xbox One tiene un peor de los casos de 0,3 ms a 0,5 ms para `do_work` según el número de gráficos sociales creado. El caso típico es 0,01 ms para con ningún ms creada y hasta 0.2 de grupos para un grupo con 1000 usuarios en ella.
