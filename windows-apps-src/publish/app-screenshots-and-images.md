---
author: jnHs
Description: You can select the screenshots, logos, and other art assets (such as trailers and promotional images) to include in your app's Store listing.
title: Capturas de pantalla, imágenes y tráileres de aplicaciones
ms.assetid: D216DD2B-F43D-4D26-82EE-0CD34DB929D8
ms.author: wdg-dev-content
ms.date: 8/20/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, tráiler, vídeo, captura de pantalla, imagen, icono, descripción de Store, imágenes de la descripción de Store
ms.localizationpriority: medium
ms.openlocfilehash: d787ac620f9d0114d67d85def758416afd9f5f15
ms.sourcegitcommit: c6d6f8b54253e79354f8db14e5cf3b113a3e5014
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "2830371"
---
# <a name="app-screenshots-images-and-trailers"></a>Capturas de pantalla, imágenes y tráileres de aplicaciones

Disponer de imágenes bien diseñadas son una de las principales opciones que tienes para mostrar tu aplicación a posibles clientes en Store.

Puedes proporcionar [capturas de pantalla](#screenshots), [logotipos](#store-logos) y otros activos gráficos (como [tráileres](#trailers) e [imágenes promocionales](##additional-art-assets)) para incluirlos en la descripción de Store de la aplicación. Algunos de estos elementos son obligatorios y otros opcionales (aunque algunas de las imágenes opcionales son importantes para aportar una mejor visualización de Store). 

Durante el [proceso de envío de la aplicación](app-submissions.md), debes proporcionar estos activos gráficos en el paso [Descripciones de Store](create-app-store-listings.md). Recuerda que las imágenes que se usan en Store, y la forma en que aparecen, pueden variar en función del sistema operativo del cliente y de otros factores.

Store también puede usar el icono de la aplicación y otras imágenes que incluyas en el paquete de la aplicación. Ejecuta el [Kit para la certificación de aplicaciones en Windows](../debug-test-perf/windows-app-certification-kit.md) para determinar si te falta alguna de las imágenes necesarias antes de enviar tu aplicación. Para obtener instrucciones y recomendaciones acerca de estas imágenes, consulta [Activos de mosaico y de icono](../design/shell/tiles-and-notifications/app-assets.md).

## <a name="screenshots"></a>Capturas de pantalla

Las capturas de pantalla son imágenes de tu aplicación que se muestran a los clientes en la descripción de Store de tu aplicación.

Tienes la opción de proporcionar capturas de pantalla para las diferentes familias de dispositivos que admita tu aplicación, y que las capturas de pantallas adecuadas aparezcan cuando un cliente vea la descripción de Store de la aplicación para ese tipo de dispositivo. 

Solo se necesita una captura de pantalla (para cualquier familia de dispositivos) para el envío, aunque puedes proporcionar varias; hasta 9 capturas de pantalla de equipos de escritorio y hasta 8 de otras familias de dispositivos. Te recomendamos proporcionar al menos cuatro capturas de pantalla de cada familia de dispositivos que admita la aplicación, para que las personas puedan ver el aspecto que la aplicación tendrá en su tipo de dispositivo. (No incluyas capturas de pantalla para las familias de dispositivos con los que no es compatible tu aplicación). Ten en cuenta que las capturas de pantalla de **Escritorio** también se mostrarán a los clientes con dispositivos de Surface Hub.

> [!NOTE]
> Microsoft Visual Studio proporciona una [herramienta para ayudarte a obtener estas capturas de pantalla](https://docs.microsoft.com/visualstudio/debugger/run-windows-store-apps-in-the-simulator#BKMK_Capture_a_screenshot_of_your_app_for_submission_to_the_Microsoft_Store).

Cada captura de pantalla debe ser un archivo .png en orientación horizontal o vertical y el tamaño del archivo no puede ser superior a 50 MB.

Los requisitos de tamaño pueden variar en función de la familia de dispositivos:
- Escritorio: 1366 x 768 píxeles o superior. Admite imágenes 4K (3840 x 2160). (También se mostrarán a los clientes con dispositivos Surface Hub.)
- Móvil: las imágenes deben ser de uno de los siguientes tipos: 1080 x 1920, 1920 x 1080, 768 x 1280, 1280 x 768, 720 x 1280, 1280 x 720, 800 x 480, o 480 x 800 píxeles.
- Xbox: 3480 x 2160 píxeles o inferior. Admite imágenes 4K (3840 x 2160).
- Holográfica: 1268 x 720 píxeles o superior. Admite imágenes 4K (3840 x 2160).

Para obtener la mejor visualización, ten en cuenta las siguientes directrices al crear las capturas de pantalla:
- Mantener los elementos visuales y el texto de carácter crítico en las 3/4 partes de la parte superior de la imagen. Las superposiciones de texto pueden aparecer en un 1/4 de la parte inferior. 
- No agregues logotipos adicionales, iconos o mensajes de publicidad en las capturas de pantalla.
- No uses colores muy claros u oscuros o bandas de alto contraste que puedan interferir en la legibilidad de las superposiciones del texto.

También puedes proporcionar un título corto que describa cada captura de pantalla en 200 caracteres o menos.

> [!TIP]
> Las capturas de pantalla se muestran en orden en tu lista. Después de cargar las capturas de pantalla, puedes arrastrar y colocarlas para reordenarlas. 

Ten en cuenta que si creas descripciones de Store para [varios idiomas](supported-languages.md), tendrás una página de **Descripción de Store** para cada uno de ellos. Tendrás que subir imágenes para cada idioma por separado (incluso si vas a utilizar las mismas imágenes) y proporcionar títulos para cada uno de ellos.


## <a name="store-logos"></a>Logotipos de Store

Puedes cargar logotipos de Store para crear una visualización más personalizada en Store. Te recomendamos que proporciones estas imágenes, para que la descripción de Store aparezca óptimamente en todos los dispositivos y versiones del sistema operativo que la aplicación admita. Ten en cuenta que si la aplicación está disponible para los clientes en Xbox, algunas de estas imágenes son obligatorias.

Puedes proporcionar estas imágenes como archivos .png (no mayores de 50MB), debiendo seguir todos ellos las directrices que se indican a continuación:

### <a name="916-poster-art-720-x-1080-or-1440-x-2160-pixels"></a>Ilustración de póster 9:16 (720 x 1080 or 1440 x 2160 píxeles)

Se usa como la imagen de logotipo principal para clientes de Windows 10 y dispositivos Xbox, por lo que **recomendamos encarecidamente** facilitar esta imagen para asegurar una adecuada visualización. Tu descripción puede no tener buen aspecto si no la incluyes y no será coherente con otras descripciones que los clientes verán mientras miran Store. Esta imagen también puede usarse en resultados de búsqueda o en colecciones supervisadas editorialmente.

Esta imagen debe incluir el nombre de tu aplicación y todo el texto que aparezca en la imagen debe cumplir los requisitos de legibilidad accesible (relación de contraste 4.51). Ten en cuenta que puede aparecer texto superpuesto en el cuarto inferior de esta imagen, por lo que debes asegurarte de no incluir allí imágenes de texto o que sean clave.

> [!NOTE]
> Si la aplicación está disponible para los clientes en Xbox, esta imagen es **obligatoria** y debe incluir el título del producto. El título debe aparecer en los tres cuartos superiores de la imagen, ya que pueden aparecer superposiciones de texto en la parte inferior de la imagen.

### <a name="11-box-art-1080-x-1080-or-2160-x-2160-pixels"></a>Ilustración de cuadro 1:1 (1080 x 1080 or 2160 x 2160 píxeles)

Esta imagen puede aparecer en varias páginas de Store para Windows 10 (incluida la Xbox) y, si no proporcionas la **Ilustración de póster 9:16**, se usará como logotipo principal. Esta imagen también debe incluir el nombre de tu aplicación. Las superposiciones de texto pueden aparecer en el cuarto inferior de esta imagen, por lo que no debes incluir allí imágenes de texto o que sean clave. Asegúrate de incluir en esta imagen el nombre de tu aplicación. 

> [!NOTE]
> Si la aplicación está disponible para los clientes en Xbox, esta imagen es **obligatoria** y debe incluir el título del producto. El título debe aparecer en los tres cuartos superiores de la imagen, ya que pueden aparecer superposiciones de texto en la parte inferior de la imagen.

### <a name="11-app-tile-icon-300-x-300-pixels"></a>Icono de ventana de aplicación 1:1 (300 x 300 píxeles)

Esta imagen es necesaria para la visualización correcta en Windows Phone 8.1 y versiones anteriores. Si tu aplicación es compatible con Windows Phone 8.1 o versiones anteriores y no proporcionas esta imagen, los clientes en cuestión verán un icono en blanco junto con la descripción de la aplicación. (Esto también se aplica a los clientes de Windows10, solo si tu aplicación tiene paquetes destinados a WindowsPhone8.1 o versiones anteriores). Si tu envío *solo* incluye paquetes para UWP, no tienes que proporcionar esta imagen. (Ten en cuenta que si tu envío incluye paquetes tanto de Windows Phone 8.x como para UWP y proporcionas esta imagen, se podrá usar en Windows 10, en algunos diseños de Store. Para evitar esto, puedes crear una [descripción específica de la plataforma](create-platform-specific-store-listings.md) para las versiones de Windows Phone con las que sea compatible tu aplicación y solo incluir allí el icono de la aplicación).

### <a name="display-only-uploaded-logo-images-in-the-store"></a>Para mostrar sólo carga imágenes de logotipo en el almacén

Tiene la opción de impedir que el almacén de uso de las imágenes de logotipo en paquetes de su aplicación cuando se muestra el anuncio a los clientes en 10 de Windows (incluidos Xbox) y, en su lugar que tengan el almacén sólo las imágenes que se cargan. Esto te ofrece más control sobre la apariencia de la aplicación en diversas pantallas por todo Store, para los clientes de Windows10 (incluyendo Xbox). (Si su producto es compatible con versiones anteriores del sistema operativo, los clientes es posible que aún vea imágenes de los paquetes, incluso si usa esta opción).

Para que el almacén de usar sólo las imágenes que se carga (para clientes en 10 de Windows, incluidos Xbox), y no usar todas las imágenes de los paquetes, active la casilla que dice **para los clientes en 10 de Windows y Xbox, mostrar cargan imágenes de logotipo en lugar de las imágenes de Mis paquetes **.

Cuando se activa esta casilla, aparece una nueva sección denominada **almacén de mostrar imágenes** . En este caso, puede cargar 3 imágenes, incluido el tamaño de la **aplicación de 1:1 Mosaico icono (300 x 300 píxeles)** (si se activa la casilla, el campo para proporcionar que dicha imagen se moverán en esta sección). Te recomendamos que proporciones los tres tamaños de imagen si usas esta opción: 71 x 71, 300 x 300 y 150 x 150 píxeles. Sin embargo, solo el tamaño de 300 x 300 es obligatorio.


<span id="promotional-images" />

## <a name="additional-art-assets"></a>Activos de imágenes adicionales

Esta sección te permite suministrar ilustraciones para mostrar tu producto de manera más eficaz en Store. Recomendamos proporcionar estas imágenes para crear una descripción de Store más atractiva.

> [!TIP]
> **Ilustración principal súper 16:9** se recomienda especialmente si tienes previsto incluir [tráileres de vídeo](#trailers) en la descripción de Store; si no la incluyes, los tráileres no aparecerán en la parte superior de la descripción.

Para agregar estas imágenes, selecciona **Mostrar detalles** en la sección **Activos gráficos adicionales**.



### <a name="windows-10-and-xbox-image-169-super-hero-art"></a>Imagen de Windows 10 y Xbox (imagen principal súper 16:9)

La imagen **Ilustración principal súper 16:9 (1920 x 1080 o 3840 x 2160 píxeles)** se usa en varios diseños en Microsoft Store, en todos los tipos de dispositivos con Windows10 (incluyendo Xbox). Recomendamos proporcionar esta imagen, independientemente de las versiones del sistema operativo o de los tipos de dispositivo al que tu aplicación esté destinada.

Esta imagen es *obligatoria* para la visualización correcta si tu descripción incluye [tráileres de vídeo](#trailers). En el caso de clientes de Windows10, versión 1607 o posterior (que incluye Xbox), se usa como imagen principal en la parte superior de la descripción de Store (o aparece después de finalizar la reproducción de los tráileres). También puede usarse para presentar tu aplicación en diseños promocionales por todo Store. Ten en cuenta que esta imagen no debe incluir el título del producto ni otros textos.

Recomendaciones a tener en cuenta al diseñar esta imagen:

- La imagen debe ser un archivo .png de 1920 x 1080 píxeles o 3840 x 2160 píxeles.
- Selecciona una imagen dinámica relacionada con la aplicación, para propiciar el reconocimiento y la diferenciación. Evita fotografías almacenadas o elementos visuales genéricos.
- No incluyas texto en la imagen.
- Evita colocar elementos visuales clave en el tercio inferior de la imagen (ya que en algunos diseños podemos aplicar un degradado sobre esa parte).
- Sitúa los detalles más importantes en el centro de la imagen (ya que en algunos diseños puede que la recortemos).
- Minimiza el espacio vacío.
- Evita mostrar la interfaz de usuario de la aplicación y no uses imágenes específicas de dispositivo.
- Evita temas políticos y nacionales, banderas o símbolos religiosos.
- No incluyas imágenes de gestos desconsiderados, desnudez, apuestas, monedas, drogas, tabaco o alcohol.
- No uses armas apuntando hacia el usuario ni violencia o sangre excesiva.

Mientras que proporcionar esta imagen nos permite tener en cuenta a tu aplicación para oportunidades promocionales seleccionadas, eso no garantiza que tu aplicación se seleccione. Consulta [Facilitar la promoción de la aplicación](make-your-app-easier-to-promote.md) para obtener más información.


### <a name="xbox-images"></a>Imágenes de Xbox

Estas imágenes son necesarias para una correcta visualización si publicas tu aplicación en Xbox. 

Hay 3 tamaños diferentes que puedes cargar:
- **Ilustración clave con marca, 584 x 800 píxeles**: debe incluir el título del producto. Una barra de personalización de marca es necesaria en esta imagen. Mantén el título y todas las imágenes clave en los tres cuartos superiores de la imagen, ya que pueden aparecer superposiciones sobre el cuarto inferior.
- **Ilustración principal con título, 1920 x 1080 píxeles**: debe incluir el título del producto. Mantén el título y todas las imágenes clave en los tres cuartos superiores de la imagen, ya que pueden aparecer superposiciones sobre el cuarto inferior.
- **Ilustración cuadrada promocional destacada, 1080 x 1080 píxeles**: *no* debe incluir el título del producto.

> [!NOTE]
> Para una mejor visualización en Xbox, también debes proporcionar una imagen **9:16 (720 x 1080 o 1440 x 2160 píxeles)** en la sección [Logotipos de Store](#store-logos).


### <a name="holographic-image"></a>Imagen holográfica

El formato de imagen **2:1 (2400 x 1200)** solo se usa si la aplicación es compatible con la familia de dispositivos holográficos. Si es así, se recomienda proporcionar esta imagen.


<span id="optional-promotional-images" />

### <a name="images-only-for-windows-8x-andor-windows-phone-8x"></a>Imágenes solo para Windows 8.x y/o Windows Phone 8.x 

Si la aplicación es compatible con versiones anteriores del sistema operativo (Windows8.x o WindowsPhone8.x), estas imágenes deben proporcionarse para que podamos tener en cuenta tu aplicación en los diseños promocionales (aunque no garantizan que tu aplicación se muestre en las opciones destacadas). Si la aplicación no admite estas versiones anteriores del sistema operativo, puedes omitir esta sección. (Esta sección se llamaba anteriormente **Imágenes promocionales opcionales**).

**En el caso de WindowsPhone8.1 y versiones anteriores**, se pueden usar dos tamaños de imagen en los diseños promocionales: **1000 x 800 píxeles (5:4)** y **358 x 358 píxeles (1:1)**. Si la aplicación se ejecuta en Windows Phone 8.1 o versiones anteriores, te recomendamos que proporciones imágenes en estos dos tamaños para la consideración promocional.  

> [!TIP]
> Asegúrate de incluir una imagen de icono de la aplicación de 300 x 300 en la sección [Logotipos de Store](#store-logos) para cualquier envío que admita Windows Phone 8.1 o versiones anteriores. Esto garantiza que la aplicación no aparezca en Store con un icono en blanco.  

**Para Windows 8.1 y versiones anteriores**, los diseños promocionales pueden usar una imagen con un tamaño de **414 x 180** píxeles. Si tu aplicación se ejecuta en Windows 8.1 o versiones anteriores, te recomendamos que proporciones una imagen de este tamaño para la consideración promocional.


## <a name="trailers"></a>Tráileres

Los tráileres son vídeos cortos que proporcionan a los clientes una manera de ver tu producto en acción, para que puedan comprender mejor de qué se trata. Se muestran en la parte superior de la descripción de Store de la aplicación (siempre que incluyas una **imagen 1920 x 1080 píxeles (16:9)** en la sección [Imágenes promocionales](#promotional-images)). 

Los tráileres se codifican con [Smooth Streaming](http://www.iis.net/downloads/microsoft/smooth-streaming), que adapta la calidad de una secuencia de vídeo y llega a los clientes en tiempo real en función del ancho de banda disponible y de los recursos de CPU.

> [!NOTE]
> Los tráileres solo se muestran a los clientes en Windows 10, versión 1607 o posterior (que incluye Xbox).

### <a name="upload-trailers"></a>Cargar tráileres

Puedes agregar hasta 15 tráileres a la descripción de Store. Asegúrate de que cumplan todos los [requisitos](#trailer-requirements) enumerados a continuación.

En cada tráiler que proporciones, debes cargar un archivo de vídeo (.mp4 o .mov), una imagen en miniatura y un título.

> [!IMPORTANT]
> Al utilizar tráileres, debes también proporcionar una **imagen 1920 x 1080 píxeles (16:9)** en la sección [Imágenes promocionales](#promotional-images) para que los tráileres aparezcan en la parte superior de la descripción de Store. Esta imagen aparecerá después de finalizar la reproducción de los tráileres.

Sigue estas recomendaciones para facilitar tus tráileres de manera eficaz:
- Los tráileres deben ser de buena calidad y con una longitud mínima (se recomienda 60 segundos o menos y menos de 2GB). 
- Usa una vista en miniatura diferente para cada tráiler para que los clientes sepan que son únicos.
- Algunos diseños de Microsoft Store pueden cortarse ligeramente en la parte superior e inferior del tráiler, así que asegúrate de que la información principal aparezca en el centro de la pantalla.
- La resolución y la velocidad de fotogramas deben coincidir con el material de origen. Por ejemplo, un contenido capturado a 720p60 debe codificarse y subirse a 720p60. 

También debes seguir los requisitos que se describen a continuación.

**Para agregar tráileres a tu descripción:**
1. Subir el **archivo de vídeo** de tu tráiler en el cuadro indicado. También se muestra un menú desplegable en caso de que también quieras reutilizar una tráiler que ya has subido (quizá para descripción de Store en un idioma diferente).
2. Una vez que hayas subido el tráiler, tendrás que subir una **imagen en miniatura** que lo complemente. Debe ser un archivo .png de 1920 x 1080 píxeles y, por lo general, suele ser una imagen estática sacada del tráiler.
3. Haz clic en el icono de lápiz para agregar un **título** para tu tráiler (255 caracteres o menos).
4. Si quieres agregar más tráileres a la lista, haz clic en **Agregar tráiler** y repite los pasos descritos anteriormente.

> [!TIP]
> Si has creado descripciones de Microsoft Store en varios idiomas, puedes seleccionar **Choose from existing trailers** para volver a usar los tráileres que ya has cargado. No tienes que cargarlos de forma individual para cada idioma.

Para quitar un tráiler de una descripción, haz clic en la **X** junto a su nombre de archivo. Puedes optar por quitarlo solo de la descripción de Microsoft Store actual en la que estás trabajando o quitarlo de todas las descripciones de Microsoft Store de tus productos (en cada idioma).


### <a name="trailer-requirements"></a>Requisitos de tráiler

Al proporcionar los tráileres, asegúrate de seguir estos requisitos:

- El formato del vídeo debe ser MOV o MP4. 
- La duración del vídeo no debe superar los 60 segundos.
- El tamaño del archivo del tráiler no debe superar los 2GB. 
- La resolución de vídeo debe ser 1920 x 1080 píxeles o 3840 x 2160 píxeles.
- La vista en miniatura debe ser un archivo PNG con una resolución de 1920 x 1080 píxeles o 3840 x 2160 píxeles.
- El título no puede superar los 255 caracteres. 
- No incluyas clasificaciones por edades en tus tráileres.

Al igual que el resto de campos de la página de descripción de Store, los tráileres deben pasar la certificación antes de poder publicarlos en Microsoft Store. Asegúrate de que tus tráileres cumplan con las [directivas de Microsoft Store](https://docs.microsoft.com/legal/windows/agreements/store-policies).

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

