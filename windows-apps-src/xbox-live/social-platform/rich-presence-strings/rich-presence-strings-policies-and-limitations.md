---
title: Limitaciones y las directivas de presencia enriquecida
description: Obtenga información sobre las directivas y las limitaciones del sistema de presencia de Xbox Live enriquecido.
ms.assetid: 0ad21a75-0524-45a8-8d8a-0dec0f7d6d6f
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, presencia enriquecida, directivas
ms.localizationpriority: medium
ms.openlocfilehash: f85974c0ccb38f3c33fb214ddaf24b98dd8dcdd0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57643600"
---
# <a name="rich-presence-policies-and-limitations"></a>Limitaciones y las directivas de presencia enriquecida

Al implementar la presencia enriquecida para el título, debe cumplir las directivas y los límites siguientes.

-   Cada título debe tener al menos 1 conjunto de cadena, pero no hay ningún límite superior en cuántos conjuntos de cadenas que puede tener.
-   Debe definir una cadena de forma predeterminada, así como cadenas de referencia cultural neutra para cada enumeración y para cada cadena de presencia enriquecida.
-   Puede usar numérico o cadena estadísticas para rellenar los parámetros en las cadenas. No se puede utilizar las estadísticas de fecha y hora.
-   Si usa las estadísticas en las cadenas de presencia enriquecida, a continuación, esas estadísticas (incluidas las enumeraciones para estadísticas) deben estar disponibles en el mismo ¿SCID & espacio aislado.
-   Tiene 1 línea de 44 caracteres en total (incluidos los valores de los parámetros). Esto es similar a los límites de presencia enriquecida de Xbox 360. Se trabajará con el cliente para ver si puede alcanzar la longitud de la cadena. Habrá un anuncio si la cadena puede ser más largo.
    -   Los caracteres Unicode son necesarios y deben ser capaz de trabajar con codificación UTF-8 para su presentación.
-   Los nombres descriptivos deben seguir estas reglas:
    -   Los caracteres permitidos son 'A' a 'Z', 'a' a 'z', un carácter de subrayado ('\_') y ' 0' a ' 9'.

        No hay ningún límite de caracteres.

-   No se realiza ninguna comprobación de la cadena en las cadenas; debe realizar cualquier comprobación de cadena, por ejemplo, el corrector ortográfico y comprobar que la cadena se ha traducido correctamente.
    -   Si se siente que un conjunto de cadena no es apropiado (por ejemplo, lenguaje ofensivo o abusiva), Microsoft impide que los títulos de con presencia enriquecida hasta que se han actualizado las cadenas para satisfacción nuestra.
-   Si el título no es la integración con la plataforma de datos, no hay ninguna opción para utilizar las estadísticas como parámetros en las cadenas.
    -   Todas las cadenas deben ser completamente predefinidas en ese caso (no se permite ningún token).
-   Los nombres de enumeración deben ser únicos en todas las enumeraciones y deben ser únicos para los nombres de estadística.
-   Si una línea supera el número de caracteres que se pueden mostrar, y hay un salto de línea, la línea se trunca automáticamente.
