---
description: Este artículo explica cómo admitir las funciones copiar y pegar en aplicaciones para la Plataforma universal de Windows (UWP) usando el portapapeles.
title: Copiar y pegar
ms.assetid: E882DC15-E12D-4420-B49D-F495BB484BEE
ms.date: 12/19/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 117c09eb2dd0f24a330060da9b9766cb33e90d58
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175769"
---
# <a name="copy-and-paste"></a>Copiar y pegar

Este artículo explica cómo admitir las funciones copiar y pegar en aplicaciones para la Plataforma universal de Windows (UWP) usando el portapapeles. Copiar y pegar es la forma clásica de intercambiar datos entre aplicaciones o dentro de una aplicación y casi todas las aplicaciones admiten operaciones del portapapeles hasta cierto punto. Para obtener ejemplos de código completos que muestran distintos escenarios de copia y pegado, vea el [ejemplo del portapapeles](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/Clipboard).

## <a name="check-for-built-in-clipboard-support"></a>Comprobar la compatibilidad integrada del portapapeles

En muchos casos, es posible que no necesites escribir código para admitir las operaciones del Portapapeles. Muchos de los controles XAML predeterminados que puedes usar para crear aplicaciones ya admiten las operaciones del portapapeles. 

## <a name="get-set-up"></a>Prepárate

En primer lugar, incluye el espacio de nombres [**Windows.ApplicationModel.DataTransfer**](/uwp/api/Windows.ApplicationModel.DataTransfer) en la aplicación. A continuación, agrega una instancia del objeto [**DataPackage**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage). Este objeto contiene los datos que el usuario quiere copiar y también las propiedades (como la descripción) que quieres incluir.

```cs
DataPackage dataPackage = new DataPackage();
```

<!-- AuthenticateAsync-->

## <a name="copy-and-cut"></a>Copiar y cortar

Copiar y cortar (también denominado *mover*) funcionan prácticamente de la misma manera. Elige qué operación quieres realizar con el uso de la propiedad [**RequestedOperation**](/uwp/api/windows.applicationmodel.datatransfer.datapackage.requestedoperation).

```cs
// copy 
dataPackage.RequestedOperation = DataPackageOperation.Copy;
// or cut
dataPackage.RequestedOperation = DataPackageOperation.Move;
```

## <a name="set-the-copied-content"></a>Establecer el contenido copiado

A continuación, puedes agregar los datos que un usuario ha seleccionado para el objeto [**DataPackage**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage). Si esta información es compatible con la clase de **paquete** de datos, puede usar uno de los [métodos](/uwp/api/windows.applicationmodel.datatransfer.datapackage#methods) correspondientes del objeto de **paquete** de datos. Aquí se muestra cómo agregar texto mediante el método [**setText**](/uwp/api/windows.applicationmodel.datatransfer.datapackage.settext) :

```cs
dataPackage.SetText("Hello World!");
```

El último paso es agregar [**DataPackage**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) al portapapeles llamando al método estático [**SetContent**](/uwp/api/windows.applicationmodel.datatransfer.clipboard.setcontent).

```cs
Clipboard.SetContent(dataPackage);
```

## <a name="paste"></a>Pegar

Para obtener el contenido del portapapeles, llama al método estático [**GetContent**](/uwp/api/windows.applicationmodel.datatransfer.clipboard.getcontent). Este método devuelve un objeto [**DataPackageView**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackageView) que incluye el contenido. Este objeto es casi idéntico a un objeto [**DataPackage**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage), con la excepción de que su contenido es de solo lectura. Con ese objeto, puedes usar los métodos [**AvailableFormats**](/uwp/api/windows.applicationmodel.datatransfer.datapackageview.availableformats) o [**Contains**](/uwp/api/windows.applicationmodel.datatransfer.datapackageview.contains) para identificar los formatos disponibles. Después, puedes llamar al método [**DataPackageView**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackageView) correspondiente para obtener los datos.

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

Además de los comandos copiar y pegar, también puedes realizar un seguimiento de cambios en el portapapeles. Para hacerlo, debes controlar el evento [**ContentChanged**](/uwp/api/windows.applicationmodel.datatransfer.clipboard.contentchanged) del portapapeles.

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

## <a name="see-also"></a>Vea también

* [Ejemplo Clipboard](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/Clipboard)
* [Comunicación entre aplicaciones](index.md)
* [DataTransfer](/uwp/api/windows.applicationmodel.datatransfer)
* [DataPackage](/uwp/api/windows.applicationmodel.datatransfer.datapackage)
* [DataPackageView](/uwp/api/windows.applicationmodel.datatransfer.datapackageview)
* [DataPackagePropertySet]( /uwp/api/Windows.ApplicationModel.DataTransfer.DataPackagePropertySet)
* [Solicitud de consulta](/uwp/api/windows.applicationmodel.datatransfer.datarequest) 
* [DataRequested]( /uwp/api/Windows.ApplicationModel.DataTransfer.DataTransferManager)
* [FailWithDisplayText](/uwp/api/windows.applicationmodel.datatransfer.datarequest.failwithdisplaytext)
* [ShowShareUi](/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.showshareui)
* [RequestedOperation](/uwp/api/windows.applicationmodel.datatransfer.datapackage.requestedoperation) 
* [ControlsList](../design/controls-and-patterns/index.md)
* [SetContent](/uwp/api/windows.applicationmodel.datatransfer.clipboard.setcontent)
* [GetContent](/uwp/api/windows.applicationmodel.datatransfer.clipboard.getcontent)
* [AvailableFormats](/uwp/api/windows.applicationmodel.datatransfer.datapackageview.availableformats)
* [Contains](/uwp/api/windows.applicationmodel.datatransfer.datapackageview.contains)
* [ContentChanged](/uwp/api/windows.applicationmodel.datatransfer.clipboard.contentchanged)