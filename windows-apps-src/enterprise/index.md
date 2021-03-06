---
ms.assetid: 4b0c86d3-f05b-450b-bf9c-6ab4d3f07d31
description: En esta guía básica se proporciona información general sobre las características empresariales fundamentales para las aplicaciones de Windows 10 y de la Plataforma universal de Windows (UWP).
title: Enterprise
ms.date: 01/16/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ff96cd25c32418f518f6b1807e08eb72c3e14c6a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168439"
---
# <a name="enterprise"></a>Enterprise

En este artículo se proporciona información general sobre las características empresariales fundamentales que proporcionan las aplicaciones de la Plataforma universal de Windows (UWP) y Windows 10. Puedes ver un vídeo donde se muestran estas características de forma detallada en [Rapidly Construct LOB Applications with UWP and Visual Studio](https://channel9.msdn.com/Events/Build/2018/BRK3502) (Construir rápidamente aplicaciones de línea de negocio con UWP y Visual Studio).

## <a name="feature-highlights"></a>Características destacadas

<a id="template-studio" />

### <a name="windows-template-studio"></a>Windows Template Studio

Windows Template Studio es una extensión de Visual Studio 2019 que acelera la creación de aplicaciones de la Plataforma universal de Windows (UWP) mediante una experiencia basada en asistentes. El proyecto de UWP resultante es un código con el formato correcto y legible que incorpora las funciones de Windows 10 más recientes y a la vez implementa procedimientos recomendados y patrones demostrados.

![Windows Template Studio](images/windows-template-studio.png)

Consulta [Windows Template Studio](https://marketplace.visualstudio.com/items?itemName=WASTeamAccount.WindowsTemplateStudio)

<a id="desktop-style-UI" />

### <a name="controls-to-create-desktop-style-uis"></a>Controles para crear interfaces de usuario de estilo de escritorio

Hemos publicado nuevos controles de XAML para UWP que vienen a salvar la distancia entre la interfaz de usuario de una aplicación de escritorio tradicional y la interfaz de usuario de UWP.

Por ejemplo, los nuevos controles [MenuBar](../design/controls-and-patterns/menus.md), [DropDownButton](../design/controls-and-patterns/buttons.md#create-a-drop-down-button), [SplitButton](../design/controls-and-patterns/buttons.md#create-a-split-button) y [CommandBarFlyout](../design/controls-and-patterns/command-bar-flyout.md) ofrecen maneras más flexibles de exponer los comandos, y [EditableComboBox](../design/controls-and-patterns/combo-box.md#make-a-combo-box-editable) permite que el usuario especifique valores que no aparecen en una lista predefinida de opciones.

![MenuBar](images/menu-bar.png)

<a id="enterprise" />

### <a name="controls-to-support-enterprise-scenarios"></a>Controles para admitir escenarios empresariales

[DataGridView](/windows/communitytoolkit/controls/datagrid) ofrece una manera flexible de mostrar una colección de datos en filas y columnas.

[TreeView](../design/controls-and-patterns/tree-view.md) permite una lista jerárquica con nodos que se expanden y se contraen, y que contienen elementos anidados. Puede usarse para ilustrar una estructura de carpetas o relaciones anidadas en la interfaz de usuario.

![Control DataGrid](images/DataGrid.gif)


### <a name="windows-ui-library"></a>Biblioteca de interfaz de usuario de Windows

La biblioteca de interfaz de usuario de Windows es un conjunto de paquetes NuGet que proporcionan controles y otros elementos de interfaz de usuario para aplicaciones UWP. También permite la compatibilidad de nivel inferior con versiones anteriores de Windows 10, de modo que la aplicación funciona incluso si los usuarios no tienen el sistema operativo más reciente.

![Biblioteca de interfaz de usuario de Windows](images/win-ui.png)

Consulta [Windows UI Library (Preview version)](/uwp/toolkits/winui/) (Biblioteca de interfaz de usuario de Windows [versión preliminar]).

<a id="xaml-islands" />

### <a name="uwp-controls-in-desktop-applications-xaml-islands"></a>Controles de UWP en aplicaciones de escritorio (islas XAML)

Ahora, Windows 10 permite usar controles de UWP en WPF, Windows Forms y aplicaciones de escritorio de Win32 de C++ mediante una característica denominada *islas XAML*. Esto significa que puedes mejorar el aspecto, la sensación y la funcionalidad de las aplicaciones de escritorio existentes con las características más novedosas de la interfaz de usuario de Windows 10 que solo están disponibles mediante controles UWP, como Windows Ink y controles que admiten Fluent Design System. Esta característica se conoce como islas XAML.

Consulta [Hospedar controles UWP en aplicaciones de escritorio](/windows/apps/desktop/modernize/xaml-islands).

<a id="standard" />

### <a name="net-standard-20"></a>.NET Standard 2.0

.NET Standard incluye más de 20 000 API más que .NET Standard 1.x. Como consecuencia, es mucho más fácil migrar las bibliotecas de .NET Framework existentes y usarlas luego en distintas aplicaciones .NET, incluida tu aplicación para UWP.

![net-standard](images/dot-net-standard-project-template.png)

Consulta [Compartir código entre una aplicación de escritorio y una aplicación para UWP](../porting/desktop-to-uwp-migrate.md).

<a id="sql-server" />

### <a name="sql-server-connectivity"></a>Conectividad de SQL Server

Tu aplicación puede conectarse directamente a una base de datos de SQL Server y a continuación almacenar y recuperar datos mediante clases en el espacio de nombres [System.Data.SqlClient](/dotnet/api/system.data.sqlclient).

Consulta [Usar una base de datos de SQL Server en una aplicación para UWP](../data-access/sql-server-databases.md).

<a id="MSIX" />

### <a name="msix-deployment"></a>Implementación de MSIX

MSIX es un formato de paquete de la aplicación de Windows que combina las mejores características de MSI, .appx, App-V y ClickOnce para proporcionar una experiencia de empaquetado moderna y confiable para todas las aplicaciones de Windows. El formato de paquete MSIX conserva la funcionalidad de los paquetes de la aplicación y de los archivos de instalación existentes, además de permitir modernas características de empaquetado e implementación en Win32, WPF y aplicaciones de Windows Forms. 

![Icono de MSIX](images/MSIX-App-Package.ico)

Consulta la [documentación de MSIX](/windows/msix/).

<a id="distribution" />

## <a name="security"></a>Seguridad

Windows 10 proporciona un conjunto de características de seguridad para desarrolladores de aplicaciones con la finalidad de proteger la identidad de los usuarios, la seguridad de las redes corporativas y todos los datos de la empresa almacenados en dispositivos. Microsoft Passport es una característica nueva en Windows 10, una alternativa a la contraseña de dos factores, que es fácil de implementar y a la que se puede acceder con un PIN o Windows Hello. Proporciona seguridad de nivel de empresa y admite el reconocimiento de huella digital, rostro e iris.

| Tema | Descripción |
|-------|-------------|
| [Introducción al desarrollo seguro de aplicaciones de Windows](../security/intro-to-secure-windows-app-development.md) | En este artículo de introducción se explican varias características de seguridad de Windows a través de las fases de autenticación, los datos en desarrollo y los datos en reposo. También se describe cómo integrar esas fases en tus aplicaciones. Trata una amplia variedad de temas y su objetivo principal es que los arquitectos de aplicaciones comprendan mejor las características de Windows que permiten crear aplicaciones para la Plataforma universal de Windows de manera rápida y sencilla. |
| [Autenticación e identidad de usuario](../security/authentication-and-user-identity.md) | Las aplicaciones para UWP ofrecen varias opciones para la autenticación de usuario, que describimos en este artículo. Para empresas, se recomienda encarecidamente la nueva característica Microsoft Passport. Microsoft Passport reemplaza las contraseñas con autenticación sólida en dos fases (2FA) mediante la comprobación de las credenciales existentes y la creación de una credencial específica protegida por un gesto de usuario basado en datos biométricos o en un PIN; de este modo, ofrece una experiencia cómoda y extremadamente segura. |
| [Criptografía](../security/cryptography.md) | La sección de criptografía proporciona una visión general de las características de criptografía disponibles para las aplicaciones para UWP. Los artículos incluyen desde tutoriales de introducción sobre cómo cifrar datos empresariales confidenciales de manera sencilla hasta temas avanzados como la manipulación de claves criptográficas y el trabajo con MAC, hash y firmas. |
| [Windows Information Protection (WIP)](wip-hub.md) | En este tema del centro se describe un panorama completo de desarrollador sobre cómo Windows Information Protection (WIP) se relaciona con los archivos, los búferes, el Portapapeles, las redes, las tareas en segundo plano y la protección de datos con la pantalla bloqueada. |

## <a name="data-binding-and-databases"></a>Enlace de datos y bases de datos

El enlace de datos es una manera de que la interfaz de usuario de la aplicación muestre datos de un origen externo, como una base de datos, y opcionalmente, se sincronice con dichos datos. El enlace de datos permite separar lo que concierne a los datos de lo que concierne a la interfaz de usuario, lo que da como resultado un modelo conceptual más sencillo y una mejor legibilidad, comprobación y mantenimiento de la aplicación.

| Tema | Descripción |
|-------|-------------|
| [Introducción al enlace de datos](../data-binding/data-binding-quickstart.md) | En este tema se muestra cómo enlazar un control (o cualquier otro elemento de interfaz de usuario) a un solo elemento o cómo enlazar un control de elementos a una colección de elementos en una aplicación para la Plataforma universal de Windows (UWP). Además, se muestra cómo controlar la representación de los elementos, implementar una vista de detalles basada en una selección y convertir datos para mostrarlos. |
| [Entity Framework 7 para UWP](/ef/core/get-started/) | Entity Framework 7 (que admite UWP) simplifica enormemente la realización de consultas complejas en grandes conjuntos de datos. En este tutorial, compilarás una aplicación para UWP que realiza el acceso a datos básicos en una base de datos SQLite local mediante Entity Framework. |
| [Base de datos SQLite local](https://channel9.msdn.com/Series/A-Developers-Guide-to-Windows-10/10) | Este vídeo es una guía completa para desarrolladores sobre el uso de SQLite, la solución recomendada para las bases de datos locales de aplicaciones. Visita [SQLite](https://www.sqlite.org/download.html) para descargar la versión más reciente de UWP o usa la versión que se proporciona con SDK de Windows 10. |

## <a name="networking-and-data-serialization"></a>Redes y serialización de datos

A menudo, las aplicaciones de línea de negocio necesitan comunicarse con otros sistemas o almacenar datos en ellos. Por lo general, esto se logra con la conexión a un servicio de red (usando protocolos como REST o SOAP) y, a continuación, la serialización o deserialización de los datos en un formato común. Trabajar con redes y serialización de datos en aplicaciones para UWP similares a las aplicaciones WPF, WinForms y ASP.NET. Para obtener más información, consulta los siguientes artículos.

| Tema | Descripción |
|-------|-------------|
| [Conceptos básicos de redes](../networking/networking-basics.md) | Este tutorial explica los conceptos básicos de redes apropiados para todas las aplicaciones para UWP, independientemente de los protocolos de comunicación en uso.  |
| [¿Qué tecnología de red?](../networking/which-networking-technology.md) | Una introducción rápida a las tecnologías de redes disponibles para las aplicaciones para UWP, con sugerencias sobre cómo elegir las tecnologías más adecuadas para tu aplicación. |
| [Serialización de SOAP y XML](/dotnet/framework/serialization/xml-and-soap-serialization) | La serialización de XML convierte objetos en un flujo XML que se ajusta a un lenguaje de definición de esquema XML (XSD) específico. Para convertir entre XML y una clase fuertemente tipada, puedes usar la clase nativa [XDocument](/dotnet/api/system.xml.linq.xdocument) o una biblioteca externa. |
| [Serialización de JSON](/uwp/api/Windows.Data.Json) | La serialización de JSON (notación de objetos JavaScript) es un formato popular para la comunicación con las API de REST. [Newtonsoft Json.NET](https://www.newtonsoft.com/json), que es totalmente compatible con aplicaciones para UWP. |

## <a name="devices"></a>Dispositivos

Para poder integrarse con herramientas de línea de negocio como impresoras, escáneres de códigos de barras o lectores de tarjetas inteligentes, es posible que sea necesario integrar sensores o dispositivos externos en la aplicación. Estos son algunos ejemplos de características que se pueden agregar a tu aplicación con la tecnología descrita en esta sección.

| Tema  | Descripción |
|--------|-------------|
| [Enumerar dispositivos](../devices-sensors/enumerate-devices.md) | En este artículo se explica cómo usar el espacio de nombres [Windows.Devices.Enumeration](/uwp/api/Windows.Devices.Enumeration) para buscar dispositivos que estén conectados al sistema de forma interna o externa o que se puedan detectar mediante protocolos de redes o de redes inalámbricas. Comienza aquí si vas a crear una aplicación que funcione con dispositivos. |
| [Impresión y digitalización](../devices-sensors/printing-and-scanning.md) | Describe cómo imprimir y digitalizar desde una aplicación, incluido conectarse y trabajar con dispositivos de empresa como sistemas de punto de venta (POS), impresoras de recibos y escáneres con alimentador de gran capacidad. |
| [Bluetooth](../devices-sensors/bluetooth.md) | Además de usar las conexiones tradicionales de Bluetooth para enviar y recibir datos o controlar dispositivos, Windows 10 permite el uso de Bluetooth de bajo consumo (BTLE) para enviar o recibir balizas en segundo plano. Úsalo para mostrar notificaciones o habilitar las funciones cuando un usuario se acerque a una ubicación específica o se aleje de esta. |
| [Almacenamiento compartido de empresa](enterprise-shared-storage.md) | En escenarios de bloqueo de dispositivos, obtén información sobre cómo se pueden compartir datos en la misma aplicación, entre instancias de una aplicación o entre aplicaciones. |

## <a name="device-targeting"></a>Selección de destinos de dispositivo

Hoy en día, muchos usuarios llevan su teléfono o tableta personal al trabajo, que varían en factores de forma y tamaños de pantalla. Gracias a la Plataforma universal de Windows (UWP), puedes escribir una sola aplicación de línea de negocio que se ejecute sin problemas en todos los tipos de dispositivos, incluidos los equipos de escritorio y las pantallas de PPP, lo que permite maximizar el alcance de la aplicación y la eficacia del código.

| Tema | Descripción |
|-------|-------------|
| [Guía de aplicaciones para UWP](../get-started/universal-application-platform-guide.md) | En esta guía de introducción, te familiarizarás con la plataforma UWP de Windows 10, lo que incluye: qué es una familia de dispositivos y cómo decidir cuál seleccionar como destino, controles de interfaz de usuario y paneles nuevos que permiten adaptar la interfaz de usuario a dispositivos con distintos factores de forma y, por último, cómo comprender y controlar la superficie de la API que está disponible para la aplicación. |
| [Muestra de código de interfaz de usuario XAML adaptable](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) | Este ejemplo de código incluye todos los controles y opciones de diseño posibles para la aplicación, sin considerar el tipo de dispositivo, y te permite interactuar con los paneles para mostrarte cómo puedes lograr el diseño que buscas. Además de mostrar cómo responde cada control a los diferentes factores de forma, la propia aplicación tiene capacidad de respuesta y muestra los distintos métodos para lograr una interfaz de usuario adaptable. |
| [Tema de Xamarin](/xamarin/) | Xamarin para la selección de destino de teléfono |

## <a name="deployment"></a>Implementación

Tienes varias opciones para distribuir aplicaciones a los usuarios de la organización mediante paquetes MSIX. Puedes configurar una implementación basada en el Instalador de la aplicación, usar herramientas de administración de dispositivos como Microsoft Endpoint Configuration Manager y Microsoft Intune, publicar en Microsoft Store para Empresas o transferir localmente aplicaciones a dispositivos. También puedes poner tus aplicaciones a disposición del público general publicándolas en Microsoft Store.

| Tema | Descripción |
|-------|-------------|
| [Documentación de MSIX](/windows/msix/) | MSIX es un formato de paquete de la aplicación de Windows que combina las mejores características de MSI, .appx, App-V y ClickOnce para proporcionar una experiencia de empaquetado moderna y confiable. |
| [Distribuir aplicaciones de línea de negocio a empresas](../publish/distribute-lob-apps-to-enterprises.md) | Descubre las opciones para distribuir aplicaciones de línea de negocio sin que las aplicaciones estén disponibles en general para el público, incluida la implementación basada en el Instalador de la aplicación, Microsoft Endpoint Configuration Manager y Microsoft Intune, y publicar en Microsoft Store para Empresas. |
| [Instalación de prueba de aplicaciones](/windows/deploy/sideload-apps-in-windows-10) | Al realizar la instalación de prueba de una aplicación, se implementa un paquete de la aplicación firmado en un dispositivo. Es necesario mantener la firma, el hospedaje y la implementación de estas aplicaciones. Se ha simplificado el proceso de instalación de prueba de aplicaciones para Windows 10.             |
| [Publicar aplicaciones en Microsoft Store](https://developer.microsoft.com/store/publish-apps) | El panel unificado de Microsoft Store te permite publicar y administrar todas las aplicaciones de todos los dispositivos Windows. Personaliza la disponibilidad de la aplicación con el precio de cada mercado, los controles de distribución y visibilidad, y otras opciones. |

## <a name="enterprise-uwp-samples"></a>Ejemplos de UWP de empresa

| Tema |  Descripción |
|------ |--------------|
| [Ejemplo de inventario de VanArsdel](https://github.com/Microsoft/InventorySample) | Una aplicación de ejemplo para UWP que muestra escenarios de línea de negocio. El ejemplo gira en torno a la creación y administración de clientes, pedidos y productos para la empresa ficticia VanArsdel. |
| [Ejemplo de base de datos de pedidos de clientes](https://github.com/Microsoft/Windows-appsample-customers-orders-database) | Una aplicación de ejemplo para UWP que muestra características útiles para los desarrolladores empresariales, como autenticación de Azure Active Directory (AAD), controles de interfaz de usuario (incluida una cuadrícula de datos), integración de bases de datos de Sqlite y SQL Azure, Entity Framework y servicios de API en la nube. El ejemplo gira en torno a la creación y administración de cuentas de cliente, pedidos y productos para la empresa ficticia Contoso. |

## <a name="patterns-and-practices"></a>Patrones y prácticas

Las bases de código para aplicaciones empresariales a gran escala pueden ser difíciles de usar. Prism es un marco para la creación de aplicaciones de XAML acopladas ligeramente que se pueden mantener y probar en WPF, Windows 10, UWP y Xamarin Forms. Prism proporciona la implementación de una colección de patrones de diseño útiles para escribir aplicaciones XAML estructuradas y fáciles de mantener, que incluyen MVVM, inserción de dependencias, comandos, EventAggregator etc.

Para obtener más información acerca de Prism, consulta el [repositorio de GitHub](https://github.com/PrismLibrary/Prism).

 

 