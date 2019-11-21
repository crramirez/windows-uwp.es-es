---
title: 'Novedades en los documentos de Windows de septiembre de 2017: desarrollar aplicaciones para UWP'
description: Se han agregado nuevas características, vídeos y directrices para los desarrolladores a la documentación de septiembre de 2017 para los desarrolladores de Windows 10.
keywords: novedades, actualización, características, directrices para desarrolladores, Windows 10, 09-17
ms.date: 09/06/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: bf88814e00e81608f698231f31c2f4ab52cd4ea6
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258776"
---
# <a name="whats-new-in-the-windows-developer-docs-in-september-2017"></a>Novedades en la documentación para los desarrolladores de Windows de septiembre de 2017

La documentación para desarrolladores de Windows se actualiza constantemente con información sobre las nuevas características disponibles para los desarrolladores a través de la plataforma de Windows. Las siguientes introducciones generales sobre directrices para los desarrolladores y muestras están disponibles desde hace poco y contienen información nueva y actualizada para los desarrolladores de Windows.

Por supuesto, Fall Creators Update está a la vuelta de la esquina, así que permanezca atento para obtener gran cantidad de documentación que llegará el próximo mes.

[Instala las herramientas y el SDK](https://developer.microsoft.com/windows/downloads#_blank) en Windows 10 y estarás listo para [crear una nueva aplicación universal de Windows](../get-started/your-first-app.md) o para explorar cómo puedes usar tu [código de aplicación existente en Windows](../porting/index.md).

## <a name="features"></a>Características

### <a name="xbox-live-creators-program"></a>Programa de creadores de Xbox Live

El Programa de creadores de Xbox Live ahora es dinámico, lo que le permite crear y publicar juegos para UWP fácilmente que pueden ejecutarse en equipos Windows 10 y consolas Xbox One. Para obtener más información, consulte [Introducción al Programa de creadores de Xbox Live](https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md).

## <a name="developer-guidance"></a>Guía para desarrolladores

### <a name="xaml-basics-tutorials"></a>Tutoriales de conceptos básicos de XAML

Hemos redactado cuatro [tutoriales de conceptos básicos de XAML](https://docs.microsoft.com/windows/uwp/design/basics/xaml-basics-ui) para anexarlos al nuevo [ejemplo PhotoLab](https://github.com/Microsoft/Windows-appsample-photo-lab), que trata cuatro aspectos principales de la programación XAML, interfaces de usuario, enlace de datos, estilos personalizados y diseños adaptativos. Cada pista de tutorial comienza con una versión parcialmente completa del ejemplo PhotoLab y genera paso a paso un componente que falta de la aplicación final. 

![Captura de pantalla del ejemplo PhotoLab que muestra la página de galería de fotos.](images/PhotoLab-gallery-page.png)  

Este es un resumen de los artículos nuevos:

+ [**Crear interfaces de usuario**](https://docs.microsoft.com/windows/uwp/design/basics/xaml-basics-ui) muestra cómo crear la interfaz básica de la galería de fotos.
+ [**Crear enlaces de datos**](https://docs.microsoft.com/windows/uwp/data-binding/xaml-basics-data-binding) muestra cómo agregar enlaces de datos a la galería de fotos, rellenándola con datos de imágenes reales.
+ [**Crear estilos personalizados**](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/xaml-basics-style) muestra cómo agregar estilos personalizados decorativos al menú de edición de fotos.
+ [**Crear diseños adaptativos**](https://docs.microsoft.com/windows/uwp/design/basics/xaml-basics-adaptive-layout) muestra cómo hacer que el diseño de la galería sea adaptable, de manera que quede bien en todos los dispositivos y tamaños de pantalla.

### <a name="get-started-tutorials"></a>Tutoriales de introducción

La sección Introducción de los documentos para UWP se ha actualizado con una [nueva página de inicio para la sección de tutoriales](https://docs.microsoft.com/windows/uwp/get-started/create-uwp-apps). En esta sección se proporciona una nueva y mejorada estructura para la experiencia de Introducción, lo que ayuda a los usuarios a encontrar y usar fácilmente los tutoriales adecuados para ellos, incluidos los tutoriales sobre conceptos básicos de XAML mencionados anteriormente.

### <a name="voice-and-tone"></a>Voz y tono

Hemos agregado nuevas [instrucciones sobre la voz y el tono de las aplicaciones para UWP](https://docs.microsoft.com/windows/uwp/in-app-help/voice-and-tone) para ofrecerle asesoramiento para escribir un texto en la aplicación. Independientemente de lo que cree, es importante que el lenguaje que use sea cercano, sencillo e informativo.

## <a name="samples"></a>Muestras

### <a name="photolab-sample"></a>Ejemplo PhotoLab

El [Ejemplo PhotoLab](https://github.com/Microsoft/windows-appsample-photo-lab) ofrece una experiencia básica de edición de fotos y galería de fotos.

![Captura de pantalla del ejemplo PhotoLab que muestra la página de edición.](images/PhotoLab-editing-page.png)  

### <a name="customer-orders"></a>Pedidos de clientes

El ejemplo [Base de datos de pedidos de clientes](https://github.com/Microsoft/Windows-appsample-customers-orders-database) se ha actualizado para usar los nuevos .NET Core 2.0 y Entity Framework.