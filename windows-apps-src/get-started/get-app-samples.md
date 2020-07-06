---
title: Obtener ejemplos de aplicaciones de Windows
description: Obtenga información sobre cómo descargar ejemplos de código de Windows de GitHub.
ms.date: 06/30/2020
ms.topic: article
keywords: Windows 10, código de ejemplo, ejemplos de código
ms.assetid: 393c5a81-ee14-45e7-acd7-495e5d916909
ms.localizationpriority: medium
ms.openlocfilehash: 285388c4c1b4791e1ca271a853476416b6d13c13
ms.sourcegitcommit: 179f8098d10e338ad34fa84934f1654ec58161cd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/01/2020
ms.locfileid: "85757640"
---
# <a name="get-windows-app-samples"></a>Obtener ejemplos de aplicaciones de Windows

Muchos ejemplos de código de Windows oficiales están disponibles en varios repositorios de GitHub, incluidos los [ejemplos de aplicaciones para la Plataforma universal de Windows (UWP)](https://github.com/microsoft/Windows-universal-samples), los [ejemplos clásicos de Windows](https://github.com/microsoft/Windows-classic-samples) e incluso una colección de [ejemplos de documentación para desarrolladores de Windows](#windows-developer-documentation-samples). En estos ejemplos se muestra la mayoría de las características de Windows y los patrones de uso de la API.

:::image type="content" source="images/github-windows-samples-page.png" alt-text="Repositorio de ejemplos de la Plataforma universal de Windows de GitHub":::

Para facilitar la búsqueda de ejemplos concretos, puede examinar y buscar una colección categorizada de ejemplos de código para diversas tecnologías y herramientas para desarrolladores de Microsoft a través del [explorador de ejemplos](https://docs.microsoft.com/samples/browse/).

:::image type="content" source="images/samples-browser-windows.png" alt-text="Explorador de ejemplos de Microsoft":::

### <a name="windows-developer-documentation-samples"></a>Ejemplos de documentación para desarrolladores de Windows

La siguiente es una lista de ejemplos de miniaplicaciones creadas específicamente como apoyo para la documentación para desarrolladores de Windows. A menos que se indique lo contrario, todos los ejemplos siguientes son aplicaciones para la Plataforma universal de Windows (UWP) que se actualizaron para usar los controles más recientes de [WinUI 2.4](/windows/apps/winui/winui2/release-notes/winui-2.4).

- [Lector RSS](https://github.com/Microsoft/Windows-appsample-rssreader): permite recuperar fuentes RSS y consultar artículos.
- [Notas familiares](https://github.com/Microsoft/Windows-appsample-familynotes):permiten explorar diferentes modalidades de entrada y escenarios de reconocimiento de usuarios.
- [Pedidos de los clientes](https://github.com/Microsoft/Windows-appsample-customers-orders-database): características útiles para los desarrolladores empresariales, como la autenticación de Azure Active Directory (AAD), controles de interfaz de usuario (incluida una cuadrícula de datos), la integración de bases de datos de Sqlite y SQL Azure, Entity Framework y servicios de API en la nube.
- [Programador de comidas](https://github.com/Microsoft/Windows-appsample-lunch-scheduler): permite programar comidas con amigos y compañeros de trabajo.
- [Libro para colorear](https://github.com/Microsoft/Windows-appsample-coloringbook): Windows Ink (incluida la barra de herramientas de Windows Ink) y las características del controlador radial (para dispositivos de rueda, como Surface Dial).
- [Aplicación auxiliar de red (juego de preguntas)](https://github.com/Microsoft/Windows-appsample-networkhelper): detección y comunicación de redes.
- [Controlador de luces Hue](https://github.com/Microsoft/Windows-appsample-huelightcontroller): automatización inteligente del hogar con Cortana y Bluetooth de bajo consumo (Bluetooth LE).
- [Marble Maze](https://github.com/Microsoft/Windows-appsample-marble-maze): juego 3D básico con DirectX.
- [PhotoLab](https://github.com/Microsoft/Windows-appsample-photo-lab): permite ver y editar archivos de imagen.

## <a name="download-the-code"></a>Descarga del código

Para descargar los ejemplos, visite uno de los repositorios de Microsoft, como el de [ejemplos de aplicaciones para la Plataforma universal de Windows (UWP)](https://github.com/microsoft/Windows-universal-samples). Selecciona **Clone or download o** (Clonar o descargar) y, después, selecciona **Download ZIP** (Descargar ZIP).

![Descarga de ejemplos](images/SamplesDownloadButton.png)

Los ejemplos del archivo. zip siempre son los más recientes. Para descargar el archivo no se necesita una cuenta de GitHub. Si se publica una actualización del SDK o si quieres tener los cambios o incorporaciones más recientes, descargar el último archivo zip.

> [!NOTE]
> Para abrir, compilar y ejecutar los ejemplos de Windows, es preciso tener Visual Studio y Windows SDK. Puede obtener una copia gratuita de [Visual Studio Community](https://www.microsoft.com/?ref=go).  
>
> Para que los ejemplos funcionen correctamente, asegúrate de descomprimir todo el archivo, no solo algunos ejemplos. Muchos de los ejemplos dependen de archivos comunes de la carpeta *SharedContent* y usan archivos vinculados para reducir los duplicados, incluidos los archivos de las plantillas de ejemplo y los recursos de imágenes.

## <a name="open-the-samples"></a>Apertura de los ejemplos

Cuando hayas descargado el archivo zip, abre los ejemplos en Visual Studio.

1. Antes de descomprimir el archivo, haga clic en él con el botón derecho del mouse y seleccione **Propiedades** > **Desbloquear** > **Aplicar**. Luego, descomprime el archivo en una carpeta local del equipo.

    ![Archivos descomprimido](images/SamplesUnzip1.png)

2. Todas las subcarpetas de la carpeta Ejemplos contienen un ejemplo de característica de Windows.

    ![Carpetas de muestra](images/SamplesUnzip2.png)

3. Seleccione un ejemplo. Los idiomas admitidos se indican mediante una subcarpeta específica del idioma.

    ![Carpetas de idioma](images/SamplesUnzip3.png)

4. Selecciona la carpeta del idioma que quieres usar. En el contenido de la carpeta, verás un archivo de solución de Visual Studio (. sln) que se puede abrir en Visual Studio.

    ![Solución VS](images/SamplesUnzip4.png)

## <a name="give-feedback-ask-questions-and-report-issues"></a>Envío de comentarios, formulación de preguntas e informes de problemas

Si tienes cualquier problema o pregunta, usa la pestaña **Issues** (Problemas) del repositorio para crear un nuevo informe de problema.

![Imagen de comentarios](images/GitHubUWPSamplesFeedback.png)
