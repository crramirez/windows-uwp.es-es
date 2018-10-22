---
author: laurenhughes
title: Introducción a los paquetes de activos
description: Los paquetes de recurso son un tipo de paquete que actúa como una ubicación centralizada para archivos comunes de la aplicación, lo que elimina la necesidad de archivos duplicados en todos los paquetes de arquitectura.
ms.author: lahugh
ms.date: 09/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, packaging, empaquetado, package layout, distribución de paquete, asset package, paquete de recursos
ms.localizationpriority: medium
ms.openlocfilehash: 8aafac1c1217ce082cd9d6176c530967f32e4cdd
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2018
ms.locfileid: "5402419"
---
# <a name="introduction-to-asset-packages"></a>Introducción a los paquetes de activos

> [!IMPORTANT]
> Si quieres enviar la aplicación a la Store, debes ponerte en contacto con [Soporte técnico para desarrolladores de Windows](https://developer.microsoft.com/windows/support) para obtener la aprobación para utilizar paquetes de activos.

Los paquetes de activos son un tipo de paquete que actúa como una ubicación centralizada para archivos comunes de la aplicación, lo que elimina la necesidad de archivos duplicados en todos los paquetes de arquitectura. Los paquetes de activos son similares a los paquetes de recursos en que se han diseñado para incluir contenido estático necesario para que se ejecute tu aplicación, pero diferentes en que todos los paquetes de activos siempre se descargan, independientemente de la arquitectura, idioma o escala de pantalla del sistema del usuario.

![Diagrama de recopilación de paquetes de activos](images/primary-bundle.png)

Dado que los paquetes de activos contienen todos los archivos independientes de arquitectura, idioma y escala, aprovechando los resultados de los paquetes de activos en un tamaño general de aplicaciones empaquetadas reducido (ya que estos archivos ya no se duplican), ayudando a administrar el uso del espacio de disco de desarrollo local para aplicaciones de gran tamaño y administrar los paquetes de la aplicación en general. 

### <a name="how-do-asset-packages-affect-publishing"></a>¿Cómo afectan los paquetes de activos a la publicación?
La ventaja más evidente de los paquetes de activos es el tamaño reducido de las aplicaciones empaquetadas. Los paquetes de la aplicación más pequeños aceleran el proceso de publicación de la aplicación al permitir que la Store procese menos archivos; sin embargo, esta no es la principal ventaja de los paquetes de activos.

Cuando se crea un paquete de recursos, puedes especificar si se debe permitir que se ejecute el paquete. Dado que los paquetes de activos solamente contienen archivos independientes de arquitectura, por lo general, no contienen ningún archivo .dll o .exe, por lo que normalmente no hay que ejecutar los paquetes de activos. La importancia de esta distinción es que durante el proceso de publicación, deben analizarse todos los paquetes ejecutables para garantizar que no contienen malware y este proceso de exploración tarda más tiempo para los paquetes más grandes. Sin embargo, si un paquete está designado como no ejecutable, la instalación de la aplicación garantizará que no se puedan ejecutar los archivos contenidos en este paquete. Esta garantía elimina la necesidad de una exploración completa del paquete y reducirá en gran medida el tiempo de exploración de malware durante la publicación de la aplicación (y también en actualizaciones):, por consiguiente, acelera notablemente la publicación en aplicaciones que usan los paquetes de activos. Ten en cuenta que [los paquetes de aplicaciones de recopilación plana](flat-bundles.md) también debe usarse para obtener esta ventaja de publicación, dado que esto es lo que permite a la tienda procesar cada archivo de paquete .appx o .msix en paralelo. 


### <a name="should-i-use-asset-packages"></a>¿Debo usar paquetes de activos?
Actualizar la estructura de archivos de tu aplicación puede aprovechar el uso de paquetes de activos puede ofrecer ventajas tangibles: tamaño del paquete reducido e iteraciones de desarrollo más eficiente. Si todos los paquetes de arquitectura contienen gran cantidad de archivos en común o si la mayor parte de la aplicación se compone de archivos no ejecutables, es muy recomendable que inviertas el tiempo adicional para cambiar a utilizar paquetes de activos.

Sin embargo, debe advertirse que los paquetes de activos no son un medio para lograr la opcionalidad del contenido de la aplicación. Los archivos de paquetes de activos son no opcionales y **siempre** se descargarán independientemente de la arquitectura, el idioma o la escala del dispositivo de destino; cualquier contenido opcional que quieras que tu aplicación admita debe implementarse con [paquetes opcionales](optional-packages.md). 


### <a name="how-to-create-an-asset-package"></a>Cómo crear un paquete de recursos
La forma más sencilla de crear paquetes de activos es usar el diseño de empaquetado. Sin embargo, los paquetes de activos también pueden crearse manualmente con MakeAppx.exe. Para especificar qué archivos incluir en el paquete de activos, deberás crear un "archivo de asignación". En este ejemplo, el archivo único en el paquete de elemento es "Video.mp4", pero los archivos de todos del paquete de activos deben aparecer aquí. Ten en cuenta que el especificador **ResourceDimensions** en **ResourceMetadata** se omite para los paquetes de activos (en comparación con un archivo de asignación para paquetes de recursos).

```example 
[ResourceMetadata]
"ResourceId"        "Videos"

[Files]
"Video.mp4"         "Video.mp4"
```

Usa este comando para crear el paquete de activos con MakeAppx.exe: 

```syntax 
MakeAppx.exe pack /r /m AppxManifest.xml /f MappingFile.txt /p Videos.appx

...

MakeAppx.exe pack /r /m AppxManifest.xml /f MappingFile.txt /p Videos.msix

```
Aquí se debe tener en cuenta que todos los archivos referenciados en AppxManifest (archivos de logotipos) no se pueden mover en paquetes de activos, estos archivos se deben duplicar en todos los paquetes de arquitectura. Los paquetes de activos no deben contener un resources.pri; MRT no puede usarse para acceder a los archivos de paquetes de activos. Para obtener más información sobre cómo tener acceso a los archivos de recursos y por qué los paquetes de activos necesitan la instalación de tu aplicación en una unidad NTFS, consulta [Desarrollar con paquetes de activos y plegado de paquete](Package-Folding.md).

Para controlar si se permite ejecutar o no un paquete de recursos, puedes usar **[uap6:AllowExecution](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap6-allowexecution)** en el elemento **Propiedades** de AppxManifest . Debes también agregar **uap6** en el elemento de nivel superior **Paquete** para convertirse en lo siguiente: 

```XML
<Package IgnorableNamespaces="uap uap6" 
xmlns:uap6="http://schemas.microsoft.com/appx/manifest/uap/windows10/6" 
xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10" 
xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10">
```

 Si no se especifica, el valor predeterminado de **AllowExecution** es **true**; establécelo en **false** para los paquetes de activos sin ejecutables para hacer la publicación con mayor rapidez.  



