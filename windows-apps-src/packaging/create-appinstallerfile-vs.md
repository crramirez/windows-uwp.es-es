---
author: ridomin
title: Crear un archivo de Instalador de aplicación con Visual Studio
description: Obtén información sobre cómo usar Visual Studio para habilitar las actualizaciones automáticas mediante el archivo .appinstaller.
ms.author: rmpablos
ms.date: 5/2/2018
ms.topic: article
keywords: windows 10, uwp, app installer, instalador de aplicaciones, AppInstaller, sideload, realizar instalación de prueba
ms.localizationpriority: medium
ms.openlocfilehash: 8d4740c132f274cb3eeee5776790d077d2cbec47
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2018
ms.locfileid: "5561743"
---
# <a name="create-an-app-installer-file-with-visual-studio"></a>Crear un archivo de Instalador de aplicación con Visual Studio

A partir de Windows 10, versión 1804 y Visual Studio 2017, Update 15.7, las aplicaciones con instalación de prueba pueden configurarse para recibir actualizaciones automáticas con un archivo `.appinstaller`. Visual Studio admite la habilitación de estas actualizaciones.

## <a name="app-installer-file-location"></a>Ubicación del archivo del Instalador de aplicación
El archivo `.appinstaller` puede estar alojado en una ubicación compartida como un extremo HTTP o una carpeta UNC compartida e incluye la ruta de acceso para buscar los paquetes de aplicaciones que se van a instalar. Los usuarios instalan la aplicación desde la ubicación compartida y habilitan comprobaciones periódicas de nuevas actualizaciones. 


### <a name="configure-the-project-to-target-the-correct-windows-version"></a>Configurar el proyecto para orientarlo a la versión correcta de Windows

Puedes configurar la propiedad `TargetPlatformMinVersion` al crear el proyecto o cambiarla más adelante desde las propiedades del proyecto. 

>[!IMPORTANT]
> El archivo del instalador de la aplicación solo se genera cuando la `TargetPlatformMinVersion` es Windows 10, versión 1804 o superior.


### <a name="create-packages"></a>Crear paquetes

Para distribuir una aplicación a través de la instalación de prueba, debes crear un paquete de la aplicación (.appx/.msix) o un lote de aplicaciones (.appxbundle/.msixbundle) y publicarlo en una ubicación compartida.

Para ello, usa el asistente **Crear paquetes de aplicaciones** en Visual Studio con los siguientes pasos.

1. Haz clic con el botón derecho en el proyecto y elige **Store** -> **Crear paquetes de aplicaciones**.  

![Menú contextual con navegación para crear paquetes de aplicaciones](images/packaging-screen2.jpg)   

A continuación, se mostrará el asistente **Crear paquetes de aplicaciones**.

2. Selecciona **I want to create packages for sideloading.** y **Habilitar las actualizaciones automáticas**  

![Visualización de la ventana de diálogo Crear los paquetes](images/select-sideloading.png)  

**Habilitar las actualizaciones automáticas** se habilita únicamente si la `TargetPlatformMinVersion` del proyecto se establece en la versión correcta de Windows 10.

3. El cuadro de diálogo **Seleccionar y configurar paquetes** permite seleccionar las configuraciones de arquitectura admitidas. Si seleccionas una recopilación, se generará un instalador único; sin embargo, si no desea una recopilación y prefieres un paquete por arquitectura también obtendrás un archivo de instalador por arquitectura.  Si no estás seguro de qué arquitecturas elegir o quieres obtener más información sobre qué arquitecturas se usan en varios dispositivos, consulta [Arquitecturas de paquete de aplicación](device-architecture.md).

4. Configura detalles adicionales, como la numeración de la versión o la ubicación de salida del paquete.

![Visualización de la ventana de Crear paquetes de aplicaciones con la configuración del paquete](images/packaging-screen5.jpg)  

5. Si has marcado **Habilitar las actualizaciones automáticas** en el paso 2, aparecerá el diálogo **Configure Update Settings**. Aquí, puedes especificar la **URL de instalación** y la frecuencia de las comprobaciones de actualizaciones.

![Configurar la ventana de configuración de actualizaciones con la configuración de ubicación de publicación mostrada](images/sideloading-screen.png)  

6. Cuando la aplicación se haya empaquetada correctamente, un cuadro de diálogo mostrará la ubicación de la carpeta de resultados que contiene el paquete de la aplicación. La carpeta de resultados incluye todos los archivos necesarios para realizar instalaciones de prueba de la aplicación, como una página HTML que puede usarse para promocionar la aplicación.

### <a name="publish-packages"></a>Publicar paquetes

Para que la aplicación esté disponible, los archivos generados deben publicarse en la ubicación especificada:

#### <a name="publish-to-shared-folders-unc"></a>Publicar en carpetas compartidas (UNC)

Si quieres publicar los paquetes en carpetas compartidas de convención de nomenclatura universal (UNC), configura la carpeta de salida del paquete de aplicación y la dirección URL de instalación (consulta el paso 6 para obtener más detalles) en la misma ruta de acceso. El asistente generará los archivos en la ubicación correcta y los usuarios obtendrán la aplicación y futuras actualizaciones desde la misma ruta de acceso.

#### <a name="publish-to-a-web-location-http"></a>Publicar en una ubicación web (HTTP)

Publicar en una ubicación web requiere acceso para publicar el contenido en el servidor web, asegurándose de que la dirección URL final coincida con la dirección URL de instalación definida en el asistente (consulta el paso 6 para obtener más información). Por lo general, se usa el protocolo de transferencia de archivos (FTP) o el protocolo de transferencia de archivos SSH (SFTP) para cargar los archivos, pero hay otros métodos de publicación como almacenamiento MSDeploy, SSH o Blob, según el proveedor web.

Para configurar el servidor web, debes verificar los tipos MIME usados para los tipos de archivo en uso. Este ejemplo es de `web.config` para Internet Information Services (IIS):

```xml
<configuration>
  <system.webServer>
    <staticContent>
      <mimeMap fileExtension=".appx" mimeType="application/vns.ms-appx" />
      <mimeMap fileExtension=".appxbundle" mimeType="application/vns.ms-appx" />
      <mimeMap fileExtension=".appinstaller" mimeType="application/xml" />
    </staticContent>  
  </system.webServer>  
</configuration>
```




















