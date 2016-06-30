---
title: Codificar y descodificar datos
description: "En este código de ejemplo se muestra cómo codificar y descodificar datos hexadecimales y base64 en una aplicación para la Plataforma universal de Windows (UWP)."
ms.assetid: 2CC23863-E840-48F4-B087-0479045743AC
author: awkoren
translationtype: Human Translation
ms.sourcegitcommit: b41fc8994412490e37053d454929d2f7cc73b6ac
ms.openlocfilehash: cd70a84e498c390684a59b33ec8a34375e1eb863

---

# Codificar y descodificar datos


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Este código de ejemplo muestra cómo codificar y descodificar datos hexadecimales y base64 en una aplicación para la Plataforma universal de Windows (UWP).

```cs
public void EncodeDecodeBase64()
{
    // Define a Base64 encoded string.
    String strBase64 = "uiwyeroiugfyqcajkds897945234==";

    // Decoded the string from Base64 to binary.
    IBuffer buffer = CryptographicBuffer.DecodeFromBase64String(strBase64);

    // Encode the buffer back into a Base64 string.
    String strBase64New = CryptographicBuffer.EncodeToBase64String(buffer);
}

public void EncodeDecodeHex()
{
    // Define a hexadecimal string.
    String strHex = "30310AFF";

    // Decode a hexadecimal string to binary.
    IBuffer buffer = CryptographicBuffer.DecodeFromHexString(strHex);

    // Encode the buffer back into a hexadecimal string.
    String strHexNew = CryptographicBuffer.EncodeToHexString(buffer);
}
```



<!--HONumber=Jun16_HO4-->


