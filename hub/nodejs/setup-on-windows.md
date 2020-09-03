---
title: Configuración de NodeJS en Windows nativo
description: Una guía que facilita la configuración del entorno de desarrollo de Node.js directamente en Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Node.js, windows 10, native windows, directly on windows
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 8c865610ba2678c1c5ab1b25ff7a2c7410d11f15
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166589"
---
# <a name="set-up-your-nodejs-development-environment-directly-on-windows"></a>Configuración del entorno de desarrollo de Node.js directamente en Windows

A continuación, se ofrece una guía detallada para empezar a usar Node.js en un entorno de desarrollo de Windows nativo.

## <a name="install-nvm-windows-nodejs-and-npm"></a>Instalación de nvm-windows, node.js y npm

Hay varias maneras de instalar Node.js. Se recomienda usar un administrador de versiones, ya que las versiones cambian con mucha rapidez. Probablemente tendrás que cambiar entre varias versiones en función de las necesidades de los distintos proyectos en los que estás trabajando. El administrador de versiones de Node, más comúnmente denominado nvm, es la forma más habitual de instalar varias versiones de Node.js, pero solo está disponible para Mac/Linux y no se admite en Windows. En su lugar, te guiaremos por los pasos necesarios para instalar nvm-windows y luego usarlo para instalar Node.js y el administrador de paquetes de Node (NPM). Hay [administradores de versiones alternativos](#alternative-version-managers) para tener en cuenta, que también se tratan en la sección siguiente.

> [!IMPORTANT]
> Siempre se recomienda quitar cualquier instalación existente de Node.js o npm del sistema operativo antes de instalar un administrador de versiones, ya que los distintos tipos de instalación pueden provocar conflictos extraños y confusos. Esto incluye la eliminación de los directorios de instalación nodejs existentes (por ejemplo, "C:\Archivos de programa\nodejs") que puedan permanecer. El elemento symlink generado de NVM no sobrescribirá un directorio de instalación existente (ni siquiera vacío). Para obtener ayuda con la eliminación de instalaciones anteriores, consulta [Cómo eliminar node.js completamente de Windows](https://stackoverflow.com/questions/20711240/how-to-completely-remove-node-js-from-windows).

1. Abre el [repositorio windows-nvm](https://github.com/coreybutler/nvm-windows#node-version-manager-nvm-for-windows) en el explorador de Internet y selecciona el vínculo **Descargar ahora**.
2. Descarga el archivo **nvm-setup.zip** de la versión más reciente.
3. Una vez descargado, abre el archivo ZIP y el archivo **nvm-setup.exe**.
4. El Asistente para instalación de Setup-NVM-for-Windows te guiará por los pasos de configuración, como la elección del directorio donde se instalarán nvm-windows y Node.js.

    ![Asistente para instalación de NVM para Windows](../images/install-nvm-for-windows-wizard.png)

5. Cuando se complete la instalación. Abre PowerShell e intenta usar windows-nvm para mostrar qué versiones de Node están instaladas actualmente (en este momento no debe haber ninguna): `nvm ls`

    ![Lista de NVM que no muestra ninguna versión de Node](../images/windows-nvm-powershell-no-node.png)

6. Instala la versión actual de Node.js (para probar las mejoras de las características más recientes, pero es más probable que tengas más problemas que con la versión de LTS): `nvm install latest`

7. Instala la versión de LTS estable más reciente de Node.js (recomendada); para ello, primero debes buscar el número de versión de LTS actual con: `nvm list available` y, a continuación, instalar el número de versión de LTS con: `nvm install <version>` (reemplazando `<version>` por el número, es decir: `nvm install 12.14.0`).

    ![Lista de las versiones disponibles de NVM](../images/windows-nvm-list.png)

8. Enumera qué versiones de Node están instaladas: `nvm ls`... ahora deberías ver las dos versiones que acabas de instalar.

    ![Lista de NVM que muestra las versiones de Node instaladas](../images/windows-nvm-node-installs.png)

9. Después de instalar los números de versión de Node.js necesarios, selecciona la versión que quieras usar. Para ello, escribe `nvm use <version>` (reemplaza `<version>` por el número, es decir, `nvm use 12.9.0`).

10. Para cambiar la versión de Node.js que deseas usar para un proyecto, crea un directorio de proyecto `mkdir NodeTest`, escribe el directorio `cd NodeTest` y, a continuación, escribe `nvm use <version>`, reemplazando `<version>` por el número de versión que te gustaría usar (por ejemplo, v10.16.3).

11. Comprueba qué versión de npm está instalada con: `npm --version`; este número de versión cambiará automáticamente a cualquier versión de npm que esté asociada a la versión actual de Node.js.

## <a name="alternative-version-managers"></a>Administradores de versiones alternativos

Aunque windows-nvm es actualmente el administrador de versiones más popular para Node, existen alternativas que se deben tener en cuenta:

- [nvs](https://github.com/jasongin/nvs) (conmutador de versiones de Node) es una alternativa `nvm` multiplataforma con la capacidad de [integrarse con VS Code](https://github.com/jasongin/nvs/blob/master/doc/VSCODE.md).

- [Volta](https://github.com/volta-cli/volta#installing-volta) es un nuevo administrador de versiones del equipo de LinkedIn que notifica la velocidad mejorada y la compatibilidad multiplataforma.

Para instalar Volta como administrador de versiones (en lugar de windows-nvm), consulta la sección **Instalación de Windows** de su [Guía de introducción](https://docs.volta.sh/guide/getting-started); después, descarga y ejecuta Windows Installer siguiendo las instrucciones de configuración.

> [!IMPORTANT]
> Debes asegurarte de que el [modo de desarrollador](/windows/uwp/get-started/enable-your-device-for-development#accessing-settings-for-developers) está habilitado en el equipo Windows antes de instalar Volta.

Para más información sobre el uso de Volta para instalar varias versiones de Node.js en Windows, consulta los [documentos de Volta](https://docs.volta.sh/guide/understanding#managing-your-toolchain).

## <a name="install-your-favorite-code-editor"></a>Instalación de un editor de código favorito

Se recomienda [instalar VS Code](https://code.visualstudio.com), además del [paquete de extensiones de Node.js](https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack), para desarrollar con Node.js en Windows. Instálalos todos o selecciona y elige la opción que te resulte más útil.

Para instalar el paquete de extensiones de Node.js:

1. Abre la ventana **Extensiones** (Ctrl+Mayús+X) en VS Code.
2. En el cuadro de búsqueda de la parte superior de la ventana Extensiones, escribe: "Paquete de extensiones de Node" o el nombre de cualquier extensión que estés buscando.
3. Selecciona **Instalar**. Una vez instalada, la extensión aparecerá en la carpeta "Habilitado" de la ventana **Extensiones**. Puedes deshabilitar, desinstalar o configurar las opciones seleccionando el icono de engranaje situado junto a la descripción de la nueva extensión.

Algunas de las extensiones adicionales que puedes considerar son las siguientes:

- [Debugger for Chrome](https://code.visualstudio.com/blogs/2016/02/23/introducing-chrome-debugger-for-vs-code): una vez que hayas terminado de desarrollar en el lado servidor con Node.js, deberás desarrollar y probar el lado cliente. Esta extensión integra el editor de VS Code con el servicio de depuración del explorador Chrome, lo que permite que las operaciones sean un poco más eficaces.
- [Keymaps from other editors](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Downloads): estas extensiones pueden ayudarte a sentirte como en casa con tu entorno en caso de que realices la transición desde otro editor de texto (como Atom, Sublime, Vim, eMacs, Notepad++, etc.).
- [Settings Sync](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync): te permite sincronizar la configuración de VS Code entre diferentes instalaciones mediante GitHub. Si trabajas en diferentes máquinas, te ayuda a mantener el entorno coherente entre ellas.

## <a name="install-git-optional"></a>Instalar GIT (opcional)

Si planeas colaborar con otras personas u hospedar el proyecto en un sitio de código abierto (como GitHub), VS Code admite el [control de versiones con GIT](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support). La pestaña Control de código fuente de VS Code realiza un seguimiento de todos los cambios y tiene comandos GIT comunes (agregar, confirmar, enviar cambios e incorporar cambios) integrados directamente en la interfaz de usuario. Primero, debes instalar GIT para alimentar el panel de control de código fuente.

1. Descarga e instala GIT para Windows desde el [sitio web git-scm](https://git-scm.com/download/win).

2. Se incluye un asistente para instalación que te formulará una serie de preguntas sobre la configuración de la instalación de GIT. Te recomendamos que uses todas las opciones de configuración predeterminadas, a menos que tengas un motivo concreto para cambiar algo.

3. Si nunca has trabajado con GIT, las [guías de GitHub](https://guides.github.com/) pueden resultarte de ayuda para empezar.

4. Se recomienda agregar un [archivo .gitignore](https://help.github.com/en/articles/ignoring-files) a los proyectos de Node. Aquí tienes la [plantilla de gitignore predeterminada de GitHub para Node.js](https://github.com/github/gitignore/blob/master/Node.gitignore).

## <a name="use-windows-subsystem-for-linux-for-production"></a>Uso del Subsistema de Windows para Linux para producción

Usar Node.js directamente en Windows es ideal para aprender y experimentar con lo que puedes hacer. Una vez que estés preparado para compilar aplicaciones web listas para producción, que normalmente se implementan en un servidor basado en Linux, se recomienda usar el Subsistema de Windows para Linux versión 2 (WSL 2) para desarrollar aplicaciones web Node.js. Muchos marcos y paquetes de Node.js se crean con un entorno *nix en mente, y la mayoría de las aplicaciones Node.js se implementan en Linux, por lo que el desarrollo en WSL garantiza la coherencia entre los entornos de desarrollo y producción. Para configurar un entorno de desarrollo de WSL, consulta [Configurar el entorno de desarrollo de Node.js con WSL 2](./setup-on-wsl2.md).

> [!NOTE]
> Si te encuentras en una situación (poco frecuente) en la que necesitas hospedar una aplicación Node.js en un servidor Windows, el escenario más común parece ser [mediante un proxy inverso](https://medium.com/intrinsic/why-should-i-use-a-reverse-proxy-if-node-js-is-production-ready-5a079408b2ca). Existen dos formas de hacerlo: 1) [usar iisnode](https://harveywilliams.net/blog/installing-iisnode) o [directamente](https://dev.to/petereysermans/hosting-a-node-js-application-on-windows-with-iis-as-reverse-proxy-397b). No mantenemos estos recursos y recomendamos [usar servidores Linux para hospedar las aplicaciones Node.js](/azure/app-service/app-service-web-get-started-nodejs).