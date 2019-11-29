---
title: Introducción al uso de Python en Windows para principiantes
description: Guía que te ayudará a empezar si no estás familiarizado en el uso de Python en Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: python, windows 10, microsoft, aprender python, python en windows para principiantes, instalar python con microsoft store, python con vs code, pygame en windows
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: d4c1cb6d65eb38a93e8bf9f0c34afd9e28f20129
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314929"
---
# <a name="get-started-using-python-on-windows-for-beginners"></a>Introducción al uso de Python en Windows para principiantes

A continuación, te ofrecemos una guía paso a paso para aquellos usuarios principiantes interesados en aprender Python con Windows 10.

## <a name="set-up-your-development-environment"></a>Configurar el entorno de desarrollo

Si eres un usuario principiante y no estás familiarizado con Python, te recomendamos [instalar Python desde Microsoft Store](https://www.microsoft.com/en-us/p/python-37/9nj46sx7x90p?activetab=pivot:overviewtab). La instalación a través de Microsoft Store utiliza el intérprete de Python3 básico, pero controla el establecimiento de la configuración del valor PATH para el usuario actual (lo que evita la necesidad de contar con acceso de administrador) y, además, proporciona actualizaciones automáticas. Resulta especialmente útil si te encuentras en un entorno educativo o en un departamento de una organización que restringe los permisos o el acceso administrativo en la máquina.

Si usas Python en Windows para el **desarrollo web**, te recomendamos que definas una configuración diferente para el entorno de desarrollo. En lugar de realizar la instalación directamente en Windows, te recomendamos que instales y uses Python a través del Subsistema de Windows para Linux. Para obtener ayuda, consulta lo siguiente: [Introducción al uso de Python para el desarrollo web en Windows](./web-frameworks.md). Si estás interesado en automatizar las tareas comunes en el sistema operativo, consulta nuestra guía: [Introducción al uso de Python en Windows para el scripting y la automatización](./scripting.md). En algunos escenarios avanzados (por ejemplo, si necesitas acceder a los archivos instalados de Python o modificarlos, hacer copias en archivos binarios o usar archivos DLL de Python directamente), es posible que quieras considerar la posibilidad de descargar una versión específica de Python directamente desde [python.org](https://www.python.org/downloads/) o una [alternativa](https://www.python.org/download/alternatives), como Anaconda, Jython, PyPy, WinPython, IronPython, etc. Solo te lo recomendamos si eres un programador de Python más avanzado y tienes un motivo específico para elegir una implementación alternativa.

## <a name="install-python"></a>Instalar Python

Para instalar Python con Microsoft Store:

1. Ve al menú **Inicio** (icono de Windows de la esquina inferior izquierda), escribe "Microsoft Store" y selecciona el vínculo para abrir Store.

2. Una vez que lo hayas abierto, selecciona **Buscar** en el menú superior derecho y escribe "Python". Abre "Python 3.7" en los resultados de las aplicaciones. Selecciona **Obtener**.

3. Una vez que Python haya completado el proceso de descarga e instalación, abre Windows PowerShell mediante el menú **Inicio** (icono de Windows de la esquina inferior izquierda). Cuando PowerShell esté abierto, escribe `Python --version` para confirmar que Python3 está instalado en la máquina.

4. La instalación de Microsoft Store de Python incluye **PIP**, el administrador de paquetes estándar. PIP te permite instalar y administrar paquetes adicionales que no forman parte de la biblioteca estándar de Python. Para confirmar que también dispones de PIP para instalar y administrar paquetes, escribe `pip --version`.

## <a name="install-visual-studio-code"></a>Instalar Visual Studio Code

Al usar VS Code como editor de texto/entorno de desarrollo integrado (IDE), puedes aprovechar [IntelliSense](https://code.visualstudio.com/docs/editor/intellisense) (una ayuda de finalización de código), el [detector de errores](https://code.visualstudio.com/docs/python/linting) (permite evitar que se produzcan errores en el código), el [soporte técnico de depuración](https://code.visualstudio.com/docs/python/debugging) (ayuda a buscar errores en el código después de ejecutarlo), los [fragmentos de código](https://code.visualstudio.com/docs/editor/userdefinedsnippets) (plantillas para pequeños bloques de código reutilizables) y las [pruebas unitarias](https://code.visualstudio.com/docs/python/unit-testing) (para probar la interfaz del código con distintos tipos de entrada).

VS Code también contiene un [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal) que te permite abrir una línea de comandos de Python con el símbolo del sistema de Windows, PowerShell o cualquier otra herramienta que prefieras, y establece un flujo de trabajo sin interrupciones entre el editor de código y la línea de comandos.

1. Para instalar VS Code, descarga VS Code para Windows: [https://code.visualstudio.com](https://code.visualstudio.com).

2. Python es un lenguaje interpretado y, para ejecutar el código de Python, debes indicar a VS Code el intérprete que debe usar. Te recomendamos que uses Python 3.7, a menos que tengas un motivo específico para elegir otra opción. Para seleccionar un intérprete de Python 3, abre la **paleta de comandos** (Control + Mayús + P) y empieza a escribir el comando **Python: Select Interpreter** para buscarlo y, luego, selecciónalo. También puedes usar la opción **Select Python Environment** (Seleccionar entorno de Python) en la barra de estado inferior si está disponible (es posible que ya se muestre un intérprete seleccionado). El comando presenta una lista de los intérpretes disponibles que VS Code puede buscar automáticamente, incluidos los entornos virtuales. Si no ves el intérprete que quieres, consulta [Configuración de los entornos de Python](https://code.visualstudio.com/docs/python/environments).

    ![Selección del intérprete de Python en VS Code](../images/interpreterselection.gif)

3. Para abrir el terminal en VS Code, selecciona **Ver** > **Terminal**, o bien usa el acceso directo **Control + `** (mediante el carácter de tilde aguda). El terminal predeterminado es PowerShell.

4. En el terminal de VS Code, simplemente escribe el comando `python` para abrir Python.

5. Para probar el intérprete de Python, escribe `print("Hello World")`. Python devolverá la instrucción "Hola mundo".

    ![Línea de comandos de Python en VS Code](../images/python-in-vscode.png)

## <a name="install-git-optional"></a>Instalar GIT (opcional)

Si planeas colaborar con otras personas en el código de Python u hospedar el proyecto en un sitio de código abierto (como GitHub), VS Code admite el [control de versiones con GIT](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support). La pestaña Control de código fuente de VS Code realiza un seguimiento de todos los cambios y tiene comandos GIT comunes (agregar, confirmar, enviar cambios e incorporar cambios) integrados directamente en la interfaz de usuario. Primero, debes instalar GIT para alimentar el panel de control de código fuente.

1. Descarga e instala GIT para Windows desde el [sitio web git-scm](https://git-scm.com/download/win).

2. Se incluye un asistente para instalación que te formulará una serie de preguntas sobre la configuración de la instalación de GIT. Te recomendamos que uses todas las opciones de configuración predeterminadas, a menos que tengas un motivo concreto para cambiar algo.

3. Si nunca has trabajado con GIT, las [guías de GitHub](https://guides.github.com/) pueden resultarte de ayuda para empezar.

## <a name="hello-world-tutorial-for-some-python-basics"></a>Tutorial de Hola mundo para algunos aspectos básicos de Python

Python, según su creador Guido van Rossum, es un "lenguaje de programación de alto nivel y su filosofía de diseño básico trata sobre la legibilidad del código y una sintaxis que permite a los programadores expresar conceptos en unas pocas líneas de código".

Python es un lenguaje interpretado. A diferencia de los lenguajes compilados, en los que el código que escribes debe traducirse en código máquina para que lo ejecute el procesador del equipo, el código de Python se pasa a un intérprete y se ejecuta directamente. Solo tienes que escribir el código y ejecutarlo. Probémoslo.

1. Con la línea de comandos de PowerShell abierta, escribe `python` para ejecutar el intérprete de Python 3. (Algunas instrucciones prefieren usar el comando `py` o `python3` y también deberían funcionar). Sabrás que se ha ejecutado correctamente porque se mostrará un aviso con tres símbolos de mayor que (>>>).

2. Hay varios métodos integrados que permiten realizar modificaciones en las cadenas de Python. Crea una variable con `variable = 'Hello World!'`. Presiona Entrar para que se muestre una nueva línea.

3. Imprime la variable con `print(variable)`. Se mostrará el texto "Hello World!".

4. Averigua la longitud (el número de caracteres que se usan) de la variable de cadena con `len(variable)`. Se mostrará que se usan 12 caracteres. (Ten en cuenta que el espacio en blanco se cuenta como un carácter en la longitud total).

5. Convierte la variable de cadena en letras mayúsculas: `variable.upper()`. Convierte la variable de cadena en letras minúsculas: `variable.lower()`.

6. Cuenta el número de veces que se usa la letra "l" en la variable de cadena: `variable.count("l")`.

7. Busca un carácter específico en la variable de cadena. En este caso, buscaremos el signo de exclamación con `variable.find("!")`. Se mostrará que el signo de exclamación se encuentra en el carácter undécimo de la cadena.

8. Reemplaza el signo de exclamación por un signo de interrogación: `variable.replace("!", "?")`.

9. Para salir de Python, puedes escribir `exit()` o `quit()`, o seleccionar Control-Z.

![Captura de pantalla de PowerShell de este tutorial](../images/hello-world-basics.png)

Esperamos que te hayas divertido usando algunos de los métodos de modificación de cadenas integrados de Python. Ahora intenta crear un archivo de programa de Python y ejecutarlo con VS Code.

## <a name="hello-world-tutorial-for-using-python-with-vs-code"></a>Tutorial Hola mundo para usar Python con VS Code

El equipo de VS Code ha elaborado el excelente tutorial [Introducción a Python](https://code.visualstudio.com/docs/python/python-tutorial#_start-vs-code-in-a-project-workspace-folder) en el que se explica cómo crear un programa Hola mundo con Python, ejecutar el archivo de programa, configurar y ejecutar el depurador e instalar paquetes como *matplotlib* y *NumPy* para crear un trazado gráfico dentro de un entorno virtual.

1. Abre PowerShell y crea una carpeta vacía denominada "hello", navega a esta carpeta y ábrela en VS Code:

    ```console
    mkdir hello
    cd hello
    code .
    ```

2. Una vez que se abra VS Code y se muestre la nueva carpeta *Hello* en la ventana **Explorador** del lado izquierdo, abre una ventana de línea de comandos en el panel inferior de VS Code. Para ello, presiona **Control + `** (mediante el carácter de tilde aguda) o selecciona **Ver** > **Terminal**. Al iniciar VS Code en una carpeta, esa carpeta se convierte en tu "área de trabajo". VS Code almacena la configuración específica de esa área de trabajo en. vscode/settings.json, que es independiente de la configuración de usuario que se almacena globalmente.

3. Continúa con el tutorial en la documentación de VS Code: [Creación de un archivo de código fuente de Hola mundo de Python](https://code.visualstudio.com/docs/python/python-tutorial#_create-a-python-hello-world-source-code-file).

## <a name="create-a-simple-game-with-pygame"></a>Crear un juego sencillo con Pygame

![Pygame ejecutando un juego de ejemplo](../images/pygame-shmup.jpg)

Pygame es un paquete de Python conocido para escribir juegos, que anima a los alumnos a aprender a programar a la vez que crean algo divertido. Pygame muestra los gráficos en una nueva ventana, por lo que no funcionará en el enfoque solo de línea de comandos de WSL. Sin embargo, si instalaste Python mediante Microsoft Store tal y como se detalla en este tutorial, funcionará correctamente.

1. Una vez hayas instalado Python, instala Pygame desde la línea de comandos (o el terminal de VS Code). Para ello, escribe `python -m pip install -U pygame --user`.

2. Prueba la instalación mediante la ejecución de un juego de ejemplo: `python -m pygame.examples.aliens`.

3. Si todo va bien, el juego abrirá una ventana. Cierra la ventana cuando termines de jugar.

A continuación, te indicamos cómo puedes empezar a escribir tu propio juego.

1. Abre PowerShell (o el símbolo del sistema de Windows) y crea una carpeta vacía denominada "bounce". Navega hasta esta carpeta y crea un archivo denominado "bounce.py". Abre la carpeta en VS Code.

    ```powershell
    mkdir bounce
    cd bounce
    new-item bounce.py
    code .
    ```

2. Con VS Code, escribe el código siguiente de Python (o cópialo y pégalo):

    ```python
    import sys, pygame

    pygame.init()

    size = width, height = 640, 480
    dx = 1
    dy = 1
    x= 163
    y = 120
    black = (0,0,0)
    white = (255,255,255)

    screen = pygame.display.set_mode(size)

    while 1:

        for event in pygame.event.get():
            if event.type == pygame.QUIT: sys.exit()

        x += dx
        y += dy

        if x < 0 or x > width:   
            dx = -dx

        if y < 0 or y > height:
            dy = -dy

        screen.fill(black)

        pygame.draw.circle(screen, white, (x,y), 8)

        pygame.display.flip()
    ```

3. Guárdalo como `bounce.py`.

4. En el terminal de PowerShell, escribe `python bounce.py` para ejecutarlo.

    ![Pygame ejecutando la próxima gran idea](../images/pygame.jpg)

Intenta ajustar algunos de los números para ver el efecto que tienen en la bola que bota.

Obtén más información sobre la escritura de juegos con Pygame en [pygame.org](http://www.pygame.org).

## <a name="resources-for-continued-learning"></a>Recursos para el aprendizaje continuo

Te recomendamos los siguientes recursos que te permitirán seguir obteniendo información sobre el desarrollo de Python en Windows.

### <a name="online-courses-for-learning-python"></a>Cursos en línea para aprender Python

- [Introducción a Python en Microsoft Learn](https://docs.microsoft.com/en-us/learn/modules/intro-to-python/): prueba la plataforma de Microsoft Learn interactiva y gana puntos de experiencia por completar este módulo, en el que se describen los aspectos básicos sobre cómo escribir código de Python básico, declarar variables y trabajar con la entrada y salida de la consola. Con el entorno de espacio aislado interactivo, resulta un buen punto de partida para aquellas personas que aún no cuentan con su propio entorno de desarrollo de Python configurado.

- [Python en Pluralsight: 8 cursos, 29 horas](https://app.pluralsight.com/paths/skills/python): la ruta de aprendizaje de Python en Pluralsight ofrece cursos en línea que abarcan una gran variedad de temas relacionados con Python, incluida una herramienta para medir tus aptitudes y averiguar tus debilidades.

- [Tutoriales de LearnPython.org](https://www.learnpython.org/): empieza a aprender el lenguaje Python sin necesidad de instalar o configurar nada con estos tutoriales interactivos y gratuitos de Python de los usuarios de DataCamp.

- [Tutoriales de Python.org](https://docs.python.org/3/tutorial/index.html): introduce al lector los conceptos básicos y las características del lenguaje y el sistema de Python de una manera informativa.

- [Aprendizaje de Python en Lynda.com](https://www.lynda.com/Python-tutorials/Learning-Python/661773-2.html): introducción básica a Python.

### <a name="working-with-python-in-vs-code"></a>Trabajar con Python en VS Code

- [Edición de Python en VS Code](https://code.visualstudio.com/docs/python/editing): obtén más información sobre cómo aprovechar las ventajas de la compatibilidad de Autocompletar e IntelliSense de VS Code con Python, incluida la forma de personalizar sus comportamientos, o simplemente desactívalas.

- [Detección de errores de Python](https://code.visualstudio.com/docs/python/linting): la detección de errores es el proceso de ejecución de un programa que analizará el código para detectar posibles errores. Obtén información sobre las distintas formas de compatibilidad de la detección de errores que VS Code proporciona para Python y cómo configurarlas.

- [Depuración de Python](https://code.visualstudio.com/docs/python/debugging): la depuración es el proceso de identificar y quitar errores de un programa. En este artículo se describe cómo inicializar y configurar la depuración de Python con VS Code, establecer y validar puntos de interrupción, adjuntar un script local, realizar la depuración para diferentes tipos de aplicaciones o en un equipo remoto, y algunos procedimientos de solución de problemas básicos.

- [Pruebas unitarias de Python](https://code.visualstudio.com/docs/python/unit-testing): incluye un poco de información sobre qué significa el término pruebas unitarias, un tutorial de ejemplo, la habilitación de un marco de pruebas, la creación y ejecución de pruebas, la depuración de pruebas y la configuración de pruebas. 
