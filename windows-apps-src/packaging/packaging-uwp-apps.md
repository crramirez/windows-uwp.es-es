---
author: laurenhughes
ms.assetid: 96361CAF-C347-4671-9721-8208CE118CA4
title: Empaquetado de aplicaciones para UWP
description: "Para vender tu aplicación para la Plataforma universal de Windows (UWP) o distribuirla a otros usuarios, necesitas crear un paquete .appxupload para esta."
ms.author: lahugh
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: ca57f50f4827ba5de7a140f1353ba864c5e2fb6c
ms.lasthandoff: 02/07/2017

---
# <a name="packaging-uwp-apps"></a>Empaquetado de aplicaciones para UWP

\[ Actualizado para las aplicaciones para UWP en Windows 10. Para leer artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Para vender tu aplicación para la Plataforma universal de Windows (UWP) o distribuirla a otros usuarios, necesitas crear un paquete .appxupload para esta. Al crear el paquete .appxupload, se generará otro paquete .appx destinado a pruebas y a la instalación de prueba. Puedes distribuir la aplicación directamente mediante la instalación de prueba del paquete .appx a un dispositivo. Este artículo describe el proceso de configuración, creación y prueba de un paquete de la aplicación para UWP. Para obtener más información sobre la instalación de prueba, consulta [Sideload Apps in Windows 10 (Transferir localmente aplicaciones en Windows 10)](https://technet.microsoft.com/library/mt269549.aspx).

Para Windows 10, se genera un paquete (.appxupload) que se puede cargar en la Tienda Windows. La aplicación estará disponible para instalarse y ejecutarse en cualquier dispositivo de Windows 10. Estos son los pasos para crear un paquete de la aplicación.

1.  [Antes de empaquetar la aplicación](#before-packaging-your-app). Sigue estos pasos para asegurarte de que la aplicación está lista para empaquetarse para el envío a la tienda.
2.  [Configura un paquete de la aplicación](#configure-an-app-package). Usa el diseñador de manifiestos para configurar el paquete. Por ejemplo, agrega imágenes de icono y elige las orientaciones que admitirá la aplicación.
3.  [Crea un paquete de la aplicación](#create-an-app-package). Usa el asistente de Microsoft Visual Studio para crear un paquete de la aplicación y, después, certifica el paquete con el Kit para la certificación de aplicaciones en Windows.
4.  [Transfiere localmente el paquete de la aplicación](#sideload-your-app-package). Después de realizar la instalación de prueba de la aplicación en un dispositivo, puedes probar si funciona correctamente.

Después de completar los pasos anteriores, estás listo para vender tu aplicación en la tienda. Si tienes una aplicación de línea de negocio (LOB), que no tienes previsto vender porque es solo para los usuarios internos, puedes realizar una instalación de prueba de dicha aplicación para instalarla en cualquier dispositivo de Windows 10.

## <a name="before-packaging-your-app"></a>Antes de empaquetar la aplicación

1.  Prueba la aplicación. Antes de empaquetar la aplicación para su envío a la tienda, asegúrate de que funciona según lo previsto en todas las familias de dispositivos que planeas admitir. Estas familias de dispositivos pueden incluir los equipos de escritorio, móviles, Surface Hub, XBOX, dispositivos de IoT, etc.
2.  Optimiza la aplicación. Puedes usar las herramientas de generación de perfiles y depuración de Visual Studio para optimizar el rendimiento de tu aplicación para UWP. Por ejemplo, la herramienta Línea de tiempo para la capacidad de respuesta de la interfaz de usuario, la herramienta Uso de memoria, la herramienta Uso de CPU, etc. Para obtener más información acerca de estas herramientas, consulta [Run profiling tools without debugging (Ejecutar las herramientas de diagnóstico sin depuración)](https://msdn.microsoft.com/library/dn957936.aspx).
3.  Comprueba la compatibilidad de .NET nativo (para aplicaciones de VB y C#). UWP incluye un nuevo compilador nativo que mejorará el rendimiento en tiempo de ejecución de la aplicación. Una vez hecho este cambio, se recomienda encarecidamente probar la aplicación en este entorno de compilación. De manera predeterminada, la configuración de compilación **Release** habilita la cadena de herramientas de .NET nativa, por lo que es importante probar la aplicación con esta configuración **Release** y comprobar que se comporta según lo esperado. Algunos problemas de depuración comunes que pueden producirse con .NET nativo se explican con más detalle [aquí](http://blogs.msdn.com/b/visualstudioalm/archive/2015/07/29/debugging-net-native-windows-universal-apps.aspx).

## <a name="configure-an-app-package"></a>Configurar un paquete de la aplicación

El archivo de manifiesto de la aplicación (package.appxmanifest.xml) presenta las propiedades y la configuración necesarias para crear el paquete de la aplicación. Por ejemplo, las propiedades del archivo de manifiesto describen la imagen que se usará como icono de la aplicación y las orientaciones que la aplicación admite cuando el usuario gira el dispositivo.

Visual Studio tiene un diseñador de manifiestos que facilita el proceso de actualización del archivo de manifiesto sin modificar el lenguaje XML sin formato del archivo.

Visual Studio puede asociar el paquete a la Tienda. Al hacerlo, se actualizan automáticamente algunos de los campos en la pestaña Empaquetado del diseñador de manifiestos.

**Configurar un paquete con el diseñador de manifiestos**

1.  En el **Explorador de soluciones**, expande el nodo del proyecto de la aplicación para UWP.
2.  Haz doble clic en el archivo **package.appxmanifest**. Si el archivo de manifiesto ya está abierto en la vista de código XML, Visual Studio te pedirá que lo cierres.
3.  Ahora puedes decidir cómo configurar la aplicación. Cada pestaña contiene información que se puede configurar sobre la aplicación y vínculos para obtener más información si es necesario.<br/>
    ![Diseñador de manifiestos en Visual Studio](images/packaging-screen1.jpg)

    Comprueba que tienes todas las imágenes necesarias para una aplicación para UWP en la pestaña **Activos visuales**.

    En la pestaña **Empaquetado** puedes escribir los datos de publicación. Aquí es donde puedes elegir el certificado que se usará para firmar la aplicación. Todas las aplicaciones para UWP deben estar firmadas con un certificado. Para realizar la instalación de prueba de un paquete de la aplicación, debes confiar en el paquete. El certificado debe instalarse en ese dispositivo para confiar en el paquete. Para obtener más información acerca de la instalación de prueba, consulta [Habilitar el dispositivo para el desarrollo](https://msdn.microsoft.com/library/windows/apps/Dn706236).

4.  Guarda el archivo después de realizar las modificaciones necesarias para la aplicación.

## <a name="create-an-app-package"></a>Crear un paquete de la aplicación

Para distribuir una aplicación a través de la Tienda, debes crear un paquete .appxupload. Puedes hacerlo mediante el asistente **Crear paquetes de aplicaciones**. Sigue estos pasos para crear un paquete apropiado para el envío a la tienda con Microsoft Visual Studio 2015.

**Para crear el paquete de la aplicación**

1.  En el **Explorador de soluciones**, abre la solución del proyecto de la aplicación para UWP.
2.  Haz clic con el botón derecho en el proyecto y elige **Tienda**->**Crear paquetes de aplicaciones**. Si esta opción está deshabilitada o no aparece, comprueba que el proyecto sea un proyecto para UWP.<br/>
    ![Menú contextual con navegación para crear paquetes de aplicaciones](images/packaging-screen2.jpg)

    A continuación, se mostrará el asistente **Crear paquetes de aplicaciones**.

3.  Selecciona "Sí" en el primer cuadro de diálogo que te pregunte si quieres compilar paquetes para cargarlos en la Tienda Windows y, a continuación, haz clic en Siguiente.<br/>
    ![Visualización de la ventana de diálogo Crear los paquetes](images/packaging-screen3.jpg)

    Si eliges No aquí, Visual Studio no generará el paquete .appxupload que necesitas para el envío a la Tienda. Si solo quieres realizar la instalación de prueba de la aplicación para ejecutarla en dispositivos internos, puedes seleccionar esta opción. Para obtener más información acerca de la instalación de prueba, consulta [Habilitar el dispositivo para el desarrollo](https://msdn.microsoft.com/library/windows/apps/Dn706236).

4.  Inicia sesión con tu cuenta de desarrollador en el Centro de desarrollo de Windows. (Si aún no tienes una cuenta de desarrollador, el asistente te ayudará a crear una).
5.  Selecciona el nombre de aplicación para el paquete o reserva uno nuevo si todavía no lo has hecho en el portal del Centro de desarrollo de Windows.<br/>
    ![Visualización de la ventana Crear paquetes de aplicaciones con la selección del nombre de aplicación](images/packaging-screen4.jpg)
6.  Asegúrate de seleccionar las tres configuraciones de arquitectura (x86, x64 y ARM) en el cuadro de diálogo **Seleccionar y configurar paquetes**. De este modo, la aplicación se puede implementar en la más amplia variedad de dispositivos. En el cuadro de lista **Crear lote de aplicaciones**, selecciona **Siempre**. Esta acción simplifica enormemente el proceso de envío a la tienda, ya que solo tienes que cargar un archivo (.appxupload). El lote único contendrá todos los paquetes necesarios para la implementación en dispositivos con cada una de las arquitecturas de procesador.<br/>
    ![Visualización de la ventana de Crear paquetes de aplicaciones con la configuración del paquete](images/packaging-screen5.jpg)
7.  Es una buena idea incluir archivos de símbolos PDB completos para lograr la mejor experiencia de [análisis de bloqueo](http://blogs.windows.com/buildingapps/2015/07/13/crash-analysis-in-the-unified-dev-center/) del Centro de desarrollo de Windows. Para obtener más información sobre la depuración con símbolos, visita [Debugging with Symbols](https://msdn.microsoft.com/library/windows/desktop/Ee416588) (Depuración con símbolos).
8.  Ahora puedes configurar los detalles para crear el paquete. Cuando estés listo para publicar la aplicación, deberás cargar los paquetes desde la ubicación de salida.
9.  Haz clic en **Crear** para generar el paquete .appxupload.
10. Ahora verás este cuadro de diálogo.<br/>
    ![Visualización de la ventana de creación del paquete completada con las opciones de validación](images/packaging-screen6.jpg)

    Valida la aplicación antes de enviarla a la Tienda para la certificación en un equipo local o remoto. (Solo se pueden validar las compilaciones de versiones del paquete de la aplicación, no las compilaciones de depuración).

11. Para realizar la validación localmente, deja la opción **Máquina local** seleccionada y haz clic en **Iniciar el Kit para la certificación de aplicaciones en Windows**. Para obtener más información sobre la prueba de la aplicación con el Kit para la certificación de hardware en Windows, consulta [Kit para la certificación de aplicaciones en Windows](https://msdn.microsoft.com/library/windows/apps/Mt186449).

    El Kit para la certificación de aplicaciones en Windows realiza pruebas y muestra los resultados. Consulta [Pruebas del Kit para la certificación de aplicaciones en Windows](https://msdn.microsoft.com/library/windows/apps/mt186450).

    Si tienes un dispositivo remoto con Windows 10 que desees usar para las pruebas, deberás instalar el Kit para la certificación de aplicaciones en Windows de forma manual. La siguiente sección te guiará a través de estos pasos. Después de realizar estos pasos, puedes seleccionar **Máquina remota** y hacer clic en **Iniciar el Kit para la certificación de aplicaciones en Windows** para conectarte al dispositivo remoto y ejecutar las pruebas de validación.

12. Cuando la herramienta WACK haya terminado y la aplicación esté aprobada, podrá cargarla en la tienda. Asegúrate de cargar el archivo correcto. Puedes encontrarlo en la carpeta raíz de la solución \\[AppName]\\AppPackages y termina con la extensión de archivo .appxupload. El nombre tendrá el formato \[AppName]\_\[AppVersion\]\_x86\_x64\_arm\_bundle.appxupload.

**Validar el paquete de la aplicación en un dispositivo remoto de Windows 10**

1.  Para habilitar un dispositivo de Windows 10 para el desarrollo, sigue las instrucciones de [Habilitar el dispositivo para el desarrollo](https://msdn.microsoft.com/library/windows/apps/Dn706236) .
    **Importante**  No puedes validar el paquete de la aplicación en un dispositivo remoto de ARM para Windows 10.
2.  Descarga e instala las herramientas remotas para Visual Studio. Estas herramientas se usan para ejecutar el Kit para la certificación de aplicaciones en Windows de forma remota. Para obtener más información acerca de estas herramientas, incluida la ubicación para descargarlas, visita [Ejecutar aplicaciones de la Tienda Windows en un equipo remoto](https://msdn.microsoft.com/library/hh441469.aspx#BKMK_Starting_the_Remote_Debugger_Monitor).
3.  Descarga el [Kit para la certificación de aplicaciones en Windows](http://go.microsoft.com/fwlink/p/?LinkID=309666) necesario e instálalo en el dispositivo remoto de Windows 10.
4.  En la página **Creación del paquete completada** del asistente, elige el botón de opción **Máquina remota** y, a continuación, el botón de puntos suspensivos que se encuentra junto al botón **Probar conexión**.
    **Nota**  El botón de opción **Máquina remota** está disponible únicamente si seleccionaste al menos una configuración de solución que admita la validación. Para obtener más información sobre la prueba de la aplicación con el WACK, consulta [Kit para la certificación de aplicaciones en Windows](https://msdn.microsoft.com/library/windows/apps/Mt186449).
5.  Especifica un tipo de dispositivo de la subred, o proporciona el nombre del servidor de nombres de dominio (DNS) o la dirección IP de un dispositivo que esté fuera de la subred.
6.  En la lista **Modo de autenticación**, elige **Ninguno** si el dispositivo no requiere que inicies sesión con las credenciales de Windows.
7.  Elige el botón **Seleccionar** y el botón **Iniciar el Kit para la certificación de aplicaciones en Windows**. Si se ejecutan herramientas remotas en este dispositivo, Visual Studio se conecta a este y realiza las pruebas de validación. Consulta [Pruebas del Kit para la certificación de aplicaciones en Windows](https://msdn.microsoft.com/library/windows/apps/mt186450).

## <a name="sideload-your-app-package"></a>Realizar la instalación de prueba del paquete de la aplicación

Con los paquetes de la aplicación para UWP, no puedes instalar simplemente una aplicación en el dispositivo como sucede con las aplicaciones de escritorio. Por lo general, estas aplicaciones se descargan de la Tienda y así es como se instalan en el dispositivo. No obstante, puedes realizar la instalación de prueba de aplicaciones en el dispositivo sin enviarlas a la Tienda. Esto te permite instalarlas y probarlas usando el paquete de la aplicación (.appx) que has creado. Si tienes una aplicación que no quieres vender en la Tienda, como una aplicación de línea de negocio (LOB), puedes realizar la instalación de prueba de dicha aplicación para que otros usuarios de la empresa pueden usarla.

La siguiente lista proporciona los requisitos para la instalación de prueba de la aplicación.

-   Debes [habilitar el dispositivo para el desarrollo](https://msdn.microsoft.com/library/windows/apps/Dn706236).
-   Para realizar la instalación de prueba de la aplicación en un dispositivo de Windows 10 Mobile, debes usar la herramienta [WinAppDeployCmd.exe](install-universal-windows-apps-with-the-winappdeploycmd-tool.md) .

**Realizar la instalación de prueba de una aplicación en un equipo de escritorio, un portátil o una tableta**

1.  Copia las carpetas de la versión que deseas instalar en el dispositivo de destino.

    Si ya has creado un lote de aplicaciones, tendrás una carpeta basada en el número de versión y una carpeta \_test. Aquí tienes un ejemplo de estas dos carpetas (donde la versión que se va a instalar es 1.0.2):

    -   C:\\Projects\\MyApp\\MyApp\\AppPackages\\MyApp\_1.0.2.0
    -   C:\\Projects\\MyApp\\MyApp\\AppPackages\\MyApp\_1.0.2.0\_Test

    Si no tienes un lote de aplicaciones, puedes copiar la carpeta de la arquitectura correcta y la carpeta de prueba correspondiente. Tienes como ejemplo las dos carpetas siguientes.

    -   C:\\Projects\\MyApp\\MyApp\\AppPackages\\MyApp\_1.0.2.0\_x64
    -   C:\\Projects\\MyApp\\MyApp\\AppPackages\\MyApp\_1.0.2.0\_x64\_Test
2.  En el dispositivo de destino, abre la carpeta de prueba. Por ejemplo, C:\\Projects\\MyApp\\MyApp\\AppPackages\\MyApp\_1.0.2.0\_Test
3.  Haz clic con el botón secundario en el archivo **Add-AppDevPackage.ps1**, elige **Ejecutar con PowerShell** y sigue las instrucciones.<br/>
    ![Visualización del Explorador de archivos con navegación hasta el script de PowerShell](images/packaging-screen7.jpg)

    Después de instalar el paquete de la aplicación, verás un mensaje en la ventana de PowerShell que indica que la aplicación se ha instalado correctamente.

    **Nota**  Para abrir el menú contextual en una tableta, toca la pantalla donde quieras hacer clic con el botón derecho, sostén hasta que se muestre un círculo completo y después levanta el dedo. El menú contextual aparece después de levantar el dedo.
4.  Haz clic en el botón Inicio y después escribe el nombre de la aplicación para iniciarla.
