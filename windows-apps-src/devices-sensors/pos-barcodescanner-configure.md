---
title: Configurar un escáner de códigos de barras
description: Obtenga información sobre cómo configurar un analizador de código de barras para la aplicación deseada.
ms.date: 08/29/2018
ms.topic: article
keywords: windows 10, uwp, punto de servicio, pos
ms.localizationpriority: medium
ms.openlocfilehash: 792fb50858620ff5dd269a0d7a3b8e491b9f247b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57591070"
---
# <a name="configure-a-barcode-scanner"></a>Configurar un escáner de códigos de barras

Los escáneres de código de barras se pueden configurar en varios modos diferentes.  Es importante configurar correctamente el escáner de códigos de barra para la aplicación deseada.

Muchos de los escáneres de códigos de barra se pueden configurar en modo **cuñas de teclado**, lo hace que el escáner aparezca como un teclado de Windows.  Esto te permite digitalizar códigos de barra en aplicaciones que no están preparadas para ello, como el Bloc de notas.  Al digitalizar un código de barras en este modo, los datos descodificados desde el escáner se introducen en el punto de inserción como si hubieras escrito los datos mediante el teclado.  Si quieres tener más control sobre el escáner desde la aplicación para UWP, tendrás que configurarlo en un modo diferente al de cuña de teclado.

## <a name="usb-barcode-scanner"></a>Escáner de código de barras USB
Un escáner de código de barras conectado por USB debe configurarse en modo **HID POS Scanner** para que funcione con el controlador del escáner de código de barras que se incluye en Windows. Este controlador es una implementación de la **HID de punto de venta uso tablas** especificación publicado en [USB HID](https://www.usb.org/developers/hidpage/).  Consulta la documentación del escáner o ponte en contacto con el fabricante para obtener instrucciones sobre cómo habilitar el modo **HID POS Scanner**.  Una vez configurado como **HID POS Scanner**, el escáner de código de barras aparecerá en el Administrador de dispositivos, bajo el nodo **POS Barcode Scanner** como **escáner de código de barras POS HID**.

El fabricante del escáner de código de barras también puede tener un controlador específico del proveedor que sea compatible con las API de escáner de código de barras de UWP que usen un modo distinto al de **HID POS Scanner**.  Si ya ha instalado un controlador proporcionado por el fabricante compatible con las API de escáner de código de barras de UWP, puede ver un dispositivo específico del proveedor aparece en **POS escáner** en Administrador de dispositivos.

## <a name="bluetooth-barcode-scanner"></a>Escáner de código de barras con Bluetooth
Un escáner de código de barras conectado por Bluetooth debe configurarse en modo **protocolo de puerto serie - interfaz serie sencilla (SPP-SSI)** para funcionar con las API de escáner de código de barras de UWP.  Consulta la documentación del escáner o ponte en contacto con el fabricante para obtener instrucciones sobre cómo habilitar el modo **modo SPP-SSI**.

Para poder usar el escáner de código de barras Bluetooth debe emparejarlo con **configuración > dispositivos > Bluetooth & otros dispositivos > agregar Bluetooth u otro dispositivo**.

Puede iniciar y controlar el proceso de emparejamiento mediante la [Windows.Devices.Enumeration](https://docs.microsoft.com/uwp/api/windows.devices.enumeration) espacio de nombres.  Consulte [par dispositivos](https://docs.microsoft.com/windows/uwp/devices-sensors/pair-devices) para obtener más información.

[!INCLUDE [feedback](./includes/pos-feedback.md)]