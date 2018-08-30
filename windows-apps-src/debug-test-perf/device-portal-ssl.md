---
author: PatrickFarley
ms.assetid: e04ebe3f-479c-4b48-99d8-3dd4bb9bfaf4
title: Aprovisionar el Portal de dispositivos con un certificado SSL personalizado
description: TBD
ms.author: pafarley
ms.date: 07/11/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, portal de dispositivos
ms.localizationpriority: medium
ms.openlocfilehash: 1192c200cd42ab28cc7e763c06fd8a5638aa3400
ms.sourcegitcommit: 7efffcc715a4be26f0cf7f7e249653d8c356319b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/30/2018
ms.locfileid: "3121010"
---
# <a name="provision-device-portal-with-a-custom-ssl-certificate"></a>Aprovisionar el Portal de dispositivos con un certificado SSL personalizado
En Windows 10 Creators Update, Windows Device Portal agregado una forma para que los administradores del dispositivo instalar un certificado personalizado para su uso en comunicaciones HTTPS. 

Si bien puedes hacerlo en su PC, esta característica está pensada principalmente para empresas que tienen una infraestructura de certificado en su lugar.  

Por ejemplo, una compañía podría tener una entidad de certificación (CA) que usa para firmar los certificados para sitios Web de intranet proporcionados a través de HTTPS. Esta característica además de eso significa infraestructura. 

## <a name="overview"></a>Introducción
De manera predeterminada, Device Portal genera una CA raíz autofirma y, a continuación, que usará para firmar los certificados SSL para cada punto de conexión está escuchando. Esto incluye `localhost`, `127.0.0.1`, y `::1` (IPv6 localhost).

También se incluyen son el nombre de host del dispositivo (por ejemplo, `https://LivingRoomPC`) y cada dirección IP local de vínculo asignada al dispositivo (hasta dos [IPv4, IPv6] para cada adaptador de red). Puedes ver las direcciones IP locales del vínculo para un dispositivo, observemos la herramienta de redes de Device Portal. Empezaremos con `10.` o `192.` IPv4, o `fe80:` para IPv6. 

En la configuración predeterminada, puede aparecer una advertencia de certificado en el explorador debido a la entidad de certificación raíz de confianza. Específicamente, el certificado SSL proporcionado por Device Portal está firmado por una CA raíz que no confía en el explorador o el equipo. Esto puede corregirse mediante la creación de una CA raíz de confianza nueva.

## <a name="create-a-root-ca"></a>Crear una entidad de certificación raíz

Esto solo debe hacerse si tu empresa (o home) no tiene una infraestructura de certificados configurar y solo debe realizarse una vez. El siguiente script de PowerShell crea una CA denominada _WdpTestCA.cer_raíz. Instalación de este archivo en las entidades de certificación de raíz de confianza de la máquina local hará que el dispositivo para los certificados SSL que están firmados por esta entidad de certificación raíz de confianza. Puede (y debe) instalar este archivo .cer en cada equipo que quieres conectarte al Portal de dispositivos de Windows.  

```PowerShell
$CN = "PickAName"
$OutputPath = "c:\temp\"

# Create root certificate authority
$FilePath = $OutputPath + "WdpTestCA.cer"
$Subject =  "CN="+$CN
$rootCA = New-SelfSignedCertificate -certstorelocation cert:\currentuser\my -Subject $Subject -HashAlgorithm "SHA512" -KeyUsage CertSign,CRLSign
$rootCAFile = Export-Certificate -Cert $rootCA -FilePath $FilePath
```

Una vez creado, puedes usar el archivo _WdpTestCA.cer_ para firmar los certificados SSL. 

## <a name="create-an-ssl-certificate-with-the-root-ca"></a>Crear un certificado SSL con la entidad de certificación raíz

Los certificados SSL tienen dos funciones fundamentales: cómo proteger la conexión a través de cifrado y comprobar que realmente se está comunicando con la dirección mostrada en la barra de explorador (Bing.com, 192.168.1.37, etc.) y no un tercero malintencionado.

El siguiente script de PowerShell crea un certificado SSL para el `localhost` punto de conexión. Cada punto de conexión que escuchan Device Portal tiene su propio certificado; puedes reemplazar el `$IssuedTo` argumento en el script con cada uno de los puntos de conexión diferentes para el dispositivo: el nombre de host, localhost y la direcciones IP.

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

Si tienes varios dispositivos, puedes volver a los archivos .pfx de localhost, pero aún tendrás que crear certificados de dirección y el nombre de host IP para cada dispositivo por separado.

Cuando se genere la recopilación de los archivos .pfx, tienes que cargarlos en Windows Device Portal. 

## <a name="provision-device-portal-with-the-certifications"></a>Aprovisionar Device Portal con las certificaciones

Para cada archivo de .pfx que has creado para un dispositivo, debes ejecutar el comando siguiente desde un símbolo del sistema con privilegios elevados.

```
WebManagement.exe -SetCert <Path to .pfx file> <password for pfx> 
```

Consulta a continuación de uso por ejemplo:
```
WebManagement.exe -SetCert localhost.pfx PickAPassword
WebManagement.exe -SetCert --1.pfx PickAPassword
WebManagement.exe -SetCert MyLivingRoomPC.pfx PickAPassword
```

Una vez que haya instalado los certificados, puedes reiniciar el servicio para que los cambios surtan efecto:

```
sc stop webmanagement
sc start webmanagement
```

> [!TIP]
> Las direcciones IP pueden cambiar con el tiempo.
Muchas redes usan DHCP para dar un vistazo a las direcciones IP, para que los dispositivos no tener siempre la misma dirección IP que tenían anteriormente. Si has creado un certificado para una dirección IP en un dispositivo y que ha cambiado la dirección del dispositivo, Windows Device Portal se generará un nuevo certificado utilizando el certificado autofirmado existente y dejará de uso creó. Esto hará que la página de advertencia de certificado volver a aparecer en el explorador. Por este motivo, te recomendamos conectar a los dispositivos a través de sus nombres de host, que se pueden establecer en el Portal de dispositivos. Estos seguirá siendo la misma independientemente de las direcciones IP.
