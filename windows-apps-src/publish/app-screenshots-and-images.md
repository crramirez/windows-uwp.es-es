---
author: jnHs
Description: "Puedes seleccionar las capturas de pantalla, logotipos y otros activos gráficos (como tráileres e imágenes promocionales) para incluirlos en la descripción de la aplicación en la Tienda."
title: "Capturas de pantalla, imágenes y tráileres de aplicaciones"
ms.assetid: D216DD2B-F43D-4D26-82EE-0CD34DB929D8
ms.author: wdg-dev-content
ms.date: 08/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: 7d5da868a93f958a9432e7f8360129a8be8ca486
ms.sourcegitcommit: 6c6f3c265498d7651fcc4081c04c41fafcbaa5e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2017
---
# <a name="app-screenshots-images-and-trailers"></a>Capturas de pantalla, imágenes y tráileres de aplicaciones

Disponer de imágenes bien diseñadas son una de las principales opciones que tienes para mostrar tu aplicación a posibles clientes en la Tienda.

Puedes proporcionar [capturas de pantalla](#screenshots), [logotipos](#store-logos) y otros activos gráficos (como [tráileres](#trailers) e [imágenes promocionales](##additional-art-assets)) para incluirlos en la descripción de la Tienda de la aplicación. Algunos de estos elementos son obligatorios y otros opcionales (aunque algunas de las imágenes opcionales son importantes para aportar una mejor visualización de la Tienda). 

Durante el [proceso de envío de la aplicación](app-submissions.md), debes proporcionar estos activos gráficos en el paso [Descripciones de la Tienda](create-app-store-listings.md). Recuerda que las imágenes que se usan en la Tienda, y la forma en que aparecen, pueden variar en función del sistema operativo del cliente y de otros factores.

La Tienda también puede usar el icono de la aplicación y otras imágenes que incluyas en el paquete de la aplicación. Ejecuta el [Kit para la certificación de aplicaciones en Windows](../debug-test-perf/windows-app-certification-kit.md) para determinar si te falta alguna de las imágenes necesarias antes de enviar tu aplicación. Para obtener instrucciones y recomendaciones acerca de estas imágenes, consulta [Activos de mosaico y de icono](../controls-and-patterns/tiles-and-notifications-app-assets.md).

## <a name="screenshots"></a>Capturas de pantalla

Las capturas de pantalla son imágenes de tu aplicación que se muestran a los clientes en la descripción de la aplicación en la Tienda.

Existe la opción de proporcionar capturas de pantalla para las diferentes familias de dispositivos y que las capturas de pantallas adecuadas aparezcan cuando un cliente vea la descripción de la Tienda de la aplicación en ese tipo de dispositivo. 

Solo se necesita una captura de pantalla (para cualquier familia de dispositivos) para el envío, aunque puedes proporcionar varias; hasta 9 capturas de pantalla de equipos de escritorio y hasta 8 de otras familias de dispositivos. Te recomendamos proporcionar al menos cuatro capturas de pantalla de cada familia de dispositivos que admita la aplicación, para que las personas puedan ver el aspecto que la aplicación tendrá en su tipo de dispositivo.

> [!NOTE]
> Microsoft Visual Studio proporciona una [herramienta para ayudarte a obtener estas capturas de pantalla](https://docs.microsoft.com/visualstudio/debugger/run-windows-store-apps-in-the-simulator#BKMK_Capture_a_screenshot_of_your_app_for_submission_to_the_Microsoft_Store).

Cada captura de pantalla debe ser un archivo .png en orientación horizontal o vertical y el tamaño del archivo no puede ser superior a 5 MB.

Los requisitos de tamaño pueden variar en función de la familia de dispositivos:
- Móvil: 768 x 1280, 720 x 1280 o 480 x 800 píxeles
- Escritorio: 1366 x 768 píxeles o superior
- Holográfica: 1268 x 720 píxeles o superior
- Xbox: 3480 x 2160 píxeles o inferior

Para obtener la mejor visualización, ten en cuenta las siguientes directrices al crear las capturas de pantalla:
- Mantener los elementos visuales y el texto de carácter crítico en las 3/4 partes de la parte superior de la imagen. Las superposiciones de texto pueden aparecer en un 1/4 de la parte inferior. 
- No agregues logotipos adicionales, iconos o mensajes de publicidad en las capturas de pantalla.
- No uses colores muy claros u oscuros o bandas de alto contraste que puedan interferir en la legibilidad de las superposiciones del texto.

También puedes proporcionar un título corto que describa cada captura de pantalla en 200 caracteres o menos.

> [!TIP]
> Las capturas de pantalla se muestran en orden en tu lista. Después de cargar las capturas de pantalla, puedes arrastrar y colocarlas para reordenarlas. 

Ten en cuenta que si creas descripciones de la Tienda para [varios idiomas](supported-languages.md), tendrás una página de **Descripción de la Tienda** para cada uno de ellos. Tendrás que subir imágenes para cada idioma por separado (incluso si vas a utilizar las mismas imágenes) y proporcionar títulos para cada uno de ellos.


## <a name="store-logos"></a>Logotipos de la tienda

Los logotipos de la Tienda son imágenes opcionales que puedes cargar para una visualización más personalizada en la Tienda. Te recomendamos que proporciones estas imágenes para una mejor visualización de la descripción de la Tienda en todos los dispositivos y versiones de sistema operativo que la aplicación admita.

Puedes proporcionar estas imágenes como archivos .png, que deberían seguir las instrucciones que se indican a continuación:

- **9:16 (720 x 1080 o 1440 x 2160 píxeles)**: se usa como la imagen principal en las descripciones de la Tienda para los clientes de dispositivos Windows10 y Xbox, por lo que te recomendamos encarecidamente que proporciones esta imagen; la descripción puede no lucir un buen aspecto sin esta opción. La imagen debe incluir el nombre de tu aplicación, y todo el texto que aparezca en la imagen debe cumplir los requisitos de legibilidad accesible (relación de contraste 4.51).  
- **1:1 (1080 x 1080 o 2160 x 2160 píxeles)**: esta imagen puede aparece en algunos diseños. Si la proporcionas, asegúrate de incluir el nombre de tu aplicación.
- **1:1 (300 x 300 píxeles)**: esta imagen, a veces se denomina el **Icono de la aplicación** y se usa al mostrar la descripción de la Tienda de la aplicación a los clientes de WindowsPhone8.1 y versiones anteriores. Si no proporcionas esta imagen, los clientes de Windows Phone8.1 o versiones anteriores verán un icono en blanco junto con la descripción de la aplicación. (Esto también se aplica a los clientes de Windows10, solo si tu aplicación tiene paquetes destinados a WindowsPhone8.1 o versiones anteriores). Si tu envío *solo* incluye paquetes para UWP, no tienes que proporcionar esta imagen. Ten en cuenta que si tu envío incluye paquetes de Windows Phone 8.x y paquetes para UWP y proporcionas esta imagen, se podrá usar en Windows 10 en algunos diseños de la tienda. Para evitar esto, puedes crear una [descripción específica de la plataforma](create-platform-specific-store-listings.md) para las versiones de sistema operativo de WindowsPhone e incluir solo el icono de la aplicación allí.

Además, tienes la opción de evitar que la Tienda utilice las imágenes del logotipo en los paquetes de la aplicación al mostrar la descripción a los clientes de Windows10 y, en cambio, puedes hacer que la Tienda utilice solo las imágenes que cargues. Esto te ofrece más control sobre la apariencia de la aplicación en diversas pantallas en toda la Tienda en Windows10.

Para usar únicamente imágenes cargadas para mostrar en la Tienda en Windows10, activa la casilla que indica **Para clientes de Windows10, mostrar imágenes de logotipo cargado en lugar de las imágenes de mis paquetes**.

Al activar esta casilla, aparecerá una nueva sección denominada **Logotipos de la Tienda cargados**. Aquí, puedes cargar 3 imágenes, incluido el tamaño de 300 x 300 del "icono de la aplicación" (si marcas la casilla, el campo para proporcionar esa imagen, se moverá a esta sección). Te recomendamos que proporciones los tres tamaños de imagen si usas esta opción: 71 x 71, 300 x 300 y 150 x 150 píxeles. Sin embargo, solo el tamaño de 300 x 300 es obligatorio.


## <a name="additional-art-assets"></a>Activos de imágenes adicionales

Esta sección te permite suministrar materiales gráficos para ayudar a mostrar tu producto de manera más eficaz en la Tienda: imágenes promocionales, imágenes de Xbox, imágenes promocionales opcionales y tráileres. Recomendamos proporcionar estas imágenes para crear una descripción de la Tienda más atractiva. La opción **16:9 "prominente" (1920 x 1080 o 3840 x 2160 píxeles)** se recomienda especialmente si tienes previsto incluir [tráileres](#trailers) en la descripción de la Tienda; si no la incluyes, los tráileres no aparecerán en la parte superior de la descripción.

Para agregar estas imágenes, selecciona **Mostrar detalles** en la sección **Activos gráficos adicionales**.


## <a name="promotional-images"></a>Imágenes promocionales

Las imágenes de esta sección pueden ayudar a que la descripción de la aplicación tenga un mejor aspecto y también nos permiten que tengamos en cuenta tu aplicación para oportunidades promocionales destacadas. Ten en cuenta que proporcionar estas imágenes no garantiza que la aplicación se muestre en las opciones destacadas. Consulta [Facilitar la promoción de la aplicación](make-your-app-easier-to-promote.md) para obtener más información.

Aquí te sugerimos algunos aspectos que debes tener en cuenta al diseñar tu material gráfico promocional:

- Las imágenes deben tener un formato .png.
- Selecciona imágenes dinámicas relacionadas con la aplicación y que impulsan el reconocimiento y la diferenciación. Evita fotografías de catálogo o elementos visuales genéricos.
- No incluyas texto (aparte de la personalización de marca).
- Minimiza el espacio vacío en la imagen.
- Evita mostrar la interfaz de usuario de la aplicación y no uses imágenes específicas de dispositivo.
- Evita temas políticos y nacionales, banderas o símbolos religiosos.
- No incluyas imágenes de gestos desconsiderados, desnudez, apuestas, monedas, drogas, tabaco o alcohol.
- No uses armas apuntando hacia el usuario o violencia excesiva y sangre.

La opción **16:9 "prominente" (1920 x 1080 o 3840 x 2160 píxeles)** se usa en varios diseños en la Tienda en todos los dispositivos Windows10. Te recomendamos que proporciones esta imagen, independientemente de las versiones del sistema operativo o de los tipos de dispositivo al que tu aplicación esté destinada. Esta imagen es *obligatoria* para la visualización correcta si tu descripción incluye [tráileres](#trailers). En el caso de clientes de Windows10 con la versión 1607 o posterior, se usa como la imagen principal en la parte superior de la descripción de la Tienda (o aparece después de finalizar la reproducción de los tráileres). 

El tamaño de imagen **2:1 (2400 x 1200)** solo se utiliza si la aplicación admite la familia de dispositivos holográficos.

Al diseñar tu imagen, ten en cuenta que en algunos diseños aplicaremos un degradado por el tercio inferior para que podamos mostrar de forma legible el texto de marketing sobre la imagen. Por este motivo, evita colocar texto y elementos visuales clave en el tercio inferior de la imagen. Además, es posible que recortemos tu imagen, por lo que recomendamos que coloques la información de marca y los detalles más importantes de tu aplicación en el centro.  

<!-- update? ![guidelines for spotlight image](images/spotlight1.jpg) 
![well-designed spotlight image](images/spotlight2.jpg)

The image below shows the key proportions to keep in mind. The "safe zone" in the center will be prominent even if we crop the image. The "dynamic text area" is where text and a gradient may appear.  -->

## <a name="xbox-images"></a>Imágenes de Xbox

Estas imágenes se recomiendan para una correcta visualización si publicas tu aplicación en Xbox. 

Hay 3 tamaños diferentes que puedes subir:
- **Imagen clave de la marca, 584 x 800 píxeles**: Usado en la Tienda Xbox.com. Debes incluir el título del producto. 
- **Imagen clave del título, 1920 x 1080 píxeles**: Debe incluir el título del producto.
- **Promoción de destacados, 1080 x 1080 píxeles**: usado en la Tienda Xbox. Debes incluir el título del producto.

> [!NOTE]
> Para una mejor visualización en Xbox, también debes proporcionar una imagen **9:16 (720 x 1080 o 1440 x 2160 píxeles)** en la sección [Logotipos de la Tienda](#store-logos).


## <a name="optional-promotional-images"></a>Imágenes promocionales opcionales

Si la aplicación es compatible con versiones anteriores del sistema operativo (Windows8.x o WindowsPhone8.x), estas imágenes deben proporcionarse para que podamos tener en cuenta tu aplicación en los diseños promocionales (aunque no garantizan que tu aplicación se muestre en las opciones destacadas). Si la aplicación no admite estas versiones anteriores del sistema operativo, puedes omitir esta sección.

**En el caso de WindowsPhone8.1 y versiones anteriores**, se pueden usar dos tamaños de imagen en los diseños promocionales: **1000 x 800 píxeles (5:4)** y **358 x 358 píxeles (1:1)**. Si la aplicación se ejecuta en Windows Phone 8.1 o versiones anteriores, te recomendamos que proporciones imágenes en estos dos tamaños para la consideración promocional.  

> [!TIP]
> Asegúrate de incluir una imagen de icono de la aplicación de 300 x 300 en la sección [Logotipos de la Tienda](#store-logos) para cualquier envío que admita Windows Phone 8.1 o versiones anteriores. Esto garantiza que la aplicación no aparezca en la Tienda con un icono en blanco.  

**Para Windows 8.1 y versiones anteriores**, los diseños promocionales pueden usar una imagen con un tamaño de **414 x 180** píxeles. Si tu aplicación se ejecuta en Windows 8.1 o versiones anteriores, te recomendamos que proporciones una imagen de este tamaño para la consideración promocional.


## <a name="trailers"></a>Tráileres

Los tráileres son vídeos cortos que proporcionan a los clientes una manera de ver tu producto en acción, para que puedan comprender mejor de qué se trata. Se muestran en la parte superior de la descripción de la Tienda de la aplicación (siempre que incluyas una **imagen 1920 x 1080 píxeles (16:9)** en la sección [Imágenes promocionales](#promotional-images)). 

Los tráileres se codifican con [Smooth Streaming](http://www.iis.net/downloads/microsoft/smooth-streaming), que adapta la calidad de una secuencia de vídeo y llega a los clientes en tiempo real en función del ancho de banda disponible y de los recursos de CPU.

> [!NOTE]
> Los tráileres solo se muestran a los clientes de Windows 10, versión 1607 o posterior.

### <a name="upload-trailers"></a>Subir tráileres

Puedes agregar hasta 15 tráileres a la descripción de la Tienda. Asegúrate de que cumplan los requisitos enumerados a continuación para cada tráiler.

Debes proporcionar un archivo de vídeo (. mp4 o .mov), una imagen en miniatura y un título para cada tráiler.

> [!IMPORTANT]
> Al utilizar tráileres, debes también proporcionar una **imagen 1920 x 1080 píxeles (16:9)** en la sección [Imágenes promocionales](#promotional-images) para que los tráileres aparezcan en la parte superior de la descripción de la Tienda. Esta imagen aparecerá después de finalizar la reproducción de los tráileres.

Sigue estas recomendaciones para facilitar tus tráileres de manera eficaz:
- Los tráileres deben ser de buena calidad y con una longitud mínima (se recomienda 2 minutos o menos). 
- La resolución y la velocidad de fotogramas deben coincidir con el material de origen. Por ejemplo, un contenido capturado a 720p60 debe codificarse y subirse a 720p60. 
- Usa una vista en miniatura diferente para cada tráiler para que los clientes sepan que son únicos.
- Algunos diseños pueden cortarse ligeramente en la parte superior e inferior del tráiler, así que asegúrate de que la información principal aparezca en el centro de la pantalla.

También debes seguir los requisitos que se describen a continuación.

**Para agregar tráileres a tu descripción:**
1. Subir el **archivo de vídeo** de tu tráiler en el cuadro indicado. También se muestra un menú desplegable en caso de que también quieras reutilizar una tráiler que ya has subido (quizá para descripción de la Tienda en un idioma diferente).
2. Una vez que hayas subido el tráiler, tendrás que subir una **imagen en miniatura** que lo complemente. Debe ser un archivo .png de 1920 x 1080 píxeles y, por lo general, suele ser una imagen estática sacada del tráiler.
3. Haz clic en el icono de lápiz para agregar un **título** para tu tráiler (255 caracteres o menos).
4. Si quieres agregar más tráileres a la lista, haz clic en **Agregar tráiler** y repite los pasos descritos anteriormente.

Para quitar un tráiler, haz clic en el **X** junto a su nombre de archivo. Puede optar por quitarlo solo de la descripción de la Tienda actual de todas las descripciones de la Tiendas para tu producto (para solo este idioma o para todos los idiomas).

### <a name="trailer-requirements"></a>Requisitos de tráiler

Al proporcionar los tráileres, asegúrate de seguir estos requisitos:

- El formato del vídeo debe ser MOV o MP4. 
- La duración del vídeo debe ser inferior a 30 minutos. 
- El tamaño del archivo del tráiler no puede superar los 10 GB. 
- La vista en miniatura debe ser un archivo PNG con una resolución de 1920 x 1080 píxeles. 
- El título no puede superar los 255 caracteres. 

Al igual que el resto de campos en la página de descripción de la Tienda, los tráileres deben pasar la certificación antes de poder publicarlos en la Tienda. Asegúrate de que tu tráileres cumplan con las [directivas de la Tienda Windows](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx).

Existen requisitos adicionales según el tipo de archivo.

#### <a name="mov"></a>MOV

<table>
<tr>
<td>
**Video:**
<ul>
<li>1080p ProRes (alta calidad cuando corresponda)</li>
<li>Velocidad de fotogramas nativa; 29,97 FPS (preferida)</li>
</ul>
</td>
<td>
**Audio:**
<ul>
<li>Estéreo necesario</li>
<li>Nivel de Audio recomendado: -16 LKFS/LUFS</li>
</ul> 
</td>
</tr>
</table>

#### <a name="mp4"></a>MP4

<table>
<tr>
<td>
**Video:**
<ul>
<li>Códec: H.264</li>
<li>Escaneo progresivo (no entrelazado)</li>
<li>Perfil alto</li>
<li>2 fotogramas B consecutivos</li>
<li>GOP cerrado. GOP de la mitad de la velocidad de fotogramas</li>
<li>CABAC</li>
<li>50 MB/s </li>
<li>Espacio de colores: 4.2.0</li>
</ul>
</td>
<td>
**Audio:**
<ul>
<li>Codec: AAC-LC</li>
<li>Canales: Estéreo o el sonido envolvente</li>
<li>Velocidad de muestra: 48 KHz</li>
<li>Velocidad de bits de audio: 384 Kbps para estéreo, 512 KB/s para sonido envolvente</li>
</ul>
</td>
</tr>
</table>

Para los archivos de Mezzanine H.264, se recomienda lo siguiente:
- Contenedor: MP4
- No modificar las listas (o se puede perder la sincronización con AV)
- Moov atom al principio del archivo (inicio rápido)

