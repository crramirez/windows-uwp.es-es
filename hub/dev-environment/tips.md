---
title: Recomendaciones para mejorar el flujo de trabajo de desarrollo en Windows 10
description: Recomendaciones para mejorar el flujo de trabajo de desarrollo en Windows 10.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.technology: windows-nodejs
keywords: Microsoft, Windows, Developer, Tips, Performance, WSL
ms.localizationpriority: medium
ms.date: 07/24/2020
ms.openlocfilehash: 1135be4797893a74e398e69fcbc1c43d60e9fdb9
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172669"
---
# <a name="tips-for-improving-performance-and-development-workflows"></a>Recomendaciones para mejorar el rendimiento y el desarrollo de flujos de trabajo

Hemos recopilado algunas recomendaciones que esperamos que le ayuden para hacer que el flujo de trabajo sea más eficaz y agradable. ¿Tiene otras recomendaciones para compartir? Envíe una solicitud de incorporación de cambios con el botón "Editar" de arriba, o un problema con el botón "Comentarios" que aparece a continuación y podemos agregarlo a la lista.

## <a name="use-shortcuts-to-open-a-project-in-vs-code-or-windows-file-explorer"></a>Uso de accesos directos para abrir un proyecto en VS Code o en el explorador de archivos de Windows

Puede iniciar VS Code desde la línea de comandos en el proyecto que ha abierto con el comando: `code .` o abrir el directorio del proyecto desde la línea de comandos con el Explorador de archivos de Windows mediante `explorer.exe .` de Windows o la distribución de WSL. Es posible que tenga que agregar VS Code ejecutable a la variable de entorno PATH si esto no funciona de forma predeterminada. Obtenga más información sobre el [Inicio desde la línea de comandos](https://code.visualstudio.com/docs/editor/command-line#_launching-from-command-line).

![Captura de pantalla del Explorador de archivos de Windows](../images/wsl-file-explorer.png)

## <a name="use-the-credential-manager-to-your-streamline-authentication-process"></a>Uso del Administrador de credenciales en el proceso de autenticación optimizado

Si usa Git para el control de versiones y la colaboración, puede simplificar el proceso de autenticación mediante la [configuración del Administrador de credenciales de Git](/windows/wsl/tutorials/wsl-git#git-credential-manager-setup) para que almacene los tokens en el Administrador de credenciales de Windows. También se recomienda [agregar un archivo .gitignore](/windows/wsl/tutorials/wsl-git#adding-a-git-ignore-file) al proyecto.

## <a name="use-wsl-for-testing-your-production-pipeline-before-deploying-to-the-cloud"></a>Uso de WSL para probar la canalización de producción antes de la implementación en la nube

El Subsistema de Windows para Linux permite a los desarrolladores ejecutar un entorno de GNU/Linux, incluida la mayoría de herramientas de línea de comandos, utilidades y aplicaciones, directamente en Windows, sin modificar y sin la sobrecarga de una máquina virtual tradicional o una configuración de arranque dual.

WSL va destinado a personas desarrolladoras y está pensado para usarse como parte de un bucle de desarrollo interno. Supongamos que Sam crea una canalización de CI/CD (integración y entrega continuas) y quiere probarla primero en una máquina local (portátil) antes de implementarla en la nube. Sam puede habilitar WSL (y WSL 2 para mejorar la velocidad y el rendimiento) y, después, usar una instancia original de Ubuntu Linux (en el portátil) con el comando y la herramienta de Bash que prefiera. Después de verificar localmente la canalización del desarrollo, Sam puede insertar esa canalización de CI/CD en la nube. Para ello, debe convertirla en un contenedor de Docker e insertar dicho contenedor en una instancia en la nube, que se ejecute en una máquina virtual Ubuntu preparada para producción.

Para ver más maneras de usar WSL, consulte el [episodio sobre tabulaciones frente a espacios en WSL 2](https://channel9.msdn.com/Shows/Tabs-vs-Spaces/WSL2-Code-faster-on-the-Windows-Subsystem-for-Linux).

## <a name="improve-performance-speed-for-wsl-by-not-crossing-over-file-systems"></a>Mejora de la velocidad de rendimiento para WSL al no cruzar sistemas de archivos

Si está trabajando con Windows y el Subsistema de Windows para Linux, tiene dos sistemas de archivos instalados: NTSF (Windows) y WSL (su distribución de Linux). Para obtener un rendimiento rápido, asegúrese de que los archivos de proyecto se almacenan en el mismo sistema que las herramientas que está usando. Obtenga más información sobre la [elección del sistema de archivos correcto para un rendimiento más rápido](/windows/wsl/compare-versions#use-the-linux-file-system-for-faster-performance).

## <a name="improve-build-speeds-by-adding-windows-defender-exclusions"></a>Mejora de las velocidades de compilación mediante la adición de exclusiones de Windows Defender

Puede mejorar la velocidad de compilación actualizando la configuración de Windows Defender para agregar exclusiones de carpetas de proyecto o tipos de archivo con la confianza suficiente para poder excluirlos del examen de amenazas de seguridad. Obtenga más información sobre la [Actualización de la configuración de Windows Defender para mejorar el rendimiento](../android/defender-settings.md).

![Captura de pantalla de Windows Defender](../images/windows-defender-exclusions.png)

## <a name="launch-all-your-command-lines-in-windows-terminal-at-once"></a>Inicio de todas las líneas de comandos en Terminal Windows a la vez

* Puede iniciar varias líneas de comandos, como PowerShell, Ubuntu y la CLI de Azure, en una sola ventana con varios paneles mediante [Argumentos de la línea de comandos de Terminal Windows](/windows/terminal/command-line-arguments?tabs=powershell#multiple-panes). Después de instalar [Terminal Windows](/windows/terminal/get-started), [WSL/Ubuntu](/windows/wsl/install-win10) y la [CLI de Azure](/cli/azure/install-azure-cli?view=azure-cli-latest), escriba este comando en PowerShell para abrir una nueva ventana de varios paneles con los tres:

    ```powershell
    wt -p "Command Prompt" `; split-pane -p "Windows PowerShell" `; split-pane -H wsl.exe
    ```

## <a name="share-your-tips"></a>Uso compartido de recomendaciones

¿Tiene recomendaciones para ayudar a otros desarrolladores que usan Windows a mejorar su flujo de trabajo? [Envíe una solicitud de incorporación de cambios](https://github.com/MicrosoftDocs/windows-uwp/edit/docs/hub/dev-environment/overview.md) agregando la recomendación a la página o [registre un problema](https://github.com/MicrosoftDocs/windows-uwp/issues/new?title=&body=%0A%0A%5BEnter%20feedback%20here%5D%0A%0A%0A---%0A%23%23%23%23%20Document%20Details%0A%0A%E2%9A%A0%20*Do%20not%20edit%20this%20section.%20It%20is%20required%20for%20docs.microsoft.com%20%E2%9E%9F%20GitHub%20issue%20linking.*%0A%0A*%20ID%3A%207779352b-7b4e-dad8-7c1b-b9aba2c5e561%0A*%20Version%20Independent%20ID%3A%20a5b81b80-87a1-b6e2-8936-baf6c1a0b9c5%0A*%20Content%3A%20%5BSet%20up%20your%20Windows%2010%20development%20environment%5D(https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fwindows%2Fdev-environment%2Foverview)%0A*%20Content%20Source%3A%20%5Bhub%2Fdev-environment%2Foverview.md%5D(https%3A%2F%2Fgithub.com%2FMicrosoftDocs%2Fwindows-uwp%2Fblob%2Fdocs%2Fhub%2Fdev-environment%2Foverview.md)%0A*%20Product%3A%20**dev-environment**%0A*%20Technology%3A%20**windows-nodejs**) si lo quiere usar para agregar una recomendación a un tema determinado.

¿Tiene problemas relacionados con el rendimiento que le gustaría abordar? Regístrelo en el nuevo [repositorio de problemas de WinDev](https://github.com/microsoft/windev).

Gracias, desarrolladores. Estamos escuchando e intentando mejorar su experiencia.