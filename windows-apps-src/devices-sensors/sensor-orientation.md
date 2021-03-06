---
ms.assetid: B4A550E7-1639-4C9A-A229-31E22B1415E7
title: Orientación del sensor
description: Los datos de sensor procedentes de las clases Accelerometer, Gyrometer, Compass, Inclinometer y OrientationSensor se definen por medio de sus ejes de referencia. Estos ejes se definen a su vez mediante la orientación horizontal del dispositivo y, por tanto, giran con el dispositivo cuando el usuario lo voltea.
ms.date: 07/03/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8836753778b1dd5dcbc8856b0df5ec1f11d8e753
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159809"
---
# <a name="sensor-orientation"></a>Orientación del sensor

Los datos de sensor procedentes de las clases [**Accelerometer**](/uwp/api/Windows.Devices.Sensors.Accelerometer), [**Gyrometer**](/uwp/api/Windows.Devices.Sensors.Gyrometer), [**Compass**](/uwp/api/Windows.Devices.Sensors.Compass), [**Inclinometer**](/uwp/api/Windows.Devices.Sensors.Inclinometer) y [**OrientationSensor**](/uwp/api/Windows.Devices.Sensors.OrientationSensor) se definen por medio de sus ejes de referencia. Estos ejes se definen mediante el marco de referencia del dispositivo y giran con el dispositivo cuando el usuario lo convierte. Si tu aplicación admite el giro automático y cambia su orientación para amoldarse al dispositivo cuando el usuario lo gire, deberás ajustar los datos de sensor para el giro antes de usarlos.

### <a name="important-apis"></a>API importantes

- [**Windows.Devices.Sensors**](/uwp/api/Windows.Devices.Sensors)
- [**Windows. Devices. Sensors. Custom**](/uwp/api/Windows.Devices.Sensors.Custom)

## <a name="display-orientation-vs-device-orientation"></a>Orientación de la pantalla y orientación del dispositivo

Para poder comprender los ejes de referencia de los sensores, debes distinguir entre la orientación de la pantalla y la orientación del dispositivo. La orientación de la pantalla es la dirección en la que se muestran las imágenes y el texto en la pantalla, mientras que la orientación del dispositivo es el posicionamiento físico del dispositivo.

> [!NOTE]
> El eje z positivo se extiende desde la pantalla del dispositivo, tal como se muestra en la siguiente imagen.
> :::image type="content" source="images/sensor-orientation-zaxis-1-small.png" alt-text="Eje Z para portátil":::

En los diagramas siguientes, tanto el dispositivo como la orientación de la pantalla se encuentran en [horizontal](/uwp/api/Windows.Graphics.Display.DisplayOrientations) (los ejes del sensor mostrados son específicos de la orientación horizontal).


En este diagrama se muestra la orientación de pantalla y de dispositivo en [horizontal](/uwp/api/Windows.Graphics.Display.DisplayOrientations).

:::image type="content" source="images/sensor-orientation-a-small.jpg" alt-text="Orientación de la pantalla y del dispositivo en horizontal":::

En el diagrama siguiente se muestra la orientación de pantalla y de dispositivo en [LandscapeFlipped](/uwp/api/Windows.Graphics.Display.DisplayOrientations).

:::image type="content" source="images/sensor-orientation-b-small.jpg" alt-text="Orientación de la pantalla y del dispositivo en LandscapeFlipped":::

Este diagrama final muestra la orientación de la pantalla en horizontal mientras que la orientación del dispositivo es [LandscapeFlipped](/uwp/api/Windows.Graphics.Display.DisplayOrientations).

:::image type="content" source="images/sensor-orientation-c-small.jpg" alt-text="Orientación de pantalla en Landscape con la orientación del dispositivo en LandscapeFlipped":::

Los valores de orientación se consultan a través de la clase [**DisplayInformation**](/uwp/api/Windows.Graphics.Display.DisplayInformation), usando para ello el método [**GetForCurrentView**](/uwp/api/windows.graphics.display.displayinformation.getforcurrentview) con la propiedad [**CurrentOrientation**](/uwp/api/windows.graphics.display.displayinformation.currentorientation). Luego, se puede crear lógica contrastándolos con la enumeración [**DisplayOrientations**](/uwp/api/Windows.Graphics.Display.DisplayOrientations). No olvides que, por cada orientación que admitas, deberás admitir también una conversión de los ejes de referencia a dicha orientación.

## <a name="landscape-first-vs-portrait-first-devices"></a>Dispositivos con orientación horizontal predeterminada y dispositivos con orientación vertical predeterminada

Los fabricantes producen dispositivos tanto con orientación horizontal predeterminada como con orientación vertical predeterminada. El marco de referencia varía entre dispositivos con orientación horizontal predeterminada (por ejemplo, equipos de escritorio y portátiles) y dispositivos con orientación vertical predeterminada (por ejemplo, teléfonos y algunas tabletas). En la siguiente tabla se muestran los ejes de sensor para ambos tipos de dispositivo, con orientación horizontal predeterminada y con orientación vertical predeterminada.

| Orientación | Con orientación horizontal predeterminada | Con orientación vertical predeterminada |
|-------------|-----------------|----------------|
| **Horizontal** | :::image type="content" source="images/sensor-orientation-0-small.jpg" alt-text="Dispositivo con orientación horizontal predeterminada en orientación Landscape"::: | :::image type="content" source="images/sensor-orientation-1-small.jpg" alt-text="Dispositivo con orientación vertical predeterminada en orientación Landscape"::: |
| **Vertical** | :::image type="content" source="images/sensor-orientation-2-small.jpg" alt-text="Dispositivo con orientación horizontal predeterminada en orientación Portrait"::: | :::image type="content" source="images/sensor-orientation-3-small.jpg" alt-text="Dispositivo con orientación vertical predeterminada en orientación Portrait"::: |
| **LandscapeFlipped** | :::image type="content" source="images/sensor-orientation-4-small.jpg" alt-text="Dispositivo con orientación horizontal predeterminada en orientación LandscapeFlipped"::: | :::image type="content" source="images/sensor-orientation-5-small.jpg" alt-text="Dispositivo con orientación vertical predeterminada en orientación LandscapeFlipped":::
| **PortraitFlipped** | :::image type="content" source="images/sensor-orientation-6-small.jpg" alt-text="Dispositivo con orientación horizontal predeterminada en orientación PortraitFlipped"::: | :::image type="content" source="images/sensor-orientation-7-small.jpg" alt-text="Dispositivo con orientación vertical predeterminada en orientación PortraitFlipped"::: |

## <a name="devices-broadcasting-display-and-headless-devices"></a>Difusión de presentaciones con dispositivos y dispositivos sin periféricos

Algunos dispositivos tienen la capacidad de difundir la presentación a otro dispositivo. Por ejemplo, podrías tener una tableta y difundir la presentación a un proyector que estará orientado horizontalmente. En este escenario es importante tener en cuenta que la orientación del dispositivo se basa en el dispositivo original, no en el que presenta la pantalla. Por lo tanto, un acelerómetro notificará los datos de la tableta.

Además, algunos dispositivos no disponen de una pantalla. Con estos dispositivos, la orientación predeterminada de los mismos es vertical.

## <a name="display-orientation-and-compass-heading"></a>Orientación de la pantalla y encabezado de brújula

El encabezado de brújula depende de los ejes de referencia, de modo que cuando la orientación del dispositivo lo hace. La compensación se efectúa siguiendo esta tabla (suponiendo que el usuario mira al norte).

| Orientación de la pantalla | Eje de referencia y encabezado de brújula | Encabezado de la brújula de API cuando se enfrente al norte (horizontalmente-primero) | Encabezado de la brújula de API cuando se enfrente al norte (vertical-primero) |Compensación de encabezado de brújula (en horizontal: primero) | Compensación de encabezado de brújula (primero en vertical) |
|---------------------|------------------------------------|---------------------------------------------------------|--------------------------------------------------------|------------------------------------------------|-----------------------------------------------|
| Horizontal           | -Z | 0   | 270 | Dirección               | (Encabezado+90) % 360  |
| Vertical            |  Y | 90  | 0   | (Encabezado+270) % 360 |  Dirección              |
| LandscapeFlipped    |  Z | 180 | 90  | (Encabezado+180) % 360 | (Encabezado+270) % 360 |
| PortraitFlipped     |  Y | 270 | 180 | (Encabezado+90) % 360  | (Encabezado+180) % 360 |

Modifica el encabezado de brújula como se indica en la tabla para que dicho encabezado se muestre correctamente. En el siguiente fragmento de código, se muestra cómo hacerlo.

```csharp
private void ReadingChanged(object sender, CompassReadingChangedEventArgs e)
{
    double heading = e.Reading.HeadingMagneticNorth;
    double displayOffset;

    // Calculate the compass heading offset based on
    // the current display orientation.
    DisplayInformation displayInfo = DisplayInformation.GetForCurrentView();

    switch (displayInfo.CurrentOrientation)
    {
        case DisplayOrientations.Landscape:
            displayOffset = 0;
            break;
        case DisplayOrientations.Portrait:
            displayOffset = 270;
            break;
        case DisplayOrientations.LandscapeFlipped:
            displayOffset = 180;
            break;
        case DisplayOrientations.PortraitFlipped:
            displayOffset = 90;
            break;
     }

    double displayCompensatedHeading = (heading + displayOffset) % 360;

    // Update the UI...
}
```

## <a name="display-orientation-with-the-accelerometer-and-gyrometer"></a>Orientación de la pantalla con acelerómetro y girómetro

Esta tabla convierte los datos de acelerómetro y girómetro para los datos de la pantalla.

| Ejes de referencia        |  X |  Y | Z |
|-----------------------|----|----|---|
| **Horizontal**         |  X |  Y | Z |
| **Vertical**          |  Y | -X | Z |
| **LandscapeFlipped**  | -X | -y | Z |
| **PortraitFlipped**   | -y |  X | Z |

El siguiente ejemplo de código aplica estas conversiones al girómetro.

```csharp
private void ReadingChanged(object sender, GyrometerReadingChangedEventArgs e)
{
    double x_Axis;
    double y_Axis;
    double z_Axis;

    GyrometerReading reading = e.Reading;  

    // Calculate the gyrometer axes based on
    // the current display orientation.
    DisplayInformation displayInfo = DisplayInformation.GetForCurrentView();
    switch (displayInfo.CurrentOrientation)
    {
        case DisplayOrientations.Landscape:
            x_Axis = reading.AngularVelocityX;
            y_Axis = reading.AngularVelocityY;
            z_Axis = reading.AngularVelocityZ;
            break;
        case DisplayOrientations.Portrait:
            x_Axis = reading.AngularVelocityY;
            y_Axis = -1 * reading.AngularVelocityX;
            z_Axis = reading.AngularVelocityZ;
            break;
        case DisplayOrientations.LandscapeFlipped:
            x_Axis = -1 * reading.AngularVelocityX;
            y_Axis = -1 * reading.AngularVelocityY;
            z_Axis = reading.AngularVelocityZ;
            break;
        case DisplayOrientations.PortraitFlipped:
            x_Axis = -1 * reading.AngularVelocityY;
            y_Axis = reading.AngularVelocityX;
            z_Axis = reading.AngularVelocityZ;
            break;
     }

    // Update the UI...
}
```

## <a name="display-orientation-and-device-orientation"></a>Orientación de la pantalla y orientación del dispositivo

Los datos de [**OrientationSensor**](/uwp/api/Windows.Devices.Sensors.OrientationSensor) se cambian de otra forma. Considere estas orientaciones diferentes como giros en sentido contrario a las agujas del reloj al eje z, por lo que es necesario invertir la rotación para obtener la orientación del usuario. En el caso de los datos de cuaternión, podemos emplear la fórmula de Euler para definir un giro con un cuaternión de referencia. También podemos usar una matriz de giro de referencia.

:::image type="content" source="images/eulers-formula.png" alt-text="Fórmula de Euler":::

Para obtener la orientación relativa que deseas, multiplica el objeto de referencia por el objeto absoluto. Este cálculo no es commutativo.

:::image type="content" source="images/orientation-formula.png" alt-text="Multiplicar el objeto de referencia por el objeto absoluto":::

En la expresión anterior, los datos de sensor devuelven el objeto absoluto.

| Orientación de la pantalla  | Giro en el sentido contrario a las agujas del reloj en torno a Z | Cuaternión de referencia (giro inverso) | Matriz de giro de referencia (giro inverso) |
|----------------------|------------------------------------|-----------------------------------------|----------------------------------------------|
| **Horizontal**        | 0                                  | 1 + 0i + 0j + 0k                        | \[1 0 0<br/> 0 1 0<br/> 0 0 1\]               |
| **Vertical**         | 90                                 | cos(-45⁰) + (i + j + k)*sin(-45⁰)       | \[0 1 0<br/>-1 0 0<br/>0 0 1]              |
| **LandscapeFlipped** | 180                                | 0 - i - j - k                           | \[1 0 0<br/> 0 1 0<br/> 0 0 1]               |
| **PortraitFlipped**  | 270                                | cos(-135⁰) + (i + j + k)*sin(-135⁰)     | \[0 -1 0<br/> 1  0 0<br/> 0  0 1]             |

## <a name="see-also"></a>Vea también

[Integración de sensores de movimiento y orientación](/windows-hardware/design/whitepapers/integrating-motion-and-orientation-sensors)