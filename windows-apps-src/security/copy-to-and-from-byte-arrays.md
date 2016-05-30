---
title: Copiar a la matriz de bytes o desde ella
description: Este código de ejemplo muestra cómo copiar a matrices de bytes y desde ellas en una aplicación para la Plataforma universal de Windows (UWP).
ms.assetid: C343B08C-1FA1-40FD-8CA5-7FC9B707C5E3
author: awkoren
---

# Copiar a la matriz de bytes o desde ella


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Este código de ejemplo muestra cómo copiar a matrices de bytes y desde ellas en una aplicación para la Plataforma universal de Windows (UWP).

```cs
public void ByteArrayCopy()
{
    // Initialize a byte array.
    byte[] bytes = { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };

    // Create a buffer from the byte array.
    IBuffer buffer = CryptographicBuffer.CreateFromByteArray(bytes);

    // Encode the buffer into a hexadecimal string (for display);
    string hex = CryptographicBuffer.EncodeToHexString(buffer);

    // Copy the buffer back into a new byte array.
    byte[] newByteArray;
    CryptographicBuffer.CopyToByteArray(buffer, out newByteArray);
}
```

<!--HONumber=May16_HO2-->


