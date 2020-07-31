---
title: Ayuda para pasar de Mac (Unix) a Windows
description: Una guía para ayudarte a realizar la transición de un equipo Mac (Unix) a un entorno de desarrollo de Windows, incluida la asignación de teclas de método abreviado y una breve descripción de los conceptos que difieren entre Mac y Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.technology: windows-nodejs
keywords: Mac to Windows, shortcut key mapping, move from Unix to Windows, transition from Mac to Windows, help moving from MacBook to Surface, how to use Windows for a Macintosh user, switching from Macintosh to Windows, help changing dev environments, Mac OS X to Windows, help moving from Mac to PC
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: fa137ab51f0bb53e2907fa319d79ed77eb7ed655
ms.sourcegitcommit: 1e06168ada5ce6013b1d07c428548f084464a286
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/28/2020
ms.locfileid: "87363714"
---
# <a name="guide-for-changing-your-dev-environment-from-mac-to-windows"></a>Guía para cambiar el entorno de desarrollo de Mac a Windows

Los siguientes consejos y equivalentes de control deben ayudarte en la transición entre un entorno de desarrollo de Mac y Windows o WSL/Linux.

Para el desarrollo de aplicaciones, el equivalente más cercano a Xcode sería [Visual Studio](https://visualstudio.microsoft.com). También hay una versión de [Visual Studio para Mac](https://visualstudio.microsoft.com/vs/mac/), si alguna vez sientes la necesidad de volver atrás. Para la edición de código fuente multiplataforma y un gran número de complementos, [Visual Studio Code](https://code.visualstudio.com/?wt.mc_id=DX_841432) es la opción más popular.

## <a name="keyboard-shortcuts"></a>Métodos abreviados de teclado

| **Operación** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| Copiar | Comando+C | Ctrl+C |
| Cortar | Comando+X | Ctrl+X |
| Pegar | Comando+V | Ctrl+V |
| Deshacer | Comando+Z | Ctrl+Z |
| Guardar | Comando+S | Ctrl+S |
| Abrir | Comando+O | Ctrl+O |
| Bloquear el equipo | Comando+Control+Q | Tecla Windows+L |
| Mostrar escritorio | Comando+F3 | Tecla Windows+D |
| Abrir explorador de archivos | Comando+N | Tecla Windows+E |
| Minimizar ventanas | Comando+M | Tecla Windows+M |
| Buscar | Comando+Espacio | Tecla Windows |
| Cerrar la ventana activa | Comando+W | Control+W |
| Pasar a la tarea actual | Comando+Tabulador | Alt+Tabulador |
| Maximizar una ventana a pantalla completa | Control+Comando+F | Tecla Windows+Arriba |
| Guardar pantalla (captura de pantalla) | Comando+Mayús+3 | Tecla Windows+Mayús+S |
| Guardar ventana | Comando+Mayús+4 | Tecla Windows+Mayús+S |
| Ver información o propiedades del elemento | Comando+I | Alt+ENTRAR |
 | Seleccionar todos los elementos | Comando+A | Ctrl+A |
| Seleccionar más de un elemento en una lista (no contiguo) | Comando y, a continuación, clic en cada elemento | Control y, a continuación, clic en cada elemento |
| Escribir caracteres especiales | Opción+tecla de carácter | Alt+tecla de carácter|

## <a name="trackpad-shortcuts"></a>Accesos directos de panel táctil

Nota: Algunos de estos métodos abreviados requieren un "panel táctil de precisión", como el panel táctil de los dispositivos Surface y otros equipos portátiles de terceros.

 **Operación** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| Scroll | Deslizamiento vertical con dos dedos | Deslizamiento vertical con dos dedos |
| Zoom | Ampliar o reducir con dos dedos | Ampliar o reducir con dos dedos |
| Deslizar hacia atrás y hacia delante entre las vistas | Deslizar lateralmente con dos dedos | Deslizar lateralmente con dos dedos |
| Cambiar áreas de trabajo virtuales | Deslizar lateralmente con cuatro dedos | Deslizar lateralmente con cuatro dedos |
| Mostrar aplicaciones abiertas actualmente | Deslizar hacia arriba con cuatro dedos | Deslizar hacia arriba con tres dedos |
| Cambiar entre aplicaciones | N/A | Deslizar lentamente hacia los laterales con tres dedos |
| Ir al escritorio | Expandir con cuatro dedos | Deslizar hacia abajo con tres dedos |
| Abrir Cortana/centro de actividades | Deslizar desde la derecha con dos dedos | Pulsación de tres dedos |
| Abrir información adicional | Pulsación de tres dedos | N/A |
|Mostrar Launchpad/iniciar una aplicación | Acercar con cuatro dedos | Pulsar con cuatro dedos |

Nota: Las opciones de panel táctil se pueden configurar en ambas plataformas.

## <a name="command-line-shells-and-terminals"></a>Shells y terminales de línea de comandos

Windows admite varios shells y terminales de línea de comandos que a veces funcionan de manera ligeramente diferente en el shell BASH de Mac y en las aplicaciones de emulador de terminal, como Terminal e iTerm.

### <a name="windows-shells"></a>Shells de Windows

Windows tiene dos shells de línea de comandos principales:

1. **[PowerShell](https://docs.microsoft.com/powershell/scripting/overview?view=powershell-7)** : es un marco de administración de configuración y automatización de tareas entre plataformas, que consta de un shell de línea de comandos y un lenguaje de scripting integrado en .NET. Con PowerShell, los administradores, los desarrolladores y los usuarios avanzados pueden controlar y automatizar rápidamente tareas que administran procesos complejos y distintos aspectos del entorno y del sistema operativo en el que se ejecutan. PowerShell es [código completamente abierto](https://github.com/powershell/powershell) y, puesto que es multiplataforma, también [está disponible para Mac y Linux](https://docs.microsoft.com/powershell/scripting/install/installing-powershell?view=powershell-7).

    **Usuarios de shell BASH de Mac y Linux**: PowerShell también admite muchos alias de comandos con los que ya está familiarizado. Por ejemplo:
    - Mostrar el contenido del directorio actual con: `ls`
    - Mover archivos con: `mv`
    - Mover a un directorio nuevo con: `cd <path>`

    Algunos comandos y argumentos son diferentes en PowerShell con respecto a BASH. Para más información, escriba: [`get-help`](https://docs.microsoft.com/powershell/scripting/learn/ps101/02-help-system?view=powershell-7) en PowerShell o revise los [alias de compatibilidad](https://docs.microsoft.com/powershell/scripting/samples/appendix-1---compatibility-aliases?view=powershell-7) en los documentos.

    Para ejecutar PowerShell como administrador, escriba "PowerShell" en el menú Inicio de Windows y, a continuación, seleccione "Ejecutar como administrador".

2. **Línea de comandos de Windows (Cmd)** : Windows se sigue suministrando con el símbolo del sistema tradicional (y la consola, consulte más abajo), por lo que se proporciona compatibilidad con los archivos por lotes y los comandos compatibles con MS-DOS actuales y heredados. Cmd es útil cuando se ejecutan archivos por lotes u operaciones de línea de comandos anteriores o existentes; pero, en general, se recomienda a los usuarios que aprendan y usen PowerShell, ya que Cmd está ahora en mantenimiento y no se mejorará ni incluirá características nuevas en el futuro.

### <a name="linux-shells"></a>Shells de Linux

Ahora se puede instalar el Subsistema de Windows para Linux (WSL) para admitir la ejecución de un shell de Linux en Windows. Esto significa que puede ejecutar **bash**, con la distribución de Linux específica que elija, integrado directamente en Windows. El uso de WSL proporcionará el tipo de entorno más conocido para los usuarios de Mac. Por ejemplo, usará **ls** para enumerar los archivos de un directorio actual, no **dir** como lo haría en el shell Cmd de Windows tradicional. Para obtener información sobre la instalación y el uso de WSL, consulta la [Guía de instalación del Subsistema de Windows para Linux para Windows 10](https://docs.microsoft.com/windows/wsl/install-win10). Las distribuciones de Linux que se pueden instalar en Windows con WSL incluyen:

1. [Ubuntu 20.04 LTS](https://www.microsoft.com/store/apps/9n6svws3rx71)
2. [Kali Linux](https://www.microsoft.com/store/apps/9PKR34TNCV07)
3. [Debian GNU/Linux](https://www.microsoft.com/store/apps/9MSVKQC78PK6)
4. [OpenSUSE Leap 15.1](https://www.microsoft.com/store/apps/9NJFZK00FGKV)
5. [SUSE Linux Enterprise Server 15 SP1](https://www.microsoft.com/store/apps/9PN498VPMF3Z)

Por mencionar algunas. Puede encontrar más en los [documentos de instalación de WSL](https://docs.microsoft.com/windows/wsl/install-win10#install-your-linux-distribution-of-choice) e instalarlas directamente desde [Microsoft Store](https://www.microsoft.com/search/shop/apps?q=linux&category=Developer+tools).

## <a name="windows-terminals"></a>Terminales Windows

Además de muchas ofertas de terceros, Microsoft ofrece dos "terminales": aplicaciones GUI que proporcionan acceso a las aplicaciones y shells de línea de comandos.

1. **[Terminal Windows](https://docs.microsoft.com/windows/terminal/)** : Terminal Windows es una aplicación de terminal de línea de comandos nueva, moderna y muy configurable, que proporciona una experiencia de usuario de línea de comandos de baja latencia y rendimiento muy alto, varias pestañas, paneles de ventana divididos, temas y estilos personalizados, varios "perfiles" para diferentes shells o aplicaciones de línea de comandos, y muchas oportunidades para configurar y personalizar muchos aspectos de la experiencia del usuario de línea de comandos.

    Puede usar Terminal Windows para abrir pestañas conectadas a PowerShell, shells de WSL (como Ubuntu o Debian), el símbolo del sistema de Windows tradicional o cualquier otra aplicación de línea de comandos (por ejemplo, SSH, CLI de Azure, Git Bash).

2. **[Consola](https://docs.microsoft.com/windows/console/)** : En Mac y Linux, los usuarios suelen iniciar su aplicación de terminal preferida, que después crea y se conecta al shell predeterminado del usuario (por ejemplo, BASH).

    Sin embargo, debido a una peculiaridad del historial, los usuarios de Windows normalmente inician su shell y Windows se inicia automáticamente y conecta una aplicación de consola de GUI.

    Aunque todavía se pueden iniciar shells directamente y usar la consola de Windows heredada, se recomienda encarecidamente que los usuarios instalen y usen Terminal Windows para experimentar la mejor experiencia de línea de comandos más rápida y productiva.

## <a name="apps-and-utilities"></a>Aplicaciones y utilidades

 **Aplicación** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| Configuración y preferencias | Preferencias del sistema | Settings |
| Administrador de tareas | Monitor de actividad | Administrador de tareas |
| Formateo de disco | Utilidad de disco | Administración de discos |
| Edición de texto | TextEdit | Bloc de notas |
| Visualización de eventos | Consola | Visor de eventos |
| Buscar archivos/aplicaciones | Comando+Espacio | Tecla Windows |
