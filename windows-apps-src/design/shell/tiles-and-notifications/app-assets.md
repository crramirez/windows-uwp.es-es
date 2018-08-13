---
author: mijacobs
Description: App icon assets, which appear in a variety of forms throughout the Windows 10 operating system, are the calling cards for your Universal Windows Platform (UWP) app.
title: Activos de icono y ventana
ms.assetid: D6CE21E5-2CFA-404F-8679-36AA522206C7
label: Tile and icon assets
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: cc614f7803e7546add8ecccb7cc20dc0250d0d22
ms.sourcegitcommit: eead3c00b27d9f66f79ec08c81a97e91dc1fdb3c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/18/2018
ms.locfileid: "1522794"
---
# <a name="guidelines-for-tile-and-icon-assets"></a>Directrices sobre los activos de icono y de mosaico

 


Los activos de icono de la aplicación, que aparecen en una amplia variedad de formas en todo el sistema operativo Windows 10, son las tarjetas de llamada de la aplicación para la Plataforma universal de Windows (UWP). Estas directrices detallan el lugar donde aparecen los recursos de icono de la aplicación en el sistema y proporcionan sugerencias de diseño detalladas sobre cómo crear los iconos más sofisticados.

![windows 10 start and tiles](images/assetguidance01.jpg)

## <a name="adaptive-scaling"></a>Ajuste de escala adaptable


En primer lugar, una breve introducción sobre el ajuste de escala adaptable para comprender mejor su funcionamiento con los recursos. Windows 10 presenta una evolución del modelo de ajuste de escala existente. Además del contenido del vector de escala, hay un conjunto unificado de factores de escala que proporciona un tamaño coherente para los elementos de interfaz de usuario en diversos tamaños y resoluciones de pantalla. Los factores de escala también son compatibles con los factores de escala de otros sistemas operativos como iOS y Android, lo que facilita el uso compartido de recursos entre estas plataformas.

La Store elige los recursos para descargar en parte según los valores de PPP del dispositivo. Solo se descargan los recursos que se ajusten mejor al dispositivo.

## <a name="tile-elements"></a>Elementos de ventana


Los componentes básicos de una ventana de Inicio constan de una placa trasera, un icono, una barra de personalización de marca, márgenes y un título de la aplicación:

![desglose de los elementos de ventana](images/assetguidance02.png)

El nombre de la aplicación, el distintivo y el contador (si se usa) se encuentran en la barra de personalización de marca en la parte inferior de una ventana:

![barra de personalización de marca en el icono](images/assetguidance03.png)

El alto de la barra de personalización de marca se basa en el factor de escala del dispositivo en el que se muestra:

| Factor de escala | Píxeles |
|--------------|--------|
| 100%         | 32     |
| 125%         | 40     |
| 150%         | 48     |
| 200%         | 64     |
| 400%         | 128    |

 

El sistema establece los márgenes de ventana y no se pueden modificar. La mayoría del contenido aparece dentro de los márgenes, tal como se muestra en este ejemplo:

![márgenes de ventana](images/assetguidance04.png)

El ancho de los márgenes se basa en el factor de escala del dispositivo en el que se muestra:

| Factor de escala | Píxeles |
|--------------|--------|
| 100%         | 8      |
| 125%         | 10     |
| 150%         | 12     |
| 200%         | 16     |
| 400%         | 32     |

 

## <a name="tile-assets"></a>Recursos de ventana


Cada recurso de ventana es del mismo tamaño que la ventana en la que se coloca. Puedes personalizar la marca de los iconos de la aplicación con dos representaciones de un activo diferentes:

1. Un icono o logotipo centrado con espaciado. Esto permite que el color de la placa trasera se muestre a través de él:

![placa trasera e icono](images/assetguidance05.png)

2. Un icono de marca y sin bordes sin relleno:

![icono que se muestra sin bordes](images/assetguidance06.png)

Para que exista coherencia en todos los dispositivos, cada tamaño de mosaico (pequeño, mediano, ancho y grande) tiene su propia relación de tamaño. Para lograr una ubicación de icono coherente entre mosaicos, recomendamos algunas directrices básicas de espaciado para los siguientes tamaños de mosaico. El área de intersección entre las dos superposiciones púrpuras representa la superficie ideal para un icono. A pesar de que los iconos no siempre caben en la superficie, el volumen visual de un icono debería ser más o menos equivalente a los ejemplos proporcionados.

Tamaño de mosaico pequeño:

![ejemplo de tamaño de mosaico pequeño](images/assetguidance07a.png)

Tamaño de mosaico mediano:

![ejemplo de tamaño de mosaico mediano](images/assetguidance07b.png)

Tamaño de mosaico ancho:

![ejemplo de tamaño de mosaico ancho](images/assetguidance07c.png)

Tamaño de mosaico grande:

![ejemplo de tamaño de mosaico grande](images/assetguidance07d.png)

En este ejemplo, el icono es demasiado grande para el mosaico:

![icono demasiado grande para el mosaico](images/assetguidance08a.png)

En este ejemplo, el icono es demasiado pequeño para el mosaico:

![icono demasiado pequeño para el mosaico](images/assetguidance08b.png)

Las siguientes relaciones de espaciado interno son las óptimas para iconos orientados horizontal o verticalmente.

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

## <a name="tile-assets-in-list-views"></a>Activos de icono en las vistas de lista


Los iconos también pueden aparecer en una vista de lista. Las directrices de tamaño de los activos de icono que se muestran listas de vistas difieren un poco de los activos de icono descritos anteriormente. Esta sección describe esas especificaciones de tamaño.

![activos de icono en una vista de lista](images/assetguidance16.png)

Limita el ancho y alto del icono al 75 % del tamaño de mosaico:

![tamaño del icono del mosaico de vista de lista](images/assetguidance17.png)

En los formatos de icono vertical y horizontal, limita el ancho y alto al 75 % del tamaño de mosaico:

![tamaño del icono del mosaico de vista de lista](images/assetguidance18.png)

Para ilustraciones sin bordes de elementos de marca importantes, mantén unos márgenes de al menos el 12,5 %:

![ilustración sin bordes en el mosaico de vista de lista](images/assetguidance19.png)

En este ejemplo, el icono es demasiado grande dentro del mosaico:

![icono que es demasiado grande para el mosaico](images/assetguidance20a.png)

En este ejemplo, el icono es demasiado pequeño dentro del mosaico:

![icono que es demasiado pequeño para la ventana](images/assetguidance20b.png)

## <a name="target-based-assets"></a>Recursos basados en el destino


Los recursos basados en el destino se usan para los iconos y las ventanas que se muestran en la barra de tareas de Windows, la vista de tareas, ALT+TAB, el asistente para el espacio restante y la esquina inferior derecha de las ventanas de Inicio. No tienes que agregar espaciado interno a estos recursos porque Windows lo hará si es necesario. Estos recursos deberían contar con una superficie mínima de 16 píxeles. Te mostramos un ejemplo de dichos recursos tal como se muestran en los iconos de la barra de tareas de Windows:

![recursos de la barra de tareas de Windows](images/assetguidance21.png)

Aunque estas interfaces de usuario usan de manera predeterminada un recurso basado en destino sobre una placa trasera de color, también puedes usar un recurso basado en destino sin placa. Los activos sin placa deben crearse con la posibilidad de que puedan aparecer sobre varios colores de fondo:

![activos con y sin placa](images/assetguidance22.png)

Estas son las recomendaciones de tamaño para los activos basados en el destino, a escala del 100 %:

![target-based asset sizing at 100 % scale](images/assetguidance23.png)

## <a name="splash-screen-assets"></a>Activos de pantalla de presentación


La imagen de pantalla de presentación puede proporcionarse como una ruta de acceso directa a un archivo de imagen o como un recurso. Si usas una referencia a recursos, puedes proporcionar imágenes de diferentes escalas para que Windows pueda elegir el tamaño óptimo según la resolución de pantalla y el dispositivo. También puedes proporcionar imágenes de contraste alto para accesibilidad e imágenes localizadas para que coincidan con diferentes idiomas de interfaz de usuario.

Si abres "Package.appxmanifest" en un editor de texto, se muestra el elemento [**SplashScreen**](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-splashscreen) como secundario del elemento [**VisualElements**](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-visualelements). El marcado de la pantalla de presentación predeterminada del archivo de manifiesto tiene el siguiente aspecto en un editor de texto:

```XML
<uap:SplashScreen Image="Assets\SplashScreen.png" /></code></pre></td>
</tr>
</tbody>
</table>
```

El recurso de pantalla de presentación se centra sea cual sea el dispositivo en el que se muestra:

![tamaño del recurso de pantalla de presentación](images/assetguidance27.png)

## <a name="high-contrast-assets"></a>Activos de contraste alto


El modo de contraste alto usa conjuntos de activos independientes de blanco en contraste alto (fondo blanco con texto negro) y negro en contraste alto (fondo negro con texto blanco). Si no proporcionas activos de contraste alto para la aplicación, se usarán activos estándar.

Si los activos estándar de la aplicación proporcionan una experiencia de visualización aceptable cuando se representen en un fondo blanco y negro, entonces la aplicación debería tener, como mínimo, una apariencia satisfactoria en modo de contraste alto. Si los activos estándar no permiten una experiencia de visualización aceptable cuando se representen en un fondo blanco y negro, considera la posibilidad de incluir específicamente activos de contraste alto. Los ejemplos siguientes ilustran los dos tipos de activos de contraste alto:

![ejemplos de activos con una relación de contraste alto](images/assetguidance28.png)

Si decides proporcionar activos de contraste alto, debes incluir ambos conjuntos: blanco sobre negro y negro sobre blanco. Cuando incluyas estos activos en el paquete, podrías crear una carpeta de "contraste negro" para los activos de blanco sobre negro y una carpeta de "contraste blanco" para los activos de negro sobre blanco.

## <a name="asset-size-tables"></a>Tablas de tamaño del activo


Como mínimo, te recomendamos que proporciones activos para los factores de escala 100, 200 y 400. Proporcionar activos para todos los factores de escala ofrecerá una experiencia de usuario óptima.

<br/>

<table>
<thead>
<tr><th colspan="3">Icono pequeño (Square71x71Logo)</th></tr>
</thead>
<tbody>
<tr>
    <td width="20%">Escala al 100 %</td>
    <td width="20%">71x71</td>
    <td>Square71x71Logo.scale-100.png</td>
</tr>
<tr>
    <td>Escala al 125 %</td>
    <td>89x89</td>
    <td>Square71x71Logo.scale-125.png</td>
</tr>
<tr>
    <td>Escala al 150 %</td>
    <td>107x107</td>
    <td>Square71x71Logo.scale-150.png</td>
</tr>
<tr>
    <td>Escala al 200 %</td>
    <td>142x142</td>
    <td>Square71x71Logo.scale-200.png</td>
</tr>
<tr>
    <td>Escala al 400 %</td>
    <td>284x284</td>
    <td>Square71x71Logo.scale-400.png</td>
</tr>
</tbody>
</table>

<br/>

<table>
<thead>
<tr><th colspan="3">Icono mediano (Square150x150Logo)</th></tr>
</thead>
<tbody>
<tr>
    <td width="20%">Escala al 100 %</td>
    <td width="20%">150x150</td>
    <td>Square150x150Logo.scale-100.png</td>
</tr>
<tr>
    <td>Escala al 125 %</td>
    <td>188x188</td>
    <td>Square150x150Logo.scale-125.png</td>
</tr>
<tr>
    <td>Escala al 150 %</td>
    <td>225x225</td>
    <td>Square150x150Logo.scale-150.png</td>
</tr>
<tr>
    <td>Escala al 200 %</td>
    <td>300x300</td>
    <td>Square150x150Logo.scale-200.png</td>
</tr>
<tr>
    <td>Escala al 400 %</td>
    <td>600x600</td>
    <td>Square150x150Logo.scale-400.png</td>
</tr>
</tbody>
</table>

<br/>

<table>
<thead>
<tr><th colspan="3">Icono ancho (Wide310x150Logo)</th></tr>
</thead>
<tbody>
<tr>
    <td width="20%">Escala al 100 %</td>
    <td width="20%">310x150</td>
    <td>Wide310x150Logo.scale-100.png</td>
</tr>
<tr>
    <td>Escala al 125 %</td>
    <td>388x188</td>
    <td>Wide310x150Logo.scale-125.png</td>
</tr>
<tr>
    <td>Escala al 150 %</td>
    <td>465x225</td>
    <td>Wide310x150Logo.scale-150.png</td>
</tr>
<tr>
    <td>Escala al 200 %</td>
    <td>620x300</td>
    <td>Wide310x150Logo.scale-200.png</td>
</tr>
<tr>
    <td>Escala al 400 %</td>
    <td>1240x600</td>
    <td>Wide310x150Logo.scale-400.png</td>
</tr>
</tbody>
</table>

<br/>

<table>
<thead>
<tr><th colspan="3">Icono grande (Square310x310Logo)</th></tr>
</thead>
<tbody>
<tr>
    <td width="20%">Escala al 100 %</td>
    <td width="20%">310x310</td>
    <td>Square310x310Logo.scale-100.png</td>
</tr>
<tr>
    <td>Escala al 125 %</td>
    <td>388x388</td>
    <td>Square310x310Logo.scale-125.png</td>
</tr>
<tr>
    <td>Escala al 150 %</td>
    <td>465x465</td>
    <td>Square310x310Logo.scale-150.png</td>
</tr>
<tr>
    <td>Escala al 200 %</td>
    <td>620x620</td>
    <td>Square310x310Logo.scale-200.png</td>
</tr>
<tr>
    <td>Escala al 400 %</td>
    <td>1240x1240</td>
    <td>Square310x310Logo.scale-400.png</td>
</tr>
</tbody>
</table>

<br/>

<table>
<thead>
<tr><th colspan="3">Icono de lista de aplicaciones (Square44x44Logo)</th></tr>
</thead>
<tbody>
<tr>
    <td width="20%">Escala al 100 %</td>
    <td width="20%">44x44</td>
    <td>Square44x44Logo.scale-100.png</td>
</tr>
<tr>
    <td>Escala al 125 %</td>
    <td>55x55</td>
    <td>Square44x44Logo.scale-125.png</td>
</tr>
<tr>
    <td>Escala al 150 %</td>
    <td>66x66</td>
    <td>Square44x44Logo.scale-150.png</td>
</tr>
<tr>
    <td>Escala al 200 %</td>
    <td>88x88</td>
    <td>Square44x44Logo.scale-200.png</td>
</tr>
<tr>
    <td>Escala al 400 %</td>
    <td>176x176</td>
    <td>Square44x44Logo.scale-400.png</td>
</tr>
</tbody>
</table>

<br/>

<table>
<thead>
<tr><th colspan="3">Pantalla de presentación (SplashScreen)</th></tr>
</thead>
<tbody>
<tr>
    <td width="20%">Escala al 100 %</td>
    <td width="20%">620x300</td>
    <td>SplashScreen.scale-100.png</td>
</tr>
<tr>
    <td>Escala al 125 %</td>
    <td>775x375</td>
    <td>SplashScreen.scale-125.png</td>
</tr>
<tr>
    <td>Escala al 150 %</td>
    <td>930x450</td>
    <td>SplashScreen.scale-150.png</td>
</tr>
<tr>
    <td>Escala al 200 %</td>
    <td>1240x600</td>
    <td>SplashScreen.scale-200.png</td>
</tr>
<tr>
    <td>Escala al 400 %</td>
    <td>2480x1200</td>
    <td>SplashScreen.scale-400.png</td>
</tr>
</tbody>
</table>

<br/>
 

**Recursos basados en el destino**

Los activos basados en el destino se usan en varios factores de escala. El nombre de elemento para activos basados en el destino es **Square44x44Logo**. Te recomendamos que envíes los siguientes activos como mínimo:

16x16, 24x24, 32x32, 48x48, 256x256

La siguiente tabla enumera todos los tamaños de activos basados en el destino y sus correspondientes ejemplos de nombre de archivo:

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

 

\* Enviar estos tamaños de activos como referencia

## <a name="asset-types"></a>Tipos de activo


A continuación se enumeran todos los tipos de activos, sus usos y nombres de archivo recomendados.

**Recursos de ventana**

-   Por lo general, los activos centrados se usan en Inicio para presentar la aplicación.
-   Formato de nombre de archivo: [Square\Wide]\*x\*Logo.scale-\*.png
-   Aplicaciones afectadas: todas las aplicaciones para UWP
-   Usos:
    -   Iconos de Inicio predeterminados (móviles y de escritorio)
    -   Centro de actividades (móvil y de escritorio)
    -   Conmutador de tareas (móvil)
    -   Selector de uso compartido (móvil)
    -   Selector (móvil)
    -   Tienda

**Activos de lista escalable con placa**

-   Estos activos se usan en las superficies que solicitan factores de escala. Los activos pueden obtener la placa a través del sistema o venir con su propio color de fondo si la aplicación lo incluye.
-   Formato de nombre de archivo: Square44x44Logo.scale-\*.png
-   Aplicaciones afectadas: todas las aplicaciones para UWP
-   Usos:
    -   Lista de todas las aplicaciones de Inicio (escritorio)
    -   Lista de aplicaciones usadas con mayor frecuencia en Inicio (escritorio)
    -   Administrador de tareas (escritorio)
    -   Resultados de búsqueda de Cortana
    -   Lista de todas las aplicaciones en Inicio (móvil)
    -   Configuración

**Activos con placa de la lista de tamaño de destino**

-   Estos son tamaños de activos fijos que no se escalan con niveles. Se usan principalmente para experiencias heredadas. El sistema comprueba los activos.
-   Formato de nombre de archivo: Square44x44Logo.targetsize-\*.png
-   Aplicaciones afectadas: todas las aplicaciones para UWP
-   Usos:
    -   Lista de accesos directos de Inicio (escritorio)
    -   Inicio en esquina inferior del icono (escritorio)
    -   Accesos directos (escritorio)
    -   Panel de control (escritorio)

**Activos sin placa de la lista de tamaño de destino**

-   A estos activos el sistema no les asigna una placa ni los escala.
-   Formato de nombre de archivo: Square44x44Logo.targetsize-\*\_altform-unplated.png
-   Aplicaciones afectadas: todas las aplicaciones para UWP
-   Usos:
    -   Barra de tareas y miniatura de la barra de tareas (escritorio)
    -   Accesos directos de la barra de tareas
    -   Vista de tareas
    -   ALT+TAB

**Recursos de extensión de archivo**

-   Estos activos son específicos de las extensiones de archivo. Aparecen junto a los iconos de asociación de archivos tipo Win32 en el Explorador de archivos y deben ser válidos para todos los temas. El tamaño es diferente en plataformas móviles y de escritorio.
-   Formato de nombre de archivo: \*LogoExtensions.targetsize-\*.png
-   Aplicaciones afectadas: Música, Vídeo, Fotos, Microsoft Edge, Microsoft Office
-   Usos:
    -   Explorador de archivos
    -   Cortana
    -   Distintas superficies de interfaz de usuario (escritorio)

**Pantalla de presentación**

-   El activo que aparece en la pantalla de presentación de la aplicación. Escala automáticamente tanto en las plataformas móviles como de escritorio.
-   Formato de nombre de archivo: SplashScreen.scale-*.png
-   Aplicaciones afectadas: todas las aplicaciones para UWP
-   Usos:
    -   Pantalla de presentación de la aplicación