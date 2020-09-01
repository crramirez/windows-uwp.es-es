---
title: Uso de la herramienta winget para instalar y administrar aplicaciones
description: La herramienta de línea de comandos winget permite a los desarrolladores detectar, instalar, actualizar, quitar y configurar aplicaciones en equipos con Windows 10.
ms.date: 04/28/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: 4c918dccb2873f47a16669c195c47180e2129476
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168749"
---
# <a name="use-the-winget-tool-to-install-and-manage-applications"></a>Uso de la herramienta winget para instalar y administrar aplicaciones

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

La herramienta de línea de comandos **winget** permite a los desarrolladores detectar, instalar, actualizar, quitar y configurar aplicaciones en equipos con Windows 10. Esta herramienta es la interfaz cliente para el servicio del Administrador de paquetes de Windows.

Actualmente, la herramienta **winget** se encuentra en versión preliminar, por lo que no todas las funcionalidades planeadas están disponibles en este momento.

## <a name="install-winget"></a>Instalación de winget

Hay varias maneras de instalar la herramienta **winget**:

* La herramienta **winget** se incluye en el piloto o versión preliminar del [Instalador de aplicación de Microsoft](https://www.microsoft.com/p/app-installer/9nblggh4nns1?ocid=9nblggh4nns1_ORSEARCH_Bing&rtc=1&activetab=pivot:overviewtab). Tienes que instalar la versión preliminar del **Instalador de aplicación** para usar **winget**. Para acceder temprano, envía la solicitud al [Programa Insider del Administrador de paquetes de Windows](https://aka.ms/AppInstaller_InsiderProgram). Participar en el anillo piloto garantizará que veas las actualizaciones más recientes de la versión preliminar.

* Participa en el [anillo piloto de Windows Insider](https://insider.windows.com).

* Instala el paquete del Instalador de aplicación del escritorio de Windows, que se encuentra en la carpeta de versión del [repositorio de winget](https://github.com/microsoft/winget-cli).

> [!NOTE]
> La herramienta **winget** requiere Windows 10, versión 1709 (10.0.16299) o una versión posterior de Windows 10.

## <a name="administrator-considerations"></a>Consideraciones para administradores

El comportamiento del instalador puede variar en función de si ejecutas **winget** con privilegios de administrador.

* Al ejecutar **winget** sin privilegios de administrador, es posible que algunas aplicaciones [requieran una elevación de privilegios](https://docs.microsoft.com/windows/security/identity-protection/user-account-control/) para instalarse. Cuando se ejecute el instalador, Windows te pedirá que [eleves los privilegios](https://docs.microsoft.com/windows/security/identity-protection/user-account-control). Si decides no elevar los privilegios, la aplicación no se instalará.  

* Al ejecutar **winget** en un símbolo del sistema de administrador, no verás [peticiones de elevación](/windows/security/identity-protection/user-account-control/how-user-account-control-works) si la aplicación lo requiere. Siempre debes tener cuidado al ejecutar el símbolo del sistema como administrador y solo instalar las aplicaciones en las que confíes.

## <a name="use-winget"></a>Uso de winget

Después de instalar **Instalador de aplicación**, puedes ejecutar **winget** si escribes "winget" desde un símbolo del sistema.

Uno de los escenarios de uso más comunes es buscar e instalar una herramienta favorita.

1. Para [buscar](search.md) una herramienta, escribe `winget search \<appname>`.
2. Una vez que hayas confirmado que la herramienta que quieres está disponible, puedes [instalar](install.md) la herramienta escribiendo `winget install \<appname>`. La herramienta **winget** iniciará el instalador e instalará la aplicación en tu PC.
    ![Línea de comandos de winget](images\install.png)

3. Además de instalar y buscar, **winget** ofrece una serie de comandos que te permiten [mostrar los detalles](show.md) de las aplicaciones, [cambiar de orígenes](source.md) y [validar los paquetes](validate.md). Para obtener una lista completa de comandos, escribe: `winget --help`.
    ![Ayuda de winget](images\help.png)

### <a name="commands"></a>Comandos

La versión preliminar actual de la herramienta **winget** admite los siguientes comandos.

| Comando | Descripción |
|---------|-------------|
| [hash](hash.md) | Genera el hash SHA256 para el instalador. |
| [help](help.md) | Muestra ayuda para los comandos de la herramienta **winget**. |
| [install](install.md) | Instala la aplicación especificada. |
| [search](search.md) | Busca una aplicación. |
| [show](show.md) | Muestra los detalles de la aplicación especificada. |
| [source](source.md) | Agrega, quita y actualiza los repositorios del Administrador de paquetes de Windows a los que accede la herramienta **winget**. |
| [validate](validate.md) | Valida un archivo de manifiesto para enviarlo al repositorio del Administrador de paquetes de Windows. |

### <a name="options"></a>Opciones

La versión preliminar actual de la herramienta **winget** admite las siguientes opciones.

| Opción | Descripción |
|--------------|-------------|
| **-v,--version** | Esta opción devuelve la versión actual de winget. |
| **--info** |  Te brinda toda la información detallada sobre winget, incluidos los vínculos a la licencia y la declaración de privacidad. |
| **-?, --help** |  Busca ayuda adicional sobre winget. |

## <a name="supported-installer-formats"></a>Formatos de instalador admitidos

La versión preliminar actual de la herramienta **winget** admite los siguientes tipos de instaladores.

* EXE
* MSIX
* MSI

## <a name="scripting-winget"></a>Scripting en winget

Puedes crear scripts por lotes y scripts de PowerShell para instalar varias aplicaciones.

``` CMD
@echo off  
Echo Install Powertoys and Terminal  
REM Powertoys  
winget install Microsoft.Powertoys  
if %ERRORLEVEL% EQU 0 Echo Powertoys installed successfully.  
REM Terminal  
winget install Microsoft.WindowsTerminal  
if %ERRORLEVEL% EQU 0 Echo Terminal installed successfully.   %ERRORLEVEL%
```

> [!NOTE]
> Al crear el script, **winget** iniciará las aplicaciones en el orden especificado. Cuando un instalador devuelva un estado correcto o de error, **winget** iniciará el siguiente instalador. Si un instalador inicia otro proceso, es posible que vuelva a **winget** de forma prematura. Esto hará que **winget** instale el siguiente instalador antes de que se haya completado el instalador anterior.

## <a name="missing-tools"></a>Herramientas que faltan

Si el [repositorio de la comunidad](../package/repository.md) no incluye tu herramienta o aplicación, envía un paquete a nuestro [repositorio](https://github.com/microsoft/winget-pkgs). Al agregar tu herramienta favorita, estará disponible para ti y para todos los demás usuarios.

## <a name="open-source-details"></a>Detalles del código abierto

La herramienta **winget** es software de código abierto disponible en GitHub en el repositorio [https://github.com/microsoft/winget-cli/](https://github.com/microsoft/winget-cli/). El origen para compilar el cliente se encuentra en la [carpeta src](https://github.com/microsoft/winget-cli/tree/master/src).

El origen de **winget** está contenido en una solución de C++ en Visual Studio 2019. Para compilar la solución correctamente, instala la versión más reciente de [Visual Studio con la carga de trabajo de C++](https://visualstudio.microsoft.com/downloads/).

Te recomendamos que contribuyas al origen de **winget** en GitHub. Primero debes aceptar y firmar el Contrato de licencia de colaborador (CLA) de Microsoft.