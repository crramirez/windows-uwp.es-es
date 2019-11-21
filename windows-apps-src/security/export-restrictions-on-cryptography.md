---
title: Restricciones de exportación sobre la criptografía
description: Usa esta información para determinar si la aplicación usa algún tipo de criptografía que impida que se muestre en la Microsoft Store.
ms.assetid: 204C7D1D-6F08-4AEE-A333-434D715E7617
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, security
ms.localizationpriority: medium
ms.openlocfilehash: c647d91213ddf1fd8a3dafd80c6888a026cda576
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258935"
---
# <a name="export-restrictions-on-cryptography"></a>Restricciones de exportación sobre la criptografía



Usa esta información para determinar si la aplicación usa algún tipo de criptografía que impida que se muestre en la Microsoft Store.

La Oficina de Industria y Seguridad del Departamento de Comercio de Estados Unidos regula las exportaciones de tecnología que usan determinados tipos de cifrado. Todas las aplicaciones que se muestran en la Microsoft Store deben cumplir estas leyes y reglamentos, porque los archivos de la aplicación podrían almacenarse en Estados Unidos. Incluso las aplicaciones cargadas por desarrolladores de otros países para su distribución fuera de Estados Unidos deben cumplir con estas normas. Por lo tanto, cuando cualquier desarrollador de aplicaciones envíe una aplicación a la Microsoft Store, deberá asegurarse de que sus aplicaciones no contienen ninguna tecnología restringida según estas normas.

> **Tenga en cuenta**  la información proporcionada aquí proporciona algunas instrucciones, pero es responsabilidad suya del desarrollador de aplicaciones que está publicando aplicaciones en el Microsoft Store para asegurarse de que la aplicación cumple con todas las leyes y regulaciones aplicables.

 

Para obtener más información sobre la Oficina de Industria y Seguridad y el Departamento de Comercio de Estados Unidos, consulta la página [acerca de la Oficina de Industria y Seguridad](https://www.bis.doc.gov/about/index.htm).

Para obtener información sobre la Normativa de la Administración de Exportaciones (EAR) que regula la exportación de tecnología que incluya cifrado, consulta la página [EAR Controls for Items That Use Encryption (Controles de la EAR para productos que usen cifrado)](https://www.bis.doc.gov/index.php/policy-guidance/encryption).

## <a name="governed-uses"></a>Usos regulados

En primer lugar, determina si tu aplicación usa un tipo de criptografía que esté regulada por la Normativa de la Administración de Exportaciones. La pregunta incluye los ejemplos que se muestran en esta lista, pero recuerda que esta lista no incluye cada aplicación de criptografía posible.

> **Importante**  considere no solo el código que escribió para la aplicación, sino también todas las bibliotecas de software, las utilidades y los componentes del sistema operativo que incluye la aplicación o a los que se vincula.

-   Todo uso de firmas digitales, como autenticaciones o controles de integridad
-   Cifrado de cualquier dato o archivos que la aplicación usa o a los que accede
-   Administración de claves, administración de certificados o cualquier cosa que interactúe con una infraestructura de clave pública
-   Uso de un canal de comunicación seguro como NTLM, Kerberos, Capa de sockets seguros (SSL) o Seguridad de la capa de transporte (TLS)
-   Contraseñas de cifrado u otras formas de seguridad de la información
-   Protección de copias o administración de derechos digitales (DRM)
-   Protección antivirus

Para obtener una lista completa y actualizada de las aplicaciones criptográficas, consulta [EAR Controls for Items That Use Encryption (Controles de la EAR para productos que usen cifrado)](https://www.bis.doc.gov/index.php/policy-guidance/encryption).

## <a name="non-restricted-uses"></a>Usos no restringidos

Ten en cuenta que algunas de las aplicaciones de criptografía no están restringidas. Estas son algunas de las tareas sin restricciones:

-   Cifrado de contraseñas
-   Protección de copias
-   Authentication
-   Administración de derechos digitales
-   Uso de firmas digitales

Para obtener una lista completa y actualizada de las aplicaciones criptográficas, consulta [EAR Controls for Items That Use Encryption (Controles de la EAR para productos que usen cifrado)](https://www.bis.doc.gov/index.php/policy-guidance/encryption).

Si la aplicación llama, admite, tiene o usa criptografía o cifrado para cualquier tarea que no esté en esta lista, necesita un Número de clasificación de control de exportación (ECCN).

Si no tienes un número ECCN, consulta el tema sobre [ECCN Questions and Answers (Preguntas y respuestas sobre ECCN)](https://www.bis.doc.gov/licensing/do_i_needaneccn.html).
