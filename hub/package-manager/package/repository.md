---
title: Envío del manifiesto al repositorio
description: Después de crear un manifiesto de paquete que describa su aplicación, estará listo para enviarlo al repositorio del Administrador de paquetes de Windows.
ms.date: 04/29/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ef94a77d5012adcedf31ae1ecfddc036bcc3a059
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166509"
---
# <a name="submit-your-manifest-to-the-repository"></a>Envío del manifiesto al repositorio

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

Después de crear un [manifiesto de paquete](manifest.md) que describa tu aplicación, estarás listo para enviar el manifiesto al repositorio del Administrador de paquetes de Windows. Se trata de un repositorio público que contiene una colección de manifiestos a los que la herramienta **winget** puede acceder. Para enviar tu manifiesto, cárgalo en el repositorio de código abierto [https://github.com/microsoft/winget-pkgs](https://github.com/microsoft/winget-pkgs) en GitHub.

Después de enviar una solicitud de incorporación de cambios para agregar un nuevo manifiesto al repositorio de GitHub, un proceso automatizado validará el archivo de manifiesto y comprobará que se sepa que el paquete no es malintencionado. Si esta validación se completa correctamente, el paquete se agregará al repositorio público del Administrador de paquetes de Windows para la herramienta cliente **winget** pueda detectarlo. Ten en cuenta la diferencia entre los manifiestos del repositorio de código abierto de GitHub y el repositorio público del Administrador de paquetes de Windows.

> [!IMPORTANT]
> Microsoft se reserva el derecho de rechazar un envío por cualquier motivo.

## <a name="third-party-repositories"></a>Repositorios de terceros

Actualmente no hay ningún repositorio de terceros conocido. Microsoft trabaja con varios partners para desarrollar protocolos o una API para habilitar los repositorios de terceros.

## <a name="manifest-validation"></a>Validación del manifiesto

Cuando envías un manifiesto al repositorio [https://github.com/microsoft/winget-pkgs](https://github.com/microsoft/winget-pkgs) en GitHub, el manifiesto se validará y evaluará automáticamente para mantener la seguridad del ecosistema de Windows. Es posible que los manifiestos también se revisen manualmente.

## <a name="how-to-submit-your-manifest"></a>Procedimiento de envío del manifiesto

Para enviar un manifiesto al repositorio, sigue estos pasos.

### <a name="step-1-validate-your-manifest"></a>Paso 1: Validación del manifiesto

La herramienta **winget** ofrece el comando [validate](..\winget\validate.md) para confirmar que has creado el manifiesto correctamente. Para validar el manifiesto, usa este comando.

```CMD
winget validate \<manifest-file>
```

Si se produce un error en la validación, usa los errores para buscar el número de línea y haz una corrección. Una vez que se haya validado el manifiesto, puedes enviarlo al repositorio.

### <a name="step-2-clone-the-repository"></a>Paso 2: Clone el repositorio.

A continuación, crea una bifurcación del repositorio y clónala.

1. Ve a [https://github.com/microsoft/winget-pkgs](https://github.com/microsoft/winget-pkgs) en el explorador y haz clic en **Bifurcar**.
    ![imagen de la bifurcación](images\fork.png)

2. En un entorno de línea de comandos, como el símbolo del sistema de Windows o PowerShell, usa el siguiente comando para clonar la bifurcación.
    ```CMD
    git clone \<your-fork-name>
    ```

 3. Si vas a hacer varios envíos, crea una rama en lugar de una bifurcación. Actualmente solo se permite un archivo de manifiesto por envío.
    ```CMD
    git checkout -b \<branch-name>
    ```

### <a name="step-3-add-your-manifest-to-the-local-repository"></a>Paso 3: Adición del manifiesto al repositorio local

Tienes que agregar el archivo de manifiesto al repositorio en la siguiente estructura de carpetas:

**manifests** / **editor** / **aplicación** / **versión.yaml**

* La carpeta **manifests** es la carpeta raíz de todos los manifiestos del repositorio.
* La carpeta **editor** es el nombre de la empresa que publica el software. Por ejemplo, **Microsoft**.
* La carpeta **aplicación** es el nombre de la aplicación o herramienta. Por ejemplo, **VSCode**.
* **versión.yaml** es el nombre de archivo del manifiesto. El nombre de archivo debe establecerse en la versión actual de la aplicación. Por ejemplo, **1.0.0.yaml**.

>[!IMPORTANT]
> El valor `Id` del manifiesto debe coincidir con los nombres del editor y de la aplicación en la ruta de acceso de la carpeta de manifiesto, y el valor `version` del manifiesto debe coincidir con la versión del nombre de archivo. Para más información, consulta [Creación de un manifiesto de paquete](manifest.md#tips-and-best-practices).

### <a name="step-4-submit-your-manifest-to-the-remote-repository"></a>Paso 4: Envío del manifiesto al repositorio remoto

Ahora estás listo para enviar el nuevo manifiesto al repositorio remoto.

1. Use el comando `add` para preparar el envío.
    ```CMD
    git add manifests\Contoso\ContosoApp\1.0.0.yaml
    ```

2. Use el comando `commit` para confirmar el cambio y proporcionar información sobre el envío.
    ```CMD
    git commit -m "Submitting  ContosoApp version 1.0.0.yaml"
    ```

3. Usa el comando `push` para enviar los cambios al repositorio remoto.
    ```CMD
    git push
    ```

### <a name="step-5-create-a-pull-request"></a>Paso 5: Creación de una solicitud de incorporación de cambios

Después de insertar los cambios, vuelve a [https://github.com/microsoft/winget-pkgs](https://github.com/microsoft/winget-pkgs) y crea una solicitud de incorporación de cambios para fusionar tu bifurcación o rama con la rama **maestra**.

![imagen de la pestaña de solicitud de incorporación de cambios](images\pull-request.png)

## <a name="validation-process"></a>Proceso de validación

Al crear una solicitud de incorporación de cambios, se iniciará un proceso automatizado que valida el manifiesto y procesa la solicitud de incorporación de cambios. Agregamos etiquetas a tu solicitud de incorporación de cambios para que puedas hacer un seguimiento del progreso.

### <a name="submission-expectations"></a>Expectativas de envío

Todos los envíos de aplicaciones al repositorio del Administrador de paquetes de Windows deben tener un comportamiento correcto. Estas son algunas de las expectativas para los envíos:

* El manifiesto cumple los [requisitos de esquemas](manifest.md#manifest-contents).
* Todas las direcciones URL del manifiesto conducen a sitios web seguros.
* El instalador y la aplicación están libres de virus. Es posible que se identifique el paquete como malware por equivocación. Si cree que se trata de un falso positivo, puede enviar el instalador al equipo de Defender para su análisis desde [este vínculo](https://www.microsoft.com/wdsi/filesubmission).
* La aplicación se instala y desinstala correctamente tanto para administradores como para usuarios que no son administradores.
* El instalador admite modos no interactivos.
* Todas las entradas del manifiesto son precisas y no son engañosas.
* El instalador viene directamente del sitio web del editor.

### <a name="pull-request-labels"></a>Etiquetas de la solicitud de incorporación de cambios

Durante la validación, aplicamos una serie de etiquetas a la solicitud de incorporación de cambios para comunicar el progreso.

* **Necesidades: comentarios del autor**: Hay un error en el envío. Te reasignaremos la solicitud de incorporación de cambios. Si no solucionas el problema en un plazo de 10 días, cerraremos la solicitud de incorporación de cambios.
* **Manifest-Validation-Error**: El manifiesto enviado contiene un error de sintaxis.
* **URL-Validation-Error**: Una o varias direcciones URL en el envío no superaron la validación de [SmartScreen](/windows/security/threat-protection/microsoft-defender-smartscreen/microsoft-defender-smartscreen-overview).
* **Binary-Validation-Error**: El instalador de la aplicación enviada no superó la prueba de detección de virus o hay un error de coincidencia de hash.
* **Pull-Request-Error**: Hay un problema con la solicitud de incorporación de cambios. Por ejemplo, la estructura de carpetas no tiene el [formato obligatorio](#step-3-add-your-manifest-to-the-local-repository).
* **Validation-Error**: La aplicación enviada no superó una prueba de validación general.
* **Validation-Installation-Error**: La aplicación enviada no superó la prueba de instalación.
* **Validation-Uninstall-Error**: La aplicación enviada no superó la prueba de desinstalación.
* **Validation-Virus-Scan-Error**: La aplicación enviada no superó la prueba de análisis de virus.
* **Azure-Pipeline-Passed**: El manifiesto ha completado la primera parte de la validación. Después de este paso, la solicitud de incorporación de cambios se asigna a nuestro equipo de pruebas para la validación final.
* **Validation-Completed**: La validación se ha completado y la solicitud de incorporación de cambios se combinará.