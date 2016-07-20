---
author: awkoren
Description: "Prepara la aplicación de escritorio de Windows (por ejemplo, Win32, WPF y Windows Forms) para la conversión a una aplicación para la Plataforma universal de Windows (UWP) mediante el uso de las extensiones de conversión de escritorio."
Search.Product: eADQiWindows 10XVcnh
title: "Convertir la aplicación de escritorio a una aplicación para la Plataforma universal de Windows (UWP)"
translationtype: Human Translation
ms.sourcegitcommit: 45b9170ed311e6f17d1c51c0b7d1d288e07184a9
ms.openlocfilehash: c5ffb1e912c2953d5f813f099e036d2d7b395b69

---

# Convertir la aplicación de escritorio a una aplicación para la Plataforma universal de Windows (UWP)

\[Parte de la información hace referencia a la versión preliminar del producto, el cual puede sufrir importantes modificaciones antes de que se publique la versión comercial. Microsoft no ofrece ninguna garantía, expresa o implícita, con respecto a la información que se ofrece aquí.\]

Prepara la aplicación de escritorio de Windows (por ejemplo, Win32, WPF y Windows Forms) para la conversión a una aplicación para la Plataforma universal de Windows (UWP) mediante el uso de las extensiones de conversión de escritorio.

## Ventajas de la conversión de la aplicación a UWP

UWP con extensiones de conversión de escritorio es un puente que te permite convertir tu aplicación de escritorio clásica (por ejemplo, Win32, Windows Forms y WPF) o un juego en una aplicación o juego para la Plataforma universal de Windows (UWP). Para obtener más información, consulta [Guía de aplicaciones para UWP](https://msdn.microsoft.com/library/windows/apps/dn894631.aspx). Después de la conversión, la aplicación de escritorio clásica se empaqueta, se le realiza un mantenimiento y se implementa en forma de un paquete de la aplicación para UWP (un .appx o un .appxbundle) destinado a escritorio de Windows 10.

Hay dos elementos de la tecnología que permiten que las aplicaciones de escritorio se conviertan en paquetes para UWP. El primero es Desktop App Converter, que toma los archivos binarios existentes y vuelve a empaquetarlos como un paquete para UWP. Tu código sigue siendo el mismo, solo se empaqueta de forma diferente. La segunda parte abarca tecnologías en tiempo de ejecución en la actualización de Windows Anniversary que permiten que un paquete para UWP tenga archivos ejecutables que se ejecuten como de plena confianza en lugar de en un contenedor de aplicación. Esta tecnología también ofrece una identidad del paquete a una aplicación convertida, algo necesario para usar algunas API de UWP.

Estas son algunas de las ventajas de convertir tu aplicación de escritorio clásica.

* La experiencia de instalación de la aplicación es mucho más sencilla para los clientes. Se puede implementar en los equipos mediante una instalación de prueba (consulta [Realizar la instalación de prueba de aplicaciones de línea de negocio en Windows10](https://technet.microsoft.com/library/mt269549.aspx)) y no dejar seguimiento después de la instalación. A largo plazo, también podrás publicar la aplicación en la Tienda Windows.

* Dado que la aplicación convertida tiene identidad del paquete, puedes llamar a más API de UWP, incluso desde la partición de plena confianza, que antes.

* Puedes agregar características de UWP al paquete de la aplicación a tu propio ritmo, como una interfaz de usuario XAML, actualizaciones activas de icono, tareas en segundo plano para UWP, servicios de la aplicación y mucho más. Todas las funciones disponibles para cualquier otra aplicación para UWP están disponible en la aplicación.

* Si eliges mover todas las funciones de la aplicación desde la partición de plena confianza de la aplicación a la partición de contenedor de aplicación, la aplicación podrá ejecutarse en cualquier dispositivo de Windows 10.

* Como aplicación para UWP, esta es capaz de hacer las mismas cosas que puede hacer como una aplicación de escritorio clásica. Interactúa con una vista virtualizada del registro y el sistema de archivos que no se puede distinguir del registro y el sistema de archivos reales.

* La aplicación puede participar en instalaciones de licencia integrada y actualización automática de la Tienda Windows. La actualización automática es un mecanismo muy confiable y eficaz, ya que solo se descargan las partes modificadas de los archivos.

## Preparar la aplicación de escritorio para la conversión a UWP
No tienes que hacer mucho para preparar la aplicación para el proceso de conversión. Recuerda que la Tienda Windows administra licencias y actualizaciones automáticas, por lo tanto, puedes quitar dichas características de tu código base. Si ninguna de estas situaciones se aplica a la aplicación, debes solucionar este problema antes de la conversión.

+ 
            __La aplicación usa una versión de .NET anterior a 4.6.1__. Solo .NET 4.6.1 es compatible. Debes redestinar tu aplicación a .NET 4.6.1 antes de la conversión. 

+ 
            __La aplicación siempre se ejecuta con privilegios de seguridad elevados__. La aplicación necesita trabajar mientras se ejecuta como el usuario interactivo. Los usuarios que instalen la aplicación desde la Tienda Windows pueden no ser administradores del sistema, por lo tanto, requerir que la aplicación se ejecute con privilegios elevados significa que no se ejecutará correctamente para los usuarios estándar.

+ 
            __La aplicación requiere un controlador de modo kernel o un servicio de Windows__. El puente es adecuado para una aplicación, pero no admite un controlador en modo kernel o un servicio de Windows que necesite ejecutarse en una cuenta de sistema. En lugar de un servicio de Windows usa una [tarea en segundo plano](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task).

+ 
            __Los módulos de la aplicación se cargan en proceso a los procesos que no están en el paquete AppX__. Esto no está permitido, lo que significa que las extensiones en proceso, como [extensiones de shell](https://msdn.microsoft.com/library/windows/desktop/dd758089.aspx), no se admiten. Pero si tienes dos aplicaciones en el mismo paquete, puedes realizar la comunicación entre procesos entre ellas.

+ 
            __La aplicación usa un identificador del modelo de usuario de la aplicación (AUMID) personalizado__. Si el proceso llama a [SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422.aspx) para establecer su propio AUMID, entonces solo puede usar el AUMID generado para este por el paquete de entorno del modelo de aplicaciones/AppX. No se pueden definir AUMID personalizados.

+ 
            __La aplicación modifica el subárbol del registro HKEY_LOCAL_MACHINE (HKLM)__. Cualquier intento de tu aplicación para crear una clave HKLM o abrir una para realizar modificaciones producirá un error de acceso denegado. Recuerda que tu aplicación tiene su propia vista virtualizada privada del registro, por lo que no se aplica la noción de un subárbol del registro de nivel de usuario y de equipo (que es lo que HKLM es). Necesitas encontrar otra manera de lograr aquello para lo que usaste HKLM, como escribir en HKEY_CURRENT_USER (HKCU) en su lugar.

+ 
            __La aplicación usa una subclave del Registro ddeexec como una forma de iniciar otra aplicación__. En su lugar, usa uno de los controladores de verbo DelegateExecute como lo configuran las distintas extensiones activables* en tu [manifiesto del paquete de la aplicación](https://msdn.microsoft.com/library/windows/apps/br211474.aspx).

+ 
            __La aplicación se escribe en la carpeta AppData con la intención de compartir datos con otra aplicación__. Después de la conversión, AppData se redirige al almacén de datos locales de la aplicación, que es un almacén privado para cada aplicación para UWP. Usa un medio diferente para compartir datos entre procesos. Para más información, consulta [Almacenar y recuperar la configuración y otros datos de aplicación](https://msdn.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data).

+ 
            __La aplicación se escribe en el directorio de instalación de la aplicación__. Por ejemplo, la aplicación se escribe en un archivo de registro que se coloca en el mismo directorio que el archivo .exe. Esto no se admite, por lo que tendrás que buscar otra ubicación, como el almacén de datos locales de la aplicación.

+ 
            __La instalación de la aplicación requiere la interacción del usuario__. El instalador de la aplicación debe ser capaz de ejecutarse en silencio, y se deben instalar todos sus requisitos previos que no estén activados de forma predeterminada en una imagen de sistema operativo limpia.

+ 
            __La aplicación usa el directorio de trabajo actual__. En tiempo de ejecución, la aplicación convertida no obtendrá el mismo directorio de trabajo que especificaste anteriormente en el escritorio. Acceso directo del vínculo. Debes cambiar tu CWD en tiempo de ejecución si tener el directorio correcto es importante para que tu aplicación funcione correctamente.

+ 
            __La aplicación requiere UIAccess__. Si la aplicación especifica `UIAccess=true` en el elemento `requestedExecutionLevel` del manifiesto de UAC, la conversión a UWP no es compatible actualmente. Para más información, consulta [UI Automation Security Overview (Introducción a la seguridad de la automatización de la interfaz de usuario](https://msdn.microsoft.com/library/ms742884.aspx).

+ 
            __La aplicación expone objetos COM o ensamblados GAC para que los usen otros procesos__. En la versión actual, la aplicación no puede exponer objetos COM ni ensamblados GAC para que los usen procesos originados por archivos ejecutables externos en el paquete AppX. Los procesos internos del paquete pueden registrar y usar objetos COM y ensamblados GAC del modo habitual, pero no serán visibles externamente. Esto significa que los escenarios de interoperabilidad como OLE no funcionarán si los invocan procesos externos. 

+ 
            __La aplicación está vinculando las bibliotecas en tiempo de ejecución de C de forma no compatible__. La biblioteca en tiempo de ejecución de Microsoft C/C++ ofrece rutinas de programación para el sistema operativo Microsoft Windows. Estas rutinas automatizan muchas tareas comunes de programación que no proporcionan los lenguajes C y C++. Si la aplicación usa la biblioteca en tiempo de ejecución de C o C++, debes asegurarte de que esté vinculada de manera admitida. 
    
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

+ 
            __La aplicación se instala y carga los ensamblados desde la carpeta de Windows en paralelo__. Por ejemplo, la aplicación usa las bibliotecas en tiempo de ejecución de C VC8 o VC9 y las vincula de forma dinámica desde la carpeta de Windows en paralelo, lo que significa que el código usa los archivos DLL comunes de una carpeta compartida. Esto no se admite. Debes vincularlas de forma estática mediante la vinculación a los archivos de biblioteca redistribuibles directamente en el código.


## En esta sección:

| Tema | Descripción |
|-------|-------------|
| [Vista previa de Desktop App Converter (Project Centennial)](desktop-to-uwp-run-desktop-app-converter.md) | Muestra cómo ejecutar Desktop App Converter. Probablemente no tendrás que hacer demasiado, si es que tienes que hacer algo, para preparar la aplicación para el proceso de conversión. |
| [Convertir manualmente la aplicación de escritorio de Windows en una aplicación para la Plataforma universal de Windows (UWP)](desktop-to-uwp-manual-conversion.md) | Aprende a crear un paquete de la aplicación y un manifiesto manualmente. |
| [Extensiones para aplicaciones de escritorio convertidas](desktop-to-uwp-extensions.md) | Una aplicación de escritorio convertida puede mejorarse con una amplia gama de API para la Plataforma universal de Windows (UWP). Sin embargo, además de las API normales disponibles para todas las aplicaciones para UWP, hay algunas extensiones y API disponibles únicamente para las aplicaciones de escritorio convertidas. En este artículo se describen dichas extensiones y cómo usarlas. |
| [Implementar y depurar la aplicación para UWP convertida](desktop-to-uwp-deploy-and-debug.md) | Contiene información para ayudarte a implementar correctamente y a depurar la aplicación después de convertirla. Además, si tienes curiosidad sobre algunos de los aspectos internos de las extensiones de conversión de escritorio, este tema es para ti. |
| [Puente de la aplicación de escritorio a ejemplos de código de UWP](https://github.com/Microsoft/DesktopBridgeToUWP-Samples) | Ejemplos de código en GitHub que muestran las características de las aplicaciones convertidas. |



<!--HONumber=Jul16_HO1-->


