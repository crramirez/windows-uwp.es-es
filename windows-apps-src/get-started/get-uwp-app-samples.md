---
title: Obtener muestras de aplicaciones para UWP
description: Aprende a descargar muestras de código de UWP desde GitHub
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, código de ejemplo, ejemplos de código
ms.assetid: 393c5a81-ee14-45e7-acd7-495e5d916909
ms.localizationpriority: medium
ms.openlocfilehash: 7f56b0f9e4cb7f89b8bc929015ecdf6d5c64d42e
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/21/2019
ms.locfileid: "67321078"
---
# <a name="get-uwp-app-samples"></a>Obtener muestras de aplicaciones para UWP

Las muestras de aplicaciones para Plataforma Universal de Windows (UWP) están disponibles en los repositorios de GitHub. Consulta [Muestras](https://developer.microsoft.com/windows/samples%20%22Dev%20Center%20samples%22) para obtener una lista categorizada que permite búsquedas, o bien examina el repositorio [Microsoft/Windows-universal-samples](https://github.com/Microsoft/Windows-universal-samples "Repositorio de GitHub de muestras de aplicaciones para Plataforma universal de Windows"), que contiene ejemplos que demuestran todas las características de UWP y sus patrones de uso de API.  
![Repositorio de muestras de UWP de GitHub](images/GitHubUWPSamplesPage.png)

## <a name="download-the-code"></a>Descarga del código

Para descargar las muestras, dirígete al [repositorio](https://github.com/Microsoft/Windows-universal-samples "Repositorio de GitHub de muestras de aplicaciones para Plataforma Universal de Windows"), selecciona **Clone or download** y, luego, **Download ZIP**. También puedes hacer clic [aquí](https://github.com/Microsoft/Windows-universal-samples/archive/master.zip "Descarga de archivos ZIP de muestras de aplicaciones para Plataforma Universal de Windows").

El archivo zip siempre incluirá las muestras más recientes. No necesitas una cuenta de GitHub para descargarlo. Si se publica una actualización SDK o si quieres aprovechar adiciones o cambios recientes, vuelve a descargar el archivo zip más reciente.

![Descarga de muestras](images/SamplesDownloadButton.png)


> [!NOTE]
> Las muestras de UWP requieren Visual Studio 2015 o posterior, y Windows SDK para poder abrirlas, compilarlas y ejecutarlas. Puedes obtener una copia gratuita de Visual Studio Community con compatibilidad para crear aplicaciones para UWP [aquí](https://go.microsoft.com/fwlink/p/?LinkID=280676 "Descargas de herramientas de desarrollo de Windows").  
>
> Asegúrate también de descomprimir todo el archivo y no solo algunos ejemplos. Todas las muestras dependen de la carpeta SharedContent del archivo. Los ejemplos de características de UWP usan archivos vinculados de Visual Studio para reducir la duplicación de archivos comunes, incluidos los archivos de plantillas de muestra y activos de imagen. Estos archivos comunes se almacenan en la carpeta SharedContent de la raíz del repositorio y se hace referencia a ellos en los archivos de proyecto que incluyen vínculos.

Cuando hayas descargado el archivo zip, abre las muestras en Visual Studio:

1.  Antes de descomprimir el archivo, haz clic en él con el botón derecho del ratón, selecciona **Propiedades** > **Desbloquear** > **Aplicar**. A continuación, descomprime el archivo en una carpeta local del equipo.

    ![Archivos descomprimido](images/SamplesUnzip1.png)
2.  En la carpeta de muestras, verás un número de carpetas; cada una de ellas contiene una muestra de característica de UWP.

    ![Carpetas de muestra](images/SamplesUnzip2.png)

3.  Selecciona una muestra, como Altimeter, y verás varias carpetas que indican los idiomas admitidos.

    ![Carpetas de idioma](images/SamplesUnzip3.png)

4.  Selecciona el idioma que quieres usar (por ejemplo, CS para C\#) y verás un archivo de la solución Visual Studio que puedes abrir en Visual Studio.

    ![Solución VS](images/SamplesUnzip4.png)

## <a name="give-feedback-ask-questions-and-report-issues"></a>Envío de comentarios, formulación de preguntas e informes de problemas

Si tienes problemas o preguntas, usa la pestaña Issues (Problemas) del repositorio para crear un nuevo informe de problema y haremos lo posible por ayudar.

![Imagen de comentarios](images/GitHubUWPSamplesFeedback.png)
