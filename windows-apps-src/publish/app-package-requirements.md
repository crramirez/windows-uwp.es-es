---
author: jnHs
Description: Follow these guidelines to prepare your app's packages for submission to the Microsoft Store.
title: Requisitos del paquete de la aplicación
ms.assetid: 651B82BA-9D0C-45AC-8997-88CD93DC903C
ms.author: wdg-dev-content
ms.date: 05/16/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, requisitos del paquete, paquetes, formato del paquete, versión compatible, enviar
ms.localizationpriority: medium
ms.openlocfilehash: d7d748f36dafd93066928f01f9aa42414f2ffc1f
ms.sourcegitcommit: c6d6f8b54253e79354f8db14e5cf3b113a3e5014
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "2839915"
---
# <a name="app-package-requirements"></a>Requisitos del paquete de la aplicación

Sigue estas instrucciones para preparar los paquetes de la aplicación para enviarlos a Microsoft Store.

## <a name="before-you-build-your-apps-package-for-the-microsoft-store"></a>Antes de compilar el paquete de la aplicación para Microsoft Store

Asegúrate de [probar tu aplicación con el Kit para la certificación de aplicaciones en Windows](../debug-test-perf/windows-app-certification-kit.md). También te recomendamos que pruebes la aplicación en distintos tipos de hardware. Ten en cuenta que hasta que no certifiquemos la aplicación y hagamos que esté disponible en Microsoft Store, esta solo podrá instalarse y ejecutarse en equipos que tengan licencias de desarrollador.

## <a name="building-the-app-package-using-microsoft-visual-studio"></a>Compilar el paquete de la aplicación con Microsoft Visual Studio

Si usas MicrosoftVisual Studio como entorno de desarrollo, ya cuentas con herramientas integradas que te ayudarán a crear un paquete de la aplicación de manera rápida y sencilla. Para más información, consulta [Empaquetado de aplicaciones](../packaging/index.md).

> [!NOTE]
> Asegúrate de que todos tus nombres de archivo usen ANSI. 

Cuando crees el paquete en Visual Studio, asegúrate de iniciar sesión con la misma cuenta asociada con tu cuenta de desarrollador. Algunas partes del manifiesto del paquete tienen detalles específicos relacionados con tu cuenta. Esta información se detecta y se agrega automáticamente. Sin la información adicional agregada al manifiesto, se pueden producir errores al cargar el paquete. 

Cuando compilas los paquetes de la aplicación, Visual Studio puede crear un archivo .appx o un archivo .appxupload (o un archivo .xap para Windows Phone 8.1 y versiones anteriores). Para aplicaciones destinadas a Windows 10, carga siempre el archivo .appxupload en la página [Paquetes](upload-app-packages.md). Para obtener más información sobre cómo empaquetar aplicaciones para UWP para la Store, consulta [Empaquetar una aplicación para UWP con Visual Studio](../packaging/packaging-uwp-apps.md).

No es necesario que los paquetes de la aplicación estén firmados con un certificado con la raíz en una entidad de certificación de confianza.


### <a name="app-bundles"></a>Paquetes de aplicaciones

Para las aplicaciones que estén destinados a 10 de Windows, Windows 8.1 o Windows Phone 8.1, Visual Studio puede generar un paquete de aplicación (.appxbundle) para reducir el tamaño de la aplicación que los usuarios descargar. Esto puede ser útil si definiste recursos específicos por idioma, una variedad de recursos de escala de imagen o recursos que se apliquen a versiones específicas de Microsoft DirectX.

> [!NOTE]
> Un paquete de aplicaciones puede contener tus paquetes para todas las arquitecturas. Debes enviar solo un paquete por cada SO de destino.

Con un lote de la aplicación, un usuario solo descargará los archivos relevantes, en vez de todos los recursos posibles. Para obtener más información sobre lotes de aplicaciones, consulta [Empaquetado de aplicaciones](../packaging/index.md) y [Empaquetar una aplicación para UWP con Visual Studio](../packaging/packaging-uwp-apps.md).


## <a name="building-the-app-package-manually"></a>Compilar el paquete de la aplicación manualmente

Si no usas Visual Studio para crear tu paquete, debes [crear el manifiesto de tu paquete manualmente](https://docs.microsoft.com/uwp/schemas/appxpackage/how-to-create-a-package-manifest-manually).

Asegúrate de revisar la documentación del [manifiesto del paquete de la aplicación](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest) para conocer los requisitos y detalles completos del manifiesto. El manifiesto debe seguir el esquema de manifiesto de paquetes a fin de aprobar la certificación.

El manifiesto debe incluir información específica sobre tu cuenta y tu aplicación. Puedes encontrar esta información en [Ver detalles de identidad de la aplicación](view-app-identity-details.md) en la sección **Administración de aplicaciones** de la página de introducción a la aplicación en el panel.

> [!NOTE]
> Los valores del manifiesto distinguen mayúsculas de minúsculas. También deben coincidir los espacios y otras puntuaciones. Escribe los valores cuidadosamente y revísalos para asegurarte de que son correctos.


Paquetes de aplicación (.appxbundle) utilizar un manifiesto diferente. Revisa la documentación del [manifiesto de lotes](https://docs.microsoft.com/uwp/schemas/bundlemanifestschema/bundle-manifest) para conocer los requisitos y detalles para los manifiestos de lotes de la aplicación. Tenga en cuenta que en un .appxbundle, el .appxmanifest de cada paquete incluido debe usar los mismos elementos y atributos, salvo el atributo **ProcessorArchitecture** del elemento de [identidad](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) .

> [!TIP]
> Asegúrate de ejecutar el [Kit para la certificación de aplicaciones en Windows](../debug-test-perf/windows-app-certification-kit.md) antes de enviar tus paquetes. Esto puede ayudarte a determinar si tu manifiesto tiene algún problema que pueda causar errores de certificación o envío.


## <a name="package-format-requirements"></a>Requisitos de formato del paquete

Los paquetes de la aplicación deben cumplir con estos requisitos.

| Propiedad del paquete de la aplicación | Requisito                                                          |
|----------------------|----------------------------------------------------------------------|
| Tamaño del paquete         | .appxbundle: 25 GB como máximo por paquete <br>Paquetes .appx destinados a Windows10: 25 GB como máximo por paquete<br>Paquetes .appx destinados a Windows8.1: 8 GB como máximo por paquete <br> Paquetes .appx destinados a Windows8: 2 GB como máximo por paquete <br> Paquetes .appx destinados a WindowsPhone 8.1: 4 GB como máximo por paquete <br> Paquetes .xap: 1 GB como máximo por paquete                                                                           |
| Hash de asignación de bloque     | Algoritmo SHA2-256                                                   |


## <a name="supported-versions"></a>Versiones compatibles

Para aplicaciones para UWP, todos los paquetes deben dirigirse a una versión de Windows 10 admitida por la Store. Las versiones que admite tu paquete deben indicarse en los atributos **MinVersion** y **MaxVersionTested** del elemento [TargetDeviceFamily](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) del manifiesto de la aplicación.

El rango de versiones actualmente admitido de: 
- Mínima: 10.0.10240.0
- Máxima: 10.0.17134.0


## <a name="storemanifest-xml-file"></a>Archivo XML de StoreManifest

StoreManifest.xml es un archivo de configuración opcional que se puede incluir en paquetes de la aplicación. Su objetivo es habilitar características como, por ejemplo, declarar tu aplicación como una aplicación para dispositivo de Microsoft Store o declarar los requisitos de los que depende un paquete para que se pueda aplicar a un dispositivo, que no incluye el manifiesto del paquete. Si se usa, StoreManifest.xml se envía con el paquete de la aplicación y debe estar en la carpeta raíz del proyecto principal de su aplicación. Para obtener más información, consulta [Esquema StoreManifest](https://docs.microsoft.com/uwp/schemas/storemanifest/store-manifest-schema-portal).

 

 




