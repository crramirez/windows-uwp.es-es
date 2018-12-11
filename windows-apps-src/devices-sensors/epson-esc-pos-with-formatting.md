---
ms.assetid: 70667353-152B-4B18-92C1-0178298052D4
title: Epson ESC/POS con formato
description: Aprende a usar el lenguaje de comandos ESC/POS para dar formato al texto (como, por ejemplo, aplicar negrita o caracteres de tamaño doble) de la impresora de punto de servicio.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 11467a45021da7898c2b617e3b1b01312c795c4c
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2018
ms.locfileid: "8872964"
---
# <a name="epson-escpos-with-formatting"></a>ESC/POS de Epson con formato


**API importantes**

-   [**Impresora PointofService**](https://msdn.microsoft.com/library/windows/apps/Mt426652)
-   [**Windows.Devices.PointOfService**](https://msdn.microsoft.com/library/windows/apps/Dn298071)

Aprende a usar el lenguaje de comandos ESC/POS para dar formato al texto, como aplicar negrita o caracteres de tamaño doble, para tu impresora de punto de servicio.

## <a name="escpos-usage"></a>Uso de ESC/POS

El punto de servicio de Windows te permite usar diversos tipos de impresoras, incluidas varias impresoras de la serie Epson TM (para obtener una lista completa de las impresoras compatibles, consulta la página [Impresora PointofService](https://msdn.microsoft.com/library/windows/apps/Mt426652)). Windows admite la impresión mediante el lenguaje de control de impresoras ESC/POS, que proporciona comandos eficaces y funcionales para la comunicación con la impresora.

ESC/POS es un sistema de comandos creado por Epson usado a través de una amplia gama de sistemas de impresoras POS, destinado a evitar conjuntos de comandos incompatibles proporcionando aplicabilidad universal. Las impresoras más modernas admiten ESC/POS.

Todos los comandos empiezan con el carácter ESC (ASCII 27, HEX 1B) o GS (ASCII 29, HEX 1D), seguidos de otro carácter que especifica el comando. El texto normal simplemente se envía a la impresora, separado por saltos de línea.

La [**API PointOfService de Windows**](https://msdn.microsoft.com/library/windows/apps/Dn298071) proporciona gran parte de esa funcionalidad a través de los métodos **Print()** o **PrintLine()**. Sin embargo, para obtener determinado formato o enviar comandos específicos, debes usar comandos ESC/POS, que se integran como una cadena y se envían a la impresora.

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

Para obtener más información sobre ESC/POS, incluidos los comandos disponibles, echa un vistazo a [Epson ESC/POS FAQ (Preguntas más frecuentes sobre Epson ESC/POS)](http://content.epson.de/fileadmin/content/files/RSD/downloads/escpos.pdf). Para obtener detalles sobre [**Windows.Devices.PointOfService**](https://msdn.microsoft.com/library/windows/apps/Dn298071) y todas las funcionalidades disponibles, consulta el tema [Impresora PointofService](https://msdn.microsoft.com/library/windows/apps/Mt426652) en MSDN.
