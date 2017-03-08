---
title: "Restricciones de exportación sobre la criptografía"
description: "Usa esta información para determinar si la aplicación usa algún tipo de criptografía que impida que se muestre en la Tienda Windows."
ms.assetid: 204C7D1D-6F08-4AEE-A333-434D715E7617
author: awkoren
ms.author: alkoren
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 265f0f0d9db1a8ae53a8d6561e289f8e303e08b1
ms.lasthandoff: 02/07/2017

---

# <a name="export-restrictions-on-cryptography"></a>Restricciones de exportación sobre la criptografía


\[ Actualizado para las aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Usa esta información para determinar si la aplicación usa algún tipo de criptografía que impida que se muestre en la Tienda Windows.

La Oficina de Industria y Seguridad del Departamento de Comercio de Estados Unidos regula las exportaciones de tecnología que usan determinados tipos de cifrado. Todas las aplicaciones que se muestran en la Tienda Windows deben cumplir con estas leyes y reglamentos porque los archivos de la aplicación podrían almacenarse en Estados Unidos. Incluso las aplicaciones cargadas por desarrolladores de otros países para su distribución fuera de Estados Unidos deben cumplir con estas normas. Por lo tanto, cuando cualquier desarrollador de aplicaciones envíe una aplicación a la Tienda Windows, deberá asegurarse de que sus aplicaciones no contienen ninguna tecnología restringida según estas normas.

> **Nota** La información que se proporciona aquí es orientativa, pero es tu responsabilidad como desarrollador que publica aplicaciones en la Tienda Windows comprobar que la aplicación cumpla todas las leyes y normativas aplicables.

 

Para obtener más información sobre la Oficina de Industria y Seguridad y el Departamento de Comercio de Estados Unidos, consulta la página [acerca de la Oficina de Industria y Seguridad](http://go.microsoft.com/fwlink/p/?LinkID=245644).

Para obtener información sobre la Normativa de la Administración de Exportaciones (EAR) que regula la exportación de tecnología que incluya cifrado, consulta la página [EAR Controls for Items That Use Encryption (Controles de la EAR para productos que usen cifrado)](http://go.microsoft.com/fwlink/p/?LinkID=245645).

## <a name="governed-uses"></a>Usos regulados

En primer lugar, determina si tu aplicación usa un tipo de criptografía que esté regulada por la Normativa de la Administración de Exportaciones. La pregunta incluye los ejemplos que se muestran en esta lista, pero recuerda que esta lista no incluye cada aplicación de criptografía posible.

> **Importante** No solo debes tener en cuenta el código que has escrito para la aplicación, sino también todas las bibliotecas de software, utilidades y componentes del sistema operativo que incluye la aplicación o con los que tiene un vínculo.

-   Todo uso de firmas digitales, como autenticaciones o controles de integridad
-   Cifrado de cualquier dato o archivos que la aplicación usa o a los que accede
-   Administración de claves, administración de certificados o cualquier cosa que interactúe con una infraestructura de clave pública
-   Uso de un canal de comunicación seguro como NTLM, Kerberos, Capa de sockets seguros (SSL) o Seguridad de la capa de transporte (TLS)
-   Contraseñas de cifrado u otras formas de seguridad de la información
-   Protección de copias o administración de derechos digitales (DRM)
-   Protección antivirus

Para obtener una lista completa y actualizada de las aplicaciones criptográficas, consulta [EAR Controls for Items That Use Encryption (Controles de la EAR para productos que usen cifrado)](http://go.microsoft.com/fwlink/p/?LinkID=245645).

## <a name="non-restricted-uses"></a>Usos no restringidos

Ten en cuenta que algunas de las aplicaciones de criptografía no están restringidas. Estas son algunas de las tareas sin restricciones:

-   Cifrado de contraseñas
-   Protección de copias
-   Autenticación
-   Administración de derechos digitales
-   Uso de firmas digitales

Para obtener una lista completa y actualizada de las aplicaciones criptográficas, consulta [EAR Controls for Items That Use Encryption (Controles de la EAR para productos que usen cifrado)](http://go.microsoft.com/fwlink/p/?LinkID=245645).

Si la aplicación llama, admite, tiene o usa criptografía o cifrado para cualquier tarea que no esté en esta lista, necesita un Número de clasificación de control de exportación (ECCN).

Si no tienes un número ECCN, consulta el tema sobre [ECCN Questions and Answers (Preguntas y respuestas sobre ECCN)](http://go.microsoft.com/fwlink/p/?LinkID=245646).

