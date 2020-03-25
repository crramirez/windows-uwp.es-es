---
title: Ayuda para pasar de Mac (UNIX) a Windows
description: Una guía para ayudarle a realizar la transición de un equipo Mac (UNIX) a un entorno de desarrollo de Windows, incluida la asignación de teclas de método abreviado y una breve descripción de los conceptos que difieren entre Mac y Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.technology: windows-nodejs
keywords: Mac a Windows, asignación de teclas de método abreviado, migración de UNIX a Windows, transición de Mac a Windows, ayuda a pasar de MacBook a Surface, cómo usar Windows para un usuario de Macintosh, cambiar de Macintosh a Windows, ayuda para cambiar los entornos de desarrollo, Mac OS X a Windows, ayuda mover de Mac a PC
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 8c23fa3e6791a3cd78d259b40e68606a30fd9395
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/24/2020
ms.locfileid: "80218445"
---
# <a name="guide-for-changing-your-dev-environment-from-mac-to-windows"></a>Guía para cambiar el entorno de desarrollo de Mac a Windows

Los siguientes consejos y equivalentes de control deben ayudarle en la transición entre un entorno de desarrollo de Mac y Windows (o WSL/Linux).

Para el desarrollo de aplicaciones, el equivalente más cercano a Xcode sería [Visual Studio](https://visualstudio.microsoft.com). También hay una versión de [Visual Studio para Mac](https://visualstudio.microsoft.com/vs/mac/), si alguna vez se siente la necesidad de volver atrás. En la edición de código fuente multiplataforma (y un gran número de complementos) [Visual Studio Code](https://code.visualstudio.com/?wt.mc_id=DX_841432) es la opción más popular.

## <a name="keyboard-shortcuts"></a>Accesos directos del teclado

| **Sesión** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| Copiar | Comando + C | Ctrl+C |
| Cortar | Comando + X | Ctrl+X |
| Pegar | Comando + V | Ctrl+V |
| Deshacer | Comando + Z | Ctrl+Z |
| Guardar | Comando + S | Ctrl+S |
| Abrir | Comando + O | Ctrl+O |
| Bloquear equipo | Comando + control + Q | WindowsKey + L |
| Mostrar escritorio | Comando + F3 | WindowsKey + D |
| Abrir el explorador de archivos | Comando + N | WindowsKey + E |
| Minimizar ventanas | Comando + M | WindowsKey + M |
| Buscar | Comando + espacio | WindowsKey |
| Cerrar la ventana activa | Comando + W | Control + W |
| Cambiar tarea actual | Comando + Tab | Alt+Tab |
| Maximizar una ventana a pantalla completa | Control + Comando + F | WindowsKey + arriba |
| Guardar pantalla (captura de pantalla) | Comando + Mayús + 3 | WindowsKey + Mayús + S |
| Guardar ventana | Comando + Mayús + 4 | WindowsKey + Mayús + S |
| Ver información o propiedades del elemento | Comando + I | Alt+Entrar |
 | Seleccionar todos los elementos | Comando + A | Ctrl+A |
| Seleccionar más de un elemento en una lista (no contiguo) | Y, a continuación, haga clic en cada elemento | Control y, a continuación, haga clic en cada elemento |
| Escribir caracteres especiales | Opción + tecla de carácter | Alt + tecla de carácter|

## <a name="trackpad-shortcuts"></a>Accesos directos de panel táctil

Nota: algunos de estos métodos abreviados requieren un "menú de paneles de precisión", como el de los dispositivos de Surface y otros equipos portátiles de terceros.

 **Sesión** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| Scroll | Deslizamiento vertical con dos dedos | Deslizamiento vertical con dos dedos |
| Zoom | Dos dedos hacia dentro y fuera | Dos dedos hacia dentro y fuera |
| Deslizar hacia atrás y hacia delante entre las vistas | Deslizar el dedo lateral de dos dedos | Deslizar el dedo lateral de dos dedos |
| Cambiar áreas de trabajo virtuales | Cuatro dedos en el lado de la derecha | Cuatro dedos en el lado de la derecha |
| Mostrar aplicaciones abiertas actualmente | Cuatro dedos hacia arriba | Tres dedos hacia arriba |
| Cambiar entre aplicaciones | N/D | Deslizar rápidamente tres dedos de lado |
| Ir al escritorio | Distribuir cuatro dedos | Deslizar tres dedos hacia abajo |
| Abrir Cortana/centro de actividades | Diapositiva con dos dedos de la derecha | Pulsación de tres dedos |
| Abrir información adicional | Pulsación de tres dedos | N/D |
|Mostrar Launchpad/iniciar una aplicación | Pinch con cuatro dedos | Puntee con cuatro dedos |

Nota: las opciones de paneles de configuración se pueden configurar en ambas plataformas.

## <a name="terminal-and-shell"></a>Terminal y shell

Windows proporciona varias alternativas al emulador de terminal de Mac.

1. La línea de comandos de Windows

La línea de comandos de Windows aceptará comandos de DOS y es la herramienta de línea de comandos que se usa con más frecuencia en Windows. Para abrirlo: Presione **WindowsKey + R** para abrir el cuadro **Ejecutar** , escriba **cmd** y, a continuación, haga clic en **Aceptar**. Para abrir una línea de comandos de administrador, escriba **cmd** y, a continuación, presione **Ctrl + Mayús + entrar**.

2. PowerShell

[PowerShell](https://docs.microsoft.com/powershell/scripting/overview?view=powershell-6) es un "PowerShell es un shell de línea de comandos basado en tareas y un lenguaje de scripting integrado en .net. PowerShell ayuda a los administradores del sistema y a los usuarios avanzados a automatizar rápidamente las tareas que administran los sistemas operativos. En otras palabras, se trata de una línea de comandos muy eficaz y es especialmente querido por los administradores del sistema.

Casualmente, PowerShell [también está disponible para Mac](https://docs.microsoft.com/powershell/scripting/install/installing-powershell-core-on-macos?view=powershell-6).

3. Subsistema de Windows para Linux (WSL)

WSL permite ejecutar un shell de Linux en Windows. Esto significa que puede ejecutar *Bash** u otro Shell, en función de la elección y el distribución de Linux específico instalado. El uso de WSL proporcionará el tipo de entorno más conocido para los usuarios de Mac. Por ejemplo, **LS** para enumerar los archivos en un directorio actual, no en **dir** como lo haría con la línea de comandos de Windows. Para obtener información sobre la instalación y el uso de WSL, consulte la [Guía de instalación del subsistema de Windows para Linux para Windows 10](https://docs.microsoft.com/windows/wsl/install-win10).

4. Windows terminal (versión preliminar)

Windows terminal es una aplicación que combina herramientas de línea de comandos y shells de diversos orígenes, como la línea de comandos tradicional de Windows, PowerShell y el subsistema de Windows para Linux. Aunque actualmente se encuentra en versión preliminar, alreaedy contiene varias características útiles, como la compatibilidad con varias pestañas, paneles divididos, temas y estilos personalizados y soporte completo de Unicode. Windows terminal se puede instalar desde el [Microsoft Store en Windows 10](https://www.microsoft.com/en-us/p/windows-terminal-preview/9n0dx20hk701?activetab=pivot:overviewtab).

## <a name="apps-and-utilities"></a>Aplicaciones y utilidades

 **Aplicaciones** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| Configuración y preferencias | Preferencias del sistema | Configuración |
| Administrador de tareas | Monitor de actividad | Administrador de tareas |
| Formato de disco | Utilidad de disco | Administración de discos |
| Edición de texto | TextEdit | Bloc de notas |
| Visualización de eventos | Console | Visor de eventos |
| Buscar archivos y aplicaciones | Comando + espacio | Tecla Windows |
