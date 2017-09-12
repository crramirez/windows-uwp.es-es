---
author: normesta
Description: "Crea un paquete de aplicación de Windows moderno para tu aplicación o juego existente de Windows Forms, WPF o Win32. Agrega experiencias modernas para los usuarios de Windows 10 y simplifica la implementación y la monetización."
Search.Product: eADQiWindows 10XVcnh
title: Puente de dispositivo de escritorio
ms.author: normesta
ms.date: 05/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, Windows 10, uwp, UWP
ms.assetid: 74373c24-f948-43bb-aa85-01e2e8e87162
ms.openlocfilehash: 1830c1661325afe68e8e7cd32528ec075e098b1d
ms.sourcegitcommit: 77bbd060f9253f2b03f0b9d74954c187bceb4a30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2017
---
# <a name="desktop-bridge"></a>Puente de dispositivo de escritorio

Lleva tus aplicaciones de escritorio existentes y agrega experiencias modernos para los usuarios de Windows 10. A continuación, logra mayor alcance en los mercados internacionales distribuyéndola a través de la Tienda Windows. Puedes monetizar tu aplicación de maneras mucho más sencilla aprovechando las características integradas directamente en la tienda. Por supuesto, no tienes que usar la tienda. Puedes usar tus canales existentes.
<div style="float: left; padding: 10px">
    ![imagen del puente de dispositivo de escritorio a UWP](images/desktop-to-uwp/desktop-bridge-4.png)
</div>
El Puente de dispositivo de escritorio a UWP es la infraestructura que hemos integrado en la plataforma que te permite distribuir tu juego o aplicación de escritorio Win32, WPF o Windows Forms o de manera eficaz mediante el uso de un paquete de la aplicación de Windows moderna.

Este paquete ofrece a tu aplicación una identidad y con esa identidad, tu aplicación de escritorio tiene acceso a las API de la Plataforma universal de Windows (UWP). Puedes usarlas para mejorar las experiencias atractivas y modernas, como iconos dinámicos y notificaciones.  Usa la compilación condicional sencillas y la comprobaciones en tiempo de ejecución para ejecutar código UWP solo cuando la aplicación se ejecuta en Windows 10.

Además del código que usas para destacar las experiencias de Windows 10, la aplicación permanece intacta y puedes seguir distribuyéndola a tu base de usuarios existente de Windows 7, Windows Vista o Windows XP. En Windows 10, la aplicación continúa ejecutándose en el modo de usuario de plena confianza al igual que hace hoy.

> [!NOTE]
> Consulta <a href="https://mva.microsoft.com/en-US/training-courses/developers-guide-to-the-desktop-bridge-17373?l=oZG0B1WhD_8406218965/">esta serie</a> de vídeos cortos que publicó Microsoft Virtual Academy. Estos vídeos te proporcionarán información detallada del proceso completo para incorporar una aplicación de escritorio a la Plataforma universal de Windows (UWP).

## <a name="benefits"></a>Beneficios

Estos son algunos motivos para crear un paquete de aplicación de Windows para tu aplicación de escritorio:

**Implementación optimizada**. Las aplicaciones y juegos que usan el puente ofrecen una experiencia de implementación excelente. Esta experiencia garantiza que los usuarios puedan instalar una aplicación y actualizarla con total confianza. Si un usuario decide desinstalar la aplicación, se quita completamente sin dejar rastro. Esto reduce el tiempo necesario para crear experiencias de instalación y mantener a los usuarios actualizados.

**Actualizaciones automáticas y licencias**. La aplicación puede participar en los servicios de licencia integrada y actualización automática de la Tienda Windows. La actualización automática es un mecanismo muy confiable y eficaz, ya que solo se descargan las partes modificadas de los archivos.

**Mayor alcance y monetización simplificada**. Si decides distribuir tus aplicaciones a través de la Tienda Windows, podrás llegar a millones de usuarios de Windows 10, que pueden adquirir aplicaciones, juegos y realizar compras desde la aplicación gracias a las opciones de pago local.

**Incorporación de características de UWP**.  Puedes agregar características de UWP al paquete de la aplicación a tu propio ritmo, como una interfaz de usuario XAML, actualizaciones de iconos dinámicos, tareas en segundo plano para UWP, servicios de aplicaciones y mucho más.

**Casos de uso ampliado en dispositivos**. Al usar el puente, puedes migrar gradualmente tu código a la Plataforma universal de Windows para llegar a todos los dispositivos Windows 10, incluidos los teléfonos, Xbox One y HoloLens.

Si quieres ver una lista completa de las ventajas que se ofrecen, consulta [Puente de dispositivo de escritorio](https://developer.microsoft.com/windows/bridges/desktop).

## <a name="prepare"></a>Preparación

¿Vas a publicar la aplicación en la [Tienda de aplicaciones Windows](https://www.microsoft.com/store/apps)? Si es así, primero debes rellenar [este formulario](https://developer.microsoft.com/windows/projects/campaigns/desktop-bridge). Microsoft se pondrá en contacto contigo para iniciar el proceso de incorporación. Como parte de este proceso, podrás reservar un nombre en la tienda y obtener la información que necesitas para crear un paquete de aplicación de Windows.

A continuación, lee el artículo [Preparar la aplicación de escritorio para empaquetar](desktop-to-uwp-prepare.md) y soluciona cualquier problema derivado de la aplicación antes de crear un paquete de aplicación de Windows para ella. Es posible que no tengas que realizar muchos cambios en la aplicación antes de crear el paquete. Sin embargo, hay algunas situaciones que tienes que tener en cuenta ya que es posible que tengas que modificar la aplicación antes de crear un paquete para ella.

<span id="convert" />
## <a name="package"></a>Paquete

Estas son algunas herramientas que puedes usar para crear un paquete de aplicación de Windows para tu aplicación.

### <a name="desktop-app-converter"></a>Desktop App Converter

Aunque el término "Converter" aparece en el nombre de esta herramienta, realmente no convierte la aplicación. La aplicación no cambia. Sin embargo, esta herramienta genera un paquete de aplicación de Windows para ti. Puede ser muy útil en casos donde la aplicación realiza un gran número de modificaciones en el sistema o si tienes alguna duda sobre lo que hace el instalador.

Desktop App Converter también realiza alguna tarea extra por ti. Esta es una lista de algunas de ellas.

* Registra automáticamente los controladores de vista previa, los de miniatura, los de propiedad, las reglas del firewall y los marcadores de dirección URL.

* Registra automáticamente las asignaciones de tipo de archivo que permiten a los usuarios agrupar archivos mediante la columna **Kind** del Explorador de archivos.

* Registra los servidores COM públicos.

* Genera un certificado que puedes usar para ejecutar la aplicación.

* Valida la aplicación con los requisitos del Puente de dispositivo de escritorio y la Tienda Windows.

Consulta [Empaquetar una aplicación con Desktop App Converter (Puente de dispositivo de escritorio a UWP)](desktop-to-uwp-run-desktop-app-converter.md)

### <a name="manual-packaging"></a>Empaquetado manual

Si quieres conocer al detalle la conversión, puedes crear un archivo de manifiesto y ejecutar la herramienta **MakeAppx.exe** para crear el paquete de la aplicación de Windows.

Este enfoque puede tener sentido si estás familiarizado con los cambios que realiza el instalador en el sistema o si no tienes un programa de instalación y te encargas tú mismo de copiar los archivos físicamente a una carpeta o mediante comandos como **xcopy**. Aún así, no permitas que la ausencia del instalador te haga empaquetar la aplicación de forma manual. Puedes usar Desktop App Converter para empaquetar la aplicación, incluso si no tienes un instalador.

Consulta [Empaquetar una aplicación de forma manual (Puente de dispositivo de escritorio a Puente de UWP)](desktop-to-uwp-manual-conversion.md)

### <a name="visual-studio"></a>Visual Studio

Esta opción es similar a la opción manual que se describió anteriormente, excepto que Visual Studio realiza tareas tales como crear el paquete de aplicación y los activos visuales de la misma. Visual Studio es una herramienta que puedes usar para empaquetar manualmente la aplicación y que además te ofrece unas cuantas ventajas adicionales.

Consulta [Guía de empaquetado de Puente de dispositivo de escritorio para aplicaciones de escritorio de .NET con Visual Studio](desktop-to-uwp-packaging-dot-net.md)

### <a name="third-party-installer"></a>Instalador de terceros

 Varios productos e instaladores de terceros conocidos son compatibles con el Puente de dispositivo de escritorio a UWP. Puedes usarlos para crear los instaladores MSI o los paquetes de la aplicación con solo unos clics. Si bien no ofrecemos documentación sobre cómo usar estas herramientas, puedes visitar sus sitios web para obtener más información.

 Aquí te indicamos varias opciones:

#### <a name="advanced-installer"></a>Instalador avanzado

Caphyon proporciona una herramienta gratuita de empaquetado de aplicaciones de escritorio basada en una interfaz gráfica de usuario (GUI) que te permitirá crear un paquete de aplicación de Windows para tu aplicación en unos pocos clics. Te permite usar cualquier instalador (incluso aquellos que se ejecutan en modo silencioso) y realiza una comprobación de validación para determinar si la aplicación es adecuada para empaquetado.
<div style="float: left; padding: 10px; width: 20%">
     ![Logotipo de instalador avanzado](images/desktop-to-uwp/Advanced_Installer_Vertical.png)
</div>
La extensión Desktop App Converter está integrada con Hyper-V y [VMware](http://www.vmware.com/). Esto significa que puedes usar tus propias máquinas virtuales sin tener que descargar una imagen [Docker](https://docs.docker.com/) coincidente que puede llegar a tener más de 3 GB de tamaño.

Puedes usar el [Instalador avanzado](http://www.advancedinstaller.com/) para generar MSI y [paquetes de aplicación de Windows](http://www.advancedinstaller.com/uwp-app-package.html) a partir de proyectos existentes. De igual manera, también puedes usar el Instalador avanzado para importar los paquetes de aplicación de Windows que hayas creado mediante Microsoft Desktop App Converter. Una vez importados, puedes mantenerlos mediante herramientas visuales diseñadas específicamente para aplicaciones para UWP.

Del mismo modo, el Instalador avanzado también te proporciona una extensión de Visual Studio 2017 y 2015 que puedes usar para [compilar y depurar aplicaciones de Puente de dispositivo de escritorio](http://www.advancedinstaller.com/debug-desktop-bridge-apps.html).

Consulta este [vídeo](https://www.youtube.com/watch?v=cmLKgn04Vfg&feature=youtu.be) para ver una introducción rápida.

#### <a name="cloudhouse-compatibility-containers"></a>Contenedores de compatibilidad con Cloudhouse

Para los clientes empresariales que tienen aplicaciones de línea de negocio que son incompatibles con Windows 10 y Windows 10S, los contenedores de compatibilidad de Cloudhouse permiten que las aplicaciones de Windows XP y Windows 7 se conviertan en UWP y, a continuación, se entreguen a través de la TiendaWindows para empresas o Microsoft Intune, sin cambiar el código fuente.
<div style="float: left; padding: 10px; width: 20%">
     ![Contenedores de compatibilidad con Cloudhouse](images/desktop-to-uwp/container.png)
</div>
Cloudhouse proporciona a empaquetador automático que toma cualquier aplicación y crea un contenedor de compatibilidad empaquetando la aplicación donde se ejecuta hoy, por ejemplo, Windows XP. El contenedor se puede convertir luego en el nuevo formato de UWP mediante la integración con la herramienta de conversión del Puente de dispositivo de escritorio para crear el paquete de aplicación de Windows.

El empaquetador automático usa instalar y capturar y el análisis en tiempo de ejecución para crear un contenedor para la aplicación que incluye los archivos de la aplicación y el Registro, y el motor de compatibilidad y redireccionamiento que permite que la aplicación se ejecute en Windows 10. Además, puedes incluir y aislar los tiempos de ejecución o requisitos previos necesarios para ejecutar la aplicación para que no afecte o entre en conflicto con otras aplicaciones o tiempos de ejecución pueden estar ya instalados.


Consulta su [blog](http://www.cloudhouse.com/resources/release-solution-to-get-any-line-of-business-app-to-uwp) anuncio de compatibilidad para aplicaciones universales de Windows y la Tienda Windows para empresas.

#### <a name="firegiant"></a>FireGiant

La [extensión FireGiant Appx](https://www.firegiant.com/products/wix-expansion-pack/appx) te permite crear paquetes MSI y paquetes de aplicaciones de Windows al mismo tiempo desde el mismo código fuente de WiX. Cada vez que compiles, puedes elegir el Puente de dispositivo de escritorio en Windows 10 con un paquete de aplicación de Windows y las versiones anteriores de Windows con MSI.
<div style="float: left; padding: 10px; width: 20%">
     ![Logotipo de instalador avanzado](images/desktop-to-uwp/FG3rdPartyLogo.png)
</div>
La extensión FireGiant Appx usa el análisis estático y la emulación inteligente de tus proyectos WiX para crear paquetes de aplicación de Windows sin la sobrecarga de tiempo de ejecución y espacio en disco de contenedores o máquinas virtuales.

Como la extensión FireGiant Appx no convierte el instalador ejecutándolo, puede mantener el instalador WiX sin necesidad de convertirlo repetidamente a paquetes de aplicación de Windows. Todos los usuarios de diferentes versiones de Windows obtienen las mejoras más recientes y no tienes que preocuparte de que los paquetes de aplicación de MSI y Windows no se sincronicen.

Echa un vistazo a este [vídeo](https://www.youtube.com/watch?v=AFBpdBiAYQE) para ver cómo, en un par de líneas de código, Rob Mensching, el director general de FireGiant crea una versión de Appx (paquete de la aplicación de Windows) de la popular herramienta de compresión 7-Zip de código abierto y cómo mejora, a continuación, tanto la aplicación de Windows como los paquetes MSI con cambios en el mismo código fuente de WiX.

#### <a name="installaware"></a>InstallAware

Install**Aware** proporciona extensiones sin Install**Aware** para las versiones 2012-2017 de Visual Studio. Puedes usarlas para crear paquetes de aplicaciones de Windows con un solo clic directamente desde la [barra de herramientas de Visual Studio](https://www.installaware.com/visual-studio-installer-2015.htm).
<div style="float: left; padding: 10px; width: 20%">
    ![Logotipo de FireGiant](images/desktop-to-uwp/installaware.png)
</div>
También puedes importar cualquier instalación, incluso si no tienes el código fuente para ella, usando Package**Aware** (capturas de instalación sin instantáneas), o el Asistente para importación de base de datos (para todos los instaladores de MSI y módulos de combinación MSM). Puedes usar [herramientas de la interfaz gráfica de usuario](https://www.installaware.com/scripting-two-way-integrated-ide.htm) para mantener y mejorar las importaciones, visualmente o mediante la creación de scripts.

Las [opciones de creación de APPX avanzadas](https://www.installaware.com/mhtml5/desktop/appx.htm) te ayudan a tener como objetivo los envíos de la Tienda Windows o a producir binarios del paquete de aplicación de Windows con firma para la distribución de transferencia local a los usuarios finales. Incluso puedes compilar paquetes de **WSA**(Windows Server Applications) Installer destinados a las implementaciones en **Nano servidor**, todo ello desde un único origen y con compatibilidad completa con la [automatización de la línea de comandos](https://www.installaware.com/scripting-automation-interface.htm), además de una GUI.

Install**Aware** también [creó en código abierto](https://www.installaware.com/gnu.asp) una **biblioteca de generadores de APPX**, junto con un applet de línea de comandos de ejemplo, bajo la licencia de GNU Affero GPL. Se diseñaron para usarse con plataformas de código abierto como WiX.

#### <a name="installshield"></a>InstallShield

InstallShield proporciona una solución única para desarrollar instaladores MSI y EXE, crear paquetes de Plataforma universal de Windows y aplicación de Windows Server (WSA), y virtualizar aplicaciones con un mínimo de scripting, codificación y rediseño.
<div style="float: left; padding: 10px; width: 20%">
    ![Logotipo de InstallShield](images/desktop-to-uwp/InstallShield-logo.jpg)
</div>
Examina tu proyecto de InstallShield en cuestión de segundos para ahorrar horas de trabajo de investigación identificando automáticamente los posibles problemas de compatibilidad entre tu aplicación y los paquetes de UWP y WSA.

Prepárate para la Tienda Windows y simplifica la experiencia de instalación del software en Windows 10 creando paquetes de aplicación para UWP desde tus proyectos de InstallShield existentes. Compila paquetes tanto de Windows Installer como de aplicación de UWP para admitir los escenarios de implementación que desee de los clientes. Admite implementaciones de Nano Server y Windows Server 2016 compilando paquetes de WSA desde tus proyectos existentes de InstallShield.

Desarrolla tu instalación en módulos para facilitar la implementación y el mantenimiento y, a continuación, combina los componentes y las dependencias en el tiempo de compilación en un único paquete de aplicación para UWP para la Tienda Windows. Para la distribución directa fuera de la tienda, agrupa los paquetes de aplicación para UWP y otras dependencias junto con un programa de instalación de la interfaz de usuario Suite o avanzado.

Más información en este [libro electrónico](https://na01.safelinks.protection.outlook.com/?url=https%3A%2F%2Fresources.flexerasoftware.com%2Fweb%2Fpdf%2FeBook-IS-Your-Fast-Track-to-Profit.pdf&data=02%7C01%7Cnormesta%40microsoft.com%7C86b9a00bc8e345c2ac6208d4ba464802%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C1%7C636338258409706554&sdata=IAYNp9nFc8B5ayxwrs%2FQTWowUmOda6p%2Fn%2BjdHea257M%3D&reserved=0).


#### <a name="rad-studio"></a>RAD Studio

Consulta [RAD Studio de Embarcadero](https://www.embarcadero.com/products/rad-studio/windows-10-store-desktop-bridge)

## <a name="integrate"></a>Integración

Si la aplicación se debe integrar con el sistema (por ejemplo, establecer reglas de firewall), describe estas cuestiones en el manifiesto de paquete de la aplicación y el sistema hará el resto. Para la mayoría de estas tareas, no tendrás que escribir nada de código. Con un poco de XML en el manifiesto, puedes hacer varias cosas; por ejemplo, puedes iniciar un proceso cuando el usuario inicie sesión, integrar la aplicación en el Explorador de archivos y agregarla a una lista de los destinos de impresión que aparecen en otras aplicaciones.

Consulta [Integrar la aplicación con Windows 10 (Puente de dispositivo de escritorio de Windows)](desktop-to-uwp-extensions.md).

## <a name="enhance"></a>Mejora

Una vez empaquetada la aplicación, puedes agregarle características tales como iconos dinámicos y notificaciones de inserción. Algunas de estas funcionalidades pueden mejorar considerablemente el nivel de interacción con la aplicación y no te costará nada agregarlas. Recuerda que algunas mejoras requieren un poco más código.

Consulta [Mejorar tu aplicación de escritorio para Windows 10](desktop-to-uwp-enhance.md).

## <a name="extend"></a>Ampliación

Algunas experiencias de Windows 10 (por ejemplo, una página de interfaz de usuario habilitada para entrada táctil) deben ejecutarse dentro de un contenedor de aplicación moderna. En general, primero debes determinar si puedes agregar tu experiencia [mejorando](desktop-to-uwp-enhance.md) la aplicación de escritorio existente con las API de UWP. Si tienes que usar un componente de UWP para lograr la experiencia, puedes agregar un proyecto de UWP a la solución y usar los servicios de aplicaciones para comunicarse entre tu aplicación de escritorio y el componente de UWP.

Consulta [Ampliar tu aplicación de escritorio con componentes de UWP modernos](desktop-to-uwp-extend.md).

## <a name="migrate"></a>Migración

Puedes migrar gradualmente el código anterior a UWP a la vez que conservas la capacidad de ejecutar y publicar la aplicación en el escritorio de Windows. Una vez que hayas migrado totalmente a UWP (y que la aplicación ya no tenga ningún componente de WPF o Win32), podrás llegar a todos los dispositivos Windows, incluidos los teléfonos, Xbox One y HoloLens.

## <a name="test"></a>Prueba

Para probar la aplicación en una configuración realista mientras la preparas para su distribución, lo mejor es firmar la aplicación e instalarla. Consulta [Probar la aplicación](https://docs.microsoft.com/en-us/windows/uwp/porting/desktop-to-uwp-debug#test-your-app).

>[!IMPORTANT]
> Si tienes previsto publicar la aplicación en la Tienda Windows, debes asegurarte de que tu aplicación funciona correctamente en dispositivos que ejecutan Windows 10S. Este es un requisito de la tienda. Consulta [Probar la aplicación de Windows en Windows 10 S](desktop-to-uwp-test-windows-s.md).

## <a name="validate"></a>Validación

Para que la aplicación tenga posibilidades de publicarse en la Tienda Windows o de obtener la [certificación de Windows](http://go.microsoft.com/fwlink/p/?LinkID=309666), debes validarla y probarla localmente antes de enviarla para su certificación.

Si estás usando DAC para empaquetar la aplicación, puedes usar la nueva marca ``-Verify`` para validar el paquete con los requisitos del Puente de dispositivo de escritorio y la Tienda. Consulta [Empaquetar una aplicación, firmarla y prepararla para su envío a la Tienda](desktop-to-uwp-run-desktop-app-converter.md#optional-parameters).

Si usas Visual Studio, puedes validar la aplicación desde el asistente **Crear paquetes de aplicaciones**. Consulta [Crear un paquete de la aplicación](../packaging/packaging-uwp-apps.md#create-an-app-package).

Para ejecutar la herramienta manualmente, consulta [Kit para la certificación de aplicaciones en Windows](../debug-test-perf/windows-app-certification-kit.md).

Para revisar la lista de pruebas que usa la certificación de aplicaciones de Windows para validar la aplicación, consulta [Windows Desktop Bridge app tests (Pruebas de la aplicación del puente de dispositivo escritorio de Windows)](../debug-test-perf/windows-desktop-bridge-app-tests.md).

## <a name="distribute"></a>Distribución

Puedes distribuir la aplicación publicándola en la Tienda Windows o mediante la instalación de prueba en otros sistemas.

Consulta [Distribuir una aplicación de escritorio empaquetada (Puente de dispositivo de escritorio)](desktop-to-uwp-distribute.md).

## <a name="support-and-feedback"></a>Soporte técnico y comentarios

**Encuentra respuestas a preguntas específicas**

Nuestro equipo supervisa estas [etiquetas de StackOverflow](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge).

**Enviar comentarios o realizar sugerencias acerca de las características**

Consulta [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)

**Envíanos tus comentarios acerca de este artículo**

Usa la sección comentarios que tienes a continuación.

## <a name="in-this-section"></a>En esta sección

| Tema | Descripción |
|-------|-------------|
| [Preparación para empaquetar una aplicación](desktop-to-uwp-prepare.md) | Proporciona una lista de los elementos que hay que revisar antes de empaquetar la aplicación de escritorio. |
| [Empaquetar una aplicación con Desktop App Converter (Puente de dispositivo de escritorio)](desktop-to-uwp-run-desktop-app-converter.md) | Muestra cómo ejecutar Desktop App Converter. |
| [Empaquetar una aplicación de forma manual (Puente de dispositivo de escritorio)](desktop-to-uwp-manual-conversion.md) | Aprende a crear un paquete de la aplicación y un manifiesto manualmente. |
| [Empaquetar una aplicación .NET mediante Visual Studio (Puente de dispositivo de escritorio)](desktop-to-uwp-packaging-dot-net.md)| Muestra cómo empaquetar la aplicación de escritorio con Visual Studio. |
| [Integrar la aplicación con Windows 10 (Puente de dispositivo de escritorio)](desktop-to-uwp-extensions.md) | Integra la aplicación con Windows 10 mediante el uso de las tareas con descripciones en el archivo de manifiesto del paquete de tu proyecto de empaquetado. |
| [Mejorar tu aplicación de escritorio para Windows 10](desktop-to-uwp-enhance.md)| Usa las API de UWP para agregar modernas experiencias que destacan para los usuarios de Windows 10. |
| [API de UWP disponibles para una aplicación de escritorio empaquetada (Puente de dispositivo de escritorio)](desktop-to-uwp-supported-api.md) | Consulta qué API de UWP puedes usar en la aplicación de escritorio empaquetada. |
| [Ampliar tu aplicación de escritorio con componentes de UWP modernos](desktop-to-uwp-extend.md)| Agrega experiencias avanzadas que deben ejecutarse dentro de un contenedor de aplicación para UWP. Conecta tu aplicación de escritorio con el proceso de UWP mediante servicios de aplicación.|
| [Ejecutar, depurar y probar una aplicación de escritorio empaquetada (Puente de dispositivo de escritorio)](desktop-to-uwp-debug.md) | Explica las opciones para depurar la aplicación empaquetada. |
| [Distribuir una aplicación de escritorio empaquetada (Puente de dispositivo de escritorio)](desktop-to-uwp-distribute.md) | Consulta cómo distribuir la aplicación convertida a los usuarios.  |
| [Segundo plano del Puente de dispositivo de escritorio (Puente de dispositivo de escritorio)](desktop-to-uwp-behind-the-scenes.md) | Realiza un análisis más profundo sobre cómo funciona el puente de dispositivo de escritorio a UWP en modo no visible. |
| [Problemas conocidos (Puente de dispositivo de escritorio)](desktop-to-uwp-known-issues.md) | Enumera los problemas conocidos del puente de dispositivo de escritorio a UWP. |
| [Muestras de código del Puente de dispositivo de escritorio](https://github.com/Microsoft/DesktopBridgeToUWP-Samples) | Ejemplos de código en GitHub que muestran las características de las aplicaciones convertidas. |
