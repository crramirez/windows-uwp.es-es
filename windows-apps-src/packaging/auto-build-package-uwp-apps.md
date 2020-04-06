---
title: Configurar compilaciones automatizadas para la aplicación para UWP
description: Cómo configurar las compilaciones automatizadas para producir paquetes de instalación de prueba o de Store.
ms.date: 07/17/2019
ms.topic: article
keywords: windows 10, uwp
ms.assetid: f9b0d6bd-af12-4237-bc66-0c218859d2fd
ms.localizationpriority: medium
ms.openlocfilehash: 70415c9f3d58625cfdc651ec67c8a9f37c23cffa
ms.sourcegitcommit: 3e7a4f7605dfb4e87bac2d10b6d64f8b35229546
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/08/2020
ms.locfileid: "77089501"
---
# <a name="set-up-automated-builds-for-your-uwp-app"></a>Configurar compilaciones automatizadas para la aplicación para UWP

Puedes usar Azure Pipelines para crear compilaciones automatizadas para los proyectos de UWP. En este artículo, analizaremos diferentes formas de hacerlo. También te mostraremos cómo realizar estas tareas mediante el uso de la línea de comandos para que puedas integrar con cualquier otro sistema de compilación.

## <a name="create-a-new-azure-pipeline"></a>Creación de una nueva canalización de Azure

Comienza por [registrarte en Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/get-started/pipelines-sign-up) si todavía no lo has hecho.

Posteriormente, crea una canalización que puedas usar para compilar el código fuente. Para ver un tutorial sobre la creación de una canalización para compilar un repositorio de GitHub, consulta [Crea tu primera canalización](https://docs.microsoft.com/azure/devops/pipelines/get-started-yaml). Azure Pipelines admite los tipos de repositorios que aparecen [en este artículo](https://docs.microsoft.com/azure/devops/pipelines/repos).

## <a name="set-up-an-automated-build"></a>Configurar una compilación automatizada

Comenzaremos con la definición de compilación predeterminada de UWP, que está disponible en Azure Dev Ops y, después, te mostraremos cómo configurar la canalización.

En la lista de plantillas de definición de compilación, elige la plantilla **Plataforma universal de Windows**.

![Seleccionar la plantilla de UWP](images/select-yaml-template.png)

Esta plantilla incluye la configuración básica para compilar el proyecto de UWP:

```yml
trigger:
- master

pool:
  vmImage: 'windows-latest'

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

La plantilla predeterminada intenta firmar el paquete con el certificado especificado en el archivo .csproj. Si quieres firmar el paquete durante la compilación, debes tener acceso a la clave privada. De lo contrario, puedes agregar el parámetro `/p:AppxPackageSigningEnabled=false` a la sección `msbuildArgs` del archivo YAML para deshabilitar la firma.

## <a name="add-your-project-certificate-to-the-secure-files-library"></a>Agregar el certificado de proyecto a la biblioteca de archivos seguros

Si es posible, debes evitar el envío de certificados al repositorio, ya que Git los ignorará de forma predeterminada. Para administrar el control seguro de archivos confidenciales como los certificados, Azure DevOps admite la característica de [archivos seguros](https://docs.microsoft.com/azure/devops/pipelines/library/secure-files?view=azure-devops).

Para cargar un certificado para la compilación automatizada:

1. En Azure Pipelines, expande **Canalizaciones** en el panel de navegación y haz clic en **Biblioteca**.
2. Haz clic en la pestaña **Archivos seguros** y, a continuación, haz clic en **+ Archivo seguro**.

    ![Cómo cargar un archivo seguro](images/secure-file1.png)

3. Busca el archivo de certificado y haz clic en **Aceptar**.
4. Después de cargar el certificado, selecciónalo para ver sus propiedades. En **Permisos de canalización**, habilita la opción **Autorizar para su uso en todas las canalizaciones**.

    ![Cómo cargar un archivo seguro](images/secure-file2.png)

5. Si la clave privada del certificado tiene una contraseña, se recomienda que almacenes la contraseña en [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates) y, posteriormente, vincules la contraseña a un [grupo de variables](https://docs.microsoft.com/azure/devops/pipelines/library/variable-groups). Puedes usar las variables para acceder a la contraseña desde la canalización. Ten en cuenta que una contraseña solo se admite para la clave privada; actualmente no se admite el uso de un archivo de certificado protegido por contraseña.

> [!NOTE]
> A partir de Visual Studio 2019, ya no se genera un certificado temporal en los proyectos de UWP. Para crear o exportar certificados, usa los cmdlets de PowerShell que se describen en [este artículo](/windows/msix/package/create-certificate-package-signing).

## <a name="configure-the-build-solution-build-task"></a>Configurar la tarea de compilación de compilación de soluciones

Esta tarea compila cualquier solución que se encuentre en la carpeta de trabajo en archivos binarios y produce el archivo del paquete de la aplicación. Esta tarea utiliza argumentos de MSBuild. Tendrás que especificar el valor de los argumentos. Usa la siguiente tabla como guía.

|**Argumento de MSBuild**|**Valor**|**Descripción**|
|--------------------|---------|---------------|
| AppxPackageDir | $(Build.ArtifactStagingDirectory)\AppxPackages | Define la carpeta en la que almacenar los artefactos generados. |
| AppxBundlePlatforms | $(Build.BuildPlatform) | Te permite definir las plataformas que se incluirán en el lote. |
| AppxBundle | Siempre | Crea un archivo .msixbundle o .appxbundle con los archivos .msix o .appx para la plataforma especificada. |
| UapAppxPackageBuildMode | StoreUpload | Genera el archivo .msixupload o .appxupload y la carpeta **_Test** para la instalación de prueba. |
| UapAppxPackageBuildMode | CI | Genera el archivo .msixupload o .appxupload únicamente. |
| UapAppxPackageBuildMode | SideloadOnly | Genera la carpeta **_Test** solo para la instalación de prueba. |
| AppxPackageSigningEnabled | true | Habilita la firma del paquete. |
| PackageCertificateThumbprint | Huella digital del certificado | Este valor **debe** coincidir con la huella digital del certificado de firma o ser una cadena vacía. |
| PackageCertificateKeyFile | Ruta | La ruta de acceso al certificado que se utilizará. Se recupera de los metadatos del archivo seguro. |
| PackageCertificatePassword | Contraseña | La contraseña para la clave privada del certificado. Se recomienda que almacenes la contraseña en [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates) y vincules la contraseña a un [grupo de variables](https://docs.microsoft.com/azure/devops/pipelines/library/variable-groups). Puedes transferir la variable a este argumento. |

### <a name="configure-the-build"></a>Configurar la compilación

Si quieres compilar tu solución usando la línea de comandos o cualquier otro sistema de compilación, ejecuta MSBuild con estos argumentos.

```powershell
/p:AppxPackageDir="$(Build.ArtifactStagingDirectory)\AppxPackages\\"
/p:UapAppxPackageBuildMode=StoreUpload
/p:AppxBundlePlatforms="$(Build.BuildPlatform)"
/p:AppxBundle=Always
```

### <a name="configure-package-signing"></a>Configuración de la firma de paquetes

Para firmar un paquete MSIX (o .appx), la canalización debe recuperar el certificado de firma. Para ello, agrega una tarea DownloadSecureFile antes de la tarea VSBuild.
Esto te proporcionará acceso al certificado de firma a través de ```signingCert```.

```yml
- task: DownloadSecureFile@1
  name: signingCert
  displayName: 'Download CA certificate'
  inputs:
    secureFile: '[Your_Pfx].pfx'
```

A continuación, actualiza la tarea de MSBuild para hacer referencia al certificado de firma:

```yml
- task: VSBuild@1
  inputs:
    platform: 'x86'
    solution: '$(solution)'
    configuration: '$(buildConfiguration)'
    msbuildArgs: '/p:AppxBundlePlatforms="$(buildPlatform)" 
                  /p:AppxPackageDir="$(appxPackageDir)" 
                  /p:AppxBundle=Always 
                  /p:UapAppxPackageBuildMode=StoreUpload 
                  /p:AppxPackageSigningEnabled=true
                  /p:PackageCertificateThumbprint="" 
                  /p:PackageCertificateKeyFile="$(signingCert.secureFilePath)"'
```

> [!NOTE]
> El argumento PackageCertificateThumbprint se establece intencionadamente en una cadena vacía como precaución. Si la huella digital está establecida en el proyecto, pero no coincide con el certificado de firma, se producirá un error en la compilación: `Certificate does not match supplied signing thumbprint`.

### <a name="review-parameters"></a>Revisión de parámetros

Los parámetros definidos con la sintaxis `$()` son variables definidas en la definición de compilación y cambiarán en otros sistemas de compilación.

![variables predeterminadas](images/building-screen5.png)

Para ver todas las variables predefinidas, consulta [Variables de compilación predefinidas](https://docs.microsoft.com/azure/devops/pipelines/build/variables).

## <a name="configure-the-publish-build-artifacts-task"></a>Configuración de la tarea para publicar artefactos de compilación

La canalización de UWP predeterminada no guarda los artefactos generados. Para agregar las capacidades de publicación a la definición de YAML, agrega las siguientes tareas.

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

Puedes ver los artefactos generados en la opción **Artefactos** de la página de resultados de la compilación.

![artefactos](images/building-screen6.png)

Dado que hemos establecido la propiedad `UapAppxPackageBuildMode` en `StoreUpload`, la carpeta de artefactos incluye el paquete para el envío a Store (.msixupload/.appxupload). Ten en cuenta que también puedes enviar un paquete de la aplicación normal (.msix/.appx) o un lote de aplicaciones (.appxbundle) a Store. Para este artículo, usaremos el archivo .appxupload.

## <a name="address-bundle-errors"></a>Errores de agrupación de direcciones

Si agregas más de un proyecto de UWP a tu solución y, después, vuelves a intentar crear una agrupación, es posible que recibas un error como el siguiente.

  `MakeAppx(0,0): Error : Error info: error 80080204: The package with file name "AppOne.UnitTests_0.1.2595.0_x86.appx" and package full name "8ef641d1-4557-4e33-957f-6895b122f1e6_0.1.2595.0_x86__scrj5wvaadcy6" is not valid in the bundle because it has a different package family name than other packages in the bundle`

Este error aparece porque en el nivel de la solución, no está claro qué aplicación debería aparecer en el lote. Para resolver este problema, abre cada archivo de proyecto y agrega las siguientes propiedades al final del primer elemento `<PropertyGroup>`.

|**Proyecto**|**Propiedades**|
|-------|----------|
|Aplicación|`<AppxBundle>Always</AppxBundle>`|
|UnitTests|`<AppxBundle>Never</AppxBundle>`|

A continuación, quita el argumento de MSBuild `AppxBundle` del paso de compilación.

## <a name="related-topics"></a>Temas relacionados

- [Compilar la aplicación de .NET para Windows](https://docs.microsoft.com/vsts/build-release/get-started/dot-net)
- [Empaquetado de aplicaciones para UWP](/windows/msix/package/packaging-uwp-apps)
- [Transferir localmente aplicaciones de LOB en Windows 10](https://docs.microsoft.com/windows/deploy/sideload-apps-in-windows-10)
- [Crear un certificado para la firma de paquetes](/windows/msix/package/create-certificate-package-signing)
