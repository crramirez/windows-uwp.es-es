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
ms.openlocfilehash: 8c23fa3e6791a3cd78d259b40e68606a30fd9395
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "80218445"
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

## <a name="terminal-and-shell"></a>Terminal y shell

Windows proporciona varias alternativas al emulador de terminal de Mac.

1. Línea de comandos de Windows

La línea de comandos de Windows aceptará comandos DOS y es la herramienta de línea de comandos que se usa con más frecuencia en Windows. Para abrirla: Presiona **Tecla Windows+R** para abrir el cuadro **Ejecutar** y, a continuación, escribe **cmd** y haz clic en **Aceptar**. Para abrir una línea de comandos de administrador, escribe **cmd** y, a continuación, presiona **Ctrl+Mayús+ENTRAR**.

2. PowerShell

"[PowerShell](https://docs.microsoft.com/powershell/scripting/overview?view=powershell-6) es un shell de línea de comandos y lenguaje de scripting basado en tareas integrados en .NET. PowerShell ayuda a los administradores del sistema y a los usuarios avanzados a automatizar rápidamente las tareas que administran los sistemas operativos". En otras palabras, se trata de una línea de comandos muy eficaz y es especialmente popular entre los administradores del sistema.

Casualmente, PowerShell [también está disponible para Mac](https://docs.microsoft.com/powershell/scripting/install/installing-powershell-core-on-macos?view=powershell-6).

3. Subsistema de Windows para Linux (WSL)

WSL permite ejecutar un shell de Linux en Windows. Esto significa que puedes ejecutar *Bash** u otro shell, en función de la elección y de la distribución de Linux específica instalada. El uso de WSL proporcionará el tipo de entorno más conocido para los usuarios de Mac. Por ejemplo, usarás **ls** para enumerar los archivos de un directorio actual, no **dir** como lo harías con la línea de comandos de Windows. Para obtener información sobre la instalación y el uso de WSL, consulta la [Guía de instalación del Subsistema de Windows para Linux para Windows 10](https://docs.microsoft.com/windows/wsl/install-win10).

4. Terminal Windows (versión preliminar)

Terminal Windows es una aplicación que combina herramientas de línea de comandos y shells de diversos orígenes, como la línea de comandos tradicional de Windows, PowerShell y el Subsistema de Windows para Linux. Aunque actualmente se encuentra en versión preliminar, ya contiene varias características útiles, como la compatibilidad con varias pestañas, paneles divididos, temas y estilos personalizados y compatibilidad total con Unicode. Terminal Windows se puede instalar desde [Microsoft Store en Windows 10](https://www.microsoft.com/en-us/p/windows-terminal-preview/9n0dx20hk701?activetab=pivot:overviewtab).

## <a name="apps-and-utilities"></a>Aplicaciones y utilidades

 **Aplicación** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| Configuración y preferencias | Preferencias del sistema | Settings |
| Administrador de tareas | Monitor de actividad | Administrador de tareas |
| Formateo de disco | Utilidad de disco | Administración de discos |
| Edición de texto | TextEdit | Bloc de notas |
| Visualización de eventos | Consola | Visor de eventos |
| Buscar archivos/aplicaciones | Comando+Espacio | Tecla Windows |
