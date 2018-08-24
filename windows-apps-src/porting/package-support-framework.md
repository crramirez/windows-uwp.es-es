---
author: normesta
Description: Fix issues that prevent your desktop application from running in an MSIX container
Search.Product: eADQiWindows 10XVcnh
title: Solución de problemas que impiden que su aplicación de escritorio que se ejecute en un contenedor MSIX
ms.author: normesta
ms.date: 07/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, Windows 10, uwp, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 46d5705233af9e8254b9ac89a2d6e9891e90701f
ms.sourcegitcommit: c6d6f8b54253e79354f8db14e5cf3b113a3e5014
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "2841367"
---
# <a name="apply-runtime-fixes-to-an-msix-package-by-using-the-package-support-framework"></a>Aplicar las revisiones de tiempo de ejecución a un paquete MSIX mediante el marco de soporte técnico de paquete

El marco de trabajo de paquete de compatibilidad es un kit de código abierto que le ayudará a aplicar las revisiones a la aplicación existente de win32 cuando no tiene acceso al código fuente, para que se puede ejecutar en un contenedor MSIX. El marco de trabajo de paquete de compatibilidad con ayuda a que la aplicación siga los procedimientos recomendados del entorno de tiempo de ejecución moderno.

Para crear el marco de trabajo de paquete de compatibilidad, aprovechamos la tecnología [Detours](https://www.microsoft.com/en-us/research/project/detours) que es un marco de trabajo de código abierto desarrollado por Microsoft Research (MSR) y ayuda con la redirección de API y el enlace.

Este marco de trabajo es de código abierto, ligero, y puede usarlo para resolver problemas de aplicación rápidamente. También le ofrece la oportunidad de consultar con la Comunidad de todo el mundo y para crear encima de las inversiones de otras personas.

## <a name="a-quick-look-inside-of-the-package-support-framework"></a>Un vistazo rápido dentro el marco de trabajo de paquete de compatibilidad

El marco de soporte técnico de paquete contiene un archivo ejecutable, un archivo DLL del Administrador de tiempo de ejecución y un conjunto de correcciones de tiempo de ejecución.

![Marco de apoyo de paquete](images/desktop-to-uwp/package-support-framework.png)

Así es como funciona. Creación de un archivo de configuración que especifica la fix(s) que desea aplicar a la aplicación. A continuación, podrá modificar el paquete para que apunte al archivo ejecutable de compatibilidad (shim) del iniciador.

Cuando los usuarios inicien la aplicación, el iniciador de compatibilidad (shim) es el primer ejecutable que se ejecuta. Lee el archivo de configuración e inserta el fix(s) de tiempo de ejecución y el archivo DLL del Administrador de tiempo de ejecución en el proceso de aplicación.

![Inserción de DLL de paquete compatibilidad con Framework](images/desktop-to-uwp/package-support-framework-2.png)

El Administrador de tiempo de ejecución, aplica la corrección cuando sea necesario por la aplicación para ejecutarse dentro de un contenedor MSIX.

Esta guía le ayudará a identificar problemas de compatibilidad de aplicaciones, y buscar, aplicar y extender el tiempo de ejecución de correcciones que solucionarlos.

<a id="identify" />

## <a name="identify-packaged-application-compatibility-issues"></a>Identificar problemas de compatibilidad de paquete de la aplicación

En primer lugar, cree un paquete para su aplicación. A continuación, volver a instalarlo, ejecutarlo y observar su comportamiento. Es posible que reciba mensajes de error que pueden ayudar a identificar un problema de compatibilidad. También puede usar el [Monitor de procesos](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon) para identificar problemas.  Problemas comunes relacionados con la suposición de aplicación con respecto a los permisos de ruta de acceso de Active directory y el programa de trabajo.

### <a name="using-process-monitor-to-identify-an-issue"></a>Usar el Monitor de proceso para identificar un problema

[Process Monitor](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon) es una utilidad muy eficaz para observar el archivo de la aplicación y las operaciones del registro y sus resultados.  Esto puede ayudarle a comprender los problemas de compatibilidad de aplicaciones.  Después de abrir el Monitor de procesos, agregar un filtro (filtro > filtro...) para incluir sólo los eventos desde el archivo ejecutable de la aplicación.

![Filtro de aplicación ProcMon](images/desktop-to-uwp/procmon_app_filter.png)

Aparecerá una lista de eventos. Para muchos de estos eventos, la palabra aparecerá **correcto** en la columna **resultado** .

![Eventos ProcMon](images/desktop-to-uwp/procmon_events.png)

Opcionalmente, puede filtrar los eventos para mostrar sólo los errores sólo.

![Excluir ProcMon éxito](images/desktop-to-uwp/procmon_exclude_success.png)

Si sospecha que un error de acceso del sistema de archivos, busque eventos con errores que se encuentran en el SysWOW64/System32 o la ruta de acceso del archivo de paquete. Filtros también pueden ayudar a aquí, demasiado. Iniciar en la parte inferior de esta lista y desplácese hacia arriba. Los errores que aparecen en la parte inferior de esta lista se han producido recientemente. Preste atención mayoría a errores que contienen las cadenas como "acceso denegado" y "no se encontró la ruta de acceso/nombre" y omitir las cosas que no quedan sospechosas. El [PSFSample](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/) tiene dos problemas. Puede ver los asuntos en la lista que aparece en la siguiente imagen.

![ProcMon Config.txt](images/desktop-to-uwp/procmon_config_txt.png)

En el primer problema que aparece en esta imagen, la aplicación no se puede leer desde el archivo "Config.txt" que se encuentra en la ruta de acceso "C:\Windows\SysWOW64". Es poco probable que la aplicación está intentando hacer referencia directamente a esa ruta de acceso. Más probable es que está intentando leer desde ese archivo mediante el uso de una ruta de acceso relativa, y de forma predeterminada, "System32/SysWOW64" es el directorio de trabajo de la aplicación. Esto sugiere que la aplicación está esperando su directorio de trabajo actual se establece en algún lugar en el paquete. ¿Está buscando dentro de la appx, podemos ver que el archivo existe en el mismo directorio que el archivo ejecutable.

![Aplicación Config.txt](images/desktop-to-uwp/psfsampleapp_config_txt.png)

El segundo problema aparece en la siguiente imagen.

![Archivo de registro de ProcMon](images/desktop-to-uwp/procmon_logfile.png)

En este problema, la aplicación no se puede escribir un archivo .log en su ruta de acceso del paquete. Esto podría sugerir que puede ayudarles una compatibilidad (shim) de redirección de archivo.

<a id="find" />

## <a name="find-a-runtime-fix"></a>Buscar una corrección de tiempo de ejecución

El PSF contiene las revisiones de tiempo de ejecución que se pueden usar ahora mismo, como la corrección de redirección de archivo.

### <a name="file-redirection-shim"></a>Redirección de compatibilidad (shim) de archivos

Puede usar el [Archivo de redirección de compatibilidad (shim)](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/develop/FileRedirectionShim) para redirigir los intentos para escribir o leer datos en un directorio que no sea accesible desde una aplicación que se ejecuta en un contenedor MSIX.

Por ejemplo, si la aplicación se escribe en un archivo de registro que se encuentra en el mismo directorio que el ejecutable de aplicaciones, puede usar el [Archivo de redirección de compatibilidad (shim)](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/develop/FileRedirectionShim) para crear el archivo de registro en otra ubicación, como el almacén de datos de aplicación local.

### <a name="runtime-fixes-from-the-community"></a>Revisiones de tiempo de ejecución de la Comunidad

Asegúrese de revisar las contribuciones de la Comunidad en nuestra página de [depósito](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/develop) . Es posible que otros desarrolladores han resuelto un problema similar a la suya y han compartido una corrección de tiempo de ejecución.

## <a name="apply-a-runtime-fix"></a>Aplicar una corrección de tiempo de ejecución

Puede aplicar una corrección de tiempo de ejecución existente con algunas herramientas simples desde el SDK de Windows y siguiendo estos pasos.

> [!div class="checklist"]
> * Cree una carpeta de diseño de paquete
> * Obtener los archivos de paquete compatibilidad con Framework
> * Agregarlos al paquete
> * Modificar el manifiesto del paquete
> * Crear un archivo de configuración

Vamos a través de cada tarea.

### <a name="create-the-package-layout-folder"></a>Crear la carpeta de diseño de paquete

Si ya tiene un archivo de .appx, puede desempaquetar su contenido en una carpeta de diseño que servirá como el área de almacenamiento provisional para el paquete.  Puede hacer esto desde una **x64 nativo herramientas el símbolo del sistema para VS 2017**, o manualmente con la ruta de acceso de Papelera SDK en la ruta de acceso de búsqueda ejecutable.

```
makeappx unpack /p PSFSamplePackage_1.0.60.0_AnyCPU_Debug.appx /d PackageContents

```

Esto le proporcionará algo parecido a lo siguiente.

![Diseño de paquete](images/desktop-to-uwp/package_contents.png)

Si no tiene un archivo .appx para empezar con, puede crear la carpeta de paquete y los archivos desde el principio.

### <a name="get-the-package-support-framework-files"></a>Obtener los archivos de paquete compatibilidad con Framework

Puede obtener el paquete de Nuget PSF mediante el uso de Visual Studio. También puede obtener mediante el uso de la herramienta de línea de comandos de Nuget independiente.

#### <a name="get-the-package-by-using-visual-studio"></a>Obtiene el paquete mediante el uso de Visual Studio

En Visual Studio, haga clic en el nodo de su solución o el proyecto y elija uno de los comandos de administrar paquetes de Nuget.  Para que buscar **Microsoft.PackageSupportFramework** o **PSF** buscar el paquete en Nuget.org. A continuación, instalarlo.

#### <a name="get-the-package-by-using-the-command-line-tool"></a>Obtiene el paquete mediante la herramienta de línea de comandos

Instale la herramienta de línea de comandos de Nuget desde esta ubicación: https://www.nuget.org/downloads. A continuación, desde la línea de comandos de Nuget, ejecute este comando:

```
nuget install Microsoft.PackageSupportFramework
```

### <a name="add-the-package-support-framework-files-to-your-package"></a>Agregar los archivos de paquete compatibilidad con Framework al paquete

Agregar la DLL de PSF necesarios de 32 bits y 64 bits y los archivos ejecutables para el directorio del paquete. Usa la siguiente tabla como guía. También querrá incluir las correcciones de tiempo de ejecución que necesita. En nuestro ejemplo, necesitamos la corrección de tiempo de ejecución de redirección de archivo.

| Archivo ejecutable de la aplicación es x64 | Archivo ejecutable de la aplicación es x86 |
|-------------------------------|-----------|
| [ShimLauncher64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimLauncher/readme.md) |  [ShimLauncher32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimLauncher/readme.md) |
| [ShimRuntime64.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRuntime/readme.md) | [ShimRuntime32.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRuntime/readme.md) |
| [ShimRunDll64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRunDll/readme.md) | [ShimRunDll32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/ShimRunDll/readme.md) |

El contenido del paquete ahora debe tener un aspecto similar al siguiente.

![Archivos binarios del paquete](images/desktop-to-uwp/package_binaries.png)

### <a name="modify-the-package-manifest"></a>Modificar el manifiesto del paquete

Abra el manifiesto de paquete en un editor de texto y, a continuación, establezca el `Executable` atributo de la `Application` elemento en el nombre del archivo ejecutable de iniciador de compatibilidad (shim).  Si conoce la arquitectura de la aplicación de destino, seleccione la versión adecuada, ShimLauncher32.exe o ShimLauncher64.exe.  Si no es así, ShimLauncher32.exe funcionará en todos los casos.  Aquí tienes un ejemplo.

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

Crear un nombre de archivo ``config.json``y guardar el archivo en la carpeta raíz de su paquete. Modificar el identificador de aplicación declarado del archivo config.json para que apunte al archivo ejecutable que acaba de reemplazar. Con el conocimiento que ha obtenido de usar el Monitor de proceso, se puede también establecer el directorio de trabajo así como utilizar la corrección de redirección de archivo para redirigir lecturas o escrituras a archivos .log bajo el directorio "PSFSampleApp" relativa al paquete.

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
| applications | id |  Use el valor de la `Id` atributo de la `Application` elemento en el manifiesto del paquete. |
| applications | ejecutable | La ruta de acceso relativa al paquete para el archivo ejecutable que desea iniciar. En la mayoría de los casos, puede obtener este valor desde el archivo de manifiesto del paquete antes de modificarlo. Es el valor de la `Executable` atributo de la `Application` elemento. |
| applications | workingDirectory | (Opcional) Una ruta de acceso relativa al paquete que se utilizará como el directorio de trabajo de la aplicación que se inicia. Si no establece este valor, el sistema operativo usa el `System32` directorio como directorio de trabajo de la aplicación. |
| procesos | ejecutable | En la mayoría de los casos, este será el nombre de la `executable` configurado anteriormente con la extensión de archivo y ruta de acceso ha quitado. |
| correcciones de compatibilidad | archivo DLL | Paquete de ruta de acceso relativa del .appx de compatibilidad (shim) para cargar. |
| correcciones de compatibilidad | config | (Opcional) Controla cómo se comporta la lista de distribución de compatibilidad (shim). El formato exacto de este valor varía según la compatibilidad (shim) por (shim) como cada corrección puede interpretar este "blob" como desee. |

El `applications`, `processes`, y `shims` las claves son matrices. Esto significa que puede utilizar el archivo config.json para especificar más de una aplicación, proceso y DLL de la corrección.


### <a name="package-and-test-the-app"></a>Paquete y la aplicación de prueba

A continuación, cree un paquete.

```
makeappx pack /d PackageContents /p PSFSamplePackageFixup.appx
```

A continuación, iniciar sesión.

```
signtool sign /a /v /fd sha256 /f ExportedSigningCertificate.pfx PSFSamplePackageFixup.appx
```

Para obtener más información, vea [cómo crear un paquete de certificado de firma](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/how-to-create-a-package-signing-certificate) y [cómo firmar un paquete mediante signtool](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/how-to-sign-a-package-using-signtool)

Uso de PowerShell, instale el paquete.

>[!NOTE]
> Recuerde que desinstalar el paquete en primer lugar.

```
powershell Add-AppxPackage .\PSFSamplePackageFixup.appx
```

Ejecute la aplicación y observe el comportamiento con corrección de tiempo de ejecución que se aplican.  Repita los pasos de empaquetado según sea necesario y diagnóstico.

### <a name="use-the-trace-shim"></a>Use la corrección de seguimiento

Una técnica alternativa para diagnosticar problemas de compatibilidad de aplicación empaquetada consiste en usar la compatibilidad (shim) de seguimiento. Este archivo DLL se incluye con el PSF y proporciona una vista detallada de diagnóstico de comportamiento de la aplicación, similar al proceso de Monitor.  Especialmente diseñada para revelar problemas de compatibilidad de aplicaciones.  Para usar la corrección de seguimiento, agregar el archivo DLL para el paquete, agregue el siguiente fragmento de código a su config.json y, a continuación, empaquetar e instalar la aplicación.

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

De forma predeterminada, la corrección de seguimiento filtra los errores que se pueden considerar "esperados".  Por ejemplo, es posible que intente aplicaciones eliminar de forma incondicional un archivo sin comprobar si ya existe, se omite el resultado. Esto tiene como consecuencia una pena que algunos errores inesperados podrían obtener filtran, por lo que en el ejemplo anterior, se optar por recibir todos los errores de las funciones del sistema de archivos. Esto lo hacemos porque sabemos desde antes de que el intento de leer desde el archivo Config.txt se produce un error con el mensaje "archivo no encontrado". Este es un error que se observa con frecuencia y normalmente no se supone que es inesperado. En la práctica es probable que mejor para iniciar el filtrado sólo para errores inesperados y, a continuación, recurrir a todos los errores si hay un problema que aún no se puede identificar.

De forma predeterminada, se envía el resultado de la corrección de seguimiento para el depurador asociado. Para este ejemplo, no va a adjuntar a un depurador y usará en su lugar, el programa [DebugView](https://docs.microsoft.com/en-us/sysinternals/downloads/debugview) de SysInternals para ver los resultados. Después de ejecutar la aplicación, podemos ver los errores mismos como antes, nos que señala hacia las mismas correcciones de tiempo de ejecución.

![No se encontró el archivo TraceShim](images/desktop-to-uwp/traceshim_filenotfound.png)

![TraceShim acceso denegado](images/desktop-to-uwp/traceshim_accessdenied.png)

## <a name="debug-extend-or-create-a-runtime-fix"></a>Depurar, ampliación o crear una solución de tiempo de ejecución

Puede usar Visual Studio para depurar una corrección de tiempo de ejecución, ampliar una corrección de tiempo de ejecución o cree uno desde cero. Debe hacer estas cosas se realice correctamente.

> [!div class="checklist"]
> * Agregar un proyecto de empaquetado
> * Agregar proyecto para la corrección de tiempo de ejecución
> * Agregar un proyecto que se inicia el iniciador de compatibilidad (shim) ejecutable
> * Configurar el proyecto de empaquetado

Cuando haya terminado, la solución será similar al siguiente.

![Solución completada](images/desktop-to-uwp/runtime-fix-project-structure.png)

Veamos cada proyecto en este ejemplo.

| Proyecto | Propósito |
|-------|-----------|
| DesktopApplicationPackage | Este proyecto se basa en el [proyecto de empaquetado de la aplicación de Windows](desktop-to-uwp-packaging-dot-net.md) y envía el paquete de MSIX. |
| Runtimefix | Se trata de un proyecto de biblioteca de C++ Dynamic-Linked que contiene una o más funciones de reemplazo que sirven como la corrección de tiempo de ejecución. |
| ShimLauncher | Se trata de un proyecto vacío de C++. Este proyecto es un lugar para recopilar los archivos de tiempo de ejecución distribuible del marco de trabajo de paquete de compatibilidad. Genera un archivo ejecutable. Ese archivo ejecutable es lo primero que se ejecuta cuando se inicia la solución. |
| WinFormsDesktopApplication | Este proyecto contiene el código fuente de una aplicación de escritorio. |

Para ver un ejemplo completo que contiene todos estos tipos de proyectos, vea [PSFSample](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/).

Vamos a recorrer los pasos para crear y configurar cada uno de estos proyectos en la solución.


### <a name="create-a-package-solution"></a>Crear un paquete de soluciones

Si aún no tiene una solución para su aplicación de escritorio, cree una nueva **Solución en blanco** en Visual Studio.

![Solución en blanco](images/desktop-to-uwp/blank-solution.png)

También es posible que desee agregar los proyectos de aplicación que tiene.

### <a name="add-a-packaging-project"></a>Agregar un proyecto de empaquetado

Si aún no tiene un **Proyecto de empaquetado de aplicación de Windows**, cree uno y se agrega a la solución.

![Plantilla de proyecto paquete](images/desktop-to-uwp/package-project-template.png)

Para obtener más información sobre el proyecto de empaquetado de la aplicación de Windows, consulte el [paquete de la aplicación mediante el uso de Visual Studio](desktop-to-uwp-packaging-dot-net.md).

En el **Explorador de soluciones**, haga clic en el proyecto de empaquetado, seleccione **Editar**y, a continuación, agregue lo siguiente a la parte inferior del archivo de proyecto:

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

Agregar un proyecto de **Biblioteca de vínculos dinámicos (DLL)** de C++ a la solución.

![Biblioteca de corrección de tiempo de ejecución](images/desktop-to-uwp/runtime-fix-library.png)

Con el botón secundario que project y, a continuación, elija **Propiedades**.

En las páginas de propiedades, busque el campo **Idioma de C++ estándar** y, a continuación, en la lista desplegable situada junto a ese campo, seleccione la **ISO C ++ 17 estándar (/ std:c ++ 17)** opción.

![ISO 17 opción](images/desktop-to-uwp/iso-option.png)

Haga clic en ese proyecto y, a continuación, en el menú contextual, elija la opción **Administrar paquetes de Nuget** . Asegúrese de que la opción de **origen del paquete** se establece en **todos los** o **nuget.org**.

Haga clic en el icono configuración siguiente ese campo.

Busque el *PSF** Nuget empaquetar y, a continuación, vuelva a instalarlo para este proyecto.

![paquete de NuGet](images/desktop-to-uwp/psf-package.png)

Si desea depurar o extender una corrección de tiempo de ejecución existente, agregue los archivos de revisión de tiempo de ejecución que obtuvo mediante el uso de las instrucciones que se describen en la sección [Buscar una corrección de tiempo de ejecución](#find) de esta guía.

Si va a crear una nueva revisión, no agregue nada a este proyecto todavía. Le ayudaremos a agregar los archivos de derecha a este proyecto más adelante en esta guía. Por ahora, vamos a continuar configurando la solución.

### <a name="add-a-project-that-starts-the-shim-launcher-executable"></a>Agregar un proyecto que se inicia el iniciador de compatibilidad (shim) ejecutable

Agregar un proyecto **Proyecto vacío** de C++ a la solución.

![Proyecto vacío](images/desktop-to-uwp/blank-app.png)

Agregue el paquete de Nuget **PSF** a este proyecto mediante el uso de la misma guía que se describe en la sección anterior.

Abrir las páginas de propiedades para el proyecto y, en la página configuración **General** , establezca la propiedad de **Nombre de destino** en ``ShimLauncher32`` o ``ShimLauncher64`` dependiendo de la arquitectura de la aplicación.

![referencia del iniciador de compatibilidad (shim)](images/desktop-to-uwp/shim-exe-reference.png)

Agregue una referencia de proyecto al proyecto de corrección de tiempo de ejecución de la solución.

![referencia de corrección de tiempo de ejecución](images/desktop-to-uwp/reference-fix.png)

Haga clic en la referencia y, a continuación, en la ventana **Propiedades** , se aplican estos valores.

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

Elija el proyecto de compatibilidad (shim) del iniciador y el proyecto de aplicación de escritorio y, a continuación, elija el botón **Aceptar** .

![Proyecto de escritorio](images/desktop-to-uwp/package-project-references.png)

>[!NOTE]
> Si no dispone del código fuente para la aplicación, sólo tiene que elegir el proyecto de compatibilidad (shim) del iniciador. Le mostraremos cómo hacer referencia a su archivo ejecutable cuando se crea un archivo de configuración.

En el nodo de **aplicaciones** , haga clic en la aplicación de iniciador de compatibilidad (shim) y, a continuación, elija **establecer como punto de entrada**.

![Establecer punto de entrada](images/desktop-to-uwp/set-startup-project.png)

Agregue un archivo denominado ``config.json`` a su proyecto de empaquetado, a continuación, copie y pegue el siguiente texto json en el archivo. Establezca la propiedad **Acción de paquete** en **contenido**.

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
| applications | id |  Use el valor de la `Id` atributo de la `Application` elemento en el manifiesto del paquete. |
| applications | ejecutable | La ruta de acceso relativa al paquete para el archivo ejecutable que desea iniciar. En la mayoría de los casos, puede obtener este valor desde el archivo de manifiesto del paquete antes de modificarlo. Es el valor de la `Executable` atributo de la `Application` elemento. |
| applications | workingDirectory | (Opcional) Una ruta de acceso relativa al paquete que se utilizará como el directorio de trabajo de la aplicación que se inicia. Si no establece este valor, el sistema operativo usa el `System32` directorio como directorio de trabajo de la aplicación. |
| procesos | ejecutable | En la mayoría de los casos, este será el nombre de la `executable` configurado anteriormente con la extensión de archivo y ruta de acceso ha quitado. |
| correcciones de compatibilidad | archivo DLL | Paquete de ruta de acceso relativa la DLL para carga de compatibilidad (shim). |
| correcciones de compatibilidad | config | (Opcional) Controla cómo se comporta la lista de distribución de compatibilidad (shim). El formato exacto de este valor varía según la compatibilidad (shim) por (shim) como cada corrección puede interpretar este "blob" como desee. |

Cuando haya terminado, su ``config.json`` archivo tendrá un aspecto similar al siguiente.

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
> El `applications`, `processes`, y `shims` las claves son matrices. Esto significa que puede utilizar el archivo config.json para especificar más de una aplicación, proceso y DLL de la corrección.

### <a name="debug-a-runtime-fix"></a>Depurar una corrección de tiempo de ejecución

En Visual Studio, presione F5 para iniciar al depurador.  Lo primero que se inicia es la aplicación de iniciador de compatibilidad (shim), que a su vez, se inicia la aplicación de escritorio de destino.  Para depurar la aplicación de destino de escritorio, tendrá que adjuntar manualmente el proceso de aplicación de escritorio, elija **Depurar**->**asociar al proceso**y, a continuación, seleccionar el proceso de aplicación. Para permitir la depuración de una aplicación .NET con una solución de tiempo de ejecución nativo DLL, seleccione los tipos de código nativo y administrado (depuración de modo mixto).  

Una vez que haya configurado esto, puede establecer puntos de interrupción junto a líneas de código en el código de la aplicación de escritorio y el proyecto de corrección de tiempo de ejecución. Si no dispone del código fuente para la aplicación, podrá establecer puntos de interrupción solo junto a líneas de código en el proyecto de corrección de tiempo de ejecución.

>[!NOTE]
> Mientras Visual Studio le ofrece el desarrollo más sencillo y experiencia de depuración, existen algunas limitaciones, por lo que más adelante en esta guía, analizaremos otras técnicas de depuración que se pueden aplicar.

## <a name="create-a-runtime-fix"></a>Crear una solución de tiempo de ejecución

Si no hay aún un tiempo de ejecución de corrección para el problema que desea resolver, puede crear una nueva revisión de tiempo de ejecución por escribir funciones de reemplazo e incluidos los datos de configuración tiene sentido. Veamos cada elemento.

### <a name="replacement-functions"></a>Funciones de reemplazo

En primer lugar, identifique la función que se producirá un error en las llamadas cuando la aplicación se ejecuta en un contenedor MSIX. A continuación, puede crear funciones de reemplazo que le gustaría que el Administrador de tiempo de ejecución para llamar en su lugar. Esto le da una oportunidad para reemplazar la implementación de una función con el comportamiento que se ajusta a las reglas del entorno de tiempo de ejecución moderno.

En Visual Studio, abra el proyecto de corrección de tiempo de ejecución que creó anteriormente en esta guía.

Declarar el ``SHIM_DEFINE_EXPORTS`` macro y, a continuación, agregue una instrucción include para el `shim_framework.h` en la parte superior de cada uno. Archivo CPP donde se va a agregar las funciones de la revisión de tiempo de ejecución.

```c++
#define SHIM_DEFINE_EXPORTS
#include <shim_framework.h>
```
>[!IMPORTANT]
>Asegúrese de que el `SHIM_DEFINE_EXPORTS` macro aparece antes de la instrucción de inclusión.

Crear una función que tiene la misma firma de la función que tiene comportamiento que desea modificar. Aquí es una función de ejemplo que reemplaza el `MessageBoxW` (función).

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

La llamada a `DECLARE_SHIM` asigna el `MessageBoxW` (función) a la nueva función de reemplazo. Cuando la aplicación intenta llamar a la `MessageBoxW` (función), llamará a la función de reemplazo en su lugar.

#### <a name="protect-against-recursive-calls-to-functions-in-runtime-fixes"></a>Protege contra las llamadas recursivas a funciones en las revisiones de tiempo de ejecución

Opcionalmente, puede aplicar el `reentrancy_guard` tipo a las funciones que protegen frente a las llamadas recursivas a funciones en las revisiones de tiempo de ejecución.

Por ejemplo, podría generar una función de reemplazo para el `CreateFile` (función). Puede llamar la implementación de la `CopyFile` (función), pero la implementación de la `CopyFile` podría llamar la función el `CreateFile` (función). Esto puede dar lugar a un ciclo recursiva infinita de llamadas a la `CreateFile` (función).

Para obtener más información en `reentrancy_guard` vea [authoring.md](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/Authoring.md)

### <a name="configuration-data"></a>Datos de configuración

Si desea agregar datos de configuración para la corrección de tiempo de ejecución, considere la posibilidad de agregarlo a la ``config.json``. De este modo, puede usar el `ShimQueryCurrentDllConfig` para analizar fácilmente esos datos. En este ejemplo se analiza un valor boolean y de cadena de ese archivo de configuración.

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

Mientras que Visual Studio permite el desarrollo más simple y la experiencia de depuración, existen algunas limitaciones.

En primer lugar, depuración F5 ejecuta la aplicación mediante la implementación de archivos separados de la ruta de acceso de carpeta de diseño de paquete, en lugar de la instalación desde un paquete .appx.  Normalmente, la carpeta de diseño no tiene las mismas restricciones de seguridad como una carpeta de paquete instalado. Como resultado, es no posible reproducir errores de denegación de acceso de ruta de acceso de paquete antes de aplicar una corrección de tiempo de ejecución.

Para solucionar este problema, use la implementación del paquete .appx en lugar de implementación de archivo separado F5.  Para crear un archivo de paquete .appx, use la utilidad [MakeAppx](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/make-appx-package--makeappx-exe-) desde el SDK de Windows, como se describió anteriormente. O bien, desde dentro de Visual Studio, haga clic en el nodo del proyecto de aplicación y seleccione **almacén de**->**Crear paquetes de aplicación**.

Otro problema con Visual Studio es que no tiene compatibilidad integrada para asociar a los procesos secundarios iniciados por el depurador.   Esto dificulta la depuración de lógica en la ruta de acceso de inicio de la aplicación de destino, que se debe vincular manualmente por Visual Studio después del lanzamiento.

Para solucionar este problema, use un depurador que admite el proceso secundario adjunta.  Tenga en cuenta que normalmente no es posible adjuntar a un depurador de just-in-time (JIT) a la aplicación de destino.  Esto es debido a que la mayoría de las técnicas de manera oportuna implica de iniciar al depurador en lugar de la aplicación de destino, a través de la clave del registro ImageFileExecutionOptions.  Esto impide el mecanismo detouring utilizado por ShimLauncher.exe para insertar ShimRuntime.dll en la aplicación de destino.  WinDbg, incluidos en las [Herramientas de depuración para Windows](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/index)y obtenido en el [SDK de Windows](https://developer.microsoft.com/en-US/windows/downloads/windows-10-sdk), que admite secundarios proceso adjunta.  También ahora admite directamente [iniciar y depurar una aplicación UWP](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-a-uwp-app-using-windbg#span-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanlaunching-and-debugging-a-uwp-app).

Para depurar el inicio de la aplicación de destino como un proceso secundario, inicie ``WinDbg``.

```
windbg.exe -plmPackage PSFSampleWithFixup_1.0.59.0_x86__7s220nvg1hg3m -plmApp PSFSample
```

En el ``WinDbg`` preguntar, cómo habilitar depuración de secundarios y establecer puntos de interrupción apropiados.

```
.childdbg 1
g
```
(ejecutar hasta que la aplicación de destino se inicia y se entra en el depurador)

```
sxe ld fixup.dll
g
```
(que se ejecuta hasta la corrección para que cargar DLL)

```
bp ...
```

>[!NOTE]
> [PLMDebug](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/plmdebug) también se puede utilizar para adjuntar a un depurador a una aplicación en el inicio y también se incluye en las [Herramientas de depuración para Windows](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/index).  Sin embargo, es más complejo de usar que el soporte técnico directo ahora proporcionado por WinDbg.

## <a name="support-and-feedback"></a>Soporte técnico y comentarios

**Encuentra respuestas a tus preguntas**

¿Tienes alguna pregunta? Pregúntanos en Stack Overflow. Nuestro equipo supervisa estas [etiquetas](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). También puedes preguntarnos [aquí](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).
