---
title: Obtener muestras de aplicaciones para UWP
description: Aprende a descargar las muestras de código de UWP desde GitHub.
author: JoshuaPartlow
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, Windows 10, uwp, UWP, sample code, código de muestra, code samples, muestras de código
ms.assetid: 393c5a81-ee14-45e7-acd7-495e5d916909
ms.localizationpriority: medium
ms.openlocfilehash: ef8f99ade3fa5e4d9f12b8670bf22242e7c4e585
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/09/2018
ms.locfileid: "6197973"
---
# <a name="get-uwp-app-samples"></a>Obtener muestras de aplicaciones para UWP

Las muestras de aplicaciones para la Plataforma universal de Windows (UWP) están disponibles en los repositorios de GitHub. Consulta [Muestras](https://developer.microsoft.com/windows/samples "Muestras del Centro de desarrollo") para una lista categorizada que permite búsquedas, o examina el repositorio [Microsoft/Windows-universal-samples](https://github.com/Microsoft/Windows-universal-samples "Universal Windows Platform app samples GitHub repository"), que contiene ejemplos que demuestran todas las características de UWP y sus patrones de uso de API.  
![Repositorio de muestras de UWP de GitHub](images/GitHubUWPSamplesPage.png)

## <a name="download-the-code"></a>Descarga del código

Para descargar las muestras, dirígete al [repositorio](https://github.com/Microsoft/Windows-universal-samples "Repositorio de GitHub de muestras de aplicaciones de la Plataforma universal de Windows"), selecciona **Clone or download** y **Download ZIP**. O bien, haz clic [aquí](https://github.com/Microsoft/Windows-universal-samples/archive/master.zip "Descarga de archivos ZIP de muestras de aplicaciones de la Plataforma universal de Windows").

El archivo ZIP siempre incluirá las muestras más recientes. No necesitas una cuenta de GitHub para descargarlo. Si se publica una actualización SDK o si quieres aprovechar adiciones o cambios recientes, vuelve a descargar el archivo zip más reciente.

![Descarga de muestras](images/SamplesDownloadButton.png)


> [!NOTE]
> Las muestras de la UWP requieren Visual Studio 2015 o posterior y Windows SDK para abrirlas, compilarlas y ejecutarlas. Puedes obtener una copia gratuita de Visual Studio Community con compatibilidad para crear aplicaciones para UWP [aquí](http://go.microsoft.com/fwlink/p/?LinkID=280676 "Descargas de herramientas de desarrollo de Windows").  
>
> Asegúrate también de descomprimir todo el archivo y no solo algunos ejemplos. Todas las muestras dependen de la carpeta SharedContent del archivo. Los ejemplos de características de UWP usan archivos vinculados de Visual Studio para reducir la duplicación de archivos comunes, incluidos los archivos de plantillas de muestra y activos de imagen. Estos archivos comunes se almacenan en la carpeta SharedContent de la raíz del repositorio y se hace referencia a ellos en los archivos de proyecto que incluyen vínculos.

Cuando hayas descargado el archivo zip, abre las muestras en Visual Studio:

1.  Antes de descomprimir el archivo, haz clic en él con el botón derecho del ratón, selecciona **Propiedades** > **Desbloquear** > **Aplicar**. A continuación, descomprime el archivo en una carpeta local del equipo.

    ![Archivos descomprimido](images/SamplesUnzip1.png)
2.  En la carpeta de muestras, verás un número de carpetas; cada una de ellas contiene una muestra de característica de UWP.

    ![Carpetas de muestra](images/SamplesUnzip2.png)

3.  Selecciona una muestra, como Altimeter, y verás varias carpetas que indican los idiomas admitidos.

    ![Carpetas de idioma](images/SamplesUnzip3.png)

4.  Selecciona el idioma que quieres usar, por ejemplo, CS para C\#, y verás un archivo de la solución Visual Studio que puedes abrir en Visual Studio.

    ![Solución VS](images/SamplesUnzip4.png)

## <a name="give-feedback-ask-questions-and-report-issues"></a>Envío de comentarios, formulación de preguntas e informes de problemas

Si tienes problemas o preguntas, usa la pestaña Issues (Problemas) del repositorio para crear un nuevo informe de problema y haremos lo posible por ayudar.

![Imagen de comentarios](images/GitHubUWPSamplesFeedback.png)
