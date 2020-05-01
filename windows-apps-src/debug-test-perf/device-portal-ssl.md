---
ms.assetid: e04ebe3f-479c-4b48-99d8-3dd4bb9bfaf4
title: Aprovisionar el Portal de dispositivos con un certificado SSL personalizado
description: Obtén información sobre cómo aprovisionar el Portal de dispositivos Windows con un certificado personalizado para usarlo en la comunicación HTTPS.
ms.date: 04/19/2019
ms.topic: article
keywords: windows 10, uwp, portal de dispositivos
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: ce4e45bc23f4efb636618bb4891b9d6e9d207490
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "63774142"
---
# <a name="provision-device-portal-with-a-custom-ssl-certificate"></a>Aprovisionar el Portal de dispositivos con un certificado SSL personalizado

En la actualización Creators Update de Windows 10, el Portal de dispositivos Windows agregó una manera de que los administradores de dispositivos pudieran instalar un certificado personalizado para usarlo en la comunicación HTTPS.

Si bien puedes hacerlo en tu propio equipo, esta función está pensada principalmente para las empresas que cuentan con una infraestructura de certificados en funcionamiento.  

Por ejemplo, una empresa podría tener una entidad de certificación (CA) que use para firmar certificados para sitios web de intranet atendidos a través de HTTPS. Esta función se coloca por encima de dicha infraestructura.

## <a name="overview"></a>Introducción

De manera predeterminada, el Portal de dispositivos genera una CA de raíz autofirmada y, a continuación, la utiliza para firmar certificados SSL para cada punto de conexión en el que esté escuchando. Esto incluye `localhost`, `127.0.0.1` y `::1` (host local IPv6).

También se incluyen el nombre de host del dispositivo (por ejemplo, `https://LivingRoomPC`) y cada dirección IP con vínculo local asignada al dispositivo (hasta dos [IPv4, IPv6] por cada adaptador de red).
Puedes ver las direcciones IP locales de vínculo local para un dispositivo echando un vistazo a la herramienta de redes en el Portal de dispositivos. Comenzarán por `10.` o `192.` para IPv4, o `fe80:` para IPv6.

En la configuración predeterminada, puede aparecer una advertencia de certificado en el navegador, porque la CA raíz no es de confianza. Específicamente, el certificado SSL proporcionado por el Portal de dispositivos está firmado por una CA raíz en la que no confía el navegador o el equipo. Esto se puede solucionar mediante la creación de una nueva CA raíz de confianza.

## <a name="create-a-root-ca"></a>Crear una CA raíz

Esto solamente debe hacerse si tu empresa (u hogar) no dispone de una infraestructura de certificados establecida, y solo debe hacerse una vez. El siguiente script de PowerShell crea una CA raíz llamada _WdpTestCA.cer_. La instalación de este archivo en las entidades de certificación de raíz de confianza del equipo local hará que el dispositivo confíe en los certificados SSL que estén firmados por esta CA raíz. Puedes (y debes) instalar este archivo .cer en todos los equipos que quieras conectar al Portal de dispositivos Windows.  

```PowerShell
$CN = "PickAName"
$OutputPath = "c:\temp\"

# Create root certificate authority
$FilePath = $OutputPath + "WdpTestCA.cer"
$Subject =  "CN="+$CN
$rootCA = New-SelfSignedCertificate -certstorelocation cert:\currentuser\my -Subject $Subject -HashAlgorithm "SHA512" -KeyUsage CertSign,CRLSign
$rootCAFile = Export-Certificate -Cert $rootCA -FilePath $FilePath
```

Una vez creado, puedes usar el archivo _WdpTestCA.cer_ para firmar certificados SSL.

## <a name="create-an-ssl-certificate-with-the-root-ca"></a>Crear un certificado SSL con la CA raíz

Los certificados SSL tienen dos funciones fundamentales: proteger la conexión mediante cifrado y comprobar que realmente te comunicas con la dirección que se muestra en la barra del navegador (Bing.com, 192.168.1.37, etc.) y no un tercero malintencionado.

El siguiente script de PowerShell crea un certificado SSL para el punto de conexión `localhost`. Cada punto de conexión que el Portal de dispositivos escucha necesita su propio certificado; puedes reemplazar el argumento `$IssuedTo` en el script por cada uno de los puntos de conexión diferentes del dispositivo: el nombre de host, el host local y las direcciones IP.

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

Si tienes varios dispositivos, puedes volver a usar los archivos .pfx de host local, pero tendrás que crear direcciones IP y certificados de nombre de host para cada dispositivo por separado.

Cuando se genere el lote de archivos .pfx, deberás cargarlos en el Portal de dispositivos Windows.

## <a name="provision-device-portal-with-the-certifications"></a>Aprovisionar el Portal de dispositivos con las certificaciones

Para cada archivo .pfx que hayas creado para un dispositivo, debes ejecutar el siguiente comando desde un símbolo del sistema con privilegios elevados.

```cmd
WebManagement.exe -SetCert <Path to .pfx file> <password for pfx>
```

El siguiente es un ejemplo de uso:

```cmd
WebManagement.exe -SetCert localhost.pfx PickAPassword
WebManagement.exe -SetCert --1.pfx PickAPassword
WebManagement.exe -SetCert MyLivingRoomPC.pfx PickAPassword
```

Una vez que hayas instalado los certificados, simplemente reinicia el servicio para que los cambios surtan efecto:

```cmd
sc stop webmanagement
sc start webmanagement
```

> [!TIP]
> Las direcciones IP pueden cambiar con el tiempo.
Muchas redes usan DHCP para proporcionar las direcciones IP, para que los dispositivos no obtengan siempre la misma dirección IP que tenían anteriormente. Si has creado un certificado de una dirección IP en un dispositivo y ha cambiado la dirección del dispositivo, el Portal de dispositivos Windows generará un nuevo certificado usando el certificado autofirmado existente y dejará de usar el que hubieras creado. Esto hará que la página de advertencia de certificados vuelva a aparecer en el navegador. Por este motivo, te recomendamos conectarte a los dispositivos con sus nombres de host, que se pueden establecer en el Portal de dispositivos. Estos seguirán siendo los mismos, independientemente de las direcciones IP.
