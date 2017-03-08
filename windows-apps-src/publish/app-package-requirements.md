---
author: jnHs
Description: "Sigue estas instrucciones para preparar los paquetes de la aplicación para enviarlos a la Tienda Windows."
title: "Requisitos del paquete de la aplicación"
ms.assetid: 651B82BA-9D0C-45AC-8997-88CD93DC903C
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 59660de0adb6ff1247ea90f0ace3bcca35f19d1a
ms.lasthandoff: 02/07/2017

---

# <a name="app-package-requirements"></a>Requisitos del paquete de la aplicación

Sigue estas instrucciones para preparar los paquetes de la aplicación para enviarlos a la Tienda Windows.

## <a name="before-you-build-your-apps-package-for-the-windows-store"></a>Antes de compilar el paquete de la aplicación para la Tienda Windows

Asegúrate de [probar tu aplicación con el Kit para la certificación de aplicaciones en Windows](https://msdn.microsoft.com/library/windows/apps/mt186449). También te recomendamos que pruebes la aplicación en distintos tipos de hardware. Ten en cuenta que hasta que no certifiquemos la aplicación y hagamos que esté disponible en la Tienda Windows, esta solo podrá instalarse y ejecutarse en equipos que tengan licencias de desarrollador.

## <a name="building-the-app-package-using-microsoft-visual-studio"></a>Compilar el paquete de la aplicación con Microsoft Visual Studio

Si usas Microsoft Visual Studio como entorno de desarrollo, ya cuentas con herramientas integradas que te ayudarán a crear un paquete de la aplicación de manera rápida y sencilla. Para más información, consulta [Empaquetado de aplicaciones](https://msdn.microsoft.com/library/windows/apps/mt270969).

> **Nota** Asegúrate de que todos los nombres de archivo usan ANSI. 


Cuando crees el paquete en Visual Studio, asegúrate de iniciar sesión con la misma cuenta asociada con tu cuenta de desarrollador. Algunas partes del manifiesto del paquete tienen detalles específicos relacionados con tu cuenta. Esta información se detecta y se agrega automáticamente.

Cuando compilas los paquetes de la aplicación, Visual Studio puede crear un archivo .appx o un archivo .appxupload (o un archivo .xap para Windows Phone 8.1 y versiones anteriores). Para aplicaciones destinadas a Windows 10, carga siempre el archivo .appxupload en la página [Paquetes](upload-app-packages.md). Para más información sobre cómo empaquetar aplicaciones para UWP para la Tienda, consulta [Empaquetar aplicaciones universales de Windows para Windows 10](http://go.microsoft.com/fwlink/p/?LinkId=620193 ).

No es necesario que los paquetes de la aplicación estén firmados con un certificado con la raíz en una entidad de certificación de confianza.

### <a name="app-bundles"></a>Paquetes de aplicaciones

En el caso de aplicaciones destinadas a Windows 8.1, Windows Phone 8.1 y versiones posteriores, Visual Studio puede generar un lote de aplicaciones (.appxbundle) para reducir el tamaño de la aplicación que descargarán los usuarios. Esto puede ser útil si definiste recursos específicos por idioma, una variedad de recursos de escala de imagen o recursos que se apliquen a versiones específicas de Microsoft DirectX.

> **Nota** Un único lote de aplicaciones puede contener los paquetes para todas las arquitecturas. Debes enviar solo un paquete por cada SO de destino.


Con un lote de la aplicación, un usuario solo descargará los archivos relevantes, en vez de todos los recursos posibles. Para más información sobre lotes de aplicaciones, consulta el tema [Empaquetado de aplicaciones](https://msdn.microsoft.com/library/windows/apps/mt270969) y [Empaquetar aplicaciones universales de Windows para Windows 10](http://go.microsoft.com/fwlink/p/?LinkId=620193 ).

## <a name="building-the-app-package-manually"></a>Compilar el paquete de la aplicación manualmente

Si no usas Visual Studio para crear tu paquete, debes [crear el manifiesto de tu paquete manualmente](https://msdn.microsoft.com/library/windows/apps/br211476).

Asegúrate de revisar la documentación del [manifiesto del paquete de la aplicación](https://msdn.microsoft.com/library/windows/apps/br211474) para conocer los requisitos y detalles completos del manifiesto. El manifiesto debe seguir el esquema de manifiesto de paquetes a fin de aprobar la certificación.

El manifiesto debe incluir información específica sobre tu cuenta y tu aplicación. Puedes encontrar esta información en [Ver detalles de identidad de la aplicación](view-app-identity-details.md) en la sección **Administración de aplicaciones** de la página de introducción a la aplicación en el panel.

> **Nota** Los valores del manifiesto distinguen mayúsculas de minúsculas. También deben coincidir los espacios y otras puntuaciones. Escribe los valores cuidadosamente y revísalos para asegurarte de que son correctos.


Los lotes de la aplicación usan un manifiesto diferente. Revisa la documentación del [manifiesto de lotes](https://msdn.microsoft.com/library/windows/apps/dn263089) para conocer los requisitos y detalles para los manifiestos de lotes de aplicaciones.

> **Sugerencia** Antes de enviar la aplicación, asegúrate de ejecutar el [Kit para la certificación de aplicaciones en Windows](https://msdn.microsoft.com/library/windows/apps/mt186449). Esto puede ayudarte a determinar si tu manifiesto tiene algún problema que pueda causar errores de certificación o envío.


Si la aplicación tiene más de un paquete, estos elementos del manifiesto de la aplicación deben ser iguales en cada paquete (por SO de destino):

-   [**Paquete y funcionalidades**](https://msdn.microsoft.com/library/windows/apps/br211422)
-   [**Paquete y dependencias**](https://msdn.microsoft.com/library/windows/apps/br211428)
-   [**Paquete y recursos**](https://msdn.microsoft.com/library/windows/apps/br211462)

## <a name="package-format-requirements"></a>Requisitos de formato del paquete

Los paquetes de la aplicación deben cumplir con estos requisitos.

| Propiedad del paquete de la aplicación | Requisito                                                          |
|----------------------|----------------------------------------------------------------------|
| Tamaño del paquete         | .appxbundle: 25 GB como máximo por paquete <br>Paquetes .appx destinados a Windows 10: 25 GB como máximo por paquete<br>Paquetes .appx destinados a Windows 8.1: 8 GB como máximo por paquete <br> Paquetes .appx destinados a Windows 8: 2 GB como máximo por paquete <br> Paquetes .appx destinados a Windows Phone 8.1: 4 GB como máximo por paquete <br> Paquetes .xap: 1 GB como máximo por paquete                                                                           |
| Hash de asignación de bloque     | Algoritmo SHA2-256                                                   |
 

## <a name="storemanifest-xml-file"></a>Archivo XML de StoreManifest

StoreManifest.xml es un archivo de configuración opcional que se puede incluir en paquetes de la aplicación. Su objetivo es habilitar características como, por ejemplo, declarar tu aplicación como una aplicación para dispositivo de la Tienda Windows o declarar los requisitos de los que depende un paquete para que se pueda aplicar a un dispositivo, que no incluye el manifiesto del paquete. StoreManifest.xml se envía con el paquete de la aplicación y debe estar en la carpeta raíz del proyecto principal de la aplicación. Para obtener más información, consulta [Esquema StoreManifest](https://msdn.microsoft.com/library/windows/apps/mt617325).

 

 





