---
title: Desarrollo web con Python en Windows
description: Cómo empezar a usar Python para el desarrollo web en Windows, incluida la configuración de marcos como el matraz y Django.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Python, Windows 10, Microsoft, Python en Windows, Python web con WSL, aplicación Web de Python con el subsistema de Windows para Linux, desarrollo web de Python en Windows, aplicación de frasco en Windows, aplicación de Django en Windows, Python Web, frasco web dev en Windows, Django web dev en Windows, Windows web dev con Python, vs Code Python web dev y Remote WSL Extension, Ubuntu, WSL, venv, PIP, extensión de Microsoft Python, ejecutar Python en Windows, usar Python en Windows, compilar con Python en Windows
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: fa6da9f5151d9457aafd255c9d10c91e3d219cee
ms.sourcegitcommit: a28a32fff9d15ecf4a9d172cd0a04f4d993f9d76
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/12/2019
ms.locfileid: "68959084"
---
# <a name="get-started-using-python-for-web-development-on-windows"></a>Introducción al uso de Python para el desarrollo web en Windows

A continuación se ofrece una guía paso a paso para empezar a usar Python para el desarrollo web en Windows mediante el subsistema de Windows para Linux (WSL).

## <a name="set-up-your-development-environment"></a>Configurar el entorno de desarrollo

Se recomienda instalar Python en WSL al compilar aplicaciones Web. Muchos de los tutoriales e instrucciones para el desarrollo web de Python se escriben para los usuarios de Linux y usan herramientas de instalación y empaquetado basadas en Linux. La mayoría de las aplicaciones web también se implementan en Linux, por lo que se garantiza la coherencia entre los entornos de desarrollo y de producción.

Si usa Python para algo que no sea el desarrollo web, se recomienda instalar Python directamente en Windows 10 mediante el Microsoft Store. WSL no admite aplicaciones o escritorios de GUI (como PyGame, GNOME, KDE, etc.). Instale y use Python directamente en Windows para estos casos. Si no está familiarizado con Python, consulte nuestra guía: [Introducción al uso de Python en Windows para principiantes](./python-for-education.md). Si está interesado en automatizar las tareas comunes en el sistema operativo, consulte nuestra guía: [Introducción al uso de Python en Windows para scripting y automatización](./python-for-scripting.md). En algunos escenarios avanzados, es posible que desee considerar la posibilidad de descargar una versión específica de Python directamente de [Python.org](https://www.python.org/downloads/windows/) o considerar la posibilidad de instalar una [alternativa](https://www.python.org/download/alternatives), como Anaconda, Jython, PyPy, WinPython, IronPython, etc. Solo se recomienda si es un programador de Python más avanzado con un motivo específico para elegir una implementación alternativa.

## <a name="enable-windows-subsystem-for-linux"></a>Habilitar el subsistema de Windows para Linux

WSL permite ejecutar un entorno de GNU/Linux, que incluye la mayoría de las herramientas de línea de comandos, utilidades y aplicaciones, directamente en Windows, sin modificar y completamente integradas con el sistema de archivos de Windows y herramientas favoritas como Visual Studio Code. Antes de habilitar WSL, compruebe que tiene la [versión más reciente de Windows 10](https://www.microsoft.com/software-download/windows10).

Para habilitar WSL en el equipo, debe hacer lo siguiente:

1. Vaya al menú **Inicio** (icono de Windows inferior izquierdo), escriba "activar o desactivar las características de Windows" y seleccione el vínculo al **Panel de control** para abrir el menú emergente **características de Windows** . Busque "subsistema de Windows para Linux" en la lista y active la casilla para activar la característica.

2. Reinicie el equipo cuando se le solicite.

## <a name="install-a-linux-distribution"></a>Instalación de una distribución de Linux

Hay varias distribuciones de Linux disponibles para ejecutarse en WSL. Puede buscar e instalar su favorito en el Microsoft Store. Se recomienda comenzar con [Ubuntu 18,04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q) , ya que es actual, popular y bien compatible.

1. Abra este vínculo de [Ubuntu 18,04 LTS](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q) , abra el Microsoft Store y seleccione **obtener**. *(Se trata de una descarga bastante grande y puede tardar algún tiempo en instalarse).*

2. Una vez finalizada la descarga, seleccione **iniciar** desde el Microsoft Store o iniciar escribiendo "Ubuntu 18,04 LTS" en el menú **Inicio** .

3. Al ejecutar la distribución por primera vez, se le pedirá que cree un nombre y una contraseña de cuenta. Después, iniciará sesión automáticamente como este usuario de forma predeterminada. Puede elegir cualquier nombre de usuario y contraseña. No tienen ninguna relación con el nombre de usuario de Windows.

Para comprobar la distribución de Linux que está usando actualmente, escriba: `lsb_release -d`. Para actualizar la distribución de Ubuntu, use `sudo apt update && sudo apt upgrade`:. Se recomienda actualizar periódicamente para asegurarse de que tiene los paquetes más recientes. Windows no controla automáticamente esta actualización. Para obtener vínculos a otras distribuciones de Linux disponibles en el Microsoft Store, métodos de instalación alternativos o solución de problemas, consulte la [Guía de instalación del subsistema de Windows para Linux para Windows 10](https://docs.microsoft.com/windows/wsl/install-win10).

## <a name="set-up-visual-studio-code"></a>Configurar Visual Studio Code

Aproveche las ventajas de [IntelliSense](https://code.visualstudio.com/docs/editor/intellisense), la [detección de errores](https://code.visualstudio.com/docs/python/linting), la [compatibilidad con depuración](https://code.visualstudio.com/docs/python/debugging), [fragmentos de código](https://code.visualstudio.com/docs/editor/userdefinedsnippets) y [pruebas unitarias](https://code.visualstudio.com/docs/python/unit-testing) mediante VS Code. VS Code se integra perfectamente con el subsistema de Windows para Linux, lo que proporciona un [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal) para establecer un flujo de trabajo sin problemas entre el editor de código y la línea de comandos, además de admitir [git para el control de versiones](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support) con git común los comandos (agregar, confirmar, insertar, extraer) se integran directamente en la interfaz de usuario.

1. [Descargue e instale vs code para Windows](https://code.visualstudio.com). VS Code también está disponible para Linux, pero el subsistema de Windows para Linux no admite aplicaciones de GUI, por lo que es necesario instalarla en Windows. No se preocupe, todavía podrá integrarse con la línea de comandos y las herramientas de Linux mediante la extensión Remote-WSL.

2. Instale la [extensión Remote-WSL](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl) en vs Code. Esto le permite usar WSL como entorno de desarrollo integrado y controlará la compatibilidad y las cosas. [Más información](https://code.visualstudio.com/docs/remote/remote-overview).

> [!IMPORTANT]
> Si ya tiene VS Code instalado, debe asegurarse de que tiene la [versión 1,35](https://code.visualstudio.com/updates/v1_35) o posterior para poder instalar la [extensión Remote-WSL](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl). No se recomienda el uso de WSL en VS Code sin la extensión Remote-WSL, ya que se perderá la compatibilidad con Autocompletar, depuración, detección de errores, etc. Hecho divertido: Esta extensión WSL se instala en $HOME/.vscode-Server/Extensions.

## <a name="create-a-new-project"></a>Creación de un proyecto nuevo.

Vamos a crear un nuevo directorio de proyecto en nuestro sistema de archivos de Linux (Ubuntu) en el que trabajaremos con las aplicaciones y herramientas de Linux mediante VS Code.

1. Cierre VS Code y abra Ubuntu 18,04 (la línea de comandos de WSL). para ello, vaya al menú **Inicio** (icono de Windows inferior izquierdo) y escriba: "Ubuntu 18,04".

2. En la línea de comandos de Ubuntu, navegue a la ubicación en la que desea colocar el proyecto y cree un directorio `mkdir HelloWorld`para él:.

![Terminal Ubuntu](../../images/ubuntu-terminal.png)

> [!TIP]
> Una cuestión importante que hay que recordar al usar el subsistema de Windows para Linux (WSL) es que **ahora está trabajando entre dos sistemas de archivos diferentes**: 1) el sistema de archivos de Windows y 2) el sistema de archivos de Linux (WSL), que es Ubuntu en nuestro ejemplo. Deberá prestar atención a la ubicación en la que se instalan los paquetes y se almacenan los archivos. Puede instalar una versión de una herramienta o un paquete en el sistema de archivos de Windows y una versión completamente diferente en el sistema de archivos de Linux. La actualización de la herramienta en el sistema de archivos de Windows no tendrá ningún efecto en la herramienta en el sistema de archivos de Linux y viceversa. WSL monta las unidades fijas en el equipo en la carpeta<drive> /mnt/de la distribución de Linux. Por ejemplo, la unidad de Windows C: está montada en `/mnt/c/`. Puede tener acceso a los archivos de Windows desde el terminal Ubuntu y usar aplicaciones y herramientas de Linux en esos archivos y viceversa. Se recomienda trabajar en el sistema de archivos de Linux para el desarrollo web de Python dado que muchas de las herramientas web se han escrito originalmente para Linux y se han implementado en un entorno de producción de Linux. También evita la combinación de la semántica del sistema de archivos (como Windows no distingue entre mayúsculas y minúsculas con respecto a los nombres de archivo). Dicho esto, WSL ahora admite el salto entre los sistemas de archivos de Windows y Linux, por lo que puede hospedar sus archivos en cualquiera de ellos. [Más información](https://devblogs.microsoft.com/commandline/do-not-change-linux-files-using-windows-apps-and-tools/). También nos complace compartir que [WSL2 está disponible próximamente para Windows](https://devblogs.microsoft.com/commandline/wsl-2-is-now-available-in-windows-insiders/) y ofrecerá algunas mejoras excelentes. Puede [probarlo ahora en la compilación 18917 de Windows](https://docs.microsoft.com/windows/wsl/wsl2-install)Insider.

## <a name="install-python-pip-and-venv"></a>Instalación de Python, PIP y venv

Ubuntu 18,04 LTS incluye Python 3,6 ya instalado, pero no incluye algunos de los módulos que puede esperar obtener con otras instalaciones de Python. Todavía tendremos que instalar **PIP**, el administrador de paquetes estándar para Python y **venv**, el módulo estándar que se usa para crear y administrar entornos virtuales ligeros.  

1. Confirme que Python3 ya está instalado; para ello, abra el terminal Ubuntu `python3 --version`y escriba:. Debe devolver el número de versión de Python. Si necesita actualizar la versión de Python, actualice primero la versión de Ubuntu; para ello, `sudo apt update && sudo apt upgrade`escriba: y luego actualice `sudo apt upgrade python3`Python con.

2. Instale **PIP** escribiendo: `sudo apt install python3-pip`. PIP permite instalar y administrar paquetes adicionales que no forman parte de la biblioteca estándar de Python.

3. Para instalar **venv** , escriba `sudo apt install python3-venv`:.

## <a name="create-a-virtual-environment"></a>Crear un entorno virtual

El uso de entornos virtuales es un procedimiento recomendado para los proyectos de desarrollo de Python. Mediante la creación de un entorno virtual, puede aislar las herramientas del proyecto y evitar conflictos de versiones con herramientas para los demás proyectos. Por ejemplo, puede que esté manteniendo un proyecto web más antiguo que requiera el marco Web de Django 1,2, pero, a continuación, se incluye un nuevo proyecto emocionante con Django 2,2. Si actualiza Django globalmente, fuera de un entorno virtual, podría encontrar algunos problemas de versión más adelante. Además de evitar conflictos de versiones accidentales, los entornos virtuales permiten instalar y administrar paquetes sin privilegios administrativos.

1. Abra el terminal y, dentro de la carpeta del proyecto *HelloWorld* , use el siguiente comando para crear un entorno virtual denominado **. venv**: `python3 -m venv .venv`.

2. Para activar el entorno virtual, escriba: `source .venv/bin/activate`. Si funcionó, debería ver **(. venv)** antes del símbolo del sistema. Ahora tiene un entorno independiente preparado para escribir código e instalar paquetes. Cuando haya terminado con el entorno virtual, escriba el siguiente comando para desactivarlo: `deactivate`.

    ![Crear un entorno virtual](../../images/wsl-venv.png)

> [!TIP]
> Se recomienda crear el entorno virtual dentro del directorio en el que planea tener el proyecto. Puesto que cada proyecto debe tener su propio directorio independiente, cada uno tendrá su propio entorno virtual, por lo que no hay necesidad de nombres únicos. Nuestra sugerencia es usar name **. venv** para seguir la Convención de Python. Algunas herramientas (como pipenv) también tienen como valor predeterminado este nombre si se instala en el directorio del proyecto. No quiere usar **. env** como entra en conflicto con los archivos de definición de variables de entorno. Por lo general, no se recomiendan los nombres que no conducen a un punto `ls` , ya que no es necesario recordar que el directorio existe. También se recomienda agregar **. venv** al archivo. gitignore. (Esta es [la plantilla gitignore predeterminada de github para Python](https://github.com/github/gitignore/blob/50e42aa1064d004a5c99eaa72a2d8054a0d8de55/Python.gitignore#L99-L106) como referencia). Para obtener más información sobre cómo trabajar con entornos virtuales en VS Code, consulte [uso de entornos de Python en vs Code](https://code.visualstudio.com/docs/python/environments).

## <a name="open-a-wsl---remote-window"></a>Abrir una ventana WSL-Remote

VS Code usa la extensión Remote-WSL (instalada anteriormente) para tratar el subsistema de Linux como servidor remoto. Esto le permite usar WSL como entorno de desarrollo integrado. [Más información](https://code.visualstudio.com/docs/remote/wsl). 

1. Abra la carpeta del proyecto en vs Code desde el terminal Ubuntu; para `code .` ello, escriba: (el "." indica a vs Code que abra la carpeta actual).

2. Aparecerá una alerta de seguridad en Windows Defender y seleccione "permitir acceso". Una vez que se abre vs Code, debería ver el indicador de host de conexión remota, en la esquina inferior izquierda, que le permite saber que **está editando en WSL: Ubuntu-18,04**.

    ![VS Code indicador de host de conexión remota](../../images/wsl-remote-extension.png)

3. Cierre el terminal Ubuntu. Al avanzar, usaremos el terminal WSL integrado en VS Code.

4. Abra el terminal WSL en vs Code presionando **Ctrl + '** (mediante el carácter de acento grave) o seleccionando **Ver** > **terminal**. Se abrirá una línea de comandos Bash (WSL) abierta a la ruta de acceso de la carpeta del proyecto que creó en el terminal Ubuntu.

    ![VS Code con terminal WSL](../../images/vscode-bash-remote.png)

## <a name="install-the-microsoft-python-extension"></a>Instalación de la extensión de Microsoft Python

Tendrá que instalar las extensiones de VS Code para la WSL remota. Las extensiones ya instaladas localmente en VS Code no estarán disponibles automáticamente. [Más información](https://code.visualstudio.com/docs/remote/wsl#_managing-extensions).

1. Abra la ventana extensiones de vs Code escribiendo **Ctrl + Mayús + X** (o use el menú para navegar a **Ver** > **extensiones**).

2. En el cuadro principales **extensiones de búsqueda en Marketplace** , escriba:  **Python**.

3. Busque la extensión **Python (MS-Python. Python) de Microsoft** y seleccione el botón de **instalación** verde.

4. Una vez finalizada la instalación de la extensión, deberá seleccionar el botón azul **recarga necesaria** . Se volverá a cargar vs Code y se mostrará **un WSL: Ubuntu-18,04: sección** instalada en la ventana extensiones de vs Code que muestra que ha instalado la extensión de Python.

## <a name="run-a-simple-python-program"></a>Ejecutar un programa de Python sencillo

Python es un lenguaje interpretado y admite distintos tipos de intérpretes (python2, Anaconda, PyPy, etc.). VS Code debe tener como valor predeterminado el intérprete asociado al proyecto. Si tiene un motivo para cambiarlo, seleccione el intérprete que se muestra actualmente en la barra azul en la parte inferior de la ventana de vs Code o abra la **paleta de comandos** (Ctrl + Mayús + P **) y escriba el comando Python: Seleccione intérprete**. Se mostrará una lista de los intérpretes de Python que tiene instalados actualmente. [Más información sobre la configuración de entornos de Python](https://code.visualstudio.com/docs/python/environments).

Vamos a crear y ejecutar un programa de Python sencillo como prueba y asegurarse de que se ha seleccionado el intérprete de Python correcto.

1. Abra la ventana del explorador de archivos de vs Code escribiendo **Ctrl + Mayús + E** (o use el menú para ir al**Explorador**de **vistas** > ).

2. Si aún no está abierto, abra el terminal de WSL integrado presionando **Ctrl + Mayús + '** y asegúrese de que la carpeta del proyecto **HelloWorld** Python está seleccionada.

3. Cree un archivo de Python escribiendo: `touch test.py`. Debería ver que el archivo que acaba de crear aparece en la ventana del explorador bajo las carpetas. venv y. vscode que ya se encuentra en el directorio del proyecto.

4. Seleccione el archivo **Test.py** que acaba de crear en la ventana del explorador para abrirlo en vs Code. Dado que. py en nuestro nombre de archivo indica VS Code que se trata de un archivo de Python, la extensión de Python que cargó anteriormente elegirá y cargará automáticamente un intérprete de Python que verá en la parte inferior de la ventana de VS Code.

    ![Seleccione el intérprete de Python en VS Code](../../images/interpreterselection.gif)

5. Pegue este código Python en el archivo test.py y, a continuación, guarde el archivo (Ctrl + S): 

    ```python
    print("Hello World")
    ```

6. Para ejecutar el programa "Hola mundo" de Python que acabamos de crear, seleccione el archivo **Test.py** en la ventana explorador de vs Code y, a continuación, haga clic con el botón derecho en el archivo para mostrar un menú de opciones. Seleccione **Ejecutar archivo de Python en terminal**. Como alternativa, en la ventana de terminal WSL integrada, escriba `python test.py` : para ejecutar el programa "Hola mundo". El intérprete de Python imprimirá "Hola mundo" en la ventana de terminal.

¡Enhorabuena! Ya está todo listo para crear y ejecutar programas de Python. Ahora vamos a intentar crear una aplicación Hola mundo con dos de los marcos Web de Python más populares: Frasco y Django.

## <a name="hello-world-tutorial-for-flask"></a>Tutorial de Hola mundo para el frasco

[Frasco](http://flask.pocoo.org/) es un marco de aplicación web para Python. En este breve tutorial, creará una pequeña aplicación de frasco "Hola mundo" con VS Code y WSL.

1. Abra Ubuntu 18,04 (la línea de comandos de WSL). para ello, vaya al menú **Inicio** (icono de Windows inferior izquierdo) y escriba: "Ubuntu 18,04".

2. Cree un directorio para el proyecto: `mkdir HelloWorld-Flask`y, `cd HelloWorld-Flask` a continuación, para especificar el directorio.

3. Cree un entorno virtual para instalar las herramientas de proyecto:`python3 -m venv .venv`

4. Abra el proyecto **HelloWorld-frasco** en vs Code escribiendo el comando:`code .`

5. Dentro de VS Code, abra el terminal de WSL integrado (también conocido como bash) escribiendo **Ctrl + Mayús + '** (la carpeta de proyecto **HelloWorld-frasco** ya debe estar seleccionada). *Cierre la línea de comandos de Ubuntu a medida que trabajamos en el terminal WSL integrado con VS Code avanzando.*

6. Active el entorno virtual que creó en el paso #3 mediante el terminal de bash en VS Code `source .venv/bin/activate`:. Si funcionó, debería ver (. venv) antes del símbolo del sistema.

7. Para instalar el frasco en el entorno virtual, `python3 -m pip install flask`escriba:. Compruebe que se ha instalado; para ello `python3 -m flask --version`, escriba:.

8. Cree un archivo nuevo para el código de Python:`touch app.py`

9. Abra el archivo **app.py** en el explorador de archivos de`Ctrl+Shift+E`vs Code (y seleccione el archivo app.py). Esto activará la extensión de Python para elegir un intérprete. Debe tener como valor predeterminado **Python 3.6.8 64 bits ('. venv ': venv)** . Tenga en cuenta que también ha detectado su entorno virtual.

    ![Entorno virtual activado](../../images/virtual-environment.png)

10. En **app.py**, agregue el código para importar el frasco y cree una instancia del objeto de frasco:

    ```python
    from flask import Flask
    app = Flask(__name__)
    ```

11. También en **app.py**, agregue una función que devuelva contenido, en este caso una cadena simple. Use el decorador **app. Route** del frasco para asignar la ruta de la dirección URL "/" a esa función:

    ```python
    @app.route("/")
    def home():
        return "Hello World! I'm using Flask."
    ```

    > [!TIP]
    > Puede usar varios decoradores en la misma función, uno por línea, en función de cuántas rutas diferentes desee asignar a la misma función.

12. Guarde el archivo **app.py** (**Ctrl + S**).

13. En el terminal, ejecute la aplicación escribiendo el siguiente comando:

    ```python
    python3 -m flask run
    ```

    Esto ejecuta el servidor de desarrollo de frasco. El servidor de desarrollo busca **app.py** de forma predeterminada. Al ejecutar el frasco, debería ver una salida similar a la siguiente:

    ```bash
    (env) user@USER:/mnt/c/Projects/HelloWorld$ python3 -m flask run
     * Environment: production
       WARNING: This is a development server. Do not use it in a production deployment.
       Use a production WSGI server instead.
     * Debug mode: off
     * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
    ```

14. Abra el explorador Web predeterminado en la página representada, **Ctrl + clic** en la http://127.0.0.1:5000/ dirección URL del terminal. Debería ver el mensaje siguiente en el explorador:

    ![¡ Hola, frasco!](../../images/hello-flask.png)

15. Observe que cuando visita una dirección URL como "/", aparece un mensaje en el terminal de depuración que muestra la solicitud HTTP:

    ```bash
    127.0.0.1 - - [19/Jun/2019 13:36:56] "GET / HTTP/1.1" 200 -
    ```

16. Detenga la aplicación con **Ctrl + C** en el terminal.

> [!TIP]
> Si desea usar un nombre de archivo diferente que **app.py**, como **Program.py**, defina una variable de entorno denominada **FLASK_APP** y establezca su valor en el archivo elegido. A continuación, el servidor de desarrollo del frasco usa el valor de **FLASK_APP** en lugar del archivo **app.py**predeterminado. Para obtener más información, consulte [la documentación de la interfaz de línea de comandos del frasco](http://flask.pocoo.org/docs/1.0/cli/).

Enhorabuena, ha creado una aplicación Web de frasco con Visual Studio Code y el subsistema de Windows para Linux. Para obtener un tutorial más detallado sobre el uso de VS Code y el frasco, consulte [tutorial de frasco en Visual Studio Code](https://code.visualstudio.com/docs/python/tutorial-flask).

## <a name="hello-world-tutorial-for-django"></a>Tutorial de Hola mundo para Django

[Django](https://www.djangoproject.com) es un marco de aplicación web para Python. En este breve tutorial, creará una pequeña aplicación de "Hola mundo" Django con VS Code y WSL.

1. Abra Ubuntu 18,04 (la línea de comandos de WSL). para ello, vaya al menú **Inicio** (icono de Windows inferior izquierdo) y escriba: "Ubuntu 18,04".

2. Cree un directorio para el proyecto: `mkdir HelloWorld-Django`y, `cd HelloWorld-Django` a continuación, para especificar el directorio.

3. Cree un entorno virtual para instalar las herramientas de proyecto:`python3 -m venv .venv`

4. Abra el proyecto **HelloWorld-DJango** en vs Code escribiendo el comando:`code .`

5. Dentro de VS Code, abra el terminal de WSL integrado (también conocido como bash) escribiendo **Ctrl + Mayús + '** (la carpeta de proyecto **HelloWorld-Django** ya debe estar seleccionada). *Cierre la línea de comandos de Ubuntu a medida que trabajamos en el terminal WSL integrado con VS Code avanzando.*

6. Active el entorno virtual que creó en el paso #3 mediante el terminal de bash en VS Code `source .venv/bin/activate`:. Si funcionó, debería ver (. venv) antes del símbolo del sistema.

7. Instale Django en el entorno virtual con el comando: `python3 -m pip install django`. Compruebe que se ha instalado; para ello `python3 -m django --version`, escriba:.

8. A continuación, ejecute el siguiente comando para crear el proyecto Django:

    ```bash
    django-admin startproject web_project .
    ```

    El `startproject` comando asume (mediante el uso `.` de al final) que la carpeta actual es la carpeta del proyecto y crea lo siguiente en ella:

    - `manage.py`: La utilidad administrativa de línea de comandos Django para el proyecto. Ejecute comandos administrativos para el proyecto mediante `python manage.py <command> [options]`.

    - Una subcarpeta denominada `web_project`, que contiene los archivos siguientes:
        - `__init__.py`: un archivo vacío que indica a Python que esta carpeta es un paquete de Python.
        - `wsgi.py`: un punto de entrada para que los servidores Web compatibles con WSGI sirvan al proyecto. Normalmente se deja este archivo tal cual, ya que proporciona los enlaces para los servidores Web de producción.
        - `settings.py`: contiene la configuración para el proyecto Django, que se modifica durante el desarrollo de una aplicación Web.
        - `urls.py`: contiene una tabla de contenido para el proyecto Django, que también se modifica en el curso del desarrollo.

9. Para comprobar el proyecto Django, inicie el servidor de desarrollo de Django con `python3 manage.py runserver`el comando. El servidor se ejecuta en el puerto predeterminado 8000 y debería ver una salida similar a la siguiente en la ventana de terminal:

    ```output
    Performing system checks...

    System check identified no issues (0 silenced).

    June 20, 2019 - 22:57:59
    Django version 2.2.2, using settings 'web_project.settings'
    Starting development server at http://127.0.0.1:8000/
    Quit the server with CONTROL-C.
    ```

    Al ejecutar el servidor la primera vez, se crea una base de datos de SQLite predeterminada en `db.sqlite3`el archivo, que está pensada para fines de desarrollo, pero se puede usar en producción para aplicaciones Web de bajo volumen. Además, el servidor Web integrado de Django está diseñado *únicamente* para fines de desarrollo local. Sin embargo, cuando se implementa en un host Web, Django usa el servidor Web del host en su lugar. El `wsgi.py` módulo del proyecto Django se encarga del enlace a los servidores de producción.

    Si desea utilizar un puerto distinto del predeterminado 8000, especifique el número de puerto en la línea de comandos, `python3 manage.py runserver 5000`como.

10. `Ctrl+click`la `http://127.0.0.1:8000/` dirección URL de la ventana salida de terminal para abrir el explorador predeterminado en esa dirección. Si Django está instalado correctamente y el proyecto es válido, verá una página predeterminada. En la VS Code ventana salida de terminal también se muestra el registro del servidor.

11. Cuando haya terminado, cierre la ventana del explorador y detenga el servidor en vs Code utilizando `Ctrl+C` como se indica en la ventana salida de terminal.

12. Ahora, para crear una aplicación de Django, ejecute el comando de `startapp` la utilidad administrativa en la carpeta del `manage.py` proyecto (donde reside):

    ```bash
    python3 manage.py startapp hello
    ```

    El comando crea una carpeta denominada `hello` que contiene una serie de archivos de código y una subcarpeta. De estos, suele trabajar con `views.py` (que contiene las funciones que definen las páginas en la aplicación web) y `models.py` (que contiene las clases que definen los objetos de datos). La `migrations` utilidad administrativa de Django usa la carpeta para administrar las versiones de base de datos como se describe más adelante en este tutorial. También hay archivos `apps.py` (configuración de la aplicación), `admin.py` (para crear una interfaz administrativa) y `tests.py` (para pruebas), que no se explican aquí.

13. Modifique `hello/views.py` para que coincida con el código siguiente, que crea una única vista para la Página principal de la aplicación:

    ```python
    from django.http import HttpResponse

    def home(request):
        return HttpResponse("Hello, Django!")
    ```

14. Cree un archivo, `hello/urls.py`, con el contenido que aparece a continuación. El `urls.py` archivo es donde se especifican los patrones para enrutar las distintas direcciones URL a sus vistas adecuadas. El código siguiente contiene una ruta para asignar la dirección URL raíz de la`""`aplicación () `views.home` a la función que acaba de `hello/views.py`agregar a:

    ```python
    from django.urls import path
    from hello import views

    urlpatterns = [
        path("", views.home, name="home"),
    ]
    ```

15. La `web_project` carpeta también contiene un `urls.py` archivo, que es el lugar en el que se controla el enrutamiento de direcciones URL. Ábralo `web_project/urls.py` y modifíquelo para que coincida con el código siguiente (puede conservar los comentarios instructivos si lo desea). Este código extrae el `hello/urls.py` uso `django.urls.include`de la aplicación, que mantiene las rutas de la aplicación contenidas en la aplicación. Esta separación resulta útil cuando un proyecto contiene varias aplicaciones.

    ```python
    from django.contrib import admin
    from django.urls import include, path

    urlpatterns = [
        path("", include("hello.urls")),
    ]
    ```

16. Guarde todos los archivos modificados.

17. En el terminal de vs Code, ejecute el servidor de `python manage.py runserver` desarrollo con y abra un `http://127.0.0.1:8000/` explorador para ver una página que representa "Hello, Django".

Enhorabuena, ha creado una aplicación Web de Django con VS Code y el subsistema de Windows para Linux. Para obtener un tutorial más detallado sobre el uso de VS Code y Django, consulte el [tutorial de Django en Visual Studio Code](https://code.visualstudio.com/docs/python/tutorial-django).

## <a name="additional-resources"></a>Recursos adicionales

- [Tutorial de Python con vs Code](https://code.visualstudio.com/docs/python/python-tutorial): Un tutorial introductorio para VS Code como un entorno de Python, principalmente cómo editar, ejecutar y depurar código.
- [Compatibilidad de Git en vs Code](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support): Obtenga información sobre cómo usar los aspectos básicos del control de versiones de Git en VS Code.  
- [Obtenga información acerca de las actualizaciones disponibles próximamente con WSL 2!](https://docs.microsoft.com/windows/wsl/wsl2-index): Esta nueva versión cambia el modo en que las distribuciones de Linux interactúan con Windows, lo que aumenta el rendimiento del sistema de archivos y agrega compatibilidad completa con llamadas del sistema.
- [Trabajar con varias distribuciones de Linux en Windows](https://docs.microsoft.com/windows/wsl/wsl-config): Obtenga información sobre cómo administrar varias distribuciones de Linux diferentes en el equipo Windows.
