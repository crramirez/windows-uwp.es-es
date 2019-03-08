---
description: La API de solicitud de pago proporciona una solución integrada para aplicaciones UWP omitir el proceso de solicitar al usuario que especifique la información de pago y seleccionar métodos de trasvase de registros.
title: Simplificar los pagos con la API de solicitud de pago
ms.date: 09/26/2017
ms.topic: article
keywords: Windows 10, uwp, solicitud de pago
ms.openlocfilehash: e5fb5cead7833b8cc213c6633cae6cee0da3466b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607870"
---
# <a name="simplify-payments-with-the-payment-request-api"></a>Simplificar los pagos con la API de solicitud de pago
La API de solicitud de pago para aplicaciones UWP se basa en el [especificación de la API de solicitud de pago de W3C](https://w3c.github.io/browser-payment-api/). Proporciona la capacidad de simplificar el proceso de pago en sus aplicaciones para UWP. Pueden acelerar los usuarios a través de desprotección mediante opciones de pago y ya ha guardado con su cuenta de Microsoft de direcciones de envío. Puede aumentar la tasa de conversión y reducir el riesgo de infracciones de datos porque se convierte la información de pago. A partir de Windows 10 Creators Update, los usuarios pueden usar sus opciones de guardado de pago para pagar fácilmente en todas las experiencias en aplicaciones para UWP.

## <a name="prerequisites"></a>Requisitos previos
Antes de comenzar con la API de solicitud de pago, hay algunas cosas que debe hacer o tener en cuenta.

### <a name="getting-a-merchant-id"></a>Obtener un identificador de comerciante
Como parte del proceso de solicitud de pago, Microsoft solicita tokens de pago en el nombre de su proveedor de servicios. Por lo que necesitamos su autorización para solicitar esos tokens antes de empezar a usar la API.  Debe seguir unos pasos para registrarse para una cuenta de vendedor y proporcione la autorización necesaria. Para ello, vaya a [Microsoft Seller Center](https://seller.microsoft.com/en-us/dashboard/registration/seller/?accountprogram=uwp). Una vez haya hecho esto, copie el identificador de comerciante resultante Id. de Partner Center en su aplicación al construir la solicitud de pago. A continuación, cuando la aplicación envía una solicitud de pago, recibirá un token de pago de su procesador que necesitará para enviar el pago.

### <a name="geographic-restrictions-and-language-support"></a>Las restricciones geográficas y compatibilidad con idiomas
La API de solicitud de pago puede usarse únicamente por las empresas en Estados Unidos para procesar las transacciones en los Estados Unidos.

## <a name="using-the-payment-request-api-in-your-app-step-by-step"></a>Mediante la API de solicitud de pago de la aplicación: paso a paso
En esta sección se muestra cómo utilizar el [API de solicitud de pago de UWP](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.payments) en la aplicación. Usamos la API de aquí en su forma más sencilla por motivos de claridad. Para obtener un ejemplo de uso más avanzado de estas API, consulte el [ejemplo de aplicación para UWP de la compra en GitHub](https://github.com/Microsoft/Windows-appsample-shopping).

### <a name="1-create-a-set-of-all-the-payment-options-that-you-accept"></a>1. Crear un conjunto de todas las opciones de pago que aceptan.
> [!Note]
> Reemplace el **merchant Id. de vendedor portal** Id. de texto con el identificador de comerciante que ha recibido desde el centro del vendedor.

[!code-cs[SnippetEnumerate](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetEnumerate)]

### <a name="2-pull-the-payment-details-together"></a>2. Reunir los detalles de pago. 

Estos detalles se mostrará al usuario en la aplicación de pago. 

[!code-cs[SnippetDisplayItems](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetDisplayItems)]

### <a name="3-include-the-sales-tax"></a>3. Incluyen los impuestos. 

> [!Important]
> La API de no agregar los elementos o calcular los impuestos por usted. Recuerde que los tipos impositivos varían según la jurisdicción. Para mayor claridad, usamos una tasa de impuesto 9.5% hipotético.

[!code-cs[SnippetTaxes](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetTaxes)]

### <a name="4-optional--add-discounts-or-other-modifiers-to-the-total"></a>4. (Opcional)  Agregar otros modificadores o descuentos al total. 

Este es un ejemplo de cómo agregar un descuento para el uso de una tarjeta de crédito Contoso específica a los elementos de presentación. (*Contoso* es un nombre ficticio.)

[!code-cs[SnippetDiscountRate](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetDiscountRate)]

### <a name="5-assemble-all-the-payment-details"></a>5. Ensamble todos los detalles de pago.

[!code-cs[SnippetAggregate](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetAggregate)]
[!code-cs[SnippetPaymentOptions](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetPaymentOptions)]

### <a name="6-submit-the-payment-request"></a>6. Enviar la solicitud de pago. 

Llame a la **SubmitPaymentRequestAsync** método para enviar la solicitud de pago. Se abre la aplicación de pago que muestra las opciones de pago disponibles.

[!code-cs[SnippetSubmit](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetSubmit)]

El usuario se le pedirá que inicie sesión con su cuenta de Microsoft.

Después de que el usuario inicia sesión, pueden seleccionar opciones de pago y dirección de envío que se guardaron anteriormente.

![Solicitud de pago de la interfaz de usuario](./images/33.png "solicitud de pago de la interfaz de usuario")

La aplicación espera a que el usuario pulse **pagar**, luego, completa el pedido.

[!code-cs[SnippetComplete](./code/PaymentsApiSample/PaymentsApiSample/MainPage.xaml.cs#SnippetComplete)]

Una vez completado el pago, se presentará al usuario un **pedido confirmado** pantalla.

![Pedido confirmado](./images/44.png "pedido confirmado ")

## <a name="see-also"></a>Consulte también
- [Documentación de referencia Windows.ApplicationModel.Payments](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.payments)
- [UWP la aplicación de ejemplo en GitHub](https://github.com/Microsoft/Windows-appsample-shopping)
- [Especificación de API de solicitud de pago de W3C](https://www.w3.org/TR/payment-request/)
- [Solicitud de pago de API ](https://docs.microsoft.com/en-us/microsoft-edge/dev-guide/device/payment-request-api)

