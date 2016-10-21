---
author: DBirtolo
ms.assetid: 70667353-152B-4B18-92C1-0178298052D4
title: Epson ESC/POS con formato
description: "Aprende a usar el lenguaje de comandos ESC/POS para dar formato al texto, como aplicar negrita o caracteres de tamaño doble, para tu impresora de punto de servicio."
translationtype: Human Translation
ms.sourcegitcommit: ba620bc89265cbe8756947e1531759103c3cafef
ms.openlocfilehash: b645e41d7456f1dff664e3f61721a3564d554202

---
# Epson ESC/POS con formato

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

** API importantes **

-   [Impresora PointofService](https://msdn.microsoft.com/library/windows/apps/Mt426652)
-   [**Windows.Devices.PointOfService**](https://msdn.microsoft.com/library/windows/apps/Dn298071)

Aprende a usar el lenguaje de comandos ESC/POS para dar formato al texto, como aplicar negrita o caracteres de tamaño doble, para tu impresora de punto de servicio.

## Uso de ESC/POS

El punto de servicio de Windows te permite usar diversos tipos de impresoras, incluidas varias impresoras de la serie Epson TM (para obtener una lista completa de las impresoras compatibles, consulta la página [Impresora PointofService](https://msdn.microsoft.com/library/windows/apps/Mt426652)). Windows admite la impresión mediante el lenguaje de control de impresoras ESC/POS, que proporciona comandos eficaces y funcionales para la comunicación con la impresora.

ESC/POS es un sistema de comandos creado por Epson usado a través de una amplia gama de sistemas de impresoras POS, destinado a evitar conjuntos de comandos incompatibles proporcionando aplicabilidad universal. Las impresoras más modernas admiten ESC/POS.

Todos los comandos empiezan con el carácter ESC (ASCII 27, HEX 1B) o GS (ASCII 29, HEX 1D), seguidos de otro carácter que especifica el comando. El texto normal simplemente se envía a la impresora, separado por saltos de línea.

La [**API PointOfService de Windows**](https://msdn.microsoft.com/library/windows/apps/Dn298071) proporciona gran parte de esa funcionalidad a través de los métodos **Print()** o **PrintLine()**. Sin embargo, para obtener determinado formato o enviar comandos específicos, debes usar comandos ESC/POS, que se integran como una cadena y se envían a la impresora.

## Ejemplo de uso de caracteres de tamaño doble y negrita

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





<!--HONumber=Aug16_HO3-->


