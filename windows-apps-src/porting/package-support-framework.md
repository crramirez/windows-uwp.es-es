---
Description: Fix issues that prevent your desktop application from running in an MSIX container
Search.Product: eADQiWindows 10XVcnh
title: Solucionar problemas que impiden que la aplicación de escritorio desde que se ejecuta en un contenedor MSIX
ms.date: 07/02/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 674f5977a69855ff51cbc579ca66085aa133eb5b
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2018
ms.locfileid: "7697698"
---
# <a name="apply-runtime-fixes-to-an-msix-package-by-using-the-package-support-framework"></a>Aplicar correcciones de tiempo de ejecución a un paquete MSIX con el marco de soporte técnico de paquete

El marco de soporte técnico de paquete es un kit de código abierto que ayuda a aplicar correcciones a la aplicación de win32 existentes cuando no tienes acceso al código fuente, por lo que se puede ejecutar en un contenedor de MSIX. El marco de soporte técnico de paquete de la ayuda a la aplicación sigue los procedimientos recomendados del entorno de tiempo de ejecución modernos.

Para obtener más información, consulta el [Marco de soporte técnico del paquete](https://docs.microsoft.com/windows/msix/package-support-framework-overview).

Esta guía te ayudará a identificar problemas de compatibilidad de aplicaciones y buscar, aplicar y ampliar en tiempo de ejecución correcciones que solucionarán.

<a id="identify" />

## <a name="identify-packaged-application-compatibility-issues"></a>Identificar problemas de compatibilidad de la aplicación empaquetada

En primer lugar, crea un paquete de la aplicación. A continuación, instalarlo, ejecutarlo y observar su comportamiento. Es posible que recibas mensajes de error que pueden ayudar a identificar un problema de compatibilidad. También puedes usar [Monitor de procesos](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon) para identificar problemas.  Problemas comunes relacionados con la suposición de aplicación con respecto a los permisos de ruta de acceso de directorio y el programa de trabajo.

### <a name="using-process-monitor-to-identify-an-issue"></a>Uso de Monitor de proceso para identificar un problema

[Monitor de procesos](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon) es una utilidad muy eficaz para observar las operaciones del registro y archivos de la aplicación y los resultados.  Esto puede ayudarte a comprender los problemas de compatibilidad de la aplicación.  Después de abrir el proceso de Monitor, agregar un filtro (filtro > filtro …) para incluir solo los eventos desde el archivo ejecutable de la aplicación.

![Filtro de aplicación ProcMon](images/desktop-to-uwp/procmon_app_filter.png)

Aparecerá una lista de eventos. Para muchos de estos eventos, la palabra aparecerán **éxito** en la columna de **resultados** .

![Eventos de ProcMon](images/desktop-to-uwp/procmon_events.png)

Opcionalmente, puedes filtrar eventos para mostrar solamente solo errores.

![Excluir ProcMon éxito](images/desktop-to-uwp/procmon_exclude_success.png)

Si sospechas que hay un error de acceso de sistema de archivos, buscar eventos de error en la System32/SysWOW64 o la ruta de acceso del archivo de paquete. Filtros también pueden ayudar a aquí también. Inicia en la parte inferior de esta lista y Desplázate hacia arriba. Los errores que aparecen en la parte inferior de esta lista se han producido más recientemente. Presta atención mayoría a errores que contienen las cadenas como "acceso denegado" y "o el nombre de ruta de acceso no encontrada" y pasar por alto las cosas que no tengan un aspecto sospechosas. El [PSFSample](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/) tiene dos problemas. Puedes ver los problemas en la lista que aparece en la siguiente imagen.

![ProcMon Config.txt](images/desktop-to-uwp/procmon_config_txt.png)

En el primer número que aparece en esta imagen, la aplicación se produce un error al leer desde el archivo "Config.txt" que se encuentra en la ruta de acceso "C:\Windows\SysWOW64". Es poco probable que la aplicación está intentando hacer referencia directamente a esa ruta de acceso. Más probable es que está intentando leer desde ese archivo mediante el uso de una ruta de acceso relativa y de manera predeterminada, "System32/SysWOW64" es el directorio de trabajo de la aplicación. Esto sugiere que la aplicación espera su directorio de trabajo actual se establezca en algún lugar en el paquete. Busca dentro el paquete appx, podemos ver que el archivo existe en el mismo directorio que el archivo ejecutable.

![Config.txt de aplicación](images/desktop-to-uwp/psfsampleapp_config_txt.png)

El segundo problema aparece en la siguiente imagen.

![Archivo de registro de ProcMon](images/desktop-to-uwp/procmon_logfile.png)

En este problema, se produce un error de la aplicación escribir un archivo. log en su ruta de acceso del paquete. Esto podría sugerir que puede ayudar una corrección de la redirección de archivos.

<a id="find" />

## <a name="find-a-runtime-fix"></a>Encontrar una corrección en tiempo de ejecución

El PSF contiene correcciones de tiempo de ejecución que puedes usar ahora mismo, por ejemplo, la corrección de la redirección de archivos.

### <a name="file-redirection-fixup"></a>Corrección de la redirección de archivos

Puedes usar la [Corrección de la redirección de archivos](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/fixups/FileRedirectionFixup) para redirigir intentos de escritura o lectura de datos en un directorio que no sea accesible desde una aplicación que se ejecuta en un contenedor MSIX.

Por ejemplo, si la aplicación escribe en un archivo de registro que se encuentra en el mismo directorio que sus aplicaciones ejecutables, a continuación, puedes usar la [Corrección de la redirección de archivos](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/fixups/FileRedirectionFixup) para crear el archivo de registro en otra ubicación, como el almacén de datos locales de la aplicación.

### <a name="runtime-fixes-from-the-community"></a>Correcciones de tiempo de ejecución de la Comunidad

Asegúrate de revisar las contribuciones de la Comunidad a nuestra página de [GitHub](https://github.com/Microsoft/MSIX-PackageSupportFramework) . Es posible que otros desarrolladores han resuelve un problema similar a la tuya y han compartido una corrección en tiempo de ejecución.

## <a name="apply-a-runtime-fix"></a>Aplicar una corrección de tiempo de ejecución

Puedes aplicar una corrección de tiempo de ejecución existente con algunas herramientas sencillas desde el SDK de Windows y siguiendo estos pasos.

> [!div class="checklist"]
> * Crea una carpeta de diseño de paquete
> * Obtener los archivos de marco de soporte técnico de paquete
> * Agregar a tu paquete
> * Modificar el manifiesto del paquete
> * Crear un archivo de configuración

Vamos a través de cada tarea.

### <a name="create-the-package-layout-folder"></a>Crear la carpeta de diseño de paquete

Si ya tienes un archivo .msix (o .appx), puede desempaquetar su contenido en una carpeta de diseño que se usará como el área de ensayo para el paquete. Puedes hacerlo desde un símbolo del sistema con la herramienta makemsix, en función de la ruta de acceso de instalación del SDK, esto es donde encontrarás la herramienta makemsix.exe en tu equipo Windows 10: x86: C:\Program Files (x86) \Windows Kits\10\bin\x86\makemsix.exe x64: C:\Program Files ( x86) \Windows Kits\10\bin\x64\makemsix.exe

```ps
makemsix unpack /p PSFSamplePackage_1.0.60.0_AnyCPU_Debug.msix /d PackageContents

```

Esto te dará algo que el siguiente aspecto.

![Diseño del paquete](images/desktop-to-uwp/package_contents.png)

Si no tienes que se inicie con un archivo .msix (o .appx), puedes crear la carpeta de paquete y archivos desde cero.

### <a name="get-the-package-support-framework-files"></a>Obtener los archivos de marco de soporte técnico de paquete

Puedes obtener el paquete de Nuget PSF mediante la herramienta de línea de comandos de Nuget independiente o a través de Visual Studio.

#### <a name="get-the-package-by-using-the-command-line-tool"></a>Obtener el paquete mediante la herramienta de línea de comandos

Instalar la herramienta de línea de comandos de Nuget desde esta ubicación: https://www.nuget.org/downloads. A continuación, desde la línea de comandos de Nuget, ejecuta el siguiente comando:

```ps
nuget install Microsoft.PackageSupportFramework
```

#### <a name="get-the-package-by-using-visual-studio"></a>Obtener el paquete mediante el uso de Visual Studio

En Visual Studio, haz clic en el nodo del proyecto o solución y seleccionar uno de los comandos de administrar paquetes de Nuget.  Buscar **Microsoft.PackageSupportFramework** o **PSF** encontrar el paquete en Nuget.org. A continuación, instalarlo.

### <a name="add-the-package-support-framework-files-to-your-package"></a>Agregar los archivos de paquete de la compatibilidad con Framework al paquete

Agrega la DLL de PSF necesaria de 32 y 64 bits y los archivos ejecutables en el directorio del paquete. Usa la siguiente tabla como guía. También querrás incluir cualquier correcciones de tiempo de ejecución que necesitas. En nuestro ejemplo, necesitamos la corrección de tiempo de ejecución de redireccionamiento de archivo.

| Archivo ejecutable de la aplicación es x64 | Archivo ejecutable de la aplicación es x86 |
|-------------------------------|-----------|
| [PSFLauncher64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfLauncher/readme.md) |  [PSFLauncher32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfLauncher/readme.md) |
| [PSFRuntime64.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfRuntime/readme.md) | [PSFRuntime32.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfRuntime/readme.md) |
| [PSFRunDll64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/PsfRunDll/readme.md) | [PSFRunDll32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/PsfRunDll/readme.md) |

El contenido del paquete debería ser similar al siguiente.

![Archivos binarios del paquete](images/desktop-to-uwp/package_binaries.png)

### <a name="modify-the-package-manifest"></a>Modificar el manifiesto del paquete

Abre el manifiesto del paquete en un editor de texto y, a continuación, Establece el `Executable` atributo de la `Application` elemento en el nombre del archivo ejecutable PSF selector.  Si sabes que la arquitectura de la aplicación de destino, seleccionar la versión adecuada, PSFLauncher32.exe o PSFLauncher64.exe.  Si no es así, PSFLauncher32.exe funcionará en todos los casos.  A continuación te mostramos un ejemplo.

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

Crear un nombre de archivo ``config.json``y guardar el archivo en la carpeta raíz del paquete. Modificar el identificador de aplicación declarado del archivo config.json para que apunte al archivo ejecutable que se acaba de cambiar. Con el conocimiento que ha obtenido del uso de Monitor de proceso, puede también establece el directorio de trabajo así como usar la corrección de la redirección de archivos para redirigir las lecturas y escrituras a. log archivos en el directorio de "PSFSampleApp" relativo del paquete.

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

La siguiente es una guía para el esquema de config.json:

| Matriz | key | Valor |
|-------|-----------|-------|
| applications | id |  Usa el valor de la `Id` atributo de la `Application` elemento en el manifiesto del paquete. |
| applications | ejecutable | La ruta de acceso relativa de paquete del archivo ejecutable que quieres iniciar. En la mayoría de los casos, puedes obtener este valor desde el archivo de manifiesto de paquete antes de modificarlas. Es el valor de la `Executable` atributo de la `Application` elemento. |
| applications | workingDirectory | (Opcional) Una ruta de acceso relativa de paquete que se usará como el directorio de trabajo de la aplicación que se inicia. Si no estableces este valor, el sistema operativo usa el `System32` directorio como directorio de trabajo de la aplicación. |
| procesos | ejecutable | En la mayoría de los casos, este será el nombre de la `executable` configurado anteriormente con la extensión de archivo y ruta de acceso quitada. |
| correcciones | archivo DLL | Ruta de acceso relativa del paquete para la corrección,.msix/.appx para cargar. |
| correcciones | configuración | (Opcional) Controla cómo se comporta la lista de distribución de corrección. El formato exacto de este valor varía en una base de corrección por corrección como cada corrección puede interpretar este blob de"", como los que quiere. |

El `applications`, `processes`, y `fixups` las claves son matrices. Esto significa que puedes usar el archivo config.json para especificar más de una aplicación, los procesos y corrección DLL.

### <a name="package-and-test-the-app"></a>Paquete y prueba la aplicación

A continuación, crea un paquete.

```ps
makeappx pack /d PackageContents /p PSFSamplePackageFixup.msix
```

A continuación, firmarlo.

```ps
signtool sign /a /v /fd sha256 /f ExportedSigningCertificate.pfx PSFSamplePackageFixup.msix
```

Para obtener más información, consulta [cómo crear un certificado de firma del paquete](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/how-to-create-a-package-signing-certificate) y [cómo firmar un paquete con signtool](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/how-to-sign-a-package-using-signtool)

Uso de PowerShell, instale el paquete.

>[!NOTE]
> Recuerda que tienes que desinstalar el paquete primero.

```ps
powershell Add-MSIXPackage .\PSFSamplePackageFixup.msix
```

Ejecutar la aplicación y observará el comportamiento con la corrección de tiempo de ejecución aplicado.  Repite los pasos de empaquetado según sea necesario y diagnóstico.

### <a name="use-the-trace-fixup"></a>Usar la corrección de seguimiento

Una técnica para diagnosticar problemas de compatibilidad de la aplicación empaquetada alternativa es usar la corrección de seguimiento. Este archivo DLL se incluye con el PSF y proporciona una vista detallada de diagnóstico del comportamiento de la aplicación, similar al Monitor de proceso.  Está diseñada especialmente para mostrar los problemas de compatibilidad de la aplicación.  Usan la reparación de seguimiento, agregar el archivo DLL al paquete, agrega el siguiente fragmento de código a tu config.json y, a continuación, empaquetar e instalar la aplicación.

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

De manera predeterminada, la corrección de seguimiento filtra los errores que puedan considerarse "esperadas".  Por ejemplo, las aplicaciones intente incondicionalmente eliminar un archivo sin comprobar para ver si ya existe, omitiendo el resultado. Esto tiene como consecuencia lamentar que podrían obtener filtradas algunos errores inesperados, por lo que en el ejemplo anterior, hemos optar por recibir todos los errores de las funciones del sistema de archivos. Lo hacemos porque sabemos desde antes de que el intento de leer desde el archivo Config.txt se produce un error con el mensaje "archivo no encontrado". Este es un error que se observa con frecuencia y no por lo general, se supone que es inesperado. En la práctica es probable que es la mejor opción para iniciar el filtrado solo a errores inesperados y, a continuación, recurrir al todos los errores si hay un problema que aún no se puede identificar.

De manera predeterminada, el resultado de la corrección de seguimiento se envía al depurador adjunto. Para este ejemplo, no va a asociar a un depurador y usará en su lugar el programa [DebugView](https://docs.microsoft.com/en-us/sysinternals/downloads/debugview) de SysInternals para ver los resultados. Después de ejecutar la aplicación, podemos ver errores en el mismo que antes, que podría señalan nos las mismas correcciones de tiempo de ejecución.

![No se encontró el archivo TraceShim](images/desktop-to-uwp/traceshim_filenotfound.png)

![TraceShim acceso denegado](images/desktop-to-uwp/traceshim_accessdenied.png)

## <a name="debug-extend-or-create-a-runtime-fix"></a>Depurar, ampliar o crear una corrección en tiempo de ejecución

Puedes usar Visual Studio para depurar una corrección en tiempo de ejecución, ampliar una corrección en tiempo de ejecución o crear uno desde cero. Tendrás que realizar estas acciones se realice correctamente.

> [!div class="checklist"]
> * Agregar un proyecto de empaquetado
> * Agregar el proyecto para la corrección de tiempo de ejecución
> * Agregar un proyecto que se inicia el iniciador de PSF ejecutable
> * Configurar el proyecto de empaquetado

Cuando hayas terminado, la solución tendrá un aspecto similar al siguiente.

![Solución completa](images/desktop-to-uwp/runtime-fix-project-structure.png)

Echemos un vistazo a cada proyecto en este ejemplo.

| Proyecto | Propósito |
|-------|-----------|
| DesktopApplicationPackage | Este proyecto se basa en el [proyecto de empaquetado de aplicaciones de Windows](desktop-to-uwp-packaging-dot-net.md) y genera el paquete MSIX. |
| Runtimefix | Se trata de un proyecto de biblioteca de C++ Dynamic-Linked que contiene uno o más funciones de reemplazo que actúe como la corrección de tiempo de ejecución. |
| PSFLauncher | Se trata de un proyecto vacío de C++. Este proyecto es un lugar para recopilar los archivos distribuible en tiempo de ejecución del marco de soporte técnico de paquete. Genera un archivo ejecutable. Ese archivo ejecutable es lo primero que se ejecuta cuando se inicia la solución. |
| WinFormsDesktopApplication | Este proyecto contiene el código fuente de una aplicación de escritorio. |

Para ver una muestra completa que contiene todos estos tipos de proyectos, consulte [PSFSample](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/).

Veamos los pasos para crear y configurar cada uno de estos proyectos en la solución.

### <a name="create-a-package-solution"></a>Crear una solución de paquete

Si aún no tienes una solución para la aplicación de escritorio, crea una nueva **Solución en blanco** en Visual Studio.

![Solución en blanco](images/desktop-to-uwp/blank-solution.png)

También puedes agregar cualquier proyecto de aplicación que tiene.

### <a name="add-a-packaging-project"></a>Agregar un proyecto de empaquetado

Si aún no tienes un **Proyecto de empaquetado de aplicaciones de Windows**, crear uno y agregarlo a la solución.

![Plantilla de proyecto de paquete](images/desktop-to-uwp/package-project-template.png)

Para obtener más información sobre el proyecto de empaquetado de aplicaciones de Windows, consulte el [paquete de la aplicación con Visual Studio](desktop-to-uwp-packaging-dot-net.md).

En el **Explorador de soluciones**, haz clic en el proyecto de empaquetado, selecciona **Editar**y, a continuación, agregar esto a la parte inferior del archivo de proyecto:

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

### <a name="add-project-for-the-runtime-fix"></a>Agregar el proyecto para la corrección de tiempo de ejecución

Agrega un proyecto de **Biblioteca de vínculos dinámicos (DLL)** de C++ a la solución.

![Biblioteca de corrección de tiempo de ejecución](images/desktop-to-uwp/runtime-fix-library.png)

Con el botón secundario que del proyecto y, a continuación, elige **Propiedades**.

En las páginas de propiedades, busca el campo de **Lenguaje de C++ estándar** y, a continuación, en la lista desplegable situada junto a ese campo, selecciona el **ISO C ++ 17 Standard (/ STD: c ++ 17)** opción.

![ISO 17 opción](images/desktop-to-uwp/iso-option.png)

Haz clic en ese proyecto y, a continuación, en el menú contextual, elige la opción de **Administrar paquetes de Nuget** . Asegúrate de que la opción de **origen del paquete** está establecida en **todos los** o **nuget.org**.

Haz clic en el icono de configuración siguiente ese campo.

Busca el *PSF** Nuget empaquetar y, a continuación, se instala para este proyecto.

![paquete de NuGet](images/desktop-to-uwp/psf-package.png)

Si desea depurar o ampliar una corrección en tiempo de ejecución existente, agregar los archivos de corrección de tiempo de ejecución que obtienen mediante el uso de las instrucciones que se describen en la sección [encontrar una corrección en tiempo de ejecución](#find) de esta guía.

Si vas a crear una nueva solución, no agregues nada en este proyecto todavía. Te ayudaremos a agregar los archivos correctos a este proyecto más adelante en esta guía. Por ahora, seguiremos configurar la solución.

### <a name="add-a-project-that-starts-the-psf-launcher-executable"></a>Agregar un proyecto que se inicia el iniciador de PSF ejecutable

Agrega un proyecto de C++ **Proyecto vacío** a la solución.

![Proyecto vacío](images/desktop-to-uwp/blank-app.png)

Agregar el paquete de Nuget **PSF** a este proyecto con la misma guía que se describe en la sección anterior.

Abre las páginas de propiedades del proyecto y, en la página de configuración **General** , Establece la propiedad de **Nombre de destino** en ``PSFLauncher32`` o ``PSFLauncher64`` según la arquitectura de la aplicación.

![Referencia de selector PSF](images/desktop-to-uwp/shim-exe-reference.png)

Agrega una referencia de proyecto al proyecto de corrección de tiempo de ejecución en la solución.

![referencia de corrección de tiempo de ejecución](images/desktop-to-uwp/reference-fix.png)

Haz clic en la referencia y, a continuación, en la ventana **Propiedades** , se aplican estos valores.

| Propiedad | Valor |
|-------|-----------|
| Copia local | Verdadero |
| Copiar ensamblados satélite locales | Verdadero |
| Salida de ensamblado de referencia | Verdadero |
| Dependencias de la biblioteca de vínculo | Falso |
| Entradas de dependencia de biblioteca de vínculo | Falso |

### <a name="configure-the-packaging-project"></a>Configurar el proyecto de empaquetado

En el proyecto de empaquetado, haz clic con el botón derecho en la carpeta **Applications** y, a continuación, elige **Agregar referencia**.

![Agregar referencia de proyecto](images/desktop-to-uwp/add-reference-packaging-project.png)

Elegir el proyecto de selector PSF y el proyecto de aplicación de escritorio y, a continuación, elige el botón **Aceptar** .

![Proyecto de escritorio](images/desktop-to-uwp/package-project-references.png)

>[!NOTE]
> Si no tienes el código fuente de la aplicación, elige el proyecto de selector PSF. Te mostraremos cómo se hace referencia el archivo ejecutable cuando se crea un archivo de configuración.

En el nodo de **aplicaciones** , haz clic en la aplicación de selector PSF y, a continuación, elige **establecer como punto de entrada**.

![Establecer punto de entrada](images/desktop-to-uwp/set-startup-project.png)

Agrega un archivo denominado ``config.json`` al proyecto de empaquetado, a continuación, copiar y pegar el texto de json siguiente en el archivo. Establece la propiedad de la **Acción del paquete** para **el contenido**.

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

Proporcionar un valor para cada clave. Usa esta tabla como guía.

| Matriz | key | Valor |
|-------|-----------|-------|
| applications | id |  Usa el valor de la `Id` atributo de la `Application` elemento en el manifiesto del paquete. |
| applications | ejecutable | La ruta de acceso relativa de paquete del archivo ejecutable que quieres iniciar. En la mayoría de los casos, puedes obtener este valor desde el archivo de manifiesto de paquete antes de modificarlas. Es el valor de la `Executable` atributo de la `Application` elemento. |
| applications | workingDirectory | (Opcional) Una ruta de acceso relativa de paquete que se usará como el directorio de trabajo de la aplicación que se inicia. Si no estableces este valor, el sistema operativo usa el `System32` directorio como directorio de trabajo de la aplicación. |
| procesos | ejecutable | En la mayoría de los casos, este será el nombre de la `executable` configurado anteriormente con la extensión de archivo y ruta de acceso quitada. |
| correcciones | archivo DLL | Ruta de acceso relativa del paquete para la corrección DLL para cargar. |
| correcciones | configuración | (Opcional) Controla cómo se comporta la DLL de corrección. El formato exacto de este valor varía en una base de corrección por corrección como cada corrección puede interpretar este blob de"", como los que quiere. |

Cuando hayas terminado, su ``config.json`` archivo tendrá un aspecto similar al siguiente.

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
> El `applications`, `processes`, y `fixups` las claves son matrices. Esto significa que puedes usar el archivo config.json para especificar más de una aplicación, los procesos y corrección DLL.

### <a name="debug-a-runtime-fix"></a>Depurar una corrección en tiempo de ejecución

En Visual Studio, presiona F5 para iniciar al depurador.  Lo primero que se inicia es la aplicación de selector PSF, que a su vez, se inicia la aplicación de escritorio de destino.  Para depurar la aplicación de escritorio de destino, tendrás que asociar al proceso de aplicación de escritorio manualmente seleccionando **Depurar**->**asociar al proceso**y, a continuación, selecciona el proceso de la aplicación. Para permitir la depuración de una aplicación de .NET con una corrección de tiempo de ejecución nativo DLL, selecciona los tipos de código administrado y nativo (depuración en modo mixto).  

Una vez que hayas configurado esto, puedes establecer puntos de interrupción junto a las líneas de código en el código de aplicación de escritorio y el proyecto de corrección de tiempo de ejecución. Si no tienes el código fuente de la aplicación, podrás establecer puntos de interrupción solo junto a las líneas de código en el proyecto de corrección de tiempo de ejecución.

>[!NOTE]
> Si bien Visual Studio te ofrece el desarrollo más sencillo y experiencia de depuración, existen algunas limitaciones, por lo tanto, más adelante en esta guía, analizaremos otras técnicas de depuración que se pueden aplicar.

## <a name="create-a-runtime-fix"></a>Crear una corrección en tiempo de ejecución

Si no existe aún un tiempo de ejecución solucionar el problema que desea resolver, puedes crear una nueva corrección de tiempo de ejecución al escribir las funciones de reemplazo e incluidos los datos de configuración tiene sentido. Echemos un vistazo a cada parte.

### <a name="replacement-functions"></a>Funciones de reemplazo

En primer lugar, identificar la función que llama a un error cuando la aplicación se ejecuta en un contenedor MSIX. A continuación, puedes crear funciones de reemplazo que te gustaría llamar en su lugar el Administrador de tiempo de ejecución. Esto te ofrece una oportunidad para reemplazar la implementación de una función con el comportamiento que cumple las reglas del entorno de tiempo de ejecución modernos.

En Visual Studio, abre el proyecto de corrección de tiempo de ejecución que creaste anteriormente en esta guía.

Declarar la ``FIXUP_DEFINE_EXPORTS`` macro y, a continuación, agrega una declaración de inclusión de la `fixup_framework.h` en la parte superior de cada uno. Archivo CPP donde se va a agregar las funciones de la corrección de tiempo de ejecución.

```c++
#define FIXUP_DEFINE_EXPORTS
#include <fixup_framework.h>
```

>[!IMPORTANT]
>Asegúrate de que el `FIXUP_DEFINE_EXPORTS` macro aparece antes de la instrucción de inclusión.

Crear una función que tiene la misma firma de la función que tiene que quieres modificar el comportamiento. Esta es una función de ejemplo que reemplaza la `MessageBoxW` función.

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

La llamada a `DECLARE_FIXUP` asigna el `MessageBoxW` función a la nueva función de reemplazo. Cuando la aplicación intenta llamar a la `MessageBoxW` función, se llamará al método la función de reemplazo en su lugar.

#### <a name="protect-against-recursive-calls-to-functions-in-runtime-fixes"></a>Proteger contra recursiva llamadas a funciones de correcciones de tiempo de ejecución

Opcionalmente, puede aplicar la `reentrancy_guard` tipo a sus funciones que protegen frente a llamadas de recursiva a las funciones de correcciones de tiempo de ejecución.

Por ejemplo, podría producir una función de reemplazo para el `CreateFile` función. La implementación puede llamar a la `CopyFile` función, pero la implementación de la `CopyFile` puede llamar la función el `CreateFile` función. Esto puede provocar un ciclo de recursiva infinita de llamadas a la `CreateFile` función.

Para obtener más información sobre `reentrancy_guard` consulta [authoring.md](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/Authoring.md)

### <a name="configuration-data"></a>Datos de configuración

Si quieres agregar datos de configuración para la corrección de tiempo de ejecución, considera la posibilidad de agregarlo a la ``config.json``. De este modo, puedes usar el `FixupQueryCurrentDllConfig` fácilmente analizar esos datos. En este ejemplo se analiza un valor booleano y la cadena de ese archivo de configuración.

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

Mientras que Visual Studio permite el desarrollo más sencillo y la experiencia de depuración, existen algunas limitaciones.

En primer lugar, F5 debugging ejecuta la aplicación mediante la implementación de archivos sueltos de la ruta de acceso de carpeta de diseño de paquete, en lugar de instalación desde un .msix / paquete .appx.  Por lo general, la carpeta de diseño no tiene las mismas restricciones de seguridad como una carpeta de paquete instalado. Como resultado, puede no ser posible reproducir errores de denegación de acceso de ruta de acceso de paquete antes de aplicar una corrección de tiempo de ejecución.

Para solucionar este problema, utilice .msix / la implementación del paquete .appx en lugar de F5 sueltos implementación de archivo.  Para crear un .msix / archivo de paquete .appx usar la utilidad [MakeMSIX](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/make-appx-package--makeappx-exe-) desde el SDK de Windows, como se describió anteriormente. O bien, desde dentro de Visual Studio, haz clic en el nodo del proyecto de aplicación y seleccionar **almacén**->**Crear paquetes de aplicaciones**.

Otro problema con Visual Studio es que no tiene compatibilidad integrada para adjuntar a los procesos secundarios que se inicia el depurador.   Esto dificulta la lógica en la ruta de acceso de inicio de la aplicación de destino, que se debe adjuntar manualmente por Visual Studio después de iniciarse de depuración.

Para solucionar este problema, usa a un depurador que admite Adjuntar proceso secundario.  Ten en cuenta que por lo general, no es posible adjuntar a un depurador de just-in-time (JIT) a la aplicación de destino.  Esto es porque la mayoría de las técnicas JIT implica iniciar al depurador en lugar de la aplicación de destino, a través de la clave del registro de ImageFileExecutionOptions.  Esto impide que el mecanismo de detouring utilizado por PSFLauncher.exe para insertar FixupRuntime.dll en la aplicación de destino.  WinDbg, incluido en las [Herramientas de depuración para Windows](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/index)y obtenido desde el [Windows SDK](https://developer.microsoft.com/en-US/windows/downloads/windows-10-sdk), proceso secundario de admite adjunta.  Ahora también admite directamente [Inicio y la depuración de una aplicación para UWP.](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-a-uwp-app-using-windbg#span-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanlaunching-and-debugging-a-uwp-app)

Para depurar el inicio de la aplicación de destino como un proceso secundario, iniciar ``WinDbg``.

```ps
windbg.exe -plmPackage PSFSampleWithFixup_1.0.59.0_x86__7s220nvg1hg3m -plmApp PSFSample
```

En el ``WinDbg`` pedir, habilitar secundarios depuración y establecer puntos de interrupción apropiados.

```ps
.childdbg 1
g
```

(se ejecuta hasta que la aplicación de destino se inicia y entra en el depurador)

```ps
sxe ld fixup.dll
g
```

(que se ejecuta hasta que la corrección que se carga el archivo DLL)

```ps
bp ...
```

>[!NOTE]
> [PLMDebug](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/plmdebug) también se puede utilizar para asociar a un depurador a una aplicación al iniciarse y también se incluye en las [Herramientas de depuración para Windows](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/index).  Sin embargo, es más complejo de usar que la compatibilidad directa ahora proporcionada WinDbg.

## <a name="support-and-feedback"></a>Soporte técnico y comentarios

**Encuentra respuestas a tus preguntas**

¿Tienes alguna pregunta? Pregúntanos en Stack Overflow. Nuestro equipo supervisa estas [etiquetas](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). También puedes preguntarnos [aquí](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).
