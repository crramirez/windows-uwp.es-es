---
title: Configurar compilaciones automatizadas para la aplicación para UWP
description: Cómo configurar las compilaciones automatizadas para producir paquetes de instalaciones de prueba o paquetes de la Store.
ms.date: 09/30/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: f9b0d6bd-af12-4237-bc66-0c218859d2fd
ms.localizationpriority: medium
ms.openlocfilehash: cb21573dac0c4cc4fc2d6aa2e2345c56631fde87
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372763"
---
# <a name="set-up-automated-builds-for-your-uwp-app"></a>Configurar compilaciones automatizadas para la aplicación para UWP

Puede utilizar las canalizaciones de Azure para crear compilaciones automatizadas para los proyectos de UWP. En este artículo, daremos un vistazo en diferentes formas de hacerlo. También le mostraremos cómo realizar estas tareas mediante el uso de la línea de comandos para que pueda integrar con cualquier otro sistema de compilación.

## <a name="create-a-new-azure-pipeline"></a>Crear una nueva canalización de Azure

Empiece por [registrarse en las canalizaciones de Azure](https://docs.microsoft.com/azure/devops/pipelines/get-started/pipelines-sign-up) si no lo ha hecho ya.

A continuación, cree una canalización que puede usar para compilar el código fuente. Para ver un tutorial sobre la creación de una canalización para crear un repositorio de GitHub, consulte [crear su primera canalización](https://docs.microsoft.com/azure/devops/pipelines/get-started-yaml). Las canalizaciones de Azure es compatible con los tipos de repositorio enumerados [en este artículo](https://docs.microsoft.com/azure/devops/pipelines/repos).

## <a name="set-up-an-automated-build"></a>Configurar una compilación automatizada

Comenzaremos con el valor predeterminado para UWP compilar definición que está disponible en las operaciones de desarrollo de Azure y, a continuación, se muestra cómo configurar la canalización.

En la lista de plantillas de definición de compilación, elige la plantilla **Plataforma universal de Windows**.

![Seleccione la plantilla UWP](images/select-yaml-template.png)

Esta plantilla incluye la configuración básica para compilar el proyecto UWP:

```yml
trigger:
- master

pool:
  vmImage: 'VS2017-Win2016'

variables:
  solution: '**/*.sln'
  buildPlatform: 'x86|x64|ARM'
  buildConfiguration: 'Release'
  appxPackageDir: '$(build.artifactStagingDirectory)\AppxPackages\\'

steps:
- task: NuGetToolInstaller@0

- task: NuGetCommand@2
  inputs:
    restoreSolution: '$(solution)'

- task: VSBuild@1
  inputs:
    platform: 'x86'
    solution: '$(solution)'
    configuration: '$(buildConfiguration)'
    msbuildArgs: '/p:AppxBundlePlatforms="$(buildPlatform)" /p:AppxPackageDir="$(appxPackageDir)" /p:AppxBundle=Always /p:UapAppxPackageBuildMode=StoreUpload'

```

La plantilla predeterminada intenta firmar el paquete con el certificado especificado en el archivo .csproj. Si desea firmar el paquete durante la compilación debe tener acceso a la clave privada. En caso contrario, se puede deshabilitar la firma agregando el parámetro `/p:AppxPackageSigningEnabled=false` a la `msbuildArgs` sección en el archivo YAML.

## <a name="add-your-project-certificate-to-a-repository"></a>Agregar el certificado de proyecto a un repositorio

Canalizaciones funciona con repositorios de Azure repositorios Git y TFVC. Si usas un repositorio de Git, agrega el archivo de certificado del proyecto al repositorio para que el agente de compilación pueda firmar el paquete de aplicación. Si no haces esto, el repositorio de Git omitirá el archivo de certificado. Para agregar el archivo de certificado en el repositorio, haga clic en el archivo de certificado en **el Explorador de soluciones**y, a continuación, en el menú contextual, elija el **Add File omite al Control de código fuente** comando.

![cómo incluir un certificado](images/building-screen1.png)

## <a name="configure-the-build-solution-build-task"></a>Configurar la tarea de compilación de compilación de soluciones

Esta tarea compila cualquier solución que se encuentra en la carpeta de trabajo a los archivos binarios y genera el archivo de paquete de aplicación de salida.
Esta tarea usa argumentos de MSBuild. Tendrás que especificar el valor de los argumentos. Usa la siguiente tabla como guía.

|**Argumento de MSBuild**|**Valor**|**Descripción**|
|--------------------|---------|---------------|
| AppxPackageDir | $(Build.ArtifactStagingDirectory)\AppxPackages | Define la carpeta en la que almacenar los artefactos generados. |
| AppxBundlePlatforms | $(Build.BuildPlatform) | Permite definir las plataformas para incluir en el paquete. |
| AppxBundle | Siempre | Crea un.msixbundle/.appxbundle con los archivos.msix/.appx para la plataforma especificada. |
| UapAppxPackageBuildMode | StoreUpload | Genera el archivo.msixupload/.appxupload y **_probar** carpeta de instalación de prueba. |
| UapAppxPackageBuildMode | CI | Genera el archivo.msixupload/.appxupload solo. |
| UapAppxPackageBuildMode | SideloadOnly | Genera el **_probar** carpeta sólo la instalación de prueba |

Si desea compilar la solución mediante el uso de la línea de comandos, o mediante cualquier otro sistema de compilación, puede ejecutar MSBuild con estos argumentos.

```powershell
/p:AppxPackageDir="$(Build.ArtifactStagingDirectory)\AppxPackages\\"
/p:UapAppxPackageBuildMode=StoreUpload
/p:AppxBundlePlatforms="$(Build.BuildPlatform)"
/p:AppxBundle=Always
```

Los parámetros definidos con el `$()` sintaxis son variables definidas en la definición de compilación y compilará el cambio en otros sistemas.

![variables predeterminadas](images/building-screen5.png)

Para ver todas las variables predefinidas, vea [variables de compilación predefinidos](https://docs.microsoft.com/azure/devops/pipelines/build/variables).

## <a name="configure-the-publish-build-artifacts-task"></a>Configurar la tarea de publicar artefactos de compilación

La canalización UWP predeterminada no guarda los artefactos generados. Para agregar las capacidades de publicación a la definición de YAML, agregue las siguientes tareas.

```yml
- task: CopyFiles@2
  displayName: 'Copy Files to: $(build.artifactstagingdirectory)'
  inputs:
    SourceFolder: '$(system.defaultworkingdirectory)'
    Contents: '**\bin\$(BuildConfiguration)\**'
    TargetFolder: '$(build.artifactstagingdirectory)'

- task: PublishBuildArtifacts@1
  displayName: 'Publish Artifact: drop'
  inputs:
    PathtoPublish: '$(build.artifactstagingdirectory)'
```

Puede ver los artefactos generados en el **artefactos** página resultados de la opción de la compilación.

![artefactos](images/building-screen6.png)

Dado que hemos establecido el `UapAppxPackageBuildMode` argumento `StoreUpload`, la carpeta de artefactos incluye el paquete para enviarlo a la Store (.msixupload/.appxupload). Tenga en cuenta que también puede enviar un paquete de aplicación normal (.msix/.appx) o un grupo de aplicaciones (.msixbundle/.appxbundle/) para el Store. Para este artículo, usaremos el archivo .appxupload.

## <a name="address-bundle-errors"></a>Errores de lote de dirección

Si agrega más de un proyecto UWP a la solución y, a continuación, intente crear un paquete, puede recibir un error similar a ésta.

  `MakeAppx(0,0): Error : Error info: error 80080204: The package with file name "AppOne.UnitTests_0.1.2595.0_x86.appx" and package full name "8ef641d1-4557-4e33-957f-6895b122f1e6_0.1.2595.0_x86__scrj5wvaadcy6" is not valid in the bundle because it has a different package family name than other packages in the bundle`

Este error aparece porque en el nivel de la solución, no está claro qué aplicación debería aparecer en el lote. Para resolver este problema, abra cada archivo de proyecto y agregue las siguientes propiedades al final de la primera `<PropertyGroup>` elemento.

|**Project**|**Propiedades**|
|-------|----------|
|Aplicación|`<AppxBundle>Always</AppxBundle>`|
|UnitTests|`<AppxBundle>Never</AppxBundle>`|

A continuación, quite el `AppxBundle` argumento de MSBuild desde el paso de compilación.

## <a name="related-topics"></a>Temas relacionados

- [Compile la aplicación de .NET para Windows](https://www.visualstudio.com/docs/build/get-started/dot-net)
- [Empaquetado de aplicaciones para UWP](https://docs.microsoft.com/windows/uwp/packaging/packaging-uwp-apps)
- [Transferir localmente aplicaciones LOB de Windows 10](https://technet.microsoft.com/itpro/windows/deploy/sideload-apps-in-windows-10)
- [Crear un certificado de firma del paquete](https://docs.microsoft.com/windows/uwp/packaging/create-certificate-package-signing)
