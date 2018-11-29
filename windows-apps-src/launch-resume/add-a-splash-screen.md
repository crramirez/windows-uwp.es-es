---
title: Agregar una pantalla de presentación
description: Establece la imagen de la pantalla de presentación y el color de fondo de tu aplicación con MicrosoftVisualStudio.
ms.assetid: 41F53046-8AB7-4782-9E90-964D744B7D66
ms.date: 05/08/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 882ee548754b9fa498697a8d75a12a23f86fc9de
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2018
ms.locfileid: "7979403"
---
# <a name="add-a-splash-screen"></a>Agregar una pantalla de presentación

Establece la imagen de la pantalla de presentación y el color de fondo de tu aplicación con MicrosoftVisualStudio.

## <a name="set-the-splash-screen-image-and-background-color-in-visual-studio"></a>Establecer la imagen de la pantalla de presentación y el color de fondo en Visual Studio

Cuando usas una plantilla de Visual Studio para crear tu aplicación, se agrega una imagen predeterminada al proyecto que se establece como imagen de la pantalla de presentación. El color de fondo predeterminado de la pantalla de presentación es gris claro. Si quieres cambiar la imagen o el color predeterminados de la pantalla de presentación de tu aplicación, sigue los pasos siguientes:

1. Abre el proyecto existente de tu aplicación para Plataforma universal de Windows (UWP) en VisualStudio.
2. Abre el archivo "Package.appxmanifest" en el **Explorador de soluciones**. También puedes abrir este archivo desde la barra de menús seleccionando **Proyecto** &gt; **Tienda** &gt; **Editar manifiesto de la aplicación**.
3. Abre la pestaña **Activos visuales** y selecciona **Pantalla de presentación** en el panel **Todos los activos visuales** situado en el lado izquierdo de la ventana "Package.appxmanifest". Si vas a cambiar la pantalla de presentación por primera vez, verás la ruta de acceso "Assets\\SplashScreen.png" en el campo **Pantalla de presentación**.

    En la siguiente captura de pantalla se muestra la ventana "Package.appxmanifest" en VisualStudio. Según el tipo de proyecto, verás un conjunto de activos visuales ligeramente distinto.

    ![captura de pantalla de la ventana "package.appxmanifest" en VisualStudio2017](images/appmanifest.png)

    Si abres "Package.appxmanifest" en un editor de texto, se muestra el elemento [**SplashScreen**](https://msdn.microsoft.com/library/windows/apps/br211467) como secundario del elemento [**VisualElements**](https://msdn.microsoft.com/library/windows/apps/br211471). El marcado de la pantalla de presentación predeterminada del archivo de manifiesto tiene el siguiente aspecto en un editor de texto:

    ```xml
    <uap:SplashScreen Image="Assets\SplashScreen.png" />
    ```

4. Para seleccionar una nueva imagen para la pantalla de presentación de una aplicación para UWP, presiona el botón que muestra unos puntos suspensivos, situado junto a la etiqueta **1240 x 600 px** debajo de **Activos a escala**. Elige la imagen de 1240 x 600 píxeles (.png, .jpg o .jpeg) que quieres usar como imagen de la pantalla de presentación.

    **Importante**la imagen de pantalla de presentación que elijas debe ser de 620 x 300 píxeles con un factor de escala de 1. Además, al diseñar la pantalla de presentación, ten en cuenta que es menor que la pantalla y que está centrada. No rellena la pantalla como una pantalla de presentación para una aplicación de la Tienda de Windows Phone.

5. Si quieres seleccionar una nueva imagen para la pantalla de presentación de una aplicación para Tienda de Windows Phone, presiona el botón que muestra unos puntos suspensivos, situado junto a la etiqueta **1152 x 1920 px** debajo de **Activos a escala**. Elige la imagen de 1152 x 1920 píxeles (.png, .jpg o .jpeg) que quieres usar como imagen de la pantalla de presentación.

    **Importante**la imagen de pantalla de presentación que elijas debe ser de 1152 x 1920 píxeles que es el tamaño correcto para un 2,4 factor de escala. Si este es el único activo que proporcionas, se reducirá para los factores de escala de 1.4x y 1x.

6. En el campo **Color de fondo** de la sección **Pantalla de presentación**, establece el color de fondo que se mostrará con la imagen de la pantalla de presentación. Puedes escribir el nombre de un color o "\#" y el valor hexadecimal de un color. Para obtener una lista de los nombres de colores disponibles, consulta el elemento [**SplashScreen**](https://msdn.microsoft.com/library/windows/apps/br211467). El establecimiento de un color de fondo para tu pantalla de presentación es opcional. Si no especificas un color para la aplicación para UWP, se establecerá el gris claro (valor hexadecimal \#464646) como color de fondo predeterminado de la pantalla de presentación. Se trata del mismo color que el color de fondo del **Icono** (consulta el campo **Color de fondo** en la sección **Imágenes y logotipos en mosaico** de la pestaña **Activos visuales**). Si no especificas un color para un Windows Phone, o lo estableces en "transparente", el color de fondo de pantalla de presentación será transparente.

## <a name="summary-and-next-steps"></a>Resumen y pasos siguientes

Si tu aplicación tarda un poco en cargarse, considera la posibilidad de agregar una pantalla de presentación extendida. Para obtener instrucciones paso a paso, consulta [Crear una pantalla de presentación personalizada](create-a-customized-splash-screen.md).

## <a name="related-topics"></a>Artículos relacionados

* [Crear una pantalla de presentación personalizada](create-a-customized-splash-screen.md)
* [Referencia del esquema de manifiesto de paquete: elemento SplashScreen](https://msdn.microsoft.com/library/windows/apps/br211467)
* [Clase Windows.ApplicationModel.Activation.SplashScreen](https://msdn.microsoft.com/library/windows/apps/br224763)