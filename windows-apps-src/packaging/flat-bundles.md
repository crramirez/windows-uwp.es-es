---
author: laurenhughes
title: Paquetes de aplicaciones de recopilaciones planos
description: Describe cómo crear una recopilación plana para empaquetar los archivos de paquetes .appx de la aplicación con referencias a los paquetes de aplicaciones.
ms.author: lahugh
ms.date: 09/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, packaging, empaquetado, package configuration, configuración de paquete, flat bundle, recopilación plana
ms.localizationpriority: medium
ms.openlocfilehash: 63206619d75bedb92ad6c6d05c3188272c0760de
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2018
ms.locfileid: "5397948"
---
# <a name="flat-bundle-app-packages"></a>Paquetes de aplicaciones de recopilaciones planas 

> [!IMPORTANT]
> Si quieres enviar la aplicación a la Store, debes ponerte en contacto con [Soporte técnico para desarrolladores de Windows](https://developer.microsoft.com/windows/support) para la aprobación para utilizar recopilaciones planas.

Planas son una manera mejorada de empaquetar archivos del paquete de la aplicación. Un archivo de paquete de la aplicación usa una estructura de varios niveles de empaquetado en el que deben estar contenidos dentro de la recopilación de los archivos del paquete de aplicación de Windows típico, recopilaciones planas eliminan esta necesidad haciendo referencia solo los archivos del paquete de aplicación, lo que les permite estén fuera de la recopilación de aplicación. Dado que los archivos del paquete de aplicación ya no están incluidos en la recopilación, pueden ser paralelo procesado, lo que da como resultado, en última instancia rápida y reduce el tiempo, acelera la publicación (puesto que cada archivo de paquete de la aplicación puede procesarse al mismo tiempo) de carga desarrollo iteraciones.

![Diagrama de la recopilación plana](images/bundle-combined.png)

Otra ventaja de las recopilaciones planas es la necesidad de crear menos paquetes. Dado que solo se hace referencia a los archivos del paquete de aplicación, dos versiones de la aplicación pueden hacer referencia el mismo archivo de paquete si el paquete no cambió entre las dos versiones. Esto permite crear solo los paquetes de aplicaciones que han cambiado al compilar los paquetes para la siguiente versión de la aplicación.
De manera predeterminada, las recopilaciones planas harán referencia a los archivos de paquete de aplicación dentro de la misma carpeta por sí mismos. Sin embargo, esta referencia puede cambiarse a otras rutas de acceso (rutas de acceso relativas, recursos compartidos de red y ubicaciones http). Para ello, debes proporcionar directamente un **BundleManifest** durante la creación de la recopilación plana. 

## <a name="how-to-create-a-flat-bundle"></a>Cómo crear una recopilación plana

Una recopilación plana puede crearse con la herramienta MakeAppx.exe, o utilizando el diseño de empaquetado para definir la estructura de la recopilación.

### <a name="using-makeappxexe"></a>Uso de MakeAppx.exe
Para crear una recopilación plana con MakeAppx.exe, usa como de costumbre, pero con el modificador /fb el comando "MakeAppx.exe bundle" para generar el archivo de paquete de aplicación plana (que será muy pequeño, ya que solo hace referencia a los archivos del paquete de aplicación y no contiene ninguna carga real ). 

A continuación presentamos un ejemplo de la sintaxis del comando:

```syntax
MakeAppx bundle [options] /d <content directory> /fb <output flat bundle name>
```

Para obtener más información sobre la utilización de MakeAppx.exe, consulta [Crear un paquete de la aplicación con la herramienta MakeAppx.exe](https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool).

### <a name="using-packaging-layout"></a>Usar diseño de empaquetado
Como alternativa, puedes crear una recopilación plana usando el diseño de empaquetado. Para ello, establece el atributo **FlatBundle** en **true** en el elemento **PackageFamily** del manifiesto de recopilación de aplicación. Para obtener más información sobre el diseño de empaquetado, consulta [Creación del paquete con el diseño del empaquetado](packaging-layout.md).

## <a name="how-to-deploy-a-flat-bundle"></a>Cómo implantar una recopilación plana 
Antes de poder implantar una recopilación plana, cada uno de los paquetes de aplicaciones (además de la recopilación de aplicación) que tengan que firmarse con el mismo certificado. Esto es porque todos los archivos de paquete de aplicación (.appx/.msix) son ahora archivos independientes y ya no están contenidos dentro del archivo de paquete (.appxbundle/.msixbundle) de la aplicación. Una vez que se firman los paquetes, use el [cmdlet Add-AppxPackage](https://docs.microsoft.com/powershell/module/appx/add-appxpackage?view=win10-ps) de PowerShell para apuntar al archivo de paquete de aplicación e implementar la aplicación (suponiendo que los paquetes de aplicación están donde la recopilación de aplicación espera que esté). 