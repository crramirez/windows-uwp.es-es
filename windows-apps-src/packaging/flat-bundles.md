---
title: Paquetes de aplicaciones de lotes planos
description: Describe cómo crear una recopilación plana para empaquetar los archivos de paquetes .appx de la aplicación con referencias a los paquetes de aplicaciones.
ms.date: 09/30/2018
ms.topic: article
keywords: windows 10, packaging, empaquetado, package configuration, configuración de paquete, flat bundle, recopilación plana
ms.localizationpriority: medium
ms.openlocfilehash: b7066b7f2e5bd72ebee3169e03c7940b6fef4dba
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57638490"
---
# <a name="flat-bundle-app-packages"></a>Paquetes de aplicaciones de lotes planos 

> [!IMPORTANT]
> Si quieres enviar la aplicación a la Store, debes ponerte en contacto con [Soporte técnico para desarrolladores de Windows](https://developer.microsoft.com/windows/support) para la aprobación para utilizar recopilaciones planas.

Agrupaciones planos son una mejor forma de agrupar los archivos de paquete de la aplicación. Un Windows típico archivo de paquete de aplicación usa una estructura de empaquetado de varios niveles en la que deben estar dentro de la agrupación de los archivos del paquete de aplicación, paquetes planos quitar esta necesidad, sólo hace referencia a los archivos del paquete de aplicación, lo que les permite que se encuentre fuera del paquete de aplicaciones. Puesto que los archivos del paquete de aplicación ya no se incluyen en el paquete, pueden ser paralelo procesado, por lo reducido cargar más rápida, tiempo de publicación (ya que cada archivo de paquete de aplicación se puede procesar al mismo tiempo) y, a continuación, en última instancia, más rápido desarrollo iteraciones.

![Diagrama de la recopilación plana](images/bundle-combined.png)

Otra ventaja de las recopilaciones planas es la necesidad de crear menos paquetes. Puesto que sólo se hace referencia a los archivos de paquete de aplicación, las dos versiones de la aplicación pueden hacer referencia el mismo archivo de paquete si el paquete no ha cambiado entre las dos versiones. Esto permite crear solo los paquetes de aplicaciones que han cambiado al compilar los paquetes para la siguiente versión de la aplicación.
De forma predeterminada, los paquetes sin formato hará referencia a los archivos de paquete de aplicación dentro de la misma carpeta que el propio. Sin embargo, esta referencia puede cambiarse a otras rutas de acceso (rutas de acceso relativas, recursos compartidos de red y ubicaciones http). Para ello, debes proporcionar directamente un **BundleManifest** durante la creación de la recopilación plana. 

## <a name="how-to-create-a-flat-bundle"></a>Cómo crear una recopilación plana

Una recopilación plana puede crearse con la herramienta MakeAppx.exe, o utilizando el diseño de empaquetado para definir la estructura de la recopilación.

### <a name="using-makeappxexe"></a>Uso de MakeAppx.exe
Para crear un paquete sin formato mediante el MakeAppx.exe, use como de costumbre, pero con el modificador /fb el comando "MakeAppx.exe" para generar el archivo de paquete de aplicación sin formato (que será muy pequeño, ya que solo hace referencia a los archivos del paquete de aplicación y no contiene las cargas reales ). 

A continuación presentamos un ejemplo de la sintaxis del comando:

```syntax
MakeAppx bundle [options] /d <content directory> /fb /p <output flat bundle name>
```

Para obtener más información sobre la utilización de MakeAppx.exe, consulta [Crear un paquete de la aplicación con la herramienta MakeAppx.exe](https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool).

### <a name="using-packaging-layout"></a>Usar diseño de empaquetado
Como alternativa, puedes crear una recopilación plana usando el diseño de empaquetado. Para ello, establece el atributo **FlatBundle** en **true** en el elemento **PackageFamily** del manifiesto de recopilación de aplicación. Para obtener más información sobre el diseño de empaquetado, consulta [Creación del paquete con el diseño del empaquetado](packaging-layout.md).

## <a name="how-to-deploy-a-flat-bundle"></a>Cómo implantar una recopilación plana 
Antes de poder implantar una recopilación plana, cada uno de los paquetes de aplicaciones (además de la recopilación de aplicación) que tengan que firmarse con el mismo certificado. Esto es porque todos los archivos de paquete de aplicación (.appx/.msix) ahora son archivos independientes y ya no están contenidos en el archivo de paquete (.appxbundle/.msixbundle) de la aplicación. Una vez que haya iniciado los paquetes, use la [cmdlet Add-AppxPackage](https://docs.microsoft.com/powershell/module/appx/add-appxpackage?view=win10-ps) en PowerShell para que apunte al archivo de paquete de la aplicación e implementar la aplicación (suponiendo que los paquetes de aplicaciones son donde espera que el lote de aplicaciones que sean). 