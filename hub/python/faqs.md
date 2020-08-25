---
title: Preguntas más frecuentes sobre el uso de Python en Windows.
description: Obtenga ayuda con las respuestas a las preguntas más frecuentes (P+F) sobre el uso de Python en Windows para el desarrollo.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: python, windows 10, microsoft, pip, py.exe, rutas de acceso al archivo, PYTHONPATH, desarrollo de python, empaquetado de python
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 2541186a1dd0f205d88e1e14c146934490afff55
ms.sourcegitcommit: b6138f9565252460ace6fa0acdc2a902e591681a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/15/2020
ms.locfileid: "88243267"
---
# <a name="frequently-asked-questions-about-using-python-on-windows"></a>Preguntas más frecuentes sobre el uso de Python en Windows.

## <a name="why-cant-i-pip-install-a-certain-package"></a>¿Por qué no puedo realizar una "instalación de PIP" de un determinado paquete?

Hay una serie de motivos por los que se producirá un error en la instalación. En la mayoría de los casos, lo mejor que puedes hacer para solucionarlo es ponerte en contacto con el desarrollador del paquete.

La causa más común de los problemas es intentar realizar una instalación en una ubicación para la que no tiene permiso de modificación. Por ejemplo, la ubicación de instalación predeterminada podría requerir privilegios administrativos, pero de forma predeterminada Python no los tendrá. La mejor solución es crear un entorno virtual e realizar la instalación allí.

Algunos paquetes incluyen código nativo que requiere un compilador de C o C++ para la instalación. En general, los desarrolladores de paquetes deben publicar versiones precompiladas, pero a menudo no lo hacen. Es posible que algunos de estos paquetes funcionen si [instalas Build Tools para Visual Studio](https://visualstudio.microsoft.com/downloads/#build-tools-for-visual-studio-2019) y seleccionas la opción de C++. Sin embargo, en la mayoría de los casos, deberás ponerte en contacto con el desarrollador de paquetes.

[Sigue el debate en Stack Overflow](https://stackoverflow.com/questions/4750806/how-do-i-install-pip-on-windows/12476379).

## <a name="what-is-pyexe"></a>¿Qué es py.es?

Puedes acabar con varias versiones de Python instaladas en la máquina porque trabajas en diferentes tipos de proyectos de Python. Dado que todas estas usan el comando `python`, puede que no sea obvia la versión de Python que usas. Como norma, te recomendamos que uses el comando `python3` (o `python3.7` para seleccionar una versión específica).

El [iniciador de py.exe](https://docs.python.org/3/using/windows.html#launcher) seleccionará automáticamente la versión más reciente de Python que hayas instalado. También puedes usar comandos como `py -3.7` para seleccionar una versión determinada o `py --list` para ver qué versiones se pueden usar. **SIN EMBARGO**, el iniciador de py.exe solo funcionará si usas una versión de Python instalada de [python.org](https://www.python.org/downloads/windows/). Al instalar Python desde Microsoft Store, el comando `py` no se **incluye**. Para Linux, macOS, WSL y la versión de Microsoft Store de Python, debes usar el comando `python3` (o `python3.7`).

## <a name="why-does-running-pythonexe-open-the-microsoft-store"></a>¿Por qué se abre Microsoft Store al ejecutar python.exe?

A fin de ayudar a los nuevos usuarios a encontrar una instalación adecuada de Python, hemos agregado una combinación de teclas en Windows que te dirigirá directamente a la versión más reciente del paquete de la comunidad publicado en Microsoft Store. Este paquete se puede instalar fácilmente sin permisos de administrador y reemplazará los comandos predeterminados `python` y `python3` por los reales.

Si ejecutas el archivo ejecutable de acceso directo con cualquier argumento de la línea de comandos, se devolverá un código de error para indicar que Python no se instaló. De este modo, se evita que los scripts y los archivos por lotes abran la aplicación Store cuando dicha apertura probablemente no esté prevista.

Si instalas Python con los instaladores de [python.org](https://www.python.org/downloads/windows/) y seleccionas la opción "Agregar a PATH", el nuevo comando `python` tendrá prioridad sobre la combinación de teclas. Ten en cuenta que hay otros instaladores que pueden agregar `python` con una prioridad _inferior_ respecto a la combinación de teclas integrada.

Puedes deshabilitar las combinaciones de teclas sin instalar Python si abres "Manage app execution aliases" ("Administrar alias de ejecución de la aplicación") desde Inicio, buscas las entradas de Python "Instalador de aplicación" y las cambias a "Desactivado".

## <a name="why-dont-file-paths-work-in-python-when-i-copy-paste-them"></a>¿Por qué no funcionan las rutas de acceso a archivos en Python cuando las copias y las pegas?

Las cadenas de Python usan "secuencias de escape" para los caracteres especiales. Por ejemplo, para insertar un carácter de línea nueva en una cadena, deberías escribir `\n`. Dado que las rutas de acceso a archivos en Windows usan barras diagonales inversas, es posible que algunas partes se conviertan en caracteres especiales.

Para pegar una ruta de acceso como una cadena en Python, agrega el prefijo `r`. Esto indica que se trata de una cadena `raw` y que no se usarán caracteres de escape, excepto "\"(es posible que tengas que quitar la última barra diagonal inversa de la ruta de acceso). Así pues, la ruta de acceso podría tener este aspecto: `r"C:\Users\MyName\Documents\Document.txt"`

Al trabajar con rutas de acceso en Python, te recomendamos que uses el módulo estándar pathlib. De este modo, podrás convertir la cadena en un objeto Path enriquecido que puede realizar manipulaciones de ruta de acceso de forma coherente tanto si usa barras diagonales como barras diagonales inversas, lo que hace que el código funcione mejor en distintos sistemas operativos.

## <a name="what-is-pythonpath"></a>¿Qué es PYTHONPATH?

Python usa la variable de entorno PYTHONPATH para especificar una lista de directorios desde la que se pueden importar los módulos. Cuando se ejecute, puedes inspeccionar la variable `sys.path` para ver los directorios en los que se realizará la búsqueda cuando importes algo.

Para establecer esta variable desde el símbolo del sistema, usa `set PYTHONPATH=list;of;paths`.

Para establecer esta variable desde PowerShell, usa `$env:PYTHONPATH=’list;of;paths’` justo antes de iniciar Python.

**No** te recomendamos establecer esta variable de forma global a través de la configuración **Variables de entorno**, ya que puede que la use cualquier versión de Python en lugar de la que quieras usar.

## <a name="where-can-i-find-help-with-packaging-and-deployment"></a>¿Dónde puedo encontrar ayuda con el empaquetado y la implementación?

[Docker](https://code.visualstudio.com/docs/azure/docker): [La extensión VSCode](https://code.visualstudio.com/docs/azure/docker) te ayuda realizar rápidamente el empaquetado y la implementación con plantillas Dockerfile y docker-compose yml (genera los archivos de Docker adecuados para el proyecto).

[Azure Kubernetes Service (AKS)](https://docs.microsoft.com/azure/aks/) te permite implementar y administrar aplicaciones en contenedores a la vez que escalas recursos a petición.

## <a name="what-if-i-need-to-work-across-different-machines"></a>¿Qué ocurre si necesito trabajar en distintas máquinas?

La extensión [Settings Sync](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync) te permite sincronizar la configuración de VS Code entre diferentes instalaciones mediante GitHub. Si trabajas en diferentes máquinas, te ayuda a mantener el entorno coherente entre ellas.

## <a name="what-if-im-used-to-using-pycharm-atom-sublime-text-emacs-or-vim"></a>¿Qué ocurre si estoy acostumbrado a usar PyCharm, Atom, Sublime Text, Emacs o VIM?

La extensión de VSCode [Keymaps](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Downloads) puede ayudarte a configurar el entorno para que te resulte más familiar.

## <a name="how-do-mac-shortcut-keys-map-to-windows-shortcut-keys"></a>¿Cómo se asignan las teclas de método abreviado de Mac a Windows?

Algunos de los botones del teclado y los métodos abreviados del sistema son ligeramente diferentes entre una máquina Windows y una Macintosh. En esta [guía de transición de Mac a Windows](../dev-environment/mac-to-windows.md) se tratan los conceptos básicos.
