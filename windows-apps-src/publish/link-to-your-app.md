---
Description: Puede ayudar a los clientes a detectar la aplicación mediante la vinculación a la lista de la aplicación en la Microsoft Store.
title: Vincular a la aplicación
ms.assetid: 5420B65C-7ECE-4364-8959-D1683684E146
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, vínculo, protocolo de la tienda windows, vincular a una aplicación, vincular a aplicación
ms.localizationpriority: medium
ms.openlocfilehash: 56bc051c3c5a935f3b6b26e478731fcde9c06902
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661540"
---
# <a name="link-to-your-app"></a>Vincular a la aplicación


Puede ayudar a los clientes a detectar la aplicación mediante la vinculación a la lista de la aplicación en la Microsoft Store.

## <a name="getting-the-link-to-your-apps-store-listing"></a>Obtener el vínculo a la descripción de la aplicación en la Tienda

Para obtener la dirección URL de la descripción de la Tienda de tu aplicación, desplázate a la página [Identidad de la aplicación](view-app-identity-details.md) en la sección **Administración de aplicaciones**. La dirección URL está en el formato **`https://www.microsoft.com/store/apps/<your app's Store ID>`**.

Cuando un cliente hace clic en este vínculo, se abre la página de descripción basada en web de la aplicación. En los dispositivos Windows, la aplicación Tienda también se iniciará y mostrará la descripción de tu aplicación.


## <a name="linking-to-your-apps-store-listing-with-the-microsoft-store-badge"></a>Vincular a la lista de la aplicación Store con el distintivo Microsoft Store

Puede vincular directamente a la lista de la aplicación con una notificación personalizada para informar a los clientes de que la aplicación está en la Microsoft Store.

Para crear su notificación, visite la [los distintivos de Microsoft Store](https://go.microsoft.com/fwlink/p/?LinkID=534236) página. Debes tener el **Id. de la Tienda** de 12 caracteres de la aplicación para generar el distintivo y un vínculo. Encontrarás el **Id. de la Tienda** de la aplicación en la página [Identidad de la aplicación](view-app-identity-details.md) en la sección **Administración de aplicaciones**.

> [!NOTE]
> Consulte [aplicación directrices de marketing](app-marketing-guidelines.md) para información y requisitos relacionados con el uso de la notificación de Microsoft Store.


## <a name="linking-directly-to-your-app-in-the-microsoft-store"></a>Vincular directamente a la aplicación en la Microsoft Store

Puede crear un vínculo que inicia la Microsoft Store y va directamente a la página de listado de la aplicación sin tener que abrir un explorador utilizando la **ms-windows-store:** Esquema de URI.

Estos vínculos son muy útiles cuando sabes que los usuarios usan un dispositivo Windows y quieres que lleguen directamente a la página de descripción en la Tienda. Por ejemplo, es posible que quieras usar este vínculo después de comprobar las cadenas de agente de usuario en un navegador para confirmar que el sistema operativo del usuario es compatible con la Tienda, o cuando ya te estás comunicando a través de una aplicación para UWP.

Para utilizar este esquema de URI para un vínculo directo a Store de la aplicación enumerar, anexe el Id. de la aplicación Store a este vínculo:

`ms-windows-store://pdp/?ProductId=`

Para obtener más información sobre cómo usar el protocolo de Microsoft Store, consulte [inicie la aplicación Microsoft](../launch-resume/launch-store-app.md).

 

 




