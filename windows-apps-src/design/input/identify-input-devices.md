---
description: Identifique los dispositivos de entrada conectados a un dispositivo de aplicación de Windows e identifique sus capacidades y atributos.
title: Identificar dispositivos de entrada
ms.assetid: B2E93FBF-C508-44D9-BA46-ECFDAA8746F4
label: Identify input devices
template: detail.hbs
keywords: dispositivo, digitalizador, entrada, interacción
ms.date: 09/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 51d5ea7bf6776c2c728fd000ffea63b79a4d7464
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030428"
---
# <a name="identify-input-devices"></a>Identificar dispositivos de entrada


Identifique los dispositivos de entrada conectados a un dispositivo de aplicación de Windows e identifique sus capacidades y atributos.

> **API importantes** : [**Windows. Devices. Input**](/uwp/api/Windows.Devices.Input), [**Windows. UI. Input**](/uwp/api/Windows.UI.Core), [**Windows. UI. Xaml. Input**](/uwp/api/Windows.UI.Input)

## <a name="retrieve-mouse-properties"></a>Recuperar las propiedades del mouse


El espacio de nombres [**Windows.Devices.Input**](/uwp/api/Windows.Devices.Input) contiene la clase [**MouseCapabilities**](/uwp/api/Windows.Devices.Input.MouseCapabilities) que se usa para recuperar las propiedades expuestas por uno o varios dispositivos mouse conectados. Solo es necesario crear un nuevo objeto **MouseCapabilities** y obtener las propiedades que te interesan.

**Nota:** Los valores que devuelven las propiedades que se describen aquí se basan en todos los dispositivos mouse detectados: las propiedades booleanas devuelven un valor distinto de cero si hay al menos un mouse que admite una funcionalidad específica. Por otro lado, las propiedades numéricas devuelven el valor máximo expuesto por cualquiera de los dispositivos mouse.

 

En el código siguiente se usa una serie de elementos [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) para mostrar las propiedades y los valores de un mouse individual.

```CSharp
private void GetMouseProperties()
{
    MouseCapabilities mouseCapabilities = new Windows.Devices.Input.MouseCapabilities();
    MousePresent.Text = mouseCapabilities.MousePresent != 0 ? "Yes" : "No";
    VertWheel.Text = mouseCapabilities.VerticalWheelPresent != 0 ? "Yes" : "No";
    HorzWheel.Text = mouseCapabilities.HorizontalWheelPresent != 0 ? "Yes" : "No";
    SwappedButtons.Text = mouseCapabilities.SwapButtons != 0 ? "Yes" : "No";
    NumButtons.Text = mouseCapabilities.NumberOfButtons.ToString();
}
```

## <a name="retrieve-keyboard-properties"></a>Recuperar las propiedades del teclado


El espacio de nombres [**Windows.Devices.Input**](/uwp/api/Windows.Devices.Input) contiene la clase [**KeyboardCapabilities**](/uwp/api/Windows.Devices.Input.KeyboardCapabilities) que se usa para recuperar si hay algún teclado conectado. Solo es necesario crear un nuevo objeto **KeyboardCapabilities** y obtener la propiedad [**KeyboardPresent**](/uwp/api/windows.devices.input.keyboardcapabilities.keyboardpresent).

En el código siguiente se usa un elemento [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) para mostrar la propiedad y el valor del teclado.

```CSharp
private void GetKeyboardProperties()
{
    KeyboardCapabilities keyboardCapabilities = new Windows.Devices.Input.KeyboardCapabilities();
    KeyboardPresent.Text = keyboardCapabilities.KeyboardPresent != 0 ? "Yes" : "No";
}
```

## <a name="retrieve-touch-properties"></a>Recuperar las propiedades táctiles


El espacio de nombres [**Windows.Devices.Input**](/uwp/api/Windows.Devices.Input) contiene la clase [**TouchCapabilities**](/uwp/api/Windows.Devices.Input.TouchCapabilities) que se usa para recuperar si hay algún digitalizador táctil conectado. Solo es necesario crear un nuevo objeto **TouchCapabilities** y obtener las propiedades que te interesan.

**Nota:** Los valores que devuelven las propiedades que se describen aquí se basan en todos los digitalizadores táctiles detectados: las propiedades booleanas devuelven un valor distinto de cero si hay al menos un digitalizador que es compatible con una funcionalidad específica. Por otro lado, las propiedades numéricas devuelven el valor máximo expuesto por cualquiera de los digitalizadores.

 

En el código siguiente se usa una serie de elementos [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) para mostrar las propiedades y los valores táctiles.

```CSharp
private void GetTouchProperties()
{
    TouchCapabilities touchCapabilities = new Windows.Devices.Input.TouchCapabilities();
    TouchPresent.Text = touchCapabilities.TouchPresent != 0 ? "Yes" : "No";
    Contacts.Text = touchCapabilities.Contacts.ToString();
}
```

## <a name="retrieve-pointer-properties"></a>Recuperar las propiedades del puntero


El espacio de nombres [**Windows.Devices.Input**](/uwp/api/Windows.Devices.Input) contiene la clase [**PointerDevice**](/uwp/api/Windows.Devices.Input.PointerDevice) que se usa para recuperar si hay dispositivos detectados compatibles con la entrada de puntero (táctil, teclado táctil, mouse o pluma). Solo tienes que crear un nuevo objeto **PointerDevice** y obtener las propiedades que te interesan.

**Nota** Los valores que devuelven las propiedades que se describen aquí se basan en todos los dispositivos de puntero detectados: las propiedades booleanas devuelven un valor distinto de cero si hay al menos un digitalizador que es compatible con una funcionalidad específica. Por otro lado, las propiedades numéricas devuelven el valor máximo expuesto por cualquiera de los dispositivos de puntero.

En el código siguiente se usa una tabla para mostrar las propiedades y los valores correspondientes a cada dispositivo señalador.

```CSharp
private void GetPointerDevices()
{
    IReadOnlyList<PointerDevice> pointerDevices = Windows.Devices.Input.PointerDevice.GetPointerDevices();
    int gridRow = 0;
    int gridColumn = 0;

    for (int i = 0; i < pointerDevices.Count; i++)
    {
        // Pointer device type.
        TextBlock textBlock1 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock1);
        textBlock1.Text = (i + 1).ToString() + " Pointer Device Type:";
        Grid.SetRow(textBlock1, gridRow);
        Grid.SetColumn(textBlock1, gridColumn);

        TextBlock textBlock2 = new TextBlock();
        textBlock2.Text = pointerDevices[i].PointerDeviceType.ToString();
        Grid_PointerProps.Children.Add(textBlock2);
        Grid.SetRow(textBlock2, gridRow++);
        Grid.SetColumn(textBlock2, gridColumn + 1);

        // Is external?
        TextBlock textBlock3 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock3);
        textBlock3.Text = (i + 1).ToString() + " Is External?";
        Grid.SetRow(textBlock3, gridRow);
        Grid.SetColumn(textBlock3, gridColumn);

        TextBlock textBlock4 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock4);
        textBlock4.Text = pointerDevices[i].IsIntegrated.ToString();
        Grid.SetRow(textBlock4, gridRow++);
        Grid.SetColumn(textBlock4, gridColumn + 1);

        // Maximum contacts.
        TextBlock textBlock5 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock5);
        textBlock5.Text = (i + 1).ToString() + " Max Contacts:";
        Grid.SetRow(textBlock5, gridRow);
        Grid.SetColumn(textBlock5, gridColumn);

        TextBlock textBlock6 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock6);
        textBlock6.Text = pointerDevices[i].MaxContacts.ToString();
        Grid.SetRow(textBlock6, gridRow++);
        Grid.SetColumn(textBlock6, gridColumn + 1);

        // Physical device rectangle.
        TextBlock textBlock7 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock7);
        textBlock7.Text = (i + 1).ToString() + " Physical Device Rect:";
        Grid.SetRow(textBlock7, gridRow);
        Grid.SetColumn(textBlock7, gridColumn);

        TextBlock textBlock8 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock8);
        textBlock8.Text = pointerDevices[i].PhysicalDeviceRect.X.ToString() + "," +
            pointerDevices[i].PhysicalDeviceRect.Y.ToString() + "," +
            pointerDevices[i].PhysicalDeviceRect.Width.ToString() + "," +
            pointerDevices[i].PhysicalDeviceRect.Height.ToString();
        Grid.SetRow(textBlock8, gridRow++);
        Grid.SetColumn(textBlock8, gridColumn + 1);

        // Screen rectangle.
        TextBlock textBlock9 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock9);
        textBlock9.Text = (i + 1).ToString() + " Screen Rect:";
        Grid.SetRow(textBlock9, gridRow);
        Grid.SetColumn(textBlock9, gridColumn);

        TextBlock textBlock10 = new TextBlock();
        Grid_PointerProps.Children.Add(textBlock10);
        textBlock10.Text = pointerDevices[i].ScreenRect.X.ToString() + "," +
            pointerDevices[i].ScreenRect.Y.ToString() + "," +
            pointerDevices[i].ScreenRect.Width.ToString() + "," +
            pointerDevices[i].ScreenRect.Height.ToString();
        Grid.SetRow(textBlock10, gridRow++);
        Grid.SetColumn(textBlock10, gridColumn + 1);

        gridColumn += 2;
        gridRow = 0;
    }
```

## <a name="related-articles"></a>Artículos relacionados

### <a name="samples"></a>Ejemplos

- [Ejemplo de entrada básica](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [Ejemplo de entrada de latencia baja](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [Ejemplo de modo de interacción del usuario](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)

### <a name="archive-samples"></a>Ejemplos de archivo

- [Entrada: muestra de funcionalidades del dispositivo](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
