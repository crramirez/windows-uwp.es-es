---
title: Scripting y automatización con Python en Windows
description: Cómo empezar a usar Python para el scripting, la automatización y la administración de sistemas en Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: python, windows 10, microsoft, administración del sistema python, automatización de archivos python, scripts de python en windows, configurar python en windows, entorno para desarrolladores de python en windows, python con powershell, scripts de python para tareas del sistema de archivos
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 3f8a17de8121fed27e69442d5560f702a04c8e42
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314869"
---
# <a name="get-started-using-python-on-windows-for-scripting-and-automation"></a>Introducción al uso de Python en Windows para el scripting y la automatización

A continuación, te proporcionamos una guía detallada para configurar el entorno para desarrolladores y empezar a usar Python para crear scripts y automatizar las operaciones del sistema de archivos en Windows.

> [!NOTE]
> Este artículo trata sobre la configuración del entorno para usar algunas de las bibliotecas útiles de Python que pueden automatizar las tareas entre plataformas, como buscar en el sistema de archivos, acceder a Internet, analizar tipos de archivo, etc., desde un enfoque centrado en Windows. En el caso de las operaciones específicas de Windows, consulta [ctypes](https://docs.python.org/3/library/ctypes.html), una biblioteca de funciones externas compatible con C para Python, [winreg](https://docs.python.org/3/library/winreg.html), funciones que exponen la API de registro de Windows a Python, y [Python/WinRT](https://pypi.org/project/winrt/), que habilita el acceso a las API de Windows Runtime desde Python.

## <a name="set-up-your-development-environment"></a>Configurar el entorno de desarrollo

Al usar Python para escribir scripts que realicen operaciones del sistema de archivos, te recomendamos que [instales Python desde Microsoft Store](https://www.microsoft.com/en-us/p/python-37/9nj46sx7x90p?activetab=pivot:overviewtab). La instalación a través de Microsoft Store utiliza el intérprete de Python3 básico, pero controla el establecimiento de la configuración del valor PATH para el usuario actual (lo que evita la necesidad de contar con acceso de administrador) y, además, proporciona actualizaciones automáticas.

Si usas Python para el **desarrollo web** en Windows, te recomendamos que definas una configuración diferente con el Subsistema de Windows para Linux. Busca un tutorial en nuestra guía: [Introducción al uso de Python para el desarrollo web en Windows](./web-frameworks.md). Si no estás familiarizado con Python, prueba nuestra guía: [Introducción al uso de Python en Windows para principiantes](./beginners.md). En algunos escenarios avanzados (por ejemplo, si necesitas acceder a los archivos instalados de Python o modificarlos, hacer copias en archivos binarios o usar archivos DLL de Python directamente), es posible que quieras considerar la posibilidad de descargar una versión específica de Python directamente desde [python.org](https://www.python.org/downloads/) o una [alternativa](https://www.python.org/download/alternatives), como Anaconda, Jython, PyPy, WinPython, IronPython, etc. Solo te lo recomendamos si eres un programador de Python más avanzado y tienes un motivo específico para elegir una implementación alternativa.

## <a name="install-python"></a>Instalar Python

Para instalar Python con Microsoft Store:

1. Ve al menú **Inicio** (icono de Windows de la esquina inferior izquierda), escribe "Microsoft Store" y selecciona el vínculo para abrir Store.

2. Una vez que lo hayas abierto, selecciona **Buscar** en el menú superior derecho y escribe "Python". Abre "Python 3.7" en los resultados de las aplicaciones. Selecciona **Obtener**.

3. Una vez que Python haya completado el proceso de descarga e instalación, abre Windows PowerShell mediante el menú **Inicio** (icono de Windows de la esquina inferior izquierda). Cuando PowerShell esté abierto, escribe `Python --version` para confirmar que Python3 se ha instalado en la máquina.

4. La instalación de Microsoft Store de Python incluye **PIP**, el administrador de paquetes estándar. PIP te permite instalar y administrar paquetes adicionales que no forman parte de la biblioteca estándar de Python. Para confirmar que también dispones de PIP para instalar y administrar paquetes, escribe `pip --version`.

## <a name="install-visual-studio-code"></a>Instalar Visual Studio Code

Al usar VS Code como editor de texto/entorno de desarrollo integrado (IDE), puedes aprovechar [IntelliSense](https://code.visualstudio.com/docs/editor/intellisense) (una ayuda de finalización de código), el [detector de errores](https://code.visualstudio.com/docs/python/linting) (permite evitar que se produzcan errores en el código), el [soporte técnico de depuración](https://code.visualstudio.com/docs/python/debugging) (ayuda a buscar errores en el código después de ejecutarlo), los [fragmentos de código](https://code.visualstudio.com/docs/editor/userdefinedsnippets) (plantillas para pequeños bloques de código reutilizables) y las [pruebas unitarias](https://code.visualstudio.com/docs/python/unit-testing) (para probar la interfaz del código con distintos tipos de entrada).

Descarga VS Code para Windows y sigue las instrucciones de instalación: [https://code.visualstudio.com](https://code.visualstudio.com).

## <a name="install-the-microsoft-python-extension"></a>Instalación de la extensión de Microsoft Python

Tendrás que instalar la extensión de Microsoft Python para aprovechar las características de soporte técnico de VS Code. [Más información](https://code.visualstudio.com/docs/languages/python).

1. Para abrir la ventana Extensiones de VS Code, escribe **Control + Mayús + X** (o usa el menú para desplazarte a **Ver** > **Extensiones**).

2. En el cuadro **Buscar extensiones en Marketplace** de la parte superior, escribe:  **Python**.

3. Busca la extensión **Python (ms-python.python) de Microsoft** y selecciona el botón **Instalar** de color verde.

## <a name="open-the-integrated-powershell-terminal-in-vs-code"></a>Apertura del terminal integrado de PowerShell en VS Code

VS Code contiene un [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal) que te permite abrir una línea de comandos de Python con PowerShell, lo que establece un flujo de trabajo sin interrupciones entre el editor de código y la línea de comandos.

1. Abre el terminal en VS Code, selecciona **Ver** > **Terminal**, o bien usa el acceso directo **Control + `** (mediante el carácter de tilde aguda).

    > [!NOTE]
    > El terminal predeterminado debe ser PowerShell, pero si necesitas cambiarlo, usa **Control + Mayús + P** para acceder a la paleta de comandos. Escribe **Terminal: Select Default Shell** y se mostrará una lista de opciones de terminal con PowerShell, símbolo del sistema, WSL, etc. Selecciona el que quieras usar y escribe **Control + Mayús + `** (con el carácter de tilde aguda) para crear un nuevo terminal.

2. En el terminal de VS Code, para abrir Python, escribe `python`.

3. Para probar el intérprete de Python, escribe `print("Hello World")`. Python devolverá la instrucción "Hola mundo".

    ![Línea de comandos de Python en VS Code](../images/python-in-vscode.png)

4. Para salir de Python, puedes escribir `exit()` o `quit()`, o seleccionar Control-Z.

## <a name="install-git-optional"></a>Instalar GIT (opcional)

Si planeas colaborar con otras personas en el código de Python u hospedar el proyecto en un sitio de código abierto (como GitHub), VS Code admite el [control de versiones con GIT](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support). La pestaña Control de código fuente de VS Code realiza un seguimiento de todos los cambios y tiene comandos GIT comunes (agregar, confirmar, enviar cambios e incorporar cambios) integrados directamente en la interfaz de usuario. Primero, debes instalar GIT para alimentar el panel de control de código fuente.

1. Descarga e instala GIT para Windows desde el [sitio web git-scm](https://git-scm.com/download/win).

2. Se incluye un asistente para instalación que te formulará una serie de preguntas sobre la configuración de la instalación de GIT. Te recomendamos que uses todas las opciones de configuración predeterminadas, a menos que tengas un motivo concreto para cambiar algo.

3. Si nunca has trabajado con GIT, las [guías de GitHub](https://guides.github.com/) pueden resultarte de ayuda para empezar.

## <a name="example-script-to-display-the-structure-of-your-file-system-directory"></a>Script de ejemplo para mostrar la estructura del directorio del sistema de archivos

Las tareas comunes de administración del sistema pueden tardar un tiempo considerable, pero con un script de Python, puedes automatizarlas para que no tarden nada. Por ejemplo, Python puede leer el contenido del sistema de archivos del equipo y realizar operaciones, como imprimir un esquema de los archivos y directorios, mover carpetas de un directorio a otro o cambiar el nombre de cientos de archivos. Normalmente, las tareas de este tipo podrían tardar mucho tiempo en completarse manualmente. Usa un script de Python en su lugar.

Comencemos con un script sencillo que recorre un árbol de directorios y muestra la estructura de directorios.

1. Abre PowerShell mediante el menú **Inicio** (icono de Windows de la esquina inferior izquierda).

2. Crea un directorio para el proyecto `mkdir python-scripts` y, a continuación, abre ese directorio: `cd python-scripts`.

3. Crea algunos directorios para usarlos con el script de ejemplo:

    ```powershell
    mkdir food, food\fruits, food\fruits\apples, food\fruits\oranges, food\vegetables
    ```

4. Crea algunos archivos dentro de esos directorios para usarlos con el script:

    ```powershell
    new-item food\fruits\banana.txt, food\fruits\strawberry.txt, food\fruits\blueberry.txt, food\fruits\apples\honeycrisp.txt, food\fruits\oranges\mandarin.txt, food\vegetables\carrot.txt
    ```

5. Crea un nuevo archivo de Python en el directorio de scripts de Python:

    ```powershell
    mkdir src
    new-item src\list-directory-contents.py
    ```

6. Abre el proyecto en VS Code. Para ello, escribe `code .`.

7. Abre la ventana Explorador de archivos de VS Code. Para ello, escribe **Control + Mayús + E** (o usa el menú para ir a **Ver** > **Explorador**) y selecciona el archivo list-directory-contents.py que acabas de crear. La extensión de Microsoft Python cargará automáticamente un intérprete de Python. Puedes ver qué intérprete se cargó en la parte inferior de la ventana de VS Code.

    > [!NOTE]
    > Python es un lenguaje interpretado, lo que significa que actúa como una máquina virtual y emula un equipo físico. Hay diferentes tipos de intérpretes de Python que puedes usar: Python 2, Python 3, Anaconda, PyPy, etc. A fin de ejecutar el código de Python y obtener IntelliSense de Python, debes indicar a VS Code el intérprete que debe usar. Te recomendamos que uses el intérprete que VS Code elige de forma predeterminada (Python 3 en nuestro caso), a menos que tengas un motivo específico para elegir algo diferente. Para cambiarlo, selecciona el intérprete que se muestra actualmente en la barra azul de la parte inferior de la ventana de VS Code o abre la **Paleta de comandos** (Control + Mayús + P) y escribe el comando **Python: Select Interpreter**. Se mostrará una lista de los intérpretes de Python que tienes instalados actualmente. [Obtén más información sobre la configuración de entornos de Python](https://code.visualstudio.com/docs/python/environments).

    ![Selección del intérprete de Python en VS Code](../images/interpreterselection.gif)

8. Pega el siguiente código en el archivo list-directory-contents.py y, a continuación, selecciona **Guardar**:

    ```python
    import os

    root = os.path.join('..', 'food')
    for directory, subdir_list, file_list in os.walk(root):
        print('Directory:', directory)
        for name in subdir_list:
            print('Subdirectory:', name)
        for name in file_list:
            print('File:', name)
        print()
    ```

9. Abre el terminal integrado de VS Code (**Control + `** mediante el carácter de tilde aguda) y escribe el directorio src donde acabas de guardar el script de Python:

    ```powershell
    cd src
    ```

10. Ejecuta el script de PowerShell con:

    ```powershell
    python3 .\list-directory-contents.py
    ```

    Deberías obtener un resultado similar al siguiente:

    ```powershell
    Directory: ..\food
    Subdirectory: fruits
    Subdirectory: vegetables

    Directory: ..\food\fruits
    Subdirectory: apples
    Subdirectory: oranges
    File: banana.txt
    File: blueberry.txt
    File: strawberry.txt

    Directory: ..\food\fruits\apples
    File: honeycrisp.txt

    Directory: ..\food\fruits\oranges
    File: mandarin.txt

    Directory: ..\food\vegetables
    File: carrot.txt
    ```

11. Usa Python para imprimir la salida del directorio del sistema de archivos en su propio archivo de texto. Para ello, escribe este comando directamente en el terminal de PowerShell: `python3 list-directory-contents.py > food-directory.txt`.

Enhorabuena. Acabas de escribir un script de administración de sistemas automatizado que lee el directorio y los archivos que has creado y usa Python para mostrar y, luego, imprimir la estructura de directorios en su propio archivo de texto.

## <a name="example-script-to-modify-all-files-in-a-directory"></a>Script de ejemplo para modificar todos los archivos de un directorio

En este ejemplo se usan los archivos y directorios que acabas de crear y se cambia el nombre de cada uno de los archivos mediante la adición de la fecha de la última modificación del archivo al principio del nombre de archivo.

1. En la carpeta **src** del directorio **python-scripts**, crea un nuevo archivo de Python para el script:

    ```powershell
    new-item update-filenames.py
    ```

2. Abre el archivo update-filenames.py, pega el código siguiente en el archivo y guárdalo:

    > [!NOTE]
    > os. getmtime devuelve una marca de tiempo en tics, que no se puede leer fácilmente. Primero, se debe convertir en una cadena de fecha y hora estándar.

    ```python
    import datetime
    import os

    root = os.path.join('..', 'food')
    for directory, subdir_list, file_list in os.walk(root):
        for name in file_list:
            source_name = os.path.join(directory, name)
            timestamp = os.path.getmtime(source_name)
            modified_date = str(datetime.datetime.fromtimestamp(timestamp)).replace(':', '.')
            target_name = os.path.join(directory, f'{modified_date}_{name}')

            print(f'Renaming: {source_name} to: {target_name}')

            os.rename(source_name, target_name)
    ```

3. Prueba el script update-filenames.py. Para ello, ejecuta `python3 update-filenames.py` y vuelve a ejecutar el script list-directory-contents.py: `python3 list-directory-contents.py`.

4. Deberías obtener un resultado similar al siguiente:

    ```powershell
    Renaming: ..\food\fruits\banana.txt to: ..\food\fruits\2019-07-18 12.24.46.385185_banana.txt
    Renaming: ..\food\fruits\blueberry.txt to: ..\food\fruits\2019-07-18 12.24.46.391170_blueberry.txt
    Renaming: ..\food\fruits\strawberry.txt to: ..\food\fruits\2019-07-18 12.24.46.389174_strawberry.txt
    Renaming: ..\food\fruits\apples\honeycrisp.txt to: ..\food\fruits\apples\2019-07-18 12.24.46.395160_honeycrisp.txt
    Renaming: ..\food\fruits\oranges\mandarin.txt to: ..\food\fruits\oranges\2019-07-18 12.24.46.398151_mandarin.txt
    Renaming: ..\food\vegetables\carrot.txt to: ..\food\vegetables\2019-07-18 12.24.46.402496_carrot.txt

    PS C:\src\python-scripting\src> python3 .\list-directory-contents.py
    ..\food\
    Directory: ..\food
    Subdirectory: fruits
    Subdirectory: vegetables

    Directory: ..\food\fruits
    Subdirectory: apples
    Subdirectory: oranges
    File: 2019-07-18 12.24.46.385185_banana.txt
    File: 2019-07-18 12.24.46.389174_strawberry.txt
    File: 2019-07-18 12.24.46.391170_blueberry.txt

    Directory: ..\food\fruits\apples
    File: 2019-07-18 12.24.46.395160_honeycrisp.txt

    Directory: ..\food\fruits\oranges
    File: 2019-07-18 12.24.46.398151_mandarin.txt

    Directory: ..\food\vegetables
    File: 2019-07-18 12.24.46.402496_carrot.txt

    ```

5. Usa Python para imprimir los nuevos nombres del directorio del sistema de archivos precedidos de la marca de tiempo de la última modificación en su propio archivo de texto. Para ello, escribe este comando directamente en el terminal de PowerShell: `python3 list-directory-contents.py > food-directory-last-modified.txt`.

Esperamos que hayas aprendido algunas cosas interesantes sobre el uso de scripts de Python para automatizar las tareas básicas de administración de sistemas. Por supuesto, te queda mucho más por descubrir, pero esperamos haberte proporcionado un comienzo agradable. Hemos compartido algunos recursos adicionales para que puedas seguir aprendiendo.

## <a name="additional-resources"></a>Recursos adicionales

- [Documentos de Python: Acceso a los directorios y a los archivos:](https://docs.python.org/3.7/library/filesys.html) documentación de Python sobre el trabajo con sistemas de archivos y el uso de módulos para leer las propiedades de los archivos, manipular rutas de acceso de forma fácil y crear archivos temporales.
- [Aprender a usar Python: tutorial de String_Formatting](https://www.learnpython.org/en/String_Formatting). Más información sobre el uso del operador "%" para el formato de cadenas.
- [10 métodos del sistema de archivos de Python que debes conocer](https://towardsdatascience.com/10-python-file-system-methods-you-should-know-799f90ef13c2): artículo de Medium sobre cómo manipular archivos y carpetas con `os` y `shutil`.
- [The Hitchhikers Guide to Python: Administración de los sistemas](https://docs.python-guide.org/scenarios/admin/). Una "Guía de bien fundamentada" que ofrece información general y procedimientos recomendados sobre temas relacionados con Python. En esta sección se describen las herramientas y los marcos de administración del sistema. Esta guía se hospeda en GitHub para que pueda archivar problemas y hacer contribuciones.
