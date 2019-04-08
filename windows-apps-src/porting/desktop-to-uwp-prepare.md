---
Description: En este artículo se enumera las cosas que necesita saber antes de empaquetar su aplicación de escritorio. Es posible que no tengas que hacer mucho para preparar la aplicación para el proceso de empaquetado.
Search.Product: eADQiWindows 10XVcnh
title: Preparar empaquetar una aplicación de escritorio (Desktop Bridge)
ms.date: 05/18/20188
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 71a57ca2-ca00-471d-8ad9-52f285f3022e
ms.localizationpriority: medium
ms.openlocfilehash: 49238ba1b72cf3daf46fb076432a478e9f381bc6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57600900"
---
# <a name="prepare-to-package-a-desktop-application"></a>Preparación del empaquetado de una aplicación de escritorio

En este artículo se enumeran los aspectos que debes tener en cuenta antes de empaquetar tu aplicación de escritorio. Puede que no tenga que hacer mucho para preparar la aplicación para el proceso de empaquetado, pero si cualquiera de los siguientes elementos se aplica a la aplicación, debe solucionarlo antes de empaquetar. Recuerda que Microsoft Store administra licencias y actualizaciones automáticas; por lo tanto, puedes quitar cualquier característica que relacione esas tareas del código base.

>[!IMPORTANT]
>La capacidad para crear un paquete de aplicación de Windows para su aplicación de escritorio (conocido también como el puente de escritorio) se introdujo en Windows 10, versión 1607, y solo puede usarse en proyectos que tienen como destino Windows 10 Anniversary Update (10.0; Compilación 14393) o una versión posterior de Visual Studio.

+ __La aplicación requiere una versión de .NET anterior a la 4.6.2__. Deberá asegurarse de que la aplicación se ejecuta en .NET 4.6.2. No puedes exigir o redistribuir versiones anteriores a 4.6.2. Esta es la versión de .NET incluida en la Actualización de aniversario de Windows 10. Comprobación de que la aplicación funciona en esta versión se asegurará de que la aplicación seguirá ser compatible con las actualizaciones futuras de Windows 10.  Si la aplicación tiene como destino .NET Framework 4.0 o posterior, se espera que se ejecutan en .NET Framework 4.6.2, pero aún debe probarlo.

+ __La aplicación siempre se ejecuta con privilegios de seguridad con privilegios elevados__. La aplicación necesita trabajar mientras se ejecuta como el usuario interactivo. Los usuarios que instalen la aplicación desde la Microsoft Store pueden no ser los administradores del sistema, lo que requiere la aplicación se ejecute con privilegios elevados significa que no se ejecutará correctamente para los usuarios estándar. Las aplicaciones que requieran el uso de privilegios elevados en cualquier parte de su funcionalidad no se aceptarán en Microsoft Store.

+ __La aplicación requiere un controlador de modo kernel o un servicio de Windows__. El puente es adecuado para una aplicación, pero no admite un controlador en modo kernel o un servicio de Windows que necesite ejecutarse en una cuenta de sistema. En lugar de un servicio de Windows usa una [tarea en segundo plano](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task).

+ __Los módulos de la aplicación se cargan durante la operación a aquellos procesos que no están en el paquete de la aplicación de Windows__. Esto no está permitido, lo que significa que las extensiones en proceso, como [extensiones de shell](https://msdn.microsoft.com/library/windows/desktop/dd758089.aspx), no se admiten. Pero si tienes dos aplicaciones en el mismo paquete, puedes realizar la comunicación entre procesos entre ellas.

+ __La aplicación usa un identificador de modelo del usuario aplicación (AUMID) personalizado__. Si llama su proceso [SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422.aspx) para establecer su propio AUMID, a continuación, solo puede usar el AUMID generado para él por el paquete de aplicación de entorno/Windows del modelo de aplicación. No se pueden definir AUMID personalizados.

+ __La aplicación modifica el subárbol del registro HKEY_LOCAL_MACHINE (HKLM)__. Cualquier intento por parte de la aplicación para crear una clave HKLM o para abrir una modificación, se producirá un error de acceso denegado. Recuerde que la aplicación tiene su propia vista virtualizada privada del registro, por lo que no se aplica la noción de un subárbol del registro de nivel de usuario y equipo (que es lo que es HKLM). Necesitas encontrar otra manera de lograr aquello para lo que usaste HKLM, como escribir en HKEY_CURRENT_USER (HKCU) en su lugar.

+ __La aplicación usa una subclave del registro de ddeexec como un medio para iniciar otra aplicación__. En su lugar, usa uno de los controladores de verbo DelegateExecute como lo configuran las distintas extensiones activables* en tu [manifiesto del paquete de la aplicación](https://msdn.microsoft.com/library/windows/apps/br211474.aspx).

+ __La aplicación escribe en la carpeta AppData o en el registro con la intención de compartir datos con otra aplicación__. Después de la conversión, AppData se redirige al almacén de datos locales de la aplicación, que es un almacén privado para cada aplicación para UWP.

  Todas las entradas que su aplicación escribe en el subárbol del registro HKEY_LOCAL_MACHINE se redirigen a un archivo binario aislado y todas las entradas que su aplicación escribe en la sección HKEY_CURRENT_USER del registro se colocan en privada por usuario, ubicación por aplicación. Para obtener más información acerca de la redirección de archivos y del Registro, consulta [Segundo plano del Puente de dispositivo de escritorio](desktop-to-uwp-behind-the-scenes.md).  

  Usa un medio diferente para compartir datos entre procesos. Para obtener más información, consulta [Almacenar y recuperar la configuración y otros datos de aplicación](https://msdn.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data).

+ __La aplicación escribe en el directorio de instalación de la aplicación__. Por ejemplo, la aplicación se escribe en un archivo de registro que se coloca en el mismo directorio que el archivo exe. Esto no se admite, por lo que tendrás que buscar otra ubicación, como el almacén de datos locales de la aplicación.

+ __La instalación de la aplicación requiere la interacción del usuario__. Instalador de la aplicación debe ser capaz de ejecutar de forma silenciosa y deben instalar todos los requisitos previos que no se encuentran de forma predeterminada en una imagen limpia del sistema operativo.

+ __La aplicación usa el directorio de trabajo actual__. En tiempo de ejecución, la aplicación de escritorio empaquetada no obtendrá el mismo directorio de trabajo que especificó anteriormente en el escritorio. Método abreviado LNK. Deberá cambiar su CWD en tiempo de ejecución si el directorio correcto es importante para su aplicación funcione correctamente.

+ __La aplicación requiere UIAccess__. Si la aplicación especifica `UIAccess=true` en el elemento `requestedExecutionLevel` del manifiesto de UAC, la conversión a UWP no es compatible actualmente. Para más información, consulta [UI Automation Security Overview (Introducción a la seguridad de la automatización de la interfaz de usuario)](https://msdn.microsoft.com/library/ms742884.aspx).

+ __La aplicación expone objetos COM__. Los procesos y extensiones del paquete se pueden registrar y usar como servidores COM y OLE; asimismo, ambos pueden estar tanto dentro como fuera del proceso (OOP).  La actualización de creadores agrega el soporte de Packaged COM, que proporciona la capacidad de registrar los servidores OOP COM y OLE que ahora están visibles fuera del paquete.  Consulta [COM Server and OLE Document support for Desktop Bridge (Compatibilidad del servidor COM y del documento OLE para el Puente de dispositivo de escritorio)](https://blogs.windows.com/buildingapps/2017/04/13/com-server-ole-document-support-desktop-bridge/#bjPyETFgtpZBGrS1.97).

   El soporte de Packaged COM funciona con las API de COM, pero no así con aquellas extensiones de la aplicación que dependen de la lectura directa del registro, ya que la ubicación de Packaged COM está en una ubicación privada.

+ __La aplicación expone a los ensamblados de GAC para su uso por otros procesos__. En la versión actual, la aplicación no puede exponer los ensamblados de GAC para su uso por los procesos que se originan desde los archivos ejecutables externos al paquete de aplicación de Windows. Los procesos internos del paquete pueden registrar y usar ensamblados GAC del modo habitual, pero no serán visibles externamente. Esto significa que los escenarios de interoperabilidad como OLE no funcionarán si los invocan procesos externos.

+ __La aplicación vincula bibliotecas en tiempo de ejecución de C (CRT) de una manera no compatible__. La biblioteca en tiempo de ejecución de Microsoft C/C++ ofrece rutinas de programación para el sistema operativo Microsoft Windows. Estas rutinas automatizan muchas tareas comunes de programación que no proporcionan los lenguajes C y C++. Si la aplicación utiliza la biblioteca en tiempo de ejecución de C o C++, deberá asegurarse de que está vinculada a una forma compatible.

    Visual Studio 2017 admite tanto la vinculación dinámica y estática, para permitir que el código use los archivos DLL comunes, como la vinculación estática, para vincular la biblioteca directamente en el código a la versión actual de CRT. Si es posible, se recomienda usar una aplicación dinámica de vinculación con Visual Studio 2017.

    La compatibilidad en versiones anteriores de Visual Studio varía. Consulta la tabla siguiente para obtener más información:

    <table>
    <th>Versión de Visual Studio</td><th>Vinculación dinámica</th><th>Vinculación estática</th></th>
    <tr><td>2005 (VC 8)</td><td>No se admite</td><td>Se admite</td>
    <tr><td>2008 (VC 9)</td><td>No se admite</td><td>Se admite</td>
    <tr><td>2010 (VC 10)</td><td>Se admite</td><td>Se admite</td>
    <tr><td>2012 (VC 11)</td><td>Se admite</td><td>No se admite</td>
    <tr><td>2013 (VC 12)</td><td>Se admite</td><td>No se admite</td>
    <tr><td>2015 y 2017 (VC 14)</td><td>Se admite</td><td>Se admite</td>
    </table>

    Nota: En todos los casos, debe vincular a la versión más reciente disponible públicamente CRT.

+ __La aplicación se instala y carga los ensamblados de la carpeta de Windows side-by-side__. Por ejemplo, la aplicación usa bibliotecas en tiempo de ejecución de C VC8 o VC9 y vincula dinámicamente desde la carpeta de Windows en paralelo, lo que significa que el código usa los archivos DLL comunes desde una carpeta compartida. Esto no se admite. Deberás vincularlas de forma estática mediante la vinculación a los archivos de biblioteca redistribuibles directamente en el código.

+ __La aplicación usa una dependencia en la carpeta System32/SysWOW64__. Para que las DLL funcionen, debes incluirlas en la parte del sistema de archivos virtual del paquete de la aplicación de Windows. Esto garantiza que la aplicación se comporta como si se instalaron los archivos DLL en el **System32**/**SysWOW64** carpeta. En la raíz del paquete, crea una carpeta denominada **VFS**. Dentro de esa carpeta, crear una carpeta **SystemX64** y otra **SystemX86**. A continuación, pon la versión de 32 bits de la DLL en la carpeta **SystemX86** y coloca la versión de 64 bits en la carpeta **SystemX64**.

+ __La aplicación usa un paquete de marcos de VCLibs__. Las bibliotecas de VCLibs 11 se pueden instalar directamente desde Microsoft Store si se definen como una dependencia en el paquete de la aplicación de Windows. Por ejemplo, si la aplicación usa los paquetes de marcos VCLibs Dev11, realice el siguiente cambio en el manifiesto del paquete de aplicación: En el `<Dependencies>` nodo, agregue:  
`<PackageDependency Name="Microsoft.VCLibs.110.00.UWPDesktop" MinVersion="11.0.24217.0" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" />`  
Durante la instalación desde Microsoft Store, se instalará la versión adecuada (x86 o x64) del marco de VCLibs 11 antes de la instalación de la aplicación.  
Las dependencias no se instalará si se instala la aplicación de instalación de prueba. Para instalar las dependencias en el equipo de forma manual, debes descargar e instalar el paquete de marcos de VCLibs apropiado para Puente de dispositivo de escritorio. Para obtener más información sobre estos escenarios, consulta [Using Visual C++ Runtime in Centennial project](https://blogs.msdn.microsoft.com/vcblog/2016/07/07/using-visual-c-runtime-in-centennial-project/) (Uso de Visual C++ Runtime en un proyecto Centennial).

  **Paquetes de marcos**:

  * [Paquetes de marco de trabajo de VC 14.0 para Desktop Bridge](https://www.microsoft.com/download/details.aspx?id=53175)
  * [Paquetes de marco de trabajo de VC 12.0 para Desktop Bridge](https://www.microsoft.com/download/details.aspx?id=53176)
  * [Paquetes de marco de trabajo de VC 11.0 para Desktop Bridge](https://www.microsoft.com/download/details.aspx?id=53340)


+ __La aplicación contiene una lista personalizada de salto__. Hay varios problemas y advertencias que se deben tener en cuenta al usar las listas de accesos directos.

    - __Arquitectura de la aplicación no coincide con el sistema operativo.__  Las Jump Lists actualmente no funcionan correctamente si no coincide con la aplicación y arquitecturas de sistema operativo (por ejemplo, un x86 aplicación ejecutándose en x64 Windows). En este momento, hay ninguna solución alternativa distinto a compilar la aplicación a la arquitectura de búsqueda de coincidencias.

    - __La aplicación crea entradas de la lista de salto y llama a [ICustomDestinationList::SetAppID](https://msdn.microsoft.com/library/windows/desktop/dd378403(v=vs.85).aspx) o [SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422(v=vs.85).aspx)__. No establezcas mediante programación el AppID en el código. Si lo haces, hará que las entradas de la lista de accesos directos no aparezcan. Si la aplicación necesita un identificador personalizado, especifique mediante el archivo de manifiesto. Consulte [empaquetar manualmente una aplicación de escritorio](desktop-to-uwp-manual-conversion.md) para obtener instrucciones. El AppID de la aplicación se especifica en la sección *YOUR_PRAID_HERE*.

    - __La aplicación agrega un vínculo de shell de lista de salto que hace referencia a un archivo ejecutable en el paquete__. No puedes iniciar ejecutables directamente en el paquete de una lista de accesos directos (a excepción de la ruta de acceso absoluta del propio .exe de una aplicación). En su lugar, registre un alias de la ejecución de aplicación (que permite que la aplicación empaquetada de escritorio iniciar a través de una palabra clave como si estuviera en la ruta de acceso) y establezca la ruta de acceso de destino del vínculo en el alias en su lugar. Para obtener más información sobre cómo usar la extensión appExecutionAlias, consulte [integrar su aplicación de escritorio con Windows 10](desktop-to-uwp-extensions.md). Ten en cuenta que si necesitas que los recursos del vínculo de la lista de accesos directos coincida con el .exe original, tendrás que establecer recursos como el icono con [**SetIconLocation**](https://msdn.microsoft.com/library/windows/desktop/bb761047(v=vs.85).aspx) y el nombre para mostrar con PKEY_Title igual que harías para otras entradas personalizadas.

    - __La aplicación agrega entradas de la lista de salto que hace referencia a recursos en el paquete de la aplicación mediante rutas de acceso absolutas__. Puede cambiar la ruta de instalación de una aplicación cuando se actualizan sus paquetes, cambiar la ubicación de recursos (por ejemplo, iconos, documentos, archivo ejecutable y así sucesivamente). Si las entradas de lista de salto hacer referencia a dichos activos por rutas de acceso absolutas, entonces la aplicación debe actualizar su jump list de periódicamente (por ejemplo en la aplicación inicie) para asegurarse de que las rutas de acceso se resuelven correctamente. Como alternativa, usa las API [**Windows.UI.StartScreen.JumpList**](https://msdn.microsoft.com/library/windows/apps/windows.ui.startscreen.jumplist.aspx) de UWP en su lugar, que permiten hacer referencia a los recursos de imagen y cadena con el esquema URI de ms-resource relativo del paquete (que también reconoce el idioma, PPP y contraste alto).

+ __La aplicación inicia una utilidad para llevar a cabo tareas__. Evita iniciar utilidades de comando como PowerShell y Cmd.exe. De hecho, si los usuarios instalan la aplicación en un sistema que se ejecuta el Windows 10 S, a continuación, la aplicación no podrá iniciarlas en absoluto. Esto podría impedir que la aplicación de envío a la Microsoft Store porque todas las aplicaciones enviadas a la Microsoft Store deben ser compatibles con Windows 10 S.

Si inicias una utilidad, puedes obtener una forma sencilla de conseguir información del sistema operativo, acceder al registro o acceder a las funcionalidades del sistema. Sin embargo, puedes usar las API de UWP para llevar a cabo a este tipo de tareas en su lugar. Estas API tienen un mayor rendimiento ya que no necesitan un ejecutable independiente que se ejecuta, pero más importante aún, mantienen la aplicación lleguen fuera del paquete. Diseño de la aplicación sigue siendo coherente con el aislamiento, confianza y seguridad que se incluye con una aplicación que ha empaquetado y la aplicación se comportará según lo previsto en los sistemas que ejecutan Windows 10 S.

+ __La aplicación hospeda complementos, los complementos o extensiones__.   En muchos casos, las extensiones de estilo COM siguen funcionando siempre y cuando la extensión no se haya empaquetado y se instale como extensión de plena confianza. Eso es porque estos instaladores pueden usar sus capacidades de plena confianza para modificar el registro y coloque los archivos de extensión siempre que la aplicación host espera a encontrarlos.

   Sin embargo, si esas extensiones se empaqueta y, a continuación, se instala como un paquete de aplicación de Windows, no funcionará porque cada paquete (la aplicación host y la extensión) será completamente aislada entre sí. Para obtener más información acerca de cómo las aplicaciones están aisladas del sistema, consulte [en segundo plano del puente de escritorio](desktop-to-uwp-behind-the-scenes.md).

 Todas las aplicaciones y extensiones que instalen los usuarios en un sistema que ejecute Windows 10 S, deben instalarse como paquetes de aplicación de Windows. Por lo que si se va a empaquetar sus extensiones o va a animar a los colaboradores de empaquetarlos, tenga en cuenta cómo puede facilitar la comunicación entre el paquete de aplicación host y todos los paquetes de extensión. Una solución que puedes plantear, es usar un [servicio de aplicación](../launch-resume/app-services.md).

+ __La aplicación genera código__. La aplicación puede generar código que lo consume en la memoria, pero evite escribir genera código en el disco porque el proceso de certificación de aplicaciones de Windows no puede validar código antes del envío de la aplicación. Además, las aplicaciones que escriben código en el disco no se ejecutarán correctamente en sistemas que ejecutan Windows 10 S. Esto podría impedir que la aplicación de envío a la Microsoft Store porque todas las aplicaciones enviadas a la Microsoft Store deben ser compatibles con Windows 10 S.

>[!IMPORTANT]
> Después de crear el paquete de aplicación de Windows, por favor, pruebe la aplicación para asegurarse de que funciona correctamente en sistemas que ejecutan Windows 10 S. Todas las aplicaciones enviadas a la Microsoft Store deben ser compatibles con aplicaciones de Windows 10 S. que no son compatibles no se aceptan en el almacén. Consulta [Probar la aplicación de Windows en Windows 10 S](desktop-to-uwp-test-windows-s.md).

## <a name="next-steps"></a>Pasos siguientes

**Encuentre respuestas a sus preguntas**

¿Tienes alguna pregunta? Pregúntanos en Stack Overflow. Nuestro equipo supervisa estas [etiquetas](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). También puedes preguntarnos [aquí](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Proporcionar comentarios o hacer sugerencias**

Consulta [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).

**Crear un paquete de aplicación de Windows para su aplicación de escritorio**

Consulta [Crear un paquete de aplicación de Windows](desktop-to-uwp-root.md#convert).
