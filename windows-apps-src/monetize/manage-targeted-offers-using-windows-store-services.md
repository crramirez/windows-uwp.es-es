---
ms.assetid: 9F0A59A1-FAD7-4AD5-B78B-C1280F215D23
description: Usa la API de ofertas dirigidas de Microsoft Store para obtener ofertas dirigidas que están disponibles para el usuario actual de la aplicación.
title: Administrar ofertas dirigidas usando los servicios de la Store
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, servicios Microsoft Store, API de ofertas de destino de Microsoft Store, ofertas dirigidas
ms.localizationpriority: medium
ms.openlocfilehash: 27d99d2008352ff291f0cb620afab8ccb8f6977c
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2018
ms.locfileid: "8713022"
---
# <a name="manage-targeted-offers-using-store-services"></a>Administrar ofertas dirigidas usando los servicios de la Store

Si creas una *oferta dirigida* en el **interactuar > ofertas dirigidas** página de la aplicación Centro de partners, usa la *API de ofertas dirigidas de Microsoft Store* en el código de la aplicación para recuperar información que te ayudará a implementar la experiencia de aplicación para la oferta dirigida. Para obtener más información sobre las ofertas dirigidas y cómo crearlas en el panel, consulta [Usar ofertas dirigidas para maximizar la interacción y las conversiones](../publish/use-targeted-offers-to-maximize-engagement-and-conversions.md).

La API de ofertas dirigidas es una API REST simple que puedes utilizar para obtener las ofertas dirigidas que están disponibles para el usuario actual, en función de si el usuario es parte del segmento de cliente para la oferta dirigida o no. Para usar esta API en el código de tu aplicación, sigue estos pasos:

1.  [Obtén un token de cuenta de Microsoft](#obtain-a-microsoft-account-token) para el usuario que inició sesión actualmente en la aplicación.
2.  [Obtén las ofertas dirigidas para el usuario actual](#get-targeted-offers).
3.  Implementa la experiencia de compra desde la aplicación para el complemento que está asociado con una de las ofertas dirigidas. Para obtener más información sobre la implementación de compras desde la aplicación , consulta [este artículo](enable-in-app-purchases-of-apps-and-add-ons.md).

Para ver un ejemplo de código completo que muestra todos estos pasos, consulta el [ejemplo de código](#code-example) al final de este artículo. En las siguientes secciones se proporcionan más detalles sobre cada paso.

<span id="obtain-a-microsoft-account-token" />

## <a name="get-a-microsoft-account-token-for-the-current-user"></a>Obtener un token de cuenta de Microsoft para el usuario actual

En el código de la aplicación, obtén un token de cuenta de Microsoft (MSA) para el usuario que inició sesión actualmente. Debes pasar este token en el encabezado de la solicitud ```Authorization``` para cada uno de los métodos de la API de ofertas de destino de Microsoft Store. La Store usa este token para recuperar las ofertas dirigidas que están a disposición del usuario actual.

Para obtener el token de MSA, usa la clase [WebAuthenticationCoreManager](https://docs.microsoft.com/uwp/api/windows.security.authentication.web.core.webauthenticationcoremanager) para solicitar un token usando el ámbito ```devcenter_implicit.basic,wl.basic```. En el siguiente ejemplo, se muestra cómo hacerlo. Este ejemplo es un fragmento del [ejemplo completo](#code-example) y requiere instrucciones **using** que se proporcionan en el ejemplo completo.

[!code-cs[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#GetMSAToken)]

Para obtener más información sobre cómo obtener tokens de MSA, consulta [Administrador de cuentas web](../security/web-account-manager.md).

<span id="get-targeted-offers" />

## <a name="get-the-targeted-offers-for-the-current-user"></a>Obtener las ofertas dirigidas para el usuario actual

Una vez que tengas un token de MSA para el usuario actual, llama el método GET de la URI ```https://manage.devcenter.microsoft.com/v2.0/my/storeoffers/user``` para obtener las ofertas dirigidas disponibles para el usuario actual. Para obtener más información sobre este método REST, consulta [Obtener ofertas dirigidas](get-targeted-offers.md).

Este método devuelve los identificadores de producto de los complementos que están asociados con las ofertas dirigidas que están disponibles para el usuario actual. Con esta información, puedes ofrecer al usuario una o varias de las ofertas dirigidas como una compra desde la aplicación.

El siguiente ejemplo muestra cómo obtener las ofertas dirigidas para el usuario actual. Este ejemplo es un fragmento del [ejemplo completo](#code-example). Requiere la biblioteca de [Json.NET](http://www.newtonsoft.com/json) de Newtonsoft y clases adicionales e instrucciones **using** que se proporcionan en el ejemplo completo.

[!code-cs[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#GetTargetedOffers)]

<span id="code-example" />

## <a name="complete-code-example"></a>Ejemplo de código completo

En el siguiente ejemplo de código se demuestran las siguientes tareas:

* Obtener un token de MSA para el usuario actual.
* Obtener todas las ofertas dirigidas para el usuario actual mediante el método [Obtener ofertas dirigidas](get-targeted-offers.md).
* Compra el complemento que está asociado con una oferta dirigida.

Este ejemplo requiere la biblioteca [Json.NET](http://www.newtonsoft.com/json) de Newtonsoft. El ejemplo usa esta biblioteca para serializar y deserializar los datos con formato JSON.

[!code-cs[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#GetTargetedOffersSample)]

## <a name="related-topics"></a>Artículos relacionados

* [Usar ofertas dirigidas para maximizar la interacción y las conversiones](../publish/use-targeted-offers-to-maximize-engagement-and-conversions.md)
* [Obtener ofertas dirigidas](get-targeted-offers.md)
