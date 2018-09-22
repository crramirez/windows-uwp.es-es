---
title: Crear números aleatorios
description: Este código de ejemplo muestra cómo crear un número aleatorio o búfer para usarlo en criptografía en una aplicación para la Plataforma universal de Windows (UWP).
ms.assetid: 15746824-F93A-4DC7-836E-EBA916D2CFD3
author: msatranjr
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, seguridad
ms.localizationpriority: medium
ms.openlocfilehash: 595b4ab47e3c6c833a4b8f2e692a0cc0c8ffcaa4
ms.sourcegitcommit: a160b91a554f8352de963d9fa37f7df89f8a0e23
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/21/2018
ms.locfileid: "4128656"
---
# <a name="create-random-numbers"></a>Crear números aleatorios



Este código de ejemplo muestra cómo crear un número aleatorio o búfer para usarlo en criptografía en una aplicación para la Plataforma universal de Windows (UWP).

```cs
public string GenerateRandomData()
{
    // Define the length, in bytes, of the buffer.
    uint length = 32;

    // Generate random data and copy it to a buffer.
    IBuffer buffer = CryptographicBuffer.GenerateRandom(length);

    // Encode the buffer to a hexadecimal string (for display).
    string randomHex = CryptographicBuffer.EncodeToHexString(buffer);

    return randomHex;
}

public uint GenerateRandomNumber()
{
    // Generate a random number.
    uint random = CryptographicBuffer.GenerateRandomNumber();
    return random;
}
```