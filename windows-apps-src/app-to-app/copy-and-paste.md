---
description: "Este artículo explica cómo admitir las funciones copiar y pegar en aplicaciones para la Plataforma universal de Windows (UWP) usando el portapapeles."
title: Copiar y pegar
ms.assetid: E882DC15-E12D-4420-B49D-F495BB484BEE
author: msatranjr
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: f49a417e87199a625a023f7aa867f855cbd5d3c9
ms.sourcegitcommit: 23cda44f10059bcaef38ae73fd1d7c8b8330c95e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/19/2017
---
#<a name="copy-and-paste"></a>Copiar y pegar

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Este artículo explica cómo admitir las funciones copiar y pegar en aplicaciones para la Plataforma universal de Windows (UWP) usando el portapapeles. Copiar y pegar es la forma clásica de intercambiar datos entre aplicaciones o dentro de una aplicación, y casi todas las aplicaciones admiten operaciones del portapapeles hasta cierto punto.

## <a name="check-for-built-in-clipboard-support"></a>Comprobar la compatibilidad integrada del portapapeles

En muchos casos, es posible que no necesites escribir código para admitir las operaciones del Portapapeles. Muchos de los controles XAML predeterminados que puedes usar para crear aplicaciones ya admiten las operaciones del portapapeles. 

## <a name="get-set-up"></a>Prepárate

En primer lugar, incluye el espacio de nombres [**Windows.ApplicationModel.DataTransfer**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer) en la aplicación. A continuación, agrega una instancia del objeto [**DataPackage**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackage). Este objeto contiene los datos que el usuario quiere copiar y también las propiedades (como la descripción) que quieres incluir.

<!-- For some reason, the snippets in this file are all inline in the WDCML topic. Suggest moving to VS project with rest of snippets. -->
```cs
DataPackage dataPackage = new DataPackage();
```

<!-- AuthenticateAsync-->

## <a name="copy-and-cut"></a>Copiar y cortar

Copiar y cortar (también denominado *mover*) funcionan prácticamente de la misma manera. Elige qué operación quieres realizar con el uso de la propiedad [**RequestedOperation**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackage.RequestedOperation).

```cs
// copy 
dataPackage.RequestedOperation = DataPackageOperation.Copy;
// or cut
dataPackage.RequestedOperation = DataPackageOperation.Move;
```
## <a name="drag-and-drop"></a>Arrastrar y colocar

A continuación, puedes agregar los datos que un usuario ha seleccionado para el objeto [**DataPackage**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackage). Si la clase **DataPackage** admite estos datos, puedes usar uno de los métodos correspondientes en el objeto **DataPackage**. Aquí vemos cómo agregar texto:

```cs
dataPackage.SetText("Hello World!");
```

El último paso es agregar [**DataPackage**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackage) al portapapeles llamando al método estático [**SetContent**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.Clipboard.SetContent(Windows.ApplicationModel.DataTransfer.DataPackage)).

```cs
Clipboard.SetContent(dataPackage);
```
## <a name="paste"></a>Pegar

Para obtener el contenido del portapapeles, llama al método estático [**GetContent**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.Clipboard.GetContent). Este método devuelve un objeto [**DataPackageView**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackageView) que incluye el contenido. Este objeto es casi idéntico a un objeto [**DataPackage**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackage), con la excepción de que su contenido es de solo lectura. Con ese objeto, puedes usar los métodos [**AvailableFormats**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackageView.AvailableFormats) o [**Contains**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackageView.Contains(System.String)) para identificar los formatos disponibles. Después, puedes llamar al método [**DataPackageView**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.DataPackageView) correspondiente para obtener los datos.

```cs
async void OutputClipboardText()
{
    DataPackageView dataPackageView = Clipboard.GetContent();
    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        string text = await dataPackageView.GetTextAsync();
        // To output the text from this example, you need a TextBlock control
        TextOutput.Text = "Clipboard now contains: " + text;
    }
}
```

## <a name="track-changes-to-the-clipboard"></a>Seguimiento de cambios en el portapapeles

Además de los comandos copiar y pegar, también puedes realizar un seguimiento de cambios en el portapapeles. Para hacerlo, debes controlar el evento [**ContentChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.DataTransfer.Clipboard.ContentChanged) del portapapeles.

```cs
Clipboard.ContentChanged += async (s, e) => 
{
    DataPackageView dataPackageView = Clipboard.GetContent();
    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        string text = await dataPackageView.GetTextAsync();
        // To output the text from this example, you need a TextBlock control
        TextOutput.Text = "Clipboard now contains: " + text;
    }
}
```

## <a name="see-also"></a>Consulta también

* [Comunicación entre aplicaciones](index.md)
* [DataTransfer](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.aspx)
* [DataPackage](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackage.aspx)
* [DataPackageView](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackageview.aspx)
* [DataPackagePropertySet]( https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackagepropertyset.aspx)
* [DataRequest](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datarequest.aspx) 
* [DataRequested]( https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datatransfermanager.datarequested.aspx)
* [FailWithDisplayText](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datarequest.failwithdisplaytext.aspx)
* [ShowShareUi](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datatransfermanager.showshareui.aspx)
* [RequestedOperation](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackage.requestedoperation.aspx) 
* [ControlsList](https://msdn.microsoft.com/library/windows/apps/xaml/mt185406.aspx)
* [SetContent](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.datatransfer.clipboard.setcontent.aspx)
* [GetContent](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.datatransfer.clipboard.getcontent.aspx)
* [AvailableFormats](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackageview.availableformats.aspx)
* [Contains](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.datatransfer.datapackageview.contains.aspx)
* [ContentChanged](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.datatransfer.clipboard.contentchanged.aspx)

