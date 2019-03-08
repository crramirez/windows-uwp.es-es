---
title: Configuración de instalación de Xbox Live
description: Describe cómo puede configurar el programa de instalación de Xbox Live en el centro de partners.
ms.assetid: ''
ms.date: 10/30/2017
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox live, Xbox, juegos, uwp, windows 10, Xbox uno, centro de partners, el programa de instalación de Xbox Live
ms.openlocfilehash: 9a846a4b7f0069216e92eb123b33d9fc0f7f67c9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612830"
---
# <a name="configure-xbox-live-setup-in-partner-center"></a>Configurar el programa de instalación de Xbox Live en el centro de partners

Puede usar [centro de partners](https://developer.microsoft.com/dashboard) para configurar el conjunto inicial de propiedades de Xbox Live que están asociados con su juego. Agregar configuración haciendo lo siguiente:

1. Navegue hasta la **el programa de instalación de Xbox Live** sección del título, ubicado en **servicios** > **Xbox Live** > **el programa de instalación de Xbox Live** .
2. En esta página, puede establecer los nombres de título, configuración regional predeterminada, el tipo de producto, familias de dispositivos y la fecha existen. Cuando haya terminado la configuración, haga clic en el **guardar** botón para enviar los cambios.

## <a name="title-names"></a>Nombres de título
Al hacer clic en **agregar localizado título**, puede especificar un nombre para el producto y seleccione un idioma para localizar. Tenga en cuenta aquí que el nombre del título debe asignar a los nombres de producto localizado que haya definido en la página de propiedades de la presentación. Valor predeterminado es inglés (en-US).

![Imagen del cuadro de diálogo Agregar título localizado en el centro de partners](../../images/dev-center/xbox-live-setup/xbox-live-setup-1.png)

## <a name="default-locale"></a>Configuración regional predeterminada
Esta opción permite establecer el idioma predeterminado que se usará para configurar todas las cadenas en la configuración del servicio Xbox Live. Por ejemplo, si establece la configuración regional predeterminada para español (es-es) y desea configurar un logro, a continuación, como mínimo, el logro nombre y descripción tendría que estar en español. En otras palabras, no se puede establecer esta opción en español, pero solo proporcione la información de logro en inglés. Todos los de la configuración del servicio Xbox Live deben proporcionarse en la misma versión que el de la configuración regional predeterminada. De forma predeterminada, la configuración regional predeterminada se establece en inglés (en-US).
> [!NOTE]
> Además, todas las cadenas se pueden localizar en la página de cadenas traducidas.  

![Imagen de la lista desplegable para elegir la configuración regional predeterminada en el centro de partners, seleccione](../../images/dev-center/xbox-live-setup/xbox-live-setup-2.png)

## <a name="product-type"></a>Tipo de producto
El menú desplegable permite que cambiar el tipo de producto. El valor predeterminado es el tipo **juego**. La opción que elija afectará a las características de Xbox Live a su disposición. Tiene tres opciones para elegir:
1. Aplicación 
2. Juego 
3. Demostración del juego 

![Imagen de la lista desplegable para elegir el tipo de producto en el centro de partners, seleccione](../../images/dev-center/xbox-live-setup/xbox-live-setup-3.png)

## <a name="device-families"></a>Familias de dispositivos
Esta configuración permite elegir el tipo de dispositivos en el que el título puede tener acceso a Xbox Live. De forma predeterminada, todas las familias de dispositivos están habilitadas. Puede comprobar los dispositivos para habilitarlos.

![Imagen de las casillas de selección para seleccionar las familias de dispositivos de centro de partners](../../images/dev-center/xbox-live-setup/xbox-live-setup-4.png)

## <a name="embargo-date"></a>Existen fecha
La fecha que seleccione determinará cuando la configuración de Xbox Live lanza al público. Es importante tener en cuenta que incluso si se publican los cambios a RETAIL no irán en vivo a menos que se ha cumplido la fecha existen. Para explicarlo:
1. Si selecciona una fecha en el futuro, los cambios estará disponibles al público en esa fecha.
2. Si selecciona una fecha en el pasado, los cambios estará disponibles al público en cuanto se publican los cambios a la venta directa.

Haga clic en el selector de fecha y hora y expandirá para que pueda seleccionar la fecha y hora precisa. Al hacer clic en **Aceptar**, se establecerá la fecha existen.

![Imagen de configuración de la fecha existen en el centro de partners](../../images/dev-center/xbox-live-setup/xbox-live-setup-5.png)

## <a name="advanced-settings"></a>Configuración avanzada

Puede hacer clic en **Mostrar opciones** para establecer el **múltiples puntos de presencia**. Varios puntos de presencia permite el mismo usuario iniciar sesión en Xbox Live desde varios dispositivos al mismo tiempo. Características de Xbox Live, como, logros y participan varios jugadores tendrán acceso limitado. Por lo tanto, no se recomienda esta opción para juegos. Habilite esta opción activando la casilla.
