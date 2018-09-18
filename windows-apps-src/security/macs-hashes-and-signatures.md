---
title: MAC, hash y firmas
description: En este artículo se describe cómo se pueden utilizar los códigos de autenticación de mensaje (MAC), los hash y las firmas en las aplicaciones de la Plataforma universal de Windows (UWP) para detectar mensajes manipulados.
ms.assetid: E674312F-6678-44C5-91D9-B489F49C4D3C
author: msatranjr
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, seguridad
ms.localizationpriority: medium
ms.openlocfilehash: e7b345e520b848a3637a44fa3c3b26172c7afef0
ms.sourcegitcommit: f5321b525034e2b3af202709e9b942ad5557e193
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "4015089"
---
# <a name="macs-hashes-and-signatures"></a>MAC, hash y firmas




En este artículo se describe cómo se pueden utilizar los códigos de autenticación de mensaje (MAC), los hash y las firmas en las aplicaciones de la Plataforma universal de Windows (UWP) para detectar mensajes manipulados.

## <a name="message-authentication-codes-macs"></a>Códigos de autenticación de mensaje (MAC)


El cifrado ayuda a impedir que personas no autorizadas puedan leer un mensaje, pero no evita que una persona pueda manipular el mensaje. La modificación de un mensaje, aunque produzca algo sin sentido, puede tener un impacto grave. Los códigos de autenticación de mensaje (MAC) ayudan a prevenir la manipulación de mensajes. Por ejemplo, considera el siguiente escenario:

-   Roberto y Alicia comparten una clave secreta y se ponen de acuerdo para usar una función MAC.
-   Roberto crea un mensaje y suministra el mensaje y la clave secreta a la función MAC para obtener un valor MAC.
-   Después Roberto envía el mensaje \[sin cifrar\] y el valor MAC a Alicia a través de una red.
-   Alicia usa la clave secreta y el mensaje como entrada de la función MAC. Compara el valor MAC generado con el valor MAC enviado por Roberto. Si coinciden, significa que no se manipuló el mensaje durante el tránsito.

Eva, que interceptó la conversación de Roberto y Alicia, no puede manipular el mensaje. Eva no tiene acceso a la clave privada y, por lo tanto, no puede crear un valor MAC que pueda hacer que el mensaje manipulado para Alicia parezca genuino.

Crear un código de autenticación de mensaje solo garantiza que no se manipuló el mensaje original y, mediante una clave secreta compartida, que alguien con acceso a esa clave privada firmó el hash de mensaje.

Puedes usar el [**MacAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241530) para enumerar los algoritmos de MAC disponibles y generar una clave simétrica. Puedes usar métodos estáticos en la clase [**CryptographicEngine**](https://msdn.microsoft.com/library/windows/apps/br241490) para realizar el cifrado necesario para crear el valor MAC.

Las firmas digitales son el equivalente de clave pública a los códigos de autenticación de mensaje (MAC) de clave privada. Aunque los MAC usan claves privadas para permitir que el destinatario de un mensaje compruebe que no se ha manipulado un mensaje durante la transmisión, las firmas usan un par de claves, la pública y la privada.

Este código de ejemplo muestra cómo usar la clase [**MacAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241530) para crear un código de autenticación de mensajes basado en hash (HMAC).

```cs
using Windows.Security.Cryptography;
using Windows.Security.Cryptography.Core;
using Windows.Storage.Streams;

namespace SampleMacAlgorithmProvider
{
    sealed partial class MacAlgProviderApp : Application
    {
        public MacAlgProviderApp()
        {
            // Initialize the application.
            this.InitializeComponent();

            // Initialize the hashing process.
            String strMsg = "This is a message to be authenticated";
            String strAlgName = MacAlgorithmNames.HmacSha384;
            IBuffer buffMsg;
            CryptographicKey hmacKey;
            IBuffer buffHMAC;

            // Create a hashed message authentication code (HMAC)
            this.CreateHMAC(
                strMsg,
                strAlgName,
                out buffMsg,
                out hmacKey,
                out buffHMAC);

            // Verify the HMAC.
            this.VerifyHMAC(
                buffMsg,
                hmacKey,
                buffHMAC);
        }

        void CreateHMAC(
            String strMsg,
            String strAlgName,
            out IBuffer buffMsg,
            out CryptographicKey hmacKey,
            out IBuffer buffHMAC)
        {
            // Create a MacAlgorithmProvider object for the specified algorithm.
            MacAlgorithmProvider objMacProv = MacAlgorithmProvider.OpenAlgorithm(strAlgName);

            // Demonstrate how to retrieve the name of the algorithm used.
            String strNameUsed = objMacProv.AlgorithmName;

            // Create a buffer that contains the message to be signed.
            BinaryStringEncoding encoding = BinaryStringEncoding.Utf8;
            buffMsg = CryptographicBuffer.ConvertStringToBinary(strMsg, encoding);

            // Create a key to be signed with the message.
            IBuffer buffKeyMaterial = CryptographicBuffer.GenerateRandom(objMacProv.MacLength);
            hmacKey = objMacProv.CreateKey(buffKeyMaterial);

            // Sign the key and message together.
            buffHMAC = CryptographicEngine.Sign(hmacKey, buffMsg);

            // Verify that the HMAC length is correct for the selected algorithm
            if (buffHMAC.Length != objMacProv.MacLength)
            {
                throw new Exception("Error computing digest");
            }
         }

        public void VerifyHMAC(
            IBuffer buffMsg,
            CryptographicKey hmacKey,
            IBuffer buffHMAC)
        {
            // The input key must be securely shared between the sender of the HMAC and 
            // the recipient. The recipient uses the CryptographicEngine.VerifySignature() 
            // method as follows to verify that the message has not been altered in transit.
            Boolean IsAuthenticated = CryptographicEngine.VerifySignature(hmacKey, buffMsg, buffHMAC);
            if (!IsAuthenticated)
            {
                throw new Exception("The message cannot be verified.");
            }
        }
    }
}
```

## <a name="hashes"></a>Hash


Una función hash criptográfica toma un bloque de datos de una longitud arbitraria y devuelve una cadena de bits de tamaño fijo. Las funciones hash se suelen usar al firmar datos. Como la mayoría de las operaciones de firma con clave pública requieren realizar cálculos intensivos, suele ser más eficiente firmar (cifrar) un hash de mensaje que firmar el mensaje original. El siguiente procedimiento representa un escenario frecuente (aunque simplificado):

-   Roberto y Alicia comparten una clave secreta y se ponen de acuerdo para usar una función MAC.
-   Roberto crea un mensaje y suministra el mensaje y la clave secreta a la función MAC para obtener un valor MAC.
-   Después Roberto envía el mensaje \[sin cifrar\] y el valor MAC a Alicia a través de una red.
-   Alicia usa la clave secreta y el mensaje como entrada de la función MAC. Compara el valor MAC generado con el valor MAC enviado por Roberto. Si coinciden, significa que no se manipuló el mensaje durante el tránsito.

Ten en cuenta que Alicia envió un mensaje sin cifrar. Lo que cifró fue el hash. Este procedimiento solo garantiza que no se manipuló el mensaje original y, mediante la clave pública de Alicia, que alguien con acceso a la clave privada de Alicia (probablemente Alicia) firmó el hash de mensaje.

Puedes usar la clase [**HashAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241511) para enumerar los algoritmos hash disponibles y crear un valor [**CryptographicHash**](https://msdn.microsoft.com/library/windows/apps/br241498).

Las firmas digitales son el equivalente de clave pública a los códigos de autenticación de mensaje (MAC) de clave privada. A diferencia de los MAC, que usan claves privadas para permitir que el destinatario de un mensaje compruebe que el mensaje no se ha manipulado durante la transmisión, las firmas usan un par de claves, la pública y la privada.

El objeto [**CryptographicHash**](https://msdn.microsoft.com/library/windows/apps/br241498) puede usarse para aplicar el algoritmo hash a distintos datos repetidamente sin tener que recrear el objeto para cada uso. El método [**Append**](https://msdn.microsoft.com/library/windows/apps/br241499) agrega nuevos datos a un búfer para que se les aplique el algoritmo hash. El método [**GetValueAndReset**](https://msdn.microsoft.com/library/windows/apps/hh701376) aplica el algoritmo hash a los datos y restablece el objeto para otro uso. Esto se muestra en el siguiente ejemplo.

```cs
public void SampleReusableHash()
{
    // Create a string that contains the name of the hashing algorithm to use.
    String strAlgName = HashAlgorithmNames.Sha512;

    // Create a HashAlgorithmProvider object.
    HashAlgorithmProvider objAlgProv = HashAlgorithmProvider.OpenAlgorithm(strAlgName);

    // Create a CryptographicHash object. This object can be reused to continually
    // hash new messages.
    CryptographicHash objHash = objAlgProv.CreateHash();

    // Hash message 1.
    String strMsg1 = "This is message 1.";
    IBuffer buffMsg1 = CryptographicBuffer.ConvertStringToBinary(strMsg1, BinaryStringEncoding.Utf16BE);
    objHash.Append(buffMsg1);
    IBuffer buffHash1 = objHash.GetValueAndReset();

    // Hash message 2.
    String strMsg2 = "This is message 2.";
    IBuffer buffMsg2 = CryptographicBuffer.ConvertStringToBinary(strMsg2, BinaryStringEncoding.Utf16BE);
    objHash.Append(buffMsg2);
    IBuffer buffHash2 = objHash.GetValueAndReset();

    // Hash message 3.
    String strMsg3 = "This is message 3.";
    IBuffer buffMsg3 = CryptographicBuffer.ConvertStringToBinary(strMsg3, BinaryStringEncoding.Utf16BE);
    objHash.Append(buffMsg3);
    IBuffer buffHash3 = objHash.GetValueAndReset();

    // Convert the hashes to string values (for display);
    String strHash1 = CryptographicBuffer.EncodeToBase64String(buffHash1);
    String strHash2 = CryptographicBuffer.EncodeToBase64String(buffHash2);
    String strHash3 = CryptographicBuffer.EncodeToBase64String(buffHash3);
}

```

## <a name="digital-signatures"></a>Firmas digitales


Las firmas digitales son el equivalente de clave pública a los códigos de autenticación de mensaje (MAC) de clave privada. A diferencia de los MAC, que usan claves privadas para permitir que el destinatario de un mensaje compruebe que el mensaje no se ha manipulado durante la transmisión, las firmas usan un par de claves, la pública y la privada.

Pero como la mayoría de las operaciones de firma con clave pública requieren realizar cálculos intensivos, suele ser más eficiente firmar (cifrar) un hash de mensaje que firmar el mensaje original. El remitente crea un hash de mensaje, lo firma y envía la firma y el mensaje (sin cifrar). El destinatario calcula un hash para el mensaje, descifra la firma y compara la firma descifrada con el valor de hash. Si coinciden, el destinatario puede estar razonablemente seguro de que el mensaje proviene realmente del remitente y no se manipuló durante la transmisión.

La firma solo garantiza que no se manipuló el mensaje original y, mediante la clave pública del remitente, que alguien con acceso a la clave privada firmó el hash de mensaje.

Puedes usar un objeto [**AsymmetricKeyAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241478) para enumerar los algoritmos de firma disponibles y generar o importar un par de claves. Puedes usar métodos estáticos en la clase [**CryptographicHash**](https://msdn.microsoft.com/library/windows/apps/br241498) para firmar un mensaje o comprobar una firma.