---
author: jnHs
Description: "Puedes ayudar a los clientes a descubrir tu aplicación mediante un vínculo a su descripción en la Tienda."
title: "Vincular a la aplicación"
ms.assetid: 5420B65C-7ECE-4364-8959-D1683684E146
ms.author: wdg-dev-content
ms.date: 06/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows10, uwp, vínculo, protocolo de la tienda windows, vincular a una aplicación, vincular a aplicación"
ms.openlocfilehash: 2d0750493926937a6326c5f72f568d4294b137c5
ms.sourcegitcommit: fadde8afee46238443ec1cb71846d36c91db9fb9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/21/2017
---
# <a name="link-to-your-app"></a>Vincular a la aplicación


Puedes ayudar a los clientes a descubrir tu aplicación mediante un vínculo a su descripción en la Tienda.

## <a name="getting-the-link-to-your-apps-store-listing"></a>Obtener el vínculo a la descripción de la aplicación en la Tienda

Para obtener la dirección URL de la descripción de la Tienda de tu aplicación, desplázate a la página [Identidad de la aplicación](view-app-identity-details.md) en la sección **Administración de aplicaciones**. La dirección URL está en el formato **`https://www.microsoft.com/store/apps/<your app's Store ID>`**.

Cuando un cliente hace clic en este vínculo, se abre la página de descripción basada en web de la aplicación. En los dispositivos Windows, la aplicación Tienda también se iniciará y mostrará la descripción de tu aplicación.


## <a name="linking-to-your-apps-store-listing-with-the-windows-store-badge"></a>Crear un vínculo a la descripción de la Tienda de la aplicación con el distintivo de la Tienda Windows

Puedes crear un vínculo directo a la sinopsis de tu aplicación mediante un distintivo personalizado que permita a los clientes saber que la aplicación se encuentra en la Tienda Windows.

Para crear el distintivo, visita la página [Distintivos de la Tienda Windows](http://go.microsoft.com/fwlink/p/?LinkID=534236). Debes tener el **Id. de la Tienda** de 12 caracteres de la aplicación para generar el distintivo y un vínculo. Encontrarás el **Id. de la Tienda** de la aplicación en la página [Identidad de la aplicación](view-app-identity-details.md) en la sección **Administración de aplicaciones**.

> [!NOTE]
> Consulta [Directrices para el marketing de aplicaciones](app-marketing-guidelines.md) para obtener información y los requisitos relacionados con el uso del distintivo de la Tienda Windows.


## <a name="linking-directly-to-your-app-in-the-windows-store"></a>Vincular directamente a tu aplicación en la Tienda Windows

Puedes crear un vínculo que inicie la Tienda Windows y lleve directamente a la página de descripción de la aplicación sin tener que abrir un explorador mediante el uso del esquema de URI **ms-windows-store:**.

Estos vínculos son muy útiles cuando sabes que los usuarios usan un dispositivo Windows y quieres que lleguen directamente a la página de descripción en la Tienda. Por ejemplo, es posible que quieras usar este vínculo después de comprobar las cadenas de agente de usuario en un navegador para confirmar que el sistema operativo del usuario es compatible con la Tienda, o cuando ya te estás comunicando a través de una aplicación para UWP.

Para usar el protocolo de la Tienda Windows para vincular directamente a la lista de la Tienda de la aplicación, anexa el Id. de la Tienda de la aplicación a este vínculo:

`ms-windows-store://pdp/?ProductId=`

Para obtener más información sobre cómo usar el protocolo de la Tienda Windows, consulta [Iniciar la aplicación de la Tienda Windows](../launch-resume/launch-store-app.md).

 

 




