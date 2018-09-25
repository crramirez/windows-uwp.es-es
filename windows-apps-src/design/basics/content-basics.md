---
author: mijacobs
Description: An overview of common page patterns and UI elements for displaying content in your UWP app.
title: Conceptos básicos del diseño de contenido para aplicaciones para la Plataforma universal de Windows (UWP)
ms.assetid: 3102530A-E0D1-4C55-AEFF-99443D39D567
label: Content design basics
template: detail.hbs
op-migration-status: ready
ms.author: mijacobs
ms.date: 12/1/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 91187b176d32fcc53b51faa7e8c42a10fd4908b8
ms.sourcegitcommit: 232543fba1fb30bb1489b053310ed6bd4b8f15d5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2018
ms.locfileid: "4178254"
---
# <a name="content-design-basics-for-uwp-apps"></a>Conceptos básicos del diseño de contenido para las aplicaciones para UWP

El propósito principal de cualquier aplicación es proporcionar acceso a contenido. Como hay aplicaciones con distintas finalidades, el contenido adquiere diversas formas: en una aplicación de edición de fotos, la fotografía es el contenido; en una aplicación de viajes, los mapas y la información acerca de los destinos de viaje son el contenido, etc. 

Este artículo proporciona una introducción a cómo puedes presentar el contenido en tu aplicación. Se describen patrones de página comunes y elementos de la interfaz de usuario que puedes usar para mostrar el contenido, sea cual sea la forma que tenga.

## <a name="common-page-patterns"></a>Patrones de página comunes

Muchas aplicaciones usan algunos o todos estos patrones de página comunes para mostrar diferentes tipos de contenido. De igual modo, no dudes en mezclar y combinar estos patrones para optimizar el contenido de la aplicación.

### <a name="landing"></a>Página de aterrizaje

![página de aterrizaje](images/content-basics/hero-screen.png)

Las páginas de aterrizaje, también conocida como pantallas prominentes, suele aparecer en el nivel superior de la experiencia de una aplicación. El gran área de superficie actúa como una fase para que las aplicaciones resalten el contenido que los usuarios puedan querer explorar y consumir.

### <a name="collections"></a>Colecciones

![galería](images/content-basics/gridview.png)

Las colecciones permiten a los usuarios examinar grupos de contenido o datos. La [vista de cuadrícula](../controls-and-patterns/item-templates-gridview.md) es una buena opción para las fotos o el contenido centrado en multimedia, y la [vista de lista](../controls-and-patterns/item-templates-listview.md) es una buena opción para el contenido con mucho texto o datos.

### <a name="hub"></a>Concentrador

![concentrador](images/content-basics/hub.png)

Los [concentradores](../controls-and-patterns/hub.md) están diseñados para la compra en ventanas. Los usuarios pueden ver un buen anticipo del contenido que se ofrece; lo más importante es mostrar una gran diversidad de contenido pero en una cantidad breve. Por ejemplo, la sección 1 de un concentrador podría contener una pantalla prominente, la sección 2 podría contener una colección, la sección 3 podría contener otra colección, etc.

### <a name="masterdetail"></a>Maestro y detalles

![maestro y detalles](images/content-basics/master-detail.png)

El modelo [maestro y detalles](../controls-and-patterns/master-details.md) consta de una vista de lista (maestra) y una vista de contenido (detalles). Ambos paneles son fijos y tienen un desplazamiento vertical. Hay una relación clara entre el elemento de lista y la vista de contenido: el elemento de la vista maestra se selecciona y la vista de detalles se actualiza en consonancia. Además de proporcionar navegación por la vista de detalles, los elementos de la vista maestra pueden agregarse y quitarse.

### <a name="details"></a>Detalles

![varias vistas](images/multi-view.png)

Cuando los usuarios encuentren el contenido que buscan, piensa en la posibilidad de crear páginas de visualización de contenido para que puedan ver la página sin distracciones. Si es posible, [crea una opción de vista de pantalla completa](../layout/show-multiple-views.md) que expanda el contenido hasta llenar toda la pantalla y oculte todos los demás elementos de la interfaz de usuario. 

Para ajustar los cambios del tamaño de la pantalla, también podrías crear un [diseño adaptativo](design-and-ui-intro.md) que oculte o muestre elementos de interfaz de usuario según corresponda.

### <a name="forms"></a>Formularios
![formulario](images/content-basics/forms.png)

Un [formulario](../controls-and-patterns/forms.md) es un grupo de controles que recopila y envía datos de los usuarios. La mayoría, si no todas las aplicaciones, usan algún tipo de formulario para las páginas de configuración, iniciar sesión en portales, los centros de comentarios, la creación de cuentas u otros fines. 

## <a name="common-content-elements"></a>Elementos de contenido comunes

Para crear estos patrones de página, debes usar una combinación de elementos de contenido individuales. A continuación se facilitan algunos elementos de interfaz de usuario que se utilizan frecuentemente para mostrar contenido. (Para obtener una lista completa de los elementos de la interfaz de usuario, consulta [controles y patrones](../controls-and-patterns/index.md)).

<div class="mx-responsive-img">
<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Categoría</th>
<th align="left">Elementos</th>
<th align="left">Descripción</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Audio y vídeo<br/><br/>
    <img src="images/content-basics/media-transport.png" alt="media transport control" /></td>
<td align="left"><a href="../controls-and-patterns/media-playback.md">Controles de transporte y reproducción de contenido multimedia</a></td>
<td align="left">Reproduce audio y vídeo.</td>
</tr>
<tr class="even">
<td align="left">Visores de imágenes<br/><br/>
    <img src="images/content-basics/flipview.jpg" alt="flip view" /></td>
<td align="left"><a href="../controls-and-patterns/flipview.md">Vista invertida</a>, <a href="../controls-and-patterns/images-imagebrushes.md">imagen</a></td>
<td align="left">Muestra imágenes. El control de vista de volteo muestra una a una las imágenes de una colección, como las fotos de un álbum o los elementos de una página de detalles de un producto.</td>
</tr>
<tr class="odd">
<td align="left">Colecciones <br/><br/>
    <img src="images/content-basics/listview.png" alt="list view" /></td>
<td align="left"><a href="../controls-and-patterns/lists.md">Vista de lista y vista de cuadrícula</a></td>
<td align="left">Presenta los elementos en una lista interactiva o una cuadrícula. Usa estos elementos para permitir a los usuarios seleccionar una película de una lista de nuevos lanzamientos o administrar un inventario.</td>
</tr>
<tr class="even">
<td align="left">Texto y entrada de texto <br/><br/>
    <img src="images/content-basics/textbox.png" alt="text box" /></td>
<td align="left"><p><a href="../controls-and-patterns/text-block.md">Bloque de texto</a>, <a href="../controls-and-patterns/text-box.md">cuadro de texto</a>, <a href="../controls-and-patterns/rich-edit-box.md">cuadro de edición enriquecido</a></p>
</td>
<td align="left">Muestra texto. Algunos elementos permiten al usuario editar texto. Para obtener más información, consulta <a href="../controls-and-patterns/text-controls.md">Controles de texto</a>.
<p>Para obtener instrucciones sobre cómo mostrar texto, consulta [Tipografía](../style/typography.md).</p>
</td>
</tr>
<tr class="odd">
<td align="left">Mapas<br/><br/>
    <img src="images/content-basics/mapcontrol.png" alt="map control" /></td>
<td align="left"><a href="../../maps-and-location/display-maps.md">MapControl</a></td>
<td align="left">Muestra un mapa simbólico o fotorrealista de la Tierra.</td>
</tr>
<tr class="even">
<td align="left">WebView</td>
<td align="left"><a href="../controls-and-patterns/web-view.md">WebView</a></td>
<td align="left">Representa contenido HTML.</td>
</tr>
</tbody>
</table>
</div>
