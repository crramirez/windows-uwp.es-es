---
description: Puede ayudar a los clientes a detectar la aplicación mediante un vínculo a la lista de la aplicación en el Microsoft Store.
title: Vincular a la aplicación
ms.assetid: 5420B65C-7ECE-4364-8959-D1683684E146
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, vínculo, protocolo de la tienda Windows, vincular a una aplicación, vincular a aplicación
ms.localizationpriority: medium
ms.openlocfilehash: 916f82714feb65e3d9d4db48703c831b8128021c
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033921"
---
# <a name="link-to-your-app"></a>Vincular a la aplicación


Puede ayudar a los clientes a detectar la aplicación mediante un vínculo a la lista de la aplicación en el Microsoft Store.

## <a name="getting-the-link-to-your-apps-store-listing"></a>Obtener el vínculo a la descripción de la aplicación en la Tienda

Para obtener la dirección URL de la lista de tiendas de la aplicación, vaya a la página de [identidad](view-app-identity-details.md) de la aplicación en la sección **Administración de aplicaciones** . La dirección URL está en el formato **`https://www.microsoft.com/store/apps/<your app's Store ID>`** .

Cuando un cliente hace clic en este vínculo, se abre la página de lista basada en Web de la aplicación. En los dispositivos Windows, la aplicación de la tienda también iniciará y mostrará la lista de la aplicación.


## <a name="linking-to-your-apps-store-listing-with-the-microsoft-store-badge"></a>Vinculación a la lista de tiendas de la aplicación con el distintivo de Microsoft Store

Puede vincular directamente a la lista de la aplicación con un distintivo personalizado para que los clientes sepan que la aplicación está en el Microsoft Store.

Para crear su distintivo, visite la página de [distintivos de Microsoft Store](https://developer.microsoft.com/store/badges) . Deberá tener el **identificador de almacén** de 12 caracteres de la aplicación para generar el distintivo y el vínculo. Puede encontrar el identificador de la **tienda** de la aplicación en la página [identidad](view-app-identity-details.md) de la aplicación en la sección **Administración de aplicaciones** .

> [!NOTE]
> Consulte las [directrices de marketing de aplicaciones](app-marketing-guidelines.md) para obtener información y conocer los requisitos relacionados con el uso del distintivo de Microsoft Store.


## <a name="linking-directly-to-your-app-in-the-microsoft-store"></a>Vincular directamente a la aplicación en el Microsoft Store

Puede crear un vínculo que inicie el Microsoft Store y irá directamente a la página de lista de la aplicación sin abrir un explorador mediante el esquema de URI **MS-Windows-Store:** .

Estos vínculos son útiles si sabe que los usuarios están en un dispositivo Windows y desea que lleguen directamente a la página de lista del almacén. Por ejemplo, puede que desee usar este vínculo después de comprobar las cadenas de agente de usuario en un explorador para confirmar que el sistema operativo del usuario es compatible con el almacén o cuando ya se está comunicando a través de una aplicación para UWP.

Para usar este esquema de URI para vincular directamente a la lista de tiendas de la aplicación, Anexe el identificador de almacén de la aplicación a este vínculo:

`ms-windows-store://pdp/?ProductId=`

Para obtener más información sobre el uso del Protocolo de Microsoft Store, consulte [Inicio de la aplicación de Microsoft](../launch-resume/launch-store-app.md).

 

 




