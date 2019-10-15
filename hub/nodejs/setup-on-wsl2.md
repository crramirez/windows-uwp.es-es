---
title: Configuración de NodeJS en WSL 2
description: Una guía para ayudarle a obtener el entorno de desarrollo de node. js configurado en el subsistema de Windows para Linux (WSL).
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, node. js, Windows 10, Microsoft, Learning NodeJS, nodo en Windows, nodo en WSL, nodo en Linux en Windows, nodo de instalación en Windows, NodeJS con vs Code, desarrollar con nodo en Windows, desarrollar con NodeJS en Windows, instalar nodo en WSL, NodeJS en Windows Subsistema para Linux
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 917192d782e0a44c6de7e549960161a003c646e5
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2019
ms.locfileid: "72315069"
---
# <a name="set-up-your-nodejs-development-environment-with-wsl-2"></a>Configurar el entorno de desarrollo de node. js con WSL 2

A continuación se ofrece una guía paso a paso para ayudarle a configurar el entorno de desarrollo de node. js con el subsistema de Windows para Linux (WSL). Actualmente, esta guía requiere la instalación y ejecución de una compilación de Windows Insider Preview para poder instalar y usar [WSL 2](https://devblogs.microsoft.com/commandline/wsl-2-is-now-available-in-windows-insiders/). WSL 2 tiene mejoras significativas en la velocidad y el rendimiento respecto a WSL 1, especialmente en lo que respecta a node. js. Muchos módulos NPM y tutoriales para el desarrollo web de node. js están escritos para los usuarios de Linux y usan herramientas de instalación y empaquetado basadas en Linux. La mayoría de las aplicaciones web también se implementan en Linux, por lo que el uso de WSL 2 garantizará la coherencia entre los entornos de desarrollo y de producción.

> [!NOTE]
> Si tiene la confirmación de usar node. js directamente en Windows o tiene previsto usar un entorno de producción de Windows Server, consulte nuestra guía para [configurar el entorno de desarrollo de node. js directamente en Windows](./setup-on-windows.md).

## <a name="install-windows-10-insider-preview-build"></a>Instalación de la compilación de Windows 10 Insider Preview

1. **[Instale la versión más reciente de Windows 10](https://www.microsoft.com/software-download/windows10)** : Seleccione **Actualizar ahora** para descargar el Asistente de actualizaciones. Una vez descargado, abra el Asistente de actualizaciones para ver si está ejecutando la versión más reciente de Windows y, si no es así, seleccione **Actualizar ahora** en la ventana del Asistente para actualizar la máquina. *(Este paso es opcional si está ejecutando una versión bastante reciente de Windows 10).*

    ![Asistente para Windows Update](../images/windows-update-assistant2019.png)

2. **[Vaya a inicio > configuración > programa de Windows Insider](ms-settings:windowsinsider)** : Dentro de la ventana del programa Windows Insider, seleccione **Introducción** y **vincular una cuenta**.

    ![Configuración del programa Windows Insider](../images/windows-insider-program-settings.png)

3. **[Regístrese como Insider de Windows](https://insider.windows.com/getting-started/#register)** : Si no está registrado con el programa Insider, tendrá que hacerlo con el [cuenta de Microsoft](https://account.microsoft.com/account).

    ![Registro de Windows Insider](../images/windows-insider-account.png)

4. Elija recibir actualizaciones de **llamada rápida** o **vaya directamente al siguiente contenido de la versión de Windows** . Confirme y elija **reiniciar más tarde**. Tendrá que cambiar un par de opciones de configuración adicionales antes de reiniciar.

    ![Anillo rápido de Windows Insider](../images/windows-insider-fast.png)

## <a name="enable-windows-subsystem-for-linux-and-virtual-machine-platform"></a>Habilitar el subsistema de Windows para Linux y la plataforma de máquinas virtuales

1. Mientras sigue en la **configuración de Windows**, busque **activar o desactivar las características de Windows**.
2. Cuando aparezca la lista de **características de Windows** , desplácese a buscar **plataforma de máquina virtual** y **subsistema de Windows para Linux**, asegúrese de que la casilla esté activada para habilitar ambas y, a continuación, seleccione **Aceptar**.
3. Reinicia el equipo cuando se te solicite.

    ![Habilitar características de Windows](../images/windows-feature-settings.png)

## <a name="install-a-linux-distribution"></a>Instalación de una distribución de Linux

Hay varias distribuciones de Linux disponibles para ejecutarse en WSL. Puede buscar e instalar su favorito en el Microsoft Store. Se recomienda comenzar con [Ubuntu 18,04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q) , ya que es actual, popular y bien compatible.

1. Abra este vínculo de [Ubuntu 18,04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q) , abra el Microsoft Store y seleccione **obtener**. *(Se trata de una descarga bastante grande y puede tardar algún tiempo en instalarse).*

2. Una vez finalizada la descarga, seleccione **iniciar** desde el Microsoft Store o iniciar escribiendo "Ubuntu 18,04 LTS" en el menú **Inicio** .

3. Al ejecutar la distribución por primera vez, se le pedirá que cree un nombre y una contraseña de cuenta. Después, iniciará sesión automáticamente como este usuario de forma predeterminada. Puede elegir cualquier nombre de usuario y contraseña. No tienen ninguna relación con el nombre de usuario de Windows.

    ![Distribuciones de Linux en el Microsoft Store](../images/store-linux-distros.png)

Para comprobar la distribución de Linux que está usando actualmente, escriba: `lsb_release -dc`. Para actualizar la distribución de Ubuntu, use: `sudo apt update && sudo apt upgrade`. Se recomienda actualizar periódicamente para asegurarse de que tiene los paquetes más recientes. Windows no controla automáticamente esta actualización. Para obtener vínculos a otras distribuciones de Linux disponibles en el Microsoft Store, métodos de instalación alternativos o solución de problemas, consulte la [Guía de instalación del subsistema de Windows para Linux para Windows 10](https://docs.microsoft.com/windows/wsl/install-win10).

## <a name="install-wsl-2"></a>Instalación de WSL 2

WSL 2 es una [nueva versión de la arquitectura](https://docs.microsoft.com/windows/wsl/wsl2-about) en WSL que cambia el modo en que Linux distribuciones interactuar con Windows, mejorando el rendimiento y agregando compatibilidad completa con llamadas al sistema.

1. En PowerShell, escriba el comando: `wsl -l` para ver la lista de distribuciones de WSL que ha instalado en la máquina. Ahora debería ver Ubuntu-18,04 en esta lista.
2. Ahora, escriba el comando: `wsl --set-version Ubuntu-18.04 2` para establecer que la instalación de Ubuntu use WSL 2.
3. Compruebe la versión de WSL que cada una de las distribuciones instaladas usa con: `wsl --list --verbose` (o `wsl -l -v`).

    ![Versión del conjunto de sistema de Windows para Linux](../images/wsl-versions.png)

> [!TIP]
> Puede establecer cualquier distribución de Linux que haya instalado en WSL 2 siguiendo las mismas instrucciones (mediante PowerShell), solo tiene que cambiar ' Ubuntu-18,04 ' por el nombre de distribución instalado al que desea dirigirse. Para volver a cambiar a WSL 1, ejecute el mismo comando anterior, pero reemplace el ' 2 ' por ' 1 '.  También puede establecer WSL 2 como valor predeterminado para las distribuciones recién instaladas; para ello, escriba: `wsl --set-default-version 2`.

## <a name="install-nvm-nodejs-and-npm"></a>Instalación de NVM, node. js y NPM

Hay varias maneras de instalar node. js. Se recomienda el uso de un administrador de versiones a medida que las versiones cambian con mucha rapidez. Probablemente tendrá que cambiar entre varias versiones en función de las necesidades de los distintos proyectos en los que está trabajando. Node version Manager, más comúnmente denominado NVM, es la forma más habitual de instalar varias versiones de node. js. Se le guiará por los pasos necesarios para instalar NVM y usarlo para instalar node. js y node Package Manager (NPM). Hay [administradores de versiones alternativos](#alternative-version-managers) que se deben tener en cuenta, tal y como se describe en la sección siguiente.

> [!IMPORTANT]
> Siempre se recomienda quitar cualquier instalación existente de node. js o NPM del sistema operativo antes de instalar un administrador de versiones, ya que los distintos tipos de instalación pueden provocar conflictos extraños y confusos. Por ejemplo, la versión del nodo que se puede instalar con el comando `apt-get` de Ubuntu está actualmente obsoleta. Para obtener ayuda con la eliminación de instalaciones anteriores, consulte [Cómo quitar NodeJS de Ubuntu](https://askubuntu.com/questions/786015/how-to-remove-nodejs-from-ubuntu-16-04).

1. Abra la línea de comandos de Ubuntu 18,04.
2. Instalar rizo (una herramienta que se usa para descargar contenido de Internet en la línea de comandos) con: `sudo apt-get install curl`
3. Instale NVM con: `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash`
4. Para comprobar la instalación, escriba: `command -v nvm`... Esto debería devolver ' NVM ', si recibe ' comando no encontrado ' o ninguna respuesta, cierre el terminal actual, vuelva a abrirlo e inténtelo de nuevo. [Obtenga más información en el repositorio de github de NVM](https://github.com/nvm-sh/nvm).
5. Enumerar las versiones del nodo que están instaladas actualmente (no debe ser ninguna en este momento): `nvm ls`

    ![Lista de NVM que no muestra ninguna versión de nodo](../images/nvm-no-node.png)

6. Instale la versión actual de node. js (para probar las mejoras de las características más recientes, pero lo más probable es que tenga problemas): `nvm install node`
7. Instale la versión LTS estable más reciente de node. js (recomendado): `nvm install --lts`
8. Enumerar las versiones de node instaladas: `nvm ls`... ahora debería ver las dos versiones que acaba de instalar.

    ![Lista de NVM que muestra las versiones de LTS y el nodo actual](../images/nvm-node-installed.png)

9. Compruebe que está instalado node. js y la versión predeterminada actual con: `node --version`. Después, compruebe que también tiene NPM, con: `npm --version` (también puede usar `which node` o `which npm` para ver la ruta de acceso que se usa para las versiones predeterminadas).
10. Para cambiar la versión de node. js que desea usar para un proyecto, cree un nuevo directorio de proyecto `mkdir NodeTest` y escriba el directorio `cd NodeTest`, a continuación, escriba @no__t 2 para cambiar a la versión actual o `nvm use --lts` para cambiar a la versión de LTS. También puede usar el número específico para cualquier versión adicional que haya instalado, como `nvm use v8.2.1`. (Para obtener una lista de todas las versiones de node. js disponibles, use el comando: `nvm ls-remote`).

> [!TIP]
> Si usa NVM para instalar node. js y NPM, no es necesario usar el comando SUDO para instalar nuevos paquetes.

## <a name="alternative-version-managers"></a>Administradores de versiones alternativos

Aunque NVM es actualmente el administrador de versiones más popular para node, hay algunas alternativas a tener en cuenta:

- [n](https://www.npmjs.com/package/n#installation) es una alternativa `nvm` de larga duración que consigue lo mismo con comandos ligeramente diferentes y se instala a través de `npm` en lugar de un script de Bash.
- [FNM](https://github.com/Schniz/fnm#using-a-script) es un administrador de versiones más reciente, lo que exige que sea mucho más rápido que `nvm`. (También utiliza [Azure pipelines](https://docs.microsoft.com/azure/devops/pipelines/get-started/what-is-azure-pipelines?view=azure-devops)).
- [Volta](https://github.com/volta-cli/volta#installing-volta) es un nuevo administrador de versiones del equipo de LinkedIn que notifica la velocidad mejorada y la compatibilidad entre plataformas.
- [ASDF-VM](https://asdf-vm.com/#/core-manage-asdf-vm) es una única CLI para varios idiomas, como IKE GVM, NVM, rbenv & pyenv (y más) en uno solo.
- [NVS](https://github.com/jasongin/nvs) (conmutador de versión de nodo) es una alternativa multiplataforma `nvm` con la capacidad de [integrarse con vs Code](https://github.com/jasongin/nvs/blob/master/doc/VSCODE.md).

## <a name="install-your-favorite-code-editor"></a>Instalar su editor de código favorito

Se recomienda usar [**Visual Studio Code**] con la **extensión Remote-WSL** para los proyectos de desarrollo de node. js. Esto divide VS Code en una arquitectura "cliente-servidor", con el cliente (la interfaz de usuario) que se ejecuta en el equipo Windows y el servidor (código, GIT, complementos, etc.) que se ejecuta de forma remota.

- Se admiten IntelliSense y la detección de errores basados en Linux.
- El proyecto se compilará automáticamente en Linux.
- Puede usar todas las extensiones que se ejecutan en Linux ([es pelusa, NPM IntelliSense, fragmentos de código ES6, etc.](https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack)).

Los editores de texto basados en terminal (VIM, Emacs y nano) también son útiles para realizar cambios rápidos desde el interior de la consola. (En[este artículo se](https://medium.com/linode-cube/emacs-nano-or-vim-choose-your-terminal-based-text-editor-wisely-8f3826c92a68) explican las diferencias y un poco más sobre cómo usar cada uno de ellos).

> [!NOTE]
> Algunos editores de GUI (Atom, Sublime Text, Eclipse) pueden tener problemas para acceder a la ubicación de red compartida de WSL (\\wsl $ \Ubuntu\home @ no__t-1 e intentarán compilar los archivos de Linux mediante herramientas de Windows, que puede que no sean los deseados. La extensión Remote-WSL en VS Code controlará esta compatibilidad.

Para instalar VS Code y la extensión Remote-WSL:

1. [Descargue e instale vs code para Windows](https://code.visualstudio.com). VS Code también está disponible para Linux, pero el subsistema de Windows para Linux no admite aplicaciones de GUI, por lo que es necesario instalarla en Windows. No se preocupe, todavía podrá integrarse con la línea de comandos y las herramientas de Linux mediante la extensión Remote-WSL.

2. Instale la [extensión Remote-WSL](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl) en vs Code. Esto le permite usar WSL como entorno de desarrollo integrado y controlará la compatibilidad y las cosas. [Más información](https://code.visualstudio.com/docs/remote/remote-overview).

> [!IMPORTANT]
> Si ya tiene VS Code instalado, debe asegurarse de que tiene la [versión 1,35](https://code.visualstudio.com/updates/v1_35) o posterior para poder instalar la [extensión Remote-WSL](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl). No se recomienda el uso de WSL en VS Code sin la extensión Remote-WSL, ya que se perderá la compatibilidad con Autocompletar, depuración, detección de errores, etc. Hecho divertido: Esta extensión WSL se instala en $HOME/.vscode-Server/Extensions.

### <a name="helpful-vs-code-extensions"></a>Extensiones de VS Code útiles

Aunque VS Code incluye muchas características para el desarrollo de node. js de un momento dado, existen algunas extensiones útiles para instalar disponibles en el [paquete de extensión de node. js](https://marketplace.visualstudio.com/items?itemName=waderyan.nodejs-extension-pack). Entre ellas se incluyen las siguientes:

- ES pelusa-A Tool para "pelusa" el código. La detección de errores analiza el código y le advierte de los posibles errores.
- NPM: ejecute los scripts de NPM desde la paleta de comandos y valide los módulos instalados definidos en package. JSON.
- Fragmentos de código de JavaScript (ES6): agrega fragmentos de código para el desarrollo de JavaScript en la sintaxis de ES6.
- Búsqueda node_modules: Busque módulos de nodo en el proyecto rápidamente.
- NPM IntelliSense: agrega IntelliSense para los módulos NPM en el código.
- Ruta de acceso IntelliSense: completa automáticamente los nombres de archivo en el código.

Instálelos todos o seleccione y elija lo que le parezca más útil.

Para instalar el paquete de extensión de node. js:

1. Abra la ventana **extensiones** (Ctrl + Mayús + X) en vs Code.

    La ventana extensiones ahora está dividida en tres secciones (porque ha instalado la extensión Remote-WSL).
    - "Local-installed": Las extensiones instaladas para su uso con el sistema operativo Windows.
    - "WSL: Ubuntu-18,04-instalado": Las extensiones instaladas para su uso con el sistema operativo Ubuntu (WSL).
    - "Recomendado": Extensiones recomendadas por VS Code basadas en los tipos de archivo del proyecto actual.

    ![Extensiones de VS Code local frente a remoto](../images/vscode-extensions-local-remote.png)

2. En el cuadro de búsqueda de la parte superior de la ventana extensiones, escriba: **Node Extension Pack** (o el nombre de la extensión que está buscando). La extensión (o extensiones si es un paquete) se instalará para las instancias locales o WSL de VS Code en función de dónde tenga abierto el proyecto actual. Puede indicarle si selecciona el vínculo remoto en la esquina inferior izquierda de la ventana de VS Code (en verde). Le ofrecerá la opción de abrir o cerrar una conexión remota. Instale las extensiones de node. js en el entorno "WSL: Ubuntu-18,04".

    ![VS Code vínculo remoto](../images/wsl-remote-extension.png)

Algunas de las extensiones adicionales que puede considerar son las siguientes:

- [Depurador para Chrome](https://code.visualstudio.com/blogs/2016/02/23/introducing-chrome-debugger-for-vs-code): Una vez que haya terminado de desarrollar en el lado del servidor con node. js, deberá desarrollar y probar el lado cliente. Esta extensión integra el editor de VS Code con el servicio de depuración del explorador Chrome, lo que permite que las cosas sean un poco más eficaces.
- [Keymaps de otros editores](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Downloads): Estas extensiones pueden ayudar a su entorno a su gusto en casa si va a realizar la transición desde otro editor de texto (como Atom, sublime, Vim, eMacs, Notepad + +, etc.).
- [Sincronización de configuración](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync): Le permite sincronizar la configuración del VS Code en distintas instalaciones mediante GitHub. Si trabaja en diferentes equipos, esto ayuda a mantener el entorno coherente entre ellos.

## <a name="install-windows-terminal-optional"></a>Instalar terminal de Windows (opcional)

El nuevo terminal de Windows habilita varias pestañas (Cambie rápidamente entre el símbolo del sistema, PowerShell o varias distribuciones de Linux), enlaces de teclado personalizados (cree sus propias teclas de método abreviado para abrir o cerrar pestañas, copiar y pegar, etc.), emojis ☺ y temas personalizados ( combinaciones de colores, estilos y tamaños de fuente, imagen/desenfoque/transparencia de fondo). [Más información](https://devblogs.microsoft.com/commandline/).

1. Obtenga [Windows terminal (versión preliminar) en el Microsoft Store](https://www.microsoft.com/store/apps/9n0dx20hk701): Al instalar a través de la tienda, las actualizaciones se controlan automáticamente.

2. Una vez instalado, abra Windows terminal y seleccione **configuración** para personalizar el terminal con el archivo `profile.json`. [Más información sobre la edición de la configuración de terminal de Windows](https://github.com/microsoft/terminal/blob/master/doc/user-docs/UsingJsonSettings.md).

    ![Configuración de terminal de Windows](../images/windows-terminal-settings.png)

## <a name="set-up-git-optional"></a>Configuración de Git (opcional)

Si planea colaborar con otras personas, o hospedar el proyecto en un sitio de código abierto (como GitHub), VS Code admite el [control de versiones con git](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support). La pestaña control de código fuente de VS Code realiza un seguimiento de todos los cambios y tiene comandos git comunes (agregar, confirmar, insertar, extraer) integrados en la interfaz de usuario.

1. Git viene instalado con el subsistema de Windows para Linux distribuciones, sin embargo, tendrá que configurar el archivo de configuración de Git. Para ello, escriba en el terminal: `git config --global user.name "Your Name"` y, a continuación, `git config --global user.email "youremail@domain.com"`. Si aún no tiene una cuenta de Git, puede [suscribirse a una en github](https://github.com/join). Si nunca ha trabajado antes con git, las [guías de github](https://guides.github.com/) pueden ayudarle a empezar. Si tiene que editar la configuración de Git, puede hacerlo con un editor de texto integrado como nano: `nano ~/.gitconfig`.

2. Se recomienda agregar un [archivo. gitignore](https://help.github.com/en/articles/ignoring-files) a los proyectos de nodo. Esta es [la plantilla gitignore predeterminada de github para node. js](https://github.com/github/gitignore/blob/master/Node.gitignore). Si decide [crear un nuevo repositorio mediante el sitio web de github](https://help.github.com/articles/create-a-repo), hay casillas disponibles para inicializar el repositorio con un archivo Léame, el archivo. gitignore configurado para los proyectos de node. js y las opciones para agregar una licencia si es necesario.

## <a name="next-steps"></a>Pasos siguientes

Ahora tiene configurado un entorno de desarrollo de node. js. Para empezar a usar el entorno de node. js, considere la posibilidad de probar uno de estos tutoriales:

- [Introducción a node. js para principiantes](./beginners.md): Una guía paso a paso que le ayudará a empezar a trabajar si no está familiarizado con el desarrollo de node. js.
- [Introducción a los marcos Web de node. js en Windows](./web-frameworks.md): Una guía paso a paso que le ayudará a empezar a usar framworks Web de node. js en Windows, incluidos los siguientes:. js, Nuxt. js y Gatsby.
- [Introducción a la conexión de aplicaciones de node. js a una base de datos](./databases.md): Una guía paso a paso para ayudarle a empezar a conectar la aplicación node. js a una base de datos, como MongoDB o postgres.
- [Introducción al uso de contenedores de Docker con node. js](./containers.md): Una guía paso a paso que le ayudará a empezar a usar contenedores de Docker con sus aplicaciones de node. js.
