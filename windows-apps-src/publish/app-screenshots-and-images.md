---
author: jnHs
Description: "La aplicación debe incluir varios logotipos, capturas de pantalla e imágenes."
title: "Imágenes y capturas de pantalla de aplicación"
ms.assetid: D216DD2B-F43D-4D26-82EE-0CD34DB929D8
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 2fbbaac1b6b0f3133b9d541d6887ceb6c379f520
ms.lasthandoff: 02/07/2017

---

# <a name="app-screenshots-and-images"></a>Imágenes y capturas de pantalla de aplicación


La aplicación debe incluir varios logotipos, capturas de pantalla e imágenes. Algunos son obligatorios y otros opcionales. Recuerda que las imágenes son una de las principales formas en que representas la aplicación. Disponer de imágenes bien diseñadas puede contribuir en gran medida al atractivo de la aplicación para los clientes.

Durante el [proceso de envío de la aplicación](app-submissions.md), proporcionas las [capturas de pantalla](#screenshots) y el [material gráfico promocional](#promotional-artwork) en el paso [Descripciones de la Tienda](create-app-store-listings.md). Estas imágenes se usan para mostrar la aplicación en la Tienda.

La Tienda también usa el icono de la aplicación y otras imágenes que incluyas en el paquete de la aplicación. Ejecuta el [Kit para la certificación de aplicaciones en Windows](https://msdn.microsoft.com/library/windows/apps/mt186449) para determinar si te falta alguna de las imágenes necesarias. Para obtener instrucciones y recomendaciones acerca de estas imágenes, consulta [Activos de mosaico y de icono](../controls-and-patterns/tiles-and-notifications-app-assets.md).

> **Nota** La manera en que las imágenes se usan en la Tienda, en la pantalla Inicio del cliente y dentro de la propia aplicación puede variar en función del sistema operativo del cliente y de otros factores.


## <a name="images-provided-during-the-submission-process"></a>Imágenes proporcionadas durante el proceso de envío

Al especificar la información de descripción de la Tienda de la aplicación, tienes la opción de proporcionar varias capturas de pantalla (una captura de pantalla es obligatoria) y material gráfico promocional. Estas imágenes no se toman del paquete de la aplicación; deberás proporcionarlas en el paso **Descripción de la Tienda** para cada uno de tus idiomas.

En la siguiente tabla se indican las distintas imágenes que puedes subir y se explica cómo se usan. En las secciones siguientes se proporcionan más detalles.

| Imagen                                                       | Tamaño en píxeles                           | Uso                                                                                                                                                                           | Cuándo se incluye                                                                                                                                            |
|-------------------------------------------------------------|--------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Capturas de pantalla del escritorio](#screenshots)                         | 1366 x 768 o superior                 | Se muestra en la descripción de la aplicación en la Tienda cuando se visualiza en un dispositivo de escritorio.                                                                                                          | Se recomienda para todas las aplicaciones, especialmente si la aplicación está pensada para usarse en dispositivos de escritorio. Se necesita al menos una captura de pantalla (para cualquier familia de dispositivos). |
| [Capturas de pantalla para móviles](#screenshots)                          | 768 x 1280, 720 x 1280 o 480 x 800 | Se muestra en la descripción de la aplicación en la Tienda cuando se visualiza en un dispositivo móvil.                                                                                                           | Se recomienda para todas las aplicaciones, especialmente si la aplicación está pensada para usarse en dispositivos móviles. Se necesita al menos una captura de pantalla (para cualquier familia de dispositivos). |
| [Capturas de pantalla holográficas](#screenshots)                          | 1268 x 720 o superior                 | Se muestra en la descripción de la aplicación en la Tienda cuando se visualiza en un dispositivo holográfico.                                                                                                           | Se recomienda si la aplicación está diseñada para Microsoft HoloLens. Se necesita al menos una captura de pantalla (para cualquier familia de dispositivos). |
| [Icono de la aplicación](#app-tile-icon)                             | 300 x 300                            | Se muestra como el icono de la aplicación en la Tienda para Windows Phone 8.1 y versiones anteriores (y, si solo tienes paquetes destinados a Windows Phone 8.1 o versiones anteriores, en la Tienda de Windows 10). | Se requiere para la visualización correcta en la Tienda si la aplicación está destinada a Windows Phone 8.1 o versiones anteriores.                                                                 |
| [Imagen promocional: contenido destacado de Windows 10](#promotional-artwork) | 2400 x 1200                          | Se usa para diseños promocionales en la Tienda de Windows 10.                                                                                                                        | Se recomienda para todas las aplicaciones, especialmente aquellas con paquetes para UWP destinados a los clientes de Windows 10.                                                               |
| [Imágenes promocionales: Windows Phone 8.1 y versiones anteriores](#promotional-artwork) | 1000 x 800, 358 x 358                | Se usa para diseños promocionales en la Tienda de Windows Phone 8.1 y versiones anteriores.                                                                                                     | Se recomienda para todas las aplicaciones destinadas a Windows Phone 8.1 o versiones anteriores.                                                                                           |
| [Imágenes promocionales: Windows 8.1 y versiones anteriores](#promotional-artwork)        | 414 x 180                            | Se usa para diseños promocionales en la Tienda de Windows 8.1.                                                                                                                       | Se recomienda para todas las aplicaciones destinadas a Windows 8.1 o versiones anteriores.                                                                                                 |
 

## <a name="screenshots"></a>Capturas de pantalla

Las capturas de pantalla son imágenes de tu aplicación que se muestran a los clientes en la descripción de la Tienda de la aplicación.

En la página **Descripción de la Tienda**, verás varios campos en los que tendrás la opción de proporcionar capturas de pantalla para las distintas familias dispositivos (se mostrarán cuando un cliente vea la descripción de la Tienda de la aplicación en ese tipo de dispositivo).

Solo se necesita una captura de pantalla para el envío (aunque puedes proporcionar varias; hasta 9 capturas de pantalla de equipos de escritorio y hasta 8 de dispositivos móviles y holográficas). No hace falta proporcionar capturas de pantalla individuales para cada tipo de dispositivo, pero es recomendable proporcionar capturas de pantalla de todos los tipos de dispositivos que admita la aplicación. De este modo, los clientes verán imágenes similares al aspecto que tendrá la aplicación en sus dispositivos.

> **Nota** Microsoft Visual Studio proporciona una [herramienta para ayudarte a obtener estas capturas de pantalla](http://go.microsoft.com/fwlink/p/?LinkId=221135).

Cada captura de pantalla debe ser un archivo .png en orientación horizontal o vertical y el tamaño del archivo no puede ser superior a 2 MB.

Los requisitos de tamaño pueden variar en función de la familia de dispositivos:
- Móvil: 768 x 1280, 720 x 1280 o 480 x 800 píxeles
- Escritorio: 1366 x 768 píxeles o superior
- Holográfica: 1268 x 720 píxeles o superior

Puedes proporcionar un título corto que describa cada captura de pantalla en 200 caracteres o menos.

> **Nota** Si creas descripciones de la Tienda para [varios idiomas](supported-languages.md), tendrás una página de **Descripción de la Tienda** para cada uno de ellos. Tendrás que subir imágenes para cada idioma por separado (incluso si vas a utilizar las mismas imágenes) y proporcionar títulos para cada uno de ellos.


## <a name="app-tile-icon"></a>Icono de la aplicación

Esto no es necesario para todos los envíos, pero es muy recomendable si la aplicación se ejecuta en Windows Phone 8.1 o en versiones anteriores. El icono de la aplicación se usa cuando se muestra la descripción de la Tienda de la aplicación a los clientes en Windows Phone 8.1 y versiones anteriores. Si no proporcionas esta imagen, los clientes de Windows Phone 8.1 o versiones anteriores verán un icono en blanco junto con la descripción de la aplicación. (Esto también se aplica a los clientes de Windows 10, si tu aplicación solo tiene paquetes destinados a Windows Phone 8.1 o versiones anteriores).

Si tu envío **únicamente** incluye paquetes para UWP, no es necesario proporcionar esta imagen. Ten en cuenta que si tu envío incluye paquetes para UWP y proporcionas un icono de la aplicación, en algunos diseños de la Tienda dicho icono puede mostrarse junto con la descripción de la aplicación en Windows 10. Para impedir completamente que el icono de la aplicación se muestre a los clientes en Windows 10, puedes crear una [descripción específica de la plataforma](create-platform-specific-descriptions.md) para las versiones anteriores del sistema operativo e incluir dicho icono de la aplicación solo ahí.

El icono de la aplicación debe ser un archivo .png con una medida de 300 x 300 píxeles.

## <a name="promotional-artwork"></a>Material gráfico de promoción

El equipo editorial de la Tienda Windows usa imágenes diferentes para mostrar aplicaciones en la Tienda. El envío de material gráfico promocional permite al equipo de la Tienda Windows tener en cuenta tu aplicación en diseños promocionales.

> **Importante** El hecho de proporcionar imágenes promocionales de la aplicación no garantiza que esta se muestre, pero si no las incluyes, no se podrá tener en cuenta para todas las oportunidades de promoción. (Consulta [Facilitar la promoción de la aplicación](make-your-app-easier-to-promote.md) para más información).

Es recomendable enviar material gráfico promocional en distintos tamaños, según las versiones del sistema operativo a las que se dirige tu aplicación. Para todos los tamaños, las imágenes deben estar en formato .png.

Aquí te sugerimos algunos aspectos que debes tener en cuenta al diseñar tu material gráfico promocional:

-   Selecciona imágenes dinámicas relacionadas con la aplicación y que impulsan el reconocimiento y la diferenciación. Evita fotografías de catálogo o elementos visuales genéricos.
-   No incluyas texto (aparte de la personalización de marca).
-   Minimiza el espacio vacío en la imagen.
-   Evita mostrar la interfaz de usuario de la aplicación y no uses imágenes específicas de dispositivo.
-   Evita temas políticos y nacionales, banderas o símbolos religiosos.
-   No incluyas imágenes de gestos desconsiderados, desnudez, apuestas, monedas, drogas, tabaco o alcohol.
-   No uses armas apuntando hacia el usuario o violencia excesiva y sangre.

### <a name="for-windows-10-2400-x-1200"></a>Para Windows 10: 2400 x 1200

En la Tienda de Windows 10, en la parte superior de las páginas de categorías Aplicaciones y Juegos se incluye una imagen giratoria de productos destacados para promover el contenido. Para que la aplicación sea apta para esta colocación de contenido destacado, asegúrate de enviar una imagen 2400 x 1200.

Al diseñar tu imagen, ten en cuenta que, si la usamos el contenido destacado, aplicaremos un degradado por el tercio inferior para que podamos mostrar de forma legible el texto de marketing sobre la imagen. Por este motivo, evita colocar texto y elementos visuales clave en el tercio inferior de la imagen. Además, es posible que recortemos tu imagen, por lo que recomendamos que coloques la información de marca y los detalles más importantes de tu aplicación en el centro.

En la siguiente imagen se muestran las proporciones clave que deberías tener en cuenta. La "zona segura" del centro se destacará, aunque recortemos la imagen. La "zona de texto dinámico"" es donde pueden aparecer texto y un degradado.

![imagen de directrices de contenido destacado](images/spotlight1.jpg) En el siguiente ejemplo se muestra una imagen de contenido destacado bien diseñada que sigue estas directrices. (Las líneas en la imagen sirven para ilustrar cómo el material gráfico se ajusta a las zonas designadas y no se incluirían en la imagen final).

![Imagen de contenido destacado bien diseñada](images/spotlight2.jpg)
### <a name="for-windows-phone-81-and-earlier-1000-x-800-358-x-358"></a>Para Windows Phone 8.1 y versiones anteriores: 1000 x 800, 358 x 358

En la Tienda de Windows Phone 8.1 y versiones anteriores, se pueden usar dos tamaños de imagen en diseños promocionales: 1000 x 800 píxeles y 358 x 358 píxeles. Si la aplicación se ejecuta en Windows Phone 8.1 o versiones anteriores, te recomendamos que proporciones imágenes en estos dos tamaños para la consideración promocional.

> **Sugerencia** Asegúrate de proporcionar una [imagen de icono de la aplicación](#app-tile-icon) de 300 x 300 para que la aplicación no aparezca en la Tienda con un icono en blanco.

### <a name="for-windows-81-and-earlier-414-x-180"></a>Para Windows 8.1 y versiones anteriores: 414 x 180

En la Tienda de Windows 8.1 y versiones anteriores, los diseños promocionales pueden usar una imagen con un tamaño de 414 x 180 píxeles. Si tu aplicación se ejecuta en Windows 8.1 o versiones anteriores, te recomendamos que proporciones una imagen de este tamaño para la consideración promocional.

