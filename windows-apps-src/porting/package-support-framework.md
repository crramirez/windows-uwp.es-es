---
author: normesta
Description: Fix issues that prevent your desktop application from running in an MSIX container
Search.Product: eADQiWindows 10XVcnh
title: Corregir los problemas que impiden que la aplicación se ejecuta en un contenedor MSIX desktop
ms.author: normesta
ms.date: 07/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, Windows 10, uwp, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 46d5705233af9e8254b9ac89a2d6e9891e90701f
ms.sourcegitcommit: 9e2c34a5ed3134aeca7eb9490f05b20eb9a3e5df
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2018
ms.locfileid: "3986372"
---
# <a name="apply-runtime-fixes-to-an-msix-package-by-using-the-package-support-framework"></a>Aplicar correcciones de tiempo de ejecución a un paquete MSIX usando la estructura de soporte del paquete

El marco de apoyo del paquete es un kit de código abierto que le ayuda a aplicar correcciones a la aplicación de win32 existente cuando no tiene acceso al código fuente, por lo que se puede ejecutar en un contenedor MSIX. El marco de apoyo del paquete ayuda a que la aplicación siga las recomendaciones del entorno en tiempo de ejecución modernos.

Para crear el marco de apoyo del paquete, aprovechamos la tecnología [Detours](https://www.microsoft.com/en-us/research/project/detours) , que es un marco de código abierto desarrollado por Microsoft Research (MSR) y ayuda con la redirección de API y el enlace.

Este marco es código abierto, ligero, y puede utilizarlo para solucionar los problemas de aplicación rápidamente. También le ofrece la oportunidad de consultar con la Comunidad de todo el mundo y construir sobre las inversiones de los demás.

## <a name="a-quick-look-inside-of-the-package-support-framework"></a>Un vistazo rápido dentro del marco de apoyo del paquete

El marco de apoyo del paquete contiene un archivo ejecutable, un archivo DLL del administrador en tiempo de ejecución y un conjunto de correcciones en tiempo de ejecución.

![Marco de apoyo del paquete](images/desktop-to-uwp/package-support-framework.png)

Así es como funciona. Creará un archivo de configuración que especifica la fix(s) que desea aplicar a la aplicación. A continuación, modificará el paquete para que señale al archivo ejecutable del selector de corrección.

Cuando los usuarios inicien la aplicación, el selector de corrección es el primer ejecutable que se ejecuta. Lee el archivo de configuración e inyecta la fix(s) en tiempo de ejecución y el archivo DLL del administrador en tiempo de ejecución en el proceso de aplicación.

![Inyección de DLL de paquete compatibilidad con Framework](images/desktop-to-uwp/package-support-framework-2.png)

El Administrador de tiempo de ejecución, aplica la corrección cuando sea necesario por la aplicación se ejecute dentro de un contenedor MSIX.

Esta guía le ayudará a identificar problemas de compatibilidad de aplicaciones, y buscar, aplicar y extender el tiempo de ejecución de correcciones que se tratan.

<a id="identify" />

## <a name="identify-packaged-application-compatibility-issues"></a>Identificar problemas de compatibilidad de aplicación empaquetada

En primer lugar, cree un paquete para su aplicación. A continuación, instalarlo, ejecutarlo y observar su comportamiento. Recibirá mensajes de error que pueden ayudarle a identificar un problema de compatibilidad. También puede utilizar el [Monitor de procesos](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon) para identificar problemas.  Problemas comunes se refieren a los supuestos de aplicación con respecto a los permisos de ruta de directorio y programa de trabajo.

### <a name="using-process-monitor-to-identify-an-issue"></a>Utilizando el Monitor de procesos para identificar un problema

[Process Monitor](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon) es una utilidad muy eficaz para observar sus resultados y las operaciones de registro y archivo de la aplicación.  Esto puede ayudarle a comprender los problemas de compatibilidad de aplicaciones.  Después de abrir el Monitor de procesos, agregar un filtro (filtro > filtro...) para incluir sólo los eventos desde el archivo ejecutable de la aplicación.

![Filtro de ProcMon App](images/desktop-to-uwp/procmon_app_filter.png)

Aparecerá una lista de eventos. Para muchos de estos eventos, la palabra **correcto** aparece en la columna de **resultados** .

![Eventos ProcMon](images/desktop-to-uwp/procmon_events.png)

Opcionalmente, puede filtrar para mostrar sólo errores sólo los eventos.

![Éxito de ProcMon excluir](images/desktop-to-uwp/procmon_exclude_success.png)

Si sospecha que un error de acceso de sistema de archivos, busque los sucesos sin éxito que se encuentran bajo el SysWOW64/System32 o la ruta de acceso del archivo de paquete. Los filtros también pueden ayudar en este caso, demasiado. En la parte inferior de esta lista y desplácese hacia arriba. Que aparecen en la parte inferior de esta lista de errores más recientemente. Preste más atención a los errores que contengan cadenas como "acceso denegado", y "ruta/nombre no encontrado" y omitir las cosas que no parecen sospechosas. El [PSFSample](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/) tiene dos problemas. Puede ver los asuntos de la lista que aparece en la imagen siguiente.

![ProcMon Config.txt](images/desktop-to-uwp/procmon_config_txt.png)

En el primer número que aparece en esta imagen, la aplicación no puede leer el archivo "Config.txt" que se encuentra en la ruta de acceso "C:\Windows\SysWOW64". Es improbable que la aplicación está intentando hacer referencia directamente a esa ruta de acceso. Más probable es que está intentando leer desde ese archivo mediante una ruta de acceso relativa y de forma predeterminada, "System32/SysWOW64" es el directorio de trabajo de la aplicación. Esto sugiere que la aplicación espera su directorio de trabajo actual se establece en algún lugar en el paquete. Mirar dentro el appx, podemos ver que el archivo existe en el mismo directorio que el ejecutable.

![Config.txt de la aplicación](images/desktop-to-uwp/psfsampleapp_config_txt.png)

El segundo problema aparece en la imagen siguiente.

![El archivo de registro de ProcMon](images/desktop-to-uwp/procmon_logfile.png)

En este número, la aplicación no puede escribir un archivo .log en la ruta de acceso del paquete. Esto sugeriría que una corrección de la redirección de archivos podría ayudar.

<a id="find" />

## <a name="find-a-runtime-fix"></a>Buscar una solución en tiempo de ejecución

La FSP contiene revisiones en tiempo de ejecución puede utilizar ahora mismo, como la corrección de la redirección de archivos.

### <a name="file-redirection-shim"></a>Corrección de redireccionamiento del archivo

Puede utilizar el [Archivo de redirección de corrección](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/develop/FileRedirectionShim) para redirigir los intentos de escribir o leer datos en un directorio que no sea accesible desde una aplicación que se ejecuta en un contenedor MSIX.

Por ejemplo, si su aplicación escribe en un archivo de registro que se encuentra en el mismo directorio que el ejecutable de aplicaciones, puede utilizar la [Corrección de redireccionamiento del archivo](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/develop/FileRedirectionShim) para crear el archivo de registro en otra ubicación, como el almacén de datos local de la aplicación.

### <a name="runtime-fixes-from-the-community"></a>Correcciones de tiempo de ejecución de la Comunidad

Asegúrese de revisar las contribuciones de la Comunidad en nuestra página de [GitHub](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/develop) . Es posible que otros desarrolladores han resuelto un problema similar al suyo y han compartido una corrección en tiempo de ejecución.

## <a name="apply-a-runtime-fix"></a>Aplicar una corrección de tiempo de ejecución

Puede aplicar una corrección de tiempo de ejecución existente con algunas herramientas sencillas de Windows SDK y siguiendo estos pasos.

> [!div class="checklist"]
> * Crear una carpeta de distribución del paquete
> * Obtener los archivos de paquete marco de apoyo
> * Agregarlos al paquete
> * Modificar el manifiesto del paquete
> * Crear un archivo de configuración

Vamos a analizar cada tarea.

### <a name="create-the-package-layout-folder"></a>Crear la carpeta del paquete de diseño

Si ya tiene un archivo .appx, podrá desempaquetar su contenido a una carpeta de diseño que servirá como área de ensayo para el paquete.  Puede hacer esto desde una **x64 nativo símbolo de herramientas de VS 2017**, o manualmente con la ruta de acceso de ubicación SDK en la ruta de acceso de búsqueda ejecutable.

```
makeappx unpack /p PSFSamplePackage_1.0.60.0_AnyCPU_Debug.appx /d PackageContents

```

Esto le dará algo similar a lo siguiente.

![Diseño de paquete](images/desktop-to-uwp/package_contents.png)

Si no tienes un archivo .appx inicial, puede crear la carpeta de paquete y los archivos desde cero.

### <a name="get-the-package-support-framework-files"></a>Obtener los archivos de paquete marco de apoyo

Puede obtener el paquete Nuget PSF mediante Visual Studio. También puede obtener utilizando la herramienta de línea de comandos independiente Nuget.

#### <a name="get-the-package-by-using-visual-studio"></a>Obtiene el paquete utilizando Visual Studio

En Visual Studio, haga clic en el nodo de solución o proyecto y elegir uno de los comandos de administrar paquetes de Nuget.  Búsqueda de **Microsoft.PackageSupportFramework** o **PSF** para encontrar el paquete en Nuget.org. A continuación, instalarlo.

#### <a name="get-the-package-by-using-the-command-line-tool"></a>Obtiene el paquete mediante la herramienta de línea de comandos

Instalar la herramienta de línea de comandos de Nuget desde esta ubicación: https://www.nuget.org/downloads. A continuación, desde la línea de comandos de Nuget, ejecute este comando:

```
nuget install Microsoft.PackageSupportFramework
```

### <a name="add-the-package-support-framework-files-to-your-package"></a>Agregar los archivos de paquete marco de apoyo al paquete

Agregue el requiere 32-bit y 64-bit PSF archivos DLL y ejecutables en el directorio del paquete. Usa la siguiente tabla como guía. También deseará incluir las correcciones en tiempo de ejecución que necesite. En nuestro ejemplo, tenemos la solución de archivo redirección en tiempo de ejecución.

| Ejecutable de la aplicación es x64 | Ejecutable de la aplicación es x86 |
|-------------------------------|-----------|
| [ShimLauncher64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimLauncher/readme.md) |  [ShimLauncher32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimLauncher/readme.md) |
| [ShimRuntime64.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRuntime/readme.md) | [ShimRuntime32.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRuntime/readme.md) |
| [ShimRunDll64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRunDll/readme.md) | [ShimRunDll32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRunDll/readme.md) |

El contenido del paquete debería ser algo parecido a esto.

![Archivos binarios del paquete](images/desktop-to-uwp/package_binaries.png)

### <a name="modify-the-package-manifest"></a>Modificar el manifiesto del paquete

Abra el manifiesto del paquete en un editor de texto y, a continuación, establezca el `Executable` atributo de la `Application` el elemento en el nombre del archivo ejecutable de iniciador de corrección.  Si conoce la arquitectura de la aplicación de destino, seleccione la versión apropiada, ShimLauncher32.exe o ShimLauncher64.exe.  Si no es así, ShimLauncher32.exe funcionará en todos los casos.  Aquí tienes un ejemplo.

```xml
<Package ...>
  ...
  <Applications>
    <Application Id="PSFSample"
                 Executable="ShimLauncher32.exe"
                 EntryPoint="Windows.FullTrustApplication">
      ...
    </Application>
  </Applications>
</Package>
```

### <a name="create-a-configuration-file"></a>Crear un archivo de configuración

Crear un nombre de archivo ``config.json``y guardar el archivo en la carpeta raíz de su paquete. Modificar el identificador declarado de la aplicación del archivo config.json para que apunte al archivo ejecutable que acaba de cambiar. Con el conocimiento que ha obtenido del uso de Monitor de proceso, se puede también establecer el directorio de trabajo además usar la corrección de la redirección de archivos para redirigir lee y escribe archivos .log en el directorio "PSFSampleApp" paquete relativo a.

```json
{
    "applications": [
        {
            "id": "PSFSample",
            "executable": "PSFSampleApp/PSFSample.exe",
            "workingDirectory": "PSFSampleApp/"
        }
    ],
    "processes": [
        {
            "executable": "PSFSample",
            "shims": [
                {
                    "dll": "FileRedirectionShim.dll",
                    "config": {
                        "redirectedPaths": {
                            "packageRelative": [
                                {
                                    "base": "PSFSampleApp/",
                                    "patterns": [
                                        ".*\\.log"
                                    ]
                                }
                            ]
                        }
                    }
                }
            ]
        }
    ]
}
```
Siguiente es una guía para el esquema de config.json:

| Matriz | key | Valor |
|-------|-----------|-------|
| applications | id |  Utilice el valor de la `Id` atributo de la `Application` el elemento en el manifiesto del paquete. |
| applications | ejecutable | La ruta de acceso relativa del paquete para el archivo ejecutable que desea iniciar. En la mayoría de los casos, puede obtener este valor desde el archivo de manifiesto del paquete antes de modificarlo. Es el valor de la `Executable` atributo de la `Application` elemento. |
| applications | workingDirectory | (Opcional) Una ruta de acceso relativa del paquete para utilizar como directorio de trabajo de la aplicación que se inicia. Si no establece este valor, el sistema operativo utiliza el `System32` directorio como directorio de trabajo de la aplicación. |
| procesos | ejecutable | En la mayoría de los casos, será el nombre de la `executable` configurado anteriormente con la extensión de la ruta de acceso y quitar. |
| correcciones de compatibilidad | DLL | Ruta de acceso de paquete relativa a la .appx de la corrección para cargar. |
| correcciones de compatibilidad | config | (Opcional) Controla cómo se comporta la lista de distribución de corrección. El formato exacto de este valor varía según corrección por shim como cada corrección puede interpretar este "objeto binario" como desee. |

El `applications`, `processes`, y `shims` claves son matrices. Esto significa que puede utilizar el archivo config.json para especificar más de una aplicación, proceso y DLL de la corrección.


### <a name="package-and-test-the-app"></a>Paquete y prueba de la aplicación

A continuación, cree un paquete.

```
makeappx pack /d PackageContents /p PSFSamplePackageFixup.appx
```

A continuación, firmarlo.

```
signtool sign /a /v /fd sha256 /f ExportedSigningCertificate.pfx PSFSamplePackageFixup.appx
```

Para obtener más información, vea [cómo crear un certificado de firma de paquete](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/how-to-create-a-package-signing-certificate) y [firmar un paquete si usa signtool](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/how-to-sign-a-package-using-signtool)

Uso de PowerShell, instale el paquete.

>[!NOTE]
> Recuerde que debe desinstalar primero el paquete.

```
powershell Add-AppxPackage .\PSFSamplePackageFixup.appx
```

Ejecute la aplicación y observe el comportamiento con corrección de tiempo de ejecución aplica.  Repita el diagnóstico y los pasos de empaquetado según sea necesario.

### <a name="use-the-trace-shim"></a>Utilizar la corrección de seguimiento

Una técnica alternativa para diagnosticar problemas de compatibilidad de aplicación empaquetada es utilizar la traza de la corrección. Esta DLL se incluye con el PSF y proporciona una vista detallada de diagnóstico del comportamiento de la aplicación, similar al Monitor de procesos.  Se diseña especialmente para revelar problemas de compatibilidad de aplicaciones.  Para utilizar la corrección de la traza, agregar la DLL al paquete, agregar el fragmento siguiente a su config.json y, a continuación, empaquetar e instalar la aplicación.

```json
{
    "dll": "TraceShim.dll",
    "config": {
        "traceLevels": {
            "filesystem": "allFailures"
        }
    }
}
```

De forma predeterminada, la corrección de la traza filtra los errores que puedan considerarse "esperados".  Por ejemplo, las aplicaciones podrían intentar incondicionalmente eliminar un archivo sin comprobar si ya existe, omitiendo el resultado. Esto tiene como consecuencia una pena que algunos errores inesperados podrían obtener filtran, así que en el ejemplo anterior, optamos para recibir todos los fallos de las funciones del sistema de archivos. Hacemos esto porque sabemos desde antes de que el intento de leer el archivo Config.txt se produce un error con el mensaje "archivo no encontrado". Se trata de un error que se observa con frecuencia y generalmente no se supone que es inesperado. En la práctica es probable que mejor iniciar filtrado sólo para errores inesperados y, a continuación, recurrir a todos los errores si hay un problema que todavía no se puede identificar.

De forma predeterminada, se envía la salida de la traza de la corrección para el depurador asociado. Para este ejemplo, no va a adjuntar a un depurador y en su lugar utilizará el programa [DebugView](https://docs.microsoft.com/en-us/sysinternals/downloads/debugview) de SysInternals para ver los resultados. Después de ejecutar la aplicación, podemos ver los mismos errores que antes, que nos señalaría hacia las mismas correcciones en tiempo de ejecución.

![No se encontró el archivo TraceShim](images/desktop-to-uwp/traceshim_filenotfound.png)

![TraceShim acceso denegado](images/desktop-to-uwp/traceshim_accessdenied.png)

## <a name="debug-extend-or-create-a-runtime-fix"></a>Depurar, ampliar o crear una solución en tiempo de ejecución

Puede utilizar Visual Studio para depurar una solución en tiempo de ejecución, ampliar una corrección en tiempo de ejecución o cree uno desde cero. Debe hacer estas cosas se realice correctamente.

> [!div class="checklist"]
> * Agregue un proyecto de empaquetado
> * Agregar proyecto para la corrección de tiempo de ejecución
> * Agregar un proyecto que se inicia el Selector de corrección ejecutable
> * Configurar el proyecto de empaquetado

Cuando haya terminado, la solución será similar a éste.

![Solución completa](images/desktop-to-uwp/runtime-fix-project-structure.png)

Echemos un vistazo a cada proyecto en este ejemplo.

| Proyecto | Propósito |
|-------|-----------|
| DesktopApplicationPackage | Este proyecto se basa en el [proyecto del paquete de aplicaciones de Windows](desktop-to-uwp-packaging-dot-net.md) y genera el paquete MSIX. |
| Runtimefix | Se trata de un proyecto de biblioteca de Dynamic-Linked de C++ que contiene una o más funciones de sustitución que actúan como la corrección de tiempo de ejecución. |
| ShimLauncher | Se trata de un proyecto vacío de C++. Este proyecto es un lugar para recopilar los archivos de tiempo de ejecución distribuible el paquete del marco de apoyo. Genera un archivo ejecutable. Ese archivo ejecutable es lo primero que se ejecuta al iniciar la solución. |
| WinFormsDesktopApplication | Este proyecto contiene el código fuente de una aplicación de escritorio. |

Para ver un ejemplo completo que contiene todos estos tipos de proyectos, consulte [PSFSample](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/).

Ahora describiremos los pasos para crear y configurar cada uno de estos proyectos en la solución.


### <a name="create-a-package-solution"></a>Crear un paquete de soluciones

Si ya no tiene una solución para su aplicación de escritorio, crear una nueva **Solución en blanco** en Visual Studio.

![Solución en blanco](images/desktop-to-uwp/blank-solution.png)

También puede agregar los proyectos de aplicación que tiene.

### <a name="add-a-packaging-project"></a>Agregue un proyecto de empaquetado

Si ya no tienes un **Proyecto de empaquetado de aplicación de Windows**, cree uno y agregarlo a la solución.

![Plantilla de proyecto paquete](images/desktop-to-uwp/package-project-template.png)

Para obtener más información sobre el proyecto del paquete de aplicaciones de Windows, consulte el [paquete de la aplicación utilizando Visual Studio](desktop-to-uwp-packaging-dot-net.md).

En el **Explorador de soluciones**, haga clic en el proyecto de envase, seleccione **Editar**y, a continuación, agregue lo siguiente a la parte inferior del archivo de proyecto:

```
<Target Name="PSFRemoveSourceProject" AfterTargets="ExpandProjectReferences" BeforeTargets="_ConvertItems">
<ItemGroup>
  <FilteredNonWapProjProjectOutput Include="@(_FilteredNonWapProjProjectOutput)">
  <SourceProject Condition="'%(_FilteredNonWapProjProjectOutput.SourceProject)'=='<your runtime fix project name goes here>'" />
  </FilteredNonWapProjProjectOutput>
  <_FilteredNonWapProjProjectOutput Remove="@(_FilteredNonWapProjProjectOutput)" />
  <_FilteredNonWapProjProjectOutput Include="@(FilteredNonWapProjProjectOutput)" />
</ItemGroup>
</Target>
```

### <a name="add-project-for-the-runtime-fix"></a>Agregar proyecto para la corrección de tiempo de ejecución

Agregue un proyecto de **Biblioteca de vínculos dinámicos (DLL)** de C++ a la solución.

![Biblioteca de corrección de tiempo de ejecución](images/desktop-to-uwp/runtime-fix-library.png)

Con el botón secundario que del proyecto y, a continuación, elija **Propiedades**.

En las páginas de propiedades, busque el campo **Estándar del lenguaje de C++** y, a continuación, en la lista desplegable situada junto a ese campo, seleccione la **ISO C ++ 17 estándar (/ std:c ++ 17)** opción.

![ISO 17 opción](images/desktop-to-uwp/iso-option.png)

Haga clic en ese proyecto y, a continuación, en el menú contextual, elija la opción **Administrar paquetes de Nuget** . Asegúrese de que la opción de **origen del paquete** se establece en **todas** o en **nuget.org**.

Haga clic en el icono de configuración siguiente ese campo.

Busque el *PSF** Nuget empaquetar y, a continuación, vuelva a instalarlo para este proyecto.

![paquete de NuGet](images/desktop-to-uwp/psf-package.png)

Si desea depurar o ampliar una corrección en tiempo de ejecución existente, agregue los archivos de revisión en tiempo de ejecución que obtienen mediante el uso de las instrucciones que se describen en la sección [Buscar una solución en tiempo de ejecución](#find) de esta guía.

Si piensa crear una solución nueva, no agregue nada a este proyecto todavía. Le ayudaremos a agregar los archivos correctos para este proyecto más adelante en esta guía. Por ahora, vamos a continuar configurar la solución.

### <a name="add-a-project-that-starts-the-shim-launcher-executable"></a>Agregar un proyecto que se inicia el Selector de corrección ejecutable

Agregue un proyecto de C++ **Proyecto vacío** a la solución.

![Proyecto vacío](images/desktop-to-uwp/blank-app.png)

Agregar el paquete de Nuget **PSF** a este proyecto mediante la misma guía, que se describe en la sección anterior.

Abrir las páginas de propiedades para el proyecto y en la página configuración **General** , establezca la propiedad de **Nombre de destino** en ``ShimLauncher32`` o ``ShimLauncher64`` según la arquitectura de la aplicación.

![referencia del selector de corrección](images/desktop-to-uwp/shim-exe-reference.png)

Agregar una referencia de proyecto al proyecto de corrección de tiempo de ejecución de la solución.

![referencia de corrección de tiempo de ejecución](images/desktop-to-uwp/reference-fix.png)

(Ratón) en la referencia y, a continuación, en la ventana **Propiedades** , aplique estos valores.

| Propiedad | Valor |
|-------|-----------|-------|
| Copia local | Verdadero |
| Copiar ensamblados satélite locales | Verdadero |
| Salida del ensamblado de referencia | Verdadero |
| Vincular dependencias de biblioteca | Falso |
| Entradas de dependencia de biblioteca de vínculo | Falso |

### <a name="configure-the-packaging-project"></a>Configurar el proyecto de empaquetado

En el proyecto de empaquetado, haz clic con el botón derecho en la carpeta **Applications** y, a continuación, elige **Agregar referencia**.

![Agregar referencia de proyecto](images/desktop-to-uwp/add-reference-packaging-project.png)

Elija el proyecto de iniciador de la corrección y el proyecto de aplicación de escritorio y, a continuación, elija el botón **Aceptar** .

![Proyecto de escritorio](images/desktop-to-uwp/package-project-references.png)

>[!NOTE]
> Si no tiene el código fuente para la aplicación, simplemente elija el proyecto de iniciador de corrección. Le mostraremos cómo hacer referencia a su archivo ejecutable cuando se crea un archivo de configuración.

En el nodo **aplicaciones** , (ratón) en la aplicación de iniciador de corrección y, a continuación, elija **establecer como punto de entrada**.

![Establecer punto de entrada](images/desktop-to-uwp/set-startup-project.png)

Agregue un archivo denominado ``config.json`` a su proyecto de envase, a continuación, copie y pegue el siguiente texto json en el archivo. Establezca la propiedad **Action del paquete** al **contenido**.

```json
{
    "applications": [
        {
            "id": "",
            "executable": "",
            "workingDirectory": ""
        }
    ],
    "processes": [
        {
            "executable": "",
            "shims": [
                {
                    "dll": "",
                    "config": {
                    }
                }
            ]
        }
    ]
}
```
Proporcionar un valor para cada clave. Utilice esta tabla como guía.

| Matriz | key | Valor |
|-------|-----------|-------|
| applications | id |  Utilice el valor de la `Id` atributo de la `Application` el elemento en el manifiesto del paquete. |
| applications | ejecutable | La ruta de acceso relativa del paquete para el archivo ejecutable que desea iniciar. En la mayoría de los casos, puede obtener este valor desde el archivo de manifiesto del paquete antes de modificarlo. Es el valor de la `Executable` atributo de la `Application` elemento. |
| applications | workingDirectory | (Opcional) Una ruta de acceso relativa del paquete para utilizar como directorio de trabajo de la aplicación que se inicia. Si no establece este valor, el sistema operativo utiliza el `System32` directorio como directorio de trabajo de la aplicación. |
| procesos | ejecutable | En la mayoría de los casos, será el nombre de la `executable` configurado anteriormente con la extensión de la ruta de acceso y quitar. |
| correcciones de compatibilidad | DLL | Ruta de acceso de paquete relativa a la corrección DLL para cargar. |
| correcciones de compatibilidad | config | (Opcional) Controla cómo se comporta la lista de distribución de corrección. El formato exacto de este valor varía según corrección por shim como cada corrección puede interpretar este "objeto binario" como desee. |

Cuando haya terminado, el ``config.json`` archivo tendrá un aspecto parecido al siguiente.

```json
{
  "applications": [
    {
      "id": "DesktopApplication",
      "executable": "DesktopApplication/WinFormsDesktopApplication.exe",
      "workingDirectory": "WinFormsDesktopApplication"
    }
  ],
  "processes": [
    {
      "executable": ".*App.*",
      "shims": [ { "dll": "RuntimeFix.dll" } ]
    }
  ]
}

```

>[!NOTE]
> El `applications`, `processes`, y `shims` claves son matrices. Esto significa que puede utilizar el archivo config.json para especificar más de una aplicación, proceso y DLL de la corrección.

### <a name="debug-a-runtime-fix"></a>Depurar una solución en tiempo de ejecución

En Visual Studio, presione F5 para iniciar al depurador.  Lo primero que se inicia es la aplicación de iniciador de corrección, que a su vez, se inicia la aplicación de escritorio de destino.  Para depurar la aplicación de escritorio de destino, tendrá que adjuntar al proceso de aplicación de escritorio manualmente seleccionando **Depurar**->**asociar al proceso**y, a continuación, seleccionar el proceso de aplicación. Para permitir la depuración de una aplicación .NET con una DLL de corrección nativo en tiempo de ejecución, seleccione los tipos de código nativo y administrado (depuración en modo mixto).  

Una vez que haya configurado esto, puede establecer puntos de interrupción junto a líneas de código en el código de la aplicación de escritorio y el proyecto de corrección de tiempo de ejecución. Si no tiene el código fuente para la aplicación, podrá establecer puntos de interrupción sólo junto a líneas de código en el proyecto de corrección de tiempo de ejecución.

>[!NOTE]
> Aunque Visual Studio ofrece el desarrollo más sencillo y la experiencia en depuración, hay algunas limitaciones, más adelante en esta guía, analizaremos otras técnicas de depuración que se pueden aplicar.

## <a name="create-a-runtime-fix"></a>Crear una solución en tiempo de ejecución

Si no hay todavía un runtime de corrección para el problema que desea resolver, puede crear una nueva revisión de tiempo de ejecución al escribir funciones de reemplazo e incluidos los datos de configuración tiene sentido. Echemos un vistazo a cada parte.

### <a name="replacement-functions"></a>Funciones de reemplazo

En primer lugar, identificar qué función llamadas errores cuando la aplicación se ejecuta en un contenedor MSIX. A continuación, puede crear funciones de sustitución que desea llamar en su lugar el Administrador de tiempo de ejecución. Esto le da una oportunidad de reemplazar la implementación de una función con el comportamiento que se ajusta a las reglas del entorno en tiempo de ejecución modernos.

En Visual Studio, abra el proyecto de corrección de tiempo de ejecución que creó anteriormente en esta guía.

Declarar el ``SHIM_DEFINE_EXPORTS`` macro y, a continuación, agregue una instrucción include para el `shim_framework.h` en la parte superior de cada uno. Archivo CPP donde se va a agregar las funciones de la revisión en tiempo de ejecución.

```c++
#define SHIM_DEFINE_EXPORTS
#include <shim_framework.h>
```
>[!IMPORTANT]
>Asegúrese de que el `SHIM_DEFINE_EXPORTS` macro aparece antes de la instrucción include.

Crear una función que tiene la misma firma de la función que tiene comportamiento desee modificar. Aquí es una función de ejemplo reemplaza el `MessageBoxW` función.

```c++
auto MessageBoxWImpl = &::MessageBoxW;
int WINAPI MessageBoxWShim(
    _In_opt_ HWND hwnd,
    _In_opt_ LPCWSTR,
    _In_opt_ LPCWSTR caption,
    _In_ UINT type)
{
    return MessageBoxWImpl(hwnd, L"SUCCESS: This worked", caption, type);
}

DECLARE_SHIM(MessageBoxWImpl, MessageBoxWShim);
```

La llamada a `DECLARE_SHIM` asigna el `MessageBoxW` a su nueva función de reemplazo de la función. Cuando la aplicación intenta llamar a la `MessageBoxW` , la función llamará a la función de reemplazo en su lugar.

#### <a name="protect-against-recursive-calls-to-functions-in-runtime-fixes"></a>Protección contra las llamadas recursivas a funciones en tiempo de ejecución correcciones

Opcionalmente, puede aplicar el `reentrancy_guard` tipo de las funciones que protegen frente a las llamadas recursivas a funciones en tiempo de ejecución correcciones.

Por ejemplo, podría generar una función de reemplazo para el `CreateFile` función. Podría llamar la implementación de la `CopyFile` función, pero la implementación de la `CopyFile` podría llamar la función de la `CreateFile` función. Esto puede conducir a un ciclo infinito recursiva de llamadas a la `CreateFile` función.

Para obtener más información en `reentrancy_guard` ver [authoring.md](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/Authoring.md)

### <a name="configuration-data"></a>Datos de configuración

Si desea agregar los datos de configuración para su corrección en tiempo de ejecución, es recomendable añadirlo a la ``config.json``. De este modo, puede utilizar el `ShimQueryCurrentDllConfig` para analizar fácilmente los datos. En este ejemplo se analiza un valor boolean y string de ese archivo de configuración.

```c++
if (auto configRoot = ::ShimQueryCurrentDllConfig())
{
    auto& config = configRoot->as_object();

    if (auto enabledValue = config.try_get("enabled"))
    {
        g_enabled = enabledValue->as_boolean().get();
    }

    if (auto logPathValue = config.try_get("logPath"))
    {
        g_logPath = logPathValue->as_string().wstring();
    }
}
```

## <a name="other-debugging-techniques"></a>Otras técnicas de depuración

Aunque Visual Studio proporciona el desarrollo más sencillo y la experiencia de depuración, hay algunas limitaciones.

En primer lugar, la depuración de F5, ejecuta la aplicación implementar archivos sueltos de la ruta de carpeta de diseño de paquete, en lugar de instalar un paquete de .appx.  La carpeta de diseño normalmente no tiene las mismas restricciones de seguridad que una carpeta de paquete instalado. Como resultado, no sería posible reproducir errores de denegación de acceso de ruta de acceso de paquete antes de aplicar una corrección de tiempo de ejecución.

Para solucionar este problema, utilice la implementación del paquete .appx en lugar de la implementación de archivos sueltos de F5.  Para crear un archivo de paquete de .appx, utilice la utilidad [MakeAppx](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/make-appx-package--makeappx-exe-) desde el SDK de Windows, como se describió anteriormente. O bien, desde dentro de Visual Studio, haga clic en el nodo del proyecto de aplicación y seleccione **almacén**->**Crear paquetes de aplicación**.

Otro problema con Visual Studio es que no tiene compatibilidad integrada para adjuntar a los procesos secundarios que se inicia el depurador.   Esto dificulta la depuración lógica en la ruta de inicio de la aplicación de destino, que se debe vincular manualmente por Visual Studio después de lanzamiento.

Para solucionar este problema, utilice a un depurador que permite la conexión de proceso secundario.  Observe que normalmente no es posible adjuntar a un depurador de just-in-time (JIT) a la aplicación de destino.  Esto es como la mayoría de las técnicas JIT implica iniciando al depurador en lugar de la aplicación de destino, a través de la clave del registro ImageFileExecutionOptions.  Esto vence el mecanismo detouring utilizado ShimLauncher.exe para inyectar ShimRuntime.dll en la aplicación de destino.  WinDbg, incluida en las [Herramientas de depuración para Windows](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/index)y la obtenida desde el [SDK de Windows](https://developer.microsoft.com/en-US/windows/downloads/windows-10-sdk), proceso de secundario admite adjuntar.  Asimismo, soporta directamente [iniciar y depurar una aplicación UWP](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-a-uwp-app-using-windbg#span-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanlaunching-and-debugging-a-uwp-app).

Para depurar el inicio de la aplicación de destino como un proceso secundario, iniciar ``WinDbg``.

```
windbg.exe -plmPackage PSFSampleWithFixup_1.0.59.0_x86__7s220nvg1hg3m -plmApp PSFSample
```

En el ``WinDbg`` preguntar, habilitar la depuración de niño y establecer puntos de interrupción apropiados.

```
.childdbg 1
g
```
(ejecutar hasta que la aplicación de destino se inicia y se entra en el depurador)

```
sxe ld fixup.dll
g
```
(ejecutar hasta que la reparación que se carga la DLL)

```
bp ...
```

>[!NOTE]
> [PLMDebug](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/plmdebug) también se puede utilizar para asociar a un depurador a una aplicación en el inicio y también se incluye en las [Herramientas de depuración para Windows](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/index).  Sin embargo, es más complejo de utilizar que el apoyo directo ahora de WinDbg.

## <a name="support-and-feedback"></a>Soporte técnico y comentarios

**Encuentra respuestas a tus preguntas**

¿Tienes alguna pregunta? Pregúntanos en Stack Overflow. Nuestro equipo supervisa estas [etiquetas](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). También puedes preguntarnos [aquí](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).
