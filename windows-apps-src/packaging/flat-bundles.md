---
author: laurenhughes
title: Paquetes de aplicaciones de recopilaciones planos
description: Describe cómo crear una recopilación plana para empaquetar los archivos de paquetes .appx de la aplicación con referencias a los paquetes de aplicaciones.
ms.author: lahugh
ms.date: 04/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, packaging, empaquetado, package configuration, configuración de paquete, flat bundle, recopilación plana
ms.localizationpriority: medium
ms.openlocfilehash: 757f95a5f46bad6dbe650b4b552f3de486d84e1b
ms.sourcegitcommit: 91511d2d1dc8ab74b566aaeab3ef2139e7ed4945
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2018
ms.locfileid: "1818552"
---
# <a name="flat-bundle-app-packages"></a>Paquetes de aplicaciones de recopilaciones planas 

> [!IMPORTANT]
> Si quieres enviar la aplicación a la Store, debes ponerte en contacto con [Soporte técnico para desarrolladores de Windows](https://developer.microsoft.com/windows/support) para la aprobación para utilizar recopilaciones planas.

Las recopilaciones planas son una manera mejorada de empaquetar los archivos de paquete .appx de la aplicación. Un archivo .appxbundle típico usa una estructura de varios niveles de empaquetado en el que los archivos de paquete .appx deben estar contenidos en una recopilación; las recopilaciones planas eliminan esta necesidad haciendo referencia solo a los archivos de paquetes .appx, lo que permite que estén fuera de la recopilación de aplicación. Dado que los archivos de paquetes .appx ya no están incluidos en la recopilación, pueden procesarse en paralelo, lo que reduce el tiempo de carga, acelera la publicación (puesto que cada archivo de paquete .appx puede procesarse al mismo tiempo) y, por último, iteraciones de desarrollo más rápidas.

![Diagrama de la recopilación plana](images/bundle-combined.png)

Otra ventaja de las recopilaciones planas es la necesidad de crear menos paquetes. Dado que se hace referencia solamente a los archivos de paquetes .appx, dos versiones de la aplicación pueden hacer referencia al mismo archivo de paquete si el paquete no se cambió entre las dos versiones. Esto permite crear solo los paquetes de aplicaciones que han cambiado al compilar los paquetes para la siguiente versión de la aplicación.
De manera predeterminada, las recopilaciones planas harán referencia a los archivos de paquete .appx dentro de la misma carpeta por sí mismos. Sin embargo, esta referencia puede cambiarse a otras rutas de acceso (rutas de acceso relativas, recursos compartidos de red y ubicaciones http). Para ello, debes proporcionar directamente un **BundleManifest** durante la creación de la recopilación plana. 

## <a name="how-to-create-a-flat-bundle"></a>Cómo crear una recopilación plana

Una recopilación plana puede crearse con la herramienta MakeAppx.exe, o utilizando el diseño de empaquetado para definir la estructura de la recopilación.

### <a name="using-makeappxexe"></a>Uso de MakeAppx.exe
Para crear una recopilación plana con MakeAppx.exe, usa el comando “MakeAppx.exe bundle” como siempre pero con el modificador /fb para generar el archivo .appxbundle plano (que será muy pequeño, ya que solo hace referencia a los paquetes .appx y no contiene ninguna carga real). 

A continuación presentamos un ejemplo de la sintaxis del comando:

```syntax
MakeAppx bundle [options] /d <content directory> /fb <output flat bundle name>
```

Para obtener más información sobre la utilización de MakeAppx.exe, consulta [Crear un paquete de la aplicación con la herramienta MakeAppx.exe](https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool).

### <a name="using-packaging-layout"></a>Usar diseño de empaquetado
Como alternativa, puedes crear una recopilación plana usando el diseño de empaquetado. Para ello, establece el atributo **FlatBundle** en **true** en el elemento **PackageFamily** del manifiesto de recopilación de aplicación. Para obtener más información sobre el diseño de empaquetado, consulta [Creación del paquete con el diseño del empaquetado](packaging-layout.md).

## <a name="how-to-deploy-a-flat-bundle"></a>Cómo implantar una recopilación plana 
Antes de poder implantar una recopilación plana, cada uno de los paquetes de aplicaciones (además de la recopilación de aplicación) que tengan que firmarse con el mismo certificado. Esto es porque todos los archivos del paquete de la aplicación (.appx) son ahora archivos independientes y ya no están contenidos dentro del archivo de recopilación de aplicación (.appxbundle). Una vez que se firman los paquetes, usa el [cmdlet Add-AppxPackage](https://docs.microsoft.com/powershell/module/appx/add-appxpackage?view=win10-ps) en PowerShell para apuntar al archivo .appxbundle e implementar la aplicación (suponiendo que los paquetes de la aplicación están donde la recopilación de aplicación espera que esté). 