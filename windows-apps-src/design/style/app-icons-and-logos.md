---
Description: Cómo crear iconos o logotipos de app que representan la app en el menú Inicio, los iconos de app, la barra de tareas, Microsoft Store y mucho más.
title: Logotipos e iconos de aplicaciones
template: detail.hbs
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp
design-contact: Judysa
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 760beb6b9baf63b23efb531567f4b5319f95845c
ms.sourcegitcommit: e9dc2711f0a0758727468f7ccd0d0f0eee3363e3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2019
ms.locfileid: "69979360"
---
# <a name="app-icons-and-logos"></a>Logotipos e iconos de aplicaciones 

Todas las apps tienen un icono o logotipo que la representa y ese icono aparece en varias ubicaciones en el shell de Windows: 

:::row:::
    :::column:::
        * La lista de apps en el menú Inicio
        * El barra de tareas y el Administrador de tareas
        * Iconos de la app
        * Pantalla de presentación de la app
        * En Microsoft Store
    :::column-end:::
    :::column:::
        ![windows 10 start and tiles](images/assetguidance01.jpg)
    :::column-end:::
:::row-end:::

En este artículo se tratan los aspectos básicos de la creación de iconos de app, cómo usar Visual Studio para administrarlos y cómo administrarlos manualmente, si fuera necesario.
 
(En este artículo se tratan específicamente los iconos que representan la app en sí. Para obtener ayuda general sobre los iconos, consulta el artículo [Iconos](icons.md)).

## <a name="icon-types-locations-and-scale-factors"></a>Tipos de icono, ubicaciones y factores de escala

De manera predeterminada, Visual Studio almacena los recursos de icono en un subdirectorio de recursos. A continuación se proporciona una lista de los diferentes tipos de iconos, dónde aparecen y cómo se llaman. 

| Nombre del icono | Aparece en | Nombre de archivo de recurso |
| ---      | ---        | --- |
| Icono pequeño | Menú Inicio |  SmallTile.png  |
| Icono mediano |Menú Inicio, descripción de Store\*  |  Square150x150Logo.png |
| Icono ancho  | Menú Inicio   | Wide310x150Logo.png |
| Icono grande   | Menú Inicio, descripción de Store\* |  LargeTile.png  |
| Icono de la app | Lista de apps en el menú Inicio, barra de tareas, Administrador de tareas | Square44x44Logo.png |
| Pantalla de presentación | Pantalla de presentación de la app | SplashScreen.png  |
| Logotipo de distintivo | Iconos de la app | BadgeLogo.png  |
| Logotipo de paquete o logotipo de Microsoft Store | Instalador de la app, Centro de partners, opción "Informar sobre este producto" en Microsoft Store, opción "Escribir una opinión" en Microsoft Store | StoreLogo.png  |

\* Se usa a menos que elijas [mostrar solo imágenes cargadas en Microsoft Store](/windows/uwp/publish/app-screenshots-and-images#display-only-uploaded-logo-images-in-the-store). 

Para asegurarte de que estos iconos se muestren de manera nítida en todas las pantallas, puedes crear varias versiones del mismo icono para mostrar diferentes factores de escala. 

El factor de escala determina el tamaño de los elementos de interfaz de usuario, como texto. Los factores de escala varían del 100 % al 400 %. Los valores mayores crean elementos de interfaz de usuario más grandes, por lo que son más fáciles de ver en pantallas de ppp elevados. 

:::row:::
    :::column:::
        Windows automatically sets the scale factor for each display based on its DPI (dots-per-inch) and the viewing distance of the device. 

        (Users can override the default value by going to the **Settings &gt; Display &gt; Scale and layout** page.)
    :::column-end:::
    :::column:::
        ![](images/icons/display-settings-screen.png)
    :::column-end:::
:::row-end:::  


Dado que los recursos de iconos de app son mapas de bits y estos no se escalan bien, recomendamos que proporciones una versión de cada recurso de icono para cada factor de escala: 100 %, 125 %, 150 %, 200 % y 400 %. Eso son muchos iconos. Afortunadamente, Visual Studio proporciona una herramienta que facilita la tarea de generar y actualizar estos iconos. 

## <a name="microsoft-store-listing-image"></a>Imagen de la descripción de Store

"¿Cómo puedo especificar imágenes para la descripción de mi app en Microsoft Store?"

De manera predeterminada, usamos algunas de las imágenes de los paquetes en Microsoft Store, tal como se describe en la tabla de la parte superior de esta página (junto con otras [imágenes que proporciones durante el proceso de envío](https://docs.microsoft.com/en-us/windows/uwp/publish/app-screenshots-and-images)). Además, tienes la opción de impedir que Microsoft Store use las imágenes de logotipo en los paquetes de tu app al mostrar la descripción a los clientes en Windows 10 (incluyendo Xbox) y, en cambio, hacer que Microsoft Store use solo las imágenes que cargues. Esto te ofrece más control sobre el aspecto de la app en diversas pantallas en Microsoft Store. (Ten en cuenta que si tu producto admite versiones anteriores del sistema operativo, esos clientes todavía podrían ver las imágenes de los paquetes, incluso si usas esta opción). Puedes hacer esto en la sección **Store logos** (Logotipos de Microsoft Store) del paso **Descripción en Microsoft Store** del proceso de envío.

![Especificar los logotipos de Microsoft Store durante el proceso de envío](images/app-icons/storelogodisplay.png)

Cuando activas esta casilla, aparece una nueva sección llamada **Store display images** (Imágenes de visualización en Microsoft Store). Aquí, puedes cargar 3 tamaños de imagen que Microsoft Store usará en lugar de las imágenes de logotipo de los paquetes de la app: 300 x 300, 150 x 150 y 71 x 71 píxeles. Solo el tamaño 300 x 300 es obligatorio, aunque recomendamos que proporciones los tres tamaños.

Para más información, consulta [Mostrar solo imágenes de logotipo cargadas en Microsoft Store](/windows/uwp/publish/app-screenshots-and-images#display-only-uploaded-logo-images-in-the-store).

<!-- ### Fallback images for the Store

The simplest way to control the Store listing image is to specify it during the app submission process. If you don't provide these images during the app submission process, the Store will use a tile image:

1. Large tile
2. Medium tile

If these images aren't provided, the Store will search all matching images of the same image type with a square aspect ratio, preferable with a height greated than the scaled requested height (scaled height is the machine's resolution scale factor * display height). If none of the images meet this criteria, the Store will ignore the scale factor and select an image based on height.  -->

<!-- You can provide screenshots, logos, and other art assets (such as trailers and promotional images to include in your app's Microsoft Store listing. Some of these are required, and some are optional (although some of the optional images are important to include for the best Store display).

The Store may also use your app's tile and other images that you include in your app's package. 

For more information, see [App screenshots, images, and trailers in the Microsoft Store](/windows/uwp/publish/app-screenshots-and-images). -->


## <a name="managing-app-icons-with-the-visual-studio-manifest-designer"></a>Administrar iconos de app con el Diseñador de manifiestos de Visual Studio

Visual Studio proporciona una herramienta muy útil para administrar los iconos de app denominado **Diseñador de manifiestos**. 

> Si aún no tienes Visual Studio 2019, hay varias versiones disponibles, incluida una versión gratuita, (Visual Studio 2019 Community Edition) y las demás versiones ofrecen evaluaciones gratuitas. Puedes descargarlas aquí: [https://developer.microsoft.com/windows/downloads](https://developer.microsoft.com/windows/downloads)


Para iniciar el Diseñador de manifiestos:
<!-- 1. Use Visual Studio to open a UWP project.
2. In the **Solution Explorer**, double-click the package.appmanifest file. 

    ![The Visual Studio 2019 Solution Explorer](images/icons/vs-solution-explorer.png)

    Visual Studio displays the manifest designer.

    ![The Visual Studio 2019 manifest designer](images/icons/vs-manfiest-designer.png)
3. Click the **Visual Assets** tab.

    ![The Visual Assets tab](images/icons/vs-manfiest-designer-visual-assets.png) -->


:::row:::
    :::column:::
        1. Usa Visual Studio para abrir un proyecto de UWP.
    :::column-end:::
    :::column:::
        
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        2. En el **Explorador de soluciones**, haz doble clic en el archivo Package.appmxanifest.
    :::column-end:::
    :::column:::
        ![The Visual Studio 2019 Manifest Designer](images/icons/vs-solution-explorer.png)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
            Visual Studio displays the Manifest Designer.
    :::column-end:::
    :::column:::
            ![The Visual Assets tab](images/icons/vs-manfiest-designer.png)
    :::column-end:::
:::row-end:::    
:::row:::
    :::column:::
        3. Haz clic en la pestaña **Recursos visuales**.
    :::column-end:::
    :::column:::
        ![The Visual Assets tab](images/icons/vs-manfiest-designer-visual-assets.png)
    :::column-end:::
:::row-end:::        

## <a name="generating-all-assets-at-once"></a>Generar todos los recursos a la vez

El primer elemento de menú en la pestaña **Recursos visuales**, **Todos los recursos visuales**, hace exactamente lo que sugiere su nombre: genera todos los recursos visuales que la app necesita con tan solo presionar un botón.

![Generar todos los recursos visuales en Visual Studio](images/app-icons/all-visual-assets.png)

Todo lo que necesitas hacer es proporcionar una sola imagen y Visual Studio generará los recursos de icono pequeño, icono mediano, icono grande, icono ancho, icono de la app, pantalla de presentación y logotipo del paquete para cada factor de escala.

Para generar todos los recursos a la vez:
1. Haz clic en **...**  junto al campo **Origen** y selecciona la imagen que quieres usar. Si usas una imagen de mapa de bits, asegúrate de que sea de al menos 400 x 400 píxeles, de modo que se obtengan resultados nítidos. Las imágenes vectoriales funcionan mejor; Visual Studio te permite usar archivos AI (Adobe Illustrator) y PDF. 
2. (Opcional). En la sección **Configuración de pantalla**, configura estas opciones:

    a.  **Nombre corto**:  especifica un nombre corto para la app.

    b.  **Mostrar nombre**: indicar si quieres mostrar el nombre corto en los iconos mediano, ancho o grande. 

    c. **Fondo del icono**: especifica el valor hexadecimal o un nombre de color para el color de fondo del icono. Por ejemplo, `#464646`. El valor predeterminado es `transparent`.

    d. **Fondo de la pantalla de presentación**: especifica el valor hexadecimal o nombre de color para el fondo de la pantalla de presentación. 

3. Haz clic en **Generar**. 

Visual Studio genera los archivos de imagen y los agrega al proyecto. Si quieres cambiar tus recursos, basta con repetir el proceso. 

Los recursos de iconos de escala siguen la siguiente convención de nomenclatura de archivos:

*nombre de archivo*-scale -*factor de escala*.png

Por ejemplo,

Square150x150Logo-scale-100.png, Square150x150Logo-scale-200.png, Square150x150Logo-scale-400.png

Ten en cuenta que Visual Studio no genera un logotipo de distintivo de manera predeterminada. Eso se debe a que el logotipo de distintivo es único y probablemente no debería coincidir con los otros iconos de app. Para más información, consulta el artículo [Notificaciones para aplicaciones para UWP](/windows/uwp/design/shell/tiles-and-notifications/badges). 


## <a name="more-about-app-icon-assets"></a>Más sobre los recursos de icono de app
Visual Studio genera todos los recursos de icono de app que necesite tu proyecto, pero si quieres personalizarlos, es importante comprender sus diferencias con otros recursos de la app. 

El recurso de icono de app aparece en muchos lugares: la barra de tareas de Windows, la vista de tareas, ALT + TABULADOR y la esquina inferior derecha de los iconos de inicio. Dado que el recurso de icono de app aparece en muchos lugares, tiene algunas opciones adicionales de tamaño y placa que los demás recursos no tienen: recursos de "tamaño de destino" y "sin placa". 

### <a name="target-size-app-icon-assets"></a>Recursos de icono de app de tamaño de destino
Además de los tamaños de factor de escala estándar ("Square44x44Logo.scale-400.png"), también recomendamos crear recursos de "tamaño de destino". Estos recursos se llaman "tamaño de destino" porque se dirigen a tamaños específicos, como 16 píxeles, en lugar de factores de escala específicos, como 400. Los recursos de tamaño de destino son para las superficies que no usan el sistema de nivel predefinido de escalado:

* Lista de accesos directos de Inicio (escritorio)
* Inicio en esquina inferior del icono (escritorio)
* Accesos directos (escritorio)
* Panel de control (escritorio)

Esta es la lista de recursos de tamaño de destino:


| Tamaño del activo | Ejemplo de nombre de archivo                  |
|------------|------------------------------------|
| 16x16\*    | Square44x44Logo.targetsize-16.png  |
| 24x24\*    | Square44x44Logo.targetsize-24.png  |
| 32x32\*    | Square44x44Logo.targetsize-32.png  |
| 48x48\*    | Square44x44Logo.targetsize-48.png  |
| 256x256\*  | Square44x44Logo.targetsize-256.png |
| 20x20      | Square44x44Logo.targetsize-20.png  |
| 30x30      | Square44x44Logo.targetsize-30.png  |
| 36x36      | Square44x44Logo.targetsize-36.png  |
| 40x40      | Square44x44Logo.targetsize-40.png  |
| 60x60      | Square44x44Logo.targetsize-60.png  |
| 64x64      | Square44x44Logo.targetsize-64.png  |
| 72x72      | Square44x44Logo.targetsize-72.png  |
| 80x80      | Square44x44Logo.targetsize-80.png  |
| 96x96      | Square44x44Logo.targetsize-96.png  |

\* Como mínimo, recomendamos que proporciones estos tamaños. 

No tienes que agregar espaciado interno a estos recursos porque Windows lo hará si es necesario. Estos recursos deberían contar con una superficie mínima de 16 píxeles. 

Te mostramos un ejemplo de dichos recursos tal como se muestran en los iconos de la barra de tareas de Windows:

![recursos de la barra de tareas de Windows](images/assetguidance21.png)

### <a name="unplated-assets"></a>Recursos sin placa
De manera predeterminada, Windows usa un recurso basado en el destino sobre una placa posterior de color. Si quieres, puedes proporcionar un recurso sin placa basada en destino. "Sin placa" significa que el recurso se mostrará en un fondo transparente. Ten en cuenta que estos recursos aparecerán sobre una variedad de colores de fondo. 

![activos con y sin placa](images/assetguidance22.png)

Estas son las superficies que usan recursos de icono de app sin placa:
* Barra de tareas y miniatura de la barra de tareas (escritorio)
* Accesos directos de la barra de tareas
* Vista de tareas
* ALT+TAB

### <a name="unplated-assets-and-themes"></a>Recursos y temas sin placa

El tema seleccionado del usuario determina el color de la barra de tareas. Si el recurso sin placa específicamente no está calificado específicamente para el tema actual, el sistema comprueba el recurso para conocer el contraste. Si tiene contraste suficiente con la barra de tareas, el sistema lo usa. De lo contrario, el sistema busca una versión de alto contraste del recurso. Si no encuentra ninguno, el sistema dibuja la forma con placa del recurso en su lugar. 


### <a name="target-and-unplated-sizing"></a>Tamaño de destino y sin placa

Estas son las recomendaciones de tamaño para los recursos basados en destino, a escala del 100 %:

![target-based asset sizing at 100 % scale](images/assetguidance23.png)


## <a name="more-about-splash-screen-assets"></a>Más información sobre los recursos de la pantalla de presentación
Para más información sobre las pantallas de presentación, consulta el artículo [Pantallas de presentación para UWP](/windows/uwp/launch-resume/splash-screens).

## <a name="more-about-badge-logo-assets"></a>Más información sobre los recursos de logotipo de distintivo

Cuando usas el generador de recursos para generar todos los recursos que necesitas, hay un motivo por el que no se generan logotipos de distintivo de manera predeterminada: son muy diferentes de otros recursos de la app. El logotipo de distintivo es una imagen de estado que aparece en las notificaciones y en los iconos de la app. 

Para más información, consulta el artículo [Notificaciones para aplicaciones para UWP](/windows/uwp/design/shell/tiles-and-notifications/badges).


## <a name="customizing-asset-padding"></a>Personalización del relleno de los recursos

De manera predeterminada, el generador de recursos de Visual Studio aplica el relleno recomendado a cualquier imagen. Si las imágenes ya contienen relleno o quieres imágenes de sangrado completo que se extienden hasta el final del icono, puedes desactivar esta característica al desactivar la casilla **Aplicar el relleno recomendado**. 

### <a name="tile-padding-recommendations"></a>Recomendaciones de relleno de iconos
Si quieres proporcionar tu propio relleno, estas son nuestras recomendaciones para los iconos. 

Hay 4 tamaños de icono: pequeño (71 x 71), mediano (150 x 150), ancho (310 x 150) y grande (310 x 310). 

Cada recurso de ventana es del mismo tamaño que la ventana en la que se coloca.

![Icono con sangrado completo](images/app-icons/tile-assets1.png)

Si no quieres que el icono se extienda hasta su borde, puedes usar píxeles transparentes en el recurso para crear relleno. 

![placa trasera e icono](images/assetguidance05.png)

Para los mosaicos pequeños, limita el ancho y alto del icono al 66 % del tamaño de mosaico:

![relaciones de tamaño de mosaico pequeño](images/assetguidance09.png)

Para los mosaicos medianos, limita el ancho del icono al 66 % y el alto al 50 % del tamaño de mosaico. Esto impide la superposición de elementos de la barra de personalización de marca:

![relaciones de tamaño de mosaico mediano](images/assetguidance10.png)

Para los mosaicos anchos, limita el ancho del icono al 66 % y el alto al 50 % del tamaño de mosaico. Esto impide la superposición de elementos de la barra de personalización de marca:

![relaciones de tamaño de mosaico ancho](images/assetguidance11.png)

Para iconos grandes, limita el ancho del icono al 66 % y la altura al 50 % del tamaño del icono:

![relaciones de tamaño de mosaico grande](images/assetguidance12.png)

Algunos iconos están diseñados para una orientación horizontal o vertical, mientras que otros tienen formas más complejas que impiden que se ajusten exactamente a las dimensiones de destino. Los iconos que parece que están centrados pueden medirse conforme a un lado. En este caso, las partes de un icono pueden quedar por fuera de la superficie recomendada, siempre que ocupen el mismo peso visual que un icono ajustado exactamente:

![tres iconos centrados](images/assetguidance13.png)

Con activos sin bordes, ten en cuenta los elementos que interactúan dentro de los márgenes y bordes de los iconos. Mantén los márgenes de, al menos, el 16 % del alto o ancho del icono. Este porcentaje representa el doble de ancho de los márgenes en los tamaños de mosaico más pequeños:

![icono sin bordes con márgenes](images/assetguidance14.png)

En este ejemplo los márgenes son demasiado estrechos:

![icono sin bordes con márgenes que son demasiado pequeños](images/assetguidance15.png)


## <a name="optimizing-for-specific-themes-languages-and-other-conditions"></a>Optimizar para temas específicos, idiomas y otras condiciones 

En este artículo se describe cómo crear recursos para factores de escala específicos, pero también puedes crear recursos para una amplia variedad de condiciones y combinaciones de condiciones. Por ejemplo, puedes crear iconos para pantallas de contraste alto o para temas claros y temas oscuros. Incluso puedes crear recursos para idiomas concretos.

Para obtener instrucciones, consulta [Adaptar los recursos para idioma, escala, contraste alto y otros calificadores](../../app-resources/tailor-resources-lang-scale-contrast.md).













