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
ms.sourcegitcommit: f2f4820dd2026f1b47a2b1bf2bc89d7220a79c1a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2018
ms.locfileid: "2788060"
---
# <a name="link-to-your-app"></a>Vincular a la aplicación


Puede ayudar a los clientes detectar la aplicación mediante la vinculación al listado de su aplicación en el Microsoft Store.

## <a name="getting-the-link-to-your-apps-store-listing"></a>Obtener el vínculo a la descripción de la aplicación en la Tienda

Para obtener la dirección URL de la descripción de la Tienda de tu aplicación, desplázate a la página [Identidad de la aplicación](view-app-identity-details.md) en la sección **Administración de aplicaciones**. La dirección URL está en el formato **`https://www.microsoft.com/store/apps/<your app's Store ID>`**.

Cuando un cliente hace clic en este vínculo, se abre la página de descripción basada en web de la aplicación. En los dispositivos Windows, la aplicación Tienda también se iniciará y mostrará la descripción de tu aplicación.


## <a name="linking-to-your-apps-store-listing-with-the-microsoft-store-badge"></a>Vinculación a la tienda de su aplicación con el identificador de Microsoft Store

Puede vincular directamente al listado de su aplicación con una tarjeta de identificación personalizado para que los clientes sepan que la aplicación está en Microsoft Store.

Para crear su tarjeta de identificación, visite la página de [identificadores de almacén de Microsoft](http://go.microsoft.com/fwlink/p/?LinkID=534236) . Debes tener el **Id. de la Tienda** de 12 caracteres de la aplicación para generar el distintivo y un vínculo. Encontrarás el **Id. de la Tienda** de la aplicación en la página [Identidad de la aplicación](view-app-identity-details.md) en la sección **Administración de aplicaciones**.

> [!NOTE]
> Vea [directrices de marketing de aplicación](app-marketing-guidelines.md) para la información y los requisitos relacionados con el uso de la tarjeta de Microsoft Store.


## <a name="linking-directly-to-your-app-in-the-microsoft-store"></a>Vincula directamente a la aplicación en el almacén de Microsoft

Puede crear un vínculo que inicia la Store Microsoft e irán directamente a la página del anuncio de la aplicación sin necesidad de abrir un explorador mediante el uso de la **ms-windows-store:** esquema de URI.

Estos vínculos son muy útiles cuando sabes que los usuarios usan un dispositivo Windows y quieres que lleguen directamente a la página de descripción en la Tienda. Por ejemplo, es posible que quieras usar este vínculo después de comprobar las cadenas de agente de usuario en un navegador para confirmar que el sistema operativo del usuario es compatible con la Tienda, o cuando ya te estás comunicando a través de una aplicación para UWP.

Para utilizar este esquema URI para vincular directamente a de anuncio de tienda su aplicación, anexe el identificador de la aplicación de almacenamiento a este vínculo:

`ms-windows-store://pdp/?ProductId=`

Para obtener más información acerca de cómo utilizar el protocolo de Microsoft Store, vea [iniciar la aplicación de Microsoft](../launch-resume/launch-store-app.md).

 

 




