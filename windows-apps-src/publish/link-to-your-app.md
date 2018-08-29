---
author: jnHs
Description: You can help customers discover your app by linking to your app's listing in the Microsoft Store.
title: Vincular a la aplicación
ms.assetid: 5420B65C-7ECE-4364-8959-D1683684E146
ms.author: wdg-dev-content
ms.date: 10/26/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp, vínculo, protocolo de la tienda windows, vincular a una aplicación, vincular a aplicación
ms.localizationpriority: medium
ms.openlocfilehash: 0025321aa73a66cc0a976bd347e613de3c3c4765
ms.sourcegitcommit: 3727445c1d6374401b867c78e4ff8b07d92b7adc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2018
ms.locfileid: "2916901"
---
# <a name="link-to-your-app"></a>Vincular a la aplicación


Puede ayudar a los clientes descubrir su aplicación mediante la vinculación con el anuncio de la aplicación en la Store de Microsoft.

## <a name="getting-the-link-to-your-apps-store-listing"></a>Obtener el vínculo a la descripción de la aplicación en la Tienda

Para obtener la dirección URL de la descripción de la Tienda de tu aplicación, desplázate a la página [Identidad de la aplicación](view-app-identity-details.md) en la sección **Administración de aplicaciones**. La dirección URL está en el formato **`https://www.microsoft.com/store/apps/<your app's Store ID>`**.

Cuando un cliente hace clic en este vínculo, se abre la página de descripción basada en web de la aplicación. En los dispositivos Windows, la aplicación Tienda también se iniciará y mostrará la descripción de tu aplicación.


## <a name="linking-to-your-apps-store-listing-with-the-microsoft-store-badge"></a>Vincular a listado de almacén de la aplicación con el identificador de Microsoft Store

Puede vincular directamente al listado de sus aplicaciones con una tarjeta de identificación personalizada que los clientes conozcan que la aplicación está en la Store de Microsoft.

Para crear su tarjeta de identificación, visite la página de [insignias de almacén de Microsoft](http://go.microsoft.com/fwlink/p/?LinkID=534236) . Debes tener el **Id. de la Tienda** de 12 caracteres de la aplicación para generar el distintivo y un vínculo. Encontrarás el **Id. de la Tienda** de la aplicación en la página [Identidad de la aplicación](view-app-identity-details.md) en la sección **Administración de aplicaciones**.

> [!NOTE]
> Ver [directrices de marketing App](app-marketing-guidelines.md) para información y requisitos relacionados con el uso de la divisa de Microsoft Store.


## <a name="linking-directly-to-your-app-in-the-microsoft-store"></a>Vincular directamente a su aplicación en el almacén de Microsoft

Puede crear un vínculo que abre el Store Microsoft y va directamente a la página del anuncio de la aplicación sin tener que abrir un explorador utilizando la **ms-windows-store:** esquema de URI.

Estos vínculos son muy útiles cuando sabes que los usuarios usan un dispositivo Windows y quieres que lleguen directamente a la página de descripción en la Tienda. Por ejemplo, es posible que quieras usar este vínculo después de comprobar las cadenas de agente de usuario en un navegador para confirmar que el sistema operativo del usuario es compatible con la Tienda, o cuando ya te estás comunicando a través de una aplicación para UWP.

Para utilizar este esquema URI para enlazar directamente con la de anuncio de tienda de la aplicación, anexar el identificador de almacén de la aplicación a este vínculo:

`ms-windows-store://pdp/?ProductId=`

Para obtener más información acerca de cómo utilizar el protocolo de Microsoft Store, consulte [iniciar la aplicación de Microsoft](../launch-resume/launch-store-app.md).

 

 




