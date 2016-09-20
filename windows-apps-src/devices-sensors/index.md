---
author: DBirtolo
ms.assetid: 0b891f63-02fa-4c30-b307-9fbcccac5caa
title: "Dispositivos, sensores y energía"
description: "Para proporcionar a los usuarios una experiencia enriquecida, puede que sea necesario integrar sensores o dispositivos externos en la aplicación."
ms.sourcegitcommit: d6757aba4be1a9e326e9e0dade16663286d24f72
ms.openlocfilehash: ae6ab11963ed06083e581ce42f39ce483d53e0d6

---
# Dispositivos, sensores y energía

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Para proporcionar a los usuarios una experiencia enriquecida, puede que sea necesario integrar sensores o dispositivos externos en la aplicación. Estos son algunos ejemplos de características que se pueden agregar a tu aplicación con la tecnología descrita en esta sección.

-   Ofrecer una experiencia de impresión mejorada
-   Integración de sensores de movimiento y orientación en el juego
-   Conectarse a un dispositivo directamente o a través de un protocolo de red

| Tema | Descripción |
|-------|-------------|
| [Habilitar funcionalidades de dispositivos](enable-device-capabilities.md) | Este tutorial describe cómo declarar funcionalidades del dispositivo en Microsoft Visual Studio. Esta opción permite que la aplicación use cámaras, micrófonos, sensores de ubicación y otros dispositivos. | 
| [Habilitar el acceso de modo de usuario para Windows IoT](enable-usermode-access.md) | En este tutorial se describe cómo habilitar el acceso de modo de usuario a GPIO, I2C, SPI y UART con Windows 10 IoT Core. |
| [Enumerar dispositivos](enumerate-devices.md) | El espacio de nombres de enumeración te permite buscar dispositivos que están conectados en el sistema de forma interna o externa o que se pueden detectar mediante protocolos de redes o de redes inalámbricas. |
| [Emparejar dispositivos](pair-devices.md) | Algunos dispositivos deben estar emparejados antes de que puedan usarse. El espacio de nombres [<strong>Windows.Devices.Enumeration</strong>](https://msdn.microsoft.com/library/windows/apps/BR225459) admite tres modos de emparejar dispositivos. |
| [Emparejamientos fuera de banda](out-of-band-pairing.md) | En esta sección se describe cómo el emparejamiento fuera de banda permite que las aplicaciones puedan conectarse a determinados dispositivos sin necesidad de detección. | 
| [Sensores](sensors.md) | Los sensores permiten que las aplicaciones conozcan cuál es la relación entre un dispositivo y el entorno físico. Pueden indicar a la aplicación la dirección, la orientación y el movimiento del dispositivo. |
| [Bluetooth](bluetooth.md) | Esta sección contiene artículos acerca de cómo integrar Bluetooth en aplicaciones de la Plataforma universal de Windows (UWP) y cómo usar anuncios de bajo consumo (LE), RFCOMM y GATT. | 
| [Impresión y digitalización](printing-and-scanning.md) | En esta sección se describe cómo imprimir y digitalizar desde la aplicación universal de Windows. | 
| [Impresión en3D](3d-printing.md) | En esta sección se describe cómo usar la funcionalidad de impresión 3D en tu aplicación Universal de Windows. |
| [Crear una aplicación de tarjeta NFC inteligente](host-card-emulation.md) | Windows Phone 8.1 admitía las aplicaciones de emulación de tarjeta NFC con un elemento seguro basado en SIM, pero ese modelo requería que las aplicaciones de pago seguro estuvieran estrechamente unidas a los operadores de redes móviles (MNO). Esto limitaba la variedad de soluciones de pago posibles por otros comerciantes o desarrolladores que no estaban unidos a MNO. En Windows 10 Mobile, se ha incluido una nueva tecnología de emulación de tarjetas denominada Emulación de tarjeta de host (HCE). La tecnología HCE permite a tu aplicación comunicarse directamente con un lector de tarjetas NFC. En este tema se muestra cómo funciona la emulación de tarjeta de host (HCE) en dispositivos Windows 10 Mobile y cómo se desarrolla una aplicación HCE para que los clientes puedan acceder a los servicios a través de su teléfono en lugar de con una tarjeta física sin colaboración con un MNO. |
| [Obtener información sobre la batería](get-battery-info.md) | Aprende a obtener información detallada sobre la batería mediante las API del espacio de nombres [<strong>Windows.Devices.Power</strong>](https://msdn.microsoft.com/library/windows/apps/Dn895017). |




<!--HONumber=Jun16_HO4-->


