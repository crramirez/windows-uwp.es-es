---
author: mijacobs
Description: Good icons harmonize with typography and with the rest of the design language. They don’t mix metaphors, and they communicate only what’s needed, as speedily and simply as possible.
title: Iconos
ms.assetid: b90ac02d-5467-4304-99bd-292d6272a014
label: Icons
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
design-contact: Judysa
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 61157ad23eb55447137531922ea23fa0120e2b98
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/15/2018
ms.locfileid: "1653924"
---
# <a name="icons-for-uwp-apps"></a>Iconos de aplicaciones para UWP



Los iconos adecuados armonizan con la tipografía y con el resto del lenguaje de diseño. No mezclan metáforas y comunican solo lo necesario de la manera más rápida y simple posible. 

## <a name="linear-scaling-size-ramps"></a>Tablas de tamaño de escalado lineal 

<table>
    <tr> 
        <td>16 x 16 píxeles</td>
        <td>24 x 24 píxeles</td>
        <td>32 x 32 píxeles</td>
        <td>48 x 48 píxeles</td>
    </tr>
    <tr> 
        <td><img src="images/icons-16x16.png" alt="Icons at 16x16 effective pixels" /></td>
        <td><img src="images/icons-24x24.png" alt="Icons at 24x24 effective pixels" /></td>
        <td><img src="images/icons-32x32.png" alt="Icons at 32x32 effective pixels" /></td>
        <td><img src="images/icons-48x48.png" alt="Icons at 48x48 effective pixels" /></td>
    </tr>
</table>

## <a name="common-shapes"></a>Formas comunes

Por lo general, los iconos deben maximizar el espacio disponible dejando poco libre. Estas formas proporcionan puntos de partida para cambiar el tamaño de formas básicas. 

![Cuadrícula de 32 x 32 píxeles](images/icons-common-shapes.png)

Usa la forma que corresponda con la orientación del icono y compón alrededor de estos parámetros básicos. No es necesario que los iconos rellenen o se ajusten completamente a la forma y pueden modificarse según sea necesario para garantizar un equilibrio óptimo. 

<table class="uwpd-noborder">
    <tr>
        <td>Círculo<td>
        <td>Cuadrado</td>
        <td>Triángulo</td>
    </tr>
    <tr>
        <td><img src="images/icons-common-shapes-examples-1.png" alt="A circle" /><td>
        <td><img src="images/icons-common-shapes-examples-2.png" alt="A square" /></td>
        <td><img src="images/icons-common-shapes-examples-3.png" alt="A triangle " /></td>
    </tr>
        <tr>
        <td>Rectángulo horizontal<td>
        <td colspan="2">Rectángulo vertical</td>        
        </tr>
    <tr>
        <td><img src="images/icons-common-shapes-examples-4.png" alt="A horizontal rectangle" /><td>
        <td colspan="2"><img src="images/icons-common-shapes-examples-5.png" alt="A vertical rectangle" /></td>
         
    </tr>

</table>

## <a name="angles"></a>Ángulos

Además de usar la misma cuadrícula y el mismo grosor de línea, los iconos se construyen con elementos comunes. 

Si se usan solo estos ángulos para crear formas, se consigue coherencia entre todos los iconos y garantiza que los iconos se representen correctamente. 

Estas líneas pueden combinarse, unirse, girarse y reflejarse en la creación de iconos. 

<table>
    <tr>
        <td><b>1:1</b><br/>45°</td>
        <td><b>1:2</b><br />26,57° (vertical)<br/>63,43° (horizontal)</td>
        <td><b>1:3</b><br/>18,43° (vertical)<br/>71,57° (horizontal)</td>
        <td><b>1:4</b><br/>14,04° (vertical)<br/>75,96° (horizontal)</td>
    </tr>
    <tr>
        
        <td><img src="images/icons-grid-1-1.png" alt="1:1" /></td>
        <td><img src="images/icons-grid-1-2.png" alt="1:2" /></td>
        <td><img src="images/icons-grid-1-3.png" alt="1:3" /></td>
        <td><img src="images/icons-grid-1-4.png" alt="1:4" /></td>
    </tr>  
</table>

<p>A continuación, se muestran algunos ejemplos:</p>

<table>
    <tr>
        <td><img src="images/icons-angles-examples-1.png" alt="A 1:1 angle example" /></td>
        <td><img src="images/icons-angles-examples-2.png" alt="A 1:2 angle example" /></td>
        <td><img src="images/icons-angles-examples-3.png" alt="A 1:3 angle example" /></td>
        <td><img src="images/icons-angles-examples-4.png" alt="A 1:4 angle example" /></td>
    </tr>
</table>

## <a name="curves"></a>Curvas

Las líneas curvas se crean a partir de las secciones de un círculo completo y no se deben sesgar a menos que sea necesario para ajustarlas a la cuadrícula de píxeles. 

<table>
    <tr>
        <td>1/4 de círculo</td>
        <td>1/8 de círculo</td>
    </tr>
    <tr>
        <td><img src="images/icons-curves-14circle.png" alt="1/4 circle" /></td>
        <td><img src="images/icons-curves-18circle.png" alt="1/8 circle" /></td>
    </tr>
    <tr>
        <td><img src="images/icons-curves-examples-1.png" alt="1/4 cirlce example" /></td>
        <td><img src="images/icons-curves-examples-2.png" alt="1/8 circle example" /></td>
    </tr>    
</table>

## <a name="geometric-construction"></a>Construcción geométrica

Recomendamos el uso de formas geométricas puras para construir iconos.

![Icono de guitarra con superposición de formas geométricas ](images/icons-geometric-construction.png)

## <a name="filled-shapes"></a>Formas rellenas 

Los iconos pueden contener formas rellenas cuando sea necesario, pero no deben superar los 4 píxeles en 32 × 32 píxeles. Los círculos rellenos no deben ser mayores de 6 × 6 píxeles. 

![Relleno de 5 x 8 píxeles ](images/icons-filled-shapes.png)

## <a name="badges"></a>Distintivos

Una "distintivo" es un término genérico que se usa para describir un elemento agregado a un icono que no se ha diseñado para integrarse con el elemento base del icono. Por lo general, estos transmiten más información sobre el icono, como el estado o la acción. Otros términos comunes son: superposición, anotación o modificador. 

![Distintivo de estado ](images/icons-badge-status.png)

![Distintivo de acción ](images/icons-badge-action.png)

Los distintivos de estado se representan con un objeto relleno de color encima del icono, mientras que los distintivos de acción están integrados en el icono con el mismo estilo monocromático y grosor de línea.

<table>
<tr>
    <td>Distintivos de estado comunes</td>
    <td>Distintivos de acción comunes</td>
</tr>
<tr>
    <td><img src="images/icons-badge-common-states-1.png" alt="Status badge " /></td>
    <td><img src="images/icons-badge-common-states-2.png" alt="Action badge " /></td>
</tr>
</table>
<p></p>

### <a name="badge-color"></a>Color de los distintivos 

El color de un distintivo solo debe usarse para transmitir el estado de un icono. Los colores usados en los distintivos de estado transmiten mensajes emocionales específicos para el usuario. 

<table>
<tr><td>Verde - #128B44</td><td>Azul - #2C71B9</td><td>Amarillo - #FDC214</td></tr>
<tr><td>Positivo: listo, completado </td><td>Neutro: ayuda, notificación </td><td>Precaución: alerta, advertencia </td></tr>
<tr><td><img src="images/icons-color-inbadging-1.png" alt="Green status" /></td><td><img src="images/icons-color-inbadging-2.png" alt="Blue status" /></td>
<td><img src="images/icons-color-inbadging-3.png" alt="Yellow status" /></td></tr>
</table>
<p></p>

### <a name="badge-position"></a>Posición de los distintivos

La posición predeterminada de cualquier distintivo de estado o acción es la esquina inferior derecha. Usa las demás posiciones solo si el diseño no permite la predeterminada. 

### <a name="badge-sizing"></a>Cambio de tamaño de los distintivos

Los distintivos deben tener un tamaño de entre 10 y 18 píxeles en una cuadrícula de 32 x 32 píxeles. 

## <a name="related-articles"></a>Artículos relacionados

* [Directrices sobre los activos de icono y de ventanas](../shell/tiles-and-notifications/app-assets.md)
