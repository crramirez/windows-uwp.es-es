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
ms.sourcegitcommit: 2a63ee6770413bc35ace09b14f56b60007be7433
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2018
ms.locfileid: "3930673"
---
# <a name="link-to-your-app"></a>Vincular a la aplicación


Puede ayudar a los clientes a descubrir tu aplicación mediante un vínculo a la descripción de la aplicación en Microsoft Store.

## <a name="getting-the-link-to-your-apps-store-listing"></a>Obtener el vínculo a la descripción de la aplicación en la Tienda

Para obtener la dirección URL de la descripción de la Tienda de tu aplicación, desplázate a la página [Identidad de la aplicación](view-app-identity-details.md) en la sección **Administración de aplicaciones**. La dirección URL está en el formato **`https://www.microsoft.com/store/apps/<your app's Store ID>`**.

Cuando un cliente hace clic en este vínculo, se abre la página de descripción basada en web de la aplicación. En los dispositivos Windows, la aplicación Tienda también se iniciará y mostrará la descripción de tu aplicación.


## <a name="linking-to-your-apps-store-listing-with-the-microsoft-store-badge"></a>Un vínculo a la descripción de la tienda de la aplicación con el distintivo de Microsoft Store

Puedes vincular directamente a la descripción de la aplicación mediante un distintivo personalizado para informar a los clientes de que la aplicación está en la Microsoft Store.

Para crear el distintivo, visita la página de [distintivos de Microsoft Store](http://go.microsoft.com/fwlink/p/?LinkID=534236) . Debes tener el **Id. de la Tienda** de 12 caracteres de la aplicación para generar el distintivo y un vínculo. Encontrarás el **Id. de la Tienda** de la aplicación en la página [Identidad de la aplicación](view-app-identity-details.md) en la sección **Administración de aplicaciones**.

> [!NOTE]
> Para obtener información y requisitos relacionados con el uso de los distintivos de Microsoft Store, consulta [directrices de marketing de la aplicación](app-marketing-guidelines.md) .


## <a name="linking-directly-to-your-app-in-the-microsoft-store"></a>Vincular directamente a la aplicación en Microsoft Store

Puedes crear un vínculo que inicie Microsoft Store y lleve directamente a la página de descripción de la aplicación sin tener que abrir un explorador mediante el uso de la **ms-windows-store:** esquema de URI.

Estos vínculos son muy útiles cuando sabes que los usuarios usan un dispositivo Windows y quieres que lleguen directamente a la página de descripción en la Tienda. Por ejemplo, es posible que quieras usar este vínculo después de comprobar las cadenas de agente de usuario en un navegador para confirmar que el sistema operativo del usuario es compatible con la Tienda, o cuando ya te estás comunicando a través de una aplicación para UWP.

Para usar este esquema de URI para vincular directamente a la descripción de la tienda de la aplicación, anexa el Id. de Store de la aplicación a este vínculo:

`ms-windows-store://pdp/?ProductId=`

Para obtener más información sobre cómo usar el protocolo de Microsoft Store, consulta [iniciar la aplicación de Microsoft](../launch-resume/launch-store-app.md).

 

 




