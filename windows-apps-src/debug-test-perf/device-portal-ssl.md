---
author: PatrickFarley
ms.assetid: e04ebe3f-479c-4b48-99d8-3dd4bb9bfaf4
title: Portal de dispositivo de provisión con un certificado SSL personalizado
description: TBD
ms.author: pafarley
ms.date: 07/11/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, portal de dispositivo
ms.localizationpriority: medium
ms.openlocfilehash: 1192c200cd42ab28cc7e763c06fd8a5638aa3400
ms.sourcegitcommit: 3727445c1d6374401b867c78e4ff8b07d92b7adc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2018
ms.locfileid: "2916762"
---
# <a name="provision-device-portal-with-a-custom-ssl-certificate"></a>Portal de dispositivo de provisión con un certificado SSL personalizado
En la actualización de los creadores de Windows 10, Portal de dispositivo de Windows agrega los administradores de dispositivos instalar un certificado personalizado para su uso en comunicaciones HTTPS. 

Aunque puede hacerlo en su PC, esta característica está pensada para las empresas que tienen una infraestructura de certificados existente en su lugar.  

Por ejemplo, una compañía podría tener una entidad emisora de certificados (CA) que utiliza para firmar los certificados para los sitios Web de intranet que sirve a través de HTTPS. Esta característica de eso significa infraestructura. 

## <a name="overview"></a>Introducción
De forma predeterminada, Portal de dispositivo genera una entidad emisora de certificados raíz y utilizará para firmar los certificados SSL para cada extremo está escuchando. Esto incluye `localhost`, `127.0.0.1`, y `::1` (localhost IPv6).

También se incluyen los nombre de host del dispositivo (por ejemplo, `https://LivingRoomPC`) y cada dirección IP de enlace local asignada al dispositivo (hasta dos [IPv4, IPv6] para cada adaptador de red). Puede ver las direcciones IP de enlace local para un dispositivo mirando en la herramienta red en el Portal de dispositivo. Que se iniciarán con `10.` o `192.` para IPv4, o `fe80:` para IPv6. 

En la configuración predeterminada, puede aparecer una advertencia de certificado en su explorador debido a la entidad emisora raíz de confianza. En concreto, el certificado SSL proporcionado por el Portal de dispositivo está firmado por una CA raíz que no confía en el explorador o el PC. Esto se puede solucionar mediante la creación de una entidad emisora raíz de confianza nueva.

## <a name="create-a-root-ca"></a>Crear una entidad emisora de certificados raíz

Esto sólo debe hacerse si la empresa (o principal) no tiene una infraestructura de certificados configurar y sólo debe realizarse una vez. El siguiente script de PowerShell crea una raíz de entidad emisora de certificados denominada _WdpTestCA.cer_. Instalar este archivo a las entidades de certificación de raíz de confianza del equipo local hará que el dispositivo debe confiar en los certificados SSL están firmados por esta CA raíz. Puede (y debe) instalar este archivo .cer en cada equipo al que desea conectarse al Portal de dispositivo de Windows.  

```PowerShell
$CN = "PickAName"
$OutputPath = "c:\temp\"

# Create root certificate authority
$FilePath = $OutputPath + "WdpTestCA.cer"
$Subject =  "CN="+$CN
$rootCA = New-SelfSignedCertificate -certstorelocation cert:\currentuser\my -Subject $Subject -HashAlgorithm "SHA512" -KeyUsage CertSign,CRLSign
$rootCAFile = Export-Certificate -Cert $rootCA -FilePath $FilePath
```

Una vez creado, puede utilizar el archivo _WdpTestCA.cer_ para firmar certificados SSL. 

## <a name="create-an-ssl-certificate-with-the-root-ca"></a>Crear un certificado SSL con la entidad emisora raíz

Certificados SSL tienen dos funciones importantes: proteger su conexión a través de cifrado y comprobación de que realmente se está comunicando con la dirección mostrada en la barra del explorador (Bing.com, 192.168.1.37, etc.) y no un tercero con malas intenciones.

El siguiente script de PowerShell crea un certificado SSL para el `localhost` extremo. Cada extremo que escucha Portal de dispositivo necesita su propio certificado; puede reemplazar el `$IssuedTo` argumento en la secuencia de comandos con cada uno de los extremos diferentes de su dispositivo: el nombre de host, localhost y la direcciones IP.

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

Si tiene varios dispositivos, puede reutilizar los archivos .pfx localhost, pero todavía necesitará crear certificados de dirección y nombre de host IP para cada dispositivo por separado.

Cuando se genera el paquete de archivos .pfx, debe cargarlos en el Portal de dispositivo de Windows. 

## <a name="provision-device-portal-with-the-certifications"></a>Portal de dispositivo de provisión con las certificaciones

Para cada archivo de .pfx que ha creado para un dispositivo, debe ejecutar el siguiente comando desde un símbolo del sistema con privilegios elevados.

```
WebManagement.exe -SetCert <Path to .pfx file> <password for pfx> 
```

Vea a continuación el uso por ejemplo:
```
WebManagement.exe -SetCert localhost.pfx PickAPassword
WebManagement.exe -SetCert --1.pfx PickAPassword
WebManagement.exe -SetCert MyLivingRoomPC.pfx PickAPassword
```

Una vez haya instalado los certificados, simplemente reinicie el servicio para que surtan efecto los cambios:

```
sc stop webmanagement
sc start webmanagement
```

> [!TIP]
> Las direcciones IP pueden cambiar con el tiempo.
Muchas redes utilizan DHCP para asignar las direcciones IP, para que dispositivos no se siempre la misma dirección IP que tenían anteriormente. Si ha creado un certificado para una dirección IP en un dispositivo y que ha cambiado la dirección del dispositivo, Portal de dispositivo de Windows generará un nuevo certificado utilizando el certificado autofirmado existente y se detienen utilizará el que se creó. Esto hará que la página de advertencia de certificado aparecerá en el Explorador de nuevo. Por este motivo, se recomienda conectarse a los dispositivos a través de sus nombres de host, que se pueden establecer en el Portal de dispositivo. Estos serán siendo la misma independientemente de direcciones IP.
