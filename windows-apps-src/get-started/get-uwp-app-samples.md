---
title: Obtener muestras de aplicaciones para UWP
description: Aprende a descargar ejemplos de código de UWP de GitHub.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, código de ejemplo, ejemplos de código
ms.assetid: 393c5a81-ee14-45e7-acd7-495e5d916909
ms.localizationpriority: medium
ms.openlocfilehash: ac3c99bc364e81386a362f1d1b5530bee9d462c4
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259512"
---
# <a name="get-uwp-app-samples"></a>Obtener muestras de aplicaciones para UWP

Las aplicaciones de ejemplo para Plataforma Universal de Windows (UWP) están disponibles en los repositorios de GitHub. Consulta [Muestras de código](https://developer.microsoft.com/windows/samples) para ver una lista categorizada que permite búsquedas, o bien puedes examinar el repositorio [Microsoft/Windows-universal-samples](https://github.com/Microsoft/Windows-universal-samples "Muestras de aplicaciones para la Plataforma universal de Windows del repositorio de GitHub"). El repositorio Windows-universal-samples contiene ejemplos que muestran todas las características de UWP y los patrones de uso de sus API.

![Repositorio de ejemplos de UWP de GitHub](images/GitHubUWPSamplesPage.png)

## <a name="download-the-code"></a>Descarga del código

Para descargar los ejemplos, ve al [repositorio](https://github.com/Microsoft/Windows-universal-samples "Muestras de aplicaciones para la Plataforma universal de Windows del repositorio de GitHub"). Selecciona **Clone or download o** (Clonar o descargar) y, después, selecciona **Download ZIP** (Descargar ZIP). 

![Descarga de ejemplos](images/SamplesDownloadButton.png)

También puedes [descargar los ejemplos](https://github.com/Microsoft/Windows-universal-samples/archive/master.zip "Descarga de archivos ZIP de muestras de aplicaciones para la Plataforma universal de Windows") de este artículo.

Los ejemplos del archivo. zip siempre son los más recientes. Para descargar el archivo no se necesita una cuenta de GitHub. Si se publica una actualización del SDK o si quieres tener los cambios o incorporaciones más recientes, descargar el último archivo zip.

> [!NOTE]
> Para abrir, compilar y ejecutar los ejemplos de UWP, es preciso tener Visual Studio 2015, o cualquier versión posterior, y Windows SDK. Puedes obtener una [copia gratuita de Visual Studio Community](https://www.microsoft.com/?ref=go). Visual Studio Community es compatible con la compilación de aplicaciones para UWP.  
>
> Para que los ejemplos funcionen correctamente, asegúrate de descomprimir todo el archivo, no solo algunos ejemplos. Todas las muestras dependen de la carpeta SharedContent del archivo. Los ejemplos de características de UWP usan archivos vinculados de Visual Studio para reducir la duplicación de archivos comunes, incluidos los archivos de plantillas de muestra y activos de imagen. Los archivos comunes se almacenan en la carpeta SharedContent, que se encuentra en la raíz del repositorio. Los vínculos se usan en los archivos de proyecto para hacer referencia a archivos comunes.
> 

## <a name="open-the-samples"></a>Apertura de los ejemplos

Cuando hayas descargado el archivo zip, abre los ejemplos en Visual Studio.

1.  Antes de descomprimir el archivo, haz clic en él con el botón derecho del ratón y selecciona **Propiedades** > **Desbloquear** > **Aplicar**. Luego, descomprime el archivo en una carpeta local del equipo.

    ![Archivos descomprimido](images/SamplesUnzip1.png)
2.  Todas las subcarpetas de la carpeta Samples contienen un ejemplo de característica de UWP.

    ![Carpetas de muestra](images/SamplesUnzip2.png)
3.  Seleccione un ejemplo, como Altimeter. Las subcarpetas indican los idiomas que se admiten.

    ![Carpetas de idioma](images/SamplesUnzip3.png)
4.  Selecciona la carpeta del idioma que quieres usar. En el contenido de la carpeta, verás un archivo de solución de Visual Studio (. sln) que se puede abrir en Visual Studio. Por ejemplo, *Altimeter. sln*.

    ![Solución VS](images/SamplesUnzip4.png)

## <a name="give-feedback-ask-questions-and-report-issues"></a>Envío de comentarios, formulación de preguntas e informes de problemas

Si tienes cualquier problema o pregunta, usa la pestaña **Issues** (Problemas) del repositorio para crear un nuevo informe de problema. Haremos lo que podamos para ayudarle.

![Imagen de comentarios](images/GitHubUWPSamplesFeedback.png)
