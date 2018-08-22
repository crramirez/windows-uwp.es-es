---
author: PatrickFarley
ms.assetid: e04ebe3f-479c-4b48-99d8-3dd4bb9bfaf4
title: Aprovisionamiento de Portal de dispositivo con un certificado SSL personalizado
description: TBD
ms.author: pafarley
ms.date: 07/11/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, portal de dispositivo
ms.localizationpriority: medium
ms.openlocfilehash: 1192c200cd42ab28cc7e763c06fd8a5638aa3400
ms.sourcegitcommit: f2f4820dd2026f1b47a2b1bf2bc89d7220a79c1a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2018
ms.locfileid: "2786704"
---
# <a name="provision-device-portal-with-a-custom-ssl-certificate"></a>Aprovisionamiento de Portal de dispositivo con un certificado SSL personalizado
En la actualización de los creadores del 10 de Windows, Portal de dispositivo de Windows agrega una forma para los administradores de dispositivos instalar un certificado personalizado para su uso en las comunicaciones HTTPS. 

Aunque puede hacerlo en su PC, esta característica está pensada para las empresas que tienen una infraestructura de certificado existente en su lugar.  

Por ejemplo, una empresa puede tener una entidad emisora de certificados (CA) que usa para firmar los certificados para los sitios Web de intranet que presta servidos a través de HTTPS. Esta característica de dicho significa infraestructura. 

## <a name="overview"></a>Introducción
De forma predeterminada, Portal de dispositivo genera una entidad emisora raíz autofirma y, a continuación, utiliza para firmar certificados SSL para cada extremo está escuchando en. Esto incluye `localhost`, `127.0.0.1`, y `::1` (localhost IPv6).

También incluye las nombre de host del dispositivo (por ejemplo, `https://LivingRoomPC`) y cada dirección IP de vínculo local asignada al dispositivo (hasta dos [IPv4, IPv6] para cada adaptador de red). Puede ver las direcciones IP local de vínculo para un dispositivo mirando en la herramienta de red en el Portal de dispositivo. Que se iniciarán con `10.` o `192.` para IPv4, o `fe80:` para IPv6. 

En la configuración predeterminada, es posible que aparezca una advertencia de certificado en su explorador debido a la entidad de certificación raíz de confianza. Específicamente, el certificado SSL proporcionado por el Portal de dispositivo está firmado por una CA raíz que no confía en el explorador o el PC. Esto se puede solucionar mediante la creación de una entidad de certificación raíz de confianza nueva.

## <a name="create-a-root-ca"></a>Crear una entidad de certificación raíz

Esto debe realizarse sólo si su empresa (o principal) no tiene una infraestructura de certificados configurar y sólo debe realizarse una vez. El siguiente script de PowerShell crea una raíz de la entidad emisora de certificados denominada _WdpTestCA.cer_. Instalación de este archivo a las entidades de certificación de raíz de confianza de la máquina local hará que el dispositivo para los certificados SSL que estén firmados por esta entidad de certificación raíz de confianza. Puede (y debe) instalar este archivo .cer en cada equipo que se va a conectar al Portal de dispositivo de Windows.  

```PowerShell
$CN = "PickAName"
$OutputPath = "c:\temp\"

# Create root certificate authority
$FilePath = $OutputPath + "WdpTestCA.cer"
$Subject =  "CN="+$CN
$rootCA = New-SelfSignedCertificate -certstorelocation cert:\currentuser\my -Subject $Subject -HashAlgorithm "SHA512" -KeyUsage CertSign,CRLSign
$rootCAFile = Export-Certificate -Cert $rootCA -FilePath $FilePath
```

Una vez creada, puede usar el archivo _WdpTestCA.cer_ para firmar certificados SSL. 

## <a name="create-an-ssl-certificate-with-the-root-ca"></a>Crear un certificado SSL con la entidad de certificación raíz

Certificados SSL tienen dos funciones críticas: protección de la conexión a través de cifrado y comprobación de que realmente se está comunicando con la dirección que se muestra en la barra del explorador (Bing.com, 192.168.1.37, etc.) y no un tercero malintencionado.

El siguiente script de PowerShell crea un certificado SSL para el `localhost` extremo. Cada extremo que escucha el Portal de dispositivo de necesita su propio certificado; puede reemplazar la `$IssuedTo` argumento en la secuencia de comandos con cada uno de los extremos diferentes de su dispositivo: el nombre de host, localhost y las direcciones IP.

```PowerShell
$IssuedTo = "localhost"
$Password = "PickAPassword"
$OutputPath = "c:\temp\"
$rootCA = Import-Certificate -FilePath C:\temp\WdpTestCA.cer -CertStoreLocation Cert:\CurrentUser\My\

# Create SSL cert signed by certificate authority
$IssuedToClean = $IssuedTo.Replace(":", "-").Replace(" ", "_")
$FilePath = $OutputPath + $IssuedToClean + ".pfx"
$Subject = "CN="+$IssuedTo
$cert = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -Subject $Subject -DnsName $IssuedTo -Signer $rootCA -HashAlgorithm "SHA512"
$certFile = Export-PfxCertificate -cert $cert -FilePath $FilePath -Password (ConvertTo-SecureString -String $Password -Force -AsPlainText)
```

Si tiene varios dispositivos, puede volver a usar los archivos .pfx de localhost, pero aún necesitará crear certificados de dirección y el nombre de host IP para cada dispositivo por separado.

Cuando se genera la agrupación de los archivos .pfx, debe cargarlos en el Portal de dispositivo de Windows. 

## <a name="provision-device-portal-with-the-certifications"></a>Portal de dispositivo de aprovisionamiento con las certificaciones

Para cada archivo de .pfx que ha creado para un dispositivo, debe ejecutar el comando siguiente desde un símbolo del sistema con privilegios elevados.

```
WebManagement.exe -SetCert <Path to .pfx file> <password for pfx> 
```

Vea a continuación uso por ejemplo:
```
WebManagement.exe -SetCert localhost.pfx PickAPassword
WebManagement.exe -SetCert --1.pfx PickAPassword
WebManagement.exe -SetCert MyLivingRoomPC.pfx PickAPassword
```

Una vez haya instalado los certificados, simplemente reinicie el servicio para que los cambios surtan efecto:

```
sc stop webmanagement
sc start webmanagement
```

> [!TIP]
> Pueden cambiar las direcciones IP a través del tiempo.
Muchas redes usar DHCP para proporcionar las direcciones IP, por lo que los dispositivos no obtienen siempre la misma dirección IP que tenían anteriormente. Si se ha creado un certificado para una dirección IP en un dispositivo y ha cambiado la dirección del dispositivo, Portal de dispositivo de Windows generará un nuevo certificado utilizando el certificado autofirmado existente y se detendrá utilizará el que ha creado. Esto hará que la página de advertencia de certificado aparecerá en el Explorador de nuevo. Por este motivo, se recomienda que se conectan a los dispositivos a través de sus nombres de host, lo que se pueden establecer en el Portal de dispositivo. Estos seguirá siendo la misma, independientemente de las direcciones IP.
