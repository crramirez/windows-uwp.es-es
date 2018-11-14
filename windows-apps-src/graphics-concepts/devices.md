---
title: Dispositivos
description: Un dispositivo de Direct3D es el componente de representación de Direct3D. Un dispositivo encapsula y almacena el estado de representación, transforma, ilumina y rasteriza una imagen en una superficie.
ms.assetid: BC903462-A32A-46BA-8411-FB294F5D2CD9
keywords:
- Dispositivos
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d1c35af3dd1f8826fbd61268c5c47cef9d77146a
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/09/2018
ms.locfileid: "6181701"
---
# <a name="devices"></a>Dispositivos


Un dispositivo de Direct3D es el componente de representación de Direct3D. Un dispositivo encapsula y almacena el estado de representación, transforma, ilumina y rasteriza una imagen en una superficie.

Desde el punto de vista de la arquitectura, los dispositivos de Direct3D contienen un módulo de transformación, un módulo de iluminación y un módulo de rasterización, como se muestra en el siguiente diagrama.

![diagrama de la arquitectura del dispositivo de Direct3D](images/d3ddev.png)

Direct3D admite dos tipos principales de dispositivos de Direct3D:

-   Un dispositivo HAL con rasterización acelerada por hardware y sombreado con el procesamiento del vértice de hardware y software
-   Un dispositivo de referencia

Estos dispositivos son dos controladores independientes. Los dispositivos de referencia y de software se representan mediante controladores de software, y el dispositivo HAL se representa mediante un controlador de hardware. La manera más común de sacar provecho de estos dispositivos es usar el dispositivo HAL para el envío de aplicaciones, y el dispositivo de referencia para probar características. Estos los proporcionan terceros para emular dispositivos concretos, por ejemplo, hardware de desarrollo que aún no se ha lanzado.

El dispositivo de Direct3D que crea una aplicación debe coincidir con las funcionalidades del hardware en el que se ejecuta la aplicación. Direct3D proporciona funcionalidades de representación, ya que accede a hardware 3D que está instalado en el equipo o emula las funcionalidades de hardware 3D en el software. Por lo tanto, Direct3D proporciona dispositivos tanto para el acceso al hardware como para la emulación de software.

Los dispositivos acelerados por hardware proporcionan un rendimiento mucho mejor que los dispositivos de software. El tipo de dispositivo HAL está disponible en todos los adaptadores de gráficos que admite Direct3D. En la mayoría de los casos, las aplicaciones están destinadas a equipos que tienen aceleración de hardware y dependen de emulación de software, a fin de abarcar equipos de gama baja.

A excepción del dispositivo de referencia, los dispositivos de software no siempre admiten las mismas características que un dispositivo de hardware. Las aplicaciones siempre deberían consultar las funcionalidades del dispositivo para determinar qué características son compatibles.

Dado que el comportamiento de los dispositivos de referencia y de software proporcionados con Direct3D 9 es idéntico al del dispositivo HAL, el código de la aplicación creado para funcionar con el dispositivo HAL funcionará con los dispositivos de software o de referencia sin modificaciones. El comportamiento de los dispositivos de referencia o de software proporcionados es idéntico al del dispositivo HAL, pero varían las funcionalidades del dispositivo, y un dispositivo determinado de software puede implementar un conjunto de funcionalidades mucho más pequeño.

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>En esta sección


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Tema</th>
<th align="left">Descripción</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="device-types.md">Tipos de dispositivos</a></p></td>
<td align="left"><p>Los tipos de dispositivos de Direct3D incluyen dispositivos de la capa de abstracción de hardware (HAL) y el rasterizador de referencia.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="windowed-vs--full-screen-mode.md">Modo de ventana frente a modo de pantalla completa</a></p></td>
<td align="left"><p>Las aplicaciones de Direct3D pueden ejecutarse en cualquiera de los dos modos de pantalla: completa o de ventana. En el <em>modo de ventana</em>, la aplicación comparte el espacio de pantalla del escritorio disponible con todas las aplicaciones en ejecución. En el <em>modo de pantalla completa</em>, la ventana en la que la aplicación se ejecuta abarca todo el escritorio, ocultando todas las aplicaciones en ejecución (incluido el entorno de desarrollo).</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="lost-devices.md">Dispositivos perdidos</a></p></td>
<td align="left"><p>Un dispositivo de Direct3D puede estar en un estado operativo o perdido. El estado <em>operativo</em> es el estado normal del dispositivo, en el que el dispositivo ejecuta y presenta toda la representación según lo esperado. El dispositivo realiza una transición al estado <em>perdido</em> cuando un evento, como la pérdida de foco del teclado en una aplicación de pantalla completa, hace que la representación sea imposible.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="swap-chains.md">Cadenas de intercambio</a></p></td>
<td align="left"><p>Una cadena de intercambio es una colección de búferes que se usan para mostrar fotogramas al usuario. Cada vez que una aplicación presenta un nuevo marco para mostrar, el primer búfer de la cadena de intercambio toma el lugar del búfer que se muestra. Este proceso se denomina <em>intercambio</em> o <em>inversión</em>.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="introduction-to-rasterization-rules.md">Introducción a las reglas de rasterización</a></p></td>
<td align="left"><p>A menudo, los puntos especificados para los vértices no coinciden con precisión con los píxeles en la pantalla. Cuando esto ocurre, Direct3D aplica reglas de rasterización de triángulo para decidir qué píxeles se aplican a un triángulo determinado.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Guía de aprendizaje de gráficos de Direct3D](index.md)

 

 




