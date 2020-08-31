---
title: Agregar una pantalla de presentación
description: Establezca el color de fondo y la imagen de la pantalla de presentación de la aplicación mediante Microsoft Visual Studio.
ms.assetid: 41F53046-8AB7-4782-9E90-964D744B7D66
ms.date: 05/08/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 06f9d412976ab9ccd5af5c8108bd5632792e4112
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167919"
---
# <a name="add-a-splash-screen"></a>Agregar una pantalla de presentación

Establezca el color de fondo y la imagen de la pantalla de presentación de la aplicación mediante Microsoft Visual Studio.

## <a name="set-the-splash-screen-image-and-background-color-in-visual-studio"></a>Establecer el color de fondo y la imagen de la pantalla de presentación en Visual Studio

Cuando se usa una plantilla de Visual Studio para crear la aplicación, se agrega una imagen predeterminada al proyecto y se establece como la imagen de la pantalla de presentación. El color de fondo predeterminado de la pantalla de presentación es gris claro. Si quieres cambiar la imagen o el color predeterminados de la pantalla de presentación de tu aplicación, sigue los pasos siguientes:

1. Abra el proyecto de aplicación de Plataforma universal de Windows (UWP) existente en Visual Studio.
2. Abre el archivo "Package.appxmanifest" en el **Explorador de soluciones**. También puedes abrir este archivo desde la barra de menús seleccionando **Proyecto** &gt; **Tienda** &gt; **Editar manifiesto de la aplicación**.
3. Abra la pestaña **activos visuales** y seleccione **pantalla de presentación** en el panel **todos los recursos visuales** en el lado izquierdo de la ventana "package. appxmanifest". Si va a cambiar la pantalla de presentación por primera vez, verá la ruta de acceso "assets \\SplashScreen.png" en el campo de la **pantalla de presentación** .

    En la captura de pantalla siguiente se muestra la ventana "package. appxmanifest" en Visual Studio. Según el tipo de proyecto, verás un conjunto de activos visuales ligeramente distinto.

    ![una captura de pantalla de la ventana "package. appxmanifest" en Visual Studio 2019](images/appmanifest.png)

    Si abre "package. appxmanifest" en un editor de texto, el [**elemento SplashScreen**](/uwp/schemas/appxpackage/appxmanifestschema/element-splashscreen) aparece como un elemento secundario del [**elemento VisualElements**](/uwp/schemas/appxpackage/appxmanifestschema/element-visualelements). El marcado de la pantalla de presentación predeterminada del archivo de manifiesto tiene el siguiente aspecto en un editor de texto:

    ```xml
    <uap:SplashScreen Image="Assets\SplashScreen.png" />
    ```

4. Para seleccionar una nueva imagen para la pantalla de presentación de una aplicación para UWP, presiona el botón que muestra unos puntos suspensivos, situado junto a la etiqueta **1240 x 600 px** debajo de **Activos a escala**. Elige la imagen de 1240 x 600 píxeles (.png, .jpg o .jpeg) que quieres usar como imagen de la pantalla de presentación.

    **Importante**    La imagen de la pantalla de presentación que elija debe ser 620 x 300 píxeles con un factor de escala 1x. Además, al diseñar la pantalla de presentación, ten en cuenta que es menor que la pantalla y que está centrada. No rellena la pantalla como una pantalla de presentación para una aplicación de la Tienda de Windows Phone.

5. Si quieres seleccionar una nueva imagen para la pantalla de presentación de una aplicación para Tienda de Windows Phone, presiona el botón que muestra unos puntos suspensivos, situado junto a la etiqueta **1152 x 1920 px** debajo de **Activos a escala**. Elige la imagen de 1152 x 1920 píxeles (.png, .jpg o .jpeg) que quieres usar como imagen de la pantalla de presentación.

    **Importante**    La imagen de la pantalla de presentación que elija debe tener 1152 x 1920 píxeles, que es el tamaño correcto para un factor de escala de 2,4 x. Si este es el único activo que proporcionas, se reducirá para los factores de escala de 1.4x y 1x.

6. En el campo **Color de fondo** de la sección **Pantalla de presentación**, establece el color de fondo que se mostrará con la imagen de la pantalla de presentación. Puede escribir el nombre de un color o ' \# ' y el valor hexadecimal de un color. Para obtener una lista de los nombres de colores disponibles, consulta el elemento [**SplashScreen**](/uwp/schemas/appxpackage/appxmanifestschema/element-splashscreen). El establecimiento de un color de fondo para tu pantalla de presentación es opcional. Si no se especifica un color para una aplicación para UWP, el color de fondo de la pantalla de presentación se toma de forma predeterminada en gris claro (valor hexadecimal \# 464646). Se trata del mismo color que el color de fondo del **Icono** (consulta el campo **Color de fondo** en la sección **Imágenes y logotipos en mosaico** de la pestaña **Activos visuales**). Si no especificas un color para un Windows Phone, o lo estableces en "transparente", el color de fondo de pantalla de presentación será transparente.

## <a name="summary-and-next-steps"></a>Resumen y pasos siguientes

Si tu aplicación tarda un poco en cargarse, considera la posibilidad de agregar una pantalla de presentación extendida. Para obtener instrucciones paso a paso, consulte [crear una pantalla de presentación personalizada](create-a-customized-splash-screen.md).

## <a name="related-topics"></a>Temas relacionados

* [Crear una pantalla de presentación personalizada](create-a-customized-splash-screen.md)
* [Referencia del esquema de manifiesto de paquete: elemento SplashScreen](/uwp/schemas/appxpackage/appxmanifestschema/element-splashscreen)
* [Clase Windows.ApplicationModel.Activation.SplashScreen](/uwp/api/Windows.ApplicationModel.Activation.SplashScreen)