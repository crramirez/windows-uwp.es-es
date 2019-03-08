---
Description: Corregir problemas que impiden su aplicación de escritorio se ejecute en un contenedor de MSIX
Search.Product: eADQiWindows 10XVcnh
title: Corregir problemas que impiden su aplicación de escritorio se ejecute en un contenedor de MSIX
ms.date: 07/02/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 80f9c8bad9445bd9cfef9b09c00f99929fda37aa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57590730"
---
# <a name="apply-runtime-fixes-to-an-msix-package-by-using-the-package-support-framework"></a>Aplicar correcciones en tiempo de ejecución a un paquete MSIX mediante el marco de trabajo de paquete de soporte técnico

El marco de trabajo de paquete de soporte técnico es un kit de código abierto que le permite aplicar correcciones a la aplicación existente de win32 cuando no tienes acceso al código fuente, por lo que se pueda ejecutar en un contenedor MSIX. El marco de trabajo de paquete de soporte técnico le ayuda a la aplicación siga las prácticas recomendadas del entorno en tiempo de ejecución modernos.

Para obtener más información, consulte [el marco de trabajo de paquete de soporte técnico](https://docs.microsoft.com/windows/msix/package-support-framework-overview).

Esta guía le ayudará a identificar problemas de compatibilidad de aplicaciones, y buscar, aplicar y extender el tiempo de ejecución de correcciones que se tratan.

<a id="identify" />

## <a name="identify-packaged-application-compatibility-issues"></a>Identificar problemas de compatibilidad de aplicaciones empaquetadas

En primer lugar, cree un paquete para su aplicación. A continuación, vuelva a instalarlo, ejecutarlo y observar su comportamiento. Recibirá mensajes de error que pueden ayudarle a identificar un problema de compatibilidad. También puede usar [Process Monitor](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon) para identificar problemas.  Problemas comunes relacionados con ideas acerca de los permisos de ruta de acceso de directorio y el programa de trabajo de aplicación.

### <a name="using-process-monitor-to-identify-an-issue"></a>Usar el Monitor de proceso para identificar un problema

[Monitor de procesos](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon) es una herramienta eficaz para observar las operaciones de registro y archivos de una aplicación y sus resultados.  Esto puede ayudarle a comprender los problemas de compatibilidad de aplicaciones.  Después de abrir el Monitor de procesos, agregar un filtro (filtro > filtro...) para incluir únicamente los eventos de la aplicación ejecutable.

![Filtro de aplicación ProcMon](images/desktop-to-uwp/procmon_app_filter.png)

Aparecerá una lista de eventos. Para muchos de estos eventos, la palabra **éxito** aparecerá en la **resultado** columna.

![Eventos ProcMon](images/desktop-to-uwp/procmon_events.png)

Si lo desea, puede filtrar eventos para mostrar solo solo los errores.

![Excluir ProcMon correcto](images/desktop-to-uwp/procmon_exclude_success.png)

Si sospecha que un error de acceso del sistema de archivos, busque eventos con errores que se encuentran bajo el System32/SysWOW64 o la ruta de acceso del archivo de paquete. Los filtros también pueden ayudar en este caso, demasiado. A partir de la parte inferior de esta lista y desplácese hacia arriba. Errores que aparecen en la parte inferior de esta lista se han producido recientemente. Preste atención mayoría para errores que contienen cadenas como "acceso denegado" y "ruta de acceso/nombre no encontrado" y pasar por alto las cosas que no tienen un aspecto sospechosas. El [PSFSample](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/) tiene dos problemas. Puede ver estos problemas en la lista que aparece en la siguiente imagen.

![ProcMon Config.txt](images/desktop-to-uwp/procmon_config_txt.png)

En el primer problema que aparece en esta imagen, la aplicación no se puede leer el archivo "Config.txt" que se encuentra en la ruta de acceso "C:\Windows\SysWOW64". No es probable que la aplicación está intentando hacer referencia directamente a esa ruta de acceso. Probablemente, está intentando leer desde ese archivo mediante una ruta de acceso relativa, y de forma predeterminada, "System32/SysWOW64" es el directorio de trabajo de la aplicación. Esto sugiere que la aplicación espera que su directorio de trabajo actual se establecerá en algún lugar en el paquete. Examinar dentro el appx, podemos ver que el archivo existe en el mismo directorio que el archivo ejecutable.

![Aplicación Config.txt](images/desktop-to-uwp/psfsampleapp_config_txt.png)

El segundo problema aparece en la siguiente imagen.

![Archivo de registro ProcMon](images/desktop-to-uwp/procmon_logfile.png)

En este problema, la aplicación no se puede escribir un archivo .log en su ruta de acceso del paquete. Esto sugeriría que podría ayudar una corrección de redirección del archivo.

<a id="find" />

## <a name="find-a-runtime-fix"></a>Buscar una solución en tiempo de ejecución

El PSF contiene correcciones de tiempo de ejecución que puede usar ahora mismo, como la corrección de redirección del archivo.

### <a name="file-redirection-fixup"></a>Corrección de redirección de archivos

Puede usar el [archivo redirección corrección](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/fixups/FileRedirectionFixup) para redirigir los intentos de escribir o leer datos en un directorio que no es accesible desde una aplicación que se ejecuta en un contenedor MSIX.

Por ejemplo, si su aplicación escribe en un archivo de registro que se encuentra en el mismo directorio que las aplicaciones ejecutables, a continuación, puede usar el [archivo redirección corrección](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/fixups/FileRedirectionFixup) para crear ese archivo de registro en otra ubicación, como el almacén de datos de la aplicación local.

### <a name="runtime-fixes-from-the-community"></a>Correcciones de tiempo de ejecución de la Comunidad

No olvide revisar las contribuciones de la Comunidad a nuestra [GitHub](https://github.com/Microsoft/MSIX-PackageSupportFramework) página. Es posible que otros desarrolladores han resuelto un problema similar al suyo y comparten una corrección en tiempo de ejecución.

## <a name="apply-a-runtime-fix"></a>Aplicar una corrección en tiempo de ejecución

Puede aplicar una corrección de tiempo de ejecución existentes con algunas herramientas simple desde el SDK de Windows y siguiendo estos pasos.

> [!div class="checklist"]
> * Cree una carpeta de diseño del paquete
> * Obtener los archivos de soporte técnico de Package Framework
> * Agregarlos al paquete
> * Modificar el manifiesto de paquete
> * Crear un archivo de configuración

Analicemos cada tarea.

### <a name="create-the-package-layout-folder"></a>Cree la carpeta de diseño del paquete

Si ya tiene un archivo .msix (o .appx), puede desempaquetar su contenido en una carpeta de diseño que servirá como el área de ensayo para el paquete. Puede hacerlo desde un símbolo del sistema mediante la herramienta MakeAppx, según la ruta de instalación del SDK, esto es donde encontrará la herramienta makeappx.exe en tu PC con Windows 10: x86: C:\Program archivos (x86) \Windows Kits\10\bin\x86\makeappx.exe x64: C:\Program archivos (x86) \Windows Kits\10\bin\x64\makeappx.exe

```ps
makeappx unpack /p PSFSamplePackage_1.0.60.0_AnyCPU_Debug.msix /d PackageContents

```

Esto le dará algo parecido a lo siguiente.

![Diseño del paquete](images/desktop-to-uwp/package_contents.png)

Si no tiene un archivo .msix (o .appx) para empezar, puede crear la carpeta de paquetes y archivos desde el principio.

### <a name="get-the-package-support-framework-files"></a>Obtener los archivos de soporte técnico de Package Framework

Puede obtener el paquete Nuget PSF mediante la herramienta de línea de comandos de Nuget independiente o a través de Visual Studio.

#### <a name="get-the-package-by-using-the-command-line-tool"></a>Obtención del paquete mediante la herramienta de línea de comandos

Instalar la herramienta de línea de comandos de Nuget desde esta ubicación: https://www.nuget.org/downloads. A continuación, desde la línea de comandos de Nuget, ejecute este comando:

```ps
nuget install Microsoft.PackageSupportFramework
```

#### <a name="get-the-package-by-using-visual-studio"></a>Obtención del paquete mediante Visual Studio

En Visual Studio, haga clic en el nodo de solución o proyecto y seleccione uno de los comandos de administración de paquetes de Nuget.  Busque **Microsoft.PackageSupportFramework** o **PSF** para buscar el paquete en Nuget.org. A continuación, instalarla.

### <a name="add-the-package-support-framework-files-to-your-package"></a>Agregar los archivos del marco de soporte técnico de paquete al paquete

Agregue el requiere 32 bits y 64 bits PSF archivos DLL y ejecutables en el directorio del paquete. Usa la siguiente tabla como guía. También deseará incluir todas las correcciones en tiempo de ejecución que necesita. En nuestro ejemplo, necesitamos la corrección en tiempo de ejecución de redirección de archivos.

| Archivo ejecutable de aplicación es x64 | Archivo ejecutable de aplicación es x86 |
|-------------------------------|-----------|
| [PSFLauncher64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfLauncher/readme.md) |  [PSFLauncher32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfLauncher/readme.md) |
| [PSFRuntime64.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfRuntime/readme.md) | [PSFRuntime32.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfRuntime/readme.md) |
| [PSFRunDll64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/PsfRunDll/readme.md) | [PSFRunDll32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/PsfRunDll/readme.md) |

El contenido del paquete ahora un aspecto similar al siguiente.

![Archivos binarios de paquete](images/desktop-to-uwp/package_binaries.png)

### <a name="modify-the-package-manifest"></a>Modificar el manifiesto de paquete

Abra el manifiesto del paquete en un editor de texto y, a continuación, establezca el `Executable` atributo de la `Application` elemento en el nombre del archivo ejecutable PSF del iniciador.  Si conoce la arquitectura de la aplicación de destino, seleccione la versión adecuada, PSFLauncher32.exe o PSFLauncher64.exe.  Si no es así, PSFLauncher32.exe funcionará en todos los casos.  A continuación te mostramos un ejemplo.

```xml
<Package ...>
  ...
  <Applications>
    <Application Id="PSFSample"
                 Executable="PSFLauncher32.exe"
                 EntryPoint="Windows.FullTrustApplication">
      ...
    </Application>
  </Applications>
</Package>
```

### <a name="create-a-configuration-file"></a>Crear un archivo de configuración

Cree un archivo denominado ``config.json``y guarde el archivo en la carpeta raíz del paquete. Modificar el identificador de aplicación declarado del archivo config.json para que apunte al archivo ejecutable que acaba de sustituirse. Con el conocimiento que ha obtenido del uso de Process Monitor, se puede también establecer el directorio de trabajo así como usar la corrección de redirección de archivo para redirigir las lecturas y escrituras a los archivos .log en el directorio de "PSFSampleApp" relacionados con el paquete.

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
            "fixups": [
                {
                    "dll": "FileRedirectionFixup.dll",
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

Siguiente es una guía para el esquema config.json:

| matriz | key | Valor |
|-------|-----------|-------|
| applications | id |  Use el valor de la `Id` atributo de la `Application` elemento en el manifiesto del paquete. |
| applications | executable | La ruta de acceso relativa del paquete al archivo ejecutable que desea iniciar. En la mayoría de los casos, puede obtener este valor desde el archivo de manifiesto del paquete antes de modificarlo. Es el valor de la `Executable` atributo de la `Application` elemento. |
| applications | workingDirectory | (Opcional) Ruta de acceso relativa del paquete que se usará como el directorio de trabajo de la aplicación que se inicia. Si no establece este valor, el sistema operativo usa el `System32` directorio como directorio de trabajo de la aplicación. |
| procesos | executable | En la mayoría de los casos, este será el nombre de la `executable` configurado anteriormente con la extensión de ruta de acceso y quitar. |
| correcciones | dll | Ruta de acceso de paquete relativa a la corrección,.msix/.appx para cargar. |
| correcciones | config | (Opcional) Controla cómo se comporta la lista de distribución de corrección. El formato exacto de este valor varía según una corrección mediante la corrección, como cada corrección puede interpretar este "blob" como desee. |

El `applications`, `processes`, y `fixups` claves son matrices. Esto significa que puede usar el archivo config.json para especificar más de una aplicación, procesos y corrección de DLL.

### <a name="package-and-test-the-app"></a>Paquete y probar la aplicación

A continuación, cree un paquete.

```ps
makeappx pack /d PackageContents /p PSFSamplePackageFixup.msix
```

A continuación, firmarlo.

```ps
signtool sign /a /v /fd sha256 /f ExportedSigningCertificate.pfx PSFSamplePackageFixup.msix
```

Para obtener más información, consulte [cómo crear un certificado de firma del paquete](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/how-to-create-a-package-signing-certificate) y [cómo firmar un paquete usando signtool](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/how-to-sign-a-package-using-signtool)

Uso de PowerShell, instale el paquete.

>[!NOTE]
> No olvide desinstalar primero el paquete.

```ps
powershell Add-AppPackage .\PSFSamplePackageFixup.msix
```

Ejecute la aplicación y observar el comportamiento con corrección de tiempo de ejecución que se ha aplicado.  Repita el diagnóstico y los pasos de empaquetado según sea necesario.

### <a name="use-the-trace-fixup"></a>Usar la corrección de seguimiento

Una técnica alternativa para diagnosticar problemas de compatibilidad de la aplicación empaquetada es usar la corrección de seguimiento. Este archivo DLL se incluye con el PSF y proporciona una vista de diagnóstico detallada del comportamiento de la aplicación, similar al Monitor de procesos.  Está diseñado especialmente para revelar problemas de compatibilidad de aplicaciones.  Para usar la corrección de seguimiento, agregar el archivo DLL para el paquete, agregue el siguiente fragmento en el archivo config.json y, a continuación, empaquetar e instalar la aplicación.

```json
{
    "dll": "TraceFixup.dll",
    "config": {
        "traceLevels": {
            "filesystem": "allFailures"
        }
    }
}
```

De forma predeterminada, la corrección de seguimiento filtra los errores que se pueden considerar "esperados".  Por ejemplo, las aplicaciones intentan incondicionalmente eliminar un archivo sin comprobar para ver si ya existe, se omite el resultado. Esto tiene como consecuencia desafortunada que podrían obtener filtradas algunos errores inesperados, por lo que en el ejemplo anterior, se opta por recibir todos los errores de las funciones del sistema de archivos. Lo hacemos porque sabemos desde antes de que el intento de leer el archivo Config.txt produce un error con el mensaje "archivo no encontrado". Se trata de un error que se observa con frecuencia y normalmente no se supone que no esperan. En la práctica es probable que lo mejor para iniciar el filtrado solo para errores inesperados y, a continuación, revirtiendo a todos los errores si hay un problema que todavía no se puede identificar.

De forma predeterminada, el resultado de la corrección de seguimiento se envía al depurador adjunto. En este ejemplo, hemos no va a adjuntar un depurador y en su lugar se usará el [DebugView](https://docs.microsoft.com/en-us/sysinternals/downloads/debugview) programa de SysInternals para consultar el resultado. Después de ejecutar la aplicación, podemos ver los mismos errores como antes, lo que nos señalaría hacia las mismas soluciones en tiempo de ejecución.

![No se encontró el archivo TraceShim](images/desktop-to-uwp/traceshim_filenotfound.png)

![TraceShim acceso denegado](images/desktop-to-uwp/traceshim_accessdenied.png)

## <a name="debug-extend-or-create-a-runtime-fix"></a>Depurar, extender o crear una corrección en tiempo de ejecución

Puede usar Visual Studio para depurar una solución en tiempo de ejecución, ampliar una corrección en tiempo de ejecución o crear uno desde cero. Deberá realizar estas acciones se realice correctamente.

> [!div class="checklist"]
> * Agregar un proyecto de empaquetado
> * Agregue el proyecto para la corrección en tiempo de ejecución
> * Agregar un proyecto que se inicia el selector de PSF ejecutable
> * Configurar el proyecto de empaquetado

Cuando haya terminado, la solución tendrá un aspecto similar al siguiente.

![Solución completa](images/desktop-to-uwp/runtime-fix-project-structure.png)

Echemos un vistazo a cada proyecto en este ejemplo.

| Proyecto | Propósito |
|-------|-----------|
| DesktopApplicationPackage | Este proyecto se basa en el [proyecto de empaquetado de aplicaciones de Windows](desktop-to-uwp-packaging-dot-net.md) y lo envía el paquete MSIX. |
| Runtimefix | Se trata de un proyecto de biblioteca de C++ Dynamic-Linked que contiene una o varias funciones de reemplazo que actúan como la corrección en tiempo de ejecución. |
| PSFLauncher | Se trata de un proyecto vacío de C++. Este proyecto es un lugar para recopilar los archivos distribuible en tiempo de ejecución de paquete de soporte técnico de Framework. Genera un archivo ejecutable. Ese archivo ejecutable es lo primero que se ejecuta al iniciar la solución. |
| WinFormsDesktopApplication | Este proyecto contiene el código fuente de una aplicación de escritorio. |

Para ver un ejemplo completo que contiene todos estos tipos de proyectos, vea [PSFSample](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/).

Vamos a recorrer los pasos para crear y configurar cada uno de estos proyectos en la solución.

### <a name="create-a-package-solution"></a>Crear un paquete de soluciones

Si aún no tiene una solución para su aplicación de escritorio, cree un nuevo **solución en blanco** en Visual Studio.

![Solución en blanco](images/desktop-to-uwp/blank-solution.png)

También puede agregar cualquier proyecto de aplicación que tiene.

### <a name="add-a-packaging-project"></a>Agregar un proyecto de empaquetado

Si aún no tiene un **proyecto de empaquetado de aplicaciones de Windows**, cree uno y agregarlo a la solución.

![Plantilla de proyecto de paquete](images/desktop-to-uwp/package-project-template.png)

Para obtener más información sobre el proyecto de empaquetado de aplicaciones de Windows, consulte [empaquetar la aplicación mediante Visual Studio](desktop-to-uwp-packaging-dot-net.md).

En **el Explorador de soluciones**, haga clic en el proyecto de empaquetado, seleccione **editar**y, a continuación, agregue lo siguiente a la parte inferior del archivo del proyecto:

```xml
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

### <a name="add-project-for-the-runtime-fix"></a>Agregue el proyecto para la corrección en tiempo de ejecución

Agregar un C++ **biblioteca de vínculos dinámicos (DLL)** proyecto a la solución.

![Biblioteca de corrección de tiempo de ejecución](images/desktop-to-uwp/runtime-fix-library.png)

Haga que al proyecto y, a continuación, elija **propiedades**.

En las páginas de propiedades, busque el **estándar de lenguaje C++** campo y, a continuación, en la lista desplegable situada junto a ese campo, seleccione el **estándar ISO C ++ 17 (/ STD: c ++ 17)** opción.

![ISO 17 opción](images/desktop-to-uwp/iso-option.png)

Haga clic en ese proyecto y, a continuación, en el menú contextual, elija el **administrar paquetes de Nuget** opción. Asegúrese de que el **origen del paquete** opción está establecida en **todas** o **nuget.org**.

Haga clic en el icono de configuración siguiente ese campo.

Busque el *PSF** Nuget del paquete y, a continuación, vuelva a instalarlo para este proyecto.

![paquete de NuGet](images/desktop-to-uwp/psf-package.png)

Si desea depurar o ampliar una corrección de tiempo de ejecución existente, agregue los archivos de revisión de tiempo de ejecución que obtenidos mediante los pasos descritos en la [encontrar una solución en tiempo de ejecución](#find) sección de esta guía.

Si piensa crear una solución nueva, no agregue nada a este proyecto aún. Le ayudaremos a agregar los archivos correctos para este proyecto más adelante en esta guía. Por ahora, vamos a seguir la configuración de la solución.

### <a name="add-a-project-that-starts-the-psf-launcher-executable"></a>Agregar un proyecto que se inicia el selector de PSF ejecutable

Agregar un C++ **proyecto vacío** proyecto a la solución.

![Proyecto vacío](images/desktop-to-uwp/blank-app.png)

Agregar el **PSF** paquete Nuget para este proyecto mediante el uso de las mismas instrucciones que se describe en la sección anterior.

Abra las páginas de propiedades para el proyecto y, en el **General** página Configuración, establezca el **nombre de destino** propiedad ``PSFLauncher32`` o ``PSFLauncher64`` según la arquitectura de la aplicación.

![Referencia de iniciador PSF](images/desktop-to-uwp/shim-exe-reference.png)

Agregue una referencia de proyecto al proyecto de corrección en tiempo de ejecución de la solución.

![referencia de corrección de tiempo de ejecución](images/desktop-to-uwp/reference-fix.png)

Haga clic en la referencia y, a continuación, en el **propiedades** ventana, se aplican estos valores.

| Propiedad | Valor |
|-------|-----------|
| Copiar en local | True |
| Copiar ensamblados satélite locales | True |
| Salida de ensamblado de referencia | True |
| Vincular dependencias de biblioteca | False |
| Entradas de dependencia de biblioteca de vínculo | False |

### <a name="configure-the-packaging-project"></a>Configurar el proyecto de empaquetado

En el proyecto de empaquetado, haz clic con el botón derecho en la carpeta **Applications** y, a continuación, elige **Agregar referencia**.

![Agregar referencia de proyecto](images/desktop-to-uwp/add-reference-packaging-project.png)

Elija el proyecto de iniciador PSF y su proyecto de aplicación de escritorio y, a continuación, elija el **Aceptar** botón.

![Proyecto de escritorio](images/desktop-to-uwp/package-project-references.png)

>[!NOTE]
> Si no tiene el código fuente a la aplicación, elija el proyecto de iniciador PSF. Le mostraremos cómo hacer referencia a su archivo ejecutable cuando se crea un archivo de configuración.

En el **aplicaciones** nodo, haga clic en la aplicación de iniciador PSF y, a continuación, elija **establecer como punto de entrada**.

![Establecer punto de entrada](images/desktop-to-uwp/set-startup-project.png)

Agregue un archivo denominado ``config.json`` al proyecto de empaquetado, a continuación, copie y pegue el siguiente texto json en el archivo. Establecer el **acción del paquete** propiedad **contenido**.

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
            "fixups": [
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

Proporcione un valor para cada clave. Use esta tabla como guía.

| matriz | key | Valor |
|-------|-----------|-------|
| applications | id |  Use el valor de la `Id` atributo de la `Application` elemento en el manifiesto del paquete. |
| applications | executable | La ruta de acceso relativa del paquete al archivo ejecutable que desea iniciar. En la mayoría de los casos, puede obtener este valor desde el archivo de manifiesto del paquete antes de modificarlo. Es el valor de la `Executable` atributo de la `Application` elemento. |
| applications | workingDirectory | (Opcional) Ruta de acceso relativa del paquete que se usará como el directorio de trabajo de la aplicación que se inicia. Si no establece este valor, el sistema operativo usa el `System32` directorio como directorio de trabajo de la aplicación. |
| procesos | executable | En la mayoría de los casos, este será el nombre de la `executable` configurado anteriormente con la extensión de ruta de acceso y quitar. |
| correcciones | dll | Ruta de acceso de paquete relativa a la corrección del archivo DLL para cargar. |
| correcciones | config | (Opcional) Controla cómo se comporta la corrección del archivo DLL. El formato exacto de este valor varía según una corrección mediante la corrección, como cada corrección puede interpretar este "blob" como desee. |

Cuando haya terminado, su ``config.json`` archivo tendrá un aspecto algo parecido a esto.

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
      "fixups": [ { "dll": "RuntimeFix.dll" } ]
    }
  ]
}

```

>[!NOTE]
> El `applications`, `processes`, y `fixups` claves son matrices. Esto significa que puede usar el archivo config.json para especificar más de una aplicación, procesos y corrección de DLL.

### <a name="debug-a-runtime-fix"></a>Depurar una solución en tiempo de ejecución

En Visual Studio, presione F5 para iniciar al depurador.  Lo primero que se inicia es la aplicación de iniciador PSF, que a su vez, se inicia la aplicación de escritorio de destino.  Para depurar la aplicación de escritorio de destino, debe asociar manualmente al proceso de aplicación de escritorio eligiendo **depurar**->**asociar al proceso**y, a continuación, seleccione la aplicación proceso. Para permitir la depuración de una aplicación .NET con una corrección en tiempo de ejecución nativo DLL, seleccione los tipos de código administrado y nativo (depuración en modo mixto).  

Una vez que ha configurado esto, puede establecer puntos de interrupción junto a las líneas de código en el código de aplicación de escritorio y el proyecto de corrección de tiempo de ejecución. Si no tiene el código fuente a la aplicación, podrá establecer puntos de interrupción solo junto a las líneas de código en el proyecto de corrección de tiempo de ejecución.

>[!NOTE]
> Aunque Visual Studio le ofrece el desarrollo más sencillo y experiencia de depuración, hay algunas limitaciones, por lo que más adelante en esta guía, trataremos otras técnicas de depuración que se pueden aplicar.

## <a name="create-a-runtime-fix"></a>Crear una solución en tiempo de ejecución

Si no hay todavía un tiempo de ejecución se corrigió el problema que desea resolver, puede crear una nueva revisión de tiempo de ejecución escribiendo las funciones de reemplazo y incluidos los datos de configuración tiene sentido. Echemos un vistazo a cada parte.

### <a name="replacement-functions"></a>Funciones de reemplazo

En primer lugar, identifique qué función se llama a un error cuando la aplicación se ejecuta en un contenedor MSIX. A continuación, puede crear funciones de reemplazo que le gustaría que el Administrador de tiempo de ejecución para llamar en su lugar. Esto ofrece una oportunidad para reemplazar la implementación de una función con un comportamiento que se ajusta a las reglas del entorno en tiempo de ejecución modernos.

En Visual Studio, abra el proyecto de corrección de tiempo de ejecución que creó anteriormente en esta guía.

Declare el ``FIXUP_DEFINE_EXPORTS`` macro y, a continuación, agregue una instrucción de inclusión para el `fixup_framework.h` en la parte superior de cada uno. Archivo CPP donde se va a agregar las funciones de la corrección en tiempo de ejecución.

```c++
#define FIXUP_DEFINE_EXPORTS
#include <fixup_framework.h>
```

>[!IMPORTANT]
>Asegúrese de que el `FIXUP_DEFINE_EXPORTS` macro aparece antes de la instrucción de inclusión.

Crear una función que tiene la misma firma de la función que tiene comportamiento que desea modificar. Esta es una función de ejemplo que reemplaza el `MessageBoxW` función.

```c++
auto MessageBoxWImpl = &::MessageBoxW;
int WINAPI MessageBoxWFixup(
    _In_opt_ HWND hwnd,
    _In_opt_ LPCWSTR,
    _In_opt_ LPCWSTR caption,
    _In_ UINT type)
{
    return MessageBoxWImpl(hwnd, L"SUCCESS: This worked", caption, type);
}

DECLARE_FIXUP(MessageBoxWImpl, MessageBoxWFixup);
```

La llamada a `DECLARE_FIXUP` asigna el `MessageBoxW` función a la nueva función de reemplazo. Cuando la aplicación intenta llamar a la `MessageBoxW` función, llamará la función de reemplazo en su lugar.

#### <a name="protect-against-recursive-calls-to-functions-in-runtime-fixes"></a>Protegerse frente a las llamadas recursivas a funciones en tiempo de ejecución correcciones

Opcionalmente puede aplicar el `reentrancy_guard` tipo a las funciones que protegen frente a las llamadas recursivas a funciones en tiempo de ejecución correcciones.

Por ejemplo, podría generar una función de reemplazo para el `CreateFile` función. La implementación podría llamar a la `CopyFile` función, pero la implementación de la `CopyFile` podría llamar la función el `CreateFile` función. Esto puede dar lugar a un ciclo infinito recursiva de las llamadas a la `CreateFile` función.

Para obtener más información sobre `reentrancy_guard` vea [authoring.md](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/Authoring.md)

### <a name="configuration-data"></a>Datos de configuración

Si desea agregar datos de configuración para su revisión en tiempo de ejecución, considere la posibilidad de agregarlo a la ``config.json``. De este modo, puede usar el `FixupQueryCurrentDllConfig` fácilmente analizar esos datos. En este ejemplo se analiza un valor boolean y string desde ese archivo de configuración.

```c++
if (auto configRoot = ::FixupQueryCurrentDllConfig())
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

Aunque Visual Studio ofrece el desarrollo más sencillo y experiencia de depuración, hay algunas limitaciones.

En primer lugar, depuración de F5, ejecuta la aplicación mediante la implementación de archivos separados de la ruta de acceso de carpeta de diseño de paquete, en lugar de la instalación desde un .msix o paquete. AppX.  Normalmente, la carpeta de diseño no tiene las mismas restricciones de seguridad como una carpeta del paquete instalado. Como resultado, es no posible reproducir los errores de denegación de acceso de ruta de acceso de paquete antes de aplicar una corrección en tiempo de ejecución.

Para solucionar este problema, utilice .msix / implementación del paquete .appx en lugar de F5 pierden la implementación del archivo.  Para crear un .msix / archivo de paquete .appx, utilice el [MakeAppx](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/make-appx-package--makeappx-exe-) utilidad desde el SDK de Windows, como se describió anteriormente. O bien, desde dentro de Visual Studio, haga clic en el nodo del proyecto de aplicación y seleccione **Store**->**crear paquetes de aplicaciones**.

Otro problema con Visual Studio es que no tiene compatibilidad integrada para adjuntar a procesos secundarios iniciados por el depurador.   Esto dificulta la depuración lógica en la ruta de acceso de inicio de la aplicación de destino, que se debe adjuntar manualmente en Visual Studio después de iniciarla.

Para solucionar este problema, use un depurador que admite la asociación de procesos secundarios.  Tenga en cuenta que generalmente no es posible adjuntar un depurador de just-in-time (JIT) a la aplicación de destino.  Esto es porque la mayoría de las técnicas JIT implican iniciando al depurador en lugar de la aplicación de destino, mediante la clave del registro ImageFileExecutionOptions.  Esto anula el mecanismo detouring usando PSFLauncher.exe para inyectar FixupRuntime.dll en la aplicación de destino.  WinDbg, que se incluye en el [herramientas de depuración para Windows](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/index)y se obtienen de la [Windows SDK](https://developer.microsoft.com/en-US/windows/downloads/windows-10-sdk), adjuntar el proceso secundario de admite.  Ahora también admite directamente [iniciar y depurar una aplicación para UWP](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-a-uwp-app-using-windbg#span-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanlaunching-and-debugging-a-uwp-app).

Para depurar el inicio de la aplicación de destino como un proceso secundario, inicie ``WinDbg``.

```ps
windbg.exe -plmPackage PSFSampleWithFixup_1.0.59.0_x86__7s220nvg1hg3m -plmApp PSFSample
```

En el ``WinDbg`` símbolo del sistema, habilitar secundarios depuración y establecer puntos de interrupción apropiados.

```ps
.childdbg 1
g
```

(se ejecutan hasta que la aplicación de destino se inicia y entra en el depurador)

```ps
sxe ld fixup.dll
g
```

(se ejecutan hasta que la corrección que se carga el archivo DLL)

```ps
bp ...
```

>[!NOTE]
> [PLMDebug](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/plmdebug) también se puede utilizar para adjuntar un depurador a una aplicación al iniciarse y también se incluye en el [herramientas de depuración para Windows](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/index).  Sin embargo, resulta más compleja de usar que el soporte técnico de direct proporcionada ahora en WinDbg.

## <a name="support-and-feedback"></a>Soporte técnico y comentarios

**Encuentre respuestas a sus preguntas**

¿Tienes alguna pregunta? Pregúntanos en Stack Overflow. Nuestro equipo supervisa estas [etiquetas](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). También puedes preguntarnos [aquí](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).
