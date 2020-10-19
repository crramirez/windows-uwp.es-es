---
title: Desarrollo web con Python en Windows
description: Cómo empezar a usar Python para el desarrollo web en Windows, incluida la configuración de marcos como Flask y Django.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: python, windows 10, microsoft, python en windows, python web con wsl, aplicación web de python con el subsistema de windows para linux, desarrollo web de python en windows, aplicación de flask en windows, aplicación de django en windows, python web, desarrollo web de flask en windows, desarrollo web de django en windows, desarrollo web de windows con python, desarrollo web de python de vs code, extensión remota de wsl, ubuntu, wsl, venv, pip, extensión de python de microsoft, ejecutar python en windows, usar python en windows, compilar con python en windows
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 187195133dd614818d3c68473cc53b71a0b32333
ms.sourcegitcommit: 27552ed7d3d889f50d8e01776a24b8d486a8d97c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/12/2020
ms.locfileid: "91958748"
---
# <a name="get-started-using-python-for-web-development-on-windows"></a>Introducción al uso de Python para el desarrollo web en Windows

A continuación, te proporcionamos una guía detallada para empezar a usar Python para el desarrollo web en Windows mediante el Subsistema de Windows para Linux (WSL).

## <a name="set-up-your-development-environment"></a>Configurar el entorno de desarrollo

Te recomendamos que instales Python en WSL cuando compiles aplicaciones web. Muchos de los tutoriales e instrucciones para el desarrollo web de Python se escriben para los usuarios de Linux y usan herramientas de instalación y empaquetado basadas en Linux. La mayoría de las aplicaciones web también se implementan en Linux, por lo que esto te garantizará una coherencia entre los entornos de desarrollo y producción.

Si usas Python para tareas que no estén relacionadas con el desarrollo web, te recomendamos que lo instales directamente en Windows 10 mediante Microsoft Store. WSL no admite las aplicaciones o los escritorios de GUI (por ejemplo, PyGame, Gnome, KDE, etc). Instala y usa Python directamente en Windows para estos casos. Si no estás familiarizado con Python, consulta nuestra guía: [Introducción al uso de Python en Windows para principiantes](./beginners.md). Si estás interesado en automatizar las tareas comunes en el sistema operativo, consulta nuestra guía: [Introducción al uso de Python en Windows para el scripting y la automatización](./scripting.md). En algunos escenarios avanzados, es posible que quieras considerar la posibilidad de descargar una versión específica de Python directamente desde [python.org](https://www.python.org/downloads/windows/) o una [alternativa](https://www.python.org/download/alternatives), como Anaconda, Jython, PyPy, WinPython, IronPython, etc. Solo te lo recomendamos si eres un programador de Python más avanzado y tienes un motivo específico para elegir una implementación alternativa.

## <a name="install-windows-subsystem-for-linux"></a>Instalación del Subsistema de Windows para Linux

WSL permite ejecutar un entorno de línea de comandos de GNU/Linux integrado directamente con Windows y tus herramientas favoritas, como Visual Studio Code, Outlook, etc.

Para habilitar e instalar WSL (o WSL 2 según tus necesidades), sigue los pasos que se describen en la [documentación de instalación de WSL](/windows/wsl/install-win10). Estos pasos incluirán la elección de una distribución de Linux (por ejemplo, Ubuntu).

Una vez que hayas instalado WSL y una distribución de Linux, abre la distribución de Linux (puedes encontrarla en el menú Inicio de Windows), y comprueba la versión y el nombre de código mediante el comando: `lsb_release -dc`.

Se recomienda actualizar la distribución de Linux con regularidad, incluso inmediatamente después de instalarla, para asegurarse de que tiene los paquetes más recientes. Windows no controla automáticamente esta actualización. Para actualizar la distribución, usa el comando: `sudo apt update && sudo apt upgrade`.  

> [!TIP]
> Considera la posibilidad de [instalar el nuevo Terminal Windows desde Microsoft Store](https://www.microsoft.com/store/apps/9n0dx20hk701) para habilitar varias pestañas (cambia rápidamente entre varias líneas de comando de Linux, el símbolo del sistema de Windows, PowerShell, la CLI de Azure, etc.), crear enlaces de teclado personalizados (teclas de método abreviado para abrir o cerrar pestañas, copiar y pegar, etc.), usar la característica de búsqueda y configurar temas personalizados (esquemas de colores, estilos y tamaños de fuente, imagen de fondo/desenfoque/transparencia). [Más información](/windows/terminal).

## <a name="set-up-visual-studio-code"></a>Configuración de Visual Studio Code

Aprovecha las ventajas de [IntelliSense](https://code.visualstudio.com/docs/editor/intellisense), la [detección de errores](https://code.visualstudio.com/docs/python/linting), la [compatibilidad con la depuración](https://code.visualstudio.com/docs/python/debugging), los [fragmentos de código](https://code.visualstudio.com/docs/editor/userdefinedsnippets) y las [pruebas unitarias](https://code.visualstudio.com/docs/python/unit-testing) mediante el uso de VS Code. VS Code se integra perfectamente con el Subsistema de Windows para Linux, lo que proporciona un [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal) para establecer un flujo de trabajo sin problemas entre el editor de código y la línea de comandos, además de admitir [GIT para el control de versiones](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support) con comandos de Git comunes (agregar, confirmar, enviar cambios e incorporar cambios) integrados directamente en la interfaz de usuario.

1. [Descarga e instala VS Code para Windows](https://code.visualstudio.com). VS Code también está disponible para Linux, pero el Subsistema de Windows para Linux no admite aplicaciones de GUI, por lo que deberemos instalarlo en Windows. Pero no te preocupes. Igualmente podrás realizar la integración con la línea de comandos y las herramientas de Linux mediante la extensión Remote-WSL.

2. Instala la [extensión Remote-WSL](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl) de VS Code. De este modo, podrás usar WSL como entorno de desarrollo integrado y la compatibilidad y las rutas se controlarán automáticamente. [Más información](https://code.visualstudio.com/docs/remote/remote-overview).

> [!IMPORTANT]
> Si ya tienes VS Code instalado, deberás asegurarte de que dispones de la [versión 1.35 de mayo ](https://code.visualstudio.com/updates/v1_35) o una posterior a fin de poder instalar la [extensión Remote-WSL](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl). No te recomendamos que uses WSL en VS Code sin la extensión Remote-WSL, ya que perderás la compatibilidad con Autocompletar, la depuración, la detección de errores, etc. Dato curioso: Esta extensión de WSL se instala en $HOME/.vscode-server/extensions.

## <a name="create-a-new-project"></a>Creación de un proyecto nuevo.

Vamos a crear un nuevo directorio de proyecto en nuestro sistema de archivos de Linux (Ubuntu) en el que trabajaremos con las aplicaciones y herramientas de Linux mediante VS Code.

1. Cierra VS Code y abre Ubuntu 18.04 (la línea de comandos de WSL). Para ello, ve al menú **Inicio** (icono de Windows de la esquina inferior izquierda) y escribe "Ubuntu 18.04".

2. En la línea de comandos de Ubuntu, navega a la ubicación en la que quieras colocar el proyecto y crea un directorio para este (`mkdir HelloWorld`).

![Terminal de Ubuntu](../images/ubuntu-terminal.png)

> [!TIP]
> Una cuestión importante que hay que recordar al usar el Subsistema de Windows para Linux (WSL) es que **ahora trabajas entre dos sistemas de archivos diferentes**: 1) el sistema de archivos de Windows y 2) el sistema de archivos de Linux (WSL), que es Ubuntu en nuestro ejemplo. Deberás prestar atención a la ubicación en la que instalas los paquetes y almacenas los archivos. Puedes instalar una versión de una herramienta o un paquete en el sistema de archivos de Windows y una versión completamente diferente en el sistema de archivos de Linux. La actualización de la herramienta en el sistema de archivos de Windows no tendrá ningún efecto en la herramienta del sistema de archivos de Linux, y viceversa. WSL monta las unidades fijas en el equipo en la carpeta `/mnt/<drive>` de la distribución de Linux. Por ejemplo, la unidad C: de Windows se monta en `/mnt/c/`. Puedes acceder a los archivos de Windows desde el terminal de Ubuntu y usar aplicaciones y herramientas de Linux en esos archivos, y viceversa. Te recomendamos que trabajes en el sistema de archivos de Linux para el desarrollo web de Python, dado que muchas de las herramientas web se han escrito originalmente para Linux y se han implementado en un entorno de producción de Linux. Además, así también se evita la combinación de la semántica del sistema de archivos (como el hecho de que Windows no distingue mayúsculas de minúsculas en los nombres de archivo). Dicho esto, WSL ahora admite el salto entre los sistemas de archivos de Windows y Linux, por lo que puedes hospedar tus archivos en cualquiera de ellos. [Más información](https://devblogs.microsoft.com/commandline/do-not-change-linux-files-using-windows-apps-and-tools/).

## <a name="install-python-pip-and-venv"></a>Instalación de Python, PIP y venv

Ubuntu 18.04 LTS se suministra con Python 3.6 ya instalado, pero no incluye algunos de los módulos que puedes esperar obtener con otras instalaciones de Python. Por este motivo, tendremos que instalar **PIP**, el administrador de paquetes estándar para Python, y **venv**, el módulo estándar que se usa para crear y administrar entornos virtuales ligeros.  

1. Confirma que Python3 ya está instalado. Para ello, abre el terminal de Ubuntu y escribe `python3 --version`. Se te debería devolver el número de versión de Python. Si necesitas actualizar la versión de Python, primero actualiza la versión de Ubuntu. Para ello, escribe `sudo apt update && sudo apt upgrade` y, luego, actualiza Python con `sudo apt upgrade python3`.

2. Para instalar **PIP**, escribe `sudo apt install python3-pip`. PIP te permite instalar y administrar paquetes adicionales que no forman parte de la biblioteca estándar de Python.

3. Para instalar **venv**, escribe `sudo apt install python3-venv`.

## <a name="create-a-virtual-environment"></a>Creación de un entorno virtual

El uso de entornos virtuales es un procedimiento recomendado para los proyectos de desarrollo de Python. Mediante la creación de un entorno virtual, puedes aislar las herramientas del proyecto y evitar conflictos de versiones con las herramientas de los demás proyectos. Por ejemplo, es posible que mantengas un proyecto web más antiguo que requiera el marco web de Django 1.2, pero, a continuación, te aparezca un nuevo proyecto emocionante con Django 2.2. Si actualizas Django globalmente, fuera de un entorno virtual, más adelante podrías encontrarte con algunos problemas de versiones. Además de evitar conflictos de versiones accidentales, los entornos virtuales permiten instalar y administrar paquetes sin privilegios administrativos.

1. Abre el terminal y, dentro de la carpeta del proyecto *HelloWorld*, usa el siguiente comando para crear un entorno virtual denominado **.venv**: `python3 -m venv .venv`.

2. Para activar el entorno virtual, escribe `source .venv/bin/activate`. Si funcionó, deberías ver **(.venv)** antes del símbolo del sistema. Ahora tienes un entorno independiente preparado para escribir código e instalar paquetes. Cuando hayas terminado con el entorno virtual, escribe el siguiente comando para desactivarlo: `deactivate`.

    ![Creación de un entorno virtual](../images/wsl-venv.png)

> [!TIP]
> Te recomendamos que crees el entorno virtual dentro del directorio en el que planeas tener el proyecto. Puesto que cada proyecto debe tener su propio directorio independiente, cada uno tendrá su propio entorno virtual, por lo que no hay necesidad de usar nombres únicos. Nuestra sugerencia es usar el nombre **.venv** para seguir la convención de Python. Algunas herramientas (como pipenv) también tienen como valor predeterminado este nombre si las instalas en el directorio del proyecto. Te recomendamos que no uses **.env**, dado que entra en conflicto con los archivos de definición de la variable de entorno. Por lo general, no se recomienda el uso de nombres que empiecen con un punto, ya que no necesitas que `ls` te recuerde de manera constante que el directorio existe. También te recomendamos que agregues **.venv** a tu archivo .gitignore. (Aquí tienes la [plantilla de gitignore predeterminada de GitHub para Python](https://github.com/github/gitignore/blob/50e42aa1064d004a5c99eaa72a2d8054a0d8de55/Python.gitignore#L99-L106) a modo de referencia). Para obtener más información sobre cómo trabajar con entornos virtuales en VS Code, consulta [Uso de entornos de Python en VS Code](https://code.visualstudio.com/docs/python/environments).

## <a name="open-a-wsl---remote-window"></a>Apertura de una ventana WSL-Remote

VS Code usa la extensión Remote-WSL (instalada anteriormente) para tratar el subsistema de Linux como un servidor remoto. Esto te permite usar WSL como entorno de desarrollo integrado. [Más información](https://code.visualstudio.com/docs/remote/wsl).

1. Para abrir la carpeta del proyecto en VS Code desde el terminal de Ubuntu, escribe `code .` (el "." indica a VS Code que abra la carpeta actual).

2. Aparecerá una alerta de seguridad de Windows Defender, en la que deberás seleccionar "Permitir acceso". Una vez que se abra VS Code, deberías ver el indicador de host de conexión remota en la esquina inferior izquierda, lo que te permitirá saber que realizas la edición en **WSL: Ubuntu-18.04**.

    ![Indicador de host de conexión remota de VS Code](../images/wsl-remote-extension.png)

3. Cierra el terminal de Ubuntu. Más adelante, usaremos el terminal de WSL integrado en VS Code.

4. Abre el terminal de WSL en VS Code. Para ello, presiona **Control + `** (mediante el carácter de tilde aguda) o selecciona **Ver** > **Terminal**. Se abrirá una línea de comandos de Bash (WSL) en la ruta de acceso de la carpeta del proyecto que creó en el terminal de Ubuntu.

    ![VS Code con el terminal de WSL](../images/vscode-bash-remote.png)

## <a name="install-the-microsoft-python-extension"></a>Instalación de la extensión de Microsoft Python

Tendrás que instalar las extensiones de VS Code para tu extensión de Remote-WSL. Las extensiones que ya estén instaladas localmente en VS Code no estarán disponibles automáticamente. [Más información](https://code.visualstudio.com/docs/remote/wsl#_managing-extensions).

1. Para abrir la ventana Extensiones de VS Code, escribe **Control + Mayús + X** (o usa el menú para desplazarte a **Ver** > **Extensiones**).

2. En el cuadro **Buscar extensiones en Marketplace** de la parte superior, escribe:  **Python**.

3. Busca la extensión **Python (ms-python.python) de Microsoft** y selecciona el botón **Instalar** de color verde.

4. Una vez finalizada la instalación de la extensión, deberás seleccionar el botón **Reload required** (Recarga necesaria) de color azul. Se volverá a cargar VS Code y se mostrara la sección **WSL: UBUNTU-18.04 - Installed** (WSL: UBUNTU-18.04 [instalado]) en la ventana VS Code Extensions (Extensiones de VS Code), que mostrará que ha instalado la extensión de Python.

## <a name="run-a-simple-python-program"></a>Ejecución de un programa de Python simple

Python es un lenguaje interpretado y admite distintos tipos de intérpretes (Python2, Anaconda, PyPy, etc.). VS Code debe tener como valor predeterminado el intérprete asociado al proyecto. Si tienes algún motivo para cambiarlo, selecciona el intérprete que se muestra actualmente en la barra azul de la parte inferior de la ventana de VS Code o abre la **Paleta de comandos** (Control + Mayús + P) y escribe el comando **Python: Select Interpreter**. Se mostrará una lista de los intérpretes de Python que tienes instalados actualmente. [Obtén más información sobre la configuración de entornos de Python](https://code.visualstudio.com/docs/python/environments).

Para probarlo, crearemos y ejecutaremos un programa de Python sencillo. Así mismo, nos aseguraremos de que hemos seleccionado el intérprete de Python correcto.

1. Para abrir la ventana VS Code File Explorer (Explorador de archivos de VS Code), escribe **Control + Mayús + E** (o usa el menú para navegar a **Ver** > **Extensiones**).

2. Si aún no está abierto, escribe **Control + Mayús + `** para abrir el terminal de WSL integrado y asegúrate de que la carpeta del proyecto de Python **HelloWorld** esté seleccionada.

3. Para crear un archivo de Python, escribe `touch test.py`. Deberías ver que el archivo que acabas de crear aparece en la ventana Explorador bajo las carpetas. venv y. vscode que ya se encuentran en el directorio del proyecto.

4. Selecciona el archivo **test.py** que acabas de crear en la ventana Explorador para abrirlo en VS Code. Dado que la extensión .py de nuestro nombre de archivo indica a VS Code que se trata de un archivo de Python, la extensión de Python que cargó anteriormente elegirá y cargará de manera automática un intérprete de Python que verás en la parte inferior de la ventana de VS Code.

    ![Selección del intérprete de Python en VS Code](../images/interpreterselection.gif)

5. Pega este código Python en el archivo test.py y, a continuación, guarda el archivo (Control + S): 

    ```python
    print("Hello World")
    ```

6. Para ejecutar el programa "Hola mundo" de Python que acabamos de crear, selecciona el archivo **test.py** en la ventana Explorador de VS Code y, a continuación, haz clic con el botón derecho en el archivo para mostrar un menú de opciones. Selecciona **Run Python File in Terminal** (Ejecutar archivo de Python en terminal). Como alternativa, en la ventana del terminal de WSL integrada, escribe `python test.py` para ejecutar el programa "Hola mundo". El intérprete de Python imprimirá "Hola mundo" en la ventana del terminal.

Enhorabuena. Ya lo tienes todo listo para crear y ejecutar programas de Python. Ahora vamos a intentar crear una aplicación Hola mundo con dos de los marcos web de Python más populares: Flask y Django.

## <a name="hello-world-tutorial-for-flask"></a>Tutorial de Hola mundo para Flask

[Flask](http://flask.pocoo.org/) es un marco de aplicación web para Python. En este breve tutorial, creará una pequeña aplicación "Hola mundo" de Flask con VS Code y WSL.

1. Para abrir Ubuntu 18.04 (la línea de comandos de WSL), ve al menú **Inicio** (icono de Windows de la esquina inferior izquierda) y escribe "Ubuntu 18.04".

2. Crea un directorio para el proyecto `mkdir HelloWorld-Flask` y, a continuación, escribe `cd HelloWorld-Flask` para entrar al directorio.

3. Crea un entorno virtual para instalar las herramientas del proyecto: `python3 -m venv .venv`.

4. Para abrir el proyecto **HelloWorld-Flask** en VS Code, escribe el comando `code .`.

5. En VS Code, abre el terminal de WSL integrado (también conocido como Bash). Para ello, escribe **Control + Mayús + `** (la carpeta del proyecto **HelloWorld-Flask** ya debería estar seleccionada). *Cierra la línea de comandos de Ubuntu, ya que a partir de ahora trabajaremos en el terminal de WSL integrado con VS Code.*

6. Activa el entorno virtual que creaste en el paso #3 mediante el terminal de Bash de VS Code: `source .venv/bin/activate`. Si funcionó, deberías ver (.venv) antes del símbolo del sistema.

7. Instala Flask en el entorno virtual escribiendo `python3 -m pip install flask`. Para que verificar se haya instalado, escribe `python3 -m flask --version`.

8. Crea un archivo nuevo para el código de Python: `touch app.py`.

9. Abre el archivo **app.py** en el explorador de archivos de VS Code (`Ctrl+Shift+E`y, a continuación, selecciona el archivo app.py). Se activará la extensión de Python para elegir un intérprete. El valor predeterminado debería ser **Python 3.6.8 64-bit ('.venv': venv)** . Ten en cuenta que también detecta tu entorno virtual.

    ![Entorno virtual activado](../images/virtual-environment.png)

10. En **app.py**, agrega el código para importar Flask y crea una instancia del objeto de Flask:

    ```python
    from flask import Flask
    app = Flask(__name__)
    ```

11. En **app.py**, agrega también una función que devuelva contenido. En este caso, una cadena simple. Usa el decorador **app.route** de Flask para asignar la ruta de la URL "/" a esa función:

    ```python
    @app.route("/")
    def home():
        return "Hello World! I'm using Flask."
    ```

    > [!TIP]
    > Puedes usar varios decoradores en la misma función, uno por línea, en función de cuántas rutas diferentes quieras asignar a la misma función.

12. Guarda el archivo **app.py** (**Control + S**).

13. En el terminal, escribe el siguiente comando para ejecutar la aplicación.

    ```python
    python3 -m flask run
    ```

    Se ejecutará el servidor de desarrollo de Flask. De forma predeterminada, el servidor de desarrollo busca el archivo **app.py**. Al ejecutar Flask, deberías ver un resultado similar al siguiente:

    ```bash
    (env) user@USER:/mnt/c/Projects/HelloWorld$ python3 -m flask run
     * Environment: production
       WARNING: This is a development server. Do not use it in a production deployment.
       Use a production WSGI server instead.
     * Debug mode: off
     * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
    ```

14. Abre el explorador web predeterminado en la página representada y presiona **Control + clic** en la dirección URL http://127.0.0.1:5000/ del terminal. Deberías ver el mensaje siguiente en el explorador:

    ![Hello, Flask!](../images/hello-flask.png)

15. Observa que, cuando visitas una dirección URL como "/", se muestra un mensaje en el terminal de depuración con la solicitud HTTP siguiente:

    ```bash
    127.0.0.1 - - [19/Jun/2019 13:36:56] "GET / HTTP/1.1" 200 -
    ```

16. Para detener la aplicación, usa **Control + C** en el terminal.

> [!TIP]
> Si quieres usar un nombre de archivo diferente que **app.py**, como **program.py**, define una variable de entorno denominada **FLASK_APP** y establece su valor en el archivo elegido. A continuación, el servidor de desarrollo de Flask usará el valor de **FLASK_APP** en lugar del archivo predeterminado **app.py**. Para obtener más información, ve la [documentación de la interfaz de línea de comandos de Flask](http://flask.pocoo.org/docs/1.0/cli/).

Enhorabuena, has creado una aplicación web de Flask con Visual Studio Code y el Subsistema de Windows para Linux. Para ver un tutorial más detallado sobre el uso de VS Code y Flask, consulta el [Tutorial de Flask en Visual Studio Code](https://code.visualstudio.com/docs/python/tutorial-flask).

## <a name="hello-world-tutorial-for-django"></a>Tutorial de Hola mundo para Django

[Django](https://www.djangoproject.com) es un marco de aplicación web para Python. En este breve tutorial, crearás una pequeña aplicación "Hola mundo" de Django con VS Code y WSL.

1. Para abrir Ubuntu 18.04 (la línea de comandos de WSL), ve al menú **Inicio** (icono de Windows de la esquina inferior izquierda) y escribe "Ubuntu 18.04".

2. Crea un directorio para el proyecto `mkdir HelloWorld-Django` y, a continuación, escribe `cd HelloWorld-Django` para entrar al directorio.

3. Crea un entorno virtual para instalar las herramientas del proyecto: `python3 -m venv .venv`.

4. Para abrir el proyecto **HelloWorld-DJango** en VS Code, escribe el comando `code .`.

5. En VS Code, abre el terminal de WSL integrado (también conocido como Bash). Para ello, escribe **Control + Mayús + `** (la carpeta del proyecto **HelloWorld-Django** ya debería estar seleccionada). *Cierra la línea de comandos de Ubuntu, ya que a partir de ahora trabajaremos en el terminal de WSL integrado con VS Code.*

6. Activa el entorno virtual que creaste en el paso #3 mediante el terminal de Bash de VS Code: `source .venv/bin/activate`. Si funcionó, deberías ver (.venv) antes del símbolo del sistema.

7. Instala Django en el entorno virtual con el comando `python3 -m pip install django`. Para que verificar se haya instalado, escribe `python3 -m django --version`.

8. A continuación, ejecuta el siguiente comando para crear el proyecto de Django:

    ```bash
    django-admin startproject web_project .
    ```

    El comando `startproject` presupone (mediante el uso de `.` al final) que la carpeta actual es la carpeta del proyecto, y crea lo siguiente en ella:

    - `manage.py`: La utilidad administrativa de la línea de comandos de Django para el proyecto. Debes ejecutar comandos administrativos para el proyecto mediante `python manage.py <command> [options]`.

    - Una subcarpeta denominada `web_project`, que contiene los archivos siguientes:
        - `__init__.py`: un archivo vacío que indica a Python que esta carpeta es un paquete de Python.
        - `wsgi.py`: un punto de entrada para los servidores web compatibles con WSGI que van a proporcionar servicios al proyecto. Normalmente, deberás dejar los archivos tal cual, ya que sirven de enlace para los servidores web de producción.
        - `settings.py`: contiene la configuración del proyecto de Django, que se modifica durante el desarrollo de una aplicación web.
        - `urls.py`: contiene una tabla de contenido para el proyecto de Django, que también se modifica durante el desarrollo.

9. Para verificar el proyecto de Django, inicia el servidor de desarrollo de Django con el comando `python3 manage.py runserver`. El servidor se ejecuta en el puerto predeterminado 8000 y deberías ver un resultado similar al siguiente en la ventana del terminal:

    ```output
    Performing system checks...

    System check identified no issues (0 silenced).

    June 20, 2019 - 22:57:59
    Django version 2.2.2, using settings 'web_project.settings'
    Starting development server at http://127.0.0.1:8000/
    Quit the server with CONTROL-C.
    ```

    Cuando ejecutes el servidor por primera vez, se creará una base de datos de SQLite predeterminada en el archivo `db.sqlite3`. Está diseñada con fines de desarrollo, pero se puede usar en producción para aplicaciones web de volumen bajo. Además, el servidor web integrado de Django *solo* está pensado para usarse con fines de desarrollo local. Sin embargo, cuando realiza una implementación en un host web, Django utiliza el servidor web del host en su lugar. El módulo `wsgi.py` del proyecto de Django se ocupa de servir como enlace a los servidores de producción.

    Si quieres utilizar un puerto distinto del predeterminado (8000), especifica el número de puerto en la línea de comandos, como `python3 manage.py runserver 5000`.

10. Presiona `Ctrl+click` en la dirección URL `http://127.0.0.1:8000/` de la ventana de salida del terminal para abrir el explorador predeterminado en esa dirección. Si Django está instalado correctamente y el proyecto es válido, verás una página predeterminada. En la ventana de salida del terminal de VS Code, también se muestra el registro del servidor.

11. Cuando hayas terminado, cierra la ventana del explorador y detén el servidor en VS Code con `Ctrl+C`, tal y como se indica en la ventana de salida del terminal.

12. Ahora, para crear una aplicación de Django, ejecuta el comando `startapp` de la utilidad administrativa en la carpeta del proyecto (donde reside `manage.py`):

    ```bash
    python3 manage.py startapp hello
    ```

    El comando crea una carpeta denominada `hello`, que contiene una serie de archivos de código y una subcarpeta. De estos, normalmente trabajarás con `views.py` (que contiene las funciones que definen las páginas de la aplicación web) y `models.py` (que contiene las clases que definen los objetos de datos). La utilidad administrativa de Django usa la carpeta `migrations` para administrar las versiones de base de datos, tal como se describe más adelante en este tutorial. También hay los archivos `apps.py` (configuración de la aplicación), `admin.py` (para crear una interfaz administrativa) y `tests.py` (para pruebas), que no se describen aquí.

13. Modifica `hello/views.py` para que coincida con el código siguiente. Se creará una vista única para la página principal de la aplicación:

    ```python
    from django.http import HttpResponse

    def home(request):
        return HttpResponse("Hello, Django!")
    ```

14. Crea un archivo, `hello/urls.py`, con el contenido que aparece a continuación. En el archivo `urls.py` se especifican los patrones para enrutar las distintas direcciones URL a sus vistas adecuadas. El código siguiente contiene una ruta para asignar la dirección URL raíz de la aplicación (`""`) a la función `views.home` que acabas de agregar a `hello/views.py`:

    ```python
    from django.urls import path
    from hello import views

    urlpatterns = [
        path("", views.home, name="home"),
    ]
    ```

15. La carpeta `web_project` también contiene un archivo `urls.py`, que es el lugar en el que se controla el enrutamiento de direcciones URL. Abre el archivo `web_project/urls.py` y modifícalo para que coincida con el código siguiente (si quieres, puedes conservar los comentarios instructivos). Este código extrae el archivo `hello/urls.py` de la aplicación mediante `django.urls.include`, que mantiene las rutas de la aplicación contenidas en la aplicación. Esta separación resulta útil cuando un proyecto contiene varias aplicaciones.

    ```python
    from django.contrib import admin
    from django.urls import include, path

    urlpatterns = [
        path("", include("hello.urls")),
    ]
    ```

16. Guarda todos los archivos modificados.

17. En el terminal de VS Code, ejecuta el servidor de desarrollo con `python3 manage.py runserver` y abre la URL `http://127.0.0.1:8000/` en un explorador para ver una página que represente "Hello, Django".

Enhorabuena, has creado una aplicación web de Django con VS Code y el Subsistema de Windows para Linux. Para ver un tutorial más detallado sobre el uso de VS Code y Django, consulta el [Tutorial de Django en Visual Studio Code](https://code.visualstudio.com/docs/python/tutorial-django).

## <a name="additional-resources"></a>Recursos adicionales

- [Tutorial de Python con VS Code](https://code.visualstudio.com/docs/python/python-tutorial): un tutorial introductorio para VS Code como entorno de Python, en el que principalmente se describe cómo editar, ejecutar y depurar código.
- [Compatibilidad de GIT en VS Code](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support): obtén información sobre cómo usar los aspectos básicos del control de versiones de GIT en VS Code.  
- [Información sobre las actualizaciones disponibles próximamente con WSL 2](/windows/wsl/wsl2-index): esta nueva versión cambia el modo en que las distribuciones de Linux interactúan con Windows, lo que aumenta el rendimiento del sistema de archivos y agrega compatibilidad completa con las llamadas del sistema.
- [Trabajo con varias distribuciones de Linux en Windows](/windows/wsl/wsl-config): obtén información sobre cómo administrar varias distribuciones de Linux diferentes en la máquina Windows.
