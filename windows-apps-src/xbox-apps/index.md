---
author: Mtoepke
title: UWP en Xbox One
description: Cómo crear aplicaciones de la Plataforma universal de Windows (UWP) en Xbox One.
ms.author: mstahl
ms.date: 10/25/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: 2d935f53-84db-4108-86dc-cb6a0749782f
ms.localizationpriority: medium
ms.openlocfilehash: e7c14c51c0b5b09b96859a8aba97172485a14c85
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2018
ms.locfileid: "6838213"
---
# <a name="uwp-on-xbox-one"></a>UWP en Xbox One

Introducción a la creación de una aplicación para la Plataforma universal de Windows (UWP) en Xbox One.

UWP en Xbox One admite el desarrollo de aplicaciones y juegos. No hace falta formar parte de un programa para desarrolladores para experimentar, crear y probar juegos o aplicaciones en Xbox. Todo lo que necesitas es una [cuenta de desarrollador](https://developer.microsoft.com/en-us/store/register) en [El centro de partners](https://partner.microsoft.com/dashboard). Cuando estés listo para publicar y vender juegos en Xbox One o aprovechar las ventajas de Xbox Live en Windows 10, tendrás que unirte al [Programa de creadores de Xbox Live](https://developer.microsoft.com/games/xbox/xboxlive/creator) o ser un desarrollador [ID@Xbox](http://www.xbox.com/Developers/id). Si estás planeando ser un desarrollador ID@Xbox, recomendamos enviar, en primer lugar, una solicitud al programa antes de registrarte para conseguir una cuenta de desarrollador. Para más información, consulta [Introducción al programa para desarrolladores](../xbox-live/developer-program-overview.md).

Esta sección incluye pasos de configuración, una guía por el proceso de autenticación, información sobre la instalación de las versiones de las herramientas de Visual Studio y Windows 10 necesarias y, finalmente, los pasos para configurar, compilar, ejecutar y depurar tu primera aplicación sencilla. 

| Tema      | Descripción |
|------------|-------------|
|[Introducción](getting-started.md)| Guía de introducción al desarrollo de UWP en Xbox One. |
|[Novedades](whats-new.md)| Destaca las nuevas características de UWP en Xbox One. |
|[Activación del modo de desarrollador de Xbox One](devkit-activation.md)| Explica cómo habilitar el modo de desarrollador en Xbox One. |
|[Deshabilitar el modo de desarrollador en Xbox One](devkit-deactivation.md)| Explica cómo deshabilitar el modo de desarrollador en Xbox One. |
|[Configurar el entorno de desarrollo de UWP en Xbox](development-environment-setup.md)| Describe los pasos para configurar y probar el entorno de desarrollo de Xbox One. |
|[Muestras](samples.md)| Puntero a la ubicación de GitHub (TVHelpers) donde encontrarás muestras útiles de XAML y JavaScript para comenzar a desarrollar para Xbox. Las muestras incluyen una plantilla completa de la aplicación de multimedia XAML, así como la navegación por controlador automática, la reproducción de elementos multimedia enriquecidos y la búsqueda de tecnologías basadas en web. |
|[Problemas conocidos](known-issues.md)| Problemas conocidos de UWP en Xbox One. |
|[Preguntas más frecuentes](frequently-asked-questions.md)| Preguntas más frecuentes relacionadas con UWP en Xbox One. |
|[Herramientas](introduction-to-xbox-tools.md)| Describe _Dev Home_, la herramienta específica para Xbox One, cómo usar Windows Device Portal y cómo configurar Visual Studio para el desarrollo. Esta sección también orienta a un desarrollador principiante durante su primera aplicación para UWP de Xbox y explica cómo usar la herramienta Fiddler para ver el tráfico de red. |
| [Desarrollo de aplicaciones en evento de Xbox](https://developer.microsoft.com/windows/projects/campaigns/app-dev-on-xbox-event) | El desarrollo de aplicaciones en evento de Xbox es un fantástico punto de partida para los desarrolladores nuevos en la creación de aplicaciones en Xbox. Mira las sesiones grabadas y lee las entradas de blog desde el evento. |
|[Diseñar para Xbox y televisión](../design/devices/designing-for-tv.md)| Describe los procedimientos recomendados para diseñar una aplicación que se verá en un televisor y usará un controlador para la entrada. |
|[Procedimientos recomendados de Xbox](tailoring-for-xbox.md)| Cómo desactivar el modo de mouse, dibujar los bordes de la pantalla y deshabilitar el ajuste de escala. |
|[Usar la voz para invocar elementos de la interfaz de usuario](ves-on-xbox.md)| Describe los procedimientos recomendados para admitir el shell de voz habilitado en aplicaciones para UWP en Xbox. |
|[Recursos del sistema de aplicaciones para UWP y juegos en Xbox One](system-resource-allocation.md)| Describe los recursos disponibles para la aplicación cuando se ejecuta en Xbox One. |
|[Introducción a las aplicaciones multiusuario](multi-user-applications.md)| Describe las aplicaciones multiusuario en Xbox One. |
| [Automatizar tareas de desarrollo de Xbox One](https://github.com/Microsoft/WindowsDevicePortalWrapper/tree/v0.9.4) | El proyecto WindowsDevicePortalWrapper en GitHub proporciona una biblioteca que te permite automatizar tareas comunes de desarrollo, como la implementación o el inicio de una aplicación. El proyecto incluye un ejemplo, XboxWdpDriver.exe, que muestra cómo usar las API para tareas comunes. |
|[Llevar los juegos existentes a Xbox](development-lanes-landing.md)|En función de en qué tecnología se base el juego, podemos dirigirte a instrucciones paso a paso que pueden acelerar el proceso de incorporación de tu juego a Xbox mediante UWP.|
|[Características UWP que aún no se admiten en Xbox One](http://go.microsoft.com/fwlink/p/?LinkId=760755)|  Describe las áreas de características de UWP que aún no son totalmente funcionales en Xbox One.|

## <a name="videos"></a>Vídeos

Las siguientes conversaciones que encontrarás en Channel 9 te serán de gran ayuda para crear aplicaciones sorprendentes en Xbox:

* [Building Great Universal Windows Platform (UWP) Apps for Xbox (Crear excelentes aplicaciones para la Plataforma universal de Windows para Xbox)](https://channel9.msdn.com/Events/Build/2016/B883)
* [Adapt Your App for Xbox One and TV (Adapta tu aplicación para Xbox One y el televisor)](https://channel9.msdn.com/Events/Build/2016/T651-R1)
* [UWP Development 1: Building an Adaptive UI (Desarrollo UWP 1: crear una interfaz de usuario adaptable)](https://channel9.msdn.com/Events/Build/2016/L724-R1)
* [Web Apps Beyond the Browser: Cross-Platform Meets Cross Device (Aplicaciones web más allá del navegador: opción multiplataforma para todos los dispositivos)](https://channel9.msdn.com/Events/Build/2016/B888)

## <a name="see-also"></a>Ver también

- [Automatizar el inicio de aplicaciones de Windows 10 para UWP](automate-launching-uwp-apps.md)
- [CPUSets para el desarrollo de juegos](cpusets-games.md)