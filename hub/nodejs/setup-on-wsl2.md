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
ms.openlocfilehash: c987f5bea387c630a1b9ef23c928d7a1bb8fadfc
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "75835378"
---
# <a name="set-up-your-nodejs-development-environment-with-wsl-2"></a>Configuración del entorno de desarrollo de Node.js con WSL 2

A continuación, se presenta una guía detallada que facilita la configuración del entorno de desarrollo de Node.js con el Subsistema de Windows para Linux (WSL). Actualmente, esta guía requiere la instalación y ejecución de una compilación de Windows Insider Preview para poder instalar y usar [WSL 2](https://devblogs.microsoft.com/commandline/wsl-2-is-now-available-in-windows-insiders/). WSL 2 tiene mejoras significativas en la velocidad y el rendimiento respecto a WSL 1, especialmente en lo que respecta a Node.js. Muchos módulos y tutoriales de npm para el desarrollo web de Node.js se escriben para los usuarios de Linux y usan herramientas de instalación y empaquetado basadas en Linux. La mayoría de las aplicaciones web también se implementan en Linux, por lo que el uso de WSL 2 garantizará una coherencia entre los entornos de desarrollo y producción.

> [!NOTE]
> Si te comprometes a usar Node.js directamente en Windows o si planeas usar un entorno de producción de Windows Server, consulta nuestra guía para [configurar el entorno de desarrollo de Node.js directamente en Windows](./setup-on-windows.md).

## <a name="install-windows-10-insider-preview-build"></a>Instalación de la compilación de Windows 10 Insider Preview

1. **[Instala la última versión de Windows 10](https://www.microsoft.com/software-download/windows10)** : selecciona **Actualizar ahora** para descargar el Asistente para actualización. Una vez descargado, abre el Asistente para actualización para ver si actualmente estás ejecutando la última versión de Windows y, si no es así, selecciona **Actualizar ahora** dentro de la ventana del asistente para actualizar el equipo. *(Este paso es opcional si estás ejecutando una versión bastante reciente de Windows 10)* .

    ![Asistente de Windows Update](../images/windows-update-assistant2019.png)

2. **[Ve a Inicio > Configuración > Programa Windows Insider](ms-settings:windowsinsider)** : dentro de la ventana Programa Windows Insider, selecciona **Introducción** y, a continuación, **Vincular una cuenta**.

    ![Configuración del Programa Windows Insider](../images/windows-insider-program-settings.png)

3. **[Regístrate como un usuario de Windows Insider](https://insider.windows.com/getting-started/#register)** : si no estás registrado en el Programa Insider, tendrás que hacerlo con la [cuenta de Microsoft](https://account.microsoft.com/account).

    ![Registro en Windows Insider](../images/windows-insider-account.png)

4. Elige recibir actualizaciones de **Anillo anticipado** o el contenido sobre **Ir a la siguiente versión de Windows**. Confirma y elige **Reiniciar más tarde**. Tendremos que cambiar un par de opciones de configuración adicionales antes de reiniciar.

    ![Anillo anticipado de Windows Insider](../images/windows-insider-fast.png)

## <a name="enable-windows-subsystem-for-linux-and-virtual-machine-platform"></a>Habilitar el Subsistema de Windows para Linux y la plataforma de máquina virtual

1. En **Configuración de Windows**, busca **Activar o desactivar características de Windows**.
2. Una vez que aparezca la lista de **Características de Windows**, desplázate para buscar **Plataforma de máquina virtual** y **Subsistema de Windows para Linux**, asegúrate de que la casilla está activada para habilitar ambas opciones y, a continuación, selecciona **Aceptar**.
3. Reinicia el equipo cuando se te solicite.

    ![Habilitar características de Windows](../images/windows-feature-settings.png)

## <a name="install-a-linux-distribution"></a>Instalación de una distribución de Linux

Hay varias distribuciones de Linux disponibles para ejecutar en WSL. Puedes buscar e instalar tu favorita en Microsoft Store. Te recomendamos que empieces con [Ubuntu 18.04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q), ya que es la versión actual, popular y compatible.

1. Abre este vínculo de [Ubuntu 18.04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q), abre Microsoft Store y selecciona **Obtener**. *(Se trata de una descarga bastante grande y puede tardar en instalarse).*

2. Una vez finalizada la descarga, selecciona **Iniciar** en Microsoft Store o bien escribe "Ubuntu 18.04 LTS" en el menú **Inicio** para iniciarla.

3. Se te pedirá que crees un nombre de cuenta y una contraseña cuando ejecutes la distribución por primera vez. Después, iniciarás sesión automáticamente con este usuario de forma predeterminada. Puedes elegir cualquier nombre de usuario y contraseña. No tienen nada que ver con tu nombre de usuario de Windows.

    ![Distribuciones de Linux en Microsoft Store](../images/store-linux-distros.png)

Para comprobar la distribución de Linux que usas actualmente, puedes escribir lo siguiente: `lsb_release -dc`. Para actualizar la distribución de Ubuntu, usa `sudo apt update && sudo apt upgrade`. Te recomendamos que realices actualizaciones periódicamente para asegurarte de que cuentas con los paquetes más recientes. Windows no controla automáticamente esta actualización. Para obtener vínculos a otras distribuciones de Linux disponibles en Microsoft Store, métodos de instalación alternativos o soluciones de problemas, consulta la [Guía de instalación del Subsistema de Windows para Linux para Windows 10](https://docs.microsoft.com/windows/wsl/install-win10).

## <a name="install-wsl-2"></a>Instalación de WSL 2

WSL 2 es una [nueva versión de la arquitectura](https://docs.microsoft.com/windows/wsl/wsl2-about) en WSL que cambia la manera en que las distribuciones de Linux interactúan con Windows, de tal forma que mejoran el rendimiento y aportan compatibilidad completa con las llamadas del sistema.

1. En PowerShell, escribe el comando `wsl -l` para ver la lista de distribuciones de WSL que has instalado en el equipo. Ahora deberías ver Ubuntu-18.04 en esta lista.
2. Ahora, escribe el comando `wsl --set-version Ubuntu-18.04 2` para configurar la instalación de Ubuntu para que use WSL 2.
3. Comprueba la versión de WSL que cada una de las distribuciones instaladas usa con `wsl --list --verbose` o `wsl -l -v`.

    ![Definición de la versión del Subsistema de Windows para Linux](../images/wsl-versions.png)

> [!TIP]
> Puedes establecer cualquier distribución de Linux que hayas instalado en WSL 2 siguiendo las mismas instrucciones (mediante PowerShell), solo tienes que cambiar "Ubuntu-18.04" por el nombre de la distribución instalada a la que deseas dirigirte. Para volver a cambiar a WSL 1, ejecuta el mismo comando que antes, pero reemplaza "2" por "1".  También puedes establecer WSL 2 como opción predeterminada para las distribuciones recién instaladas; para ello, escribe: `wsl --set-default-version 2`.

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

> [!TIP]
> Si usas NVM para instalar Node.js y NPM, no es necesario usar el comando SUDO para instalar nuevos paquetes.

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

## <a name="install-windows-terminal-optional"></a>Instalación de Terminal Windows (opcional)

El nuevo Terminal Windows habilita varias pestañas (cambia rápidamente entre el símbolo del sistema, PowerShell o varias distribuciones de Linux), enlaces de teclado personalizados (crea tus propias teclas de método abreviado para abrir o cerrar pestañas, copiar y pegar, etc.), emojis ☺ y temas personalizados (esquemas de colores, estilos y tamaños de fuente, imagen de fondo/desenfoque/transparencia). [Más información](https://devblogs.microsoft.com/commandline/).

1. Obtén [Terminal Windows (versión preliminar) en Microsoft Store](https://www.microsoft.com/store/apps/9n0dx20hk701): al instalar a través de Microsoft Store, las actualizaciones se controlan automáticamente.

2. Una vez instalado, abre Terminal Windows y selecciona **Configuración** para personalizar el terminal con el archivo `profile.json`. [Obtén más información sobre la edición de la configuración de Terminal Windows](https://github.com/microsoft/terminal/blob/master/doc/user-docs/UsingJsonSettings.md).

    ![Configuración de Terminal Windows](../images/windows-terminal-settings.png)

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
