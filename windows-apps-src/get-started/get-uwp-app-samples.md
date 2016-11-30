---
title: Obtener las muestras de Plataforma universal de Windows (UWP) desde GitHub
description: "Aprende a descargar las muestras de características de UWP desde GitHub."
author: JoshuaPartlow
translationtype: Human Translation
ms.sourcegitcommit: 27f98124d8360c3d0266645660b35cf040af0ef0
ms.openlocfilehash: dbd2d5d7924cfbfd48d20f48ccfcd4bfe62db645

---

#Obtener las muestras de Plataforma universal de Windows (UWP) desde GitHub
Las muestras de aplicaciones para UWP están disponibles en los repositorios de GitHub. Si es la primera vez que trabajas con UWP, puedes comenzar con el repositorio [Microsoft/Windows-universal-samples](https://github.com/Microsoft/Windows-universal-samples), que contiene ejemplos en los que se muestran todas las características de UWP y sus modelos de utilización de API.  
![Repositorio de muestras de UWP de GitHub](images/GitHubUWPSamplesPage.png) Se pueden encontrar más muestras en la sección [Muestras](https://developer.microsoft.com/windows/samples) del Centro de desarrollo.  

##Descarga del código
Para descargar las muestras, ve al [repositorio](https://github.com/Microsoft/Windows-universal-samples) y selecciona **Clone or download (Clonar o descargar)**, a continuación, **Download ZIP (Descargar ZIP)**. O bien haz clic [aquí](https://github.com/Microsoft/Windows-universal-samples/archive/master.zip).

El archivo zip siempre incluirá las muestras más recientes. No necesitas una cuenta de GitHub para descargarlo. Si se publica una actualización SDK o si quieres aprovechar adiciones o cambios recientes, vuelve a descargar el archivo zip más reciente.

![Descarga de muestras](images/SamplesDownloadButton.png)


> **Nota**: Las muestras de la UWP requieren Visual Studio 2015 y Windows SDK para abrirlas, compilarlas y ejecutarlas. Si no tienes Visual Studio instalado, puedes obtener una copia gratuita de Visual Studio 2015 Community Edition con compatibilidad para crear aplicaciones para UWP [aquí](http://go.microsoft.com/fwlink/p/?LinkID=280676).  
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

## Envío de comentarios, formulación de preguntas e informes de problemas

Si tienes problemas o preguntas, usa la pestaña Issues (Problemas) del repositorio para crear un nuevo informe de problema y haremos lo posible por ayudar.

![Imagen de comentarios](images/GitHubUWPSamplesFeedback.png)



<!--HONumber=Nov16_HO1-->


