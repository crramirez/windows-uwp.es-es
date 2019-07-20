---
title: Preguntas más frecuentes sobre el uso de Python en Windows
description: Preguntas más frecuentes sobre el uso de Python en Windows
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.prod: windows
ms.technology: hub
keywords: Python, Windows 10, Microsoft, PIP, py. exe, rutas de acceso de archivo, PYTHONPATH, implementación de Python, empaquetado de Python
ms.localizationpriority: medium
ms.date: 07/19/2019
ms.openlocfilehash: 4f7f5c325dfd114093e1434259489459a8c78151
ms.sourcegitcommit: 161eac985af11faaff78797d86343d4fa7d6a05f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2019
ms.locfileid: "68366731"
---
# <a name="frequently-asked-questions-about-using-python-on-windows"></a>Preguntas más frecuentes sobre el uso de Python en Windows

## <a name="why-cant-i-pip-install-a-certain-package"></a>¿Por qué no se puede "instalar PIP" un determinado paquete?

Hay una serie de motivos por los que se producirá un error en la instalación. en la mayoría de los casos, la solución correcta es ponerse en contacto con el desarrollador del paquete.

La causa más común de los problemas es intentar instalar en una ubicación para la que no tiene permiso de modificación. Por ejemplo, la ubicación de instalación predeterminada podría requerir privilegios administrativos, pero de forma predeterminada Python no las tendrá. La mejor solución es crear un entorno virtual e instalarlo allí.

Algunos paquetes incluyen código nativo que requiere un C o C++ un compilador para instalar. En general, los desarrolladores de paquetes deben publicar versiones previamente compiladas, pero a menudo no lo hacen. Algunos de estos paquetes pueden funcionar si [instala las herramientas de compilación para Visual Studio](https://visualstudio.microsoft.com/downloads/#build-tools-for-visual-studio-2019) y selecciona C++ la opción; sin embargo, en la mayoría de los casos, deberá ponerse en contacto con el desarrollador de paquetes.

[Siga la explicación de stackoverflow](https://stackoverflow.com/questions/4750806/how-do-i-install-pip-on-windows/12476379).

## <a name="what-is-pyexe"></a>¿Qué es py. exe?

Puede acabar con varias versiones de Python instaladas en el equipo porque está trabajando en diferentes tipos de proyectos de Python. Dado que todos usan el `python` comando, es posible que no sea obvio cuál es el que está usando. El [iniciador de py. exe](https://docs.python.org/3/using/windows.html#launcher) seleccionará automáticamente la versión más reciente de Python que haya instalado. También puede usar comandos como `py -3.7` para seleccionar una versión determinada o `py --list` para ver qué versiones se pueden usar. **Sin embargo**, el iniciador de py. exe solo funcionará si usa una versión de Python instalada desde [Python.org](https://www.python.org/downloads/windows/). Al instalar Python desde el Microsoft Store, no se `py` **incluye**el comando. Para Linux, MacOS, WSL y la versión Microsoft Store de Python, debe usar el `python3` comando.

## <a name="why-dont-file-paths-work-in-python-when-i-copy-paste-them"></a>¿Por qué no funcionan las rutas de acceso de archivo en Python cuando las copia y las pega?

Las cadenas de Python usan "secuencias de escape" para los caracteres especiales. Por ejemplo, para insertar un nuevo carácter de línea en una cadena, debe escribir `\n`. Dado que las rutas de acceso de archivo en Windows usan barras diagonales inversas, es posible que algunas partes se conviertan en caracteres especiales.

Para pegar una ruta de acceso como una cadena en Python, `r` agregue el prefijo. Esto indica que es una `raw` cadena y que no se usarán caracteres de escape excepto \ "(es posible que tenga que quitar la última barra diagonal inversa de la ruta de acceso). Por lo tanto, la ruta de acceso podría ser similar a la siguiente: r "C:\Users\MyName\Documents\Document.txt"

Al trabajar con rutas de acceso en Python, se recomienda usar el módulo estándar pathlib. Esto le permitirá convertir la cadena en un objeto de trazado enriquecido que puede realizar manipulaciones de ruta de acceso de forma coherente tanto si usa barras diagonales inversas como barras diagonales inversas, lo que hace que el código funcione mejor en distintos sistemas operativos.

## <a name="what-is-pythonpath"></a>¿Qué es PYTHONPATH?

Python usa la variable de entorno PYTHONPATH para especificar una lista de directorios desde los que se pueden importar los módulos. Al ejecutar, puede inspeccionar la `sys.path` variable para ver los directorios en los que se buscará al importar algo.

Para establecer esta variable desde el símbolo del sistema, use `set PYTHONPATH=list;of;paths`:.

Para establecer esta variable desde PowerShell, use: `$env:PYTHONPATH=’list;of;paths’` justo antes de iniciar Python.

**No** se recomienda establecer esta variable globalmente a través de las opciones de configuración de **variables de entorno** , ya que puede ser utilizada por cualquier versión de Python en lugar de la que pretenda usar.

## <a name="where-can-i-find-help-with-packaging-and-deployment"></a>¿Dónde puedo encontrar ayuda con el empaquetado y la implementación?

[Docker](https://code.visualstudio.com/docs/azure/docker): La [extensión VSCode](https://code.visualstudio.com/docs/azure/docker) le ayuda a empaquetar e implementar rápidamente con plantillas Dockerfile y Docker-Compose. yml (genere los archivos de Docker adecuados para el proyecto).

[Azure Kubernetes Service (AKS)](https://docs.microsoft.com/azure/aks/) permite implementar y administrar aplicaciones en contenedores a la vez que se escalan recursos a petición.

## <a name="what-if-i-need-to-work-across-different-machines"></a>¿Qué ocurre si necesito trabajar en distintos equipos?

La sincronización de la [configuración](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync) le permite sincronizar la configuración del vs Code en distintas instalaciones mediante github. Si trabaja en diferentes equipos, esto ayuda a mantener el entorno coherente entre ellos.

## <a name="what-if-im-used-to-using-pycharm-atom-sublime-text-emacs-or-vim"></a>¿Qué ocurre si utilizo usar PyCharm, Atom, Sublime Text, Emacs o VIM?

La extensión VSCode [keymaps](https://marketplace.visualstudio.com/search?target=VSCode&category=Keymaps&sortBy=Downloads) puede ayudar a su entorno a su gusto en casa.

## <a name="how-do-mac-shortcut-keys-map-to-windows-shortcut-keys"></a>¿Cómo se asignan las teclas de método abreviado a Windows?

Algunos de los botones del teclado y los métodos abreviados del sistema son ligeramente diferentes entre una máquina de Windows y un equipo Macintosh. Esta [Guía de asignación de teclado](https://support.microsoft.com/help/970299/keyboard-mappings-using-a-pc-keyboard-on-a-macintosh) de soporte técnico de Microsoft trata los aspectos básicos.
