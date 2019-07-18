---
ms.assetid: 96361CAF-C347-4671-9721-8208CE118CA4
title: Empaquetado de aplicaciones para UWP
description: Para distribuir o vender tu aplicación de Plataforma universal de Windows (UWP), necesitas crear un paquete de aplicación para ella.
ms.date: 07/17/2019
ms.topic: article
keywords: windows 10, uwp
f1_keywords:
- vs.packagewizard
- vs.storeassociationwizard
ms.localizationpriority: medium
ms.openlocfilehash: 1038913732c8b25a5baa3daf2c04176ff747a85f
ms.sourcegitcommit: 2062d06567ef087ad73507a03ecc726a7d848361
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/17/2019
ms.locfileid: "68303592"
---
# <a name="package-a-uwp-app-with-visual-studio"></a>Empaquetar una aplicación para UWP con Visual Studio

Para vender tu aplicación Plataforma universal de Windows (UWP) o distribuirla a otros usuarios, necesitas empaquetarla. Si no quieres distribuir tu aplicación a través de Microsoft Store, puedes transferir localmente el paquete de la aplicación directamente a un dispositivo o distribuirla a través de [Web Install](installing-UWP-apps-web.md). Este artículo describe el proceso de configuración, creación y prueba de un paquete de la aplicación para UWP con Visual Studio. Para obtener más información sobre cómo administrar e implementar aplicaciones de línea de negocio (LOB), consulta [Administración de aplicaciones de empresa](https://docs.microsoft.com/windows/client-management/mdm/enterprise-app-management).

En Windows 10, puede enviar un paquete de la aplicación, un lote de aplicaciones o un archivo completo de carga del paquete de la aplicación al [centro de Partners](https://partner.microsoft.com/dashboard). De estas opciones, el envío de un archivo de carga del paquete de la aplicación proporcionará la mejor experiencia.

## <a name="types-of-app-packages"></a>Tipos de paquetes de aplicación

- **Paquete de la aplicación (. appx o. msix)**  
    Un archivo que contiene tu aplicación en un formato que pueda instalarse de prueba en un dispositivo. Cualquier archivo de paquete de aplicación único creado por Visual Studio **no** está diseñado para enviarse al centro de Partners y debe usarse únicamente para la instalación de prueba y con fines de prueba. Si desea enviar la aplicación al centro de Partners, use el archivo de carga del paquete de aplicaciones.  

- **Lote de aplicaciones (. appxbundle o. msixbundle)**  
    Una recopilación de aplicación es un tipo de paquete que puede contener varios paquetes de aplicación, cada uno de ellos integrado para admitir una arquitectura de dispositivo específica. Por ejemplo, una recopilación de aplicación puede contener tres paquetes de aplicación independientes para las configuraciones x86, x64 y ARM. Las recopilaciones de aplicaciones deberían generarse siempre que sea posible, ya que permiten que tu aplicación esté disponible en la gama de dispositivos más amplia posible.  

- **Archivo de carga del paquete de aplicaciones (. appxupload o. msixupload)**  
    Un único archivo que puede contener varios paquetes de aplicación o una recopilación de aplicación para admitir distintas arquitecturas de procesador. El archivo de carga del paquete de aplicaciones también contiene un archivo de símbolos para [analizar el rendimiento](https://docs.microsoft.com/windows/uwp/publish/analytics) de la aplicación después de que la aplicación se haya publicado en el Microsoft Store. Este archivo se creará automáticamente si va a empaquetar la aplicación con Visual Studio con la intención de enviarla al centro de partners para su publicación.

A continuación describimos los pasos para preparar y crear un paquete de la aplicación:

1.  [Antes de empaquetar la aplicación](#before-packaging-your-app). Siga estos pasos para asegurarse de que la aplicación está lista para empaquetarse para el envío del centro de Partners.
2.  [Configurar un paquete de la aplicación](#configure-an-app-package). Usa el diseñador de manifiestos de Visual Studio para configurar el paquete. Por ejemplo, agrega imágenes de icono y elige las orientaciones que admitirá la aplicación.
3.  [Crear un archivo de carga del paquete de aplicación](#create-an-app-package-upload-file). Usa el asistente del paquete de aplicación de Visual Studio para crear un paquete de la aplicación y, después, certifica el paquete con el Kit para la certificación de aplicaciones en Windows.
4.  [Transfiere localmente el paquete de la aplicación](#sideload-your-app-package). Después de realizar la instalación de prueba de la aplicación en un dispositivo, puedes probar si funciona de la forma esperada.

Después de completar los pasos anteriores, estás listo para distribuir tu aplicación. Si tiene una aplicación de línea de negocio (LOB) que no tiene previsto vender porque es solo para usuarios internos, puede transferir localmente esta aplicación para instalarla en cualquier dispositivo de Windows 10.

## <a name="before-packaging-your-app"></a>Antes de empaquetar la aplicación

1.  **Pruebe la aplicación.** Antes de empaquetar la aplicación para el envío del centro de Partners, asegúrese de que funciona según lo previsto en todas las familias de dispositivos que tiene previsto admitir. Estas familias de dispositivos pueden incluir los equipos de escritorio, móviles, Surface Hub, Xbox, dispositivos de IoT, etc. Para obtener más información sobre la implementación y prueba de la aplicación con Visual Studio, consulte [implementación y depuración de aplicaciones para UWP](../debug-test-perf/deploying-and-debugging-uwp-apps.md).
2.  **Optimice su aplicación.** Puedes usar las herramientas de generación de perfiles y depuración de Visual Studio para optimizar el rendimiento de tu aplicación para UWP. Por ejemplo, la herramienta Línea de tiempo para la capacidad de respuesta de la interfaz de usuario, la herramienta Uso de memoria, la herramienta Uso de CPU, etc. Para obtener más información acerca de estas herramientas de línea de comandos, consulta el tema [Recorrido por las funciones de perfiles](https://docs.microsoft.com/visualstudio/profiling/profiling-feature-tour):
3.  **Compruebe la compatibilidad de .NET Native (para C# VB y aplicaciones).** En la Plataforma universal de Windows, hay un compilador nativo que mejorará el rendimiento en tiempo de ejecución de la aplicación. Una vez hecho este cambio, debe probar la aplicación en este entorno de compilación. De manera predeterminada, la configuración de compilación **Release** habilita la cadena de herramientas de .NET nativa, por lo que es importante probar la aplicación con esta configuración **Release** y comprobar que se comporta según lo esperado. Algunos problemas de depuración comunes que pueden producirse con .NET Native se explican con más detalle en [Depuración de aplicaciones universales de .NET Native](https://devblogs.microsoft.com/devops/debugging-net-native-windows-universal-apps/).

## <a name="configure-an-app-package"></a>Configurar un paquete de la aplicación

El archivo de manifiesto de la aplicación (Package.appxmanifest.xml) es un archivo XML que contiene las propiedades y la configuración necesarias para crear el paquete de la aplicación. Por ejemplo, las propiedades del archivo de manifiesto de aplicación describen la imagen que se usará como icono de la aplicación y las orientaciones que la aplicación admite cuando el usuario gira el dispositivo.

El diseñador de manifiestos de Visual Studio le permite actualizar el archivo de manifiesto sin modificar el lenguaje XML sin formato del archivo.

### <a name="configure-a-package-with-the-manifest-designer"></a>Configurar un paquete con el diseñador de manifiestos

1.  En el **Explorador de soluciones**, expande el nodo del proyecto de la aplicación para UWP.
2.  Haz doble clic en el archivo **package.appxmanifest**. Si el archivo de manifiesto ya está abierto en la vista de código XML, Visual Studio te pedirá que lo cierres.
3.  Ahora puedes decidir cómo configurar la aplicación. Cada pestaña contiene información que se puede configurar sobre la aplicación y vínculos para obtener más información si es necesario.  
    ![Diseñador de manifiestos en Visual Studio](images/packaging-screen1.jpg)

    Comprueba que tienes todas las imágenes necesarias para una aplicación para UWP en la pestaña **Activos visuales**.

    En la pestaña **Empaquetado** puedes escribir los datos de publicación. Aquí es donde puedes elegir el certificado que se usará para firmar la aplicación. Todas las aplicaciones para UWP deben estar firmadas con un certificado.

    > [!NOTE]
    > A partir de Visual Studio 2019, ya no se genera un certificado temporal en los proyectos de UWP. Para crear o exportar certificados, use los cmdlets de PowerShell que se describen en [este artículo](create-certificate-package-signing.md).

    > [!IMPORTANT]
    > Si publicas tu aplicación en Microsoft Store, la aplicación se firmará con un certificado de confianza para ti. Esto permite al usuario instalar y ejecutar la aplicación sin necesidad de instalar el certificado de firma de la aplicación asociado.

    Si no vas a publicar la aplicación o simplemente quieres instalar un paquete de aplicación de prueba, primero debes indicar que el paquete es de confianza. Para indicar que el paquete es de confianza, el certificado debe instalarse en el dispositivo del usuario. Para obtener más información acerca de la instalación de prueba, consulta [Habilitar el dispositivo para el desarrollo](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development).

4.  Guarda el archivo **Package.appxmanifest** después de realizar las modificaciones necesarias para la aplicación.

Si va a distribuir la aplicación a través del Microsoft Store, Visual Studio puede asociar el paquete a la tienda. Para ello, haga clic con el botón derecho en el nombre del proyecto en explorador de soluciones y elija **tienda**->**asociar aplicación con la tienda**. También puede hacerlo en el Asistente para **crear paquetes de aplicaciones** , que se describe en la sección siguiente. Cuando asocias tu aplicación, se actualizan automáticamente algunos de los campos en la pestaña Empaquetado del diseñador de manifiestos.

## <a name="create-an-app-package-upload-file"></a>Crear un archivo de carga del paquete de aplicación

Para distribuir una aplicación a través de Microsoft Store debe crear un paquete de la aplicación (. appx o. msix), un lote de aplicaciones (. appxbundle o. msixbundle) o un archivo de carga del paquete de aplicaciones (. appxupload o. msixupload) y [enviar la aplicación empaquetada al centro de Partners](https://docs.microsoft.com/windows/uwp/publish/app-submissions). Aunque es posible enviar un paquete de aplicación o un lote de aplicaciones solo al centro de Partners, se recomienda que envíe un archivo de carga del paquete de la aplicación. Puede crear un archivo de carga de paquete de aplicaciones mediante el Asistente para **crear paquetes de aplicaciones** de Visual Studio, o bien puede crear uno manualmente a partir de paquetes de aplicaciones o conjuntos de aplicaciones existentes.

>[!NOTE]
> Si quieres crear un paquete de la aplicación (. appx o. msix) o un lote de aplicaciones (. appxbundle o. msixbundle) manualmente, consulta [crear un paquete de la aplicación con la herramienta MakeAppx. exe](https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool).

### <a name="create-your-app-package-upload-file-using-visual-studio"></a>Crear el archivo de carga del paquete de aplicaciones con Visual Studio

1.  En el **Explorador de soluciones**, abre la solución del proyecto de la aplicación para UWP.
2.  Haz clic con el botón derecho en el proyecto y elige **Tienda**->**Crear paquetes de aplicaciones**. Si esta opción está deshabilitada o no aparece, comprueba que el proyecto sea un proyecto de universal de Windows.  
    ![Menú contextual con navegación para crear paquetes de aplicaciones](images/packaging-screen2.jpg)

    A continuación, se mostrará el asistente **Crear paquetes de aplicaciones**.

3.  Seleccione **deseo crear paquetes para cargarlos en el Microsoft Store con un nuevo nombre de aplicación** en el primer cuadro de diálogo y, a continuación, haga clic en **siguiente**.  
    ![Cuadro de diálogo crear paquetes mostrados](images/packaging-screen3.jpg)

    Si ya ha asociado el proyecto con una aplicación en la tienda, también tiene la opción de crear paquetes para la aplicación de la tienda asociada. Si elige **crear paquetes para la instalación de prueba**, Visual Studio no generará el archivo de carga del paquete de aplicaciones (. msixupload o. appxupload) para los envíos del centro de Partners. Si solo quieres realizar la instalación de prueba de la aplicación para ejecutarla en dispositivos internos o para pruebas, puedes seleccionar esta opción. Para obtener más información acerca de la instalación de prueba, consulta [Habilitar el dispositivo para el desarrollo](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development).
4.  En la página siguiente, inicie sesión con su cuenta de Desarrollador en el centro de Partners. Si aún no tienes una cuenta de desarrollador, el asistente te ayudará a crear una.
    ![Ventana crear paquetes de aplicaciones con la selección de nombre de aplicación mostrada](images/packaging-screen4.jpg)
5.  Seleccione el nombre de la aplicación para el paquete en la lista de aplicaciones registradas actualmente en su cuenta o Reserve uno nuevo si aún no ha reservado uno en el centro de Partners.  
6.  Asegúrate de seleccionar las tres configuraciones de arquitectura (x86, x64 y ARM) en el diálogo **Seleccionar y configurar paquetes** para garantizar que tu aplicación se puede implementar en la gama de dispositivos más amplia. En el cuadro de lista **Crear lote de aplicaciones**, selecciona **Siempre**. Se prefiere un lote de aplicaciones (. appxbundle o. msixbundle) a un solo archivo de paquete de aplicación, ya que contiene una colección de paquetes de aplicación configurada para cada tipo de arquitectura de procesador. Cuando eliges generar el lote de aplicaciones, el lote de aplicaciones se incluirá en el archivo final de carga del paquete de aplicaciones (. appxupload o. msixupload) junto con la información analítica de depuración y bloqueo. Si no estás seguro de qué arquitecturas elegir o quieres obtener más información sobre qué arquitecturas se usan en varios dispositivos, consulta [Arquitecturas de paquete de aplicación](https://docs.microsoft.com/windows/uwp/packaging/device-architecture).  
    ![Ventana crear paquetes de aplicaciones con la configuración de paquetes mostrada](images/packaging-screen5.jpg)
7.  Incluya archivos de símbolos PDB completos para [analizar el rendimiento](https://docs.microsoft.com/windows/uwp/publish/analytics) de la aplicación desde el centro de Partners una vez publicada la aplicación. Configura detalles adicionales, como la numeración de la versión o la ubicación de salida del paquete.
8.  Haz clic en **Crear** para generar el paquete de aplicación. Si ha seleccionado una de las opciones **deseo crear paquetes para cargar en las Microsoft Store en el** paso 3 y está creando un paquete para el envío del centro de Partners, el asistente creará un archivo de carga de paquetes (. appxupload o. msixupload). Si seleccionó la opción **deseo crear paquetes para** la instalación de prueba en el paso 3, el asistente creará un único paquete de aplicación o un grupo de aplicaciones en función de las selecciones del paso 6.
9. Cuando la aplicación se haya empaquetado correctamente, verá este cuadro de diálogo y podrá recuperar el archivo de carga del paquete de la aplicación desde la ubicación de salida especificada. En este momento, puede [validar el paquete de la aplicación en el equipo local o en un equipo remoto](#validate-your-app-package) y automatizar los envíos de [almacén](#automate-store-submissions).
    ![Ventana de creación de paquete completada con opciones de validación mostradas](images/packaging-screen6.jpg)

### <a name="create-your-app-package-upload-file-manually"></a>Crear el archivo de carga del paquete de la aplicación manualmente

1. Coloque los siguientes archivos en una carpeta:
    - Uno o más paquetes de aplicación (. msix o. appx) o un lote de aplicaciones (. msixbundle o. appxbundle).
    - Un archivo. appxsym. Se trata de un archivo. pdb comprimido que contiene los símbolos públicos de la aplicación que se usan para el [análisis](../publish/health-report.md) de bloqueos en el centro de Partners. Puede omitir este archivo, pero si lo hace, no habrá información de depuración o análisis de bloqueo disponible para la aplicación.
2. Comprimir la carpeta.
3. Cambie el nombre de la extensión de carpeta comprimida de. zip a. msixupload o. appxupload.

## <a name="validate-your-app-package"></a>Validar el paquete de la aplicación

Valide la aplicación antes de enviarla al centro de partners para su certificación en un equipo local o remoto. Solo se pueden validar las compilaciones de versiones del paquete de la aplicación, no las compilaciones de depuración. Para obtener más información sobre el envío de su aplicación al centro de Partners, consulte envíos de [aplicaciones](https://docs.microsoft.com/windows/uwp/publish/app-submissions).

### <a name="validate-your-app-package-locally"></a>Validar el paquete de la aplicación localmente

1. En la página última **creación de paquete completada** del Asistente para **crear paquetes de aplicaciones** , deje seleccionada la opción **equipo local** y haga clic en **iniciar el kit de certificación de aplicaciones de Windows**. Para obtener más información sobre la prueba de la aplicación con el Kit para la certificación de hardware en Windows, consulta [Kit para la certificación de aplicaciones en Windows](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit).

    El kit para la certificación de aplicaciones de Windows (WACK) realiza varias pruebas y devuelve los resultados. Consulta [Pruebas del Kit para la certificación de aplicaciones en Windows](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit-tests) para obtener información más especifica.

    Si tiene un dispositivo remoto de Windows 10 que desea usar para las pruebas, deberá instalar el kit de certificación de aplicaciones de Windows manualmente en ese dispositivo. La siguiente sección te guiará a través de estos pasos. Después de realizar estos pasos, puedes seleccionar **Máquina remota** y hacer clic en **Iniciar el Kit para la certificación de aplicaciones en Windows** para conectarte al dispositivo remoto y ejecutar las pruebas de validación.

2. Una vez que WACK haya finalizado y que la aplicación haya superado la certificación, está listo para enviar la aplicación al centro de Partners. Asegúrate de cargar el archivo correcto. La ubicación predeterminada del archivo puede encontrarse en la carpeta raíz de la solución `\[AppName]\AppPackages` y finalizará con la extensión de archivo. appxupload o. msixupload. El nombre tendrá el formato `[AppName]_[AppVersion]_x86_x64_arm_bundle.appxupload` o `[AppName]_[AppVersion]_x86_x64_arm_bundle.msixupload` si ha optado por una agrupación de aplicaciones con todas las arquitecturas de paquetes seleccionadas.

### <a name="validate-your-app-package-on-a-remote-windows10-device"></a>Validar el paquete de la aplicación en un dispositivo remoto de Windows 10

1. Habilite el dispositivo Windows 10 para el desarrollo siguiendo las instrucciones de [Habilitar el dispositivo para el desarrollo](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development) .
    >[!IMPORTANT]
    > No se puede validar el paquete de la aplicación en un dispositivo ARM remoto para Windows 10.
2. Descarga e instala las herramientas remotas para Visual Studio. Estas herramientas se usan para ejecutar el Kit para la certificación de aplicaciones en Windows de forma remota. Para obtener más información acerca de estas herramientas, incluida la ubicación para descargarlas, visita [Ejecutar aplicaciones para UWP en un equipo remoto](https://docs.microsoft.com/visualstudio/debugger/run-windows-store-apps-on-a-remote-machine?view=vs-2015).
3. Descargue el [Kit de certificación de aplicaciones de Windows](https://go.microsoft.com/fwlink/p/?LinkID=309666) necesario e instálelo en el dispositivo remoto de Windows 10.
4. En la página **Creación del paquete completada** del asistente, elige el botón de opción **Máquina remota** y, a continuación, el botón de puntos suspensivos que se encuentra junto al botón **Probar conexión**.
    >[!NOTE]
    > El botón de opción **equipo remoto** solo está disponible si seleccionó al menos una configuración de soluciones que admita la validación. Para obtener más información sobre la prueba de la aplicación con el WACK, consulta [Kit para la certificación de aplicaciones en Windows](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit).
5. Especifica un tipo de dispositivo de la subred, o proporciona el nombre del servidor de nombres de dominio (DNS) o la dirección IP de un dispositivo que esté fuera de la subred.
6. En la lista **Modo de autenticación**, elige **Ninguno** si el dispositivo no requiere que inicies sesión con las credenciales de Windows.
7. Elige el botón **Seleccionar** y el botón **Iniciar el Kit para la certificación de aplicaciones en Windows**. Si se ejecutan herramientas remotas en este dispositivo, Visual Studio se conecta a al dispositivo y realiza las pruebas de validación. Consulta [Pruebas del Kit para la certificación de aplicaciones en Windows](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit-tests).

## <a name="automate-store-submissions"></a>Automatización de los envíos de tiendas

A partir de Visual Studio 2019, puede enviar el archivo. appxupload generado al Microsoft Store directamente desde el IDE; para ello, seleccione la opción **enviar automáticamente al Microsoft Store después** de la validación del kit de certificación de aplicaciones de Windows al final de la [Asistente para crear paquetes de aplicaciones](#create-your-app-package-upload-file-using-visual-studio). Esta característica aprovecha Azure Active Directory para acceder a la información de la cuenta del centro de Partners necesaria para publicar la aplicación. Para usar esta característica, necesitará asociar Azure Active Directory con su cuenta del centro de Partners y recuperar varias credenciales necesarias para los envíos.

### <a name="associate-azure-active-directory-with-your-partner-center-account"></a>Asociar Azure Active Directory con su cuenta del centro de Partners

Antes de poder recuperar las credenciales necesarias para el envío automático de almacenes, primero debe seguir estos pasos en el panel del [centro de Partners](https://partner.microsoft.com/dashboard) si aún no lo ha hecho.

1. [Asocie su cuenta del centro de Partners con el Azure Active Directory de su organización](https://docs.microsoft.com/windows/uwp/publish/associate-azure-ad-with-partner-center). Si la organización ya usa Office 365 u otros servicios empresariales de Microsoft, ya tienes Azure AD. De lo contrario, puede crear un nuevo inquilino de Azure AD desde el centro de Partners sin cargo adicional.

2. [Agregue una aplicación Azure ad a su cuenta del centro de Partners](https://docs.microsoft.com/windows/uwp/publish/add-users-groups-and-azure-ad-applications#add-azure-ad-applications-to-your-partner-center-account). Esta aplicación Azure AD representa la aplicación o el servicio que usará para obtener acceso a los envíos de la cuenta del centro de desarrollo. Debe asignar esta aplicación al rol de **Administrador** . Si esta aplicación ya existe en el directorio de Azure AD, puedes seleccionarla en la página **Agregar aplicaciones de Azure AD** para agregarla a tu cuenta del Centro de desarrollo. De lo contrario, puedes crear una nueva aplicación de Azure AD en la página **Adición de aplicaciones de Azure AD**.

### <a name="retrieve-the-credentials-required-for-submissions"></a>Recuperación de las credenciales necesarias para los envíos

A continuación, puede recuperar las credenciales del centro de Partners necesarias para los envíos: el **identificador de inquilino de Azure**, el identificador de **cliente** y la **clave de cliente**.

1. Vaya al [panel del centro de Partners](https://partner.microsoft.com/dashboard) e inicie sesión con sus credenciales de Azure ad.

2. En el panel del centro de Partners, seleccione el icono de engranaje (cerca de la esquina superior derecha del panel) y, después, seleccione **configuración del desarrollador**.

3. En el menú **configuración** del panel izquierdo, haga clic en **usuarios**.

4. Haga clic en el nombre de la aplicación Azure AD para ir a la configuración de la aplicación. En esta página, copie el **identificador de inquilino** y los valores de ID. de **cliente** .

5. En la sección **claves** , haga clic en **Agregar nueva clave**. En la siguiente pantalla, copie el valor de **clave** , que se corresponde con el secreto de cliente. No podrá volver a acceder a esta información después de salir de esta página, así que asegúrese de no perderla. Para obtener más información, consulta [Administrar claves para una aplicación de Azure AD](https://docs.microsoft.com/windows/uwp/publish/add-users-groups-and-azure-ad-applications#manage-keys-for-an-azure-ad-application).

### <a name="configure-automatic-store-submissions-in-visual-studio"></a>Configuración de envíos de almacén automáticos en Visual Studio

Después de completar los pasos anteriores, puede configurar el envío automático de tiendas en Visual Studio 2019.

1. Al final del [Asistente para crear paquetes de aplicaciones](#create-your-app-package-upload-file-using-visual-studio), seleccione **enviar automáticamente al Microsoft Store después de la validación del kit de certificación de aplicaciones de Windows** y haga clic en volver a **configurar**.

2. En el cuadro de diálogo **configurar las opciones de envío de Microsoft Store** , escriba el identificador de inquilino de Azure, el identificador de cliente y la clave de cliente.

    ![Configuración del envío de Microsoft Store](images/packaging-screen8.jpg)

    > [!Important]
    > Las credenciales se pueden guardar en el perfil para su uso en envíos futuros.

3. Haga clic en **Aceptar**.

El envío se iniciará una vez finalizada la prueba de WACK. Puede realizar un seguimiento del progreso del envío en la ventana **comprobar y publicar** .

![Comprobar y publicar el progreso](images/packaging-screen9.jpg)

## <a name="sideload-your-app-package"></a>Realizar la instalación de prueba del paquete de la aplicación

Con los paquetes de aplicación de UWP, las aplicaciones no se instalan en un dispositivo como lo hacen con las aplicaciones de escritorio. Por lo general, descargas aplicaciones para UWP desde Microsoft Store, que también instala la aplicación en el dispositivo para ti. Las aplicaciones se pueden instalar sin publicarlas en la Store (instalación de prueba). Esto le permite instalar y probar aplicaciones mediante el archivo de paquete de aplicación que creó. Si tienes una aplicación que no quieres vender en la Tienda, como una aplicación de línea de negocio (LOB), puedes realizar la instalación de prueba de dicha aplicación para que otros usuarios de la empresa pueden usarla.

Antes de poder transferir localmente la aplicación a un dispositivo de destino, debe [Habilitar el dispositivo para el desarrollo](../get-started/enable-your-device-for-development.md).

Para transferir localmente la aplicación a un dispositivo Windows 10 Mobile, use la herramienta [WinAppDeployCmd. exe](install-universal-windows-apps-with-the-winappdeploycmd-tool.md) . En el caso de equipos de escritorio, portátiles y tabletas, siga las instrucciones que se indican a continuación.

### <a name="sideload-your-app-package-on-windows-10-anniversary-update-or-later"></a>Transferir localmente el paquete de la aplicación en la actualización de aniversario de Windows 10 o posterior

Presentado en la actualización de aniversario de Windows 10 (Windows 10, versión 1607), los paquetes de aplicaciones se pueden instalar simplemente haciendo doble clic en el archivo de paquete de la aplicación. Para usarlo, navegue hasta el paquete de la aplicación o el archivo del lote de aplicaciones y haga doble clic en él. El instalador de la [aplicación](https://docs.microsoft.com/windows/msix/app-installer/app-installer-root) se inicia y proporciona la información básica de la aplicación, así como un botón instalar, la barra de progreso de la instalación y cualquier mensaje de error pertinente.

![Pantalla del instalador de aplicaciones para instalar una aplicación de ejemplo llamada Contoso](images/appinstaller-screen.png)

> [!NOTE]
> El instalador de la aplicación supone que el dispositivo es de confianza para la aplicación. Si estás realizando la instalación de prueba de una aplicación de desarrollador o empresarial, tendrás que instalar el certificado de firma en el almacén de Entidades de certificación raíz de personas de confianza o editores de confianza en el dispositivo. Si no estás seguro de cómo hacerlo, consulta [Instalar certificados de prueba](https://docs.microsoft.com/windows-hardware/drivers/install/installing-test-certificates).

### <a name="sideload-your-app-package-on-previous-versions-of-windows"></a>Transferir localmente el paquete de la aplicación en versiones anteriores de Windows

1.  Copia las carpetas de la versión de la aplicación que deseas instalar en el dispositivo de destino.

    Si ya has creado un lote de aplicaciones, tendrás una carpeta basada en el número de versión y una carpeta `*_Test`. Aquí tienes un ejemplo de estas dos carpetas (donde la versión que se va a instalar es 1.0.2.0):

    -   `C:\Projects\MyApp\MyApp\AppPackages\MyApp_1.0.2.0`
    -   `C:\Projects\MyApp\MyApp\AppPackages\MyApp_1.0.2.0_Test`

    Si no tienes un lote de aplicaciones, copia la carpeta de la arquitectura correcta y su carpeta `*_Test` correspondiente. Estas dos carpetas son un ejemplo de un paquete de la aplicación con la arquitectura x64 y su carpeta `*_Test`:

    -   `C:\Projects\MyApp\MyApp\AppPackages\MyApp_1.0.2.0_x64`
    -   `C:\Projects\MyApp\MyApp\AppPackages\MyApp_1.0.2.0_x64_Test`

2.  En el dispositivo de destino, abre la carpeta `*_Test`.
3.  Haz clic con el botón derecho en el archivo **Add-AppDevPackage.ps1**. Elige **Ejecutar con PowerShell** y sigue las instrucciones.  
    ![Explorador de archivos navegado hasta el script de PowerShell mostrado](images/packaging-screen7.jpg)

    Cuando se ha instalado el paquete de la aplicación, la ventana de PowerShell muestra este mensaje: **La aplicación se instaló correctamente.**
    >[!TIP]
    > Para abrir el menú contextual en una tableta, toque la pantalla en la que desea hacer clic con el botón derecho, mantenga presionado hasta que aparezca un círculo completo y, a continuación, levante el dedo. El menú contextual se abre después de levantar el dedo.

4.  Haz clic en el botón Inicio para buscar la aplicación por nombre y luego, iníciala.
