---
title: Configurar un escáner de códigos de barras
description: Obtén información sobre cómo configurar un escáner de códigos de barras para la aplicación deseada.
ms.date: 08/29/2018
ms.topic: article
keywords: windows 10, uwp, punto de servicio, pos
ms.localizationpriority: medium
ms.openlocfilehash: 8f88b376dca80043f88260700bb7ef4168b3a445
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2018
ms.locfileid: "8758225"
---
# <a name="configure-a-barcode-scanner"></a>Configurar un escáner de códigos de barras

Los escáneres de código de barras se pueden configurar en varios modos diferentes.  Es importante configurar correctamente el escáner de códigos de barra para la aplicación deseada.

Muchos de los escáneres de códigos de barra se pueden configurar en modo **cuñas de teclado**, lo hace que el escáner aparezca como un teclado de Windows.  Esto te permite digitalizar códigos de barra en aplicaciones que no están preparadas para ello, como el Bloc de notas.  Al digitalizar un código de barras en este modo, los datos descodificados desde el escáner se introducen en el punto de inserción como si hubieras escrito los datos mediante el teclado.  Si quieres tener más control sobre el escáner desde la aplicación para UWP, tendrás que configurarlo en un modo diferente al de cuña de teclado.

## <a name="usb-barcode-scanner"></a>Escáner de código de barras USB
Un escáner de código de barras conectado por USB debe configurarse en modo **HID POS Scanner** para que funcione con el controlador del escáner de código de barras que se incluye en Windows. Este controlador es una implementación de la especificación de **Punto de venta tablas de uso HID** publicada en [USB HID](http://www.usb.org/developers/hidpage/).  Consulta la documentación del escáner o ponte en contacto con el fabricante para obtener instrucciones sobre cómo habilitar el modo **HID POS Scanner**.  Una vez configurado como **HID POS Scanner**, el escáner de código de barras aparecerá en el Administrador de dispositivos, bajo el nodo **POS Barcode Scanner** como **escáner de código de barras POS HID**.

El fabricante del escáner de código de barras también puede tener un controlador específico del proveedor que sea compatible con las API de escáner de código de barras de UWP que usen un modo distinto al de **HID POS Scanner**.  Si ya tiene instalado a un controlador proporcionado por el fabricante compatible con la API de escáner de códigos de barras de UWP, es posible que veas un dispositivo específico del proveedor que aparece en **POS Barcode Scanner** en el Administrador de dispositivos.

## <a name="bluetooth-barcode-scanner"></a>Escáner de código de barras con Bluetooth
Un escáner de código de barras conectado por Bluetooth debe configurarse en modo **protocolo de puerto serie - interfaz serie sencilla (SPP-SSI)** para funcionar con las API de escáner de código de barras de UWP.  Consulta la documentación del escáner o ponte en contacto con el fabricante para obtener instrucciones sobre cómo habilitar el modo **modo SPP-SSI**.

Puedes usar el escáner de códigos de barras con Bluetooth debes emparejarlo mediante **Configuración > dispositivos > Bluetooth & otros dispositivos > agregar Bluetooth u otro dispositivo**.

Puedes iniciar y controlar el proceso de emparejamiento con el espacio de nombres [Windows.Devices.Enumeration](https://docs.microsoft.com/uwp/api/windows.devices.enumeration) .  Para obtener más información, consulta [Emparejar dispositivos](https://docs.microsoft.com/windows/uwp/devices-sensors/pair-devices).

[!INCLUDE [feedback](./includes/pos-feedback.md)]