---
title: Configuración de NodeJS en WSL 2
description: Una guía que facilita la configuración del entorno de desarrollo de Node.js en el Subsistema de Windows para Linux.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, Node.js, windows 10, microsoft, learning nodejs, node on windows, node on wsl, node on linux on windows, install node on windows, nodejs with vs code, develop with node on windows, develop with nodejs on windows, install node on WSL, NodeJS on Windows Subsystem for Linux
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 1ea8973e1db665d1fe66ef6b5f5699319131d605
ms.sourcegitcommit: 2af814b7f94ee882f42fae8f61130b9cc9833256
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2020
ms.locfileid: "83717134"
---
# <a name="set-up-your-nodejs-development-environment-with-wsl-2"></a>Configuración del entorno de desarrollo de Node.js con WSL 2

A continuación, se presenta una guía detallada que facilita la configuración del entorno de desarrollo de Node.js con el Subsistema de Windows para Linux (WSL).

Se recomienda instalar y ejecutar la versión actualizada de WSL 2 para poder beneficiarse de las mejoras significativas en la velocidad del rendimiento y la compatibilidad con las llamadas del sistema, incluida la capacidad de ejecutar [Docker Desktop](https://docs.docker.com/docker-for-windows/wsl-tech-preview/#download). Muchos módulos y tutoriales de npm para el desarrollo web de Node.js se escriben para los usuarios de Linux y usan herramientas de instalación y empaquetado basadas en Linux. La mayoría de las aplicaciones web también se implementan en Linux, por lo que el uso de WSL 2 garantizará una coherencia entre los entornos de desarrollo y producción.

> [!NOTE]
> Si te comprometes a usar Node.js directamente en Windows o si planeas usar un entorno de producción de Windows Server, consulta nuestra guía para [configurar el entorno de desarrollo de Node.js directamente en Windows](./setup-on-windows.md).

## <a name="install-wsl-2"></a>Instalación de WSL 2

Para habilitar e instalar WSL 2, sigue los pasos que se describen en la [documentación de instalación de WSL](https://docs.microsoft.com/windows/wsl/install-win10). Estos pasos incluirán la elección de una distribución de Linux (por ejemplo, Ubuntu).

Una vez que hayas instalado WSL 2 y una distribución de Linux, abre la distribución de Linux (puedes encontrarla en el menú Inicio de Windows), y comprueba la versión y el nombre de código mediante el comando: `lsb_release -dc`.

Se recomienda actualizar la distribución de Linux con regularidad, incluso inmediatamente después de instalarla, para asegurarse de que tiene los paquetes más recientes. Windows no controla automáticamente esta actualización. Para actualizar la distribución, usa el comando: `sudo apt update && sudo apt upgrade`.  

## <a name="install-windows-terminal-optional"></a>Instalación de Terminal Windows (opcional)

El nuevo Terminal Windows permite habilitar varias pestañas (cambia rápidamente entre varias líneas de comando de Linux, el símbolo del sistema de Windows, PowerShell, la CLI de Azure, etc.), crear enlaces de teclado personalizados (teclas de método abreviado para abrir o cerrar pestañas, copiar y pegar, etc.), usar la característica de búsqueda y configurar temas personalizados (esquemas de colores, estilos y tamaños de fuente, imagen de fondo/desenfoque/transparencia). [Más información](https://docs.microsoft.com/windows/terminal).

1. Obtén [Terminal Windows (versión preliminar) en Microsoft Store](https://www.microsoft.com/store/apps/9n0dx20hk701): al instalar a través de Microsoft Store, las actualizaciones se controlan automáticamente.

2. Una vez instalado, abre Terminal Windows y selecciona **Configuración** para personalizar el terminal con el archivo `settings.json`.

    ![Configuración de Terminal Windows](../images/windows-terminal-settings.png)

## <a name="install-nvm-nodejs-and-npm"></a>Instalación de nvm, node.js y npm

Hay varias maneras de instalar Node.js. Se recomienda usar un administrador de versiones, ya que las versiones cambian con mucha rapidez. Probablemente tendrás que cambiar entre varias versiones en función de las necesidades de los distintos proyectos en los que estás trabajando. El administrador de versiones de Node, más comúnmente denominado nvm, es la forma más habitual de instalar varias versiones de Node.js. Te guiaremos por los pasos necesarios para instalar nvm y luego usarlo para instalar Node.js y el administrador de paquetes de Node (npm). Hay [administradores de versiones alternativos](#alternative-version-managers) para tener en cuenta, que también se tratan en la sección siguiente.

> [!IMPORTANT]
> Siempre se recomienda quitar cualquier instalación existente de Node.js o npm del sistema operativo antes de instalar un administrador de versiones, ya que los distintos tipos de instalación pueden provocar conflictos extraños y confusos. Por ejemplo, la versión de Node que se puede instalar con el comando `apt-get` de Ubuntu está obsoleta actualmente. Para obtener ayuda con la eliminación de instalaciones anteriores, consulta [Cómo eliminar nodejs de Ubuntu](https://askubuntu.com/questions/786015/how-to-remove-nodejs-from-ubuntu-16-04).

1. Abre la línea de comandos de Ubuntu 18.04.
2. Instala cURL (una herramienta que se usa para descargar contenido de Internet en la línea de comandos) con: `sudo apt-get install curl`
3. Instala nvm con: `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.2/install.sh | bash`
4. Para verificar la instalación, escribe: `command -v nvm`... esto debería devolver "nvm"; si recibes "No se encuentra el comando" o ninguna respuesta, cierra el terminal actual, vuelve a abrirlo e inténtalo de nuevo. [Obtén más información en el repositorio de GitHub de nvm](https://github.com/nvm-sh/nvm).
5. Enumera qué versiones de Node están instaladas actualmente (en este momento no debe haber ninguna): `nvm ls`

    ![Lista de NVM que no muestra ninguna versión de Node](../images/nvm-no-node.png)

6. Instala la versión actual de Node.js (para probar las mejoras de las características más recientes, pero es más probable que tengas problemas): `nvm install node`
7. Instala la última versión de LTS estable de Node.js (recomendada): `nvm install --lts`
8. Enumera qué versiones de Node están instaladas: `nvm ls`... ahora deberías ver las dos versiones que acabas de instalar.

    ![Lista de NVM en la que se enumeran las versiones de Node actuales y de LTS](../images/nvm-node-installed.png)

9. Comprueba que Node.js está instalado y la versión predeterminada actualmente con: `node --version`. Después, comprueba que también tienes npm, con: `npm --version` (También puedes usar `which node` o `which npm` para ver la ruta de acceso utilizada para las versiones predeterminadas).
10. Para cambiar la versión de Node.js que deseas usar para un proyecto, crea un directorio de proyecto `mkdir NodeTest`, escribe el directorio `cd NodeTest` y, a continuación, escribe `nvm use node`, para cambiar a la versión actual, o bien `nvm use --lts` para cambiar a la versión de LTS. También puedes usar el número específico de cualquier versión adicional que hayas instalado, como `nvm use v8.2.1`. (Para enumerar todas las versiones de Node.js disponibles, usa el comando: `nvm ls-remote`).

Si usas NVM para instalar Node.js y NPM, no es necesario usar el comando SUDO para instalar nuevos paquetes.

> [!NOTE]
> En el momento de la publicación, NVM v0.35.2 era la última versión disponible. Puedes consultar la [página del proyecto de GitHub para obtener la última versión de NVM](https://github.com/nvm-sh/nvm) y ajustar el comando anterior para incluir la versión más reciente.
La instalación de la versión más reciente de NVM con cURL reemplazará la anterior, lo que dejará intacta la versión de Node utilizada para instalar NVM. Por ejemplo: `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.36.0/install.sh | bash`

## <a name="alternative-version-managers"></a>Administradores de versiones alternativos

Aunque nvm es actualmente el administrador de versiones más popular para Node, existen algunas alternativas que se deben tener en cuenta:

- [n](https://www.npmjs.com/package/n#installation) es una alternativa a `nvm` de larga duración que consigue lo mismo con comandos ligeramente diferentes y se instala a través de `npm` en lugar de con un script de Bash.
- [fnm](https://github.com/Schniz/fnm#using-a-script) es un administrador de versiones más reciente, lo que exige que sea mucho más rápido que `nvm`. (También utiliza [Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/get-started/what-is-azure-pipelines?view=azure-devops)).
- [Volta](https://github.com/volta-cli/volta#installing-volta) es un nuevo administrador de versiones del equipo de LinkedIn que notifica la velocidad mejorada y la compatibilidad multiplataforma.
- [asdf-vm](https://asdf-vm.com/#/core-manage-asdf-vm) es una única CLI para varios lenguajes, como ike gvm, nvm, rbenv & pyenv y muchos más, todo en uno.
- [nvs](https://github.com/jasongin/nvs) (conmutador de versiones de Node) es una alternativa `nvm` multiplataforma con la capacidad de [integrarse con VS Code](https://github.com/jasongin/nvs/blob/master/doc/VSCODE.md).

## <a name="install-your-favorite-code-editor"></a>Instalación de un editor de código favorito

Se recomienda utilizar **Visual Studio Code** con la **extensión Remote-WSL** para los proyectos de Node.js. Esto divide VS Code en una arquitectura "cliente-servidor", con el cliente (la interfaz de usuario) que se ejecuta en el equipo Windows y el servidor (código, GIT, complementos, etc.) que se ejecuta de forma remota.

- Se admiten linting e Intellisense basados en Linux.
- El proyecto se compilará automáticamente en Linux.
- Puedes usar todas las extensiones que se ejecutan en Linux ([ES Lint, NPM IntelliSense, fragmentos de código de ES6, etc.](https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack)).

Los editores de texto basados en terminal (vim, emacs y nano) también son útiles para realizar cambios rápidos directamente en la consola. ([Este artículo](https://medium.com/linode-cube/emacs-nano-or-vim-choose-your-terminal-based-text-editor-wisely-8f3826c92a68) resulta muy útil, ya que explica la diferencia entre ellos y ofrece algo de información sobre cómo usar cada uno).

> [!NOTE]
> Algunos editores de GUI (Atom, Sublime Text y Eclipse) pueden tener problemas para acceder a la ubicación de red compartida de WSL (\\wsl$\Ubuntu\home\) e intentarán compilar los archivos de Linux mediante las herramientas de Windows, aunque puede que no sean las que deseas. La extensión Remote-WSL en VS Code controlará esta compatibilidad automáticamente.

Para instalar VS Code y la extensión Remote-WSL:

1. [Descarga e instala VS Code para Windows](https://code.visualstudio.com). VS Code también está disponible para Linux, pero el Subsistema de Windows para Linux no admite aplicaciones de GUI, por lo que deberemos instalarlo en Windows. Pero no te preocupes. Igualmente podrás realizar la integración con la línea de comandos y las herramientas de Linux mediante la extensión Remote-WSL.

2. Instala la [extensión Remote-WSL](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl) de VS Code. De este modo, podrás usar WSL como entorno de desarrollo integrado y la compatibilidad y las rutas se controlarán automáticamente. [Más información](https://code.visualstudio.com/docs/remote/remote-overview).

> [!IMPORTANT]
> Si ya tienes VS Code instalado, deberás asegurarte de que dispones de la [versión 1.35 de mayo ](https://code.visualstudio.com/updates/v1_35) o una posterior a fin de poder instalar la [extensión Remote-WSL](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl). No te recomendamos que uses WSL en VS Code sin la extensión Remote-WSL, ya que perderás la compatibilidad con Autocompletar, la depuración, la detección de errores, etc. Dato curioso: Esta extensión de WSL se instala en $HOME/.vscode-server/extensions.

### <a name="helpful-vs-code-extensions"></a>Extensiones útiles de VS Code

Aunque VS Code incluye muchas características para el desarrollo con Node.js listas para usar, existen algunas extensiones útiles que puedes instalar y que se encuentran disponibles en el [paquete de extensiones de Node.js](https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack). Instálalas todas o selecciona y elige la opción que te resulte más útil.

Para instalar el paquete de extensiones de Node.js:

1. Abre la ventana **Extensiones** (Ctrl+Mayús+X) en VS Code.

    La ventana Extensiones ahora está dividida en tres secciones (porque has instalado la extensión Remote-WSL).
    - "Local - Installed": las extensiones instaladas para utilizarlas con el sistema operativo Windows.
    - "WSL:Ubuntu-18.04-Installed": las extensiones instaladas para utilizarlas con el sistema operativo Ubuntu (WSL).
    - "Recommended": las extensiones recomendadas por VS Code según los tipos de archivo del proyecto actual.

    ![Diferencias entre las extensiones locales y remotas de VS Code](../images/vscode-extensions-local-remote.png)

2. En el cuadro de búsqueda de la parte superior de la ventana Extensiones, escribe: **Paquete de extensiones de Node** o el nombre de cualquier extensión que estés buscando. La extensión se instalará para las instancias locales o WSL de VS Code, dependiendo de dónde tengas abierto el proyecto actual. Puedes indicarlo si seleccionas el vínculo remoto en la esquina inferior izquierda de la ventana de VS Code (en verde). Ofrecerá la opción de abrir o cerrar una conexión remota. Instala las extensiones de Node.js en el entorno "WSL:Ubuntu-18.04".

    ![Vínculo remoto a VS Code](../images/wsl-remote-extension.png)

Algunas de las extensiones adicionales que puedes considerar son las siguientes:

- [Debugger for Chrome](https://code.visualstudio.com/blogs/2016/02/23/introducing-chrome-debugger-for-vs-code): una vez que hayas terminado de desarrollar en el lado servidor con Node.js, deberás desarrollar y probar el lado cliente. Esta extensión integra el editor de VS Code con el servicio de depuración del explorador Chrome, lo que permite que las operaciones sean un poco más eficaces.
- [Keymaps from other editors](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Downloads): estas extensiones pueden ayudarte a sentirte como en casa con tu entorno en caso de que realices la transición desde otro editor de texto (como Atom, Sublime, Vim, eMacs, Notepad++, etc.).
- [Settings Sync](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync): te permite sincronizar la configuración de VS Code entre diferentes instalaciones mediante GitHub. Si trabajas en diferentes máquinas, te ayuda a mantener el entorno coherente entre ellas.

## <a name="set-up-git-optional"></a>Configuración de GIT (opcional)

Si planeas colaborar con otras personas u hospedar el proyecto en un sitio de código abierto (como GitHub), VS Code admite el [control de versiones con GIT](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support). La pestaña Control de código fuente de VS Code realiza un seguimiento de todos los cambios y tiene comandos GIT comunes (agregar, confirmar, enviar cambios e incorporar cambios) integrados directamente en la interfaz de usuario.

1. GIT viene instalado con las distribuciones del Subsistema de Windows para Linux; sin embargo, tendrás que configurar el archivo de configuración de GIT. Para ello, en el terminal, escribe `git config --global user.name "Your Name"` y, a continuación, `git config --global user.email "youremail@domain.com"`. Si aún no tienes una cuenta de GIT, puedes [registrarte para crear una en GitHub](https://github.com/join). Si nunca has trabajado con GIT, las [guías de GitHub](https://guides.github.com/) pueden resultarte de ayuda para empezar. Si tienes que editar la configuración de GIT, puedes hacerlo con un editor de texto integrado como nano: `nano ~/.gitconfig`.

2. Se recomienda agregar un [archivo .gitignore](https://help.github.com/en/articles/ignoring-files) a los proyectos de Node. Aquí tienes la [plantilla de gitignore predeterminada de GitHub para Node.js](https://github.com/github/gitignore/blob/master/Node.gitignore). Si eliges [crear un repositorio mediante el sitio web de GitHub](https://help.github.com/articles/create-a-repo), hay casillas disponibles para inicializar el repositorio con un archivo LÉAME, el archivo. gitignore configurado para los proyectos de Node.js y las opciones para agregar una licencia si es necesario.

## <a name="next-steps"></a>Pasos siguientes

Ya tienes configurado un entorno de desarrollo de Node.js. Para empezar a usar el entorno de Node.js, considera la posibilidad de probar uno de estos tutoriales:

- [Introducción a Node.js para principiantes](./beginners.md)
- [Introducción a los marcos web de Node.js en Windows](./web-frameworks.md)
- [Introducción a la conexión de aplicaciones de Node.js a una base de datos](./databases.md)
- [Introducción al uso de contenedores de Docker con Node.js](./containers.md)
