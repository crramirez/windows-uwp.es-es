---
ms.assetid: 96361CAF-C347-4671-9721-8208CE118CA4
title: Empaquetado de aplicaciones para UWP
description: Para distribuir o vender tu aplicación de Plataforma universal de Windows (UWP), necesitas crear un paquete de aplicación para ella.
ms.date: 01/02/2019
ms.topic: article
keywords: windows 10, uwp
f1_keywords:
- vs.packagewizard
- vs.storeassociationwizard
ms.localizationpriority: medium
ms.openlocfilehash: f2e89490a76c9174c1e938466bf1fbcc9cc13455
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599140"
---
# <a name="package-a-uwp-app-with-visual-studio"></a>Empaquetar una aplicación para UWP con Visual Studio

Para vender tu aplicación Plataforma universal de Windows (UWP) o distribuirla a otros usuarios, necesitas empaquetarla. Si no quieres distribuir tu aplicación a través de Microsoft Store, puedes transferir localmente el paquete de la aplicación directamente a un dispositivo o distribuirla a través de [Web Install](installing-UWP-apps-web.md). Este artículo describe el proceso de configuración, creación y prueba de un paquete de la aplicación para UWP con Visual Studio. Para obtener más información sobre cómo administrar e implementar aplicaciones de línea de negocio (LOB), consulta [Administración de aplicaciones de empresa](https://docs.microsoft.com/windows/client-management/mdm/enterprise-app-management).

En Windows 10, puede enviar un paquete de aplicación, paquete de aplicación o un archivo de carga del paquete de aplicación completa a [centro de partners](https://partner.microsoft.com/dashboard). De estas opciones, enviar un archivo de carga del paquete de aplicación proporcionará la mejor experiencia.

## <a name="types-of-app-packages"></a>Tipos de paquetes de aplicación

- **Paquete de aplicación (.appx o .msix)**  
    Un archivo que contiene tu aplicación en un formato que pueda instalarse de prueba en un dispositivo. Cualquier archivo de paquete de aplicación único creado por Visual Studio es **no** diseñado para ser enviado al centro de partners y se debe usar para la instalación de prueba y solo con fines de prueba. Si desea enviar la aplicación al centro de partners, use el archivo de carga del paquete de aplicación.  

- **Paquete de aplicación (.appxbundle o .msixbundle)**  
    Una recopilación de aplicación es un tipo de paquete que puede contener varios paquetes de aplicación, cada uno de ellos integrado para admitir una arquitectura de dispositivo específica. Por ejemplo, una recopilación de aplicación puede contener tres paquetes de aplicación independientes para las configuraciones x86, x64 y ARM. Las recopilaciones de aplicaciones deberían generarse siempre que sea posible, ya que permiten que tu aplicación esté disponible en la gama de dispositivos más amplia posible.  

- **Archivo de carga de paquete de aplicación (.appxupload o .msixupload)**  
    Un único archivo que puede contener varios paquetes de aplicación o una recopilación de aplicación para admitir distintas arquitecturas de procesador. El archivo de carga del paquete de aplicación también contiene un archivo de símbolos para [analizar el rendimiento de la aplicación](https://docs.microsoft.com/windows/uwp/publish/analytics) después de la aplicación se ha publicado en la Microsoft Store. Este archivo se crearán automáticamente para usted si se está empaquetando la aplicación con Visual Studio con la intención de enviar al centro de partners para la publicación.

A continuación describimos los pasos para preparar y crear un paquete de la aplicación:

1.  [Antes de empaquetar la aplicación](#before-packaging-your-app). Siga estos pasos para asegurarse de que la aplicación está lista para empaquetarse para el envío del centro de partners.
2.  [Configurar un paquete de la aplicación](#configure-an-app-package). Usa el diseñador de manifiestos de Visual Studio para configurar el paquete. Por ejemplo, agrega imágenes de icono y elige las orientaciones que admitirá la aplicación.
3.  [Crear un archivo de carga del paquete de aplicación](#create-an-app-package-upload-file). Usa el asistente del paquete de aplicación de Visual Studio para crear un paquete de la aplicación y, después, certifica el paquete con el Kit para la certificación de aplicaciones en Windows.
4.  [Transfiere localmente el paquete de la aplicación](#sideload-your-app-package). Después de realizar la instalación de prueba de la aplicación en un dispositivo, puedes probar si funciona de la forma esperada.

Después de completar los pasos anteriores, estás listo para distribuir tu aplicación. Si tiene una aplicación de línea de negocio (LOB) que no va a vender porque es solo para usuarios internos, puede transferirla localmente esta aplicación para instalarla en cualquier dispositivo Windows 10.

## <a name="before-packaging-your-app"></a>Antes de empaquetar la aplicación

1.  **Pruebe la aplicación.** Antes de empaquetar la aplicación para enviarla de centro de partners, asegúrese de que funciona según lo previsto en todas las familias de dispositivos que va a admitir. Estas familias de dispositivos pueden incluir los equipos de escritorio, móviles, Surface Hub, Xbox, dispositivos de IoT, etc. Para obtener más información sobre cómo implementar y probar la aplicación con Visual Studio, consulte [implementación y depuración de aplicaciones para UWP](../debug-test-perf/deploying-and-debugging-uwp-apps.md).
2.  **Optimizar la aplicación.** Puedes usar las herramientas de generación de perfiles y depuración de Visual Studio para optimizar el rendimiento de tu aplicación para UWP. Por ejemplo, la herramienta Línea de tiempo para la capacidad de respuesta de la interfaz de usuario, la herramienta Uso de memoria, la herramienta Uso de CPU, etc. Para obtener más información acerca de estas herramientas de línea de comandos, consulta el tema [Recorrido por las funciones de perfiles](https://docs.microsoft.com/visualstudio/profiling/profiling-feature-tour):
3.  **Comprobar la compatibilidad de .NET Native (para VB y C# aplicaciones).** En la Plataforma universal de Windows, hay un compilador nativo que mejorará el rendimiento en tiempo de ejecución de la aplicación. Una vez hecho este cambio, debe probar la aplicación en este entorno de compilación. De manera predeterminada, la configuración de compilación **Release** habilita la cadena de herramientas de .NET nativa, por lo que es importante probar la aplicación con esta configuración **Release** y comprobar que se comporta según lo esperado. Algunos problemas de depuración comunes que pueden producirse con .NET Native se explican con más detalle en [Depuración de aplicaciones universales de .NET Native](https://blogs.msdn.com/b/visualstudioalm/archive/2015/07/29/debugging-net-native-windows-universal-apps.aspx).

## <a name="configure-an-app-package"></a>Configurar un paquete de la aplicación

El archivo de manifiesto de la aplicación (Package.appxmanifest.xml) es un archivo XML que contiene las propiedades y la configuración necesarias para crear el paquete de la aplicación. Por ejemplo, las propiedades del archivo de manifiesto de aplicación describen la imagen que se usará como icono de la aplicación y las orientaciones que la aplicación admite cuando el usuario gira el dispositivo.

El diseñador de manifiestos de Visual Studio le permite actualizar el archivo de manifiesto sin modificar el lenguaje XML sin formato del archivo.

**Configurar un paquete con el Diseñador de manifiestos**

1.  En el **Explorador de soluciones**, expande el nodo del proyecto de la aplicación para UWP.
2.  Haz doble clic en el archivo **package.appxmanifest**. Si el archivo de manifiesto ya está abierto en la vista de código XML, Visual Studio te pedirá que lo cierres.
3.  Ahora puedes decidir cómo configurar la aplicación. Cada pestaña contiene información que se puede configurar sobre la aplicación y vínculos para obtener más información si es necesario.  
    ![Diseñador de manifiestos en Visual Studio](images/packaging-screen1.jpg)

    Comprueba que tienes todas las imágenes necesarias para una aplicación para UWP en la pestaña **Activos visuales**.

    En la pestaña **Empaquetado** puedes escribir los datos de publicación. Aquí es donde puedes elegir el certificado que se usará para firmar la aplicación. Todas las aplicaciones para UWP deben estar firmadas con un certificado.

    >[!IMPORTANT]
    >Si publicas tu aplicación en Microsoft Store, la aplicación se firmará con un certificado de confianza para ti. Esto permite al usuario instalar y ejecutar la aplicación sin necesidad de instalar el certificado de firma de la aplicación asociado.

    Si no vas a publicar la aplicación o simplemente quieres instalar un paquete de aplicación de prueba, primero debes indicar que el paquete es de confianza. Para indicar que el paquete es de confianza, el certificado debe instalarse en el dispositivo del usuario. Para obtener más información acerca de la instalación de prueba, consulta [Habilitar el dispositivo para el desarrollo](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development).

4.  Guarda el archivo **Package.appxmanifest** después de realizar las modificaciones necesarias para la aplicación.

Si va a distribuir la aplicación a través de la Microsoft Store, Visual Studio puede asociar su paquete con el Store. Para ello, haga clic en el nombre del proyecto en el Explorador de soluciones y elija **Store**->**asociar aplicación con el Store**. También puede hacer esto el **crear paquetes de aplicaciones** asistente, que se describe en la sección siguiente. Cuando asocias tu aplicación, se actualizan automáticamente algunos de los campos en la pestaña Empaquetado del diseñador de manifiestos.

## <a name="create-an-app-package-upload-file"></a>Crear un archivo de carga del paquete de aplicación

Para distribuir una aplicación a través de Microsoft Store debe crear un paquete de aplicación (.appx o .msix), paquete de aplicación (.appxbundle o .msixbundle) o un archivo de carga de paquete de aplicación (.appxupload o .msixupload) y [enviar la aplicación empaquetada al centro de partners](https://docs.microsoft.com/windows/uwp/publish/app-submissions). Aunque es posible enviar un paquete de aplicación o paquete de aplicaciones al centro de partners por sí solo, se recomienda que envíe un archivo de carga del paquete de aplicación. Puede crear un archivo de carga del paquete de aplicación mediante el **crear paquetes de aplicaciones** asistente en Visual Studio, o bien puede crear uno manualmente desde los paquetes de aplicación existentes o grupos de aplicaciones.

>[!NOTE]
> Si desea crear un paquete de aplicación (.appx o .msix) o un lote de aplicaciones (.appxbundle o .msixbundle) manualmente, consulte [crear un paquete de aplicación con la herramienta MakeAppx.exe](https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool).

### <a name="to-create-your-app-package-upload-file-using-visual-studio"></a>Para crear el archivo de carga del paquete de aplicación mediante Visual Studio

1.  En el **Explorador de soluciones**, abre la solución del proyecto de la aplicación para UWP.
2.  Haz clic con el botón derecho en el proyecto y elige **Tienda**->**Crear paquetes de aplicaciones**. Si esta opción está deshabilitada o no aparece, comprueba que el proyecto sea un proyecto de universal de Windows.  
    ![Menú contextual con navegación para crear paquetes de aplicaciones](images/packaging-screen2.jpg)

    A continuación, se mostrará el asistente **Crear paquetes de aplicaciones**.

3.  Seleccione **quiero crear paquetes para cargarlos en la Microsoft Store con un nuevo nombre de aplicación** en el primer cuadro de diálogo y, a continuación, haga clic en **siguiente**.  
    ![Crear ventana de cuadro de diálogo de paquetes que se muestra](images/packaging-screen3.jpg)

    Si ya ha asociado el proyecto con una aplicación en el Store, también tiene una opción para crear paquetes para la aplicación Store asociada. Si elige **quiero crear paquetes para la instalación de prueba**, Visual Studio no generará la aplicación carga (.msixupload o .appxupload) archivo de paquete para envíos de centro de partners. Si solo quieres realizar la instalación de prueba de la aplicación para ejecutarla en dispositivos internos o para pruebas, puedes seleccionar esta opción. Para obtener más información acerca de la instalación de prueba, consulta [Habilitar el dispositivo para el desarrollo](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development).
4.  En la siguiente página, inicie sesión con su cuenta de desarrollador al centro de partners. Si aún no tienes una cuenta de desarrollador, el asistente te ayudará a crear una.
    ![Crear ventana de paquetes de aplicaciones con la selección del nombre de aplicación que se muestra](images/packaging-screen4.jpg)
5.  Seleccione el nombre de la aplicación para el paquete de la lista de aplicaciones registradas actualmente en su cuenta o reserve uno nuevo si no lo ha reservado uno en el centro de partners.  
6.  Asegúrate de seleccionar las tres configuraciones de arquitectura (x86, x64 y ARM) en el diálogo **Seleccionar y configurar paquetes** para garantizar que tu aplicación se puede implementar en la gama de dispositivos más amplia. En el cuadro de lista **Crear lote de aplicaciones**, selecciona **Siempre**. Se prefiere un lote de aplicaciones (.appxbundle o .msixbundle) a través de un archivo de paquete de aplicación único porque contiene una colección de paquetes de aplicaciones configurado para cada tipo de arquitectura de procesador. Cuando se elige generar el paquete de aplicación, el lote de aplicaciones se incluirán en la aplicación final cargar (.appxupload o .msixupload) archivo de paquete junto con información de depuración y análisis de bloqueo. Si no estás seguro de qué arquitecturas elegir o quieres obtener más información sobre qué arquitecturas se usan en varios dispositivos, consulta [Arquitecturas de paquete de aplicación](https://docs.microsoft.com/windows/uwp/packaging/device-architecture).  
    ![Crear ventana de paquetes de aplicaciones con la configuración de paquete que se muestra](images/packaging-screen5.jpg)
7.  Incluir archivos de símbolos PDB completos para [analizar el rendimiento de la aplicación](https://docs.microsoft.com/windows/uwp/publish/analytics) desde el centro de partners, una vez publicada su aplicación. Configura detalles adicionales, como la numeración de la versión o la ubicación de salida del paquete.
9.  Haz clic en **Crear** para generar el paquete de aplicación. Si ha seleccionado uno de los **quiero crear paquetes para cargarlos en la Microsoft Store** opciones en el paso 3 y están creando un paquete para el envío del centro de partners, el asistente creará un archivo de carga de paquete (.appxupload o .msixupload). Si seleccionó **quiero crear paquetes para la instalación de prueba** en el paso 3, el asistente creará un paquete de aplicación única o un grupo de aplicaciones según las selecciones realizadas en el paso 6.
10. Cuando la aplicación se ha empaquetado correctamente, verá este cuadro de diálogo y se puede recuperar el archivo de carga del paquete de aplicación de la ubicación de salida especificado. En este momento, puede [validar el paquete de aplicación en el equipo local o en un equipo remoto](#validate-your-app-package).
    ![Creación del paquete completado de ventana con las opciones de validación que se muestran](images/packaging-screen6.jpg)

### <a name="to-create-your-app-package-upload-file-manually"></a>Para crear manualmente el archivo de carga del paquete de aplicación

1. Coloque los archivos siguientes en una carpeta:
    - Uno o varios paquetes de aplicación (.msix o .appx) o un grupo de aplicaciones (.msixbundle o .appxbundle).
    - Un archivo .appxsym. Se trata de un archivo .pdb comprimido que contiene símbolos públicos de la aplicación se usa para [crash analytics](../publish/health-report.md) en el centro de partners. Puede omitir este archivo, pero si lo hace, no hay información de bloqueo analítico o de depuración estarán disponible para la aplicación.
2. Comprima la carpeta.
3. Cambie el nombre de extensión de la carpeta comprimida de .zip a .msixupload o appxupload.

### <a name="validate-your-app-package"></a>Validar el paquete de aplicación

Valide la aplicación antes de enviarla al centro de partners para la certificación en un equipo local o remoto. Solo se pueden validar las compilaciones de versiones del paquete de la aplicación, no las compilaciones de depuración. Para obtener más información sobre el envío de la aplicación al centro de partners, consulte [envíos de aplicaciones](https://docs.microsoft.com/windows/uwp/publish/app-submissions).

**Para validar el paquete de la aplicación localmente**

1. En el último **creación de paquete completada** página de la **crear paquetes de aplicaciones** asistente, deje el **máquina Local** opción seleccionada y haga clic en **iniciar Kit de certificación de aplicaciones de Windows**. Para obtener más información sobre la prueba de la aplicación con el Kit para la certificación de hardware en Windows, consulta [Kit para la certificación de aplicaciones en Windows](https://msdn.microsoft.com/library/windows/apps/Mt186449).

    El Kit para la certificación de aplicaciones en Windows realiza diversas pruebas y devuelve los resultados. Consulta [Pruebas del Kit para la certificación de aplicaciones en Windows](https://msdn.microsoft.com/library/windows/apps/mt186450) para obtener información más especifica.

    Si tiene un dispositivo remoto de Windows 10 que desea usar para las pruebas, deberá instalar el Kit de certificación de aplicaciones de Windows manualmente en ese dispositivo. La siguiente sección te guiará a través de estos pasos. Después de realizar estos pasos, puedes seleccionar **Máquina remota** y hacer clic en **Iniciar el Kit para la certificación de aplicaciones en Windows** para conectarte al dispositivo remoto y ejecutar las pruebas de validación.

2. Cuando termine WACK y haya pasado la certificación de su aplicación, está listo para enviar la aplicación al centro de partners. Asegúrate de cargar el archivo correcto. La ubicación predeterminada del archivo puede encontrarse en la carpeta raíz de la solución `\[AppName]\AppPackages` y termina con la extensión de archivo .appxupload o .msixupload. Será el nombre del formulario `[AppName]_[AppVersion]_x86_x64_arm_bundle.appxupload` o `[AppName]_[AppVersion]_x86_x64_arm_bundle.msixupload` si ha optado por un grupo de aplicaciones con toda la arquitectura del paquete seleccionada.

**Para validar el paquete de aplicación en un dispositivo remoto de Windows 10**

1.  Habilitar el dispositivo Windows 10 para el desarrollo siguiendo la [habilitar el dispositivo para el desarrollo](https://msdn.microsoft.com/library/windows/apps/Dn706236) instrucciones.
    >[!IMPORTANT]
    > No se puede validar el paquete de aplicación en un dispositivo remoto de ARM para Windows 10.
2.  Descarga e instala las herramientas remotas para Visual Studio. Estas herramientas se usan para ejecutar el Kit para la certificación de aplicaciones en Windows de forma remota. Para obtener más información acerca de estas herramientas, incluida la ubicación para descargarlas, visita [Ejecutar aplicaciones para UWP en un equipo remoto](https://msdn.microsoft.com/library/hh441469.aspx#BKMK_Starting_the_Remote_Debugger_Monitor).
3.  Descargue los [Windows App Certification Kit](https://go.microsoft.com/fwlink/p/?LinkID=309666) e instálelo en su dispositivo Windows 10 remoto.
4.  En la página **Creación del paquete completada** del asistente, elige el botón de opción **Máquina remota** y, a continuación, el botón de puntos suspensivos que se encuentra junto al botón **Probar conexión**.
    >[!NOTE]
    > El **máquina remota** botón de opción solo está disponible si ha seleccionado al menos una configuración de solución que admita la validación. Para obtener más información sobre la prueba de la aplicación con el WACK, consulta [Kit para la certificación de aplicaciones en Windows](https://msdn.microsoft.com/library/windows/apps/Mt186449).
5.  Especifica un tipo de dispositivo de la subred, o proporciona el nombre del servidor de nombres de dominio (DNS) o la dirección IP de un dispositivo que esté fuera de la subred.
6.  En la lista **Modo de autenticación**, elige **Ninguno** si el dispositivo no requiere que inicies sesión con las credenciales de Windows.
7.  Elige el botón **Seleccionar** y el botón **Iniciar el Kit para la certificación de aplicaciones en Windows**. Si se ejecutan herramientas remotas en este dispositivo, Visual Studio se conecta a al dispositivo y realiza las pruebas de validación. Consulta [Pruebas del Kit para la certificación de aplicaciones en Windows](https://msdn.microsoft.com/library/windows/apps/mt186450).

## <a name="sideload-your-app-package"></a>Realizar la instalación de prueba del paquete de la aplicación

Con paquetes de aplicaciones para UWP, no se instalan las aplicaciones en un dispositivo tal como están con aplicaciones de escritorio. Por lo general, descargas aplicaciones para UWP desde Microsoft Store, que también instala la aplicación en el dispositivo para ti. Las aplicaciones se pueden instalar sin publicarlas en la Store (instalación de prueba). Esto le permite instalar y probar aplicaciones mediante el paquete de aplicación de archivos que haya creado. Si tienes una aplicación que no quieres vender en la Tienda, como una aplicación de línea de negocio (LOB), puedes realizar la instalación de prueba de dicha aplicación para que otros usuarios de la empresa pueden usarla.

Antes de poder transferir localmente la aplicación en un dispositivo de destino, debe [habilitar el dispositivo para el desarrollo](../get-started/enable-your-device-for-development.md).

Para transferir localmente la aplicación en un dispositivo Windows 10 Mobile, use el [WinAppDeployCmd.exe](install-universal-windows-apps-with-the-winappdeploycmd-tool.md) herramienta. Para equipos de escritorio, portátiles y tabletas, siga las instrucciones siguientes.

### <a name="sideload-your-app-package-on-windows-10-anniversary-update-or-later"></a>Transferir localmente la aplicación del paquete de actualización de aniversario de Windows 10 o posterior

Introducidos en la Actualización de aniversario de Windows 10, los paquetes de la aplicación se pueden instalar simplemente haciendo doble clic en el archivo de paquete de la aplicación. Para ello, vaya a su paquete de aplicación o el archivo de paquete de aplicación y haga doble clic en él. El instalador de aplicaciones se inicia y proporciona la información de la aplicación básica, así como un botón de instalación, barra de progreso de instalación y los mensajes de error pertinentes.

![Pantalla del instalador de aplicaciones para instalar una aplicación de ejemplo llamada Contoso](images/appinstaller-screen.png)

> [!NOTE]
> El instalador de aplicación da por hecho que el dispositivo confía en la aplicación. Si estás realizando la instalación de prueba de una aplicación de desarrollador o empresarial, tendrás que instalar el certificado de firma en el almacén de Entidades de certificación raíz de personas de confianza o editores de confianza en el dispositivo. Si no estás seguro de cómo hacerlo, consulta [Instalar certificados de prueba](https://docs.microsoft.com/windows-hardware/drivers/install/installing-test-certificates).

### <a name="sideload-your-app-package-on-previous-versions-of-windows"></a>Transferir localmente la aplicación de paquetes en versiones anteriores de Windows

1.  Copia las carpetas de la versión de la aplicación que deseas instalar en el dispositivo de destino.

    Si ya has creado un lote de aplicaciones, tendrás una carpeta basada en el número de versión y una carpeta `*_Test`. Aquí tienes un ejemplo de estas dos carpetas (donde la versión que se va a instalar es 1.0.2.0):

    -   `C:\Projects\MyApp\MyApp\AppPackages\MyApp_1.0.2.0`
    -   `C:\Projects\MyApp\MyApp\AppPackages\MyApp_1.0.2.0_Test`

    Si no tienes un lote de aplicaciones, copia la carpeta de la arquitectura correcta y su carpeta `*_Test` correspondiente. Estas dos carpetas son un ejemplo de un paquete de la aplicación con la arquitectura x64 y su carpeta `*_Test`:

    -   `C:\Projects\MyApp\MyApp\AppPackages\MyApp_1.0.2.0_x64`
    -   `C:\Projects\MyApp\MyApp\AppPackages\MyApp_1.0.2.0_x64_Test`

2.  En el dispositivo de destino, abre la carpeta `*_Test`.
3.  Haz clic con el botón derecho en el archivo **Add-AppDevPackage.ps1**. Elige **Ejecutar con PowerShell** y sigue las instrucciones.  
    ![Navegado al script de PowerShell que se muestra el Explorador de archivos](images/packaging-screen7.jpg)

    Cuando se ha instalado el paquete de aplicación, la ventana de PowerShell muestra este mensaje: **La aplicación se instaló correctamente.**
    >[!TIP]
    > Para abrir el menú contextual en una tableta, toque la pantalla donde desea que haga, mantenga hasta que aparezca un círculo completo y levante el dedo. El menú contextual se abre después de levantar el dedo.

4.  Haz clic en el botón Inicio para buscar la aplicación por nombre y luego, iníciala.
