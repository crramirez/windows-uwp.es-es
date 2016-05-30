---
ms.assetid: 4b0c86d3-f05b-450b-bf9c-6ab4d3f07d31
description: Esta guía básica proporciona una visión general de las características fundamentales de empresa para la Plataforma universal de Windows (UWP) de Windows 10.
title: Empresa
author: awkoren
---

# Empresa


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Esta guía básica proporciona una visión general de las características fundamentales de empresa para la Plataforma universal de Windows (UWP) Windows 10. Windows 10 te permite escribir una vez e implementa en todos los dispositivos, porque crea una aplicación que se adapta a cualquier dispositivo. Esto te permite crear las fantásticas experiencias que los usuarios esperan y ofrecer control sobre la seguridad, la administración y la configuración requerida por tu organización.

**Nota**  Este artículo está dirigido a los desarrolladores que escriben aplicaciones de empresa para UWP. Para el desarrollo general de UWP, consulta las [Guías de procedimientos para aplicaciones Windows 10](https://msdn.microsoft.com/library/windows/apps/mt244352). Para el desarrollo de WPF, Windows Forms o Win32, ve al [Centro de desarrollo de escritorio](https://dev.windows.com/en-us/desktop). Para los recursos de profesionales de TI como la implementación de Windows 10 o la administración de características de seguridad de la empresa, consulta [Windows 10 en TechNet](https://msdn.microsoft.com/library/dn986868).

 

## Seguridad


Windows 10 proporciona un conjunto de características de seguridad para desarrolladores de aplicaciones con el fin de que protejan la identidad de los usuarios, la seguridad de las redes corporativas y todos los datos de la empresa almacenados en dispositivos. Microsoft Passport es una característica nueva en Windows 10, una alternativa de contraseña de dos factores fácil de implementar que se puede acceder mediante el uso de un PIN o Windows Hello. Proporciona una seguridad en el nivel de empresa y admite el reconocimiento de huella digital, rostro e iris.

| Tema | Descripción |
|-------|-------------|
| [Introducción al desarrollo seguro de aplicaciones Windows](https://msdn.microsoft.com/library/windows/apps/mt622741) | En este artículo de introducción se explican varias características de seguridad de Windows a través de las fases de autenticación, los datos en desarrollo y los datos en reposo. También se describe cómo integrar esas fases en tus aplicaciones. Trata una amplia variedad de temas y su objetivo principal es que los arquitectos de aplicaciones comprendan mejor las características de Windows que permiten crear aplicaciones de la Plataforma universal de Windows de manera rápida y sencilla. |
| [Autenticación e identidad de usuario](https://msdn.microsoft.com/library/windows/apps/mt270184) | Las aplicaciones para UWP ofrecen varias opciones para la autenticación de usuario, que describimos en este artículo. Para empresas, recomendamos la nueva característica Microsoft Passport. Microsoft Passport reemplaza las contraseñas con autenticación sólida en dos fases (2FA) mediante la comprobación de las credenciales existentes y la creación de una credencial específica protegida por un gesto de usuario basado en datos biométricos o en un PIN; de este modo, ofrece una experiencia cómoda y extremadamente segura. |
| [Criptografía](https://msdn.microsoft.com/library/windows/apps/mt270191) | La sección de criptografía proporciona una visión general de las características de criptografía disponibles para las aplicaciones para UWP. Los artículos incluyen desde tutoriales de introducción sobre cómo cifrar datos empresariales confidenciales de manera sencilla hasta temas avanzados como la manipulación de claves criptográficas y el trabajo con MAC, hash y firmas. |
| [Protección de datos de empresa (EDP)](edp-hub.md) | En este tema del centro se describe un panorama completo de desarrollador sobre cómo Protección de datos de empresa (EDP) se relaciona con los archivos, los búferes, el Portapapeles, las redes, las tareas en segundo plano y la protección de datos con la pantalla bloqueada. |

 

## Enlace de datos y bases de datos


El enlace de datos es una manera de que la interfaz de usuario de la aplicación muestre datos de un origen externo, como una base de datos, y opcionalmente, se sincronice con dichos datos. El enlace de datos permite separar lo que concierne a los datos de lo que concierne a la interfaz de usuario y esto da como resultado un modelo conceptual más sencillo y una mejor legibilidad, comprobación y mantenimiento de la aplicación.

| Tema | Descripción |
|-------|-------------|
| [Introducción al enlace de datos](https://msdn.microsoft.com/library/windows/apps/mt269383) | En este tema se muestra cómo enlazar un control (o cualquier otro elemento de interfaz de usuario) a un solo elemento o cómo enlazar un control de elementos a una colección de elementos en una aplicación para la Plataforma universal de Windows (UWP). Además, se muestra cómo controlar la representación de los elementos, implementar una vista de detalles basada en una selección y convertir datos para mostrarlos. |
| [Entity Framework 7 para UWP](https://msdn.microsoft.com/library/windows/apps/mt592863) | Entity Framework 7 (que admite UWP) simplifica enormemente la realización de consultas complejas en grandes conjuntos de datos. En este tutorial, compilarás una aplicación para UWP que realiza el acceso a datos básicos en una base de datos SQLite local mediante Entity Framework. |
| [Base de datos SQLite local](https://channel9.msdn.com/Series/A-Developers-Guide-to-Windows-10/10) | Este vídeo es una guía completa para desarrolladores sobre el uso SQLite, la solución recomendada para las bases de datos locales de aplicaciones. Visita [SQLite](https://www.sqlite.org/download.html) para descargar la versión más reciente de UWP o usa la versión que se proporciona con SDK de Windows 10. |

 

## Redes y serialización de datos


A menudo, las aplicaciones de línea de negocio necesitan comunicarse con otros sistemas o almacenar datos en ellos. Por lo general, esto se logra si se conecta a un servicio de red (usando protocolos como REST o SOAP) y, a continuación, se serializan o deserializan los datos en un formato común. Trabajar con redes y serialización de datos en aplicaciones para UWP similares a las aplicaciones WPF, WinForms y ASP.NET. Para obtener más información, consulta los siguientes artículos.

| Tema | Descripción |
|-------|-------------|
| [Conceptos básicos de redes](https://msdn.microsoft.com/library/windows/apps/mt280233) | Este tutorial explica los conceptos básicos de redes apropiados para todas las aplicaciones para UWP, independientemente de los protocolos de comunicación en uso.  |
| [¿Qué tecnología de red?](https://msdn.microsoft.com/library/windows/apps/mt280235) | Una introducción rápida a las tecnologías de redes disponibles para las aplicaciones para UWP, con sugerencias sobre cómo elegir las tecnologías más adecuadas para tu aplicación. |
| [Serialización de XML y SOAP](https://msdn.microsoft.com/library/90c86ass.aspx) | La serialización de XML convierte objetos en un flujo XML que se ajusta lenguaje de definición de esquema XML (XSD) específico. Para convertir entre XML y una clase fuertemente tipada, puedes usar la clase nativa [XDocument](https://msdn.microsoft.com/library/system.xml.linq.xdocument.aspx) o una biblioteca externa. |
| [Serialización JSON](https://msdn.microsoft.com/library/windows/apps/br240639) | La serialización de JSON (notación de objetos JavaScript) es un formato popular para la comunicación con las API de REST. [Newtonsoft Json.NET](http://www.newtonsoft.com/json), que es totalmente compatible con aplicaciones para UWP. |

 

## Dispositivos


Para poder integrarse con herramientas de línea de negocio como impresoras, escáneres de códigos de barras o lectores de tarjetas inteligentes, es posible que sea necesario integrar sensores o dispositivos externos en la aplicación. Estos son algunos ejemplos de características que se pueden agregar a tu aplicación con la tecnología descrita en esta sección.

| Tema  | Descripción |
|--------|-------------|
| [Enumerar dispositivos](https://msdn.microsoft.com/library/windows/apps/mt187355) | En este artículo se explica cómo usar el espacio de nombres [Windows.Devices.Enumeration](https://msdn.microsoft.com/library/windows/apps/br225459) para buscar dispositivos que están conectados al sistema de forma interna o externa o que se pueden detectar mediante protocolos de redes o de redes inalámbricas. Comienza aquí si vas a crear una aplicación que funciona con dispositivos. |
| [Impresión y digitalización](https://msdn.microsoft.com/library/windows/apps/mt204544) | Describe cómo imprimir y digitalizar desde una aplicación, incluido conectarse y trabajar con dispositivos de empresa como sistemas de punto de venta (POS), impresoras de recibos y escáneres alimentadores de gran capacidad. |
| [Bluetooth](https://msdn.microsoft.com/library/windows/apps/mt270288) | Además de usar las conexiones tradicionales de Bluetooth para enviar y recibir datos o controlar dispositivos, Windows 10 permite el uso del Bluetooth de bajo consumo (BTLE) para enviar o recibir balizas en segundo plano. Usa esto para mostrar las notificaciones o habilitar las funciones cuando un usuario se acerca o aleja de una ubicación en particular. |
| [Almacenamiento compartido de empresa](enterprise-shared-storage.md) | En escenarios de bloqueo de dispositivos, obtén información sobre cómo se pueden compartir datos en la misma aplicación, entre instancias de una aplicación o entre aplicaciones. |

 

## Selección de destinos de dispositivo


Hoy en día, muchos usuarios llevan su teléfono o tableta personal al trabajo, que varían en factores de forma y tamaños de pantalla. Gracias a la Plataforma universal de Windows (UWP), puedes escribir una sola aplicación de línea de negocio que se ejecute sin problemas en todos los tipos de dispositivos, incluidos los equipos de escritorio y las pantallas de PPP, lo que permite maximizar el alcance de la aplicación y la eficacia del código.

| Tema | Descripción |
|-------|-------------|
| [Guía de aplicaciones para UWP](https://msdn.microsoft.com/library/windows/apps/dn894631) | En esta guía de introducción, te familiarizarás con la plataforma de Windows 10 UWP, lo que incluye: qué es una familia de dispositivos y cómo decidir cuál seleccionar como destino, controles de interfaz de usuario y paneles nuevos que permiten adaptar la interfaz de usuario a los cambios de factor de forma del dispositivo y, por último, cómo comprender y controlar la superficie de API que está disponible para la aplicación. |
| [Muestra de código de interfaz de usuario XAML adaptable](http://go.microsoft.com/fwlink/p/?LinkId=619992) | Esta muestra de código incluye todas las opciones de diseño y controles posibles para la aplicación, sin considerar el tipo de dispositivo, así mismo, te permite interactuar con los paneles para mostrarte cómo puedes lograr el diseño que buscas. Además de mostrar cómo responde cada control a los diferentes factores de forma, la propia aplicación tiene capacidad de respuesta y muestra los distintos métodos para lograr la interfaz de usuario adaptable. |

 

## Implementación


Tienes varias opciones para distribuir aplicaciones a los usuarios de la organización. Puedes usar la Tienda Windows para empresas, la administración de dispositivos móviles existentes, o bien puedes transferir las aplicaciones a los dispositivos localmente. También puedes poner tus aplicaciones a disposición del público general publicándolas en la Tienda Windows.

| Tema | Descripción |
|-------|-------------|
| [Distribuir aplicaciones de línea de negocio a empresas](https://msdn.microsoft.com/library/windows/apps/mt608995) | Puedes publicar aplicaciones de línea de negocio (LOB) directamente para que las empresas las compren por volumen a través de la Tienda Windows para empresas, sin necesidad de que las aplicaciones estén disponibles en la Tienda de forma general. |
| [Instalación de prueba de aplicaciones](https://technet.microsoft.com/en-us/library/mt269549) | Al realizar la instalación de prueba de una aplicación, se implementa un paquete de la aplicación firmado en un dispositivo. Es necesario mantener la firma, el hospedaje y la implementación de estas aplicaciones. Se ha simplificado el proceso de instalación de prueba de aplicaciones para Windows 10.             |
| [Publicar aplicaciones en la Tienda Windows](https://dev.windows.com/publish) | El panel de la Tienda Windows unificado te permite publicar y administrar todas las aplicaciones para todos los dispositivos Windows. Personalizar la disponibilidad de la aplicación con el precio de cada mercado, la distribución y visibilidad de los controles y otras opciones. |

 

## Patrones y prácticas


Las bases de código para aplicaciones en el nivel de empresa de gran escala, pueden llegar a ser difíciles de usar. Prism es un marco para compilar aplicaciones de XAML acopladas ligeramente que se pueden mantener y probar en WPF, Windows 10 UWP y Xamarin Forms. Prism proporciona la implementación de una colección de patrones de diseño útiles para escribir aplicaciones de XAML estructuradas y fáciles de mantener, incluidos MVVM, la inserción de dependencias, los comandos, EventAggregator y otros.

Para obtener más información acerca de Prism, consulta el [repositorio de GitHub](https://github.com/PrismLibrary/Prism).

 

 





<!--HONumber=May16_HO2-->


