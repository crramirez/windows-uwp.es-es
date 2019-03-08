---
title: Prácticas recomendadas de presencia enriquecida
description: Conozca los procedimientos recomendados para el uso de la presencia de Xbox Live enriquecido.
ms.assetid: 51a84137-37e4-4f98-b3d3-5ae70e27753d
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, presencia enriquecida, procedimientos recomendados
ms.localizationpriority: medium
ms.openlocfilehash: 75268575dd9dce59141d8909a59909bc973edbec
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57615670"
---
# <a name="rich-presence-best-practices"></a>Prácticas recomendadas de presencia enriquecida

Las siguientes sugerencias le ayudará a obtener el máximo partido de presencia enriquecida en el juego. Tenga en cuenta que la presencia enriquecida más cadenas defina, más rica será la experiencia de otros jugadores que detecten las personas de su juego.

-   Utilice las estadísticas en las cadenas, que le permite establecer la cadena y, a continuación, no se preocupe.

    Si la cadena tiene el nombre del mapa en ella, y usa el stat CurrentMap para rellenar el espacio en blanco, a continuación, el servicio actualizará la cadena como corresponda, durante la transmisión de los jugadores de asignación para asignar en el juego. Este enfoque le permite no preocuparse de mantenerlos actualizados, la cadena como el título está enviando los eventos adecuados para la plataforma de datos.

    El título debe establecer la cadena base presencia enriquecida con el servicio de presencia periódicamente, para asegurarse de que la información de presencia enriquecida para un usuario es correcta y el servicio está utilizando la cadena base correcta.

-   Use presencia enriquecida para abrir nuevas oportunidades de conversación. Creación de cadenas que están probables que generen interés en un juego para jugadores nuevos, así como los jugadores ocasionales que podrían haber pasado por alto una característica especial.

-   Crear cadenas de presencia enriquecida que motivan reproductores para tomar medidas. Por ejemplo, en lugar de decir "Jugar en mausoleo," por ejemplo, "solicita ayuda; Defensa mausoleo". Use el estado de presencia enriquecida para habilitar escenarios interesantes, como unirse a un juego en curso. A continuación, otro jugador puede unirse y ayuda.

-   Crear cadenas de presencia enriquecida que permite a los jugadores presumir de sus logros, como la finalización de los niveles o detección de áreas de secreto.

-   Para que los jugadores de Xbox en todo el mundo pueden formar parte de la Comunidad que están fomentando, localizar las cadenas de presencia enriquecida y sus parámetros asociados.

-   Algunos parámetros cambian con rapidez, pero puede tardar tiempo para los nuevos datos de presencia enriquecida mostrar un amigo. Si la cadena contiene "arma actual" y el jugador tiene la capacidad de alternar entre sus pistola y rifle, su cadena de presencia enriquecida es posible que no sea completamente exacto en un momento dado. Sin embargo, en algunos casos, que no es un problema. Si la cadena de presencia enriquecida contiene el valor de los enemigos total en contra, y el valor está desactivada de forma 1 o 2 durante unos segundos, puede que sea correcto para su escenario.
