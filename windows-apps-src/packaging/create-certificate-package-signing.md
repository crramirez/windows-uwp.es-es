---
title: Crear un certificado para firmar paquetes
description: Crea y exporta un certificado para firmar paquetes de la aplicación con herramientas de PowerShell.
ms.date: 09/30/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 7bc2006f-fc5a-4ff6-b573-60933882caf8
ms.localizationpriority: medium
ms.openlocfilehash: 1b9a538dc36818c065e790170f693576650f5024
ms.sourcegitcommit: 34671182c26f5d0825c216a6cededc02b0059a9e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67286928"
---
# <a name="create-a-certificate-for-package-signing"></a>Crear un certificado para firmar paquetes


En este artículo se explica cómo crear y exportar un certificado para firmar paquetes de la aplicación con herramientas de PowerShell. Se recomienda usar Visual Studio para el [empaquetado de aplicaciones para UWP](https://docs.microsoft.com/windows/uwp/packaging/packaging-uwp-apps), pero aún puedes empaquetar una aplicación de la Tienda manualmente si no usas Visual Studio para desarrollar la aplicación.

> [!IMPORTANT] 
> Si usaste Visual Studio para desarrollar tu aplicación, es recomendable que uses al Asistente de Visual Studio para importar un certificado y firmar el paquete de la aplicación. Para más información, consulta [Empaquetado de aplicaciones para UWP con Visual Studio](https://docs.microsoft.com/windows/uwp/packaging/packaging-uwp-apps).

## <a name="prerequisites"></a>Requisitos previos

- **Una aplicación empaquetada o sin empaquetar**  
Una aplicación que contiene un archivo AppxManifest.xml. Debes hacer referencia al archivo de manifiesto al crear el certificado que se usará para firmar el paquete final de la aplicación. Para obtener más información acerca de cómo empaquetar una aplicación manualmente, consulta [Crear un paquete de la aplicación con la herramienta MakeAppx.exe](https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool).

- **Cmdlets de la infraestructura de clave pública (PKI)**  
Necesitas cmdlets de PKI para crear y exportar el certificado de firma. Para obtener más información, consulta [Cmdlets de la Infraestructura de clave pública](https://docs.microsoft.com/powershell/module/pkiclient/).

## <a name="create-a-self-signed-certificate"></a>Crear un certificado autofirmado

Un certificado autofirmado es útil para probar la aplicación cuando esté listo para publicarlo en el Store. Siga los pasos descritos en esta sección para crear un certificado autofirmado.

> [!NOTE]
> Los certificados autofirmados son estrictamente para pruebas. Cuando esté listo para publicar la aplicación en la tienda o desde otros lugares, cambiar el certificado a la fuente de confianza. Si no lo hace posible la incapacidad de la aplicación se instalan los clientes.

### <a name="determine-the-subject-of-your-packaged-app"></a>Determinar al asunto de la aplicación empaquetada  

Para usar un certificado para firmar el paquete de la aplicación, "Subject" en el certificado **debe** coincidir con la sección "Publisher" en el manifiesto de la aplicación.

Por ejemplo, la sección "Identity" en el archivo AppxManifest.xml de tu aplicación debería ser similar al siguiente:

```xml
  <Identity Name="Contoso.AssetTracker" 
    Version="1.0.0.0" 
    Publisher="CN=Contoso Software, O=Contoso Corporation, C=US"/>
```

En este caso, "Publisher" es "CN = Contoso Software, O = Contoso Corporation, C = US", que debe usarse para crear el certificado.

### <a name="use-new-selfsignedcertificate-to-create-a-certificate"></a>Usa **New-SelfSignedCertificate** para crear un certificado

Usa el cmdlet de PowerShell **New-SelfSignedCertificate** para crear un certificado autofirmado. **New-SelfSignedCertificate** tiene varios parámetros que deben personalizarse, pero, a los efectos de este artículo, nos centraremos en cómo crear un certificado sencillo que funcione con **SignTool**. Para obtener más ejemplos y usos de este cmdlet, consulta [New-SelfSignedCertificate](https://docs.microsoft.com/powershell/module/pkiclient/New-SelfSignedCertificate).

En función del archivo AppxManifest.xml del ejemplo anterior, debes usar la siguiente sintaxis para crear un certificado. En un símbolo del sistema de PowerShell con privilegios elevados:

```powershell
New-SelfSignedCertificate -Type Custom -Subject "CN=Contoso Software, O=Contoso Corporation, C=US" -KeyUsage DigitalSignature -FriendlyName "Your friendly name goes here" -CertStoreLocation "Cert:\CurrentUser\My" -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.3", "2.5.29.19={text}")
```

Tenga en cuenta los detalles acerca de algunos de los parámetros siguientes:

- **KeyUsage**: Este parámetro define lo que puede utilizarse el certificado. Para obtener un certificado autofirmado, este parámetro debe establecerse en **DigitalSignature**.

- **TextExtension**: Este parámetro incluye la configuración para las siguientes extensiones:

  - Uso mejorado de clave (EKU): Esta extensión indica otros propósitos para los que se puede usar la clave pública certificada. Para obtener un certificado autofirmado, este parámetro debe incluir la cadena de extensión **"2.5.29.37={text}1.3.6.1.5.5.7.3.3"** , que indica que el certificado que se usará para la firma de código.

  - Restricciones básicas: Esta extensión indica si el certificado es una entidad de certificación (CA). Para obtener un certificado autofirmado, este parámetro debe incluir la cadena de extensión **"2.5.29.19={text}"** , lo que indica que el certificado es una entidad final (no una entidad de certificación).

Después de ejecutar este comando, el certificado se agregará al almacén local de certificados, como se especifica en el parámetro "-CertStoreLocation". El resultado del comando también producirá la huella digital del certificado.  

Puedes ver el certificado en una ventana de PowerShell mediante los siguientes comandos:

```powershell
Set-Location Cert:\CurrentUser\My
Get-ChildItem | Format-Table Subject, FriendlyName, Thumbprint
```

Esta acción mostrará todos los certificados en el almacén local.

## <a name="export-a-certificate"></a>Exportar un certificado 

Para exportar el certificado en el almacén local a un archivo de intercambio de información personal (PFX), usa el cmdlet **Export-PfxCertificate**.

Al usar **Export-PfxCertificate**, debes crear y usar una contraseña o usar el parámetro "-ProtectTo" para especificar qué usuarios o grupos pueden acceder al archivo sin una contraseña. Ten en cuenta que se mostrará un error si no usas el parámetro "-Password" o "-ProtectTo".

### <a name="password-usage"></a>Uso de contraseñas

```powershell
$pwd = ConvertTo-SecureString -String <Your Password> -Force -AsPlainText 
Export-PfxCertificate -cert "Cert:\CurrentUser\My\<Certificate Thumbprint>" -FilePath <FilePath>.pfx -Password $pwd
```

### <a name="protectto-usage"></a>Uso de ProtectTo

```powershell
Export-PfxCertificate -cert Cert:\CurrentUser\My\<Certificate Thumbprint> -FilePath <FilePath>.pfx -ProtectTo <Username or group name>
```

Después de crear y exportar el certificado, estás listo para firmar el paquete de la aplicación con **SignTool**. Para el siguiente paso en el proceso de empaquetado manual, consulta [Firmar un paquete de la aplicación con SignTool](https://docs.microsoft.com/windows/uwp/packaging/sign-app-package-using-signtool).

## <a name="security-considerations"></a>Consideraciones de seguridad

Al agregar un certificado a los [almacenes de certificados del equipo local](https://docs.microsoft.com/windows-hardware/drivers/install/local-machine-and-current-user-certificate-stores), afectas a la confianza de los certificados de todos los usuarios en el equipo. Se recomienda quitar dichos certificados cuando ya no sean necesarios, para impedir que se usen para poner en peligro la confianza del sistema.
