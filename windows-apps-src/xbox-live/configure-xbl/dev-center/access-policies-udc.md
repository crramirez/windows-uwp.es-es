---
title: Configurar directivas de acceso en el centro de partners
description: Describe cómo puede configurar directivas de acceso en el centro de partners para permitir que otras aplicaciones, juegos y servicios de acceso a la configuración de Xbox Live.
ms.assetid: ''
ms.date: 02/21/2018
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, udc, Centro para desarrolladores universal
ms.openlocfilehash: ae26c18abdac30ff988e90ee5c56f178bf14b74a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658220"
---
# <a name="configure-access-policies-in-partner-center"></a>Configurar directivas de acceso en el centro de partners

Puede usar [centro de partners](https://partner.microsoft.com/dashboard) para permitir que otros servicios, juegos y aplicaciones para tener acceso a datos y la configuración de Xbox Live su título. Por ejemplo, es posible que desee que un servicio web para mostrar marcadores en el sitio Web o puede tener una aplicación complementaria que puede acceder al almacenamiento de título del juego para ver o modificar datos de juegos guardados.

De forma predeterminada, solo el título en sí misma puede tener acceso a la configuración y los datos almacenados en el servicio Xbox Live. Puede cambiar esto configurando directivas de acceso en el centro de partners.

> [!NOTE]
> En este tema no se aplica a los títulos en el programa de creadores de Xbox Live.

Agregar configuración haciendo lo siguiente:

1. Después de seleccionar el título en [centro de partners](https://partner.microsoft.com/dashboard), vaya a **servicios** > **Xbox Live**.

2. Haga clic en el vínculo a **las directivas de acceso**.

3. Haga clic en la configuración que desea conceder acceso a y haga clic en el botón de la aplicación o el servicio de agregar. Esto agregará una nueva fila a la parte inferior de la lista de aplicaciones o servicios configurados para tener acceso a esa configuración.

4. Seleccione el tipo de aplicación o servicio en el cuadro de lista desplegable y rellene el cuadro de detalle para indicar la aplicación, el identificador de título o el Id. de servicio de la aplicación o servicio que tendrá acceso a los datos.

5. Seleccione si la aplicación o servicio sólo puede leer los datos, o si tiene acceso completo a los datos.

6. Repetir para cada configuración y para cada aplicación o servicio que necesita tener acceso a esos valores. Puede hacer clic en **eliminar** para quitar una entrada.

7. Cuando haya terminado, haga clic en el **guardar** botón para guardar los cambios.

![Las directivas de acceso agregan pantalla de la aplicación o servicio](../../images/dev-center/data-sharing-2.png)
