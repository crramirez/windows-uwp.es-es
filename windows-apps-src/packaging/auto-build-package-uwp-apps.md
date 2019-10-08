---
title: Configurar compilaciones automatizadas para la aplicación para UWP
description: Cómo configurar las compilaciones automatizadas para producir paquetes de instalaciones de prueba o paquetes de la Store.
ms.date: 07/17/2019
ms.topic: article
keywords: windows 10, uwp
ms.assetid: f9b0d6bd-af12-4237-bc66-0c218859d2fd
ms.localizationpriority: medium
ms.openlocfilehash: b7d38464a26af0df03c1aa381b16fbddf1de55cc
ms.sourcegitcommit: e0644abf76a2535ea24758d1904ff00dfcd86a51
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/07/2019
ms.locfileid: "72008044"
---
# <a name="set-up-automated-builds-for-your-uwp-app"></a>Configurar compilaciones automatizadas para la aplicación para UWP

Puede usar Azure Pipelines para crear compilaciones automatizadas para proyectos de UWP. En este artículo, veremos diferentes maneras de hacerlo. También le mostraremos cómo realizar estas tareas mediante la línea de comandos para que pueda integrar con cualquier otro sistema de compilación.

## <a name="create-a-new-azure-pipeline"></a>Creación de una nueva canalización de Azure

Empiece por [registrarse en Azure pipelines](https://docs.microsoft.com/azure/devops/pipelines/get-started/pipelines-sign-up) si aún no lo ha hecho.

A continuación, cree una canalización que pueda usar para compilar el código fuente. Para ver un tutorial sobre la creación de una canalización para compilar un repositorio de GitHub, consulte [creación de la primera canalización](https://docs.microsoft.com/azure/devops/pipelines/get-started-yaml). Azure Pipelines admite los tipos de repositorios que se enumeran [en este artículo](https://docs.microsoft.com/azure/devops/pipelines/repos).

## <a name="set-up-an-automated-build"></a>Configurar una compilación automatizada

Comenzaremos con la definición de compilación predeterminada de UWP que está disponible en las operaciones de desarrollo de Azure y, después, le mostraremos cómo configurar la canalización.

En la lista de plantillas de definición de compilación, elige la plantilla **Plataforma universal de Windows**.

![Selección de la plantilla UWP](images/select-yaml-template.png)

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

La plantilla predeterminada intenta firmar el paquete con el certificado especificado en el archivo. csproj. Si desea firmar el paquete durante la compilación, debe tener acceso a la clave privada. De lo contrario, puede deshabilitar la firma agregando el parámetro `/p:AppxPackageSigningEnabled=false` a la sección `msbuildArgs` del archivo YAML.

## <a name="add-your-project-certificate-to-the-secure-files-library"></a>Agregar el certificado de proyecto a la biblioteca de archivos seguros

Debe evitar el envío de certificados al repositorio si es posible, y git los omite de forma predeterminada. Para administrar el control seguro de archivos confidenciales como los certificados, Azure DevOps admite la característica de [archivos seguros](https://docs.microsoft.com/azure/devops/pipelines/library/secure-files?view=azure-devops) .

Para cargar un certificado para la compilación automatizada:

1. En Azure Pipelines, expanda **canalizaciones** en el panel de navegación y haga clic en **biblioteca**.
2. Haga clic en la pestaña **archivos seguros** y, a continuación, haga clic en **+ archivo seguro**.

    ![Cómo cargar un archivo seguro](images/secure-file1.png)

3. Busque el archivo de certificado y haga clic en **Aceptar**.
4. Después de cargar el certificado, selecciónelo para ver sus propiedades. En **permisos de canalización**, active la alternancia **autorizar para usar en todas las canalizaciones** .

    ![Cómo cargar un archivo seguro](images/secure-file2.png)

5. Si la clave privada del certificado tiene una contraseña, se recomienda que almacene la contraseña en [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates) y, a continuación, vincule la contraseña a un [grupo de variables](https://docs.microsoft.com/azure/devops/pipelines/library/variable-groups). Puede usar la variable para tener acceso a la contraseña desde la canalización. Tenga en cuenta que solo se admite una contraseña para la clave privada; Actualmente no se admite el uso de un archivo de certificado protegido por contraseña.

> [!NOTE]
> A partir de Visual Studio 2019, ya no se genera un certificado temporal en los proyectos de UWP. Para crear o exportar certificados, use los cmdlets de PowerShell que se describen en [este artículo](/windows/msix/package/create-certificate-package-signing).

## <a name="configure-the-build-solution-build-task"></a>Configurar la tarea de compilación de compilación de soluciones

Esta tarea compila cualquier solución que se encuentra en la carpeta de trabajo en archivos binarios y genera el archivo de paquete de aplicación de salida. Esta tarea usa argumentos de MSBuild. Tendrás que especificar el valor de los argumentos. Usa la siguiente tabla como guía.

|**Argumento de MSBuild**|**Valor**|**Descripción**|
|--------------------|---------|---------------|
| AppxPackageDir | $(Build.ArtifactStagingDirectory)\AppxPackages | Define la carpeta en la que almacenar los artefactos generados. |
| AppxBundlePlatforms | $(Build.BuildPlatform) | Permite definir las plataformas que se van a incluir en la agrupación. |
| AppxBundle | Siempre | Crea un archivo. msixbundle/. appxbundle con los archivos. msix/. appx para la plataforma especificada. |
| UapAppxPackageBuildMode | StoreUpload | Genera el archivo. msixupload/. appxupload y la carpeta **_Test** para la instalación de prueba. |
| UapAppxPackageBuildMode | CI | Genera el archivo. msixupload/. appxupload únicamente. |
| UapAppxPackageBuildMode | SideloadOnly | Genera la carpeta **_Test** solo para la instalación de prueba. |
| AppxPackageSigningEnabled | true | Habilita la firma de paquetes. |
| PackageCertificateThumbprint | Huella digital del certificado | Este valor **debe** coincidir con la huella digital del certificado de firma o ser una cadena vacía. |
| PackageCertificateKeyFile | Path | Ruta de acceso al certificado que se va a usar. Esto se recupera de los metadatos de archivo seguros. |
| PackageCertificatePassword | Contraseña | La contraseña de la clave privada en el certificado. Se recomienda que almacene la contraseña en [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates) y vincule la contraseña al [grupo de variables](https://docs.microsoft.com/azure/devops/pipelines/library/variable-groups). Puede pasar la variable a este argumento. |

### <a name="configure-the-build"></a>Configurar la compilación

Si desea compilar la solución mediante la línea de comandos o con cualquier otro sistema de compilación, ejecute MSBuild con estos argumentos.

```powershell
/p:AppxPackageDir="$(Build.ArtifactStagingDirectory)\AppxPackages\\"
/p:UapAppxPackageBuildMode=StoreUpload
/p:AppxBundlePlatforms="$(Build.BuildPlatform)"
/p:AppxBundle=Always
```

### <a name="configure-package-signing"></a>Configurar la firma de paquetes

Para firmar el paquete MSIX (o APPX), la canalización debe recuperar el certificado de firma. Para ello, agregue una tarea DownloadSecureFile antes de la tarea VSBuild.
Esto le proporcionará acceso al certificado de firma a través de ```signingCert```.

```yml
- task: DownloadSecureFile@1
  name: signingCert
  displayName: 'Download CA certificate'
  inputs:
    secureFile: '[Your_Pfx].pfx'
```

A continuación, actualice la tarea VSBuild para que haga referencia al certificado de firma:

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
> El argumento PackageCertificateThumbprint se establece intencionadamente en una cadena vacía como precaución. Si la huella digital está establecida en el proyecto pero no coincide con el certificado de firma, se producirá un error en la compilación: `Certificate does not match supplied signing thumbprint`.

### <a name="review-parameters"></a>Revisar parámetros

Los parámetros definidos con la sintaxis `$()` son variables definidas en la definición de compilación y cambiarán en otros sistemas de compilación.

![variables predeterminadas](images/building-screen5.png)

Para ver todas las variables predefinidas, vea [variables de compilación predefinidas](https://docs.microsoft.com/azure/devops/pipelines/build/variables).

## <a name="configure-the-publish-build-artifacts-task"></a>Configurar la tarea publicar artefactos de compilación

La canalización de UWP predeterminada no guarda los artefactos generados. Para agregar las capacidades de publicación a la definición de YAML, agregue las siguientes tareas.

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

Puede ver los artefactos generados en la opción **artefactos** de la página resultados de la compilación.

![artefactos](images/building-screen6.png)

Dado que hemos establecido el argumento `UapAppxPackageBuildMode` en `StoreUpload`, la carpeta artefactos incluye el paquete para enviarlo al almacén (. msixupload/. appxupload). Tenga en cuenta que también puede enviar un paquete de aplicación normal (. msix/. appx) o un lote de aplicaciones (. msixbundle/. appxbundle/) al almacén. Para este artículo, usaremos el archivo .appxupload.

## <a name="address-bundle-errors"></a>Errores de agrupación de direcciones

Si agrega más de un proyecto de UWP a la solución y, a continuación, intenta crear un paquete, es posible que reciba un error similar al siguiente.

  `MakeAppx(0,0): Error : Error info: error 80080204: The package with file name "AppOne.UnitTests_0.1.2595.0_x86.appx" and package full name "8ef641d1-4557-4e33-957f-6895b122f1e6_0.1.2595.0_x86__scrj5wvaadcy6" is not valid in the bundle because it has a different package family name than other packages in the bundle`

Este error aparece porque en el nivel de la solución, no está claro qué aplicación debería aparecer en el lote. Para resolver este problema, abra cada archivo de proyecto y agregue las propiedades siguientes al final del primer elemento `<PropertyGroup>`.

|**Proyecto**|**Propiedades**|
|-------|----------|
|Aplicación|`<AppxBundle>Always</AppxBundle>`|
|UnitTests|`<AppxBundle>Never</AppxBundle>`|

A continuación, quite el argumento de MSBuild `AppxBundle` del paso de compilación.

## <a name="related-topics"></a>Temas relacionados

- [Compilar la aplicación .NET para Windows](https://docs.microsoft.com/vsts/build-release/get-started/dot-net)
- [Empaquetar aplicaciones para UWP](/windows/msix/package/packaging-uwp-apps)
- [Transferir localmente aplicaciones de LOB en Windows 10](https://docs.microsoft.com/windows/deploy/sideload-apps-in-windows-10)
- [Crear un certificado para la firma de paquetes](/windows/msix/package/create-certificate-package-signing)
