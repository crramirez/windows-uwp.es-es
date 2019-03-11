---
title: Logros de 2017
description: Describe cómo puede configurar los logros en el centro de partners para entregar las recompensas.
ms.assetid: ''
ms.date: 11/10/2017
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, udc, Centro para desarrolladores universal
ms.openlocfilehash: 9ef2365ee560e273108c38570697d599adde4c0b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655580"
---
# <a name="configure-achievements-2017-in-partner-center"></a>Configurar 2017 logros en el centro de partners

> [!IMPORTANT]
> Logros solo son aplicables a ID@Xbox o socios administrados. No se admiten los juegos que participan en el programa de creadores de Xbox Live.

Puede usar [centro de partners](https://partner.microsoft.com/dashboard) para configurar el [logros 2017](../../achievements-2017/simplified-achievements.md) que están asociados con su juego. Agregue una mención especial de nuevo mediante los pasos siguientes:

1. Vaya a la sección logros en su título, ubicado en **servicios** > **Xbox Live** > **logros**.
2. Haga clic en el **nuevo logro** botón y rellene el formulario.  Una vez completado, haga clic en **guardar**.

![Captura de pantalla para crear una mención especial de nuevo en el centro de partners](../../images/dev-center/achievement-table.png)

## <a name="description"></a>Descripción
La sección Descripción es donde puede especificar los aspectos básicos de la mención especial, como el nombre y las descripciones bloqueado o desbloqueado. Puede agregar compatibilidad de localización a logros mediante el acceso a la **cadenas traducidas** sección de configuración de servicio [centro de partners](https://partner.microsoft.com/dashboard).

![Captura de pantalla de los campos de descripción al configurar una mención especial de nuevo en el centro de partners](../../images/dev-center/achievements-2.png)

El **logro nombre** campo es el nombre público del logro.

El **bloqueado descripción** campo es la descripción que verán los jugadores cuando no ha conseguido el logro. Tiene un límite de 100 caracteres.

El **descripción desbloqueado** campo es la descripción que verán los jugadores una vez que ha conseguido el logro. Tiene un límite de 100 caracteres.

## <a name="details"></a>Detalles
La sección de detalles se usa para asociar información importante como la imagen, el tipo de logro, la recompensa de puntuación (si existe) y si debe ocultarse la consecución hasta desbloqueado.

![Captura de pantalla de los campos de detalles al configurar una mención especial de nuevo en el centro de partners](../../images/dev-center/achievements-3.png)

El **icono de imagen** campo es la imagen que se mostrará junto con la realización. Debe ser un archivo png de 1920 x 1080.

**Basar logros** están disponibles para los jugadores cuando se ha publicado la partida inicial. **No son de Base logros** están disponibles después de que ha publicado el juego inicial (por ejemplo con DLC nuevo contenido).

El **puntuación** campo es la cantidad de puntos de puntuación que iremos concediendo sus logros cuando desbloquea. Puede recompensar cada mención especial de entre 0-200 puntos.  

**Pública** logros son visibles para todos los jugadores, independientemente de si ha conseguido el logro o no. **Secreto** logros están ocultos hasta que se han desbloqueado.

**Vínculo profundo de logro** es una manera de obtener un parámetro de la mención especial que le permite vincular a un lugar en su juego en donde se puede ganar el logro. El vínculo profundo se devuelve en la respuesta de obtención de API. La dirección URL especificada debe contener `ms-xbl-{titleID}://` prefijo.

> [!TIP]
> Vínculos profundos logro requieren el TitleId hexadecimal del juego. Puede encontrarlo en [el programa de instalación de Xbox Live](xbox-live-setup.md) pantalla en [centro de partners](https://developer.microsoft.com/dashboard).

## <a name="additional-rewards"></a>Ventajas adicionales
En algunos casos, es posible que desea ofrecer una recompensa de juego o una ilustración cuando un reproductor desbloquea un logro. Puede definir las recompensas (si existe) que están asociados con una mención especial en el **recompensas adicionales** sección. Un logro puede contener dos recompensas adicionales: uno de cada tipo de recompensa. Puede leer más sobre la [logro recompensas](../../achievements-2017/achievement-rewards.md) artículo.

Para crear un premio nuevo, haga clic en el **agregar Reward** situado en la **recompensas adicionales** sección y rellene el formulario.

![Captura de pantalla de agregar las recompensas a una mención especial en el centro de partners](../../images/dev-center/achievement-reward.png)

### <a name="reward-details"></a>Detalles de recompensa
Rellene los detalles de recompensa para asociar un premio nuevo. Una vez completado, haga clic en **agregar**.

![Captura de pantalla de configuración de detalles del premio para un logro en Centro de partners](../../images/dev-center/achievements-5.png)

Hay dos tipos de recompensas mención especial que se pueden crear. Estos son:

1. El **arte** tipo se puede utilizar si desea recompensar el Reproductor con cosas como alta resolución concepto imágenes, dibujos principios de diseño, creado especialmente activos gráficos u otra imagen digital. Activos de material gráfico se muestran en el panel de Xbox One y ser también pueden mostrarse en las experiencias de complementaria al consultar el servicio logros.
2. El **en juego** tipo se puede utilizar si desea recompensar el Reproductor personalizada recompensas en juego sin actualizar el título. Algunos escenarios posibles son extra en el juego moneda/puntos o acceso a los mapas, armas o caracteres especiales.

El **nombre para mostrar** campo es el nombre de la recompensa que verán los jugadores. Tiene un límite de 57 caracteres.

El **descripción** campo es la descripción de la recompensa que los jugadores aparecerán y deben incluir las instrucciones de canje. Tiene un límite de 90 caracteres.

El **imagen** o **arte** campo es la imagen asociada a la recompensa. Si el tipo se establece en el material gráfico, se trata de la ilustración que se recompensa. En caso contrario, representará la recompensa de juego que van a recibir. La imagen debe ser un archivo png de 1920 x 1080.

El **valor en el juego** campo solo está visible si selecciona el tipo de recompensa de **en juego**. Sirve para especificar el valor que se pasa al código del juego en la que se puede usar para desbloquear la recompensa en juego.
