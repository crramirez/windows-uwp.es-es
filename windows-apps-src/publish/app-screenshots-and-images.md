---
Description: Puede seleccionar las capturas de pantallas, los logotipos y otros recursos artísticos (como finalizadores e imágenes promocionales) que desee incluir en la lista de la tienda de la aplicación.
title: Capturas de pantalla, imágenes y tráileres de aplicaciones
ms.assetid: D216DD2B-F43D-4D26-82EE-0CD34DB929D8
ms.date: 03/07/2019
ms.topic: article
keywords: Windows 10, UWP, finalizador, vídeo, captura de pantalla, imagen, icono, lista de tiendas, imágenes de la lista de tiendas
ms.localizationpriority: medium
ms.openlocfilehash: da9d6517a43550693596d15c735e3134c5c60a7a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155319"
---
# <a name="app-screenshots-images-and-trailers"></a>Capturas de pantalla, imágenes y tráileres de aplicaciones

Las imágenes bien diseñadas son una de las principales formas de representar la aplicación para los posibles clientes de la tienda.

Puede proporcionar [capturas de pantallas](#screenshots), [logotipos](#store-logos), [finalizadores](#trailers)y otros recursos de arte para incluirlos en la descripción de la tienda de la aplicación. Algunos de ellos son obligatorios y otros son opcionales (aunque es importante que algunas de las imágenes opcionales se incluyan en la mejor visualización de la tienda).

Durante el [proceso de envío](app-submissions.md)de la aplicación, debe proporcionar estos recursos de arte en el paso de los [listados de tiendas](create-app-store-listings.md) . Tenga en cuenta que las imágenes que se usan en el almacén y el modo en que aparecen, pueden variar según el sistema operativo del cliente y otros factores.

El almacén también puede usar el icono de la aplicación y otras imágenes que incluya en el paquete de la aplicación. Ejecute el [Kit](../debug-test-perf/windows-app-certification-kit.md) para la certificación de aplicaciones de Windows para determinar si faltan imágenes necesarias antes de enviar la aplicación. Para obtener instrucciones y recomendaciones sobre estas imágenes, consulte [iconos y logotipos](../design/style/app-icons-and-logos.md)de la aplicación.

## <a name="screenshots"></a>Capturas de pantalla

Las capturas de pantalla son imágenes de tu aplicación que se muestran a los clientes en la descripción de la Tienda de la aplicación.

Tiene la opción de proporcionar capturas de pantallas para las distintas familias de dispositivos que admite la aplicación, de modo que las capturas de pantallas adecuadas aparezcan cuando un cliente vea la lista de tiendas de la aplicación en ese tipo de dispositivo. 

Solo se necesita una captura de pantalla (para cualquier familia de dispositivos) para el envío, aunque puede proporcionar varios; hasta 9 capturas de pantallas de escritorio y hasta 8 capturas de pantallas para las demás familias de dispositivos. Se recomienda proporcionar al menos cuatro capturas de pantallas para cada familia de dispositivos que admita la aplicación para que los usuarios puedan ver el aspecto que tendrá la aplicación en el tipo de dispositivo. (No incluya capturas de pantallas de las familias de dispositivos que no admita la aplicación). Tenga en cuenta que las capturas de pantallas del **escritorio** también se mostrarán a los clientes en Surface Hub dispositivos.

> [!NOTE]
> Microsoft Visual Studio proporciona una [herramienta que le ayudará a capturar capturas de pantallas](/visualstudio/debugger/run-windows-store-apps-in-the-simulator#BKMK_Capture_a_screenshot_of_your_app_for_submission_to_the_Microsoft_Store).

Cada captura de pantalla debe ser un archivo. png en orientación horizontal o vertical, y el tamaño del archivo no puede ser mayor que 50 MB.

Los requisitos de tamaño pueden variar en función de la familia de dispositivos:
- Escritorio: 1366 x 768 píxeles o más. Admite imágenes de 4K (3840 x 2160). (También se mostrará a los clientes en dispositivos Surface Hub).
- Mobile: las imágenes deben ser una de las siguientes: 1080 x 1920, 1920 x 1080, 768 x 1280, 1280 x 768, 720 x 1280, 1280 x 720, 800 x 480 o 480 x 800 píxeles.
- Xbox: 3480 x 2160 píxeles o inferior. Admite imágenes de 4K (3840 x 2160).
- Holographic: 1268 x 720 píxeles o más. Admite imágenes de 4K (3840 x 2160).

Para obtener la mejor visualización, tenga en cuenta las siguientes directrices al crear las capturas de pantalla:
- Mantenga los objetos visuales críticos y el texto en la parte superior 3/4 de la imagen. Las superposiciones de texto pueden aparecer en la parte inferior 1/4. 
- No agregue logotipos, iconos ni mensajes de marketing adicionales a las capturas de pantallas.
- No use colores muy claros o oscuros ni bandas de contraste alto que puedan interferir con la legibilidad de las superposiciones de texto.

También puede proporcionar un título corto que describa cada captura de pantalla en 200 caracteres o menos.

> [!TIP]
> Las capturas de pantalla se muestran en la lista en orden. Después de cargar las capturas de pantallas, puede arrastrarlas y colocarlas para reordenarlas. 

Tenga en cuenta que si crea listas de tiendas para [varios idiomas](supported-languages.md), tendrá una página de **lista de tiendas** para cada una. Tendrás que subir imágenes para cada idioma por separado (incluso si vas a utilizar las mismas imágenes) y proporcionar títulos para cada uno de ellos. (Si tiene listas de almacenes en muchos idiomas, puede que le resulte más fácil actualizarlas mediante [la exportación de los datos de la lista y el trabajo sin conexión](import-and-export-store-listings.md)).


## <a name="store-logos"></a>Logotipos de Store

Puede cargar logotipos de la tienda para crear una pantalla más personalizada en el almacén. Se recomienda proporcionar estas imágenes para que la lista de tiendas se muestre de forma óptima en todos los dispositivos y versiones de sistema operativo compatibles con la aplicación. Tenga en cuenta que si la aplicación está disponible para los clientes en Xbox, algunas de estas imágenes son necesarias.

Puede proporcionar estas imágenes como archivos. png (no mayor que 50 MB), cada una de las cuales debe seguir las instrucciones que se indican a continuación.

### <a name="23-poster-art-720-x-1080-or-1440-x-2160-pixels"></a>imagen de póster 2:3 (720 x 1080 o 1440 x 2160 píxeles)

Se usa como la imagen principal del logotipo para clientes en dispositivos con Windows 10 y Xbox, por lo que se **recomienda encarecidamente** proporcionar esta imagen para garantizar una presentación adecuada. Es posible que la lista no tenga un aspecto correcto si no lo incluye, y no será coherente con otras listas que los clientes ven al examinar la tienda. Esta imagen también puede usarse en los resultados de la búsqueda o en colecciones editorially-seleccionada.

Esta imagen debe incluir el nombre de la aplicación y cualquier texto de la imagen debe cumplir los requisitos de legibilidad accesibles (relación de contraste de 4,51). Tenga en cuenta que las superposiciones de texto pueden aparecer en el trimestre inferior de esta imagen, por lo que debe asegurarse de que no incluye texto o imagen clave allí.

> [!NOTE]
> Si la aplicación está disponible para los clientes en Xbox, esta imagen es **obligatoria** y debe incluir el título del producto. El título debe aparecer en los tres últimos trimestres de la imagen, ya que pueden aparecer superposiciones de texto en el trimestre inferior de la imagen.

### <a name="11-box-art-1080-x-1080-or-2160-x-2160-pixels"></a>gráfico de caja 1:1 (1080 x 1080 o 2160 x 2160 píxeles)

Esta imagen puede aparecer en varias páginas de la tienda para Windows 10 (incluida Xbox) y, si no se proporciona la imagen de **gráfico de póster 2:3** , se usará como su logotipo principal. Esta imagen también debe incluir el nombre de la aplicación. Las superposiciones de texto pueden aparecer en el trimestre inferior de esta imagen, por lo que no incluya el texto o la imagen clave allí. Asegúrese de incluir el nombre de la aplicación en esta imagen. 

> [!NOTE]
> Si la aplicación está disponible para los clientes en Xbox, esta imagen es **obligatoria** y debe incluir el título del producto. El título debe aparecer en los tres últimos trimestres de la imagen, ya que pueden aparecer superposiciones de texto en el trimestre inferior de la imagen.

### <a name="11-app-tile-icon-300-x-300-pixels"></a>1:1 icono de icono de la aplicación (300 x 300 píxeles)

Esta imagen es necesaria para la presentación adecuada en Windows Phone 8,1 y versiones anteriores. Si la aplicación publicada anteriormente admite Windows Phone 8,1 o una versión anterior, y no proporciona esta imagen, los clientes verán un icono en blanco con la lista de la aplicación. (Esto también se aplica a los clientes de Windows 10 si la aplicación solo tiene paquetes que tienen como destino Windows Phone 8,1 o una versión anterior).

Si el envío *solo* incluye paquetes UWP, no es necesario que proporcione esta imagen (a menos que active la casilla para  **clientes en Windows 10 y Xbox), muestre las imágenes de logotipo cargadas en lugar de las imágenes de mis paquetes**, tal como se describe en la sección siguiente).

### <a name="display-only-uploaded-logo-images-in-the-store"></a>Mostrar solo las imágenes de logotipo cargadas en la tienda

Tiene la opción de evitar que el almacén use las imágenes de logotipo en los paquetes de la aplicación al mostrar la lista a los clientes de Windows 10 (incluido Xbox) y, en su lugar, hacer que el almacén use solo las imágenes que cargue. Esto le proporciona más control sobre la apariencia de la aplicación en varias pantallas de la tienda para clientes en Windows 10 (incluida Xbox). (Si la aplicación publicada anteriormente es compatible con versiones anteriores del sistema operativo, es posible que los clientes sigan viendo imágenes de los paquetes).

Para que el almacén use solo las imágenes que cargue (para los clientes de Windows 10, incluida Xbox), y no use ninguna imagen de los paquetes, active la casilla que indica los **clientes en Windows 10 y Xbox, y muestre las imágenes de logotipo cargadas en lugar de las imágenes de mis paquetes**.

Cuando activas esta casilla, aparece una nueva sección llamada **Store display images** (Imágenes de visualización en Microsoft Store). Aquí puede cargar 3 imágenes, incluido el tamaño del **icono de la aplicación 1:1 (300 x 300 píxeles)** (si activa la casilla, el campo para indicar que la imagen se moverá a esta sección). Se recomienda proporcionar los tres tamaños de imagen si usa esta opción: 300 x 300, 150 x 150 y 71 x 71 píxeles. Sin embargo, solo se requiere el tamaño 300 x 300.


<span id="promotional-images" />

## <a name="trailers-and-additional-assets"></a>Finalizadores y recursos adicionales

Esta sección le permite proporcionar ilustraciones para ayudar a mostrar el producto de forma más eficaz en la tienda. Se recomienda proporcionar estas imágenes para crear una lista de tiendas más invitadas.

> [!TIP]
> La imagen gráfica de gran [héroe 16:9](#windows-10-and-xbox-image-169-super-hero-art) se recomienda especialmente si tiene previsto incluir [finalizadores de vídeo](#trailers) en la lista de la tienda. Si no lo incluye, los finalizadores no aparecerán en la parte superior de la lista.


### <a name="trailers"></a>Clips finales

Los finalizadores son breves vídeos que ofrecen a los clientes una forma de ver su producto en acción, de modo que puedan comprender mejor lo que le gusta. Se muestran en la parte superior de la lista de tiendas de la aplicación (siempre y cuando incluya una imagen gráfica de gran tamaño en [16:9](#windows-10-and-xbox-image-169-super-hero-art) ). 

Los finalizadores se codifican con [Smooth streaming](https://www.iis.net/downloads/microsoft/smooth-streaming), que adapta la calidad de un flujo de vídeo entregado a los clientes en tiempo real según el ancho de banda disponible y los recursos de CPU.

> [!NOTE]
> Los finalizadores solo se muestran a los clientes de Windows 10, versión 1607 o posterior (que incluye Xbox).

### <a name="upload-trailers"></a>Carga de finalizadores

Puede Agregar hasta 15 finalizadores en la lista de la tienda. Asegúrese de cumplir todos los [requisitos](#trailer-requirements) que se enumeran a continuación.

Para cada finalizador que proporcione, debe cargar un archivo de vídeo (. MP4 o. mov), una imagen en miniatura y un título.

> [!IMPORTANT]
> Al usar finalizadores, también debe proporcionar una sección de imagen [gráfica de 16:9 súper Hero](#windows-10-and-xbox-image-169-super-hero-art) para que los finalizadores aparezcan en la parte superior de la lista de la tienda. Esta imagen aparecerá después de que los finalizadores hayan terminado de reproducirse.

Siga estas recomendaciones para que sus finalizadores sean eficaces:
- Los finalizadores deben ser de buena calidad y de longitud mínima (60 segundos o menos y menos de 2 GB recomendado). 
- Use una miniatura diferente para cada finalizador para que los clientes sepan que son únicos.
- Dado que algunos diseños de tienda pueden recortar ligeramente la parte superior e inferior del finalizador, asegúrese de que la información de clave aparece en el centro de la pantalla.
- La velocidad de fotogramas y la resolución deben coincidir con el material de origen. Por ejemplo, el contenido que se captura en 720p60 debe codificarse y cargarse en 720p60. 

También debe seguir los requisitos que se indican a continuación.

**Para agregar finalizadores a la lista:**
1. Cargue el **archivo de vídeo** del finalizador en el cuadro indicado. También se muestra un cuadro desplegable en caso de que quiera volver a usar un finalizador que haya cargado (quizás para una lista de tiendas en otro idioma).
2. Una vez cargado el finalizador, deberá cargar una **imagen en miniatura** para acompañarlo. Debe ser un archivo. png de 1920 x 1080 píxeles y, por lo general, una imagen que se toma del finalizador.
3. Haga clic en el icono de lápiz para agregar un **título** para el finalizador (255 caracteres como máximo).
4. Si desea agregar más finalizadores a la lista, haga clic en **Agregar finalizador** y repita los pasos indicados anteriormente.

> [!TIP]
> Si ha creado listas de tiendas en varios idiomas, puede seleccionar **elegir entre los finalizadores existentes** para volver a usar los finalizadores que ya ha cargado. No es necesario cargarlos individualmente para cada idioma.

Para quitar un finalizador de una lista, haga clic en la **X** junto a su nombre de archivo. Puede decidir si desea quitarlo solo de la lista de tiendas actual en la que está trabajando o quitarlo de todas las listas de la tienda del producto (en todos los idiomas).


### <a name="trailer-requirements"></a>Requisitos de finalizador

Al proporcionar los finalizadores, asegúrese de seguir estos requisitos:

- El formato de vídeo debe ser MOV o MP4.
- El tamaño de archivo del finalizador no debe superar los 2 GB.
- La resolución de vídeo debe ser 1920 x 1080 píxeles.
- La miniatura debe ser un archivo PNG con una resolución de 1920 x 1080 píxeles.
- El título no puede superar los 255 caracteres.
- No incluya clasificaciones de edad en los finalizadores.

> [!WARNING]
> La excepción al requisito de incluir la clasificación de edad en los finalizadores **solo** se aplica a los finalizadores de los **Microsoft Store** que se muestran **en la página del producto**. Cualquier finalizador publicado fuera del centro de Partners, que no está diseñado para mostrarse exclusivamente en la página del producto del Microsoft Store **debe** Mostrar información de clasificación insertada, cuando sea necesario, de acuerdo con las directrices de la autoridad de clasificación adecuada.  

Al igual que los demás campos de la página de la lista de tiendas, los finalizadores deben superar la certificación antes de publicarlos en el Microsoft Store. Asegúrese de que sus finalizadores cumplen con las [directivas de Microsoft Store](store-policies.md).

Hay requisitos adicionales en función del tipo de archivo.

#### <a name="mov"></a>MOV

| Vídeo | Audio | 
| --- | --- | 
| <ul><li>1080p ProRes (HQ, donde corresponda)</li><li>Velocidad de fotogramas nativa; 29,97 FPS preferidos</li></ul> | <ul><li>Se requiere estéreo</li><li>Nivel de audio recomendado:-16 LKFS/LUFS</li></ul> |


#### <a name="mp4"></a>MP4

| Vídeo | Audio |
| --- | --- |
| <ul><li>Códec: [H. 264](/windows/desktop/DirectShow/h-264-video-types) (avc1)  </li><li>Examen progresivo (sin entrelazado)</li><li>Perfil alto</li><li>2 fotogramas B consecutivos</li><li>GOP cerrado. GOP de la mitad de la velocidad de fotogramas</li><li>CABAC</li><li>50 MB/s </li><li>Espacio de colores: 4.2.0</li></ul> | <ul><li>Códec: AAC-LC</li><li>Canales: sonido estéreo o envolvente</li><li>Frecuencia de muestreo: 48 KHz</li><li>Velocidad de bits de audio: 384 KB/s para estéreo, 512 KB/s para sonido envolvente</li></ul> |

> [!WARNING]
> Es posible que los clientes no escuchen audio para archivos MP4 codificados con códecs distintos de AVC1.

En el caso de los archivos.
- Contenedor: MP4
- No hay listas de edición (o podría perder la sincronización de AV)
- Moov Atom al principio del archivo (inicio rápido)

### <a name="windows-10-and-xbox-image-169-super-hero-art"></a>Imagen de Windows 10 y Xbox (ilustración de 16:9 Super Hero)

En la sección **imagen de Windows 10 y Xbox** , se usa la imagen **16:9 súper hero Art (1920 x 1080 o 3840 x 2160 píxeles)** en varios diseños del Microsoft Store en todos los tipos de dispositivo de Windows 10 (incluido Xbox). Se recomienda proporcionar esta imagen, independientemente de las versiones del sistema operativo o los tipos de dispositivos a los que se dirige la aplicación.

Esta imagen es *necesaria* para que se muestren correctamente si la lista incluye [finalizadores de vídeo](#trailers). En el caso de los clientes de Windows 10, versión 1607 o posterior (que incluye Xbox), se usa como imagen principal en la parte superior de la lista de la tienda (o cuando finaliza la reproducción de los finalizadores). También se puede usar para incluir la aplicación en diseños promocionales en todo el almacén. Tenga en cuenta que esta imagen no debe incluir el título o el texto del producto.

Estas son algunas sugerencias que deben tenerse en cuenta al diseñar esta imagen:

- La imagen debe ser un. png que sea 1920 x 1080 píxeles o 3840 x 2160 píxeles.
- Seleccione una imagen dinámica relacionada con la aplicación para impulsar el reconocimiento y la diferenciación. Evita fotografías de catálogo o elementos visuales genéricos.
- No incluya texto en la imagen.
- Evite colocar elementos visuales clave en el tercio inferior de la imagen (ya que, en algunos diseños, podemos aplicar un degradado en esa parte).
- Coloque los detalles más importantes en el centro de la imagen (dado que en algunos diseños podemos recortar la imagen).
- Minimice el espacio vacío.
- Evita mostrar la interfaz de usuario de la aplicación y no uses imágenes específicas de dispositivo.
- Evita temas políticos y nacionales, banderas o símbolos religiosos.
- No incluyas imágenes de gestos desconsiderados, desnudez, apuestas, monedas, drogas, tabaco o alcohol.
- No uses armas apuntando hacia el usuario o violencia excesiva y sangre.

Aunque proporcionar esta imagen nos permite considerar la aplicación para las oportunidades promocionales destacadas, no garantiza que la aplicación se incluirá. Vea facilitar [la promoción de la aplicación](make-your-app-easier-to-promote.md) para obtener más información.


### <a name="xbox-images"></a>Imágenes de Xbox

Estas imágenes son necesarias para que se muestren correctamente si publica la aplicación en Xbox. 

Hay 3 tamaños diferentes que puede cargar:
- **Material gráfico de marca, 584 x 800 píxeles**: debe incluir el título del producto. En esta imagen se requiere una barra de personalización de marca. Mantenga el título y todas las imágenes clave en los tres últimos trimestres de la imagen, ya que puede aparecer una superposición en el trimestre inferior.
- **Titulado Hero Art, 1920 x 1080 píxeles**: debe incluir el título del producto. Mantenga el título y todas las imágenes clave en los tres últimos trimestres de la imagen, ya que puede aparecer una superposición en el trimestre inferior.
- **Arte de promoción destacado, 1080 x 1080 píxeles**: *no* debe incluir el título del producto.

> [!NOTE]
> Para obtener la mejor visualización en Xbox, también debe proporcionar una imagen **9:16 (720 x 1080 o 1440 x 2160 píxeles)** en la sección [logotipos](#store-logos) de la tienda.


### <a name="holographic-image"></a>Imagen holográfica

La imagen **2:1 (2400 x 1200)** solo se usa si la aplicación es compatible con la familia de dispositivos holográfica. Si es así, se recomienda proporcionar esta imagen.


<span id="optional-promotional-images" />

### <a name="images-only-for-windows-8x-andor-windows-phone-8x"></a>Imágenes solo para Windows 8. x y/o Windows Phone 8. x 

Si la aplicación enviada previamente es compatible con las versiones anteriores del sistema operativo (Windows 8. x y/o Windows Phone 8. x), estas imágenes se deben proporcionar para que podamos tener en cuenta la aplicación en diseños promocionales (aunque no garantizan que la aplicación se incluya). Si la aplicación no admite estas versiones anteriores del sistema operativo, omita esta sección. (Esta sección se llamaba anteriormente **imágenes promocionales opcionales**).

En **Windows Phone 8,1 y versiones anteriores**, se pueden usar dos tamaños de imagen en los diseños promocionales: **1000 x 800 píxeles (5:4)** y **358 x 358 píxeles (1:1)**. Si la aplicación se ejecuta en Windows Phone 8,1 o versiones anteriores, se recomienda proporcionar imágenes en ambos tamaños.  

> [!TIP]
> Asegúrese de proporcionar una imagen de icono de icono de la aplicación 300 x 300 en la sección [logotipos](#store-logos) de la tienda para cualquier envío que admita Windows Phone 8,1 o una versión anterior. Esto garantizará que la aplicación no aparezca en el almacén con un icono en blanco.  

**Por Windows 8.1 y versiones anteriores**, algunos diseños promocionales pueden utilizar una imagen con un tamaño de **414 x 180** píxeles. Si la aplicación se ejecuta en Windows 8.1 o versiones anteriores, se recomienda proporcionar una imagen en este tamaño.