---
title: Introducción a los certificados
description: En este artículo se describe el uso de certificados en las aplicaciones de la Plataforma universal de Windows (UWP).
ms.assetid: 4EA2A9DF-BA6B-45FC-AC46-2C8FC085F90D
author: msatranjr
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, seguridad
ms.localizationpriority: medium
ms.openlocfilehash: 8caae5110b137245fd15fcc6e1b3cb61025d72ef
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2018
ms.locfileid: "7148587"
---
# <a name="intro-to-certificates"></a>Introducción a los certificados




En este artículo se describe el uso de certificados en las aplicaciones de la Plataforma universal de Windows (UWP). Los certificados digitales se usan en criptografía de clave pública para enlazar una clave pública a una persona, un equipo o una organización. Las identidades enlazadas suelen usarse para autenticar una entidad a otra. Por ejemplo, los certificados se suelen usar para autenticar un servidor web a un usuario y un usuario a un servidor web. Puedes crear solicitudes de certificados e instalar o importar certificados emitidos. También puedes inscribir un certificado en una jerarquía de certificados.

### <a name="shared-certificate-stores"></a>Almacenes de certificados compartidos

Las aplicaciones para UWP usan el nuevo modelo de aplicación aislacionista introducido en Windows8. En este modelo, las aplicaciones se ejecutan en una estructura de sistema operativo de bajo nivel, denominada contenedor de la aplicación, que prohíbe que la aplicación tenga acceso a recursos o archivos fuera de sí misma a menos que se le permita explícitamente. En las siguientes secciones se describen las implicaciones que esto tiene en la infraestructura de clave pública (PKI).

### <a name="certificate-storage-per-app-container"></a>Almacenamiento de certificados por contenedor de aplicación

Los certificados destinados al uso en un contenedor de aplicación específico se almacenan en ubicaciones de contenedor por usuario y por aplicación. Una aplicación que se ejecuta en un contenedor de aplicación tiene acceso de escritura solo a su propio almacén de certificados. Si la aplicación agrega certificados a cualquiera de sus almacenes, no los podrán leer otras aplicaciones. Si se desinstala una aplicación, también se quitarán los certificados específicos de esta. Una aplicación también tiene acceso de lectura a los almacenes de certificados del equipo local que no sean Mi almacén y el almacén de solicitudes.

### <a name="cache"></a>Memoria caché

Cada contenedor de aplicación tiene una caché aislada en la que puede almacenar certificados de emisor necesarios para la validación, listas de revocación de certificados (CRL) y respuestas de protocolo de estado de certificados en línea (OCSP).

### <a name="shared-certificates-and-keys"></a>Certificados y claves compartidos

Cuando se inserta una tarjeta inteligente en un lector, los certificados y las claves contenidos en la tarjeta se propagan a Mi almacén de usuario, donde pueden ser compartidos por cualquier aplicación de plena confianza que el usuario ejecute. Sin embargo, los contenedores de aplicación no tienen acceso a Mi almacén por usuario de manera predeterminada.

Para solucionar este problema y permitir el acceso de grupos de entidades de seguridad a grupos de recursos, el modelo de aislamiento del contenedor de aplicación admite el concepto de las funcionalidades. Una funcionalidad permite que un contenedor de aplicación acceda a un recurso específico. La funcionalidad sharedUserCertificates concede a un contenedor de aplicación acceso de lectura a los certificados y claves contenidos en Mi almacén de usuario y el almacén Raíces de confianza de tarjetas inteligentes. La funcionalidad no concede acceso de lectura al almacén de solicitudes de usuario.

La funcionalidad sharedUserCertificates del manifiesto se especifica como se indica en el siguiente ejemplo.

```xml
<Capabilities>
    <Capability Name="sharedUserCertificates" />
</Capabilities>
```

## <a name="certificate-fields"></a>Campos de un certificado


El estándar de certificado de clave pública X.509 se ha revisado varias veces desde su creación. Cada versión sucesiva de la estructura de datos ha conservado los campos que existían en las versiones anteriores y ha agregado otros nuevos, como se muestra en la siguiente ilustración.

![Certificado x.509 versiones 1, 2 y 3](images/x509certificateversions.png)

Algunos de estos campos y extensiones se pueden especificar directamente al usar la clase [**CertificateRequestProperties**](https://msdn.microsoft.com/library/windows/apps/br212079) para crear una solicitud de certificado. Pero la mayoría no se puede especificar. La autoridad emisora puede rellenar estos campos o pueden dejarse en blanco. Para obtener más información acerca de los campos, consulta las siguientes secciones:

### <a name="version-1-fields"></a>Campos de la versión 1

| Campo | Descripción |
|-------|-------------|
| Versión | Especifica el número de versión del certificado codificado. Actualmente, los valores posibles de este campo son 0, 1 o 2. |
| Número de serie | Contiene un entero positivo único asignado por la entidad de certificación (CA) al certificado. |
| Algoritmo de firma | Contiene un identificador de objeto (OID) que especifica el algoritmo usado por la CA para firmar el certificado. Por ejemplo, 1.2.840.113549.1.1.5 especifica un algoritmo hash SHA-1 combinado con el algoritmo de cifrado RSA de RSA Laboratories. |
| Emisor | Contiene el nombre distintivo (DN) X.500 de la entidad de certificación que creó y firmó el certificado. |
| Validez | Especifica el intervalo de tiempo durante el cual el certificado es válido. Las fechas hasta el final de 2049 usan el formato de Hora universal coordinada (Hora del meridiano de Greenwich) (aammddhhmmssz). Las fechas a partir del 1 de enero de 2050 usan el formato de hora generalizada (aaaammddhhmmssz). |
| Sujeto | Contiene un nombre distintivo X.500 de la entidad asociada con la clave pública contenida en el certificado. |
| Clave pública | Contiene la clave pública y la información de algoritmo asociada. |

### <a name="version-2-fields"></a>Campos de la versión 2

Un certificado X.509 versión 2 contiene los campos básicos definidos en la versión 1 y, además, agrega los siguientes campos.

| Campo | Descripción |
|-------|-------------|
| Identificador único de emisor | Contiene un valor único que se puede utilizar para hacer que el nombre X.500 de la CA no sea ambiguo si lo reutilizan distintas entidades a lo largo del tiempo. |
| Identificador único de firmante | Contiene un valor único que se puede utilizar para hacer que el nombre X.500 del firmante del certificado no sea ambiguo si lo reutilizan distintas entidades a lo largo del tiempo. |

### <a name="version-3-extensions"></a>Extensiones de la versión 3

Un certificado X.509 versión 3 contiene los campos definidos en la versión 1 y la versión 2, y además agrega extensiones de certificado.

| Campo  | Descripción |
|--------|-------------|
| Identificador de clave de entidad emisora | Identifica la clave pública de la entidad de certificación (CA) correspondiente a la clave privada de CA utilizada para firmar el certificado. |
| Restricciones básicas | Especifica si la entidad se puede utilizar como una CA y, en caso afirmativo, el número de CA subordinadas que pueden existir por debajo en la cadena de certificados. |
| Directivas del certificado | Especifica las directivas con las que se ha emitido el certificado y los fines para los que se puede utilizar. |
| Puntos de distribución CRL | Contiene el URI de la lista de revocación de certificados (CRL) de base. |
| Uso mejorado de claves | Especifica la manera en que se puede utilizar la clave pública contenida en el certificado. |
| Nombre alternativo del emisor | Especifica una o más formas de nombre alternativas para el emisor de la solicitud de certificado. |
| Uso de claves | Especifica restricciones en las operaciones que puede realizar la clave pública contenida en el certificado.|
| Restricciones de nombre  | Especifica el espacio de nombres en el que deben encontrarse todos los nombres de firmantes en una jerarquía de certificados. La extensión solo se utiliza en un certificado de CA. |
| Restricciones de directiva | Restringen la validación de rutas prohibiendo la asignación de directivas o exigiendo que cada certificado de la jerarquía contenga un identificador de directiva aceptable. La extensión solo se utiliza en un certificado de CA. |
| Asignaciones de directiva | Especifica las directivas de una CA subordinada correspondientes a directivas de la CA emisora. |
| Período de uso de clave privada | Especifica un período de validez distinto para la clave privada que para el certificado con el que la clave privada está asociada. |
| Nombre alternativo del firmante | Especifica una o más formas de nombre alternativas para el firmante de la solicitud de certificado. Algunos ejemplos de formas alternativas son direcciones de correo electrónico, nombres DNS, direcciones IP y URI. |
| Atributos de directorio de firmantes | Contiene atributos de identificación, como la nacionalidad del firmante del certificado. El valor de extensión es una secuencia de pares de valores OID. |
| Identificador de clave del firmante | Diferencia entre varias claves públicas de las que el firmante del certificado es titular. El valor de extensión suele ser un hash SHA-1 de la clave. |

