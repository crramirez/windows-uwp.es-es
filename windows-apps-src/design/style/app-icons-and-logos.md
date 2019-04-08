---
Description: Cómo crear iconos o logotipos de la aplicación que representan tu aplicación en el menú Inicio, los iconos de aplicación, la barra de tareas, la Microsoft Store y mucho más.
title: Logotipos e iconos de aplicaciones
template: detail.hbs
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp
design-contact: Judysa
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: b755e8d165d58ce4303d9fefe6d051abce6c9765
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622460"
---
# <a name="app-icons-and-logos"></a>Logotipos e iconos de aplicaciones 

Todas las aplicaciones tienen un icono o logotipo que lo representa y ese icono aparece en varias ubicaciones en el shell de Windows: 

:::row:::
    :::column:::
        * La barra de título de la ventana de aplicación
        * La lista de aplicaciones en el menú Inicio
        * El Administrador de tareas y la barra de tareas
        * Iconos de la aplicación
        * Pantalla de presentación de la aplicación
        * En la Microsoft Store
    :::column-end:::
    :::column:::
        ![windows 10 start and tiles](images/assetguidance01.jpg)
    :::column-end:::
:::row-end:::

En este artículo se trata los aspectos básicos de la creación de iconos de aplicación, cómo usar Visual Studio para administrarlos y cómo administrarlos manualmente, si es necesario.
 
(En este artículo es específica para los iconos que representan la propia aplicación; para obtener instrucciones generales de icono, vea el [iconos](icons.md) artículo.)

## <a name="icon-types-locations-and-scale-factors"></a>Tipos de icono, ubicaciones y factores de escala

De forma predeterminada, Visual Studio almacena los recursos de icono en un subdirectorio de activos. Esta es una lista de los diferentes tipos de iconos, dónde aparecen, y se llama. 

| Nombre del icono | Aparece en | Nombre de archivo activo |
| ---      | ---        | --- |
| Icono pequeño | Menú Inicio |  SmallTile.png  |
| Icono mediano |Menú Inicio, el anuncio de Microsoft Store\*  |  Square150x150Logo.png |
| Icono ancho  | Menú Inicio   | Wide310x150Logo.png |
| Icono grande   | Menú Inicio, el anuncio de Microsoft Store\* |  LargeTile.png  |
| Icono de la aplicación | Lista de aplicaciones en el menú Inicio, barra de tareas, el Administrador de tareas | Square44x44Logo.png |
| Pantalla de presentación | Pantalla de presentación de la aplicación | SplashScreen.png  |
| Logotipo de distintivo | Iconos de la aplicación | BadgeLogo.png  |
| Logotipo de paquete logotipo/Store | Instalador de aplicaciones, centro de partners, la opción "Una aplicación de informes" en el Store, la opción "Escribir una opinión" en el Store | StoreLogo.png  |

\* Usar a menos que elija [mostrar sólo carga imágenes en el Store](/windows/uwp/publish/app-screenshots-and-images#display-only-uploaded-logo-images-in-the-store). 

Para asegurarse de que estos iconos muestran nítidos en cada pantalla, puede crear varias versiones del mismo icono para mostrar diferentes factores de escala. 

El factor de escala determina el tamaño de los elementos de interfaz de usuario, como texto. Intervalo de factores de escala de 100% al 400%. Los valores mayores crean elementos de interfaz de usuario más grandes, haciéndolos más fácil de ver en alta resolución. 

:::row:::
    :::column:::
        Windows automatically sets the scale factor for each display based on its DPI (dots-per-inch) and the viewing distance of the device. 

        (Users can override the default value by going to the **Settings &gt; Display &gt; Scale and layout** page.)
    :::column-end:::
    :::column:::
        ![](images/icons/display-settings-screen.png)
    :::column-end:::
:::row-end:::  


Debido a los recursos de iconos de aplicación son mapas de bits y mapas de bits no se escalan bien, le recomendamos que proporcione una versión de cada recurso de icono para cada factor de escala: 100%, 125%, 150%, 200% y 400%. Eso es una gran cantidad de iconos. Afortunadamente, Visual Studio proporciona una herramienta que facilita la tarea generar y actualizar estos iconos. 

## <a name="microsoft-store-listing-image"></a>Imagen del anuncio de Microsoft Store

"¿Cómo puedo especificar las imágenes de la lista de mi aplicación en la Microsoft Store?"

De forma predeterminada, utilizamos algunas de las imágenes de los paquetes en el Store, como se describe en la tabla en la parte superior de esta página (junto con otros [imágenes que proporciona durante el proceso de envío](https://docs.microsoft.com/en-us/windows/uwp/publish/app-screenshots-and-images)). Sin embargo, tiene la opción para impedir que el Store usando las imágenes de logotipo en los paquetes de la aplicación al mostrar la lista de los clientes en Windows 10 (incluidos Xbox) y en su lugar tiene el Store usar solo las imágenes que se cargan. Esto te ofrece más control sobre la apariencia de la aplicación en diversas pantallas en toda la Tienda. (Tenga en cuenta que si su producto es compatible con versiones anteriores del sistema operativo, los clientes todavía pueden ver las imágenes de los paquetes, incluso si usa esta opción.) Puede hacerlo en el **Store logotipos** sección de la **lista Store** paso del proceso de envío.

![Especificar los logotipos de Store durante el proceso de envío](images/app-icons/storelogodisplay.png)

Cuando activa esta casilla, se llama una nueva sección **mostrar imágenes de Store** aparece. En este caso, puede cargar 3 tamaños de imagen que utilizará el Store en lugar de imágenes de logotipo de paquetes de la aplicación: 71 x 71, 150 x 150 y 300 x 300 píxeles. Solo el tamaño de 300 x 300 es obligatorio, aunque le recomendamos que proporcione todos los tamaños de 3.

Para obtener más información, consulte [mostrar sólo carga imágenes de logotipo en el Store](/windows/uwp/publish/app-screenshots-and-images#display-only-uploaded-logo-images-in-the-store).

<!-- ### Fallback images for the Store

The simplest way to control the Store listing image is to specify it during the app submission process. If you don't provide these images during the app submission process, the Store will use a tile image:

1. Large tile
2. Medium tile

If these images aren't provided, the Store will search all matching images of the same image type with a square aspect ratio, preferable with a height greated than the scaled requested height (scaled height is the machine's resolution scale factor * display height). If none of the images meet this criteria, the Store will ignore the scale factor and select an image based on height.  -->

<!-- You can provide screenshots, logos, and other art assets (such as trailers and promotional images to include in your app's Microsoft Store listing. Some of these are required, and some are optional (although some of the optional images are important to include for the best Store display).

The Store may also use your app's tile and other images that you include in your app's package. 

For more information, see [App screenshots, images, and trailers in the Microsoft Store](/windows/uwp/publish/app-screenshots-and-images). -->


## <a name="managing-app-icons-with-the-visual-studio-manifest-designer"></a>Administrar iconos de aplicación con el Diseñador de manifiestos de Visual Studio

Visual Studio proporciona una herramienta muy útil para administrar los iconos de aplicación denominado el **Diseñador de manifiestos**. 

> Si aún no tiene Visual Studio 2017, hay varias versiones disponibles, incluida una versión gratuita, (Visual Studio 2017 Community Edition), y las demás versiones ofrecen evaluaciones gratuitas. Puede descargarlos aquí: [https://developer.microsoft.com/windows/downloads](https://developer.microsoft.com/windows/downloads)


Para iniciar el Diseñador de manifiestos:
<!-- 1. Use Visual Studio to open a UWP project.
2. In the **Solution Explorer**, double-click the package.appmanifest file. 

    ![The Visual Studio 2017 Solution Explorer](images/icons/vs-solution-explorer.png)

    Visual Studio displays the manifest designer.

    ![The Visual Studio 2017 manifest designer](images/icons/vs-manfiest-designer.png)
3. Click the **Visual Assets** tab.

    ![The Visual Assets tab](images/icons/vs-manfiest-designer-visual-assets.png) -->


:::row:::
    :::column:::
        1. Use Visual Studio para abrir un proyecto de UWP.
    :::column-end:::
    :::column:::
        
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        2. En el **el Explorador de soluciones**, haga doble clic en el archivo Package.appmxanifest.
    :::column-end:::
    :::column:::
        ![The Visual Studio 2017 Manifest Designer](images/icons/vs-solution-explorer.png)
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
        3. Haga clic en el **activos visuales** ficha.
    :::column-end:::
    :::column:::
        ![The Visual Assets tab](images/icons/vs-manfiest-designer-visual-assets.png)
    :::column-end:::
:::row-end:::        

## <a name="generating-all-assets-at-once"></a>Generar todos los recursos a la vez

El primer elemento de menú en el **activos visuales** ficha, **todos los activos visuales**, hace exactamente lo que sugiere su nombre: genera todos los activos visual necesita la aplicación con la acción de presionar un botón.

![Generar todos los activos visuales en Visual Studio](images/app-icons/all-visual-assets.png)

Todo lo que necesita hacer es proporcionar una sola imagen, y Visual Studio generará el icono pequeño, icono mediano, icono grande, mosaico amplio, icono grande, icono de la aplicación, pantalla de presentación y empaquetar los activos de logotipo para cada factor de escala.

Para generar todos los recursos a la vez:
1. Haga clic en el **...**  junto a la **origen** campo y seleccione la imagen que desea utilizar. Si usa una imagen de mapa de bits, asegúrese de que es al menos de 400 x 400 píxeles de modo que se obtienen resultados sharp. Imágenes vectoriales funcionan mejor; Visual Studio le permite usar inteligencia artificial (Adobe Illustrator) y archivos PDF. 
2. (Opcional). En el **configuración de pantalla** sección, configure estas opciones:

    a.  **Nombre corto**:  Especifique un nombre corto para la aplicación.

    b.  **Mostrar nombre**: Indicar si desea mostrar el nombre corto en mosaicos de medio, ancho o grandes. 

    c. **Fondo de mosaico**: Especifique el valor hexadecimal o un nombre de color para el color de fondo de mosaico. Por ejemplo, `#464646`. El valor predeterminado es `transparent`.

    d. **Fondo de pantalla de presentación**: Especifique el nombre de color o el valor hexadecimal para el fondo de pantalla de presentación. 

3. Haga clic en **generar**. 

Visual Studio genera los archivos de imagen y agrega al proyecto. Si desea cambiar sus activos, basta con repetir el proceso. 

Recursos de iconos de escala siguen esta convención de nomenclatura de archivos:

*nombre de archivo*- escala -*factor de escala*.png

Por ejemplo,

Escalado-Square150x150Logo-100.png, Square150x150Logo-escala-200.png, Square150x150Logo-escala-400.png

Tenga en cuenta que Visual Studio no genera un logotipo de distintivo de forma predeterminada. Eso es porque su logotipo de distintivo es único y probablemente no debería coincidir con los otros iconos de aplicación. Para obtener más información, consulte el [distintivo de notificaciones para el artículo de las aplicaciones UWP](/windows/uwp/design/shell/tiles-and-notifications/badges). 


## <a name="more-about-app-icon-assets"></a>Más información acerca de los recursos de icono de aplicación
Visual Studio genera todos los recursos de icono de aplicación necesarios para su proyecto, pero si desea personalizar, es importante para comprender cómo son diferentes de otros activos de la aplicación. 

El recurso de icono de la aplicación aparece en muchos lugares: la barra de tareas de Windows, la vista de tareas, ALT + TAB y la esquina inferior derecha de los iconos de inicio. Dado que el recurso de icono de la aplicación aparece en muchos lugares, tiene algún ajuste de tamaño adicional y placas de opciones no tiene el resto de recursos: activos de "tamaño de destino" y "sin placa" activos. 

### <a name="target-size-app-icon-assets"></a>Recursos de icono de aplicación de tamaño de destino
Además de los tamaños de factor de escala estándar ("Square44x44Logo.scale-400.png"), también se recomienda crear recursos de "tamaño de destino". Llamamos a estos recursos: tamaño de destino porque afectan a tamaños específicos, como 16 píxeles, en lugar de los factores de escala específica, como 400. Son activos de tamaño de destino para las superficies que no usan el sistema meseta escalado:

* Lista de accesos directos de Inicio (escritorio)
* Inicio en esquina inferior del icono (escritorio)
* Accesos directos (escritorio)
* Panel de control (escritorio)

Esta es la lista de activos de tamaño de destino:


| Tamaño del activo | Ejemplo de nombre de archivo                  |
|------------|------------------------------------|
| 16 x 16\*    | Square44x44Logo.targetsize-16.png  |
| 24 x 24\*    | Square44x44Logo.targetsize-24.png  |
| 32 x 32\*    | Square44x44Logo.targetsize-32.png  |
| 48 x 48\*    | Square44x44Logo.targetsize-48.png  |
| 256 x 256\*  | Square44x44Logo.targetsize-256.png |
| 20x20      | Square44x44Logo.targetsize-20.png  |
| 30x30      | Square44x44Logo.targetsize-30.png  |
| 36x36      | Square44x44Logo.targetsize-36.png  |
| 40x40      | Square44x44Logo.targetsize-40.png  |
| 60x60      | Square44x44Logo.targetsize-60.png  |
| 64x64      | Square44x44Logo.targetsize-64.png  |
| 72x72      | Square44x44Logo.targetsize-72.png  |
| 80x80      | Square44x44Logo.targetsize-80.png  |
| 96x96      | Square44x44Logo.targetsize-96.png  |

\* Como mínimo, le recomendamos que proporcione estos tamaños. 

No tienes que agregar espaciado interno a estos recursos porque Windows lo hará si es necesario. Estos recursos deberían contar con una superficie mínima de 16 píxeles. 

Te mostramos un ejemplo de dichos recursos tal como se muestran en los iconos de la barra de tareas de Windows:

![recursos de la barra de tareas de Windows](images/assetguidance21.png)

### <a name="unplated-assets"></a>Sin placa activos
De forma predeterminada, Windows usa un recurso basado en el destino sobre una placa posterior coloreado de forma predeterminada. Si lo desea, puede proporcionar un recurso sin placa basada en destino. "Sin placa" significa que el recurso se mostrará en un fondo transparente. Tenga en cuenta que va a aparecer estos recursos a través de una variedad de colores de fondo. 

![activos con y sin placa](images/assetguidance22.png)

Estas son las superficies que utilizan recursos de icono de aplicación sin placa:
* Barra de tareas y miniatura de la barra de tareas (escritorio)
* Accesos directos de la barra de tareas
* Vista de tareas
* ALT+TAB


### <a name="target-and-unplated-sizing"></a>Destino y el tamaño sin placa

Estas son las recomendaciones de tamaño para los recursos en función de destino, en la escala al 100%:

![target-based asset sizing at 100 % scale](images/assetguidance23.png)


## <a name="more-about-splash-screen-assets"></a>Más información acerca de los activos de la pantalla de presentación
Para obtener más información acerca de las pantallas de presentación, consulte el [artículo de las pantallas de presentación UWP](/windows/uwp/launch-resume/splash-screens).

## <a name="more-about-badge-logo-assets"></a>Más información acerca de los activos de logotipo de distintivo

Cuando se utiliza el generador de activos para generar todos los recursos que necesita, hay un motivo por qué no genere los logotipos de notificación de forma predeterminada: son muy diferentes de otros activos de la aplicación. El logotipo de distintivo es una imagen de estado que aparece en las notificaciones y los iconos de la aplicación. 

Para obtener más información, consulte el [distintivo de notificaciones para el artículo de las aplicaciones UWP](/windows/uwp/design/shell/tiles-and-notifications/badges).


## <a name="customizing-asset-padding"></a>Personalización de relleno de activos

De forma predeterminada, generador de activos de Visual Studio aplica relleno recomendado a cualquier imagen. Si sus imágenes ya contienen el relleno o si desean que las imágenes de sangrado completo que se extienden hasta el final del icono, puede desactivar esta característica para ello, desactive la **aplicar recomendada relleno** casilla de verificación. 

### <a name="tile-padding-recommendations"></a>Recomendaciones de relleno del icono
Si desea proporcionar su propio relleno, estas son nuestras recomendaciones para los iconos. 

Hay 4 tamaños de icono: pequeño (71 x 71), mediano (150 x 150), todo (310 x 150) y grande (310 x 310). 

Cada recurso de ventana es del mismo tamaño que la ventana en la que se coloca.

![Sangrado completo que muestra de icono](images/app-icons/tile-assets1.png)

Si no desea que su icono ampliar hasta el borde del icono, puede usar los píxeles transparentes en el recurso para crear el relleno. 

![placa trasera e icono](images/assetguidance05.png)

Para los mosaicos pequeños, limita el ancho y alto del icono al 66 % del tamaño de mosaico:

![relaciones de tamaño de mosaico pequeño](images/assetguidance09.png)

Para los mosaicos medianos, limita el ancho del icono al 66 % y el alto al 50 % del tamaño de mosaico. Esto impide la superposición de elementos de la barra de personalización de marca:

![relaciones de tamaño de mosaico mediano](images/assetguidance10.png)

Para los mosaicos anchos, limita el ancho del icono al 66 % y el alto al 50 % del tamaño de mosaico. Esto impide la superposición de elementos de la barra de personalización de marca:

![relaciones de tamaño de mosaico ancho](images/assetguidance11.png)

Para los iconos grandes, limita el ancho del icono al 66 % y el alto al 50 % del tamaño del icono.

![relaciones de tamaño de mosaico grande](images/assetguidance12.png)

Algunos iconos están diseñados para una orientación horizontal o vertical, mientras que otros tienen formas más complejas que impiden que se ajusten exactamente a las dimensiones de destino. Los iconos que parece que están centrados pueden medirse conforme a un lado. En este caso, las partes de un icono pueden quedar por fuera de la superficie recomendada, siempre que ocupen el mismo peso visual que un icono ajustado exactamente:

![tres iconos centrados](images/assetguidance13.png)

Con activos sin bordes, ten en cuenta los elementos que interactúan dentro de los márgenes y bordes de los iconos. Mantén los márgenes de, al menos, el 16 % del alto o ancho del icono. Este porcentaje representa el doble de ancho de los márgenes en los tamaños de mosaico más pequeños:

![icono sin bordes con márgenes](images/assetguidance14.png)

En este ejemplo los márgenes son demasiado estrechos:

![icono sin bordes con márgenes que son demasiado pequeños](images/assetguidance15.png)









