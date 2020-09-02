---
ms.assetid: 9F0A59A1-FAD7-4AD5-B78B-C1280F215D23
description: Use la API de ofertas de destino de Microsoft Store para obtener ofertas de destino que están disponibles para el usuario actual de la aplicación.
title: Administrar ofertas de destino mediante los servicios de almacenamiento
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, UWP, servicios de tienda, Microsoft Store API de ofertas de destino, ofertas de destino
ms.localizationpriority: medium
ms.openlocfilehash: 836ef99f827eba52699663d4a24ea58598fe3400
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363848"
---
# <a name="manage-targeted-offers-using-store-services"></a>Administrar ofertas de destino mediante los servicios de almacenamiento

Si crea una *oferta de destino* en la página **participar en > ofertas de destino** de su aplicación en el centro de Partners, use la API de ofertas de destino *Microsoft Store* en el código de la aplicación para recuperar información que le ayude a implementar la experiencia en la aplicación para la oferta de destino. Para obtener más información sobre las ofertas de destino y cómo crearlas en el panel, consulte [uso de ofertas de destino para maximizar el compromiso y las conversiones](../publish/use-targeted-offers-to-maximize-engagement-and-conversions.md).

La API de ofertas de destino es una sencilla API de REST que puede usar para obtener las ofertas de destino que están disponibles para el usuario actual, en función de si el usuario forma parte del segmento de cliente para la oferta de destino. Para usar esta API en el código de la aplicación, siga estos pasos:

1.  [Obtiene un token de cuenta de Microsoft](#obtain-a-microsoft-account-token) para el usuario que ha iniciado sesión actualmente de la aplicación.
2.  [Obtener las ofertas de destino para el usuario actual](#get-targeted-offers).
3.  Implemente la experiencia de compra desde la aplicación para el complemento que está asociado a una de las ofertas de destino. Para obtener más información sobre la implementación de compras desde la aplicación, consulte [este artículo](enable-in-app-purchases-of-apps-and-add-ons.md).

Para obtener un ejemplo de código completo en el que se muestren todos estos pasos, consulte el [ejemplo de código](#code-example) al final de este artículo. En las secciones siguientes se proporcionan más detalles sobre cada paso.

<span id="obtain-a-microsoft-account-token" />

## <a name="get-a-microsoft-account-token-for-the-current-user"></a>Obtener un token de cuenta de Microsoft para el usuario actual

En el código de la aplicación, obtenga un token de cuenta de Microsoft (MSA) para el usuario que ha iniciado sesión actualmente. Debe pasar este token en el ```Authorization``` encabezado de solicitud para la API de Microsoft Store de ofertas de destino. El almacén usa este token para recuperar las ofertas de destino que están disponibles para el usuario actual.

Para obtener el token de MSA, use la clase [WebAuthenticationCoreManager](/uwp/api/windows.security.authentication.web.core.webauthenticationcoremanager) para solicitar un token mediante el ámbito ```devcenter_implicit.basic,wl.basic``` . En el ejemplo siguiente se muestra cómo hacerlo: Este ejemplo es un extracto del [ejemplo completo](#code-example)y requiere instrucciones **using** que se proporcionan en el ejemplo completo.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs" id="GetMSAToken":::

Para obtener más información sobre cómo obtener tokens de MSA, consulte [Administrador de cuentas web](../security/web-account-manager.md).

<span id="get-targeted-offers" />

## <a name="get-the-targeted-offers-for-the-current-user"></a>Obtener las ofertas de destino para el usuario actual

Después de tener un token MSA para el usuario actual, llame al método GET del ```https://manage.devcenter.microsoft.com/v2.0/my/storeoffers/user``` URI para obtener las ofertas de destino disponibles para el usuario actual. Para obtener más información sobre este método REST, vea [obtener ofertas de destino](get-targeted-offers.md).

Este método devuelve los identificadores de producto de los complementos que están asociados a las ofertas de destino que están disponibles para el usuario actual. Con esta información, puede ofrecer al usuario una o varias de las ofertas de destino como una compra desde la aplicación.

En el ejemplo siguiente se muestra cómo obtener las ofertas de destino para el usuario actual. Este ejemplo es un extracto del [ejemplo completo](#code-example). Requiere la biblioteca [JSON.net](https://www.newtonsoft.com/json) de Newtonsoft y las clases adicionales y las instrucciones **using** que se proporcionan en el ejemplo completo.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs" id="GetTargetedOffers":::

<span id="code-example" />

## <a name="complete-code-example"></a>Finalización del ejemplo de código

En el ejemplo de código siguiente se muestran las siguientes tareas:

* Obtiene un token de MSA para el usuario actual.
* Obtener todas las ofertas de destino para el usuario actual mediante el método [Get Target offers](get-targeted-offers.md) .
* Compre el complemento asociado a una oferta de destino.

Este ejemplo requiere la biblioteca de [JSON.net](https://www.newtonsoft.com/json) de Newtonsoft. En el ejemplo se usa esta biblioteca para serializar y deserializar los datos con formato JSON.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs" id="GetTargetedOffersSample":::

## <a name="related-topics"></a>Temas relacionados

* [Usar ofertas de destino para maximizar el compromiso y las conversiones](../publish/use-targeted-offers-to-maximize-engagement-and-conversions.md)
* [Obtener ofertas dirigidas](get-targeted-offers.md)
