---
author: awkoren
Description: "En este artículo se enumeran los aspectos que debes tener en cuenta antes de convertir la aplicación con el puente de escritorio a UWP. Es posible que no tengas que hacer mucho para preparar la aplicación para el proceso de conversión."
Search.Product: eADQiWindows 10XVcnh
title: "Preparar la aplicación para el puente de escritorio a UWP"
translationtype: Human Translation
ms.sourcegitcommit: f7a8b8d586983f42fe108cd8935ef084eb108e35
ms.openlocfilehash: 81a2485d5be22dd392c21aaff281c1c9263883a9

---

# <a name="prepare-an-app-for-conversion-with-the-desktop-bridge"></a>Preparar una aplicación para convertirla con el puente de escritorio

En este artículo se enumeran los aspectos que debes tener en cuenta antes de convertir la aplicación con el puente de escritorio a UWP. Es posible que no tengas que hacer mucho para preparar la aplicación para el proceso de conversión, pero si alguno de los siguientes elementos se aplica a tu aplicación, tendrás que abordarlo antes de la conversión. Recuerda que la Tienda Windows administra licencias y actualizaciones automáticas; por lo tanto, puedes quitar dichas características de tu código base.

+ __La aplicación usa una versión de .NET anterior a 4.6.1__. Solo .NET 4.6.1 es compatible. Debes redestinar tu aplicación a .NET 4.6.1 antes de la conversión. 

+ __La aplicación siempre se ejecuta con privilegios de seguridad elevados__. La aplicación necesita trabajar mientras se ejecuta como el usuario interactivo. Los usuarios que instalen la aplicación desde la Tienda Windows pueden no ser administradores del sistema, por lo tanto, requerir que la aplicación se ejecute con privilegios elevados significa que no se ejecutará correctamente para los usuarios estándar.

+ __La aplicación requiere un controlador de modo kernel o un servicio de Windows__. El puente es adecuado para una aplicación, pero no admite un controlador en modo kernel o un servicio de Windows que necesite ejecutarse en una cuenta de sistema. En lugar de un servicio de Windows usa una [tarea en segundo plano](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task).

+ __Los módulos de la aplicación se cargan en proceso a los procesos que no están en el paquete AppX__. Esto no está permitido, lo que significa que las extensiones en proceso, como [extensiones de shell](https://msdn.microsoft.com/library/windows/desktop/dd758089.aspx), no se admiten. Pero si tienes dos aplicaciones en el mismo paquete, puedes realizar la comunicación entre procesos entre ellas.

+ __La aplicación llama a [SetDllDirectory](https://msdn.microsoft.com/library/windows/desktop/ms686203) o [AddDllDirectory](https://msdn.microsoft.com/library/windows/desktop/hh310513)__. Actualmente no se admiten estas funciones para aplicaciones convertidas. Estamos trabajando para agregar compatibilidad en una versión futura. Como solución alternativa, puedes copiar los archivos .dll que buscaste mediante estas funciones a la raíz del paquete. 

+ __La aplicación usa un identificador del modelo de usuario de la aplicación (AUMID) personalizado__. Si el proceso llama a [SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422.aspx) para establecer su propio AUMID, entonces solo puede usar el AUMID generado para este por el entorno del modelo de aplicaciones o el paquete AppX. No se pueden definir AUMID personalizados.

+ __La aplicación modifica el subárbol del Registro HKEY_LOCAL_MACHINE (HKLM)__. Cualquier intento de tu aplicación para crear una clave HKLM o abrir una para realizar modificaciones producirá un error de acceso denegado. Recuerda que tu aplicación tiene su propia vista virtualizada privada del registro, por lo que no se aplica la noción de un subárbol del registro de nivel de usuario y de equipo (que es lo que HKLM es). Necesitas encontrar otra manera de lograr aquello para lo que usaste HKLM, como escribir en HKEY_CURRENT_USER (HKCU) en su lugar.

+ __La aplicación usa una subclave del Registro ddeexec como un mecanismo para iniciar otra aplicación__. En su lugar, usa uno de los controladores de verbo DelegateExecute como lo configuran las distintas extensiones activables* en tu [manifiesto del paquete de la aplicación](https://msdn.microsoft.com/library/windows/apps/br211474.aspx).

+ __La aplicación escribe en la carpeta AppData con la intención de compartir datos con otra aplicación__. Después de la conversión, AppData se redirige al almacén de datos locales de la aplicación, que es un almacén privado para cada aplicación para UWP. Usa un medio diferente para compartir datos entre procesos. Para más información, consulta [Almacenar y recuperar la configuración y otros datos de aplicación](https://msdn.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data).

+ __La aplicación escribe en el directorio de instalación de la aplicación__. Por ejemplo, la aplicación se escribe en un archivo de registro que se coloca en el mismo directorio que el archivo .exe. Esto no se admite, por lo que tendrás que buscar otra ubicación, como el almacén de datos locales de la aplicación.

+ __La instalación de la aplicación requiere la interacción del usuario__. El instalador de la aplicación debe ser capaz de ejecutarse en silencio, y se deben instalar todos sus requisitos previos que no estén activados de forma predeterminada en una imagen de sistema operativo limpia.

+ __La aplicación usa el directorio de trabajo actual__. En tiempo de ejecución, la aplicación convertida no obtendrá el mismo directorio de trabajo que especificaste anteriormente en el escritorio. Acceso directo del vínculo. Debes cambiar tu CWD en tiempo de ejecución si tener el directorio correcto es importante para que tu aplicación funcione correctamente.

+ __La aplicación requiere UIAccess__. Si la aplicación especifica `UIAccess=true` en el elemento `requestedExecutionLevel` del manifiesto de UAC, la conversión a UWP no es compatible actualmente. Para más información, consulta [UI Automation Security Overview (Introducción a la seguridad de la automatización de la interfaz de usuario](https://msdn.microsoft.com/library/ms742884.aspx).

+ __La aplicación expone objetos COM o ensamblados GAC para que los usen otros procesos__. En la versión actual, la aplicación no puede exponer objetos COM ni ensamblados GAC para que los usen procesos originados por archivos ejecutables externos en el paquete AppX. Los procesos internos del paquete pueden registrar y usar objetos COM y ensamblados GAC del modo habitual, pero no serán visibles externamente. Esto significa que los escenarios de interoperabilidad como OLE no funcionarán si los invocan procesos externos. 

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

+ __La aplicación usa una dependencia en la carpeta System32/SysWOW64__. Para que las DLL funcionen, debes incluirlas en la parte del sistema de archivos virtual del paquete AppX. Esto garantiza que la aplicación se comporta como si las DLL se hubieran instalado en la carpeta **System32**/**SysWOW64**. En la raíz del paquete, crea una carpeta denominada **VFS**. Dentro de esa carpeta, crear una carpeta **SystemX64** y otra **SystemX86**. A continuación, pon la versión de 32 bits de la DLL en la carpeta **SystemX86** y coloca la versión de 64 bits en la carpeta **SystemX64**.

+ __La aplicación usa el paquete de marcos de Dev11 VCLibs__. Las bibliotecas de VCLibs 11 se pueden instalar directamente desde la Tienda Windows si se definen como una dependencia en el paquete AppX. Para ello, realizar el siguiente cambio en el manifiesto del paquete de la aplicación. En el nodo `<Dependencies>`, agrega:  
`<PackageDependency Name="Microsoft.VCLibs.110.00.UWPDesktop" MinVersion="11.0.24217.0" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" />`  
Durante la instalación desde la Tienda Windows, se instalará la versión adecuada (x86 o x64) del marco de VCLibs 11 antes de la instalación de la aplicación.  
Las dependencias no se instalarán si la aplicación se instala mediante el método de instalación de prueba. Para instalar las dependencias en el equipo de forma manual, debe descargar e instalar los [paquetes de marcos de VC 11.0 para puente de escritorio](https://www.microsoft.com/download/details.aspx?id=53340&WT.mc_id=DX_MVP4025064). Para obtener más información sobre estos escenarios, consulta [Using Visual C++ Runtime in Centennial project](https://blogs.msdn.microsoft.com/vcblog/2016/07/07/using-visual-c-runtime-in-centennial-project/) (Uso de Visual C++ Runtime en el proyecto Centennial.


<!--HONumber=Dec16_HO1-->


