---
title: Modos de direccionamiento de las texturas
description: La aplicación de Direct3D puede asignar coordenadas de textura a todos los vértices de cualquier primitivo.
ms.assetid: 925E8F2E-43EC-404E-8870-03E39155F697
keywords:
- Modos de direccionamiento de las texturas
- Modo de direccionamiento de las texturas Wrap
- Modo de direccionamiento de las texturas Mirror
- Modo de direccionamiento de las texturas Clamp
- Modo de direccionamiento de las texturas Border Color
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 5e263876f414e5683ffc8a5645a12e5031b3d6fb
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2018
ms.locfileid: "8480287"
---
# <a name="texture-addressing-modes"></a>Modos de direccionamiento de las texturas


La aplicación de Direct3D puede asignar coordenadas de textura a todos los vértices de cualquier primitivo. Por lo general, las coordenadas de textura u y v que se asignan a un vértice se encuentran en el intervalo de 0.0 a 1.0, ambos incluidos. Sin embargo, si se asignan coordenadas de textura fuera de ese intervalo, puedes crear determinados efectos especiales de texturas. .

Lo que hace Direct3D con las coordenadas de textura que se encuentran fuera del intervalo \[0.0, 1.0\] se controla estableciendo el modo de direccionamiento de las texturas. Por ejemplo, puedes hacer que tu aplicación establezca el modo de direccionamiento de las texturas de manera que una textura se coloque en mosaico en un primitivo.

Direct3D permite que las aplicaciones realicen el ajuste de la textura. Consulta [Ajuste de texturas](texture-wrapping.md).

Si se habilita el ajuste de la textura de forma efectiva, las coordenadas de textura situadas fuera del intervalo \[0.0, 1.0\] no son válidas y el comportamiento para rasterizar estas coordenadas de textura no válidas no está definido en este caso. Cuando se habilita el ajuste de las texturas, no se usan modos de direccionamiento de las texturas. Ten cuidado para que la aplicación no especifique coordenadas de textura inferiores a 0.0 ni superiores a 1.0 cuando el ajuste de las texturas esté habilitado.

## <a name="span-idsummaryofthetextureaddressingmodesspanspan-idsummaryofthetextureaddressingmodesspanspan-idsummaryofthetextureaddressingmodesspansummary-of-the-texture-addressing-modes"></a><span id="Summary_of_the_texture_addressing_modes"></span><span id="summary_of_the_texture_addressing_modes"></span><span id="SUMMARY_OF_THE_TEXTURE_ADDRESSING_MODES"></span>Resumen de los modos de direccionamiento de las texturas


| Modo de direccionamiento de las texturas | Descripción                                                                                                                           |
|-------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
| Wrap                    | Repite la textura en la unión de cada entero.                                                                                        |
| Mirror                  | Refleja la textura en el límite de cada entero.                                                                                        |
| Clamp                   | Comprime las coordenadas de la textura en el intervalo \[0.0, 1.0\]; el modo Clamp aplica la textura una vez y luego difumina el color de los píxeles de los bordes. |
| Border Color            | Usa un *color del borde* arbitrario para todas las coordenadas de textura situadas fuera del intervalo de 0.0 a 1.0, ambos inclusive.                         |

 

## <a name="span-idwraptextureaddressmodespanspan-idwraptextureaddressmodespanspan-idwraptextureaddressmodespanwrap-texture-address-mode"></a><span id="Wrap_texture_address_mode"></span><span id="wrap_texture_address_mode"></span><span id="WRAP_TEXTURE_ADDRESS_MODE"></span>Modo de direccionamiento de las texturas Wrap


El modo de direccionamiento de las texturas Wrap hace que Direct3D repita la textura en la unión de cada entero.

Supongamos, por ejemplo, que la aplicación crea un primitivo cuadrado y especifica unas coordenadas de textura de (0.0,0.0), (0.0,3.0), (3.0,3.0) y (3.0,0.0). Si el modo de direccionamiento de la textura se establece en "Wrap", la textura se aplica tres veces en las direcciones u y v, como se muestra en la siguiente ilustración.

![Ilustración de la textura de una cara ajustada en la dirección u y la dirección v](images/wrap.png)

Esto contrasta con el **modo de direccionamiento de las texturas Mirror**, que se muestra a continuación.

## <a name="span-idmirrortextureaddressmodespanspan-idmirrortextureaddressmodespanspan-idmirrortextureaddressmodespanmirror-texture-address-mode"></a><span id="Mirror_texture_address_mode"></span><span id="mirror_texture_address_mode"></span><span id="MIRROR_TEXTURE_ADDRESS_MODE"></span>Modo de direccionamiento de las texturas Mirror


El modo de direccionamiento de las textura Mirror hace que Direct3D refleje la textura en el límite de cada entero.

Supongamos, por ejemplo, que la aplicación crea un primitivo cuadrado y especifica unas coordenadas de textura de (0.0,0.0), (0.0,3.0), (3.0,3.0) y (3.0,0.0). Si el modo de direccionamiento de la textura se establece en "Mirror", la textura se aplica tres veces en las direcciones u y v. Una de cada dos filas y columnas a las que se aplica es una imagen reflejada de la fila o columna anteriores, como se muestra en la ilustración siguiente.

![Ilustración de imágenes reflejadas en una cuadrícula de 3 x 3](images/mirror.png)

Esto contrasta esto con el **modo de direccionamiento de las texturas Wrap**.

## <a name="span-idclamptextureaddressmodespanspan-idclamptextureaddressmodespanspan-idclamptextureaddressmodespanclamp-texture-address-mode"></a><span id="Clamp_texture_address_mode"></span><span id="clamp_texture_address_mode"></span><span id="CLAMP_TEXTURE_ADDRESS_MODE"></span>Modo de direccionamiento de las texturas Clamp


El modo de direccionamiento de las texturas Clamp hace que Direct3D comprima las coordenadas de textura en el intervalo \[0.0, 1.0\]; el modo Clamp aplica la textura una vez y luego difumina el color de los píxeles de los bordes.

Imaginemos que la aplicación crea un primitivo cuadrado y asigna unas coordenadas de textura de (0.0,0.0), (0.0,3.0), (3.0,3.0) y (3.0,0.0) a los vértices de dicho primitivo. Si el modo de direccionamiento de la textura se establece en "Clamp", la textura se aplica una sola vez. Los colores de los píxeles en la parte superior de las columnas y al final de las filas se extienden hasta la parte superior y la parte derecha del primitivo, respectivamente.

La siguiente ilustración muestra una textura comprimida.

![Ilustración de una textura y una textura comprimida](images/clamp.png)

## <a name="span-idbordercolortextureaddressmodespanspan-idbordercolortextureaddressmodespanspan-idbordercolortextureaddressmodespanborder-color-texture-address-mode"></a><span id="Border_Color_texture_address_mode"></span><span id="border_color_texture_address_mode"></span><span id="BORDER_COLOR_TEXTURE_ADDRESS_MODE"></span>Modo de direccionamiento de las texturas Border Color


El modo de dirección de textura Border Color hace que Direct3D use un color arbitrario, conocido como *color del borde*, para todas las coordenadas de textura situadas fuera del intervalo de 0.0 a 1.0, ambos inclusive.

En la siguiente ilustración, la aplicación especifica que la textura se aplique al primitivo con un borde rojo.

![Ilustración de una textura y una textura con un borde rojo.](images/border.png)

## <a name="span-iddevicelimitationsspanspan-iddevicelimitationsspanspan-iddevicelimitationsspandevice-limitations"></a><span id="Device_Limitations"></span><span id="device_limitations"></span><span id="DEVICE_LIMITATIONS"></span>Limitaciones del dispositivo


Aunque normalmente el sistema permite las coordenadas de textura fuera del intervalo de 0.0 y 1.0, ambos inclusive, a menudo las limitaciones de hardware afectan a lo fuera de ese intervalo que pueden estar las coordenadas de textura. Un dispositivo de representación comunica el límite del intervalo completo de coordenadas de textura permitido por el dispositivo cuando se recuperan las funcionalidades del dispositivo.

Por ejemplo, si este valor es de 128, las coordenadas de textura de entrada deben mantenerse en el intervalo de -128.0 a +128.0. Pasar vértices con coordenadas de textura situadas fuera de este intervalo no es válido. Esta misma restricción se aplica a las coordenadas de textura generadas como resultado de la generación automática de coordenadas de textura y de las transformaciones de coordenadas de textura.

Las limitaciones de repetición de textura pueden depender del tamaño de la textura indexado por las coordenadas de textura. En ese caso, suponiendo que la dimensión de la textura es de 32 y que el intervalo de las coordenadas de textura permitido por el dispositivo es de 512, el intervalo de las coordenadas de textura válido real sería 512/32 = 16, por lo que las coordenadas de textura para este dispositivo deben estar dentro del intervalo de -16.0 a +16.0.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Texturas](textures.md)

 

 




