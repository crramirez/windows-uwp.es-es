---
ms.assetid: 4b0c86d3-f05b-450b-bf9c-6ab4d3f07d31
description: Esta guía básica proporciona una visión general de las características fundamentales de empresa para las aplicaciones de Windows 10 y la plataforma Universal de Windows (UWP).
title: Enterprise
ms.date: 08/30/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 6cce98591cdaa78a887d7a5fb495e999a4ffc453
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/07/2018
ms.locfileid: "8800050"
---
# <a name="enterprise"></a>Enterprise

En este artículo se proporciona una visión general de las características fundamentales de empresa proporcionados por la plataforma Universal de Windows (UWP) para las aplicaciones de Windows 10.

## <a name="whats-new-and-recent-for-enterprise-applications"></a>Novedades y recientes para aplicaciones de empresa

> [!div class="checklist"]
> * [Windows Template Studio](#template-studio)
> * [Controles para crear interfaces de usuario de estilo de escritorio](#desktop-style-UI)
> * [Controles para admitir escenarios empresariales](#enterprise)
> * [Biblioteca de interfaz de usuario de Windows](#UI-library)
> * [Controles UWP en aplicaciones de escritorio](#xaml-islands)
> * [.NET Standard 2.0](#standard)
> * [Conectividad de SQL Server](#sql-server)
> * [Implementación de MSIX](#MSIX)

Para ver un vídeo que muestra algunas de estas características en detalle, [Rápidamente construir las aplicaciones de LOB con UWP y Visual Studio](https://channel9.msdn.com/Events/Build/2018/BRK3502).

<a id="template-studio" />

### <a name="windows-template-studio"></a>Windows Template Studio

Windows Template Studio es una extensión de Visual Studio 2017 que acelera la creación de nuevas aplicaciones de plataforma Universal de Windows (UWP) con una experiencia basada en el asistente. El proyecto UWP resultante es correcto y legible del código que incorporan las últimas características de Windows 10 durante la implementación de patrones demostrados y procedimientos recomendados.

![Windows Template Studio](images/windows-template-studio.png)

Consulta [Windows Template Studio](https://marketplace.visualstudio.com/items?itemName=WASTeamAccount.WindowsTemplateStudio)

<a id="desktop-style-UI" />

### <a name="controls-to-create-desktop-style-uis"></a>Controles para crear interfaces de usuario de estilo de escritorio

Hemos publicamos nuevos controles de XAML de UWP que rellenar el espacio entre una aplicación de escritorio tradicional la interfaz de usuario y una UI de UWP.

Por ejemplo, los nuevos controles de [barra de menús](https://review.docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/menus?branch=jimwalk%2Frs5-menu-bar), [DropDownButton](https://docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/buttons#create-a-drop-down-button), [botón de división](https://docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/buttons#create-a-split-button)y [CommandBarFlyout](https://review.docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/command-bar-flyout?branch=jimwalk%2Frs5-command-bar-flyout) ofrecen maneras más flexible para exponer comandos y el [EditableComboBox](https://review.docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/combo-box?branch=rs5#make-a-combo-box-editable) vamos al usuario escribe los valores que no se mencionan en una lista predefinida de opciones.

![Barra de menús](images/menu-bar.png)

<a id="enterprise" />

### <a name="controls-to-support-enterprise-scenarios"></a>Controles para admitir escenarios empresariales

[DataGridView](https://docs.microsoft.com/en-us/windows/communitytoolkit/controls/datagrid) proporciona una forma flexible para mostrar una colección de datos en filas y columnas.

La [vista de árbol](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/tree-view) permite una lista jerárquica con la expansión y contracción de nodos que contienen elementos anidados. Puede usarse para ilustrar una estructura de carpetas o relaciones anidadas en la interfaz de usuario.

![Control DataGrid](images/DataGrid.gif)


### <a name="windows-ui-library"></a>Biblioteca de interfaz de usuario de Windows

La biblioteca de la interfaz de usuario de Windows es un conjunto de paquetes de NuGet que proporcionan controles y otros elementos de interfaz de usuario para aplicaciones para UWP. También permite la compatibilidad de nivel inferior con versiones anteriores de Windows 10, por lo que la aplicación funciona incluso si los usuarios no tengan el sistema operativo más reciente.

![Biblioteca de interfaz de usuario de Windows](images/win-ui.png)

Consulta la [Biblioteca de la interfaz de usuario de Windows (versión preliminar)](https://docs.microsoft.com/en-us/uwp/toolkits/winui/).

<a id="xaml-islands" />

### <a name="uwp-controls-in-desktop-applications"></a>Controles UWP en aplicaciones de escritorio

Windows 10 ahora te permite usar los controles UWP en aplicaciones de escritorio de WPF, Windows Forms y Win32 de C++. Esto significa que puede mejorar el aspecto, sensación y la funcionalidad de las aplicaciones de escritorio existentes con las últimas características de la interfaz de usuario de Windows 10 que solo están disponibles a través de los controles UWP, como entrada de lápiz de Windows y los controles que admiten el sistema Fluent Design. Esta característica se denomina Islas XAML.

Ver [los controles UWP en aplicaciones de escritorio](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-host-controls).

<a id="standard" />

### <a name="net-standard-20"></a>.NET Standard 2.0

.NET estándar incluye más de 20 000 más API de .NET estándar 1.x. Esto facilita mucho migrar las bibliotecas de .NET Framework existentes y, a continuación, usarlos entre diferentes aplicaciones. NET, incluida la aplicación para UWP.

![NET estándar](images/dot-net-standard-project-template.png)

Consulta [compartir código entre una aplicación de escritorio y una aplicación para UWP](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-migrate).

<a id="sql-server" />

### <a name="sql-server-connectivity"></a>Conectividad de SQL Server

Tu aplicación puede conectarse directamente a una base de datos de SQL Server y a continuación almacenar y recuperar datos mediante las clases del espacio de nombres [System.Data.SqlClient](https://docs.microsoft.com/dotnet/api/system.data.sqlclient?redirectedfrom=MSDN&view=netframework-4.7.2).

Consulta [Usar una base de datos de SQL Server en una aplicación para UWP](https://docs.microsoft.com/en-us/windows/uwp/data-access/sql-server-databases).

<a id="MSIX" />

### <a name="msix-deployment"></a>Implementación de MSIX

MSIX es el formato de paquete de aplicación de Windows que proporciona una experiencia moderna de empaquetado para todas las aplicaciones de Windows. El formato del paquete MSIX conserva la funcionalidad de los paquetes de aplicación existentes e instalar archivos además de habilitación de características de empaquetado e implementación nuevas, modernas a las aplicaciones de Win32, WPF y Windows Forms.

MSIX es un formato de empaquetado integrado para que sea seguro y fiable, en función de una combinación de MSI, AppX, las tecnologías de instalación de App-V y ClickOnce.

![Icono MSIX](images/MSIX-App-Package.ico)

Consulta la [documentación de MSIX](https://docs.microsoft.com/windows/msix/).

<a id="distribution" />

## <a name="security"></a>Seguridad

Windows 10 proporciona un conjunto de características de seguridad para los desarrolladores de aplicaciones proteger la identidad de sus usuarios, la seguridad de las redes corporativas y todos los datos empresariales almacenados en dispositivos. Para Windows 10 es una característica nueva Microsoft Passport, una alternativa de contraseña de dos factores fácil de implementar que es accesible mediante el uso de un PIN o Windows Hello, que proporciona seguridad de nivel de empresa y admite la huella digital, rostro y reconocimiento de iris en función.

| Tema | Descripción |
|-------|-------------|
| [Introducción al desarrollo seguro de aplicaciones Windows](https://msdn.microsoft.com/library/windows/apps/mt622741) | En este artículo de introducción se explican varias características de seguridad de Windows a través de las fases de autenticación, los datos en desarrollo y los datos en reposo. También se describe cómo integrar esas fases en tus aplicaciones. Trata una amplia variedad de temas y su objetivo principal es que los arquitectos de aplicaciones comprendan mejor las características de Windows que permiten crear aplicaciones de la Plataforma universal de Windows de manera rápida y sencilla. |
| [Autenticación e identidad de usuario](https://msdn.microsoft.com/library/windows/apps/mt270184) | Las aplicaciones para UWP ofrecen varias opciones para la autenticación de usuario, que describimos en este artículo. Para empresas, recomendamos la nueva característica Microsoft Passport. Microsoft Passport reemplaza las contraseñas con autenticación sólida en dos fases (2FA) mediante la comprobación de las credenciales existentes y la creación de una credencial específica protegida por un gesto de usuario basado en datos biométricos o en un PIN; de este modo, ofrece una experiencia cómoda y extremadamente segura. |
| [Criptografía](https://msdn.microsoft.com/library/windows/apps/mt270191) | La sección de criptografía proporciona una visión general de las características de criptografía disponibles para las aplicaciones para UWP. Los artículos incluyen desde tutoriales de introducción sobre cómo cifrar datos empresariales confidenciales de manera sencilla hasta temas avanzados como la manipulación de claves criptográficas y el trabajo con MAC, hash y firmas. |
| [Windows Information Protection (WIP)](wip-hub.md) | En este tema del centro se describe un panorama completo de desarrollador sobre cómo Windows Information Protection (WIP) se relaciona con los archivos, los búferes, el Portapapeles, las redes, las tareas en segundo plano y la protección de datos con la pantalla bloqueada. |

## <a name="data-binding-and-databases"></a>Enlace de datos y bases de datos

El enlace de datos es una manera de que la interfaz de usuario de la aplicación muestre datos de un origen externo, como una base de datos, y opcionalmente, se sincronice con dichos datos. Enlace de datos permite separar la preocupación de los datos de la preocupación de la interfaz de usuario y que da como resultado un modelo conceptual más sencillo y una mejor legibilidad, comprobación y mantenimiento de la aplicación.

| Tema | Descripción |
|-------|-------------|
| [Introducción al enlace de datos](https://msdn.microsoft.com/library/windows/apps/mt269383) | En este tema se muestra cómo enlazar un control (o cualquier otro elemento de interfaz de usuario) a un solo elemento o cómo enlazar un control de elementos a una colección de elementos en una aplicación para la Plataforma universal de Windows (UWP). Además, se muestra cómo controlar la representación de los elementos, implementar una vista de detalles basada en una selección y convertir datos para mostrarlos. |
| [Entity Framework 7 para UWP](https://msdn.microsoft.com/library/windows/apps/mt592863) | Entity Framework 7 (que admite UWP) simplifica enormemente la realización de consultas complejas en grandes conjuntos de datos. En este tutorial, compilarás una aplicación para UWP que realiza el acceso a datos básicos en una base de datos SQLite local mediante Entity Framework. |
| [Base de datos SQLite local](https://channel9.msdn.com/Series/A-Developers-Guide-to-Windows-10/10) | Este vídeo es una guía completa para desarrolladores sobre el uso SQLite, la solución recomendada para las bases de datos locales de aplicaciones. Visita [SQLite](https://www.sqlite.org/download.html) para descargar la versión más reciente de UWP o usa la versión que se proporciona con SDK de Windows 10. |

## <a name="networking-and-data-serialization"></a>Redes y serialización de datos

A menudo, las aplicaciones de línea de negocio necesitan comunicarse con otros sistemas o almacenar datos en ellos. Por lo general, esto se logra si se conecta a un servicio de red (usando protocolos como REST o SOAP) y, a continuación, se serializan o deserializan los datos en un formato común. Trabajar con redes y serialización de datos en aplicaciones para UWP similares a las aplicaciones WPF, WinForms y ASP.NET. Para obtener más información, consulta los siguientes artículos.

| Tema | Descripción |
|-------|-------------|
| [Conceptos básicos de redes](https://msdn.microsoft.com/library/windows/apps/mt280233) | Este tutorial explica los conceptos básicos de redes apropiados para todas las aplicaciones para UWP, independientemente de los protocolos de comunicación en uso.  |
| [¿Qué tecnología de red?](https://msdn.microsoft.com/library/windows/apps/mt280235) | Una introducción rápida a las tecnologías de redes disponibles para las aplicaciones para UWP, con sugerencias sobre cómo elegir las tecnologías más adecuadas para tu aplicación. |
| [Serialización de XML y SOAP](https://msdn.microsoft.com/library/90c86ass.aspx) | La serialización de XML convierte objetos en un flujo XML que se ajusta lenguaje de definición de esquema XML (XSD) específico. Para convertir entre XML y una clase fuertemente tipada, puedes usar la clase nativa [XDocument](https://msdn.microsoft.com/library/system.xml.linq.xdocument.aspx) o una biblioteca externa. |
| [Serialización JSON](https://msdn.microsoft.com/library/windows/apps/br240639) | La serialización de JSON (notación de objetos JavaScript) es un formato popular para la comunicación con las API de REST. [Newtonsoft Json.NET](http://www.newtonsoft.com/json), que es totalmente compatible con aplicaciones para UWP. |

## <a name="devices"></a>Dispositivos

Para poder integrarse con herramientas de línea de negocio como impresoras, escáneres de códigos de barras o lectores de tarjetas inteligentes, es posible que sea necesario integrar sensores o dispositivos externos en la aplicación. Estos son algunos ejemplos de características que se pueden agregar a tu aplicación con la tecnología descrita en esta sección.

| Tema  | Descripción |
|--------|-------------|
| [Enumerar dispositivos](https://msdn.microsoft.com/library/windows/apps/mt187355) | En este artículo se explica cómo usar el espacio de nombres [Windows.Devices.Enumeration](https://msdn.microsoft.com/library/windows/apps/br225459) para buscar dispositivos que están conectados al sistema de forma interna o externa o que se pueden detectar mediante protocolos de redes o de redes inalámbricas. Comienza aquí si vas a crear una aplicación que funciona con dispositivos. |
| [Impresión y digitalización](https://msdn.microsoft.com/library/windows/apps/mt204544) | Describe cómo imprimir y digitalizar desde una aplicación, incluido conectarse y trabajar con dispositivos de empresa como sistemas de punto de venta (POS), impresoras de recibos y escáneres alimentadores de gran capacidad. |
| [Bluetooth](https://msdn.microsoft.com/library/windows/apps/mt270288) | Además de usar las conexiones tradicionales de Bluetooth para enviar y recibir datos o controlar dispositivos, Windows 10 permite el uso del Bluetooth de bajo consumo (BTLE) para enviar o recibir balizas en segundo plano. Usa esto para mostrar las notificaciones o habilitar las funciones cuando un usuario se acerca o aleja de una ubicación en particular. |
| [Almacenamiento compartido de empresa](enterprise-shared-storage.md) | En escenarios de bloqueo de dispositivos, obtén información sobre cómo se pueden compartir datos en la misma aplicación, entre instancias de una aplicación o entre aplicaciones. |

## <a name="device-targeting"></a>Selección de destinos de dispositivo

Hoy en día, muchos usuarios llevan su teléfono o tableta personal al trabajo, que varían en factores de forma y tamaños de pantalla. Gracias a la Plataforma universal de Windows (UWP), puedes escribir una sola aplicación de línea de negocio que se ejecute sin problemas en todos los tipos de dispositivos, incluidos los equipos de escritorio y las pantallas de PPP, lo que permite maximizar el alcance de la aplicación y la eficacia del código.

| Tema | Descripción |
|-------|-------------|
| [Guía de aplicaciones para UWP](https://msdn.microsoft.com/library/windows/apps/dn894631) | En esta guía de introducción, te familiarizarás con la plataforma de Windows 10 UWP, lo que incluye: qué es una familia de dispositivos y cómo decidir cuál seleccionar como destino, controles de interfaz de usuario y paneles nuevos que permiten adaptar la interfaz de usuario a los cambios de factor de forma del dispositivo y, por último, cómo comprender y controlar la superficie de API que está disponible para la aplicación. |
| [Muestra de código de interfaz de usuario XAML adaptable](http://go.microsoft.com/fwlink/p/?LinkId=619992) | Esta muestra de código incluye todas las opciones de diseño y controles posibles para la aplicación, sin considerar el tipo de dispositivo, así mismo, te permite interactuar con los paneles para mostrarte cómo puedes lograr el diseño que buscas. Además de mostrar cómo responde cada control a los diferentes factores de forma, la propia aplicación tiene capacidad de respuesta y muestra los distintos métodos para lograr la interfaz de usuario adaptable. |
| [Tema Xamarin]() | Xamarin para seleccionar el destino de teléfono |

## <a name="deployment"></a>Implementación

Tienes varias opciones para distribuir aplicaciones a los usuarios de la organización. Puedes usar Microsoft Store para empresas, administración de dispositivos móviles existentes, o bien puedes transferir localmente aplicaciones en dispositivos. Puedes también realizar las aplicaciones disponibles en general pública publicándolas en Microsoft Store.

| Tema | Descripción |
|-------|-------------|
| [Distribuir aplicaciones de línea de negocio a empresas](https://msdn.microsoft.com/library/windows/apps/mt608995) | Puedes publicar aplicaciones de línea de negocio directamente en las empresas por volumen a través de Microsoft Store para empresas, sin que las aplicaciones estén disponibles para el público. |
| [Aplicaciones en instalación de prueba](https://technet.microsoft.com/library/mt269549) | Al realizar la instalación de prueba de una aplicación, se implementa un paquete de la aplicación firmado en un dispositivo. Es necesario mantener la firma, el hospedaje y la implementación de estas aplicaciones. Se ha simplificado el proceso de instalación de prueba de aplicaciones para Windows 10.             |
| [Publicar aplicaciones en Microsoft Store](https://dev.windows.com/publish) | La Microsoft Store unificado te permite publicar y administrar todas las aplicaciones para todos los dispositivos de Windows. Personalizar la disponibilidad de la aplicación con el precio de cada mercado, la distribución y visibilidad de los controles y otras opciones. |

## <a name="enterprise-uwp-samples"></a>Muestras de UWP de empresa

| Tema |  Descripción |
|------ |--------------|
| [Muestra de inventario de VanArsdel](https://github.com/Microsoft/InventorySample) | Una aplicación de muestra UWP que presenta escenarios de línea de negocio. La muestra se basa en la creación y administración de clientes, pedidos y productos de la empresa ficticia VanArsdel. |
| [Muestra de base de datos de pedidos de cliente](https://github.com/Microsoft/Windows-appsample-customers-orders-database) | Una aplicación de muestra UWP que presenta características útiles para los desarrolladores empresariales, como la autenticación de Azure Active Directory (AAD), controles (como una cuadrícula de datos) de la interfaz de usuario, integración de base de datos de Sqlite y SQL Azure, Entity Framework y servicios de API en la nube. La muestra se basa en la creación y administración de cuentas de clientes, pedidos y productos de la empresa ficticia Contoso. |

## <a name="patterns-and-practices"></a>Patrones y prácticas

Las bases de código para aplicaciones en el nivel de empresa de gran escala, pueden llegar a ser difíciles de usar. Prism es un marco para compilar imprecisa acopladas, puede mantener y probar aplicaciones de XAML en WPF, Windows 10 UWP y Xamarin Forms. Prism proporciona la implementación de una colección de patrones de diseño útiles para escribir aplicaciones de XAML estructuradas y fáciles de mantener, incluidos MVVM, la inserción de dependencias, los comandos, EventAggregator y otros.

Para obtener más información acerca de Prism, consulta el [repositorio de GitHub](https://github.com/PrismLibrary/Prism).

 

 
