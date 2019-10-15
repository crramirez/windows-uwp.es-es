---
title: Scripting y automatización con Python en Windows
description: Cómo empezar a usar Python para el scripting, la automatización y la administración de sistemas en Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Python, Windows 10, Microsoft, administración del sistema de Python, automatización de archivos de Python, scripts de Python en Windows, configuración de Python en Windows, entorno de desarrollo de Python en Windows, entorno de desarrollo de Python en Windows, Python con PowerShell, scripts de Python para tareas del sistema de archivos
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 3f8a17de8121fed27e69442d5560f702a04c8e42
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314869"
---
# <a name="get-started-using-python-on-windows-for-scripting-and-automation"></a>Introducción al uso de Python en Windows para scripting y automatización

La siguiente es una guía paso a paso para configurar el entorno de desarrollo y empezar a usar Python para crear scripts y automatizar las operaciones del sistema de archivos en Windows.

> [!NOTE]
> En este artículo se trata la configuración del entorno para usar algunas de las bibliotecas útiles de Python que pueden automatizar las tareas entre plataformas, como buscar en el sistema de archivos, acceder a Internet, analizar tipos de archivo, etc., desde un enfoque centrado en Windows. En el caso de las operaciones específicas de Windows, consulte [ctypes](https://docs.python.org/3/library/ctypes.html), una biblioteca de funciones externas compatible con C para Python, [winreg](https://docs.python.org/3/library/winreg.html), funciones que exponen la API del registro de Windows a Python y [Python/WinRT](https://pypi.org/project/winrt/), habilitando el acceso Windows Runtime las API desde Python.

## <a name="set-up-your-development-environment"></a>Configurar el entorno de desarrollo

Al usar Python para escribir scripts que realicen operaciones del sistema de archivos, se recomienda [instalar Python desde el Microsoft Store](https://www.microsoft.com/en-us/p/python-37/9nj46sx7x90p?activetab=pivot:overviewtab). La instalación a través del Microsoft Store utiliza el intérprete de Python3 básico, pero controla la configuración de la ruta de acceso del usuario actual (evitando la necesidad de acceso de administrador), además de proporcionar actualizaciones automáticas.

Si usa Python para el **desarrollo web** en Windows, se recomienda una configuración diferente con el subsistema de Windows para Linux. Busque un tutorial en nuestra guía: Comience a [usar Python para el desarrollo web en Windows](./web-frameworks.md). Si no está familiarizado con Python, pruebe nuestra guía: [Introducción al uso de Python en Windows para principiantes](./beginners.md). En algunos escenarios avanzados (como la necesidad de tener acceso a los archivos instalados de Python, modificarlos o de usarlos directamente), es posible que desee considerar la posibilidad de descargar una versión específica de Python directamente de [Python.org](https://www.python.org/downloads/) o considerar la posibilidad de instalar una [alternativa](https://www.python.org/download/alternatives), como Anaconda, Jython, PyPy, WinPython, IronPython, etc. Solo se recomienda si es un programador de Python más avanzado con un motivo específico para elegir una implementación alternativa.

## <a name="install-python"></a>Instalación de Python

Para instalar Python mediante el Microsoft Store:

1. Vaya al menú **Inicio** (icono de Windows inferior izquierdo), escriba "Microsoft Store" y seleccione el vínculo para abrir el almacén.

2. Una vez que el almacén está abierto, seleccione **Buscar** en el menú superior derecho y escriba "Python". Abra "Python 3,7" en los resultados de aplicaciones. Seleccione **obtener**.

3. Una vez que Python haya completado el proceso de descarga e instalación, abra Windows PowerShell mediante el menú **Inicio** (icono de Windows inferior izquierdo). Una vez que PowerShell esté abierto, escriba `Python --version` para confirmar que se ha instalado Python3 en la máquina.

4. La instalación de Microsoft Store de Python incluye **PIP**, el administrador de paquetes estándar. PIP permite instalar y administrar paquetes adicionales que no forman parte de la biblioteca estándar de Python. Para confirmar que también tiene PIP disponible para instalar y administrar paquetes, escriba `pip --version`.

## <a name="install-visual-studio-code"></a>Instalar Visual Studio Code

Mediante el uso de VS Code como el editor de texto o el entorno de desarrollo integrado (IDE), puede aprovechar [IntelliSense](https://code.visualstudio.com/docs/editor/intellisense) (una ayuda de finalización de código), la [detección de errores](https://code.visualstudio.com/docs/python/linting) (ayuda a evitar cometer errores en el código), la [compatibilidad de depuración](https://code.visualstudio.com/docs/python/debugging) (ayuda a encontrar errores en el código después de ejecutarlo), [fragmentos de código](https://code.visualstudio.com/docs/editor/userdefinedsnippets) (plantillas para pequeños bloques de código reutilizables) y [pruebas unitarias](https://code.visualstudio.com/docs/python/unit-testing) (prueba de la interfaz del código con distintos tipos de entrada).

Descargue VS Code para Windows y siga las instrucciones de instalación: [https://code.visualstudio.com](https://code.visualstudio.com).

## <a name="install-the-microsoft-python-extension"></a>Instalación de la extensión de Microsoft Python

Tendrá que instalar la extensión de Microsoft Python para aprovechar las características de soporte técnico de VS Code. [Más información](https://code.visualstudio.com/docs/languages/python).

1. Abra la ventana VS Code Extensions; para ello, escriba **Ctrl + Mayús + X** (o use el menú para desplazarse hasta **Ver** **las extensiones**de  > ).

2. En el cuadro principales **extensiones de búsqueda en Marketplace** , escriba:  **Python**.

3. Busque la extensión **Python (MS-Python. Python) de Microsoft** y seleccione el botón de **instalación** verde.

## <a name="open-the-integrated-powershell-terminal-in-vs-code"></a>Abra el terminal integrado de PowerShell en VS Code

VS Code contiene un [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal) que permite abrir una línea de comandos de Python con PowerShell, estableciendo un flujo de trabajo sin problemas entre el editor de código y la línea de comandos.

1. Abra el terminal en VS Code, seleccione **Ver** **terminal** > , o bien use el método abreviado **Ctrl + '** (mediante el carácter de acento grave).

    > [!NOTE]
    > El terminal predeterminado debe ser PowerShell, pero si necesita cambiarlo, use **Ctrl + Mayús + P** para escribir el comando paleta. Escriba **Terminal: Seleccione shell predeterminado @ no__t-0 y se mostrará una lista de opciones de terminal con PowerShell, símbolo del sistema, WSL, etc. Seleccione el que desee usar y **presione Ctrl + Mayús + '** (con el acento grave) para crear un nuevo terminal.

2. En el terminal de VS Code, abra Python; para ello, escriba: `python`

3. Para probar el intérprete de Python, escriba: `print("Hello World")`. Python devolverá la instrucción "Hola mundo".

    ![Línea de comandos de Python en VS Code](../images/python-in-vscode.png)

4. Para salir de Python, puede escribir `exit()`, `quit()` o seleccionar Ctrl-Z.

## <a name="install-git-optional"></a>Instalación de Git (opcional)

Si planea colaborar con otras personas en el código de Python o hospedar el proyecto en un sitio de código abierto (como GitHub), VS Code admite el [control de versiones con git](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support). La pestaña control de código fuente de VS Code realiza un seguimiento de todos los cambios y tiene comandos git comunes (agregar, confirmar, insertar, extraer) integrados en la interfaz de usuario. En primer lugar, debe instalar git para encender el panel de control de código fuente.

1. Descargue e instale git para Windows desde [el sitio web de Git-SCM](https://git-scm.com/download/win).

2. Se incluye un asistente de instalación que le preguntará una serie de preguntas sobre la configuración de la instalación de Git. Se recomienda usar todas las opciones de configuración predeterminadas, a menos que tenga una razón concreta para cambiar algo.

3. Si nunca ha trabajado antes con git, las [guías de github](https://guides.github.com/) pueden ayudarle a empezar.

## <a name="example-script-to-display-the-structure-of-your-file-system-directory"></a>Script de ejemplo para mostrar la estructura del directorio del sistema de archivos

Las tareas comunes de administración del sistema pueden tardar un tiempo considerable, pero con un script de Python, puede automatizar estas tareas para que no tarden ningún tiempo. Por ejemplo, Python puede leer el contenido del sistema de archivos del equipo y realizar operaciones como imprimir un esquema de los archivos y directorios, mover carpetas de un directorio a otro o cambiar el nombre de cientos de archivos. Normalmente, las tareas como estas podrían tardar un tiempo en realizarse manualmente. Use un script de Python en su lugar.

Comencemos con un script sencillo que recorre un árbol de directorios y muestra la estructura de directorios.

1. Abra PowerShell mediante el menú **Inicio** (icono de Windows inferior izquierdo).

2. Cree un directorio para el proyecto: `mkdir python-scripts` y, a continuación, abra el directorio siguiente: `cd python-scripts`.

3. Cree algunos directorios para usarlos con el script de ejemplo:

    ```powershell
    mkdir food, food\fruits, food\fruits\apples, food\fruits\oranges, food\vegetables
    ```

4. Cree algunos archivos dentro de esos directorios para usarlos con el script:

    ```powershell
    new-item food\fruits\banana.txt, food\fruits\strawberry.txt, food\fruits\blueberry.txt, food\fruits\apples\honeycrisp.txt, food\fruits\oranges\mandarin.txt, food\vegetables\carrot.txt
    ```

5. Cree un nuevo archivo de Python en el directorio de scripts de Python:

    ```powershell
    mkdir src
    new-item src\list-directory-contents.py
    ```

6. Abra el proyecto en VS Code; para ello, escriba: `code .`

7. Abra la ventana del explorador de archivos de VS Code escribiendo **Ctrl + Mayús + E** (o use el menú para navegar a **Ver** > **Explorer**) y seleccione el archivo list-Directory-Contents.py que acaba de crear. La extensión de Microsoft Python cargará automáticamente un intérprete de Python. Puede ver qué intérprete se cargó en la parte inferior de la ventana de VS Code.

    > [!NOTE]
    > Python es un lenguaje interpretado, lo que significa que actúa como una máquina virtual y emula un equipo físico. Hay diferentes tipos de intérpretes de Python que puede usar: Python 2, Python 3, Anaconda, PyPy, etc. Para ejecutar el código de Python y obtener IntelliSense de Python, debe indicar VS Code qué intérprete usar. Se recomienda que se adhiera al intérprete que VS Code elige de forma predeterminada (Python 3 en nuestro caso) a menos que tenga un motivo específico para elegir algo diferente. Para cambiar el intérprete de Python, seleccione el intérprete que se muestra actualmente en la barra azul en la parte inferior de la ventana de VS Code o abra la **paleta de comandos** (Ctrl + Mayús + P) y escriba el comando **Python: Seleccione intérprete @ no__t-0. Se mostrará una lista de los intérpretes de Python que tiene instalados actualmente. [Más información sobre la configuración de entornos de Python](https://code.visualstudio.com/docs/python/environments).

    ![Seleccione el intérprete de Python en VS Code](../images/interpreterselection.gif)

8. Pegue el código siguiente en el archivo list-directory-contents.py y, a continuación, seleccione **Guardar**:

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

9. Abra el VS Code terminal integrado (**Ctrl + "** , mediante el carácter de acento grave) y escriba el directorio src donde acaba de guardar el script de Python:

    ```powershell
    cd src
    ```

10. Ejecute el script de PowerShell con:

    ```powershell
    python3 .\list-directory-contents.py
    ```

    Debería ver una salida similar a la siguiente:

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

11. Use Python para imprimir la salida del directorio del sistema de archivos en su propio archivo de texto escribiendo este comando directamente en el terminal de PowerShell: `python3 list-directory-contents.py > food-directory.txt`

¡Enhorabuena! Acaba de escribir un script de administración automatizada de sistemas que lee el directorio y los archivos que ha creado y usa Python para mostrar y, a continuación, imprimir la estructura de directorios en su propio archivo de texto.

## <a name="example-script-to-modify-all-files-in-a-directory"></a>Script de ejemplo para modificar todos los archivos de un directorio

En este ejemplo se usan los archivos y directorios que acaba de crear, cambiando el nombre de cada uno de los archivos agregando la fecha de la última modificación del archivo al principio del nombre de archivo.

1. Dentro de la carpeta **src** del directorio **Python-scripts** , cree un nuevo archivo Python para el script:

    ```powershell
    new-item update-filenames.py
    ```

2. Abra el archivo update-filenames.py, pegue el código siguiente en el archivo y guárdelo:

    > [!NOTE]
    > os. getmtime devuelve una marca de tiempo en pasos, que no se puede leer fácilmente. Primero se debe convertir en una cadena de fecha y hora estándar.

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

3. Pruebe el script update-filenames.py, para lo que debe ejecutarlo: `python3 update-filenames.py` y, a continuación, vuelva a ejecutar el script list-directory-contents.py: `python3 list-directory-contents.py`

4. Debería ver una salida similar a la siguiente:

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

5. Use Python para imprimir los nombres de los nuevos directorios del sistema de archivos con la marca de tiempo que se modificó por última vez en su propio archivo de texto escribiendo este comando directamente en el terminal de PowerShell: `python3 list-directory-contents.py > food-directory-last-modified.txt`

Esperamos que haya aprendido algunas cosas divertidas sobre el uso de scripts de Python para automatizar las tareas básicas de administración de sistemas. Por supuesto, hay una gran cantidad de conocimiento, pero esperamos que se inicie en el pie derecho. Hemos compartido algunos recursos adicionales para seguir aprendiendo.

## <a name="additional-resources"></a>Recursos adicionales

- Documentos de @no__t 0Python: Acceso a archivos y directorios @ no__t-0: Documentación de Python sobre el trabajo con sistemas de archivos y el uso de módulos para leer las propiedades de los archivos, manipular rutas de acceso de forma portátil y crear archivos temporales.
- @no__t 0Learn Python: Tutorial de String_Formatting @ no__t-0: Más información sobre el uso del operador "%" para el formato de cadenas.
- [10 métodos del sistema de archivos python que debe conocer](https://towardsdatascience.com/10-python-file-system-methods-you-should-know-799f90ef13c2): Artículo medio sobre cómo manipular archivos y carpetas con `os` y `shutil`.
- @no__t 0The Hitchhikers a Python: System Administration @ no__t-0: Una "Guía de bien fundamentadas" que ofrece información general y prácticas recomendadas sobre temas relacionados con Python. En esta sección se describen las herramientas y los marcos de administración del sistema. Esta guía se hospeda en GitHub para que pueda archivar problemas y hacer contribuciones.
