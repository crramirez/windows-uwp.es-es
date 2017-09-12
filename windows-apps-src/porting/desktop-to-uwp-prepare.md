---
author: normesta
Description: "En este artículo se enumeran los aspectos que debes tener en cuenta antes de empaquetar la aplicación con el Puente de dispositivo de escritorio. Es posible que no tengas que hacer mucho para preparar la aplicación para el proceso de empaquetado."
Search.Product: eADQiWindows 10XVcnh
title: "Preparar para empaquetar una aplicación (Puente de dispositivo de escritorio)"
ms.author: normesta
ms.date: 05/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 71a57ca2-ca00-471d-8ad9-52f285f3022e
ms.openlocfilehash: f7337ee7bf78730e2300a11d7d606f328c7c418b
ms.sourcegitcommit: 77bbd060f9253f2b03f0b9d74954c187bceb4a30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2017
---
# <a name="prepare-to-package-an-app-desktop-bridge"></a>Preparar para empaquetar una aplicación (Puente de dispositivo de escritorio)

En este artículo se enumeran los aspectos que debes tener en cuenta antes de empaquetar tu aplicación de escritorio. Es posible que no tengas que hacer mucho para preparar la aplicación para el proceso de empaquetado, pero si alguno de los siguientes elementos tiene algo que ver con la aplicación, tendrás que abordarlo antes del empaquetado. Recuerda que la Tienda Windows administra licencias y actualizaciones automáticas; por lo tanto, puedes quitar cualquier característica que relacione esas tareas del código base.

+ __La aplicación usa una versión de .NET anterior a 4.6.1__. Solo .NET 4.6.1 es compatible. Tendrás que volver a destinar la aplicación a .NET 4.6.1 antes de empaquetarla.

+ __La aplicación siempre se ejecuta con privilegios de seguridad elevados__. La aplicación necesita trabajar mientras se ejecuta como el usuario interactivo. Los usuarios que instalen la aplicación desde la Tienda Windows pueden no ser administradores del sistema, por lo tanto, requerir que la aplicación se ejecute con privilegios elevados significa que no se ejecutará correctamente para los usuarios estándar.

+ __La aplicación requiere un controlador de modo kernel o un servicio de Windows__. El puente es adecuado para una aplicación, pero no admite un controlador en modo kernel o un servicio de Windows que necesite ejecutarse en una cuenta de sistema. En lugar de un servicio de Windows usa una [tarea en segundo plano](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task).

+ __Los módulos de la aplicación se cargan durante la operación a aquellos procesos que no están en el paquete de la aplicación de Windows__. Esto no está permitido, lo que significa que las extensiones en proceso, como [extensiones de shell](https://msdn.microsoft.com/library/windows/desktop/dd758089.aspx), no se admiten. Pero si tienes dos aplicaciones en el mismo paquete, puedes realizar la comunicación entre procesos entre ellas.

+ __La aplicación usa un identificador del modelo de usuario de la aplicación (AUMID) personalizado__. Si el proceso llama a [SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422.aspx) para establecer su propio AUMID, entonces solo puede usar el AUMID generado para este por el entorno del modelo de aplicaciones o el paquete de la aplicación de Windows. No se pueden definir AUMID personalizados.

+ __La aplicación modifica el subárbol del Registro HKEY_LOCAL_MACHINE (HKLM)__. Cualquier intento de tu aplicación para crear una clave HKLM o abrir una para realizar modificaciones producirá un error de acceso denegado. Recuerda que tu aplicación tiene su propia vista virtualizada privada del registro, por lo que no se aplica la noción de un subárbol del registro de nivel de usuario y de equipo (que es lo que HKLM es). Necesitas encontrar otra manera de lograr aquello para lo que usaste HKLM, como escribir en HKEY_CURRENT_USER (HKCU) en su lugar.

+ __La aplicación usa una subclave del Registro ddeexec como un mecanismo para iniciar otra aplicación__. En su lugar, usa uno de los controladores de verbo DelegateExecute como lo configuran las distintas extensiones activables* en tu [manifiesto del paquete de la aplicación](https://msdn.microsoft.com/library/windows/apps/br211474.aspx).

+ __La aplicación escribe en la carpeta AppData o en el Registro con la intención de compartir datos con otra aplicación__. Después de la conversión, AppData se redirige al almacén de datos locales de la aplicación, que es un almacén privado para cada aplicación para UWP.

  Todas las entradas que tu aplicación escribe en el subárbol del Registro HKEY_LOCAL_MACHINE se redirigen a un archivo binario aislado y las entradas que tu aplicación escribe en el subárbol del Registro HKEY_CURRENT_USER se colocan en una ubicación privada por usuario y por aplicación. Para obtener más información acerca de la redirección de archivos y del Registro, consulta [Segundo plano del Puente de dispositivo de escritorio](desktop-to-uwp-behind-the-scenes.md).  

  Usa un medio diferente para compartir datos entre procesos. Para más información, consulta [Almacenar y recuperar la configuración y otros datos de aplicación](https://msdn.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data).

+ __La aplicación escribe en el directorio de instalación de la aplicación__. Por ejemplo, la aplicación se escribe en un archivo de registro que se coloca en el mismo directorio que el archivo .exe. Esto no se admite, por lo que tendrás que buscar otra ubicación, como el almacén de datos locales de la aplicación.

+ __La instalación de la aplicación requiere la interacción del usuario__. El instalador de la aplicación debe ser capaz de ejecutarse en silencio, y se deben instalar todos sus requisitos previos que no estén activados de forma predeterminada en una imagen de sistema operativo limpia.

+ __La aplicación usa el directorio de trabajo actual__. En tiempo de ejecución, la aplicación de escritorio empaquetada no obtendrá el mismo directorio de trabajo que especificaste anteriormente en el escritorio. Acceso directo del vínculo. Debes cambiar tu CWD en tiempo de ejecución si tener el directorio correcto es importante para que tu aplicación funcione correctamente.

+ __La aplicación requiere UIAccess__. Si la aplicación especifica `UIAccess=true` en el elemento `requestedExecutionLevel` del manifiesto de UAC, la conversión a UWP no es compatible actualmente. Para más información, consulta [UI Automation Security Overview (Introducción a la seguridad de la automatización de la interfaz de usuario)](https://msdn.microsoft.com/library/ms742884.aspx).

+ __La aplicación expone objetos COM__. Los procesos y extensiones del paquete se pueden registrar y usar como servidores COM y OLE; asimismo, ambos pueden estar tanto dentro como fuera del proceso (OOP).  La actualización de creadores agrega el soporte de Packaged COM, que proporciona la capacidad de registrar los servidores OOP COM y OLE que ahora están visibles fuera del paquete.  Consulta [COM Server and OLE Document support for Desktop Bridge (Compatibilidad del servidor COM y del documento OLE para el Puente de dispositivo de escritorio)](https://blogs.windows.com/buildingapps/2017/04/13/com-server-ole-document-support-desktop-bridge/#bjPyETFgtpZBGrS1.97).

   El soporte de Packaged COM funciona con las API de COM, pero no así con aquellas extensiones de la aplicación que dependen de la lectura directa del registro, ya que la ubicación de Packaged COM está en una ubicación privada.

+ __La aplicación expone ensamblados GAC para que los usen otros procesos__. En la versión actual, la aplicación no puede exponer ensamblados GAC para que los usen procesos originados desde archivos ejecutables externos en el paquete de la aplicación de Windows. Los procesos internos del paquete pueden registrar y usar ensamblados GAC del modo habitual, pero no serán visibles externamente. Esto significa que los escenarios de interoperabilidad como OLE no funcionarán si los invocan procesos externos.

+ __La aplicación está vinculando las bibliotecas en tiempo de ejecución de C (CRT) de forma no admitida__. La biblioteca en tiempo de ejecución de Microsoft C/C++ ofrece rutinas de programación para el sistema operativo Microsoft Windows. Estas rutinas automatizan muchas tareas comunes de programación que no proporcionan los lenguajes C y C++. Si la aplicación usa la biblioteca en tiempo de ejecución de C o C++, debes asegurarte de que esté vinculada de manera admitida.

    Visual Studio 2015 admite tanto la vinculación dinámica, para permitir que el código use los archivos DLL comunes, como la vinculación estática, para vincular la biblioteca directamente en el código a la versión actual de CRT. Si es posible, se recomienda que la aplicación use la vinculación dinámica con VS 2015.

    La compatibilidad en versiones anteriores de Visual Studio varía. Consulta la tabla siguiente para obtener más información:

    <table>
    <th>Versión de Visual Studio</td><th>Vinculación dinámica</th><th>Vinculación estática</th></th>
    <tr><td>2005 (VC 8)</td><td>No se admite</td><td>Se admite</td>
    <tr><td>2008 (VC 9)</td><td>No se admite</td><td>Se admite</td>
    <tr><td>2010 (VC 10)</td><td>Se admite</td><td>Se admite</td>
    <tr><td>2012 (VC 11)</td><td>Se admite</td><td>No se admite</td>
    <tr><td>2013 (VC 12)</td><td>Se admite</td><td>No se admite</td>
    <tr><td>2015 (VC 14)</td><td>Se admite</td><td>Se admite</td>
    </table>

    Nota: En todos los casos, debes realizar la vinculación a la última versión de CRT disponible públicamente.

+ __La aplicación se instala y carga los ensamblados desde la carpeta de Windows en paralelo__. Por ejemplo, la aplicación usa las bibliotecas en tiempo de ejecución de C VC8 o VC9 y las vincula de forma dinámica desde la carpeta de Windows en paralelo, lo que significa que el código usa los archivos DLL comunes de una carpeta compartida. Esto no se admite. Deberás vincularlas de forma estática mediante la vinculación a los archivos de biblioteca redistribuibles directamente en el código.

+ __La aplicación usa una dependencia en la carpeta System32/SysWOW64__. Para que las DLL funcionen, debes incluirlas en la parte del sistema de archivos virtual del paquete de la aplicación de Windows. Esto garantiza que la aplicación se comporta como si las DLL se hubieran instalado en la carpeta **System32**/**SysWOW64**. En la raíz del paquete, crea una carpeta denominada **VFS**. Dentro de esa carpeta, crear una carpeta **SystemX64** y otra **SystemX86**. A continuación, pon la versión de 32 bits de la DLL en la carpeta **SystemX86** y coloca la versión de 64 bits en la carpeta **SystemX64**.

+ __La aplicación usa el paquete de marcos de Dev11 VCLibs__. Las bibliotecas de VCLibs 11 se pueden instalar directamente desde la Tienda Windows si se definen como una dependencia en el paquete de la aplicación de Windows. Para ello, realizar el siguiente cambio en el manifiesto del paquete de la aplicación. En el nodo `<Dependencies>`, agrega:  
`<PackageDependency Name="Microsoft.VCLibs.110.00.UWPDesktop" MinVersion="11.0.24217.0" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" />`  
Durante la instalación desde la Tienda Windows, se instalará la versión adecuada (x86 o x64) del marco de VCLibs 11 antes de la instalación de la aplicación.  
Las dependencias no se instalarán si la aplicación se instala mediante el método de instalación de prueba. Para instalar las dependencias en el equipo de forma manual, debe descargar e instalar los [paquetes de marcos de VC 11.0 para puente de escritorio](https://www.microsoft.com/download/details.aspx?id=53340&WT.mc_id=DX_MVP4025064). Para obtener más información sobre estos escenarios, consulta [Using Visual C++ Runtime in Centennial project](https://blogs.msdn.microsoft.com/vcblog/2016/07/07/using-visual-c-runtime-in-centennial-project/) (Uso de Visual C++ Runtime en el proyecto Centennial).

+ __La aplicación contiene una lista de accesos directos personalizada__. Hay varios problemas y advertencias que se deben tener en cuenta al usar las listas de accesos directos.

    - __La arquitectura de tu aplicación no coincide con el sistema operativo.__  Actualmente, las listas de accesos directos no funcionan correctamente si las arquitecturas de la aplicación y del sistema operativo no coinciden (por ejemplo, una aplicación x86 que se ejecuta en Windows x64). En este momento, no existe ninguna solución alternativa que no sea la de volver a compilar la aplicación con la arquitectura coincidente.

    - __La aplicación crea entradas de lista de accesos directos y llama a [ICustomDestinationList::SetAppID](https://msdn.microsoft.com/library/windows/desktop/dd378403(v=vs.85).aspx) o [SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422(v=vs.85).aspx)__. No establezcas mediante programación el AppID en el código. Si lo haces, hará que las entradas de la lista de accesos directos no aparezcan. Si la aplicación necesita un identificador personalizado, especifícalo mediante el archivo de manifiesto. Consulta [Empaquetar una aplicación de forma manual (Puente de dispositivo de escritorio)](desktop-to-uwp-manual-conversion.md) para obtener instrucciones. El AppID de la aplicación se especifica en la sección *YOUR_PRAID_HERE*.

    - __La aplicación agrega un vínculo de shell de lista de accesos directos que hace referencia a un ejecutable del paquete__. No puedes iniciar ejecutables directamente en el paquete de una lista de accesos directos (a excepción de la ruta de acceso absoluta del propio .exe de una aplicación). En su lugar, registra un alias de ejecución de la aplicación (que permite que la aplicación de escritorio empaquetada se inicie a través de una palabra clave como si estuviera en PATH) y establece la ruta de acceso de destino del vínculo en el alias en su lugar. Para obtener más información sobre cómo usar la extensión appExecutionAlias, consulta [Integrar la aplicación con Windows 10 (Puente de dispositivo de escritorio)](desktop-to-uwp-extensions.md). Ten en cuenta que si necesitas que los recursos del vínculo de la lista de accesos directos coincida con el .exe original, tendrás que establecer recursos como el icono con [**SetIconLocation**](https://msdn.microsoft.com/library/windows/desktop/bb761047(v=vs.85).aspx) y el nombre para mostrar con PKEY_Title igual que harías para otras entradas personalizadas.

    - __La aplicación agrega entradas de lista de accesos directos que hacen referencia a recursos del paquete de la aplicación mediante rutas de acceso absolutas__. La ruta de instalación de una aplicación puede cambiar cuando se actualizan sus paquetes, cambiando la ubicación de los recursos (como iconos, documentos, ejecutable, etcétera). Si las entradas de la lista de accesos directos hacen referencia a estos recursos mediante rutas de acceso absolutas, la aplicación debe actualizar periódicamente su lista de accesos directos (por ejemplo, al iniciar la aplicación) para garantizar que las rutas de acceso se resuelven correctamente. Como alternativa, usa las API [**Windows.UI.StartScreen.JumpList**](https://msdn.microsoft.com/library/windows/apps/windows.ui.startscreen.jumplist.aspx) de UWP en su lugar, que permiten hacer referencia a los recursos de imagen y cadena con el esquema URI de ms-resource relativo del paquete (que también reconoce el idioma, PPP y contraste alto).

+ __La aplicación inicia una utilidad para realizar tareas__. Evita iniciar utilidades de comando como PowerShell y Cmd.exe. De hecho, si los usuarios instalan la aplicación en un sistema que ejecuta Windows 10 S, la aplicación no podrá iniciar estas utilidades. Esto podría bloquear la aplicación del envío a la Tienda Windows, ya que todas las aplicaciones enviadas a la Tienda Windows deben ser compatibles con Windows 10S.

Si inicias una utilidad, puedes obtener una forma sencilla de conseguir información del sistema operativo, acceder al registro o acceder a las funcionalidades del sistema. Sin embargo, puedes usar las API de UWP para llevar a cabo a este tipo de tareas en su lugar. Estas API son más eficaces porque no necesitan usar un archivo ejecutable independiente y, aún mejor, evitan que la aplicación salga del paquete. El diseño de la aplicación mantiene su coherencia gracias al aislamiento, confiabilidad y seguridad que proporciona una aplicación de puente de dispositivo, y la aplicación se comportará según lo esperado en aquellos sistemas que ejecuten Windows 10 S.

+ __La aplicación contiene complementos, módulos o extensiones__.   En muchos casos, las extensiones de estilo COM siguen funcionando siempre y cuando la extensión no se haya empaquetado y se instale como extensión de plena confianza. Esto es debido a que los instaladores pueden usar las funcionalidades de plena confianza para modificar el registro y colocar archivos de extensión donde se espera que la aplicación host los encuentre.

   Sin embargo, si las extensiones se empaquetan y se instalan como un paquete de aplicación de Windows, no funcionarán porque los paquetes (formados por la aplicación host y la extensión) estarán aislados entre sí. Para leer más acerca de cómo el Puente de dispositivo de escritorio aísla las aplicaciones del sistema, consulta [Segundo plano del Puente de dispositivo de escritorio](desktop-to-uwp-behind-the-scenes.md).

 Todas las aplicaciones y extensiones que instalen los usuarios en un sistema que ejecute Windows 10 S, deben instalarse como paquetes de aplicación de Windows. Por lo tanto, si vas a empaquetar las extensiones o tienes previsto animar a tus colaboradores a empaquetarlas, ten en cuenta el modo de facilitar la comunicación entre el paquete de la aplicación host y los paquetes de la extensión. Una solución que puedes plantear, es usar un [servicio de aplicación](../launch-resume/app-services.md).

+ __La aplicación genera código__. La aplicación puede generar código que consume en la memoria, pero evita escribir código generado en el disco, debido a que el proceso de certificación de aplicaciones de Windows no puede validar el código antes de enviar la aplicación. Además, las aplicaciones que escriben código en el disco no se ejecutarán correctamente en sistemas que ejecutan Windows 10S. Esto podría bloquear la aplicación del envío a la Tienda Windows, ya que todas las aplicaciones enviadas a la Tienda Windows deben ser compatibles con Windows 10S.

+ __La aplicación usa la API de MAPI__. Actualmente no se admite la [API de Outlook MAPI API](https://msdn.microsoft.com/library/office/cc765775.aspx(d=robot)) en aplicaciones de Puente de dispositivo de escritorio.

## <a name="next-steps"></a>Pasos siguientes

**Crear un paquete de aplicación de Windows para tu aplicación de escritorio**

Consulta [Crear un paquete de aplicación de Windows](desktop-to-uwp-root.md#convert).

**Encuentra respuestas a preguntas específicas**

Nuestro equipo supervisa estas [etiquetas de StackOverflow](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge).

**Envíanos tus comentarios acerca de este artículo**

Usa la sección comentarios que tienes a continuación.
