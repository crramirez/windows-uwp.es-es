---
title: Configurar un escáner de códigos de barras
description: Obtenga información sobre cómo configurar un escáner de código de barras para la aplicación deseada.
ms.date: 08/29/2018
ms.topic: article
keywords: Windows 10, UWP, punto de servicio, pos
ms.localizationpriority: medium
ms.openlocfilehash: 96deeb82f0aa04929ac33001b1aee0603e6474c7
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168509"
---
# <a name="configure-a-barcode-scanner"></a>Configurar un escáner de códigos de barras

Los escáneres de códigos de barras se pueden configurar en varios modos diferentes.  Es importante que el escáner de códigos de barras esté configurado correctamente para la aplicación deseada.

Muchos escáneres de códigos de barras se pueden configurar en el modo de forma de **teclado** , lo que hace que el escáner de códigos de barras aparezca como un teclado en Windows.  Esto le permite examinar códigos de barras en aplicaciones que no reconocen el escáner de códigos de barras como el Bloc de notas.  Al digitalizar un código de barras en este modo, los datos descodificados del escáner de códigos de barras se insertan en el punto de inserción como si hubiera escrito los datos con el teclado.  Si desea tener más control sobre el escáner de códigos de barras de la aplicación para UWP, tendrá que configurarlo en un modo de cuña que no sea un teclado.

## <a name="usb-barcode-scanner"></a>Escáner de código de barras USB
Se debe configurar un escáner de código de barras conectado USB en el modo de **escáner de pos HID** para que funcione con el controlador de escáner de código de barras incluido en Windows. Este controlador es una implementación de la especificación de **tablas de uso de punto de venta HID** Publicada en [USB-HID](https://www.usb.org/hid).  Consulte la documentación del escáner de códigos de barras o póngase en contacto con el fabricante del escáner de códigos de barras para obtener instrucciones para habilitar el modo de **escáner de PDV HID** .  Una vez configurado como un **escáner de PDV HID** , el escáner de códigos de barras aparecerá en Device Manager en el nodo **escáner de códigos de barras** de PDV como escáner de códigos de **barras HID de pos**.

El fabricante del escáner de códigos de barras también puede tener un controlador específico del proveedor que admita las API de escáner de código de barras UWP con un modo que no sea el **escáner de PDV de HID**.  Si ya ha instalado un controlador proporcionado por el fabricante compatible con las API de escáner de códigos de barras UWP, es posible que vea un dispositivo específico del proveedor en **escáner de códigos de barras de PDV** en Device Manager.

## <a name="bluetooth-barcode-scanner"></a>Escáner de código de barras Bluetooth
Para que funcione con las API del escáner de código de barras de UWP, debe configurarse un escáner conectado con Bluetooth en el modo de **interfaz de puerto serie (spp-SSI)** .  Consulte la documentación del escáner de códigos de barras o póngase en contacto con el fabricante del escáner de códigos de barras para obtener instrucciones para habilitar el **modo spp-SSI**.

Antes de poder usar el escáner de códigos de barras Bluetooth, debe emparejarlo mediante la **configuración > dispositivos > bluetooth & otros dispositivos > agregar Bluetooth u otro dispositivo**.

Puede iniciar y controlar el proceso de emparejamiento mediante el espacio de nombres [Windows. Devices. Enumeration](/uwp/api/windows.devices.enumeration) .  Consulte [Pair Devices](./pair-devices.md) para obtener más información.

[!INCLUDE [feedback](./includes/pos-feedback.md)]