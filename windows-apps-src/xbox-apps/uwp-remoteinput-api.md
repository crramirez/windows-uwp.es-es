---
title: Referencia de API de entrada remota del Portal de dispositivos
description: Aprende a enviar de manera remota la entrada del mouse, del teclado y del controlador en una Xbox.
ms.localizationpriority: medium
ms.openlocfilehash: e0db86ad50bfb1cb27f516243542a554e710c3ea
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2018
ms.locfileid: "8730595"
---
# <a name="remote-input-api-reference"></a>Referencia de API de entrada remota   
Puedes enviar la entrada de mouse, teclado y controlador en tiempo real de manera remota mediante esta API.

**Solicitud**

Método      | URI de solicitud
:------     | :-----
Websocket | /ext/remoteinput
<br />
**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Solicitud**

El websocket de una serie de mensajes de matriz de bytes. Para cada mensaje el formato es el siguiente:

El primer byte indica el tipo de entrada. Se admiten los siguientes tipos de entrada:

| Tipo de entrada        | Valor de bytes |
|------------|-------------|
Códigos de teclas de teclado | 0x01
Códigos de digitalización de teclado | 0x02
Mouse | 0x03
Borrar todo | 0x04

Para KeyboardKeyCodes y KeyboardScanCodes, el segundo byte es el valor del código de tecla o código de análisis y el tercer byte es 0x01 para una presión hacia abajo y 0x00 para una liberación.

Para un mensaje de mouse, el siguiente valor es un UINT16 en orden de red (2 bytes) que indica el tipo de evento de mouse:

| Tipo de acción        | Valor UINT16 |
|------------|-------------|
Move | 0x0001
LeftDown | 0x0002
LeftUp | 0x0004
RightDown | 0x0008
RightUp | 0x0010
MiddleDown | 0x0020
MiddleUp | 0x0040
X1Down | 0x0080
X1Up | 0x0100
X2Down | 0x0200
X2Up | 0x0400
VerticalWheelMoved | 0x0800
HorizontalWheelMoved | 0x1000

A este byte le siguen dos valores UINT32 en orden de red y un UINT32 tercero opcional para acciones de la rueda. Los dos primeros valores son las coordenada X e Y, y el tercero es el diferencial de la rueda. Se espera que las coordenadas X e Y deben ser un valor comprendido entre 0 y 65535 que indica la posición relativa del mouse en los planos horizontales y verticales.

ClearAll indica que cualquier tecla presionada actualmente se debería liberar. No se espera ningún otro byte.

Para enviar la entrada del controlador para juegos, los valores de código de tecla que representan las pulsaciones de botones del controlador para juegos se pueden usar con KeyboardKeyCodes. Esos valores son los siguientes:

| Tipo de controlador para juegos        | Valor de bytes |
|------------|-------------|
VK_GAMEPAD_A                       |  0xC3
VK_GAMEPAD_B                       |  0xC4
VK_GAMEPAD_X                       |  0xC5
VK_GAMEPAD_Y                       |  0xC6
VK_GAMEPAD_RIGHT_SHOULDER          |  0xC7
VK_GAMEPAD_LEFT_SHOULDER           |  0xC8
VK_GAMEPAD_LEFT_TRIGGER            |  0xC9
VK_GAMEPAD_RIGHT_TRIGGER           |  0xCA
VK_GAMEPAD_DPAD_UP                 |  0xCB
VK_GAMEPAD_DPAD_DOWN               |  0xCC
VK_GAMEPAD_DPAD_LEFT               |  0xCD
VK_GAMEPAD_DPAD_RIGHT              |  0xCE
VK_GAMEPAD_MENU                    |  0xCF
VK_GAMEPAD_VIEW                    |  0xD0
VK_GAMEPAD_LEFT_THUMBSTICK_BUTTON  |  0xD1
VK_GAMEPAD_RIGHT_THUMBSTICK_BUTTON |  0xD2
VK_GAMEPAD_LEFT_THUMBSTICK_UP      |  0xD3
VK_GAMEPAD_LEFT_THUMBSTICK_DOWN    |  0xD4
VK_GAMEPAD_LEFT_THUMBSTICK_RIGHT   |  0xD5
VK_GAMEPAD_LEFT_THUMBSTICK_LEFT    |  0xD6
VK_GAMEPAD_RIGHT_THUMBSTICK_UP     |  0xD7
VK_GAMEPAD_RIGHT_THUMBSTICK_DOWN   |  0xD8
VK_GAMEPAD_RIGHT_THUMBSTICK_RIGHT  |  0xD9
VK_GAMEPAD_RIGHT_THUMBSTICK_LEFT   |  0xDA


**Respuesta**   

- Ninguno

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | La solicitud se realizó correctamente.
4XX | Códigos de error
5XX | Códigos de error

<br />
**Familias de dispositivos disponibles**

* Windows Xbox