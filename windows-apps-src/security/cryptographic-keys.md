---
title: Claves criptográficas
description: En este artículo se muestra cómo usar funciones de derivación de claves estándar para derivar claves y cómo cifrar contenido mediante claves simétricas y asimétricas.
ms.assetid: F35BEBDF-28C5-4F91-A94E-F7D862B6ED59
author: msatranjr
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, seguridad
ms.localizationpriority: medium
ms.openlocfilehash: d9a47ba36c84176e83a81dd234d05315aa1e2525
ms.sourcegitcommit: 9f8010fe67bb3372db1840de9f0be36097ed6258
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2018
ms.locfileid: "7102424"
---
# <a name="cryptographic-keys"></a>Claves criptográficas




En este artículo se muestra cómo usar funciones de derivación de claves estándar para derivar claves y cómo cifrar contenido mediante claves simétricas y asimétricas. 

## <a name="symmetric-keys"></a>Claves simétricas


El cifrado de clave simétrica, también denominado cifrado de clave secreta, necesita usar para el descifrado la misma la clave que se usó para el cifrado. También puedes usar una clase [**SymmetricKeyAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241537) para especificar un algoritmo simétrico y crear o importar una clave. Puedes usar métodos estáticos en la clase [**CryptographicEngine**](https://msdn.microsoft.com/library/windows/apps/br241490) para cifrar y descifrar datos con el algoritmo y la clave.

El cifrado de clave simétrica suele usar cifrados de bloques y modos de cifrado de bloques. Un cifrado de bloques es una función de cifrado simétrica que se aplica a bloques de tamaño fijo. Si el mensaje que quieres cifrar es más largo que la longitud del bloque, debes usar un modo de cifrado de bloques. Un modo de cifrado de bloques es una función de cifrado simétrica que se crea mediante un cifrado de bloques. Cifra texto no cifrado como una serie de bloques de tamaño fijo. Se admiten los siguientes modos para las aplicaciones:

-   El modo ECB (libro de códigos electrónico) cifra cada bloque del mensaje por separado. Este modo de cifrado no se considera seguro.
-   El modo CBC (encadenamiento de bloques cifrados) usa el bloque de texto cifrado anterior para ofuscar el bloque actual. Debes determinar el valor que hay que utilizar para el primer bloque. Este valor se denomina vector de inicialización (IV).
-   El modo CCM (contador con CBC-MAC) combina el modo de cifrado de bloques CBC con un código de autenticación de mensaje (MAC).
-   El modo GCM (modo de contador Galois) combina el modo de cifrado de contador con el modo de autenticación Galois.

Algunos modos como CBC requieren que se utilice un vector de inicialización (IV) para el primer bloque de texto cifrado. A continuación se muestran vectores de inicialización comunes. El usuario especifica el IV al llamar a [**CryptographicEngine.Encrypt**](https://msdn.microsoft.com/library/windows/apps/br241494). En la mayoría de los casos es importante no reutilizar el IV con la misma clave.

-   Fijo usa el mismo IV para todos los mensajes que se van a cifrar. Así se pierde información y no se recomienda su uso.
-   Contador incrementa el IV para cada bloque.
-   Aleatorio crea un IV seudoaleatorio. Puedes usar [**CryptographicBuffer.GenerateRandom**](https://msdn.microsoft.com/library/windows/apps/br241392) para crear el IV.
-   Generado por valor nonce usa un número único para cada mensaje que se va a cifrar. Generalmente, el valor de seguridad (nonce) es un mensaje modificado o un identificador de transacción. No es necesario mantener en secreto el nonce, pero no se debe reutilizar con la misma clave.

La mayoría de los modos requieren que la longitud del texto no cifrado sea un múltiplo exacto del tamaño de bloque. Normalmente esto requiere rellenar el texto no cifrado para alcanzar la longitud correcta.

A diferencia de los cifrados de bloques, que cifran bloques de datos de tamaño fijo, los cifrados de flujos son funciones de cifrado simétrico que combinan fragmentos de texto no cifrado con un flujo de bits seudoaleatorio (denominado flujo de clave) para generar todo el texto cifrado. Algunos modos de cifrado de bloques, como el modo de realimentación de salida (OTF) y el modo de contador (CTR), convierten de manera efectiva un cifrado de bloques en un cifrado de secuencias. No obstante, los cifrados de flujos reales, como RC4, suelen ser más rápidos que los modos de cifrado de bloques.

El siguiente ejemplo muestra cómo usar la clase [**SymmetricKeyAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241537) para crear una clave simétrica y usarla para cifrar y descifrar datos.

## <a name="asymmetric-keys"></a>Claves asimétricas


La criptografía de clave asimétrica, también denominada criptografía de clave pública, usa una clave pública y una clave privada para realizar el cifrado y el descifrado. Las claves son distintas pero están relacionadas matemáticamente. Normalmente, la clave privada se mantiene en secreto y se usa para descifrar datos, mientras que la clave pública se distribuye a las partes interesadas y se usa para cifrar datos. La criptografía asimétrica también se usa para firmar datos.

Debido a que la criptografía asimétrica es mucho más lenta que la criptografía simétrica, pocas veces se usa para cifrar grandes cantidades de datos directamente. En cambio, se suele usar para cifrar claves de la siguiente manera.

-   María requiere que Francisco le envíe solo mensajes cifrados.
-   María crea un par de claves pública y privada, mantiene la clave privada en secreto y publica la clave pública.
-   Francisco quiere enviarle un mensaje a María.
-   Francisco crea una clave simétrica.
-   Francisco usa esta nueva clave simétrica para cifrar el mensaje que quiere enviar a María.
-   Francisco usa la clave pública de María para cifrar su clave simétrica.
-   Francisco envía el mensaje cifrado y la clave simétrica cifrada a María (doble cifrado).
-   María usa su clave privada (del par de claves pública y privada) para descifrar la clave simétrica de Francisco.
-   María usa la clave simétrica de Francisco para descifrar el mensaje.

Puedes usar un objeto [**AsymmetricKeyAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241478) para especificar un algoritmo asimétrico o un algoritmo de firma, para crear o importar un par de claves efímeras, o para importar la parte de la clave pública de un par de claves.

## <a name="deriving-keys"></a>Derivación de claves


Suele ser necesario derivar claves adicionales de un secreto compartido. Para derivar claves, puedes usar la clase [**KeyDerivationAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241518) y uno de los siguientes métodos especializados en la clase [**KeyDerivationParameters**](https://msdn.microsoft.com/library/windows/apps/br241524).

| Objeto                                                                            | Descripción                                                                                                                                |
|-----------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| [**BuildForPbkdf2**](https://msdn.microsoft.com/library/windows/apps/br241525)    | Crea un objeto KeyDerivationParameters para usarlo en la función de derivación de claves basada en contraseña 2 (PBKDF2).                                 |
| [**BuildForSP800108**](https://msdn.microsoft.com/library/windows/apps/br241526)  | Crea un objeto KeyDerivationParameters para usarlo en una función de derivación de claves de código de autenticación de mensajes basado en hash (HMAC) en modo de contador. |
| [**BuildForSP80056a**](https://msdn.microsoft.com/library/windows/apps/br241527)  | Crea un objeto KeyDerivationParameters para usarlo en la función de derivación de claves SP800-56A.                                                 |

 
