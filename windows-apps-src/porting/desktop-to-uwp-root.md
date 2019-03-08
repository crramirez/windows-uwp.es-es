---
Description: Crea un paquete de aplicación de Windows moderno para tu aplicación o juego existente de Windows Forms, WPF o Win32. Agrega experiencias modernas para usuarios de Windows 10 y simplificar la implementación y la monetización.
Search.Product: eADQiWindows 10XVcnh
title: Empaquetar aplicaciones de escritorio
ms.date: 09/05/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 74373c24-f948-43bb-aa85-01e2e8e87162
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 70a9b6046e3b7be9ac84678ac21c0c9f89a4a7b2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627830"
---
# <a name="package-desktop-applications-desktop-bridge"></a>Empaquetar aplicaciones de escritorio (Desktop Bridge)

Tomar la aplicación de escritorio existente y agregar experiencias modernas para Windows 10 usuarios. A continuación, logra mayor alcance en los mercados internacionales distribuyéndola a través de Microsoft Store. Puede rentabilizar la aplicación de mucho más sencillas maneras mediante el aprovechamiento de las características integradas en el almacén. Por supuesto, no tienes que usar la tienda. Puedes usar tus canales existentes.

![Puente de dispositivo de escritorio](images/desktop-to-uwp/desktop-bridge-4.png)

Cuando creas un paquete para su aplicación de escritorio, la aplicación obtendrá una identidad y con esa identidad, la aplicación de escritorio tiene acceso a Windows plataforma universal de las API. Puedes usarlas para mejorar las experiencias atractivas y modernas, como iconos dinámicos y notificaciones.  Usar la compilación condicional simple y comprueba en tiempo de ejecución para ejecutar código UWP solo cuando la aplicación se ejecuta en Windows 10.

El código que utiliza para iluminar experiencias de Windows 10, aparte de la aplicación permanece sin cambios y puede seguir para distribuirla a los existentes Windows 7, Windows Vista o base de usuarios de Windows XP. En Windows 10, la aplicación continúa ejecutándose en plena confianza al igual que el modo de usuario está haciendo hoy en día.

>[!IMPORTANT]
>La capacidad para crear un paquete de aplicación de Windows para su aplicación de escritorio (conocido también como el puente de escritorio) se introdujo en Windows 10, versión 1607, y solo puede usarse en proyectos que tienen como destino Windows 10 Anniversary Update (10.0; Compilación 14393) o una versión posterior de Visual Studio.

> [!NOTE]
> Consulta <a href="https://mva.microsoft.com/en-US/training-courses/developers-guide-to-the-desktop-bridge-17373?l=oZG0B1WhD_8406218965/">esta serie</a> de vídeos cortos que publicó Microsoft Virtual Academy. Estos vídeos le guiarán a través de todo el proceso de poner su aplicación de escritorio a la plataforma Universal de Windows (UWP).

## <a name="benefits"></a>Ventajas

Estos son algunos motivos para crear un paquete de aplicación de Windows para tu aplicación de escritorio:

:heavy_check_mark: **Implementación optimizada**. Las aplicaciones y juegos que usan el puente ofrecen una experiencia de implementación excelente. Esta experiencia se garantiza que los usuarios pueden instalar una aplicación y actualizarlo con confianza. Si un usuario decide desinstalar la aplicación, se quita completamente sin dejar rastros. Esto reduce el tiempo necesario para crear experiencias de instalación y mantener a los usuarios actualizados.

:heavy_check_mark: **Actualizaciones automáticas y licencias**. La aplicación puede participar en la Microsoft Store integrado licencias e instalaciones de actualización automática. La actualización automática es un mecanismo muy confiable y eficaz, ya que solo se descargan las partes modificadas de los archivos.

:heavy_check_mark: **Mayor alcance y monetización simplificada**. Si decides distribuir tus aplicaciones a través de Microsoft Store, podrás llegar a millones de usuarios de Windows 10, que pueden adquirir aplicaciones, juegos y realizar compras desde la aplicación gracias a las opciones de pago local.

:heavy_check_mark: **Incorporación de características de UWP**.  Puedes agregar características de UWP al paquete de la aplicación a tu propio ritmo, como una interfaz de usuario XAML, actualizaciones de iconos dinámicos, tareas en segundo plano para UWP, servicios de aplicaciones y mucho más.

:heavy_check_mark: **Casos de uso ampliado en dispositivos**. Al usar el puente, puedes migrar gradualmente tu código a la Plataforma universal de Windows para llegar a todos los dispositivos Windows 10, incluidos los teléfonos, Xbox One y HoloLens.

Si quieres ver una lista completa de las ventajas que se ofrecen, consulta [Puente de dispositivo de escritorio](https://developer.microsoft.com/windows/bridges/desktop).

## <a name="prepare"></a>Preparar

En primer lugar, prepare la aplicación si revisa el artículo [preparar empaquetar la aplicación de escritorio](desktop-to-uwp-prepare.md)y, a continuación, direccionamiento alguno de los problemas que se aplican a la aplicación antes de crear un paquete de aplicación de Windows para él. Podría no tener que realizar muchos cambios a la aplicación antes de crear el paquete. Sin embargo, existen algunas situaciones que podrían requerir ajustar la aplicación antes de crear un paquete para ella.

<a id="convert" />

## <a name="package"></a>Empaquetar

Estas son algunas herramientas que puedes usar para crear un paquete de aplicación de Windows para tu aplicación.

### <a name="desktop-app-converter"></a>Desktop App Converter

Aunque el término "Converter" aparece en el nombre de esta herramienta, realmente no convierte la aplicación. La aplicación permanece sin cambios. Sin embargo, esta herramienta genera un paquete de aplicación de Windows para ti. Puede ser muy útil en casos donde la aplicación realiza muchas de las modificaciones del sistema, o si tiene cualquier incertidumbre sobre lo que hace el instalador.

El Desktop App Converter traduce las acciones del instalador al sistema de registro que se va a usar la versión empaquetada de la aplicación y archivo virtual. Desktop App Converter también realiza alguna tarea extra por ti. Esta es una lista de algunas de ellas.

:heavy_check_mark: Registra automáticamente los controladores de vista previa, los de miniatura, los de propiedad, las reglas del firewall y los marcadores de dirección URL.

:heavy_check_mark: Asignaciones de tipos que permiten a los usuarios para agrupar los archivos mediante el uso de archivar registra automáticamente el **tipo** columna en el Explorador de archivos.

:heavy_check_mark: Registra los servidores COM públicos.

:heavy_check_mark: Genera un certificado que puede usar para ejecutar la aplicación.

:heavy_check_mark: Valida la aplicación frente a la aplicación empaquetada de escritorio y los requisitos de Microsoft Store.

Otra buena razón para usar el Desktop App Converter es si se mantiene la aplicación mediante el uso de un entorno de desarrollo diferentes que no sea de Visual Studio. Puede usar el Desktop App Converter incluso si la aplicación no tiene un instalador.

Consulte [empaquetar una aplicación de escritorio mediante el Desktop App Converter](desktop-to-uwp-run-desktop-app-converter.md)

### <a name="visual-studio"></a>Visual Studio

Si mantienes tu aplicación con Visual Studio y tu aplicación no tiene un instalador o tu instalador no realiza demasiadas tareas complicadas, plantéate usar Visual Studio en su lugar.

Visual Studio facilita considerablemente la creación de un paquete. Tendrás que agrear un proyecto de empaquetado, hacer referencia al proyecto de escritorio y luego presionar F5 para depurar la aplicación. No es necesario realizar ajustes manuales. Esta nueva experiencia optimizada es una gran mejora de la experiencia que estaba disponible en la versión anterior de Visual Studio. Aquí te mostramos algunas de las cosas que puedes hacer con él.

:heavy_check_mark: Generar automáticamente los activos visuales.

:heavy_check_mark: Realizar cambios en el manifiesto con un diseñador visual.

:heavy_check_mark: Generar el paquete mediante un asistente.

:heavy_check_mark: Asignar fácilmente una identidad de su aplicación de un nombre que ya ha reservado en [centro de partners](https://partner.microsoft.com/dashboard).

Consulte [empaquetar una aplicación de escritorio con Visual Studio](desktop-to-uwp-packaging-dot-net.md)

### <a name="third-party-installer"></a>Instalador de terceros

 Varios productos de terceros más populares y los instaladores admiten ahora la capacidad para empaquetar una aplicación de escritorio. Puedes usarlos para crear los instaladores MSI o los paquetes de la aplicación con solo unos clics. Si bien no ofrecemos documentación sobre cómo usar estas herramientas, puedes visitar sus sitios web para obtener más información.

#### <a name="advanced-installer"></a>Instalador avanzado

Caphyon proporciona una herramienta gratuita de empaquetado de aplicaciones de escritorio basada en una interfaz gráfica de usuario (GUI) que te permitirá crear un paquete de aplicación de Windows para tu aplicación en unos pocos clics. Puede usar a cualquier instalador; incluso aquellos que se ejecutan en modo silencioso y realiza una validación comprueban para determinar si la aplicación es adecuada para el empaquetado.
La extensión Desktop App Converter está integrada con Hyper-V y [VMware](https://www.vmware.com/). Esto significa que puedes usar tus propias máquinas virtuales sin tener que descargar una imagen [Docker](https://docs.docker.com/) coincidente que puede llegar a tener más de 3 GB de tamaño.

<img width="20%" src="images/desktop-to-uwp/Advanced_Installer_Vertical.png">

Puedes usar el [Instalador avanzado](https://www.advancedinstaller.com/) para generar MSI y [paquetes de aplicación de Windows](https://www.advancedinstaller.com/uwp-app-package.html) a partir de proyectos existentes. De igual manera, también puedes usar el Instalador avanzado para importar los paquetes de aplicación de Windows que hayas creado mediante Microsoft Desktop App Converter. Una vez importados, puedes mantenerlos mediante herramientas visuales diseñadas específicamente para aplicaciones para UWP.

Del mismo modo, el Instalador avanzado también te proporciona una extensión de Visual Studio 2017 y 2015 que puedes usar para [compilar y depurar aplicaciones de Puente de dispositivo de escritorio](https://www.advancedinstaller.com/debug-desktop-bridge-apps.html).

Consulta este [vídeo](https://www.youtube.com/watch?v=cmLKgn04Vfg&feature=youtu.be) para ver una introducción rápida.

> [!TIP]
> Asegúrate de echar un vistazo a la edición publicada recientemente [Edición exprés para instaladores avanzados](https://www.advancedinstaller.com/express-edition.html).

#### <a name="cloudhouse-compatibility-containers"></a>Contenedores de compatibilidad con Cloudhouse

Para los clientes empresariales que tienen aplicaciones de línea de negocio que son incompatibles con Windows 10 y Windows 10 S, los contenedores de compatibilidad de Cloudhouse permiten que las aplicaciones de Windows XP y Windows 7 se ejecuten en Windows 10 y los conviertan para ejecutarse en la Plataforma universal de Windows (UWP) para entregarse a través de Microsoft Store para Empresas o Microsoft Intune sin cambiar el código fuente. Regístrate para obtener una [Evaluación gratuita](https://www.cloudhouse.com/free-trial).

<img width="20%" src="images/desktop-to-uwp/cloudhouse-container-logo.png">

Cloudhouse proporciona un empaquetador automática para empaquetar aplicaciones de línea de negocio en [compatibilidad contenedores](https://docs.cloudhouse.com/37613-overview/266723-compatibility-containers-for-applications) en los sistemas operativos que las aplicaciones de hoy en día se ejecuta en (por ejemplo: Windows XP) y, a continuación, [prepararlo para la conversión](https://docs.cloudhouse.com/37613-overview/266725-compatibility-containers-for-desktop-bridge?from_search=17883905) a UWP. El contenedor se convierte luego en el nuevo paquete de aplicación de Windows mediante su integración con la herramienta Desktop App Converter de Microsoft.

El empaquetador automático usa instalar y capturar y el análisis en tiempo de ejecución para crear un contenedor para la aplicación que incluye los archivos, el registro, los tiempos de ejecución y las dependencias de la aplicación, así como el motor de redireccionamiento y compatibilidad que permite que la aplicación se ejecute en Windows 10. El contenedor proporciona el aislamiento de la aplicación y su tiempos de ejecución, para que no afecten ni entren en conflicto con otras aplicaciones que se ejecutan en el dispositivo del usuario.

Obtén más información sobre cómo puedes ofrecer aplicaciones empresariales a través de Microsoft Store para Empresas en nuestro [blog de lanzamientos](https://www.cloudhouse.com/resources/release-solution-to-get-any-line-of-business-app-to-uwp).

#### <a name="firegiant"></a>FireGiant

El [extensión FireGiant MSIX](https://www.firegiant.com/products/wix-expansion-pack/msix) le permite crear paquetes de aplicaciones de Windows y los paquetes MSI al mismo tiempo desde el mismo código fuente de WiX. Cada vez que compila, puede tener como destino Windows 10 con un paquete de aplicación de Windows y las versiones anteriores de Windows con MSI.

<img width="20%" src="images/desktop-to-uwp/FG3rdPartyLogo.png">

La extensión de FireGiant MSIX usa análisis estático y emulación inteligente de los proyectos de WiX para crear paquetes de aplicaciones de Windows sin la sobrecarga de espacio y tiempo de ejecución de discos de máquinas virtuales o contenedores.

Dado que la extensión FireGiant MSIX no convierte a su instalador si lo ejecuta, puede mantener al instalador de WiX sin tener que convertirla repetidamente para paquetes de aplicaciones de Windows. Todos los usuarios de diferentes versiones de Windows obtienen las mejoras más recientes y no tienes que preocuparte de que los paquetes de aplicación de MSI y Windows no se sincronicen.

Pruebe esta [vídeo](https://www.youtube.com/watch?v=AFBpdBiAYQE) y vea cómo en un par de líneas de código FireGiant CEO Rob Mensching crea una versión de Appx (paquete de aplicación de Windows) de la herramienta de compresión muy popular de 7-Zip abierto y, a continuación, cómo mejora tanto aplicaciones de Windows y Paquetes MSI con los cambios en el mismo código fuente de WiX.

#### <a name="installaware"></a>InstallAware

Install**Aware**, con un [registro de seguimiento](https://www.installaware.com/press-room.htm) de innovaciones rápidamente compatibles de Microsoft, compilaciones [paquetes de aplicaciones Windows (Puente de dispositivo de escritorio)](https://www.installaware.com/appx-builder.htm), App-V (virtualización de aplicaciones), MSI (Windows Installer) y paquetes EXE (código nativo) desde un único origen.

<img width="20%" src="images/desktop-to-uwp/installaware.png">

Install**Aware** proporciona extensiones sin Install**Aware** para las versiones 2012-2017 de Visual Studio. Puedes usarlas para crear paquetes de aplicaciones de Windows con un solo clic directamente desde la [barra de herramientas de Visual Studio](https://www.installaware.com/visual-studio-installer-2015.htm).

También puedes importar cualquier instalación, incluso si no tienes el código fuente para ella, usando Package**Aware** (capturas de instalación sin instantáneas), o el Asistente para importación de base de datos (para todos los instaladores de MSI y módulos de combinación MSM). Puedes usar [herramientas de la interfaz gráfica de usuario](https://www.installaware.com/scripting-two-way-integrated-ide.htm) para mantener y mejorar las importaciones, visualmente o mediante la creación de scripts.

Las [opciones de creación de APPX avanzadas](https://www.installaware.com/mhtml5/desktop/appx.htm) te ayudan a tener como objetivo los envíos de Microsoft Store o a producir binarios del paquete de aplicación de Windows con firma para la distribución de transferencia local a los usuarios finales. Incluso puedes compilar paquetes de **WSA**(Windows Server Applications) Installer destinados a las implementaciones en **Nano servidor**, todo ello desde un único origen y con compatibilidad completa con la [automatización de la línea de comandos](https://www.installaware.com/scripting-automation-interface.htm), además de una GUI.

Install**Aware** también [creó en código abierto](https://www.installaware.com/gnu.asp) una **biblioteca de generadores de APPX**, junto con un applet de línea de comandos de ejemplo, bajo la licencia de GNU Affero GPL. Se diseñaron para usarse con plataformas de código abierto como WiX.

#### <a name="installshield"></a>InstallShield

InstallShield proporciona una solución única para desarrollar instaladores MSI y EXE, crear paquetes de Plataforma universal de Windows y aplicación de Windows Server (WSA), y virtualizar aplicaciones con un mínimo de scripting, codificación y rediseño.

<img width="20%" src="images/desktop-to-uwp/InstallShield-logo.jpg">

Examina tu proyecto de InstallShield en cuestión de segundos para ahorrar horas de trabajo de investigación identificando automáticamente los posibles problemas de compatibilidad entre tu aplicación y los paquetes de UWP y WSA.

Prepárate para Microsoft Store y simplifica la experiencia de instalación del software en Windows 10 creando paquetes de aplicación para UWP desde tus proyectos de InstallShield existentes. Compila paquetes tanto de Windows Installer como de aplicación de UWP para admitir los escenarios de implementación que desee de los clientes. Admite implementaciones de Nano Server y Windows Server 2016 compilando paquetes de WSA desde tus proyectos existentes de InstallShield.

Desarrolla tu instalación en módulos para facilitar la implementación y el mantenimiento y, a continuación, combina los componentes y las dependencias en el tiempo de compilación en un único paquete de aplicación para UWP para Microsoft Store. Para la distribución directa fuera de la tienda, agrupa los paquetes de aplicación para UWP y otras dependencias junto con un programa de instalación de la interfaz de usuario Suite o avanzado.

Más información en este [libro electrónico](https://na01.safelinks.protection.outlook.com/?url=https%3A%2F%2Fresources.flexerasoftware.com%2Fweb%2Fpdf%2FeBook-IS-Your-Fast-Track-to-Profit.pdf&data=02%7C01%7Cnormesta%40microsoft.com%7C86b9a00bc8e345c2ac6208d4ba464802%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C1%7C636338258409706554&sdata=IAYNp9nFc8B5ayxwrs%2FQTWowUmOda6p%2Fn%2BjdHea257M%3D&reserved=0).

#### <a name="pace-suite"></a>PACE Suite

[PACE Suite](https://pacesuite.com/) es una herramienta de empaquetado de aplicaciones que puedes usar para llevar tus aplicaciones de escritorio a la Plataforma universal de Windows.

<img width="20%" src="images/desktop-to-uwp/PACE.png">

Con PACE Suite, no necesitas preparar entornos de empaquetado especiales ni instalar componentes de Windows SDK adicionales. PACE Suite puede crear paquetes de aplicación de Windows de forma independiente en tu entorno de empaquetado estándar en Windows 10 o Windows Server 2016. Echa un vistazo a este [ejemplo ilustrado](https://pacesuite.com/convert-exe-to-appx/) para descubrir cómo PACE Suite realiza el empaquetado de un instalador en un paquete de aplicación de Windows.

Además de crear paquetes de aplicación de Windows, también puedes usar PACE Suite para crear paquetes de Windows Installer (MSI), revisiones (MSP), transformaciones (MST) y paquetes App-V. En cuanto a la creación de MSI, PACE Suite ayuda con la administración de actualizaciones, la configuración de permisos, acciones personalizadas, scripts y otros. También puedes publicar tus aplicaciones directamente en System Center Configuration Manager.

Para revisar todas las funcionalidades del empaquetado de aplicaciones, consulta [Características de PACE Suite](https://pacesuite.com/features/).

#### <a name="rad-studio"></a>RAD Studio

Consulta [RAD Studio de Embarcadero](https://www.embarcadero.com/products/rad-studio/windows-10-store-desktop-bridge)

#### <a name="raypack-studio"></a>RayPack Studio

Solución, embalaje de Raynet [RayPack Studio](https://raynet.de/Raynet-Products/RayPackStudio), admite la creación de paquetes para aplicaciones de escritorio como uno de los posibles resultados de conversión eficaz y fácil de configurar y volver a empaquetar framework.

<img width="20%" src="images/desktop-to-uwp/RaynetLogo_v3.png">

Pueden usarse entornos virtuales existentes (Estación de trabajo VMware, Hyper-V) para realizar conversiones automatizadas/en bloque sin configuración de entorno prolongada. Un componente del estudio ([RayQC avanzada](https://raynet.de/Raynet-Products/RayQCad)) es capaz de realizar pruebas de filtrado y compatibilidad de conversión previa para comprobar el software que sea apto para la conversión. Además, los usuarios ya pueden realizar comprobaciones amplias de colisión y compatibilidad con distintas ediciones de Windows 10, incluyendo las actualizaciones de aniversario y Creators.

Además de para la creación de paquetes de software para formato APPX/UWP de Windows 10, RayPack Studio también puede usarse para crear paquetes clásicos de Windows Installer (MSI), revisiones (MSP), transformaciones (MST) y paquetes App-V. Además, esta solución incluye un conjunto de productos y componentes de software para paquetes de software de empresa profesional. Además de los paquetes de software y la virtualización, RayPack Studio considera todas las tareas relacionadas con el empaquetado: comprobaciones de conflictos y compatibilidad de aplicaciones y paquetes de software ([RayQC avanzada](https://raynet.de/Raynet-Products/RayQCad)), evaluación de software ([RayEval](https://raynet.de/Raynet-Products/RayEval)) y control de calidad ([RayQC](https://raynet.de/Raynet-Products/RayQC)).

Combinado con [RayFlow](https://raynet.de/Raynet-Products/RayFlow), el sistema de flujo de trabajo de empresa de Raynet, los usuarios pueden trabajar de forma eficaz en el software a lo largo de todo el ciclo de vida de la aplicación de empresa, desde los pedidos de paquetes y pasando por la evaluación, el análisis, el empaquetado, el aseguramiento de calidad, las pruebas de aceptación de usuario y la implementación. Todos los paquetes y formatos pueden almacenarse e implementarse directamente en SCCM u otras soluciones. El proceso de ciclo de vida de toda la aplicación lo sigue y administra RayFlow. Además, se pueden integrar sistemas de cualquier orden, como ServiceNow. Raynet crea fábricas de empaquetado de software en todo el mundo, con sus herramientas para los proveedores de servicios.

Convéncete tú mismo y consigue la [licencia de prueba gratuita](https://raynet.de/contact?init=license) de RayPack Studio y RayFlow de Raynet . Para obtener más información visita [www.raynet.de](https://raynet.de/home).

**Vínculos relacionados**:

* Raynet: [https://raynet.de/home](https://raynet.de/home)
* RayPack Studio: [https://raynet.de/Raynet-Products/RayPackStudio](https://raynet.de/Raynet-Products/RayPackStudio)
* RayFlow: [https://raynet.de/Raynet-Products/RayFlow](https://raynet.de/Raynet-Products/RayFlow)
* RayEval: [https://raynet.de/Raynet-Products/RayEval](https://raynet.de/Raynet-Products/RayEval)
* RayQC: [https://raynet.de/Raynet-Products/RayQC](https://raynet.de/Raynet-Products/RayQC)
* RayQC Advanced: [https://raynet.de/Raynet-Products/RayQCad](https://raynet.de/Raynet-Products/RayQCad)
* Licencia de prueba gratuita: [https://raynet.de/contact?init=license](https://raynet.de/contact?init=license)

### <a name="manual-packaging"></a>Empaquetado manual

Como última opción, puede convertir la aplicación sin usar ninguna de estas herramientas. Si quieres conocer al detalle la conversión, puedes crear un archivo de manifiesto y ejecutar la herramienta **MakeAppx.exe** para crear el paquete de aplicación de Windows.

Consulte [empaquetar manualmente una aplicación de escritorio](desktop-to-uwp-manual-conversion.md).

## <a name="integrate"></a>Integración

Si la aplicación necesita para integrarse con el sistema (por ejemplo: establecer las reglas de firewall), se describen estas cosas en el manifiesto del paquete de la aplicación y el sistema encargará del resto. Para la mayoría de estas tareas, no tendrás que escribir nada de código. Con un poco de XML en el manifiesto, puede hacer cosas como iniciar un proceso cuando el usuario inicia sesión, integrar su aplicación en el Explorador de archivos y agregar la aplicación una lista de destinos de impresión que aparecen en otras aplicaciones.

Consulte [integrar la aplicación empaquetada de escritorio con Windows 10](desktop-to-uwp-extensions.md).

## <a name="enhance"></a>Mejorar

Una vez empaquetada la aplicación, puedes agregarle características tales como iconos dinámicos y notificaciones de inserción. Algunas de estas capacidades pueden mejorar considerablemente el nivel de participación de la aplicación y costarle muy poco tiempo para agregar. Recuerda que algunas mejoras requieren un poco más código.

Consulta [Mejorar tu aplicación de escritorio para Windows 10](desktop-to-uwp-enhance.md).

## <a name="extend"></a>Extend

Algunas experiencias de Windows 10 (por ejemplo, una página de interfaz de usuario habilitada para entrada táctil) deben ejecutarse dentro de un contenedor de aplicación moderna. En general, primero debes determinar si puedes agregar tu experiencia [mejorando](desktop-to-uwp-enhance.md) la aplicación de escritorio existente con las API de UWP. Si tiene que usar un componente UWP, para lograr la experiencia, puede agregar un proyecto UWP a la solución y utilizar servicios de aplicaciones para la comunicación entre su aplicación de escritorio y el componente UWP.

Consulta [Ampliar tu aplicación de escritorio con componentes de UWP modernos](desktop-to-uwp-extend.md).

## <a name="migrate"></a>Migrar

Aunque no hay ninguna herramienta que pueda convertir una aplicación de escritorio en una aplicación para UWP, puedes reutilizar gran parte de tu código existente, lo que disminuirá el coste de la creación de uno. Puedes hacerlo moviendo tanta lógica de negocios como puedas en las bibliotecas .NET Standard 2.0.

El estándar de .NET 2.0 incluye un gran aumento en el número de las API de .NET junto con una corrección de compatibilidad para tus paquetes NuGet favoritos y las bibliotecas de terceros.

Mueve el código a las bibliotecas de .NET Standard y, a continuación, crea una app de la Plataforma universal de Windows (UWP) para tener acceso a todos los dispositivos de Windows 10.

Consulta [Compartir código entre una aplicación de escritorio y una aplicación para UWP](desktop-to-uwp-migrate.md)


## <a name="test"></a>Prueba

Para probar la aplicación en una configuración realista mientras se prepara para su distribución, es mejor firmar la aplicación y, a continuación, vuelva a instalarlo. Consulta [Probar la aplicación](https://docs.microsoft.com/en-us/windows/uwp/porting/desktop-to-uwp-debug#test-your-app).

>[!IMPORTANT]
> Si va a publicar la aplicación en la Microsoft Store, asegúrese de que la aplicación funciona correctamente en los dispositivos que ejecutan Windows 10 en modo S. Se trata de un requisito de la tienda. Consulta [Probar la aplicación de Windows para Windows 10 en modo S](desktop-to-uwp-test-windows-s.md).

## <a name="validate"></a>Validar

Para proporcionar la mejor oportunidad de ser publicado en la Microsoft Store o se están convirtiendo a su aplicación [Windows Certified](https://go.microsoft.com/fwlink/p/?LinkID=309666), validar y probar localmente antes de enviarla para su certificación.

Si usas la DAC para empaquetar la aplicación, puede usar el nuevo ``-Verify`` marca para validar el paquete en la aplicación empaquetada de escritorio y los requisitos de Store. Consulta [Empaquetar una aplicación, firmarla y prepararla para su envío a la Store](desktop-to-uwp-run-desktop-app-converter.md#optional-parameters).

Si usa Visual Studio, puede validar la aplicación desde el **crear paquetes de aplicaciones** asistente. Consulta [Crear un archivo de carga del paquete de aplicación](../packaging/packaging-uwp-apps.md#create-an-app-package-upload-file).

Para ejecutar la herramienta manualmente, consulta [Kit para la certificación de aplicaciones en Windows](../debug-test-perf/windows-app-certification-kit.md).

Para revisar la lista de pruebas que usa la certificación de aplicaciones de Windows para validar la aplicación, consulta [Windows Desktop Bridge app tests (Pruebas de la aplicación del puente de dispositivo escritorio de Windows)](../debug-test-perf/windows-desktop-bridge-app-tests.md).

## <a name="distribute"></a>Distribuir

Puede distribuir la aplicación mediante la publicación de la Microsoft Store o mediante la instalación de prueba en otros sistemas.

Consulte [distribuir una aplicación de escritorio empaquetada](desktop-to-uwp-distribute.md).

## <a name="support-and-feedback"></a>Soporte técnico y comentarios

**Encuentre respuestas a sus preguntas**

¿Tienes alguna pregunta? Pregúntanos en Stack Overflow. Nuestro equipo supervisa estas [etiquetas](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). También puedes preguntarnos [aquí](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Proporcionar comentarios o hacer sugerencias**

Consulta [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).

## <a name="in-this-section"></a>En esta sección

| Tema | Descripción |
|-------|-------------|
| [Preparar empaquetar una aplicación](desktop-to-uwp-prepare.md) | Proporciona una lista de los elementos que hay que revisar antes de empaquetar la aplicación de escritorio. |
| [Empaquetar una aplicación mediante el Desktop App Converter](desktop-to-uwp-run-desktop-app-converter.md) | Muestra cómo ejecutar Desktop App Converter. |
| [Empaquetar manualmente una aplicación de escritorio](desktop-to-uwp-manual-conversion.md) | Aprende a crear un paquete de la aplicación y un manifiesto manualmente. |
| [Empaquetar una aplicación de escritorio con Visual Studio](desktop-to-uwp-packaging-dot-net.md)| Muestra cómo empaquetar su aplicación de escritorio con Visual Studio. |
| [Integrar su aplicación de escritorio con Windows 10](desktop-to-uwp-extensions.md) | Integrar la aplicación con Windows 10 mediante el uso con la descripción de las tareas en el archivo de manifiesto de paquete del proyecto de empaquetado. |
| [Mejore su aplicación de escritorio para Windows 10](desktop-to-uwp-enhance.md)| Usa las API de UWP para agregar modernas experiencias que destacan para los usuarios de Windows 10. |
| [API de UWP disponibles para una aplicación de escritorio empaquetada](desktop-to-uwp-supported-api.md) | Vea qué están disponibles para la aplicación de escritorio empaquetada para usar las API de UWP. |
| [Ampliar su aplicación de escritorio con componentes UWP modernos](desktop-to-uwp-extend.md)| Agrega experiencias avanzadas que deben ejecutarse dentro de un contenedor de aplicación para UWP. Conectar su aplicación de escritorio con el proceso UWP mediante servicios de aplicación.|
| [Ejecutar, depurar y probar una aplicación de escritorio empaquetada](desktop-to-uwp-debug.md) | Explica las opciones para depurar la aplicación empaquetada. |
| [Distribuir una aplicación de escritorio empaquetada ](desktop-to-uwp-distribute.md) | Vea cómo puede distribuir la aplicación convertida a los usuarios.  |
| [Issues(desktop-to-uwp-known-issues.md) conocidos | Enumera los problemas conocidos con el empaquetado de aplicaciones de escritorio. |
