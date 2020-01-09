---
description: La API de solicitud de pago proporciona una solución integrada para que las aplicaciones UWP omitan el proceso de solicitar a un usuario que escriba la información de pago y seleccione métodos de envío.
title: Simplificar los pagos con la API de solicitud de pago
ms.date: 09/26/2017
ms.topic: article
keywords: Windows 10, UWP, solicitud de pago
ms.openlocfilehash: 3e54767496d9c8d25ce42ea389f43ca2075d128d
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2020
ms.locfileid: "75685043"
---
# <a name="simplify-payments-with-the-payment-request-api"></a>Simplificar los pagos con la API de solicitud de pago
La API de solicitud de pago para aplicaciones UWP se basa en la especificación de la [API de solicitud de pago de W3C](https://w3c.github.io/browser-payment-api/). Le ofrece la capacidad de simplificar el proceso de desprotección en las aplicaciones de UWP. Los usuarios pueden acelerar la desprotección mediante las opciones de pago y las direcciones de envío ya guardadas con su cuenta de Microsoft. Puede aumentar la tasa de conversión y reducir el riesgo de infracciones de datos, ya que la información de pago se convierte en tokens. A partir de Windows 10 Creators Update, los usuarios pueden usar sus opciones de pago guardadas para pagar fácilmente entre las experiencias de las aplicaciones de UWP.

## <a name="prerequisites"></a>Requisitos previos
Antes de empezar a usar la API de solicitud de pago, hay algunas cosas que debe tener en cuenta.

### <a name="getting-a-merchant-id"></a>Obtención de un identificador de comerciante
Como parte del proceso de solicitud de pago, Microsoft solicita los tokens de pago en su nombre del proveedor de servicios. Por lo tanto, antes de empezar a usar la API, necesitamos su autorización para solicitar esos tokens.  Debe seguir unos pocos pasos para registrarse en una cuenta de vendedor y proporcionar la autorización necesaria. Para ello, vaya al [centro de ventas de Microsoft](https://partner.microsoft.com/dashboard/registration/seller?accountprogram=uwp). Una vez hecho esto, copie el identificador de comerciante resultante del centro de Partners en la aplicación al construir la solicitud de pago. A continuación, cuando la aplicación envíe una solicitud de pago, recibirá un token de pago del procesador que tendrá que enviar el pago.

### <a name="geographic-restrictions-and-language-support"></a>Restricciones geográficas y compatibilidad con idiomas
La API de solicitud de pago solo la pueden usar empresas basadas en Estados Unidos para procesar transacciones en el Estados Unidos.

## <a name="using-the-payment-request-api-in-your-app-step-by-step"></a>Uso de la API de solicitud de pago en la aplicación: paso a paso
En esta sección se muestra cómo usar la [API de solicitud de pago de UWP](https://docs.microsoft.com/uwp/api/windows.applicationmodel.payments) en la aplicación. Usaremos la API aquí en su forma más sencilla para mayor claridad. Para obtener un ejemplo de uso más avanzado de estas API, consulte el [ejemplo de aplicación de compras de UWP en github](https://github.com/Microsoft/Windows-appsample-shopping).

### <a name="1-create-a-set-of-all-the-payment-options-that-you-accept"></a>1. cree un conjunto de todas las opciones de pago que acepte.
> [!Note]
> Reemplace el texto **Merchant-ID-from-vendedor-portal** por el identificador de comerciante que recibió del centro de vendedores.

[!code-csharp[SnippetEnumerate](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetEnumerate)]

### <a name="2-pull-the-payment-details-together"></a>2. Extraiga los detalles de pago juntos. 

Estos detalles se mostrarán al usuario en la aplicación de pago. 

[!code-csharp[SnippetDisplayItems](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetDisplayItems)]

### <a name="3-include-the-sales-tax"></a>3. incluya el impuesto de ventas. 

> [!Important]
> La API no agrega elementos ni calcula el impuesto de ventas para usted. Recuerde que los tipos impositivos varían según la jurisdicción. Para mayor claridad, usamos una tasa de impuestos hipotética del 9,5%.

[!code-csharp[SnippetTaxes](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetTaxes)]

### <a name="4-optional--add-discounts-or-other-modifiers-to-the-total"></a>4. (opcional) Agregue descuentos u otros modificadores al total. 

Este es un ejemplo de cómo agregar un descuento para usar una tarjeta de crédito de Contoso específica para mostrar los elementos. (*Contoso* es un nombre ficticio).

[!code-csharp[SnippetDiscountRate](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetDiscountRate)]

### <a name="5-assemble-all-the-payment-details"></a>5. ensamble todos los detalles de pago.

[!code-csharp[SnippetAggregate](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetAggregate)]
[!code-csharp[SnippetPaymentOptions](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetPaymentOptions)]

### <a name="6-submit-the-payment-request"></a>6. Envíe la solicitud de pago. 

Llame al método **SubmitPaymentRequestAsync** para enviar la solicitud de pago. Se abrirá la aplicación Payment que muestra las opciones de pago disponibles.

[!code-csharp[SnippetSubmit](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetSubmit)]

Se solicita al usuario que inicie sesión con su cuenta de Microsoft.

Una vez que el usuario inicia sesión, puede seleccionar las opciones de pago y la dirección de envío que se guardaron previamente.

![Interfaz de usuario de solicitud de pago](./images/33.png "Interfaz de usuario de solicitud de pago")

La aplicación espera a que el usuario pulse en **pagar**y, a continuación, completa el pedido.

[!code-csharp[SnippetComplete](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetComplete)]

Una vez completado el pago, se muestra al usuario la pantalla **pedido confirmado** .

![Orden confirmado](./images/44.png "Orden confirmado")

## <a name="see-also"></a>Consulta también
- [Documentación de referencia de Windows. ApplicationModel. Payments](https://docs.microsoft.com/uwp/api/windows.applicationmodel.payments)
- [Ejemplo de aplicación de compras de UWP en GitHub](https://github.com/Microsoft/Windows-appsample-shopping)
- [Especificación de la API de solicitud de pago de W3C](https://www.w3.org/TR/payment-request/)
- [API de solicitud de pago](https://docs.microsoft.com/microsoft-edge/dev-guide/windows-integration/payment-request-api)

