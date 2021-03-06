---
title: Tipos de dispositivo
description: Los tipos de dispositivos de Direct3D incluyen dispositivos de la capa de abstracción de hardware (HAL) y el rasterizador de referencia.
ms.assetid: 64084B23-10C0-4541-8E93-FB323385D2F0
keywords:
- Tipos de dispositivo
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e51b7ce49d9acba80e3ed5cb9e75a02662b5dad8
ms.sourcegitcommit: e58e27a12026454270ad9db2cf5746bcede7075d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/11/2020
ms.locfileid: "77128094"
---
# <a name="device-types"></a>Tipos de dispositivo


Los tipos de dispositivos de Direct3D incluyen dispositivos de la capa de abstracción de hardware (HAL) y el rasterizador de referencia.

## <a name="span-idhal_devicespanspan-idhal_devicespanspan-idhal_devicespanhal-device"></a><span id="HAL_Device"></span><span id="hal_device"></span><span id="HAL_DEVICE"></span>Dispositivo HAL


El tipo de dispositivo principal es el dispositivo HAL, que admite la rasterización acelerada por hardware y el procesamiento de vértice de hardware y software. Si el equipo en el que se ejecuta la aplicación está equipado con un adaptador de pantalla compatible con Direct3D, la aplicación debería usarlo para las operaciones de Direct3D. Los dispositivos HAL de Direct3D implementan todos o parte de los módulos de transformación, iluminación y rasterización de hardware.

Las aplicaciones acceden directamente a adaptadores de gráficos. Llaman a métodos y funciones de Direct3D. Direct3D accede al hardware mediante HAL. Si el equipo en que se está ejecutando tu aplicación admite HAL, obtendrá el mejor rendimiento con un dispositivo HAL.

## <a name="span-idreference_devicespanspan-idreference_devicespanspan-idreference_devicespanreference-device"></a><span id="Reference_Device"></span><span id="reference_device"></span><span id="REFERENCE_DEVICE"></span>Dispositivo de referencia


Direct3D admite un tipo de dispositivo adicional denominado dispositivo de referencia o rasterizador de referencia. A diferencia de un dispositivo de software, el rasterizador de referencia es compatible con todas las características de Direct3D. Este dispositivo está pensado para usarse con fines de depuración y, por tanto, solo está disponible en máquinas donde se ha instalado el SDK de DirectX. Dado que estas características se pueden implementar para la precisión, en lugar de la velocidad, y se implementan en software, los resultados no son muy rápidos. El rasterizador de referencia hace uso de instrucciones de CPU especiales siempre que es posible, pero no está diseñado para aplicaciones comerciales. Usa el rasterizador de referencia solo para fines de pruebas o demostración de la característica.

## <a name="span-idhal_vs_refspanspan-idhal_vs_refspanspan-idhal_vs_refspanhal-vs-ref-devices"></a><span id="HAL_vs_REF"></span><span id="hal_vs_ref"></span><span id="HAL_VS_REF"></span>HAL frente a dispositivos REF


Los dispositivos HAL (capa de abstracción de hardware) y los dispositivos REF (rasterizador de REFerencia) son los dos tipos principales de dispositivos de Direct3D. El primero se basa en la compatibilidad de hardware, es muy rápido y es posible que no sea compatible con todo; mientras que el segundo no usa la aceleración de hardware, por lo tanto, es muy lento, pero garantiza la compatibilidad con el conjunto completo de características de Direct3D, de forma correcta. En general, solo tendrás que usar dispositivos HAL, pero si estás usando algunas características avanzadas que no admite tu tarjeta gráfica, es posible que debas volver a REF.

Otro momento en que es posible que quieras usar REF es si el dispositivo HAL produce resultados extraños, es decir, si estás seguro de que tu código es correcto, pero el resultado recibido no es el que esperabas. El dispositivo REF garantiza un comportamiento correcto, por lo que puede que quieras probar tu aplicación en el dispositivo REF y ver si el comportamiento extraño continúa. Si no es así, significa que (a) la aplicación supone que la tarjeta gráfica admite algo que no puede funcionar, o (b) se trata de un error del controlador. Si tampoco funciona con el dispositivo REF, quiere decir que se trata de un error de la aplicación.

## <a name="span-idhardware_vs_softwarespanspan-idhardware_vs_softwarespanspan-idhardware_vs_softwarespanhardware-vs-software-vertex-processing"></a><span id="Hardware_vs_Software"></span><span id="hardware_vs_software"></span><span id="HARDWARE_VS_SOFTWARE"></span>Procesamiento de vértices de software y hardware


El procesamiento de vértices de hardware frente a software solo se aplica realmente a dispositivos HAL. Cuando insertas vértices a través de la canalización, estos deben transformarse (mediante las matrices de mundo, vista y proyección correspondientes) e iluminarse (mediante las luces integradas D3D): esta fase de procesamiento se conoce como T&L (para la transformación e iluminación). El procesamiento de vértices de hardware significa que esto se hace en hardware, si el hardware lo admite; Ergo, el procesamiento de vértices de software se realiza en el software. El procedimiento general consiste en crear un dispositivo de T&L de hardware, si produce errores, probar el modo mixto, y, si ese produce errores, probar el software. (Si se produce un error en el software, renuncia y sal con un error).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Dispositivos](devices.md)

 

 




