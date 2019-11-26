---
Description: Puede ayudar a los clientes a detectar la aplicación mediante un vínculo a la lista de la aplicación en el Microsoft Store.
title: Vincular a la aplicación
ms.assetid: 5420B65C-7ECE-4364-8959-D1683684E146
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, vínculo, protocolo de la tienda windows, vincular a una aplicación, vincular a aplicación
ms.localizationpriority: medium
ms.openlocfilehash: de22505cf42193932a5bbd951c983e02eea37bd7
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259985"
---
# <a name="link-to-your-app"></a>Vincular a la aplicación


Puede ayudar a los clientes a detectar la aplicación mediante un vínculo a la lista de la aplicación en el Microsoft Store.

## <a name="getting-the-link-to-your-apps-store-listing"></a>Obtener el vínculo a la descripción de la aplicación en la Tienda

Para obtener la dirección URL de la descripción de la Tienda de tu aplicación, desplázate a la página [Identidad de la aplicación](view-app-identity-details.md) en la sección **Administración de aplicaciones**. La dirección URL está en el formato **`https://www.microsoft.com/store/apps/<your app's Store ID>`** .

Cuando un cliente hace clic en este vínculo, se abre la página de descripción basada en web de la aplicación. En los dispositivos Windows, la aplicación Tienda también se iniciará y mostrará la descripción de tu aplicación.


## <a name="linking-to-your-apps-store-listing-with-the-microsoft-store-badge"></a>Vinculación a la lista de tiendas de la aplicación con el distintivo de Microsoft Store

Puede vincular directamente a la lista de la aplicación con un distintivo personalizado para que los clientes sepan que la aplicación está en el Microsoft Store.

Para crear su distintivo, visite la página de [distintivos de Microsoft Store](https://developer.microsoft.com/store/badges) . Debes tener el **Id. de la Tienda** de 12 caracteres de la aplicación para generar el distintivo y un vínculo. Encontrarás el **Id. de la Tienda** de la aplicación en la página [Identidad de la aplicación](view-app-identity-details.md) en la sección **Administración de aplicaciones**.

> [!NOTE]
> Consulte las [directrices de marketing de aplicaciones](app-marketing-guidelines.md) para obtener información y conocer los requisitos relacionados con el uso del distintivo de Microsoft Store.


## <a name="linking-directly-to-your-app-in-the-microsoft-store"></a>Vincular directamente a la aplicación en el Microsoft Store

Puede crear un vínculo que inicie el Microsoft Store y irá directamente a la página de lista de la aplicación sin abrir un explorador mediante el esquema de URI **MS-Windows-Store:** .

Estos vínculos son muy útiles cuando sabes que los usuarios usan un dispositivo Windows y quieres que lleguen directamente a la página de descripción en la Tienda. Por ejemplo, es posible que quieras usar este vínculo después de comprobar las cadenas de agente de usuario en un navegador para confirmar que el sistema operativo del usuario es compatible con la Tienda, o cuando ya te estás comunicando a través de una aplicación para UWP.

Para usar este esquema de URI para vincular directamente a la lista de tiendas de la aplicación, Anexe el identificador de almacén de la aplicación a este vínculo:

`ms-windows-store://pdp/?ProductId=`

Para obtener más información sobre el uso del Protocolo de Microsoft Store, consulte [Inicio de la aplicación de Microsoft](../launch-resume/launch-store-app.md).

 

 




