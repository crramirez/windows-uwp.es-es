---
title: Copiar a la matriz de bytes o desde ella
description: Este código de ejemplo muestra cómo copiar a matrices de bytes y desde ellas en una aplicación para la Plataforma universal de Windows (UWP).
ms.assetid: C343B08C-1FA1-40FD-8CA5-7FC9B707C5E3
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, seguridad
ms.localizationpriority: medium
ms.openlocfilehash: b3ce63ca1780f9ed1ecd32f3ab1c029a1a92e1b5
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2018
ms.locfileid: "8214581"
---
# <a name="copy-to-and-from-byte-arrays"></a>Copiar a la matriz de bytes o desde ella



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