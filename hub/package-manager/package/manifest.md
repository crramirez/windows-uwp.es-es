---
title: Creación de un manifiesto de paquete
description: ''
author: denelon
ms.author: denelon
ms.date: 04/29/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 054c8cf7fc104b78f0397f4d1536e1130668f4f8
ms.sourcegitcommit: 645cb099128072cfc905ec80b38bd280b98c9037
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/22/2020
ms.locfileid: "83825146"
---
# <a name="create-your-package-manifest"></a>Creación de un manifiesto de paquete

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

Si quieres enviar un paquete de software al [repositorio del Administrador de paquetes de Windows](repository.md), empieza por crear un manifiesto de paquete. El manifiesto es un archivo YAML que describe la aplicación que se va a instalar.

En este artículo se describe el contenido de un manifiesto de paquete para el Administrador de paquetes de Windows.

## <a name="yaml-basics"></a>Aspectos básicos de YAML

El formato YAML se eligió para los manifiestos de paquete debido a la facilidad de legibilidad humana y la coherencia con otras herramientas de desarrollo de Microsoft. Si no estás familiarizado con la sintaxis de YAML, puedes obtener información sobre los conceptos básicos en [Learn YAML in Y Minutes](https://learnxinyminutes.com/docs/yaml/).

> [!NOTE]
> Actualmente, los manifiestos para el Administrador de paquetes de Windows no admiten todas las características de YAML. Entre las características de YAML que no se admiten se incluyen delimitadores, claves complejas y conjuntos.

## <a name="conventions"></a>Convenciones

En este artículo se usan las convenciones siguientes:

* A la izquierda de `:` hay una palabra clave literal que se usa en las definiciones del manifiesto.
* A la derecha de `:` hay un tipo de datos. El tipo de datos puede ser de un tipo primitivo, como **string**, o una referencia a una estructura enriquecida definida en otra parte de este artículo.
* La notación `[` *datatype* `]` indica una matriz del tipo de datos mencionado. Por ejemplo, `[ string ]` es una matriz de cadenas.
* La notación `{` *datatype* `:` *datatype* `}` indica una asignación de un tipo de datos a otro. Por ejemplo, `{ string: string }` es una asignación de cadenas a cadenas.

## <a name="manifest-contents"></a>Contenido del manifiesto

Un manifiesto de paquete debe incluir un conjunto de elementos obligatorios y también puede incluir otros elementos opcionales que pueden ayudar a mejorar la experiencia del cliente al instalar el software. En esta sección se proporcionan breves resúmenes del esquema de manifiesto obligatorio y los esquemas de manifiesto completos, así como ejemplos de cada uno.

Cada campo del archivo de manifiesto debe usar mayúsculas y minúsculas como en Pascal y no se puede duplicar.

Para ver una lista completa y descripciones de los elementos de un manifiesto, consulta la especificación del manifiesto en el repositorio de [https://github.com/microsoft/winget-cli](https://github.com/microsoft/winget-cli).

### <a name="minimal-required-schema"></a>Esquema obligatorio mínimo

#### <a name="minimal-required-schema"></a>[Esquema obligatorio mínimo](#tab/minschema/)

```yaml
Id: string # publisher.package format
Publisher: string # the name of the publisher
Name: string # the name of the application
Version: string # version numbering format
License: string # the open source license or copyright
InstallerType: string # enumeration of supported installer types (exe, msi, msix, inno, wix, nullsoft, appx)
Installers:
  - Arch: string # enumeration of supported architectures
  - URL: string # path to download installation file
  - Sha256: string # SHA256 calculated from installer
ManifestVersion: 0.1.0
```

#### <a name="example"></a>[Ejemplo](#tab/minexample/)

```yaml
Id: microsoft.teams
Publisher: Microsoft Corporation
Name: Microsoft Teams
Version: 1.3.0.4461
License: Copyright (c) Microsoft Corporation. All rights reserved.
InstallerType: exe
Installers:
  - Arch: x64
    Url: https://statics.teams.cdn.office.net/production-windows-x64/1.3.00.4461/Teams_windows_x64.exe
    Sha256: 712f139d71e56bfb306e4a7b739b0e1109abb662dfa164192a5cfd6adb24a4e1
ManifestVersion: 0.1.0
```

* * *

### <a name="complete-schema"></a>Esquema completo

#### <a name="complete-schema"></a>[Esquema completo](#tab/compschema/)

```yaml
Id: string # publisher.package format
Publisher: string # the name of the publisher
Name: string # the name of the application
AppMoniker: string # the common name someone may use to search for the package
Version: string # version numbering format for package version
Channel: string # a string representing the flight ring
License: string # the open source license or copyright
LicenseUrl: string # valid secure URL to license
MinOSVersion: string # version numbering format for minimum version of Windows supported
Description: string # description of the package
Homepage: string # valid secure URL for the package
Tags: list # additional strings a user would use to search for the package
FileExtensions: list # list of file extensions the package could support
Protocols: list # list of protocols the package provides a handler for
Commands: list # list of commands or aliases the user would use to run the package
InstallerType: string # enumeration of supported installer types (exe, msi, msix)
Custom: string # custom switches passed to the installer
Silent: string # switches passed to the installer for silent installation
SilentWithProgress: string # switches passed to the installer for non-interactive install
Interactive: string # experimental
Language: string # experimental
Log: string # specifies log redirection switches and path
InstallLocation: string # specifies alternate location to install package
Installers: # nested map of keys for specific installer
  - Arch: string # enumeration of supported architectures
  - URL: string # path to download installation file
  - Sha256: string # SHA256 calculated from installer
  - SignatureSha256: string # SHA256 calculated from signature file's hash of MSIX file
  - Switches: # collection of entries to override root keys
  - Scope: string # experimental
  - SystemAppId: string # experimental
Localization: # nested map of keys for localization
  - Language: string # locale for display fields and localized URLs
ManifestVersion: string # version number format for manifest version
```

#### <a name="good-example"></a>[Ejemplo correcto](#tab/good/)

```yaml
Id: microsoft.teams
Publisher: Microsoft Corporation
Name: Microsoft Teams
Version: 1.3.0.4461
License: Copyright (c) Microsoft Corporation. All rights reserved.
LicenseUrl: https://docs.microsoft.com/en-us/MicrosoftTeams/assign-teams-licenses
InstallerType: exe
Installers:
  - Arch: x64
    Url: https://statics.teams.cdn.office.net/production-windows-x64/1.3.00.4461/Teams_windows_x64.exe
    Sha256: 712f139d71e56bfb306e4a7b739b0e1109abb662dfa164192a5cfd6adb24a4e1
ManifestVersion: 0.1.0
```

#### <a name="better-example"></a>[Ejemplo mejor](#tab/better/)

```yaml
Id: microsoft.teams
Publisher: Microsoft Corporation
Name: Microsoft Teams
Version: 1.3.0.4461
AppMoniker: teams
MinOSVersion: 10.0.0.0
Description: The hub for teamwork in Microsoft 365
Homepage: https://www.microsoft.com/microsoft/teams
License: Copyright (c) Microsoft Corporation. All rights reserved.
LicenseUrl: https://docs.microsoft.com/en-us/MicrosoftTeams/assign-teams-licenses
InstallerType: exe
Installers:
  - Arch: x64
    Url: https://statics.teams.cdn.office.net/production-windows-x64/1.3.00.4461/Teams_windows_x64.exe
    Sha256: 712f139d71e56bfb306e4a7b739b0e1109abb662dfa164192a5cfd6adb24a4e1
ManifestVersion: 0.1.0
```

* * *

## <a name="tips-and-best-practices"></a>Sugerencias y procedimientos recomendados

* Para ofrecer la mejor experiencia del cliente al buscar e instalar tu software, te recomendamos incluir la mayor cantidad posible de elementos opcionales más allá del esquema obligatorio. Por ejemplo, el campo `AppMoniker` es opcional. Sin embargo, si incluyes este campo, los clientes verán los resultados asociados con el valor de la `AppMoniker` al ejecutar el comando [search](../winget/search.md) (por ejemplo, **vscode** para **Visual Studio Code**). Si solo hay una aplicación con el valor `AppMoniker` especificado, los clientes podrán instalar la aplicación si escriben el moniker en lugar del identificador completo.
* `Id` debe ser único. No puedes hacer varios envíos con el mismo identificador de paquete. Evita los espacios, ya que esto obligará a los usuarios a colocar comillas alrededor de `Id` al usar el cliente de [winget](../index.md).
* Evite crear varias carpetas de editor. Por ejemplo, no crees "Contoso Ltd" si ya existe una carpeta "Contoso". Evita también los espacios al crear las carpetas.
* De ser posible, todos los paquetes se deben enviar con una instalación silenciosa. Si tienes un archivo ejecutable que no admite la instalación silenciosa, la experiencia del usuario se verá disminuida.
* Limita la longitud de las cadenas del manifiesto a 100 caracteres antes de un salto de línea.
* Cuando exista más de un tipo de instalador para la versión especificada del paquete, se puede colocar una instancia de `InstallerType` en cada una de las instancias de `Installers`.
