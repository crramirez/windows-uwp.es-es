---
ms.assetid: 70667353-152B-4B18-92C1-0178298052D4
title: ESC/POS de Epson con formato
description: Aprende a usar el lenguaje de comandos ESC/POS para dar formato al texto, como aplicar negrita o caracteres de tamaño doble, para tu impresora de punto de servicio.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9fa9a0746e65fe3cbb42ce140a62a3129023d011
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89165459"
---
# <a name="epson-escpos-with-formatting"></a>ESC/POS de Epson con formato


**API importantes**

-   [**Impresora PointofService**](/uwp/api/Windows.Devices.PointOfService)
-   [**Windows. Devices. PointOfService**](/uwp/api/Windows.Devices.PointOfService)

Aprende a usar el lenguaje de comandos ESC/POS para dar formato al texto, como aplicar negrita o caracteres de tamaño doble, para tu impresora de punto de servicio.

## <a name="escpos-usage"></a>Uso de ESC/POS

El punto de servicio de Windows te permite usar diversos tipos de impresoras, incluidas varias impresoras de la serie Epson TM (para obtener una lista completa de las impresoras compatibles, consulta la página [Impresora PointofService](/uwp/api/Windows.Devices.PointOfService)). Windows admite la impresión mediante el lenguaje de control de impresoras ESC/POS, que proporciona comandos eficaces y funcionales para la comunicación con la impresora.

ESC/POS es un sistema de comandos creado por Epson usado a través de una amplia gama de sistemas de impresoras POS, destinado a evitar conjuntos de comandos incompatibles proporcionando aplicabilidad universal. Las impresoras más modernas admiten ESC/POS.

Todos los comandos empiezan con el carácter ESC (ASCII 27, HEX 1B) o GS (ASCII 29, HEX 1D), seguidos de otro carácter que especifica el comando. El texto normal simplemente se envía a la impresora, separado por saltos de línea.

La [**API PointOfService de Windows**](/uwp/api/Windows.Devices.PointOfService) proporciona gran parte de esa funcionalidad a través de los métodos **Print()** o **PrintLine()**. Sin embargo, para obtener determinado formato o enviar comandos específicos, debes usar comandos ESC/POS, que se integran como una cadena y se envían a la impresora.

## <a name="example-using-bold-and-double-size-characters"></a>Ejemplo de uso de caracteres de tamaño doble y negrita

El siguiente ejemplo muestra cómo usar los comandos ESC/POS para imprimir en negrita y caracteres de tamaño doble. Ten en cuenta que cada comando se integra como una cadena, y luego se inserta en las llamadas a printJob.

```csharp
// … prior plumbing code removed for brevity
// this code assumed you've already created a receipt print job (printJob)
// and also that you've already checked the PosPrinter Capabilities to
// verify that the printer supports Bold and DoubleHighDoubleWide print modes

const string ESC = "\u001B";
const string GS = "\u001D";
const string InitializePrinter = ESC + "@";
const string BoldOn = ESC + "E" + "\u0001";
const string BoldOff = ESC + "E" + "\0";
const string DoubleOn = GS + "!" + "\u0011";  // 2x sized text (double-high + double-wide)
const string DoubleOff = GS + "!" + "\0";

printJob.Print(InitializePrinter);
printJob.PrintLine("Here is some normal text.");
printJob.PrintLine(BoldOn + "Here is some bold text." + BoldOff);
printJob.PrintLine(DoubleOn + "Here is some large text." + DoubleOff);

printJob.ExecuteAsync();
```

Para obtener más información sobre ESC/POS, incluidos los comandos disponibles, echa un vistazo a [Epson ESC/POS FAQ (Preguntas más frecuentes sobre Epson ESC/POS)](https://content.epson.de/fileadmin/content/files/RSD/downloads/escpos.pdf). Para obtener detalles sobre [**Windows.Devices.PointOfService**](/uwp/api/Windows.Devices.PointOfService) y todas las funcionalidades disponibles, consulta el tema [Impresora PointofService](/uwp/api/Windows.Devices.PointOfService) en MSDN.