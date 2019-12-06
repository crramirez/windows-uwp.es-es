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
ms.openlocfilehash: a4e71143730184db094df2a7e8f1416cbaf244c4
ms.sourcegitcommit: f5bb4e35d1373b982259e61547b3b1765da0e78c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/06/2019
ms.locfileid: "74881268"
---
# <a name="guide-for-changing-your-dev-environment-from-mac-to-windows"></a>Guía para cambiar el entorno de desarrollo de Mac a Windows

Los siguientes consejos y equivalentes de control deben ayudarle en la transición entre un entorno de desarrollo de Mac y Windows (o WSL/Linux).

Para el desarrollo de aplicaciones, el equivalente más cercano a Xcode sería [Visual Studio](https://visualstudio.microsoft.com). También hay una versión de [Visual Studio para Mac](https://visualstudio.microsoft.com/vs/mac/), si alguna vez se siente la necesidad de volver atrás. En la edición de código fuente multiplataforma (y un gran número de complementos) [Visual Studio Code](https://code.visualstudio.com/?wt.mc_id=DX_841432) es la opción más popular.

## <a name="keyboard-shortcuts"></a>Accesos rápidos de teclado

| **Operación** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| Copiar | Comando + C | Ctrl+C |
| Cortar | Comando + X | Ctrl+X |
| Pegar | Comando + V | Ctrl+V |
| Deshacer | Comando + Z | Ctrl+Z |
| Guardar | Comando + S | Ctrl+S |
| Abre | Comando + O | Ctrl+O |
| Bloquear equipo | Comando + control + Q | WindowsKey + L |
| Mostrar escritorio | Comando + F3 | WindowsKey + D |
| Minimizar ventanas | Comando + M | WindowsKey + M |
| Buscar | Comando + espacio | WindowsKey |
| Cerrar la ventana activa | Comando + W | Control + W |
| Cambiar tarea actual | Comando + Tab | Alt+Tab |
| Guardar pantalla (captura de pantalla) | Comando + Mayús + 3 | WindowsKey + Mayús + S |
| Guardar ventana | Comando + Mayús + 4 | WindowsKey + Mayús + S |
| Ver información o propiedades del elemento | Comando + I | Alt+ENTRAR |
 | Seleccionar todos los elementos | Comando + A | Ctrl+E |
| Seleccionar más de un elemento en una lista (no contiguo) | Y, a continuación, haga clic en cada elemento | Control y, a continuación, haga clic en cada elemento |
| Escribir caracteres especiales | Opción + tecla de carácter | Alt + tecla de carácter|

## <a name="trackpad-shortcuts"></a>Accesos directos de panel táctil

Nota: algunos de estos métodos abreviados requieren un "menú de paneles de precisión", como el de los dispositivos de Surface y otros equipos portátiles de terceros.

 **Operación** | **Mac** | **Windows** |
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

WSL permite ejecutar un shell de Linux en Windows. Esto significa que puede ejecutar *Bash** u otro Shell, en función de la elección y el distribución de Linux específico instalado. El uso de WSL proporcionará el tipo de entorno más conocido para los usuarios de Mac. Por ejemplo, **LS** para enumerar los archivos en un directorio actual, no en **dir** como lo haría con la línea de comandos de Windows. Para obtener información sobre cómo instalar y usar WSL, consulte la [Guía de instalación del subsistema de Windows para Linux para Windows 10](https://docs.microsoft.com/windows/wsl/install-win10).

## <a name="apps-and-utilities"></a>Aplicaciones y utilidades

 **Aplicación** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| Configuración y preferencias | Preferencias del sistema | Configuración |
| Administrador de tareas | Monitor de actividad | Administrador de tareas |
| Formato de disco | Utilidad de disco | Administración de discos |
| Edición de texto | TextEdit | Bloc de notas |
| Visualización de eventos | Console | Visor de eventos |
| Buscar archivos y aplicaciones | Comando + espacio | Tecla Windows |
