---
title: Configuración de NodeJS en ventanas nativas
description: Una guía para ayudarle a conseguir que su entorno de desarrollo de node. js esté configurado directamente en Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Node. js, Windows 10, Windows nativo, directamente en Windows
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 18a8d07f790c391a6e10577ff512347106e1cf21
ms.sourcegitcommit: 60d2d15dd0d365f82e4e90e4bc34b40cf5b4a247
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2019
ms.locfileid: "72517830"
---
# <a name="set-up-your-nodejs-development-environment-directly-on-windows"></a>Configurar el entorno de desarrollo de node. js directamente en Windows

A continuación se ofrece una guía paso a paso para empezar a usar node. js en un entorno de desarrollo de Windows nativo.

> [!NOTE]
> Aunque usar node. js en Windows es ciertamente una opción viable, generalmente se recomienda usar el subsistema de Windows para Linux (WSL) para desarrollar aplicaciones Web de node. js. Muchos marcos y paquetes de node. js se crean con un entorno de * Nix en mente y la mayoría de las aplicaciones de node. js se implementan en Linux, por lo que el desarrollo en WSL garantiza la coherencia entre los entornos de desarrollo y producción. Para configurar un entorno de desarrollo de WSL, consulte [configuración del entorno de desarrollo de node. js con WSL 2](./setup-on-wsl2.md).

## <a name="install-nvm-windows-nodejs-and-npm"></a>Instalación de NVM: Windows, node. js y NPM

Hay varias maneras de instalar node. js. Se recomienda el uso de un administrador de versiones a medida que las versiones cambian con mucha rapidez. Probablemente tendrá que cambiar entre varias versiones en función de las necesidades de los distintos proyectos en los que está trabajando. Node version Manager, más comúnmente denominado NVM, es la forma más habitual de instalar varias versiones de node. js, pero solo está disponible para Mac/Linux y no se admite en Windows. En su lugar, se le guiará por los pasos necesarios para instalar NVM-Windows y luego usarlo para instalar node. js y node Package Manager (NPM). Hay [administradores de versiones alternativos](#alternative-version-managers) que se deben tener en cuenta, tal y como se describe en la sección siguiente.

> [!IMPORTANT]
> Siempre se recomienda quitar cualquier instalación existente de node. js o NPM del sistema operativo antes de instalar un administrador de versiones, ya que los distintos tipos de instalación pueden provocar conflictos extraños y confusos. Esto incluye la eliminación de los directorios de instalación de NodeJS existentes (por ejemplo, "C:\Archivos de Files\nodejs") que puedan permanecer. El symlink generado de NVM no sobrescribirá un directorio de instalación existente (ni siquiera vacío). Para obtener ayuda con la eliminación de instalaciones anteriores, consulte [Cómo quitar completamente node. js de Windows](https://stackoverflow.com/questions/20711240/how-to-completely-remove-node-js-from-windows)).

1. Abra el [repositorio de Windows-NVM](https://github.com/coreybutler/nvm-windows#node-version-manager-nvm-for-windows) en el explorador de Internet y seleccione el vínculo **Descargar ahora** .
2. Descargue el archivo **NVM-Setup. zip** de la versión más reciente.
3. Una vez descargado, abra el archivo zip y, a continuación, abra el archivo **NVM-Setup. exe** .
4. El Asistente para la instalación de la instalación de NVM para Windows le guiará a través de los pasos de configuración, incluida la elección del directorio en el que se instalarán tanto NVM-Windows como node. js.

    ![Asistente para la instalación de NVM para Windows](../images/install-nvm-for-windows-wizard.png)

5. Una vez completada la instalación. Abra PowerShell e intente usar Windows-NVM para mostrar qué versiones de node están instaladas actualmente (no debe ser ninguna en este momento): `nvm ls`

    ![Lista de NVM que no muestra ninguna versión de nodo](../images/windows-nvm-powershell-no-node.png)

6. Instale la versión actual de node. js (para probar las mejoras de las características más recientes, pero es más probable que tenga problemas que la versión de LTS): `nvm install latest`
7. Instale la versión LTS estable más reciente de node. js (recomendado). para ello, primero debe buscar el número de versión LTS actual con: `nvm list available` y, a continuación, instalar el número de versión de LTS con: `nvm install <version>` (reemplazando `<version>` por el número, es decir, `nvm install 10.16.3`).

    ![Lista de las versiones disponibles de NVM](../images/windows-nvm-list.png)

8. Enumerar las versiones de node instaladas: `nvm ls`... ahora debería ver las dos versiones que acaba de instalar.

    ![Lista de NVM que muestra las versiones de nodo instaladas](../images/windows-nvm-node-installs.png)

9. Para comprobar qué versión de node. js es actualmente la predeterminada, escriba: `node --version`
10. Para cambiar la versión de node. js que desea usar para un proyecto, cree un nuevo directorio de proyecto `mkdir NodeTest` y escriba el directorio `cd NodeTest` y, a continuación, escriba `nvm use <version>` reemplazando `<version>` por el número de versión que desea usar (IE v 10.16.3 ').
11. Compruebe qué versión de NPM está instalada con: `npm --version`, este número de versión cambiará automáticamente a la versión de NPM que esté asociada a la versión actual de node. js.

## <a name="alternative-version-managers"></a>Administradores de versiones alternativos

Aunque Windows-NVM es actualmente el administrador de versiones más popular para node, existen alternativas a tener en cuenta:

- [NVS](https://github.com/jasongin/nvs) (conmutador de versión de nodo) es una alternativa multiplataforma `nvm` con la capacidad de [integrarse con vs Code](https://github.com/jasongin/nvs/blob/master/doc/VSCODE.md).
- 
- [Volta](https://github.com/volta-cli/volta#installing-volta) es un nuevo administrador de versiones del equipo de LinkedIn que notifica la velocidad mejorada y la compatibilidad entre plataformas.

Para instalar Volta como su administrador de versiones (en lugar de Windows-NVM), vaya a la sección **instalación de Windows** de su [Guía de introducción](https://docs.volta.sh/guide/getting-started)y, a continuación, descargue y ejecute su instalador de Windows siguiendo las instrucciones de configuración.

> [!IMPORTANT]
> Debe asegurarse de que el [modo de desarrollador](https://docs.microsoft.com/en-us/windows/uwp/get-started/enable-your-device-for-development#accessing-settings-for-developers) está habilitado en el equipo Windows antes de instalar Volta.

Para más información sobre el uso de Volta para instalar varias versiones de node. js en Windows, consulte los [documentos de Volta](https://docs.volta.sh/guide/understanding#managing-your-toolchain).

## <a name="install-your-favorite-code-editor"></a>Instalar su editor de código favorito

Se recomienda [instalar vs Code](https://code.visualstudio.com), así como el paquete de [extensión de node. js](https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack), para desarrollar con node. js en Windows. Instálelos todos o seleccione y elija lo que le parezca más útil.

Para instalar el paquete de extensión de node. js:

1. Abra la ventana **extensiones** (Ctrl + Mayús + X) en vs Code.
2. En el cuadro de búsqueda de la parte superior de la ventana extensiones, escriba: "node Extension Pack" (o el nombre de la extensión que esté buscando).
3. Seleccione **instalar**. Una vez instalado, la extensión aparecerá en la carpeta "habilitado" de la ventana **extensiones** . Puede deshabilitar, desinstalar o configurar las opciones seleccionando el icono de engranaje junto a la descripción de la nueva extensión.

Algunas de las extensiones adicionales que puede considerar son las siguientes:

- [Depurador para Chrome](https://code.visualstudio.com/blogs/2016/02/23/introducing-chrome-debugger-for-vs-code): una vez que haya terminado de desarrollar en el lado servidor con node. js, deberá desarrollar y probar el lado cliente. Esta extensión integra el editor de VS Code con el servicio de depuración del explorador Chrome, lo que permite que las cosas sean un poco más eficaces.
- [Keymaps de otros editores](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Downloads): estas extensiones pueden ayudar a su entorno a su gusto en casa si va a realizar la transición desde otro editor de texto (como Atom, sublime, Vim, Emacs, Notepad + +, etc.).
- [Sincronización de configuración](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync): le permite sincronizar la configuración de vs Code en distintas instalaciones mediante github. Si trabaja en diferentes equipos, esto ayuda a mantener el entorno coherente entre ellos.

## <a name="install-git-optional"></a>Instalación de Git (opcional)

Si planea colaborar con otras personas, o hospedar el proyecto en un sitio de código abierto (como GitHub), VS Code admite el [control de versiones con git](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support). La pestaña control de código fuente de VS Code realiza un seguimiento de todos los cambios y tiene comandos git comunes (agregar, confirmar, insertar, extraer) integrados en la interfaz de usuario. En primer lugar, debe instalar git para encender el panel de control de código fuente.

1. Descargue e instale git para Windows desde [el sitio web de Git-SCM](https://git-scm.com/download/win).

2. Se incluye un asistente de instalación que le preguntará una serie de preguntas sobre la configuración de la instalación de Git. Se recomienda usar todas las opciones de configuración predeterminadas, a menos que tenga una razón concreta para cambiar algo.

3. Si nunca ha trabajado antes con git, las [guías de github](https://guides.github.com/) pueden ayudarle a empezar.

4. Se recomienda agregar un [archivo. gitignore](https://help.github.com/en/articles/ignoring-files) a los proyectos de nodo. Esta es [la plantilla gitignore predeterminada de github para node. js](https://github.com/github/gitignore/blob/master/Node.gitignore).
