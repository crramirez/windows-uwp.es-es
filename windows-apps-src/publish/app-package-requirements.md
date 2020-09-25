---
Description: Siga estas instrucciones para preparar los paquetes de la aplicación para su envío a la Microsoft Store.
title: Requisitos del paquete de la aplicación
ms.assetid: 651B82BA-9D0C-45AC-8997-88CD93DC903C
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP, requisitos de paquete, paquetes, formato de paquete, versión admitida, enviar
ms.localizationpriority: medium
ms.openlocfilehash: 848adbab20765a65ef4673219c55dfbf076e47e0
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2020
ms.locfileid: "91219778"
---
# <a name="app-package-requirements"></a>Requisitos del paquete de la aplicación

Siga estas instrucciones para preparar los paquetes de la aplicación para su envío a la Microsoft Store.

## <a name="before-you-build-your-apps-package-for-the-microsoft-store"></a>Antes de compilar el paquete de la aplicación para el Microsoft Store

Asegúrate de [probar tu aplicación con el Kit para la certificación de aplicaciones en Windows](../debug-test-perf/windows-app-certification-kit.md). También te recomendamos que pruebes la aplicación en distintos tipos de hardware. Tenga en cuenta que hasta que certificamos la aplicación y la pone a disposición del Microsoft Store, solo se puede instalar y ejecutar en equipos que tengan licencias de desarrollador.

## <a name="building-the-app-package-using-microsoft-visual-studio"></a>Compilar el paquete de la aplicación con Microsoft Visual Studio

Si usas Microsoft Visual Studio como entorno de desarrollo, ya cuentas con herramientas integradas que te ayudarán a crear un paquete de la aplicación de manera rápida y sencilla. Para más información, consulta [Empaquetado de aplicaciones](../packaging/index.md).

> [!NOTE]
> Asegúrese de que todos los nombres de archivo usan ANSI. 

Cuando crees el paquete en Visual Studio, asegúrate de iniciar sesión con la misma cuenta asociada con tu cuenta de desarrollador. Algunas partes del manifiesto del paquete tienen detalles específicos relacionados con tu cuenta. Esta información se detecta y se agrega automáticamente. Sin la información adicional agregada al manifiesto, pueden producirse errores de carga de paquetes. 

Al compilar los paquetes de UWP de la aplicación, Visual Studio puede crear un archivo. msix o appx, o un archivo. msixupload o. appxupload. En el caso de las aplicaciones UWP, se recomienda cargar siempre el archivo. msixupload o. appxupload en la página [paquetes](upload-app-packages.md) . Para obtener más información sobre cómo empaquetar aplicaciones para UWP para la tienda, consulta [empaquetar una aplicación para UWP con Visual Studio](/windows/msix/package/packaging-uwp-apps).

No es necesario que los paquetes de la aplicación estén firmados con un certificado con la raíz en una entidad de certificación de confianza.


### <a name="app-bundles"></a>Paquetes de aplicaciones

En el caso de las aplicaciones UWP, Visual Studio puede generar un lote de aplicaciones (. msixbundle o. appxbundle) para reducir el tamaño de la aplicación que descargan los usuarios. Esto puede ser útil si definiste recursos específicos por idioma, una variedad de recursos de escala de imagen o recursos que se apliquen a versiones específicas de Microsoft DirectX.

> [!NOTE]
> Un grupo de aplicaciones puede contener los paquetes para todas las arquitecturas.

Con un lote de la aplicación, un usuario solo descargará los archivos relevantes, en vez de todos los recursos posibles. Para obtener más información sobre los grupos de aplicaciones, consulte [empaquetar aplicaciones](../packaging/index.md) y [empaquetar una aplicación para UWP con Visual Studio](/windows/msix/package/packaging-uwp-apps).


## <a name="building-the-app-package-manually"></a>Compilar el paquete de la aplicación manualmente

Si no usas Visual Studio para crear tu paquete, debes [crear el manifiesto de tu paquete manualmente](/uwp/schemas/appxpackage/how-to-create-a-package-manifest-manually).

Asegúrate de revisar la documentación del [manifiesto del paquete de la aplicación](/uwp/schemas/appxpackage/appx-package-manifest) para conocer los requisitos y detalles completos del manifiesto. El manifiesto debe seguir el esquema de manifiesto de paquetes a fin de aprobar la certificación.

El manifiesto debe incluir información específica sobre tu cuenta y tu aplicación. Puedes encontrar esta información en [Ver detalles de identidad de la aplicación](view-app-identity-details.md) en la sección **Administración de aplicaciones** de la página de introducción a la aplicación en el panel.

> [!NOTE]
> Los valores del manifiesto distinguen mayúsculas de minúsculas. También deben coincidir los espacios y otras puntuaciones. Escribe los valores cuidadosamente y revísalos para asegurarte de que son correctos.


Los paquetes de aplicaciones (. msixbundle o. appxbundle) usan un manifiesto diferente. Revisa la documentación del [manifiesto de lotes](/uwp/schemas/bundlemanifestschema/bundle-manifest) para conocer los requisitos y detalles para los manifiestos de lotes de aplicaciones. Tenga en cuenta que en un. msixbundle o. appxbundle, el manifiesto de cada paquete incluido debe utilizar los mismos elementos y atributos, excepto el atributo **processorArchitecture** del elemento [Identity](/uwp/schemas/appxpackage/uapmanifestschema/element-identity) .

> [!TIP]
> Asegúrese de ejecutar el [Kit de certificación de aplicaciones de Windows](../debug-test-perf/windows-app-certification-kit.md) antes de enviar los paquetes. Esto puede ayudarte a determinar si tu manifiesto tiene algún problema que pueda causar errores de certificación o envío.


## <a name="package-format-requirements"></a>Requisitos de formato del paquete

Los paquetes de la aplicación deben cumplir con estos requisitos.

| Propiedad del paquete de la aplicación | Requisito                                                          |
|----------------------|----------------------------------------------------------------------|
| Tamaño de paquete         | . msixbundle o. appxbundle: 25 GB como máximo por lote <br>paquetes. msix o. appx destinados a Windows 10:25 GB como máximo por paquete<br>Paquetes .appx destinados a Windows 8.1: 8 GB como máximo por paquete <br> Paquetes .appx destinados a Windows 8: 2 GB como máximo por paquete <br> Paquetes .appx destinados a Windows Phone 8.1: 4 GB como máximo por paquete <br> Paquetes .xap: 1 GB como máximo por paquete                                                                           |
| Hash de asignación de bloque     | Algoritmo SHA2-256                                                   |

> [!IMPORTANT]
> Ya no se pueden cargar nuevos paquetes XAP compilados con los SDK de Windows Phone 8. x. Las aplicaciones que ya se encuentran en el almacén con paquetes XAP seguirán funcionando en dispositivos Windows 10 Mobile. Para obtener más información, consulte esta [entrada de blog](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).

## <a name="supported-versions"></a>Versiones compatibles

En el caso de las aplicaciones UWP, todos los paquetes deben tener como destino una versión de Windows 10 compatible con el almacén. Las versiones que admite el paquete deben indicarse en los atributos **MinVersion** y **MaxVersionTested** del elemento [TargetDeviceFamily](/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily) del manifiesto de la aplicación.

Las versiones admitidas actualmente son de: 
- Mínimo: 10.0.10240.0
- Máximo: 10.0.17763.1


## <a name="storemanifest-xml-file"></a>Archivo XML de StoreManifest

StoreManifest.xml es un archivo de configuración opcional que se puede incluir en paquetes de la aplicación. Su finalidad es habilitar características, como la declaración de la aplicación como Microsoft Store aplicación de dispositivo o la declaración de requisitos que un paquete depende de que se aplique a un dispositivo, que el manifiesto del paquete no cubre. Si se usa, StoreManifest.xml se envía con el paquete de la aplicación y debe estar en la carpeta raíz del proyecto principal de la aplicación. Para obtener más información, consulta [Esquema StoreManifest](/uwp/schemas/storemanifest/store-manifest-schema-portal).

 

 
