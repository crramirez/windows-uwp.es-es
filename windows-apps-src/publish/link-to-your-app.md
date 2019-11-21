---
Description: You can help customers discover your app by linking to your app's listing in the Microsoft Store.
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


You can help customers discover your app by linking to your app's listing in the Microsoft Store.

## <a name="getting-the-link-to-your-apps-store-listing"></a>Obtener el vínculo a la descripción de la aplicación en la Tienda

Para obtener la dirección URL de la descripción de la Tienda de tu aplicación, desplázate a la página [Identidad de la aplicación](view-app-identity-details.md) en la sección **Administración de aplicaciones**. La dirección URL está en el formato **`https://www.microsoft.com/store/apps/<your app's Store ID>`** .

Cuando un cliente hace clic en este vínculo, se abre la página de descripción basada en web de la aplicación. En los dispositivos Windows, la aplicación Tienda también se iniciará y mostrará la descripción de tu aplicación.


## <a name="linking-to-your-apps-store-listing-with-the-microsoft-store-badge"></a>Linking to your app's Store listing with the Microsoft Store badge

You can link directly to your app's listing with a custom badge to let customers know your app is in the Microsoft Store.

To create your badge, visit the [Microsoft Store badges](https://developer.microsoft.com/store/badges) page. Debes tener el **Id. de la Tienda** de 12 caracteres de la aplicación para generar el distintivo y un vínculo. Encontrarás el **Id. de la Tienda** de la aplicación en la página [Identidad de la aplicación](view-app-identity-details.md) en la sección **Administración de aplicaciones**.

> [!NOTE]
> See [App marketing guidelines](app-marketing-guidelines.md) for info and requirements related to your use of the Microsoft Store badge.


## <a name="linking-directly-to-your-app-in-the-microsoft-store"></a>Linking directly to your app in the Microsoft Store

You can create a link that launches the Microsoft Store and goes directly to your app's listing page without opening a browser by using the **ms-windows-store:** URI scheme.

Estos vínculos son muy útiles cuando sabes que los usuarios usan un dispositivo Windows y quieres que lleguen directamente a la página de descripción en la Tienda. Por ejemplo, es posible que quieras usar este vínculo después de comprobar las cadenas de agente de usuario en un navegador para confirmar que el sistema operativo del usuario es compatible con la Tienda, o cuando ya te estás comunicando a través de una aplicación para UWP.

To use this URI scheme to link directly to your app's Store listing, append your app's Store ID to this link:

`ms-windows-store://pdp/?ProductId=`

For more about using the Microsoft Store protocol, see [Launch the Microsoft app](../launch-resume/launch-store-app.md).

 

 




