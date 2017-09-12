---
author: laurenhughes
ms.assetid: 96361CAF-C347-4671-9721-8208CE118CA4
title: Empaquetado de aplicaciones para UWP
description: "Para distribuir o vender tu aplicación de Plataforma universal de Windows (UWP), necesitas crear un paquete de aplicación para ella."
ms.author: lahugh
ms.date: 08/09/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: c6aa08c8c2e62bfed388000944de5520cbe58501
ms.sourcegitcommit: 63c815f8c6665872987b5410cabf324f2b7e3c7c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/10/2017
---
# <a name="package-a-uwp-app-with-visual-studio"></a>Empaquetar una aplicación para UWP con Visual Studio

\[ Actualizado para aplicaciones para UWP en Windows10. Para leer artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Para vender tu aplicación para la Plataforma universal de Windows (UWP) o distribuirla a otros usuarios, necesitas crear un paquete de aplicación para UWP para esta. Si no quieres distribuir tu aplicación a través de la Tienda, puedes transferir localmente el paquete de la aplicación directamente a un dispositivo. Este artículo describe el proceso de configuración, creación y prueba de un paquete de la aplicación para UWP con Visual Studio. Para obtener más información acerca de la transferencia local de las aplicaciones empresariales y de línea de negocio (LOB), consulta [Realizar la instalación de prueba de aplicaciones de línea de negocio en Windows 10](https://docs.microsoft.com/windows/application-management/sideload-apps-in-windows-10).

En Windows 10, puedes cargar un paquete de la aplicación (.appx), un lote de aplicaciones (.appxbundle) o un archivo de carga completa (.appxupload). El archivo .appxupload es una carpeta que contiene tanto el lote de aplicaciones como el paquete de la aplicación, así como otra información de depuración importante. Es muy recomendable enviar un archivo .appxupload. Una vez que el paquete de la aplicación se ha enviado a la Tienda, la aplicación está disponible para instalarse y ejecutarse en cualquier dispositivo Windows 10. 

A continuación describimos los pasos de preparar y crear un paquete de la aplicación.

1.  [Antes de empaquetar la aplicación](#before-packaging-your-app). Sigue estos pasos para asegurarte de que la aplicación está lista para empaquetarse para el envío a la Tienda.
2.  [Configura un paquete de la aplicación](#configure-an-app-package). Usa el diseñador de manifiestos para configurar el paquete. Por ejemplo, agrega imágenes de icono y elige las orientaciones que admitirá la aplicación.
3.  [Crea un paquete de la aplicación](#create-an-app-package). Usa el asistente para paquetes de la aplicación de Visual Studio para crear un paquete de la aplicación. A continuación, certifica el paquete con el Kit para la certificación de aplicaciones en Windows.
4.  [Transfiere localmente el paquete de la aplicación](#sideload-your-app-package). Después de realizar la instalación de prueba de la aplicación en un dispositivo, puedes probar si funciona correctamente.

Después de completar los pasos anteriores, estás listo para distribuir tu aplicación en la Tienda. Si tienes una aplicación de línea de negocio (LOB) que no tienes previsto vender porque es solo para los usuarios internos, puedes realizar una instalación de prueba de dicha aplicación para instalarla en cualquier dispositivo Windows 10.

## <a name="before-packaging-your-app"></a>Antes de empaquetar la aplicación

1.  **Prueba la aplicación.** Antes de empaquetar la aplicación para su envío a la tienda, asegúrate de que funciona según lo previsto en todas las familias de dispositivos que planeas admitir. Estas familias de dispositivos pueden incluir los equipos de escritorio, móviles, Surface Hub, Xbox, dispositivos de IoT, etc.
2.  **Optimiza la aplicación.** Puedes usar las herramientas de generación de perfiles y depuración de Visual Studio para optimizar el rendimiento de tu aplicación para UWP. Por ejemplo, la herramienta Línea de tiempo para la capacidad de respuesta de la interfaz de usuario, la herramienta Uso de memoria, la herramienta Uso de CPU, etc. Para obtener más información acerca de estas herramientas, consulta [Run profiling tools without debugging (Ejecutar las herramientas de diagnóstico sin depuración)](https://msdn.microsoft.com/library/dn957936.aspx).
3.  **Comprueba la compatibilidad de .NET nativo (para aplicaciones de VB y C#).** En la Plataforma universal de Windows, hay un compilador nativo que mejorará el rendimiento en tiempo de ejecución de la aplicación. Una vez hecho este cambio, se recomienda encarecidamente probar la aplicación en este entorno de compilación. De manera predeterminada, la configuración de compilación **Release** habilita la cadena de herramientas de .NET nativa, por lo que es importante probar la aplicación con esta configuración **Release** y comprobar que se comporta según lo esperado. Algunos problemas de depuración comunes que pueden producirse con .NET Native se explican con más detalle en [Depuración de aplicaciones universales de .NET Native](http://blogs.msdn.com/b/visualstudioalm/archive/2015/07/29/debugging-net-native-windows-universal-apps.aspx).

## <a name="configure-an-app-package"></a>Configurar un paquete de la aplicación

El archivo de manifiesto de la aplicación (Package.appxmanifest.xml) es un archivo XML que contiene las propiedades y la configuración necesarias para crear el paquete de la aplicación. Por ejemplo, las propiedades del archivo de manifiesto describen la imagen que se usará como icono de la aplicación y las orientaciones que la aplicación admite cuando el usuario gira el dispositivo.

El diseñador de manifiestos de Visual Studio le permite actualizar el archivo de manifiesto sin modificar el lenguaje XML sin formato del archivo.

**Configurar un paquete con el diseñador de manifiestos**

1.  En el **Explorador de soluciones**, expande el nodo del proyecto de la aplicación para UWP.
2.  Haz doble clic en el archivo **package.appxmanifest**. Si el archivo de manifiesto ya está abierto en la vista de código XML, Visual Studio te pedirá que lo cierres.
3.  Ahora puedes decidir cómo configurar la aplicación. Cada pestaña contiene información que se puede configurar sobre la aplicación y vínculos para obtener más información si es necesario.<br/>
    ![Diseñador de manifiestos en Visual Studio](images/packaging-screen1.jpg)

    Comprueba que tienes todas las imágenes necesarias para una aplicación para UWP en la pestaña **Activos visuales**.

    En la pestaña **Empaquetado** puedes escribir los datos de publicación. Aquí es donde puedes elegir el certificado que se usará para firmar la aplicación. Todas las aplicaciones para UWP deben estar firmadas con un certificado. Para realizar la instalación de prueba de un paquete de la aplicación, debes confiar en el paquete. El certificado debe instalarse en ese dispositivo para confiar en el paquete. Para obtener más información acerca de la instalación de prueba, consulta [Habilitar el dispositivo para el desarrollo](https://msdn.microsoft.com/library/windows/apps/Dn706236).

4.  Guarda el archivo después de realizar las modificaciones necesarias para la aplicación.

Visual Studio puede asociar el paquete a la Tienda. Al hacerlo, se actualizan automáticamente algunos de los campos en la pestaña Empaquetado del diseñador de manifiestos.

## <a name="create-an-app-package"></a>Crear un paquete de la aplicación

Para distribuir una aplicación a través de la Tienda, debes crear un paquete de la aplicación (.appx), un lote de aplicaciones (.appxbundle) o un paquete de carga (.appxupload). Puedes hacerlo mediante el asistente **Crear paquetes de aplicaciones**. Sigue estos pasos para crear un paquete apropiado para el envío a la Tienda con Visual Studio.

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
6.  Asegúrate de seleccionar las tres configuraciones de arquitectura (x86, x64 y ARM) en el cuadro de diálogo **Seleccionar y configurar paquetes**. De este modo, la aplicación se puede implementar en la más amplia variedad de dispositivos. En el cuadro de lista **Crear lote de aplicaciones**, selecciona **Siempre**. Esta acción simplifica enormemente el proceso de envío a la Tienda, ya que solo tienes que cargar un archivo (.appxupload). El lote único contendrá todos los paquetes necesarios para la implementación en dispositivos con cada una de las arquitecturas de procesador, así como importante información de depuración y analítica de bloqueos. Para obtener más información acerca de arquitecturas de paquete usadas por varios dispositivos, consulta [Arquitecturas de paquetes de aplicaciones](https://docs.microsoft.com/windows/uwp/packaging/device-architecture).<br/>
    ![Visualización de la ventana de Crear paquetes de aplicaciones con la configuración del paquete](images/packaging-screen5.jpg)
7.  Es muy recomendable incluir archivos de símbolos PDB completos para lograr la mejor experiencia de [análisis de bloqueo](http://blogs.windows.com/buildingapps/2015/07/13/crash-analysis-in-the-unified-dev-center/) del Centro de desarrollo de Windows. Para obtener más información sobre la depuración con símbolos, visita [Debugging with Symbols](https://msdn.microsoft.com/library/windows/desktop/Ee416588) (Depuración con símbolos).
8.  Ahora puedes configurar los detalles para crear el paquete. Cuando estés listo para publicar la aplicación, deberás cargar los paquetes desde la ubicación de salida.
9.  Haz clic en **Crear** para generar el paquete .appxupload.
10. Ahora verás este cuadro de diálogo.<br/>
    ![Visualización de la ventana de creación del paquete completada con las opciones de validación](images/packaging-screen6.jpg)

    Valida la aplicación antes de enviarla a la Tienda para la certificación en un equipo local o remoto. Solo se pueden validar las compilaciones de versiones del paquete de la aplicación, no las compilaciones de depuración.

11. Para realizar la validación localmente, deja la opción **Máquina local** seleccionada y haz clic en **Iniciar el Kit para la certificación de aplicaciones en Windows**. Para obtener más información sobre la prueba de la aplicación con el Kit para la certificación de hardware en Windows, consulta [Kit para la certificación de aplicaciones en Windows](https://msdn.microsoft.com/library/windows/apps/Mt186449).

    El Kit para la certificación de aplicaciones en Windows realiza diversas pruebas y devuelve los resultados. Consulta [Pruebas del Kit para la certificación de aplicaciones en Windows](https://msdn.microsoft.com/library/windows/apps/mt186450) para obtener información más especifica.

    Si tienes un dispositivo remoto con Windows 10 que desees usar para las pruebas, deberás instalar el Kit para la certificación de aplicaciones en Windows de forma manual. La siguiente sección te guiará a través de estos pasos. Después de realizar estos pasos, puedes seleccionar **Máquina remota** y hacer clic en **Iniciar el Kit para la certificación de aplicaciones en Windows** para conectarte al dispositivo remoto y ejecutar las pruebas de validación.

12. Cuando la herramienta WACK haya terminado y la aplicación esté aprobada, podrás enviar la aplicación a la Tienda. Asegúrate de cargar el archivo correcto. Puedes encontrarlo en la carpeta raíz de la solución \\[AppName]\\AppPackages y termina con la extensión de archivo .appxupload (o la extensión .appx/.appxbundle, si es lo que estás usando). El nombre tendrá el formato \[AppName]\_\[AppVersion\]\_x86\_x64\_arm\_bundle.appxupload.

**Validar el paquete de la aplicación en un dispositivo remoto de Windows 10**

1.  Para habilitar un dispositivo de Windows 10 para el desarrollo, sigue las instrucciones de [Habilitar el dispositivo para el desarrollo](https://msdn.microsoft.com/library/windows/apps/Dn706236) .
    **Importante**  No puedes validar el paquete de la aplicación en un dispositivo remoto de ARM para Windows10.
2.  Descarga e instala las herramientas remotas para Visual Studio. Estas herramientas se usan para ejecutar el Kit para la certificación de aplicaciones en Windows de forma remota. Para obtener más información acerca de estas herramientas, incluida la ubicación para descargarlas, visita [Ejecutar aplicaciones de la Tienda Windows en un equipo remoto](https://msdn.microsoft.com/library/hh441469.aspx#BKMK_Starting_the_Remote_Debugger_Monitor).
3.  Descarga el [Kit para la certificación de aplicaciones en Windows](http://go.microsoft.com/fwlink/p/?LinkID=309666) necesario e instálalo en el dispositivo remoto de Windows 10.
4.  En la página **Creación del paquete completada** del asistente, elige el botón de opción **Máquina remota** y, a continuación, el botón de puntos suspensivos que se encuentra junto al botón **Probar conexión**.
    **Nota**  El botón de opción **Máquina remota** está disponible únicamente si seleccionaste al menos una configuración de solución que admita la validación. Para obtener más información sobre la prueba de la aplicación con el WACK, consulta [Kit para la certificación de aplicaciones en Windows](https://msdn.microsoft.com/library/windows/apps/Mt186449).
5.  Especifica un tipo de dispositivo de la subred, o proporciona el nombre del servidor de nombres de dominio (DNS) o la dirección IP de un dispositivo que esté fuera de la subred.
6.  En la lista **Modo de autenticación**, elige **Ninguno** si el dispositivo no requiere que inicies sesión con las credenciales de Windows.
7.  Elige el botón **Seleccionar** y el botón **Iniciar el Kit para la certificación de aplicaciones en Windows**. Si se ejecutan herramientas remotas en este dispositivo, Visual Studio se conecta a este y realiza las pruebas de validación. Consulta [Pruebas del Kit para la certificación de aplicaciones en Windows](https://msdn.microsoft.com/library/windows/apps/mt186450).

## <a name="sideload-your-app-package"></a>Realizar la instalación de prueba del paquete de la aplicación

Introducidos en la Actualización de aniversario de Windows 10, los paquetes de la aplicación se pueden instalar simplemente haciendo doble clic en el archivo de paquete de la aplicación. Para usarlo, simplemente navega hasta tu archivo de paquete de la aplicación (.appx) o lote de aplicaciones (.appxbundle) y haz doble clic en él. El instalador de aplicaciones se inicia y proporciona la información de la aplicación básica, así como un botón de instalación, barra de progreso de instalación y los mensajes de error pertinentes. 

![Pantalla del instalador de aplicaciones para instalar una aplicación de ejemplo llamada Contoso](images/appinstaller-screen.png)

> [!NOTE]
> El instalador de aplicación da por hecho que el dispositivo confía en la aplicación. Si estás realizando la instalación de prueba de una aplicación de desarrollador o empresarial, tendrás que instalar el certificado de firma en el almacén de Entidades de certificación raíz de confianza en el dispositivo. Si no estás seguro de cómo hacerlo, consulta [Instalar certificados de prueba](https://docs.microsoft.com/windows-hardware/drivers/install/installing-test-certificates).

### <a name="sideload-your-app-on-previous-versions-of-windows"></a>Transferir localmente la aplicación en versiones anteriores de Windows
Con los paquetes de aplicación para UWP, las aplicaciones no se instalan en un dispositivo ya que están con aplicaciones de escritorio. Por lo general, descargas aplicaciones para UWP de la Tienda, que también instala la aplicación en el dispositivo para ti. Las aplicaciones se pueden instalar sin enviarlas a la Tienda (transferencia local). Esto te permite instalar y probar las aplicaciones usando el paquete de la aplicación (.appx) que has creado. Si tienes una aplicación que no quieres vender en la Tienda, como una aplicación de línea de negocio (LOB), puedes realizar la instalación de prueba de dicha aplicación para que otros usuarios de la empresa pueden usarla.

La siguiente lista proporciona los requisitos para la instalación de prueba de la aplicación.

-   Debes [habilitar el dispositivo para el desarrollo](https://msdn.microsoft.com/library/windows/apps/Dn706236).
-   Para transferir localmente la aplicación a un dispositivo con Windows 10 Mobile, usa la herramienta [WinAppDeployCmd.exe](install-universal-windows-apps-with-the-winappdeploycmd-tool.md).

**Realizar la instalación de prueba de una aplicación en un equipo de escritorio, un portátil o una tableta**

1.  Copia las carpetas de la versión de la aplicación que deseas instalar en el dispositivo de destino.

    Si ya has creado un lote de aplicaciones, tendrás una carpeta basada en el número de versión y una carpeta `*\_Test`. Aquí tienes un ejemplo de estas dos carpetas (donde la versión que se va a instalar es 1.0.2.0):

    -   `C:\\Projects\\MyApp\\MyApp\\AppPackages\\MyApp\_1.0.2.0`
    -   `C:\\Projects\\MyApp\\MyApp\\AppPackages\\MyApp\_1.0.2.0\_Test`

    Si no tienes un lote de aplicaciones, copia la carpeta de la arquitectura correcta y su carpeta `*\_Test` correspondiente. Estas dos carpetas son un ejemplo de un paquete de la aplicación con la arquitectura x64 y su carpeta `*\_Test`:

    -   `C:\\Projects\\MyApp\\MyApp\\AppPackages\\MyApp\_1.0.2.0\_x64`
    -   `C:\\Projects\\MyApp\\MyApp\\AppPackages\\MyApp\_1.0.2.0\_x64\_Test`

2.  En el dispositivo de destino, abre la carpeta `*\_Test`.
3.  Haz clic con el botón derecho en el archivo **Add-AppDevPackage.ps1**. Elige **Ejecutar con PowerShell** y sigue las instrucciones.<br/>
    ![Visualización del Explorador de archivos con navegación hasta el script de PowerShell](images/packaging-screen7.jpg)

    Después de instalar el paquete de la aplicación, la ventana de PowerShell muestra este mensaje: **La aplicación se instaló correctamente**.

    **Sugerencia**: Para abrir el menú contextual en una tableta, toca la pantalla donde quieras hacer clic con el botón derecho, sostén hasta que se muestre un círculo completo y después levanta el dedo. El menú contextual se abre después de levantar el dedo.
4.  Haz clic en el botón Inicio para buscar la aplicación por nombre y luego, iníciala.
