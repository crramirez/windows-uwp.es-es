---
title: Criptografía
description: El artículo ofrece una descripción general de las características de criptografía disponibles para aplicaciones de la Plataforma universal de Windows (UWP). Para obtener información detallada sobre tareas determinadas, consulta la tabla al final de este artículo.
ms.assetid: 9C213036-47FD-4AA4-99E0-84006BE63F47
author: msatranjr
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, seguridad
ms.localizationpriority: medium
ms.openlocfilehash: 0caa3f63d8a92c75bdce10cdb277967dca21fafb
ms.sourcegitcommit: bdc40b08cbcd46fc379feeda3c63204290e055af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2018
ms.locfileid: "6153915"
---
# <a name="cryptography"></a>Criptografía




El artículo ofrece una descripción general de las características de criptografía disponibles para aplicaciones de la Plataforma universal de Windows (UWP). Para obtener información detallada sobre tareas determinadas, consulta la tabla al final de este artículo.

## <a name="terminology"></a>Terminología


En criptografía e infraestructura de clave pública (PKI) se suele utilizar la siguiente terminología.

| Término                        | Descripción                                                                                                                                                                                           |
|-----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Cifrado                  | El proceso de transformar datos mediante un algoritmo criptográfico y una clave. Los datos transformados solo se pueden recuperar con el mismo algoritmo y la misma clave (simétrica) o una clave relacionada (pública). |
| Descifrado                  | El proceso de restablecer la forma original de los datos cifrados.                                                                                                                                         |
| Texto no cifrado                   | Originalmente se utilizaba para designar un mensaje de texto sin cifrar. En la actualidad hace referencia a cualquier tipo de datos sin cifrar.                                                                                                         |
| Texto cifrado                  | Originalmente designaba un mensaje de texto cifrado y, por lo tanto, ilegible. En la actualidad hace referencia a cualquier tipo de datos cifrados.                                                                                  |
| Aplicación de algoritmo hash                     | El proceso de convertir datos de longitud variable en un valor de longitud fijo, normalmente menor. Comparando los hash, puedes estar razonablemente seguro de que dos o más elementos de datos son iguales.            |
| Firma                   | Hash cifrado de datos digitales que normalmente se utiliza para autenticar el remitente de los datos o para comprobar que no se han manipulado los datos durante la transmisión.                                               |
| Algoritmo                   | Procedimiento paso a paso para cifrar datos.                                                                                                                                                         |
| Clave                         | Un número aleatorio o seudoaleatorio que se utiliza como entrada de un algoritmo criptográfico para cifrar y descifrar datos.                                                                                               |
| Criptografía de clave simétrica  | Criptografía en la que se utiliza la misma clave para el cifrado y el descifrado. También se conoce como criptografía de clave secreta.                                                                                      |
| Criptografía de clave asimétrica | Criptografía en la que se utilizan claves distintas (aunque relacionadas matemáticamente) para el cifrado y el descifrado. También se conoce como criptografía de clave pública.                                                          |
| Codificación                    | El proceso de codificación de mensajes digitales, incluidos los certificados, para su transporte a través de una red.                                                                                                     |
| Proveedor de algoritmo          | Archivo DLL que implementa un algoritmo criptográfico.                                                                                                                                                      |
| Proveedor de almacenamiento de claves        | Un contenedor para almacenar material de clave. Actualmente, las claves se pueden almacenar en software, tarjetas inteligentes o el módulo de plataforma segura (TPM).                                                                   |
| Certificado X.509           | Un documento digital, normalmente emitido por una entidad de certificación, que permite a las partes interesadas comprobar la identidad de una persona, sistema o entidad.                                            |

 
## <a name="namespaces"></a>Espacios de nombres

Los siguientes espacios de nombres están disponibles para  usarlos en aplicaciones.

### <a name="windowssecuritycryptography"></a>Windows.Security.Cryptography

Contiene la clase CryptographicBuffer y métodos estáticos que te permiten hacer lo siguiente:

-   Convertir datos en cadenas y cadenas en datos
-   Convertir datos en matrices de bytes y matrices de bytes en datos
-   Codificar mensajes para su transporte a través de una red
-   Descodificar mensajes tras el transporte

### <a name="windowssecuritycryptographycertificates"></a>Windows.Security.Cryptography.Certificates

Contiene clases, interfaces y tipos de enumeración que te permiten hacer lo siguiente:

-   Crear una solicitud de certificado
-   Instalar una respuesta de certificado
-   Importar un certificado en un archivo PFX
-   Especificar y recuperar propiedades de solicitud de certificado

### <a name="windowssecuritycryptographycore"></a>Windows.Security.Cryptography.Core

Contiene clases y tipos de enumeración que te permiten hacer lo siguiente:

-   Cifrar y descifrar datos
-   Aplicar un algoritmo hash a datos
-   Firmar datos y comprobar firmas
-   Crear, importar y exportar claves
-   Trabajar con proveedores de algoritmos de claves asimétricas
-   Trabajar con proveedores de algoritmos de claves simétricas
-   Trabajar con proveedores de algoritmos hash
-   Trabajar con proveedores de algoritmos de códigos de autenticación de equipos (MAC)
-   Trabajar con proveedores de algoritmos de derivación de claves

### <a name="windowssecuritycryptographydataprotection"></a>Windows.Security.Cryptography.DataProtection

Contiene clases que te permiten hacer lo siguiente:

-   Cifrar y descifrar datos estáticos de manera asincrónica
-   Cifrar y descifrar secuencias de datos de manera asincrónica

## <a name="crypto-and-pki-application-capabilities"></a>Funcionalidades criptográficas y de infraestructura de clave pública para aplicaciones


La interfaz de programación de aplicaciones simplificada disponible para las aplicaciones habilita las siguientes funcionalidades criptográficas y de infraestructura de clave pública (PKI).

### <a name="cryptography-support"></a>Compatibilidad con funciones criptográficas

Puedes realizar las siguientes tareas criptográficas. Para obtener más información, consulta el espacio de nombres [**Windows.Security.Cryptography.Core**](https://msdn.microsoft.com/library/windows/apps/br241547).

-   Crear claves simétricas
-   Realizar cifrado simétrico
-   Crear claves asimétricas
-   Realizar cifrado asimétrico
-   Derivar claves basadas en contraseña
-   Crear códigos de autenticación de mensajes (MAC)
-   Contenido de hash
-   Firmar contenido digitalmente

El SDK también ofrece una interfaz simplificada para la protección de datos basada en contraseña. Puedes usarla para realizar las siguientes tareas. Para obtener más información, consulta el espacio de nombres [**Windows.Security.Cryptography.DataProtection**](https://msdn.microsoft.com/library/windows/apps/br241585).

-   Protección asincrónica de datos estáticos
-   Protección asincrónica de un flujo de datos

### <a name="encoding-support"></a>Compatibilidad con la codificación

Una aplicación puede codificar datos criptográficos para la transmisión a través de una red y descodificar datos recibidos de un origen de red. Para obtener más información, consulta los métodos estáticos disponibles en el espacio de nombres [**Windows.Security.Cryptography**](https://msdn.microsoft.com/library/windows/apps/br241404).

### <a name="pki-support"></a>Compatibilidad con PKI

Las aplicaciones pueden realizar las siguientes tareas de PKI. Para obtener más información, consulta el espacio de nombres [**Windows.Security.Cryptography.Certificates**](https://msdn.microsoft.com/library/windows/apps/br241476).

-   Crear un certificado
-   Crear un certificado autofirmado
-   Instalar una respuesta de certificado
-   Importar un certificado en formato PFX
-   Utilizar claves y certificados de tarjeta inteligente (funcionalidades sharedUserCertificates)
-   Usar certificados de Mi almacén de usuario (funcionalidades sharedUserCertificates)

Además, puedes utilizar el manifiesto para realizar las siguientes acciones:

-   Especificar certificados raíz de confianza por aplicación
-   Especificar certificados de confianza del mismo nivel por aplicación
-   Deshabilitar explícitamente la herencia de la confianza del sistema
-   Especificar los criterios de selección de certificados
    -   Solo certificados de hardware
    -   Certificados que se encadenan mediante un conjunto especificado de emisores
    -   Seleccionar automáticamente un certificado del almacén de la aplicación

## <a name="detailed-articles"></a>Artículos detallados


En los siguientes artículos se ofrecen más detalles sobre los escenarios de seguridad:

| Tema                                                                         | Descripción                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
|-------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Certificados](certificates.md)                                               | En este artículo se describe el uso de certificados en las aplicaciones para UWP. Los certificados digitales se usan en criptografía de clave pública para enlazar una clave pública a una persona, un equipo o una organización. Las identidades enlazadas suelen usarse para autenticar una entidad a otra. Por ejemplo, los certificados se suelen usar para autenticar un servidor web a un usuario y un usuario a un servidor web. Puedes crear solicitudes de certificados e instalar o importar certificados emitidos. También puedes inscribir un certificado en una jerarquía de certificados. |
| [Claves criptográficas](cryptographic-keys.md)                                   | En este artículo se muestra cómo usar funciones de derivación de claves estándar para derivar claves y cómo cifrar contenido mediante claves simétricas y asimétricas.                                                                                                                                                                                                                                                                                                                                                                             |
| [Protección de datos](data-protection.md)                                         | En este artículo se explica cómo usar la clase [DataProtectionProvider](https://msdn.microsoft.com/library/windows/apps/br241559) en el espacio de nombres [Windows.Security.Cryptography.DataProtection](https://msdn.microsoft.com/library/windows/apps/br241585) para cifrar y descifrar datos digitales en una aplicación para UWP.                                                                                                                                                                                                                  |
| [MAC, hash y firmas](macs-hashes-and-signatures.md)               | En este artículo se describe cómo se pueden usar los códigos de autenticación de mensajes (MAC), los hash y las firmas en las aplicaciones para UWP para detectar mensajes manipulados.                                                                                                                                                                                                                                                                                                                                                                                |
| [Restricciones de exportación sobre la criptografía](export-restrictions-on-cryptography.md) | Usa esta información para determinar si la aplicación usa algún tipo de criptografía que impida que se muestre en la Microsoft Store.                                                                                                                                                                                                                                                                                                                                                                                            |
| [Tareas comunes de criptografía](common-cryptography-tasks.md)                     | En estos artículos se ofrece código de ejemplo para tareas comunes de criptografía de UWP, como crear números aleatorios, comparar búferes, convertir entre cadenas y datos binarios, copiar a matrices de bytes y desde ellas, y codificar y descodificar datos.                                                                                                                                                                                                                                                                                    |

 