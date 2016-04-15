---
title: Crear números aleatorios
description: Este código de ejemplo muestra cómo crear un número aleatorio o búfer para usarlo en criptografía en una aplicación para la Plataforma universal de Windows (UWP).
ms.assetid: 15746824-F93A-4DC7-836E-EBA916D2CFD3
author: awkoren
---

# Crear números aleatorios


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

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

<!--HONumber=Mar16_HO5-->


