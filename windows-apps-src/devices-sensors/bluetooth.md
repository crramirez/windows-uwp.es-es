---
ms.assetid: 404783BA-8859-4BFB-86E3-3DD2042E66F5
title: Bluetooth
description: En esta sección se incluyen artículos acerca de cómo integrar Bluetooth en aplicaciones para la Plataforma universal de Windows (UWP) y cómo usar anuncios de bajo consumo (LE), RFCOMM y GATT.
ms.date: 06/26/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: aad368adf500c5968bf6374a5855a7e0dab0a044
ms.sourcegitcommit: 015291bdf2e7d67076c1c85fc025f49c840ba475
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2020
ms.locfileid: "85469570"
---
# <a name="bluetooth"></a>Bluetooth
Esta sección contiene artículos sobre cómo integrar Bluetooth en aplicaciones Plataforma universal de Windows (UWP). Hay dos tecnologías Bluetooth diferentes que puede elegir implementar en la aplicación.

> [!Important]
> Debe declarar la funcionalidad "Bluetooth" en *Package. appxmanifest*.
>
> `<Capabilities> <DeviceCapability Name="bluetooth" /> </Capabilities>`

## <a name="classic-bluetooth-rfcomm"></a>Bluetooth clásico (RFCOMM)
Antes de Bluetooth LE, los dispositivos usaban normalmente este protocolo para comunicarse con Bluetooth. Este protocolo es sencillo y útil para la comunicación de dispositivo a dispositivo sin necesidad de ahorro energético. Para obtener más información sobre este protocolo, incluidos los ejemplos de código, vea el tema sobre [Bluetooth RFCOMM](send-or-receive-files-with-rfcomm.md) .

## <a name="bluetooth-low-energy-le"></a>Bluetooth de baja energía (LE)
Bluetooth de baja energía (LE) es una especificación que define los protocolos para la detección y la comunicación entre los dispositivos que tienen un requisito de uso de energía eficaz. Para obtener más información, incluidos los ejemplos de código, consulte el tema [Bluetooth de baja energía](bluetooth-low-energy-overview.md) .

## <a name="see-also"></a>Consulte también
- [Preguntas más frecuentes de los desarrolladores de Bluetooth](bluetooth-dev-faq.md)