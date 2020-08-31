---
ms.assetid: E322DFFE-8EEC-499D-87BC-EDA5CFC27551
description: Cada transacción Microsoft Store que tiene como resultado una compra de productos correcta puede devolver opcionalmente una confirmación de transacción.
title: Usar recibos para comprobar la compra de productos
ms.date: 04/16/2018
ms.topic: article
keywords: Windows 10, UWP, compras desde la aplicación, IAPs, confirmaciones, Windows. ApplicationModel. Store
ms.localizationpriority: medium
ms.openlocfilehash: 0bbdaa8164e5d3a7e660fc4667b7cfe3c090bc10
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171329"
---
# <a name="use-receipts-to-verify-product-purchases"></a>Usar recibos para comprobar la compra de productos

Cada transacción Microsoft Store que tiene como resultado una compra de productos correcta puede devolver opcionalmente una confirmación de transacción. Este recibo proporciona información sobre el producto enumerado y el coste abonado por el cliente.

El acceso a esta información es compatible con escenarios en los que la aplicación necesita comprobar que un usuario ha adquirido la aplicación o que ha realizado compras de complementos (también denominadas productos en la aplicación o IAP) desde el Microsoft Store. Por ejemplo, imagina un juego que ofrece contenido descargado. Si el usuario que compró el contenido quiere jugar en otro dispositivo, debes comprobar que ese usuario ya sea propietario del contenido. Esta es la manera de hacerlo.

> [!IMPORTANT]
> En este artículo se muestra cómo usar los miembros del espacio de nombres [Windows. ApplicationModel. Store](/uwp/api/Windows.ApplicationModel.Store) para obtener y validar una confirmación para una compra desde la aplicación. Si usa el espacio de nombres [Windows. Services. Store](/uwp/api/Windows.Services.Store) para las compras desde la aplicación (introducida en la versión 1607 de Windows 10 y disponible para los proyectos que tienen como destino **Windows 10 aniversario Edition (10,0). Compilación 14393)** o una versión posterior en Visual Studio), este espacio de nombres no proporciona una API para obtener confirmaciones de compra para compras desde la aplicación. Sin embargo, puede usar un método REST en la API de colección Microsoft Store para obtener datos de una transacción de compra. Para obtener más información, consulta [Recibos de las compras desde la aplicación](in-app-purchases-and-trials.md#receipts).

## <a name="requesting-a-receipt"></a>Solicitar un recibo


El espacio de nombres **Windows.ApplicationModel.Store** admite varias formas de obtener un recibo:

* Cuando realizas una compra usando [CurrentApp.RequestAppPurchaseAsync](/uwp/api/windows.applicationmodel.store.currentapp.requestapppurchaseasync) o [CurrentApp.RequestProductPurchaseAsync](/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync) (o una de las sobrecargas de este método), el valor devuelto contiene el recibo.
* Puedes llamar al método [CurrentApp.GetAppReceiptAsync](/uwp/api/windows.applicationmodel.store.currentapp.getappreceiptasync) para recuperar la información de recibo actual para tu aplicación y para cualquier complemento de la aplicación.

Un recibo de aplicación tiene la siguiente apariencia.

> [!NOTE]
> En este ejemplo se da formato para ayudar a que el código XML sea legible. Los envíos de aplicaciones reales no incluyen espacios en blanco entre los elementos.

> [!div class="tabbedCodeSnippets"]
```xml
<Receipt Version="1.0" ReceiptDate="2012-08-30T23:10:05Z" CertificateId="b809e47cd0110a4db043b3f73e83acd917fe1336" ReceiptDeviceId="4e362949-acc3-fe3a-e71b-89893eb4f528">
    <AppReceipt Id="8ffa256d-eca8-712a-7cf8-cbf5522df24b" AppId="55428GreenlakeApps.CurrentAppSimulatorEventTest_z7q3q7z11crfr" PurchaseDate="2012-06-04T23:07:24Z" LicenseType="Full" />
    <ProductReceipt Id="6bbf4366-6fb2-8be8-7947-92fd5f683530" ProductId="Product1" PurchaseDate="2012-08-30T23:08:52Z" ExpirationDate="2012-09-02T23:08:49Z" ProductType="Durable" AppId="55428GreenlakeApps.CurrentAppSimulatorEventTest_z7q3q7z11crfr" />
    <Signature xmlns="http://www.w3.org/2000/09/xmldsig#">
        <SignedInfo>
            <CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
            <SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256" />
            <Reference URI="">
                <Transforms>
                    <Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature" />
                </Transforms>
                <DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256" />
                <DigestValue>cdiU06eD8X/w1aGCHeaGCG9w/kWZ8I099rw4mmPpvdU=</DigestValue>
            </Reference>
        </SignedInfo>
        <SignatureValue>SjRIxS/2r2P6ZdgaR9bwUSa6ZItYYFpKLJZrnAa3zkMylbiWjh9oZGGng2p6/gtBHC2dSTZlLbqnysJjl7mQp/A3wKaIkzjyRXv3kxoVaSV0pkqiPt04cIfFTP0JZkE5QD/vYxiWjeyGp1dThEM2RV811sRWvmEs/hHhVxb32e8xCLtpALYx3a9lW51zRJJN0eNdPAvNoiCJlnogAoTToUQLHs72I1dECnSbeNPXiG7klpy5boKKMCZfnVXXkneWvVFtAA1h2sB7ll40LEHO4oYN6VzD+uKd76QOgGmsu9iGVyRvvmMtahvtL1/pxoxsTRedhKq6zrzCfT8qfh3C1w==</SignatureValue>
    </Signature>
</Receipt>
```

Un recibo de producto tiene la siguiente apariencia.

> [!NOTE]
> En este ejemplo se da formato para ayudar a que el código XML sea legible. Los recibos de producto reales no incluyen espacios en blanco entre los elementos.

> [!div class="tabbedCodeSnippets"]
```xml
<Receipt Version="1.0" ReceiptDate="2012-08-30T23:08:52Z" CertificateId="b809e47cd0110a4db043b3f73e83acd917fe1336" ReceiptDeviceId="4e362949-acc3-fe3a-e71b-89893eb4f528">
    <ProductReceipt Id="6bbf4366-6fb2-8be8-7947-92fd5f683530" ProductId="Product1" PurchaseDate="2012-08-30T23:08:52Z" ExpirationDate="2012-09-02T23:08:49Z" ProductType="Durable" AppId="55428GreenlakeApps.CurrentAppSimulatorEventTest_z7q3q7z11crfr" />
    <Signature xmlns="http://www.w3.org/2000/09/xmldsig#">
        <SignedInfo>
            <CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
            <SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256" />
            <Reference URI="">
                <Transforms>
                    <Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature" />
                </Transforms>
                <DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256" />
                <DigestValue>Uvi8jkTYd3HtpMmAMpOm94fLeqmcQ2KCrV1XmSuY1xI=</DigestValue>
            </Reference>
        </SignedInfo>
        <SignatureValue>TT5fDET1X9nBk9/yKEJAjVASKjall3gw8u9N5Uizx4/Le9RtJtv+E9XSMjrOXK/TDicidIPLBjTbcZylYZdGPkMvAIc3/1mdLMZYJc+EXG9IsE9L74LmJ0OqGH5WjGK/UexAXxVBWDtBbDI2JLOaBevYsyy+4hLOcTXDSUA4tXwPa2Bi+BRoUTdYE2mFW7ytOJNEs3jTiHrCK6JRvTyU9lGkNDMNx9loIr+mRks+BSf70KxPtE9XCpCvXyWa/Q1JaIyZI7llCH45Dn4SKFn6L/JBw8G8xSTrZ3sBYBKOnUDbSCfc8ucQX97EyivSPURvTyImmjpsXDm2LBaEgAMADg==</SignatureValue>
    </Signature>
</Receipt>
```

Puedes usar cualquiera de estos ejemplos de recibo para probar tu código de validación. Para obtener más información sobre el contenido del recibo, consulta las [descripciones de elementos y atributos](#receipt-descriptions).

## <a name="validating-a-receipt"></a>Validar un recibo

Para validar la autenticidad de un recibo, necesitas que tu sistema back-end (un servicio web o algo similar) compruebe la firma del recibo usando el certificado público. Para obtener este certificado, use la dirección URL ```https://lic.apps.microsoft.com/licensing/certificateserver/?cid=CertificateId%60%60%60, where ``` CertificateId ' ' ' es el valor de **CertificateId** en la recepción.

Este es un ejemplo del proceso de validación. Este código se ejecuta en una aplicación de consola de .NET Framework que incluye una referencia al ensamblado **System.Security**.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[ReceiptVerificationSample](./code/ReceiptVerificationSample/cs/Program.cs#ReceiptVerificationSample)]

<span id="receipt-descriptions" />

## <a name="element-and-attribute-descriptions-for-a-receipt"></a>Descripciones de elementos y atributos de un recibo

Esta sección describe los elementos y atributos de un recibo.

### <a name="receipt-element"></a>Elemento de recibo

El elemento raíz de este archivo es el elemento **Recibo** que contiene información sobre la aplicación y compras desde la aplicación. Este elemento contiene los siguientes elementos secundarios.

|  Elemento  |  Obligatorio  |  Cantidad  |  Descripción   |
|-------------|------------|--------|--------|
|  [AppReceipt](#appreceipt)  |    No        |  0 o 1  |  Contiene información de compra de la aplicación actual.            |
|  [ProductReceipt](#productreceipt)  |     No       |  0 o más    |   Contiene información sobre una compra desde la aplicación de la aplicación actual.     |
|  Firma  |      Sí      |  1   |   Este elemento es un estándar de [construcción XML DSIG](https://www.w3.org/TR/xmldsig-core/). Contiene un elemento de **SignatureValue**, que contiene la firma que puedes usar para validar el recibo, y un elemento de **SignedInfo**.      |

**Receipt** tiene los atributos siguientes.

|  Atributo  |  Descripción   |
|-------------|-------------------|
|  **Versión**  |    Número de versión del recibo.            |
|  **CertificateId**  |     La huella digital de certificado usada para firmar el recibo.          |
|  **ReceiptDate**  |    Fecha en la que se firmó y descargo el recibo.           |  
|  **ReceiptDeviceId**  |   Identifica el dispositivo usado para solicitar este recibo.         |  |

<span id="appreceipt" />

### <a name="appreceipt-element"></a>Elemento AppReceipt

Este elemento contiene la información de compra de la aplicación actual.

**AppReceipt** tiene los atributos siguientes.

|  Atributo  |  Descripción   |
|-------------|-------------------|
|  **Id**  |    Identifica la compra.           |
|  **AppId**  |     El valor de Nombre de familia de paquete que usa el sistema operativo para la aplicación.           |
|  **LicenseType**  |    **Completa**, si el usuario ha comprado la versión completa de la aplicación. **Prueba**, si el usuario ha descargado una versión de prueba de la aplicación.           |  
|  **PurchaseDate**  |    Fecha cuando se adquirió la aplicación.          |  |

<span id="productreceipt" />

### <a name="productreceipt-element"></a>Elemento ProductReceipt

Este elemento contiene información sobre una compra desde la aplicación de la aplicación actual.

**ProductReceipt** tiene los atributos siguientes.

|  Atributo  |  Descripción   |
|-------------|-------------------|
|  **Id**  |    Identifica la compra.           |
|  **AppId**  |     Identifica la aplicación a través del cual el usuario realizó la compra.           |
|  **ProductId**  |     Identifica el producto comprado.           |
|  **ProductType**  |    Determina el tipo de producto. Actualmente solo admite un valor de **Duradero**.          |  
|  **PurchaseDate**  |    Fecha cuando se realizó la compra.          |  |

 

 