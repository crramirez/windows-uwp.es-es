---
ms.assetid: 0b891f63-02fa-4c30-b307-9fbcccac5caa
title: Dispositivos, sensores y energía
description: Para proporcionar a los usuarios una experiencia enriquecida, puede que sea necesario integrar sensores o dispositivos externos en la aplicación.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 75a734c5cbf95bb7ddfad9199c5d1a983c10650e
ms.sourcegitcommit: f727b68e86a86c94eff00f67ed79a1c12666e7bc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "66369992"
---
# <a name="devices-sensors-and-power"></a>Dispositivos, sensores y energía


Para proporcionar a los usuarios una experiencia enriquecida, puede que sea necesario integrar sensores o dispositivos externos en la aplicación. Estos son algunos ejemplos de características que se pueden agregar a tu aplicación con la tecnología descrita en esta sección.

-   Ofrecer una experiencia de impresión mejorada
-   Integración de sensores de movimiento y orientación en el juego
-   Conectarse a un dispositivo directamente o a través de un protocolo de red

| Tema | Descripción |
|-------|-------------|
| [Habilitar funcionalidades de dispositivos](enable-device-capabilities.md) | Este tutorial describe cómo declarar funcionalidades del dispositivo en Microsoft Visual Studio. Esta opción permite que la aplicación use cámaras, micrófonos, sensores de ubicación y otros dispositivos. | 
| [Habilitar el acceso de modo de usuario para Windows IoT](enable-usermode-access.md) | En este tutorial se describe cómo habilitar el acceso de modo de usuario a GPIO, I2C, SPI y UART con Windows 10 IoT Core. |
| [Enumerar dispositivos](enumerate-devices.md) | El espacio de nombres de enumeración te permite buscar dispositivos conectados internamente con el sistema, conectados externamente o detectables mediante protocolos de redes o de redes inalámbricas. |
| [Emparejar dispositivos](pair-devices.md) | Algunos dispositivos deben estar emparejados para que puedan usarse. El espacio de nombres [<strong>Windows.Devices.Enumeration</strong>](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration) admite tres modos de emparejar dispositivos. |
| [Punto de servicio](point-of-service.md) | En esta sección se describe cómo interactuar con los periféricos de punto de servicio, como escáneres de códigos de barras, impresoras de recibos, cajas registradoras, etc. | 
| [Sensores](sensors.md) | Los sensores permiten que las aplicaciones conozcan cuál es la relación entre un dispositivo y el entorno físico. Pueden indicar a la aplicación la dirección, la orientación y el movimiento del dispositivo. |
| [Bluetooth](bluetooth.md) | En esta sección se incluyen artículos acerca de cómo integrar Bluetooth en aplicaciones para la Plataforma universal de Windows (UWP) y cómo usar anuncios de bajo consumo (LE), RFCOMM y GATT. | 
| [Impresión y digitalización](printing-and-scanning.md) | En esta sección se describe cómo imprimir y digitalizar desde la aplicación universal de Windows. | 
| [Impresión 3D](3d-printing.md) | En esta sección se describe cómo usar la funcionalidad de impresión en 3D en la aplicación universal de Windows. |
| [Crear una aplicación de tarjeta NFC inteligente](host-card-emulation.md) | Windows Phone 8.1 admitía las aplicaciones de emulación de tarjeta NFC mediante el uso de un elemento seguro basado en SIM, pero ese modelo requería que las aplicaciones de pago seguro estuvieran estrechamente unidas a los operadores de redes móviles (MNO). Esto limitaba la variedad de soluciones de pago posibles por otros comerciantes o desarrolladores que no estaban unidos a MNO. En Windows 10 Mobile, se ha incluido una nueva tecnología de emulación de tarjetas denominada Emulación de tarjeta de host (HCE). La tecnología HCE permite a tu aplicación comunicarse directamente con un lector de tarjetas NFC. En este tema se muestra cómo funciona la Emulación de tarjeta de host (HCE) en dispositivos Windows 10 Mobile y cómo se desarrolla una aplicación HCE para que los clientes puedan acceder a los servicios mediante su teléfono en lugar de con una tarjeta física sin colaboración con un MNO. |
| [Obtener información sobre la batería](get-battery-info.md) | Aprende a obtener información detallada sobre la batería mediante las API del espacio de nombres [<strong>Windows.Devices.Power</strong>](https://docs.microsoft.com/uwp/api/Windows.Devices.Power). |

