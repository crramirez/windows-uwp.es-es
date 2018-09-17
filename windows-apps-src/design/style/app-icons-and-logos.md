---
author: mijacobs
Description: How to create app icons/logos that represent your app in the Start menu, app tiles, the taskbar, the Microsoft Store, and more.
title: Logotipos e iconos de aplicación
template: detail.hbs
ms.author: mijacobs
ms.date: 04/17/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, Windows 10, uwp, UWP
design-contact: Judysa
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: e947b00c3a070a8d95a21e38c56bda07cd45d3c4
ms.sourcegitcommit: 9e2c34a5ed3134aeca7eb9490f05b20eb9a3e5df
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2018
ms.locfileid: "3983386"
---
# <a name="app-icons-and-logos"></a>Logotipos e iconos de aplicación 

Todas las aplicaciones tienen un icono o logotipo que lo representa y ese icono aparece en varias ubicaciones en el shell de Windows: 

:::row:::
    :::column:::
        * La barra de título de la ventana de la aplicación * la lista de aplicaciones en el menú Inicio * el Administrador de la barra de tareas y tarea * los iconos de la aplicación * pantalla de presentación de la aplicación * en Microsoft Store :::column-end:::
    :::column:::
        ![windows 10 start and tiles](images/assetguidance01.jpg)
    :::column-end:::
:::row-end:::

En este artículo describe los conceptos básicos de creación de iconos de aplicación, cómo usar Visual Studio para administrarlas y cómo administrarlos manualmente, si es necesario.
 
(En este artículo es específicamente para iconos que representan la aplicación en Sí; para obtener instrucciones generales de icono, consulta el artículo de [iconos](icons.md) ).

## <a name="icon-types-locations-and-scale-factors"></a>Tipos de icono, ubicaciones y factores de escala

De manera predeterminada, Visual Studio almacena los activos de icono en un subdirectorio de activos. Esta es una lista de los distintos tipos de iconos, donde aparecen, y se llaman. 

| Nombre de icono | Aparece en | Nombre de archivo de activos |
| ---      | ---        | --- |
| Ventana pequeña | Menú Inicio |  SmallTile.png  |
| Icono mediano |Menú de inicio, Microsoft Store listing\ *  |  Square150x150Logo.png |
| Icono ancho  | Menú Inicio   | Wide310x150Logo.png |
| Icono grande   | Menú de inicio, Microsoft Store listing\ * |  LargeTile.png  |
| Icono de la aplicación | Lista de aplicaciones en el menú Inicio, barra de tareas, el Administrador de tareas | Square44x44Logo.png |
| Pantalla de presentación | Pantalla de presentación de la aplicación | SplashScreen.png  |
| Logotipo del distintivo | Iconos de la aplicación | BadgeLogo.png  |
| Logotipo de la tienda o logotipo de paquete | Instalador de aplicación, centro de desarrollo, la opción "Informar de una aplicación" en la tienda, la opción "Escribir una opinión" en la tienda | StoreLogo.png  |

\ * Usa a menos que elijas para [Mostrar solo carga imágenes en la tienda](/windows/uwp/publish/app-screenshots-and-images#display-only-uploaded-logo-images-in-the-store). 

Para garantizar que estos iconos se ven nítidos en cada pantalla, puedes crear varias versiones del mismo icono para mostrar diferentes factores de escala. 

El factor de escala determina el tamaño de los elementos de la interfaz de usuario, como texto. Intervalo de factores de escala de 100% y 400%. Los valores más grandes crean elementos de interfaz de usuario más grandes, hacer que sea más fácil ver en pantallas con valores altos de PPP. 

:::row:::
    :::column:::
        Windows establece automáticamente el factor de escala para cada pantalla en función de su valor de PPP (puntos por pulgada) y la distancia de visualización del dispositivo. 

        (Users can override the default value by going to the **Settings &gt; Display &gt; Scale and layout** page.)
    :::column-end:::
    :::column:::
        ![](images/icons/display-settings-screen.png)
    :::column-end:::
:::row-end:::  


Dado que los activos de icono de aplicación son mapas de bits y mapas de bits no se escalan bien, te recomendamos que proporciones una versión de cada recurso de icono para cada factor de escala: 100%, 125%, 150%, 200% y 400%. Es una gran cantidad de iconos. Fortunatly, Visual Studio proporciona una herramienta que facilita el proceso generar y actualizar estos iconos. 

## <a name="microsoft-store-listing-image"></a>Imagen de la descripción de Microsoft Store

"¿Cómo especifica imágenes para la descripción de la mi aplicación en Microsoft Store?"

De manera predeterminada, usamos algunas de las imágenes de los paquetes en la tienda, tal como se describe en la tabla en la parte superior de esta página (junto con otras [imágenes que proporcione durante el proceso de envío](https://docs.microsoft.com/en-us/windows/uwp/publish/app-screenshots-and-images)). Sin embargo, tienes la opción de evitar que Store utilice las imágenes de logotipo en los paquetes de la aplicación al mostrar la descripción a los clientes de Windows 10 (incluyendo Xbox) y en su lugar, hacer que Store utilice solo las imágenes que cargues. Esto te ofrece más control sobre la apariencia de la aplicación en diversas pantallas por todo Store. (Ten en cuenta que si tu producto es compatible con versiones anteriores del sistema operativo, los clientes pueden seguirán viendo las imágenes de los paquetes, incluso si usas esta opción.) Puedes hacerlo en la sección de **logotipos de la tienda** del paso del proceso de envío de la **Descripción de Store** .

![Especificar logotipos de la tienda durante el proceso de envío de aplicación](images/app-icons/storelogodisplay.png)

Al marcar esta casilla, aparecerá una nueva sección denominada **Store mostrar imágenes** . Aquí, puedes cargar 3 tamaños de imagen que la tienda usará en lugar de las imágenes de logotipo de paquetes de la aplicación: 71 x 71, 150 x 150 y 300 x 300 píxeles. Solo el tamaño de 300 x 300 es obligatorio, aunque te recomendamos que proporciones 3 todos los tamaños.

Para obtener más información, consulta la [pantalla solo carga las imágenes del logotipo de la tienda](/windows/uwp/publish/app-screenshots-and-images#display-only-uploaded-logo-images-in-the-store).

<!-- ### Fallback images for the Store

The simplest way to control the Store listing image is to specify it during the app submission process. If you don't provide these images during the app submission process, the Store will use a tile image:

1. Large tile
2. Medium tile

If these images aren't provided, the Store will search all matching images of the same image type with a square aspect ratio, preferable with a height greated than the scaled requested height (scaled height is the machine's resolution scale factor * display height). If none of the images meet this criteria, the Store will ignore the scale factor and select an image based on height.  -->

<!-- You can provide screenshots, logos, and other art assets (such as trailers and promotional images to include in your app's Microsoft Store listing. Some of these are required, and some are optional (although some of the optional images are important to include for the best Store display).

The Store may also use your app's tile and other images that you include in your app's package. 

For more information, see [App screenshots, images, and trailers in the Microsoft Store](/windows/uwp/publish/app-screenshots-and-images). -->


## <a name="managing-app-icons-with-the-visual-studio-manifest-designer"></a>Administración de los iconos de la aplicación con el Diseñador de manifiestos de Visual Studio

Visual Studio proporciona una herramienta muy útil para la administración de los iconos de la aplicación llamados el **Diseñador de manifiestos**. 

> Si aún no tienes Visual Studio 2017, existen varias versiones disponibles, incluida una versión gratuita, (Visual Studio 2017 Community Edition), y las otras versiones ofrecen evaluaciones gratuitas. Se pueden descargar aquí:[https://developer.microsoft.com/windows/downloads](https://developer.microsoft.com/windows/downloads)


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
        1. usar Visual Studio para abrir un proyecto de UWP.
    :::column-end:::
    :::column:::
        
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        2. en el **Explorador de soluciones**, haz doble clic en el archivo Package.appmxanifest.
    :::column-end:::
    :::column:::
        ![El Diseñador de manifiesto de Visual Studio 2017](images/icons/vs-solution-explorer.png)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
            Visual Studio muestra el Diseñador de manifiestos.
    :::column-end:::
    :::column:::
            ![La pestaña de activos visuales](images/icons/vs-manfiest-designer.png)
    :::column-end:::
:::row-end:::    
:::row:::
    :::column:::
        3. Haz clic en la pestaña de **Activos visuales** . :::column-end:::
    :::column:::
        ![La pestaña de activos visuales](images/icons/vs-manfiest-designer-visual-assets.png)
    :::column-end:::
:::row-end:::        

## <a name="generating-all-assets-at-once"></a>Generar todos los activos a la vez

El primer elemento de menú en la pestaña de **Activos visuales** , **Todos los activos visuales**, hace lo que sugiere su nombre: genera cada activo visual que necesite tu aplicación con la presión de un botón.

![Generar todos los activos visuales en Visual Studio](images/app-icons/all-visual-assets.png)

Todo lo que necesitas hacer es una sola imagen de suministro y Visual Studio generará el icono pequeño, icono mediano, icono grande, icono ancho, icono grande, icono de la aplicación, pantalla de presentación y el paquete de activos de logotipo para cada factor de escala.

Para generar todos los activos a la vez:
1. Haga clic en el **** junto al campo de **origen** y seleccionar la imagen que quieras usar. Si estás usando una imagen de mapa de bits, asegúrate de que está al menos 400 por 400 píxeles para que Obtén resultados nítidos. Imágenes basado en vectores funcionan mejor; Visual Studio te permite usar AI (Adobe Illustrator) y archivos PDF. 
2. (Opcional). En la sección de **Configuración de pantalla** , configurar estas opciones:

    a.  **Nombre corto**: especificar un nombre corto de la aplicación.

    b.  **Mostrar el nombre**: indicar si quieres mostrar el nombre corto en los iconos de medio, ancha o grande. 

    c. **Icono en segundo plano**: especifica el valor hexadecimal o un nombre de color para el color de fondo del icono. Por ejemplo, `#464646`. El valor predeterminado es `transparent`.

    d. **Fondo de pantalla Spash**: especifica el nombre de color o el valor hexadecimal para el fondo de pantalla spash. 

3. Haz clic en **Generar**. 

Visual Studio genera los archivos de imagen y agrega al proyecto. Si quieres cambiar tus activos, basta con repetir el proceso. 

Activos de icono a escala siguen esta convención de nomenclatura de archivo:

*nombre de archivo*- scale -*factor de escala*.png

Por ejemplo:

Square150x150Logo-escala-100.png, Square150x150Logo-escala-200.png, Square150x150Logo-escala-400.png

Ten en cuenta que Visual Studio no genera un logotipo del distintivo de manera predeterminada. Eso es porque el logotipo del distintivo es único y probablemente no debe coincidir con los otros iconos de aplicación. Para obtener más información, consulta las [notificaciones para el artículo de las aplicaciones para UWP](/windows/uwp/design/shell/tiles-and-notifications/badges). 


## <a name="more-about-app-icon-assets"></a>Más información sobre los activos de icono de la aplicación
Visual Studio generará todos los activos del icono de la aplicación requeridos por el proyecto, pero si lo deseas personalizarlos, ayuda a comprender cómo están diferentes de otros activos de la aplicación. 

El activo de icono de la aplicación aparece en una gran cantidad de lugares: la barra de tareas de Windows, la vista de tareas, ALT+TAB y la esquina inferior derecha de los iconos de inicio. Dado que el activo de icono de la aplicación aparece en muchos lugares, tiene alguna variación de tamaño adicional y placas opciones no tienen los otros activos: activos de "tamaño de destino" y "sin placa" activos. 

### <a name="target-size-app-icon-assets"></a>Activos de icono de la aplicación de tamaño de destino
Además de los tamaños de factor de escala estándar ("Square44x44Logo.scale-400.png"), también recomendamos crear activos "tamaño de destino". Llamamos a estas tamaño de destino de recursos porque tienen como destino tamaños específicos, como 16 píxeles, en lugar de factores de escala específica, como 400. Activos de tamaño de destino son para las superficies que no usan el sistema de nivel predefinido de escalado:

* Lista de accesos directos de Inicio (escritorio)
* Inicio en esquina inferior del icono (escritorio)
* Accesos directos (escritorio)
* Panel de control (escritorio)

Esta es la lista de los recursos de tamaño de destino:


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

\ * Como mínimo, te recomendamos que proporciones estos tamaños. 

No tienes que agregar espaciado interno a estos recursos porque Windows lo hará si es necesario. Estos recursos deberían contar con una superficie mínima de 16 píxeles. 

Te mostramos un ejemplo de dichos recursos tal como se muestran en los iconos de la barra de tareas de Windows:

![recursos de la barra de tareas de Windows](images/assetguidance21.png)

### <a name="unplated-assets"></a>Activos sin placa
De manera predeterminada, Windows usa un recurso basado en destino en la parte superior trasera de color de manera predeterminada. Si quieres, puedes proporcionar un recurso basado en destino sin placa. "Sin placa" significa que el activo se mostrará en un fondo transparente. Ten en cuenta que estos activos aparecerá a través de una variedad de colores de fondo. 

![activos con y sin placa](images/assetguidance22.png)

Estas son las superficies que usan activos de icono de la aplicación sin placa:
* Barra de tareas y miniatura de la barra de tareas (escritorio)
* Accesos directos de la barra de tareas
* Vista de tareas
* ALT+TAB


### <a name="target-and-unplated-sizing"></a>Sin placa de la variación de tamaño y de destino

Estas son las recomendaciones de tamaño para los activos basados en destino, a una escala del 100%:

![target-based asset sizing at 100 % scale](images/assetguidance23.png)


## <a name="more-about-splash-screen-assets"></a>Más información sobre los activos de pantalla de presentación
Para obtener más información acerca de pantallas de presentación, consulta el [artículo de pantallas de presentación UWP](/windows/uwp/launch-resume/splash-screens).

## <a name="more-about-badge-logo-assets"></a>Más información sobre los activos de logotipo del distintivo

Al usar el generador de activos para generar todos los activos que necesita, hay una razón por qué, no generará distintivo logotipos de manera predeterminada: son muy diferentes de otros activos de la aplicación. El logotipo del distintivo es una imagen de estado que aparece en las notificaciones y los iconos de la aplicación. 

Para obtener más información, consulta las [notificaciones para el artículo de las aplicaciones para UWP](/windows/uwp/design/shell/tiles-and-notifications/badges).


## <a name="customizing-asset-padding"></a>Personalización de espaciado interno de activos

De manera predeterminada, generador de activos de Visual Studio aplica espaciado recomendada a cualquier imagen. Si tus imágenes ya contienen el espaciado interno o quieres que las imágenes de bordes que se extienden hasta el final de la ventana, desactivar esta característica desactivando la casilla de verificación **Aplicar recomendada espaciado interno** . 

### <a name="tile-padding-recommendations"></a>Recomendaciones de espaciado interno de icono
Si quieres proporcionar tu propia espaciado interno, estas son nuestras recomendaciones para los iconos. 

Existen 4 tamaños de icono: pequeño (71 x 71), medio (150 x 150), todo el (310 x 150) y grandes (310 x 310). 

Cada recurso de ventana es del mismo tamaño que la ventana en la que se coloca.

![Bordes de icono que muestra](images/app-icons/tile-assets1.png)

Si no quieres que tu icono para ampliar hasta el borde de la ventana, puedes usar píxeles transparentes en su activo para crear el relleno. 

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

Con activos sin bordes, ten en cuenta los elementos que interactúan dentro de los márgenes y bordes de los iconos. Mantén los márgenes de, al menos, el 16% del alto o ancho del icono. Este porcentaje representa el doble de ancho de los márgenes en los tamaños de mosaico más pequeños:

![icono sin bordes con márgenes](images/assetguidance14.png)

En este ejemplo los márgenes son demasiado estrechos:

![icono sin bordes con márgenes que son demasiado pequeños](images/assetguidance15.png)








