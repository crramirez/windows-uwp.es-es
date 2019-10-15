---
title: Introducción al uso de Python en Windows para principiantes
description: Una guía para ayudarle a comenzar si su marca es nueva en usar Python en Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Python, Windows 10, Microsoft, Learning Python, Python en Windows para principiantes, instalación de Python con Microsoft Store, Python con vs Code, Pygame en Windows
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: d4c1cb6d65eb38a93e8bf9f0c34afd9e28f20129
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314929"
---
# <a name="get-started-using-python-on-windows-for-beginners"></a>Introducción al uso de Python en Windows para principiantes

La siguiente es una guía paso a paso para principiantes interesados en aprender Python con Windows 10.

## <a name="set-up-your-development-environment"></a>Configurar el entorno de desarrollo

Para principiantes que no están familiarizados con Python, se recomienda [instalar Python desde el Microsoft Store](https://www.microsoft.com/en-us/p/python-37/9nj46sx7x90p?activetab=pivot:overviewtab). La instalación a través del Microsoft Store utiliza el intérprete de Python3 básico, pero controla la configuración de la ruta de acceso del usuario actual (evitando la necesidad de acceso de administrador), además de proporcionar actualizaciones automáticas. Esto es especialmente útil si se encuentra en un entorno educativo o en una parte de una organización que restringe los permisos o el acceso administrativo en el equipo.

Si usa Python en Windows para el **desarrollo web**, se recomienda una configuración diferente para el entorno de desarrollo. En lugar de instalar directamente en Windows, se recomienda instalar y usar Python a través del subsistema de Windows para Linux. Para obtener ayuda, consulte: Comience a [usar Python para el desarrollo web en Windows](./web-frameworks.md). Si está interesado en automatizar las tareas comunes en el sistema operativo, consulte nuestra guía: [Introducción al uso de Python en Windows para scripting y automatización](./scripting.md). En algunos escenarios avanzados (como la necesidad de tener acceso a los archivos instalados de Python, modificarlos o de usarlos directamente), es posible que desee considerar la posibilidad de descargar una versión específica de Python directamente de [Python.org](https://www.python.org/downloads/) o considerar la posibilidad de instalar una [alternativa](https://www.python.org/download/alternatives), como Anaconda, Jython, PyPy, WinPython, IronPython, etc. Solo se recomienda si es un programador de Python más avanzado con un motivo específico para elegir una implementación alternativa.

## <a name="install-python"></a>Instalación de Python

Para instalar Python mediante el Microsoft Store:

1. Vaya al menú **Inicio** (icono de Windows inferior izquierdo), escriba "Microsoft Store" y seleccione el vínculo para abrir el almacén.

2. Una vez que el almacén está abierto, seleccione **Buscar** en el menú superior derecho y escriba "Python". Abra "Python 3,7" en los resultados de aplicaciones. Seleccione **obtener**.

3. Una vez que Python haya completado el proceso de descarga e instalación, abra Windows PowerShell mediante el menú **Inicio** (icono de Windows inferior izquierdo). Una vez que PowerShell esté abierto, escriba `Python --version` para confirmar que Python3 está instalado en la máquina.

4. La instalación de Microsoft Store de Python incluye **PIP**, el administrador de paquetes estándar. PIP permite instalar y administrar paquetes adicionales que no forman parte de la biblioteca estándar de Python. Para confirmar que también tiene PIP disponible para instalar y administrar paquetes, escriba `pip --version`.

## <a name="install-visual-studio-code"></a>Instalar Visual Studio Code

Mediante el uso de VS Code como el editor de texto o el entorno de desarrollo integrado (IDE), puede aprovechar [IntelliSense](https://code.visualstudio.com/docs/editor/intellisense) (una ayuda de finalización de código), la [detección de errores](https://code.visualstudio.com/docs/python/linting) (ayuda a evitar cometer errores en el código), la [compatibilidad de depuración](https://code.visualstudio.com/docs/python/debugging) (ayuda a encontrar errores en el código después de ejecutarlo), [fragmentos de código](https://code.visualstudio.com/docs/editor/userdefinedsnippets) (plantillas para pequeños bloques de código reutilizables) y [pruebas unitarias](https://code.visualstudio.com/docs/python/unit-testing) (prueba de la interfaz del código con distintos tipos de entrada).

VS Code también contiene un [terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal) que permite abrir una línea de comandos de Python con el símbolo del sistema de Windows, PowerShell o cualquier cosa que prefiera, estableciendo un flujo de trabajo sin problemas entre el editor de código y la línea de comandos.

1. Para instalar VS Code, descargue VS Code para Windows: [https://code.visualstudio.com](https://code.visualstudio.com).

2. Python es un lenguaje interpretado y, para ejecutar el código de Python, debe indicar VS Code qué intérprete usar. Se recomienda usar Python 3,7 a menos que tenga un motivo específico para elegir algo diferente. Seleccione un intérprete de Python 3 abriendo la **paleta de comandos** (Ctrl + Mayús + P) y empiece a escribir el comando **Python: Seleccione intérprete @ no__t-0 para buscar y, después, seleccione el comando. También puede usar la opción **seleccionar el entorno de Python** en la barra de estado inferior si está disponible (es posible que ya muestre un intérprete seleccionado). El comando presenta una lista de intérpretes disponibles que VS Code pueden encontrar automáticamente, incluidos los entornos virtuales. Si no ve el intérprete deseado, consulte [configuración de entornos de Python](https://code.visualstudio.com/docs/python/environments).

    ![Seleccione el intérprete de Python en VS Code](../images/interpreterselection.gif)

3. Para abrir el terminal en VS Code, seleccione **Ver** **terminal** > , o bien use el método abreviado **Ctrl + '** (mediante el carácter de acento grave). El terminal predeterminado es PowerShell.

4. En el terminal de VS Code, abra Python con solo escribir el comando: `python`

5. Para probar el intérprete de Python, escriba: `print("Hello World")`. Python devolverá la instrucción "Hola mundo".

    ![Línea de comandos de Python en VS Code](../images/python-in-vscode.png)

## <a name="install-git-optional"></a>Instalación de Git (opcional)

Si planea colaborar con otras personas en el código de Python o hospedar el proyecto en un sitio de código abierto (como GitHub), VS Code admite el [control de versiones con git](https://code.visualstudio.com/docs/editor/versioncontrol#_git-support). La pestaña control de código fuente de VS Code realiza un seguimiento de todos los cambios y tiene comandos git comunes (agregar, confirmar, insertar, extraer) integrados en la interfaz de usuario. En primer lugar, debe instalar git para encender el panel de control de código fuente.

1. Descargue e instale git para Windows desde [el sitio web de Git-SCM](https://git-scm.com/download/win).

2. Se incluye un asistente de instalación que le preguntará una serie de preguntas sobre la configuración de la instalación de Git. Se recomienda usar todas las opciones de configuración predeterminadas, a menos que tenga una razón concreta para cambiar algo.

3. Si nunca ha trabajado antes con git, las [guías de github](https://guides.github.com/) pueden ayudarle a empezar.

## <a name="hello-world-tutorial-for-some-python-basics"></a>Hola mundo tutorial para algunos aspectos básicos de Python

Python, de acuerdo con su creador Guido van Rossum, es un lenguaje de programación de alto nivel y su filosofía de diseño básico es todo lo que se reconoce a la legibilidad del código y una sintaxis que permite a los programadores expresar conceptos en unas pocas líneas de código ".

Python es un lenguaje interpretado. A diferencia de los lenguajes compilados, en los que el código que se escribe debe traducirse en código máquina para que lo ejecute el procesador del equipo, el código de Python se pasa directamente a un intérprete y se ejecuta directamente. Solo tiene que escribir el código y ejecutarlo. ¡ Vamos a probarlo!

1. Con la línea de comandos de PowerShell abierta, escriba `python` para ejecutar el intérprete de Python 3. (Algunas instrucciones prefieren usar el comando `py` o `python3`, que también deberían funcionar). Sabrá que tiene éxito porque se mostrará > un símbolo del sistema > > con tres símbolos mayores que.

2. Hay varios métodos integrados que permiten realizar modificaciones en las cadenas de Python. Cree una variable con: `variable = 'Hello World!'`. Presione Entrar para obtener una nueva línea.

3. Imprima la variable con: `print(variable)`. Se mostrará el texto "Hola mundo!".

4. Averigüe la longitud, el número de caracteres que se usan, de la variable de cadena con: `len(variable)`. Esto mostrará que se usan 12 caracteres. (Tenga en cuenta que el espacio en blanco se cuenta como un carácter en la longitud total).

5. Convierta la variable de cadena en letras mayúsculas: `variable.upper()`. Ahora, convierta la variable de cadena en minúsculas: `variable.lower()`.

6. Cuente el número de veces que se usa la letra "l" en la variable de cadena: `variable.count("l")`.

7. Busque un carácter específico en la variable de cadena, busque el signo de exclamación, con: `variable.find("!")`. Esto mostrará que el signo de exclamación se encuentra en el carácter de posición undécimo de la cadena.

8. Reemplace el punto de exclamación por un signo de interrogación: `variable.replace("!", "?")`.

9. Para salir de Python, puede escribir `exit()`, `quit()` o seleccionar Ctrl-Z.

![Captura de pantalla de PowerShell de este tutorial](../images/hello-world-basics.png)

Esperamos que haya divertido usar algunos de los métodos de modificación de cadenas integrados de Python. Ahora intente crear un archivo de programa de Python y ejecutarlo con VS Code.

## <a name="hello-world-tutorial-for-using-python-with-vs-code"></a>Hola mundo tutorial para usar Python con VS Code

El equipo de VS Code ha elaborado un excelente [Introducción con el tutorial de Python](https://code.visualstudio.com/docs/python/python-tutorial#_start-vs-code-in-a-project-workspace-folder) , en el que se explica cómo crear un programa de Hola mundo con Python, ejecutar el archivo de programa, configurar y ejecutar el depurador e instalar paquetes como *matplotlib* y  *NumPy* para crear un trazado gráfico dentro de un entorno virtual.

1. Abra PowerShell y cree una carpeta vacía denominada "Hello", vaya a esta carpeta y ábrala en VS Code:

    ```console
    mkdir hello
    cd hello
    code .
    ```

2. Una vez que se abra VS Code, mostrando la nueva carpeta *Hello* en la ventana del **Explorador** de la izquierda, abra una ventana de línea de comandos en el panel inferior de vs Code presionando **Ctrl + '** (mediante el carácter de acento grave) o seleccionando **Ver** >  **Terminal**. Al iniciar VS Code en una carpeta, esa carpeta se convierte en el "área de trabajo". VS Code almacena la configuración específica de esa área de trabajo en. vscode/Settings. JSON, que son independientes de la configuración de usuario que se almacenan globalmente.

3. Continúe con el tutorial de la VS Code docs: [Cree un archivo de código fuente de Hola mundo de Python](https://code.visualstudio.com/docs/python/python-tutorial#_create-a-python-hello-world-source-code-file).

## <a name="create-a-simple-game-with-pygame"></a>Crear un juego sencillo con Pygame

![Pygame ejecución de un juego de ejemplo](../images/pygame-shmup.jpg)

Pygame es un conocido paquete de Python para escribir juegos: animar a los estudiantes a aprender la programación y crear algo divertido. Pygame muestra los gráficos en una nueva ventana, por lo que no funcionará en el enfoque de solo línea de comandos de WSL. Sin embargo, si instaló Python mediante el Microsoft Store tal y como se detalla en este tutorial, funcionará correctamente.

1. Una vez instalado Python, instale Pygame desde la línea de comandos (o el terminal desde VS Code) escribiendo `python -m pip install -U pygame --user`.

2. Prueba de la instalación mediante la ejecución de un juego de ejemplo: `python -m pygame.examples.aliens`

3. Todo está bien, el juego abrirá una ventana. Cierre la ventana cuando haya terminado de reproducirla.

Aquí se muestra cómo empezar a escribir su propio juego.

1. Abra PowerShell (o símbolo del sistema de Windows) y cree una carpeta vacía denominada "Bounce". Navegue hasta esta carpeta y cree un archivo denominado "bounce.py". Abra la carpeta en VS Code:

    ```powershell
    mkdir bounce
    cd bounce
    new-item bounce.py
    code .
    ```

2. Con VS Code, escriba el siguiente código de Python (o cópielo y péguelo):

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

3. Guárdelo como: `bounce.py`.

4. En el terminal de PowerShell, ejecute: `python bounce.py`.

    ![Pygame ejecución de la siguiente novedad](../images/pygame.jpg)

Intente ajustar algunos de los números para ver el efecto que tienen en la bola de rebote.

Obtenga más información sobre la escritura de juegos con Pygame en [Pygame.org](http://www.pygame.org).

## <a name="resources-for-continued-learning"></a>Recursos para el aprendizaje continuo

Se recomiendan los siguientes recursos para ayudarle a continuar con la información sobre el desarrollo de Python en Windows.

### <a name="online-courses-for-learning-python"></a>Cursos en línea para aprender Python

- [Introducción a Python en Microsoft Learn](https://docs.microsoft.com/en-us/learn/modules/intro-to-python/): Pruebe la plataforma de Microsoft Learn interactiva y Gane puntos de experiencia para completar este módulo, donde se describen los aspectos básicos sobre cómo escribir código de Python básico, declarar variables y trabajar con la entrada y salida de la consola. El entorno de espacio aislado interactivo lo convierte en un buen punto de partida para las personas que aún no tienen su entorno de desarrollo de Python configurado.

- [Python en Pluralsight: 8 cursos, 29 horas @ no__t-0: La ruta de aprendizaje de Python en Pluralsight ofrece cursos en línea que abarcan una gran variedad de temas relacionados con Python, incluida una herramienta para medir su habilidad y encontrar los huecos.

- [Tutoriales de LearnPython.org](https://www.learnpython.org/): Comience a usar el aprendizaje de Python sin necesidad de instalar o configurar nada con estos tutoriales gratuitos de Python interactivos de la gente en el elemento de información.

- [Los tutoriales de Python.org](https://docs.python.org/3/tutorial/index.html): Presenta el lector de manera informativa a los conceptos básicos y las características del lenguaje y el sistema de Python.

- [Aprendizaje de Python en Lynda.com](https://www.lynda.com/Python-tutorials/Learning-Python/661773-2.html): Una introducción básica a Python.

### <a name="working-with-python-in-vs-code"></a>Trabajar con Python en VS Code

- [Edición de Python en vs Code](https://code.visualstudio.com/docs/python/editing): Obtenga más información sobre cómo aprovechar las ventajas de la compatibilidad con Autocompletar y IntelliSense de VS Code para Python, incluida la forma de personalizar sus behvior... o simplemente desactivarlas.

- Detección de [Python](https://code.visualstudio.com/docs/python/linting): La detección de errores es el proceso de ejecución de un programa que analizará el código para detectar posibles errores. Obtenga información sobre las distintas formas de compatibilidad con la detección de errores VS Code proporciona Python y cómo configurarlo.

- [Depuración de Python](https://code.visualstudio.com/docs/python/debugging): La depuración es el proceso de identificar y quitar errores de un programa. En este artículo se describe cómo inicializar y configurar la depuración de Python con VS Code, cómo establecer y validar puntos de interrupción, adjuntar un script local, realizar la depuración para diferentes tipos de aplicaciones o en un equipo remoto, y algunos procedimientos de solución de problemas básicos.

- [Pruebas unitarias de Python](https://code.visualstudio.com/docs/python/unit-testing): Se tratan algunos antecedentes que explican qué significan las pruebas unitarias, un tutorial de ejemplo, la habilitación de un marco de pruebas, la creación y ejecución de las pruebas, la depuración de pruebas y la configuración de pruebas. 
