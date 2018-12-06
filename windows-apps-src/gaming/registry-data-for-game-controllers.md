---
title: Datos del registro para dispositivos de juego
description: Obtén información sobre los datos que se pueden agregar al registro del PC para permitir que tu controlador se use en juegos UWP.
ms.assetid: 2DD0B384-8776-4599-9E52-4FC0AA682735
ms.date: 06/25/2018
ms.topic: article
keywords: windows 10, uwp, juegos, entrada, registro, personalizado
ms.localizationpriority: medium
ms.openlocfilehash: 3d30c19a7fd7641d76e810912d33a96dbbeb3132
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2018
ms.locfileid: "8752627"
---
# <a name="registry-data-for-game-controllers"></a>Datos del registro para dispositivos de juego

> [!NOTE]
> Este tema está destinado a los fabricantes de dispositivos de juego compatibles con Windows 10 y no es de interés para la mayoría de los desarrolladores.

El [espacio de nombres Windows.Gaming.Input](https://docs.microsoft.com/uwp/api/windows.gaming.input) permite a los proveedores de hardware independientes (IHV) agregar datos al registro del equipo, habilitar sus dispositivos para que aparezcan como [Gamepads](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamepad), [RacingWheels](https://docs.microsoft.com/uwp/api/windows.gaming.input.racingwheel), [ArcadeSticks](https://docs.microsoft.com/uwp/api/windows.gaming.input.arcadestick), [FlightSticks](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.flightstick) y [UINavigationControllers](https://docs.microsoft.com/uwp/api/windows.gaming.input.uinavigationcontroller), según corresponda. Todos los IHV deberían agregar estos datos para sus controladores compatibles. Al hacer esto, todos los juegos UWP (y los juegos de escritorio que usen la API de WinRT) será compatibles con el dispositivo de juego.

## <a name="mapping-scheme"></a>Esquema de asignación

Las asignaciones de un dispositivo con id. de proveedor (VID) **VVVV**, id. de producto (PID) **PPPP**, página de uso **UUUU**e id. de uso **XXXX**, se leerán desde esta ubicación del registro:

`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\VVVVPPPPUUUUXXXX`

La siguiente tabla explica los valores esperados en la ubicación raíz del dispositivo:

<table>
    <tr>
        <th>Nombre</th>
        <th>Tipo</th>
        <th>¿Es obligatorio?</th>
        <th>Información</th>
    </tr>
    <tr>
        <td>Deshabilitada</td>
        <td>DWORD</td>
        <td>No</td>
        <td>
            <p>Indica que este dispositivo particular debe estar deshabilitada.</p>
            <ul>
                <li><b>0</b>: el dispositivo no está deshabilitado.</li>
                <li><b>1</b>: el dispositivo está deshabilitado.</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>Descripción</td>
        <td>REG_SZ <td>No</td>
        <td>Descripción breve del dispositivo</td>
    </tr>
</table>

El programa de instalación del dispositivo debería agregar estos datos al registro (ya sea a través del programa de instalación o un [archivo INF](https://docs.microsoft.com/windows-hardware/drivers/install/inf-files)).

En las siguientes secciones se explican en detalle las subclaves de la ubicación raíz del dispositivo.

### <a name="gamepad"></a>Controlador para juegos

La siguiente tabla enumera las subclaves necesarias y opcionales en la subclave **Gamepad**:

<table>
    <tr>
        <th>Subclave</th>
        <th>¿Es obligatorio?</th>
        <th>Información</th>
    </tr>
    <tr>
        <td>Menú</td>
        <td>Sí</td>
        <td rowspan="18" style="vertical-align: middle;">Véase <a href="#button-mapping">Asignación de botones</a></td>
    </tr>
    <tr>
        <td>Ver</td>
        <td>Sí</td>
    </tr>
    <tr>
        <td>A</td>
        <td>Sí</td>
    </tr>
    <tr>
        <td>B</td>
        <td>Sí</td>
    </tr>
    <tr>
        <td>X</td>
        <td>Sí</td>
    </tr>
    <tr>
        <td>Y</td>
        <td>Sí</td>
    </tr>
    <tr>
        <td>LeftShoulder</td>
        <td>Sí</td>
    </tr>
    <tr>
        <td>RightShoulder</td>
        <td>Sí</td>
    </tr>
    <tr>
        <td>LeftThumbstickButton</td>
        <td>Sí</td>
    </tr>
    <tr>
        <td>RightThumbstickButton</td>
        <td>Sí</td>
    </tr>
    <tr>
        <td>DPadUp</td>
        <td>Sí</td>
    </tr>
    <tr>
        <td>DPadDown</td>
        <td>Sí</td>
    </tr>
    <tr>
        <td>DPadLeft</td>
        <td>Sí</td>
    </tr>
    <tr>
        <td>DPadRight</td>
        <td>Sí</td>
    </tr>
    <tr>
        <td>Paddle1</td>
        <td>No</td>
    </tr>
    <tr>
        <td>Paddle2</td>
        <td>No</td>
    </tr>
    <tr>
        <td>Paddle3</td>
        <td>No</td>
    </tr>
    <tr>
        <td>Paddle4</td>
        <td>No</td>
    </tr>
    <tr>
        <td>LeftTrigger</td>
        <td>Sí</td>
        <td rowspan="6" style="vertical-align: middle;">Véase <a href="#axis-mapping">Asignación de ejes</a></td>
    </tr>
    <tr>
        <td>RightTrigger</td>
        <td>Sí</td>
    </tr>
    <tr>
        <td>LeftThumbstickX</td>
        <td>Sí</td>
    </tr>
    <tr>
        <td>LeftThumbstickY</td>
        <td>Sí</td>
    </tr>
    <tr>
        <td>RightThumbstickX</td>
        <td>Sí</td>
    </tr>
    <tr>
        <td>RightThumbstickY</td>
        <td>Sí</td>
    </tr>
</table>

> [!NOTE]
> Si has agregado tu dispositivo de juego como **Gamepad** compatible, es muy recomendable que también lo agregues como compatible con **UINavigationController**.

### <a name="racingwheel"></a>RacingWheel

La siguiente tabla enumera las subclaves necesarias y opcionales en la subclave **RacingWheel**:

<table>
    <tr>
        <th>Subclave</th>
        <th>¿Es obligatorio?</th>
        <th>Información</th>
    </tr>
    <tr>
        <td>PreviousGear</td>
        <td>Sí</td>
        <td rowspan="30" style="vertical-align: middle;">Véase <a href="#button-mapping">Asignación de botones</a></td>
    </tr>
    <tr>
        <td>NextGear</td>
        <td>Sí</td>
    </tr>
    <tr>
        <td>DPadUp</td>
        <td>No</td>
    </tr>
    <tr>
        <td>DPadDown</td>
        <td>No</td>
    </tr>
    <tr>
        <td>DPadLeft</td>
        <td>No</td>
    </tr>
    <tr>
        <td>DPadRight</td>
        <td>No</td>
    </tr>
    <tr>
        <td>Button1</td>
        <td>No</td>
    </tr>
    <tr>
        <td>Button2</td>
        <td>No</td>
    </tr>
    <tr>
        <td>Button3</td>
        <td>No</td>
    </tr>
    <tr>
        <td>Button4</td>
        <td>No</td>
    </tr>
    <tr>
        <td>Button5</td>
        <td>No</td>
    </tr>
    <tr>
        <td>Button6</td>
        <td>No</td>
    </tr>
    <tr>
        <td>Button7</td>
        <td>No</td>
    </tr>
    <tr>
        <td>Button8</td>
        <td>No</td>
    </tr>
    <tr>
        <td>Button9</td>
        <td>No</td>
    </tr>
    <tr>
        <td>Button10</td>
        <td>No</td>
    </tr>
    <tr>
        <td>Button11</td>
        <td>No</td>
    </tr>
    <tr>
        <td>Button12</td>
        <td>No</td>
    </tr>
    <tr>
        <td>Button13</td>
        <td>No</td>
    </tr>
    <tr>
        <td>Button14</td>
        <td>No</td>
    </tr>
    <tr>
        <td>Button15</td>
        <td>No</td>
    </tr>
    <tr>
        <td>Button16</td>
        <td>No</td>
    </tr>
    <tr>
        <td>FirstGear</td>
        <td>No</td>
    </tr>
    <tr>
        <td>SecondGear</td>
        <td>No</td>
    </tr>
    <tr>
        <td>ThirdGear</td>
        <td>No</td>
    </tr>
    <tr>
        <td>FourthGear</td>
        <td>No</td>
    </tr>
    <tr>
        <td>FifthGear</td>
        <td>No</td>
    </tr>
    <tr>
        <td>SixthGear</td>
        <td>No</td>
    </tr>
    <tr>
        <td>SeventhGear</td>
        <td>No</td>
    </tr>
    <tr>
        <td>ReverseGear</td>
        <td>No</td>
    </tr>
    <tr>
        <td>Rueda</td>
        <td>Sí</td>
        <td rowspan="5" style="vertical-align: middle;">Véase <a href="#axis-mapping">Asignación de ejes</a></td>
    </tr>
    <tr>
        <td>Acelerador</td>
        <td>Sí</td>
    </tr>
    <tr>
        <td>Freno</td>
        <td>Sí</td>
    </tr>
    <tr>
        <td>Embrague</td>
        <td>No</td>
    </tr>
    <tr>
        <td>Freno de mano</td>
        <td>No</td>
    </tr>
    <tr>
        <td>MaxWheelAngle</td>
        <td>Sí</td>
        <td>Véase <a href="#properties-mapping">Asignación de propiedades</a></td>
    </tr>
</table>

### <a name="arcadestick"></a>ArcadeStick

La siguiente tabla enumera las subclaves necesarias y opcionales en la subclave **ArcadeStick**:

<table>
    <tr>
        <th>Subclave</th>
        <th>¿Es obligatorio?</th>
        <th>Información</th>
    </tr>
    <tr>
        <td>Acción 1</td>
        <td>Sí</td>
        <td rowspan="12" style="vertical-align: middle;">Véase <a href="#button-mapping">Asignación de botones</a></td>
    </tr>
    <tr>
        <td>Action2</td>
        <td>Sí</td>
    </tr>
    <tr>
        <td>Action3</td>
        <td>Sí</td>
    </tr>
    <tr>
        <td>Action4</td>
        <td>Sí</td>
    </tr>
    <tr>
        <td>Action5</td>
        <td>Sí</td>
    </tr>
    <tr>
        <td>Action6</td>
        <td>Sí</td>
    </tr>
    <tr>
        <td>Special1</td>
        <td>Sí</td>
    </tr>
    <tr>
        <td>Special2</td>
        <td>Sí</td>
    </tr>
    <tr>
        <td>StickUp</td>
        <td>Sí</td>
    </tr>
    <tr>
        <td>StickDown</td>
        <td>Sí</td>
    </tr>
    <tr>
        <td>StickLeft</td>
        <td>Sí</td>
    </tr>
    <tr>
        <td>StickRight</td>
        <td>Sí</td>
    </tr>
</table>

### <a name="flightstick"></a>FlightStick

La siguiente tabla enumera las subclaves necesarias y opcionales en la subclave **FlightStick**:

<table>
    <tr>
        <th>Subclave</th>
        <th>¿Es obligatorio?</th>
        <th>Información</th>
    </tr>
    <tr>
        <td>FirePrimary</td>
        <td>Sí</td>
        <td rowspan="2" style="vertical-align: middle;">Véase <a href="#button-mapping">Asignación de botones</a></td>
    </tr>
    <tr>
        <td>FireSecondary</td>
        <td>Sí</td>
    </tr>
    <tr>
        <td>Balanceo</td>
        <td>Sí</td>
        <td rowspan="4" style="vertical-align: middle;">Véase <a href="#axis-mapping">Asignación de ejes</a></td>
    </tr>
    <tr>
        <td>Cabeceo</td>
        <td>Sí</td>
    </tr>
    <tr>
        <td>Guiñada</td>
        <td>Sí</td>
    </tr>
    <tr>
        <td>Acelerador</td>
        <td>Sí</td>
    </tr>
    <tr>
        <td>HatSwitch</td>
        <td>Sí</td>
        <td>Véase <a href="#switch-mapping">Asignación de conmutador</a></td>
    </tr>
</table>

### <a name="uinavigation"></a>UINavigation

La siguiente tabla enumera las subclaves necesarias y opcionales en la subclave **UINavigation**:

<table>
    <tr>
        <th>Subclave</th>
        <th>¿Es obligatorio?</th>
        <th>Información</th>
    </tr>
    <tr>
        <td>Menú</td>
        <td>Sí</td>
        <td rowspan="24" style="vertical-align: middle;">Véase <a href="#button-mapping">Asignación de botones</a></td>
    </tr>
    <tr>
        <td>Ver</td>
        <td>Sí</td>
    </tr>
    <tr>
        <td>Aceptar</td>
        <td>Sí</td>
    </tr>
    <tr>
        <td>Cancelar</td>
        <td>Sí</td>
    </tr>
    <tr>
        <td>PrimaryUp</td>
        <td>Sí</td>
    </tr>
    <tr>
        <td>PrimaryDown</td>
        <td>Sí</td>
    </tr>
    <tr>
        <td>PrimaryLeft</td>
        <td>Sí</td>
    </tr>
    <tr>
        <td>PrimaryRight</td>
        <td>Sí</td>
    </tr>
    <tr>
        <td>Context1</td>
        <td>No</td>
    </tr>
    <tr>
        <td>Context2</td>
        <td>No</td>
    </tr>
    <tr>
        <td>Context3</td>
        <td>No</td>
    </tr>
    <tr>
        <td>Context4</td>
        <td>No</td>
    </tr>
    <tr>
        <td>PageUp</td>
        <td>No</td>
    </tr>
    <tr>
        <td>PageDown</td>
        <td>No</td>
    </tr>
    <tr>
        <td>PageLeft</td>
        <td>No</td>
    </tr>
    <tr>
        <td>PageRight</td>
        <td>No</td>
    </tr>
    <tr>
        <td>ScrollUp</td>
        <td>No</td>
    </tr>
    <tr>
        <td>ScrollDown</td>
        <td>No</td>
    </tr>
    <tr>
        <td>ScrollLeft</td>
        <td>No</td>
    </tr>
    <tr>
        <td>ScrollRight</td>
        <td>No</td>
    </tr>
    <tr>
        <td>SecondaryUp</td>
        <td>No</td>
    </tr>
    <tr>
        <td>SecondaryDown</td>
        <td>No</td>
    </tr>
    <tr>
        <td>SecondaryLeft</td>
        <td>No</td>
    </tr>
    <tr>
        <td>SecondaryRight</td>
        <td>No</td>
    </tr>
</table>

Para obtener más información sobre los controladores de navegación de interfaz de usuario y los comandos anteriores, consulta [Controlador de navegación de la interfaz de usuario](https://docs.microsoft.com/windows/uwp/gaming/ui-navigation-controller).

## <a name="keys"></a>Claves

Las siguientes secciones explican el contenido de cada una de las subclaves de las claves **Gamepad**, **RacingWheel**, **ArcadeStick**, **FlightStick** y **UINavigation**.

### <a name="button-mapping"></a>Asignación de botones

La siguiente tabla enumera los valores necesarios para asignar un botón. Por ejemplo, si al presionar **DPadUp** en el dispositivo de juego, la asignación de **DPadUp** debe contener el valor **ButtonIndex** (el **Origen** es **Botón**). Si **DPadUp** debe asignarse desde una posición de conmutador, en ese caso la asignación **DPadUp** debe contener los valores **SwitchIndex** y **SwitchPosition** (el **Origen** es **Conmutador**).

<table>
    <tr>
        <th>Origen</th>
        <th>Nombre de valor</th>
        <th>Tipo de valor</th>
        <th>¿Es obligatorio?</th>
        <th>Información de valor</th>
    </tr>
    <tr>
        <td>Botón</td>
        <td>ButtonIndex</td>
        <td>DWORD</td>
        <td>Sí</td>
        <td>Índice en la matriz de botones <b>RawGameController</b>.</td>
    </tr>
    <tr>
        <td rowspan="4" style="vertical-align: middle;">Eje</td>
        <td>AxisIndex</td>
        <td>DWORD</td>
        <td>Sí</td>
        <td>Índice en la matriz de ejes <b>RawGameController</b>.</td>
    </tr>
    <tr>
        <td>Invertir</td>
        <td>DWORD</td>
        <td>No</td>
        <td>Indica que el valor de eje se debe invertir antes de aplicar los factores <b>Threshold Percent</b> y <b>DebouncePercent</b>.</td>
    </tr>
    <tr>
        <td>ThresholdPercent</td>
        <td>DWORD</td>
        <td>Sí</td>
        <td>Indica la posición del eje en el que el valor del botón asignado cambia entre los estados presionado y liberado. El intervalo válido de valores es de 0 a 100. El botón se considera presionado si el valor de eje es mayor o igual a este valor.</td>
    </tr>
    <tr>
        <td>DebouncePercent</td>
        <td>DWORD</td>
        <td>Sí</td>
        <td>
            <p>Define el tamaño de una ventana alrededor del valor <b>ThresholdPercent</b>, que se usa para esperar el estado notificado del botón. El intervalo válido de valores es de 0 a 100. Las transiciones de estado del botón solo se pueden producir cuando el valor de eje cruza los límites superiores o inferiores de la ventana de espera. Por ejemplo, un <b>ThresholdPercent</b> de 50 y un <b>DebouncePercent</b> de 10 darán por resultado límites de espera de 45% y 55 % de los valores a escala completa del eje. El botón no puede realizar la transición al estado presionado hasta que el valor de eje alcance el 55% o más, y no puede pasar de vuelta al estado liberado hasta que el valor de eje alcance el 45% o menos.</p>
            <p>Los límites de la ventana de espera calculado están fijados entre 0% y 100%. Por ejemplo, un umbral del 5% y una ventana de espera del 20% provocarían que los límites de la ventana de espera cayeran al 0% y al 15%. El estado del botón para valores del eje del 0% y del 100% siempre se notifican como liberado y presionado, respectivamente, independientemente de los valores de umbral y espera.</p>
        </td>
    </tr>
    <tr>
        <td rowspan="3" style="vertical-align: middle;">Conmutador</td>
        <td>SwitchIndex</td>
        <td>DWORD</td>
        <td>Sí</td>
        <td>Índice en la matriz de conmutadores <b>RawGameController</b>.</td>
    </tr>
    <tr>
        <td>SwitchPosition</td>
        <td>REG_SZ</td>
        <td>Sí</td>
        <td>
            <p>Indica la posición del conmutador que hará que el botón asignado informe de que está siendo presionado. Los valores de posición pueden ser una de las siguientes cadenas:</p>
            <ul>
                <li>Up</li> 
                <li>UpRight</li>
                <li>Right</li>
                <li>DownRight</li>
                <li>Down</li>
                <li>DownLeft</li>
                <li>Left</li>
                <li>UpLeft</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>IncludeAdjacent</td>
        <td>DWORD</td>
        <td>No</td>
        <td>Indica que las posiciones de los conmutadores adyacentes provocarán también que el botón asignado informe de que está siendo presionado.</td>
    </tr>
</table>

### <a name="axis-mapping"></a>Asignación de ejes

La siguiente tabla enumera los valores necesarios para asignar un eje.

<table>
    <tr>
        <th>Origen</th>
        <th>Nombre de valor</th>
        <th>Tipo de valor</th>
        <th>¿Es obligatorio?</th>
        <th>Información de valor</th>
    </tr>
    <tr>
        <td rowspan="2" style="vertical-align: middle;">Botón</td>
        <td>MaxValueButtonIndex</td>
        <td>DWORD</td>
        <td>Sí</td>
        <td>
            <p>Índice de la matriz de botones <b>RawGameController</b> que se traduce en el valor de eje unidireccional asignado.</p>
            <table>
                <tr>
                    <th>MaxButton</th>
                    <th>AxisValue</th>
                </tr>
                <tr>
                    <td>FALSE</td>
                    <td>0.0</td>
                </tr>
                <tr>
                    <td>TRUE</td>
                    <td>1.0</td>
                </tr>
            </table>
        </td>
    </tr>
    <tr>
        <td>MinValueButtonIndex</td>
        <td>DWORD</td>
        <td>No</td>
        <td>
            <p>Indica que el eje asignado es bidireccional. Los valores de <b>MaxButton</b> y <b>MinButton</b> se combinan en un eje bidireccional único, como se muestra a continuación.</p>
            <table>
                <tr>
                    <th>MinButton</th>
                    <th>MaxButton</th>
                    <th>AxisValue</th>
                </tr>
                <tr>
                    <td>FALSE</td>
                    <td>FALSE</td>
                    <td>0.5</td>
                </tr>
                <tr>
                    <td>FALSE</td>
                    <td>TRUE</td>
                    <td>1.0</td>
                </tr>
                <tr>
                    <td>TRUE</td>
                    <td>FALSE</td>
                    <td>0.0</td>
                </tr>
                <tr>
                    <td>TRUE</td>
                    <td>TRUE</td>
                    <td>0.5</td>
                </tr>
            </table>
        </td>
    </tr>
    <tr>
        <td rowspan="2" style="vertical-align: middle;">Eje</td>
        <td>AxisIndex</td>
        <td>DWORD</td>
        <td>Sí</td>
        <td>Índice en la matriz de ejes <b>RawGameController</b>.</td>
    </tr>
    <tr>
        <td>Invertir</td>
        <td>DWORD</td>
        <td>No</td>
        <td>Indica que el valor de eje asignado se debe invertir antes de devolverlo.</td>
    </tr>
    <tr>
        <td rowspan="3" style="vertical-align: middle;">Conmutador</td>
        <td>SwitchIndex</td>
        <td>DWORD</td>
        <td>Sí</td>
        <td>Índice en la matriz de conmutadores <b>RawGameController</b>.
    </tr>
    <tr>
        <td>MaxValueSwitchPosition</td>
        <td>REG_SZ</td>
        <td>Sí</td>
        <td>
            <p>Una de las cadenas siguientes:</p>
            <ul>
                <li>Up</li>
                <li>UpRight</li>
                <li>Right</li>
                <li>DownRight</li>
                <li>Down</li>
                <li>DownLeft</li>
                <li>Left</li>
                <li>UpLeft</li>
            </ul>
            <p>Indica la posición del conmutador que hace que el valor de eje asignados se notifique como 1.0. La dirección opuesta de <b>MaxValueSwitchPosition</b> se trata como 0.0. Por ejemplo, si <b>MaxValueSwitchPosition</b> es <b>Up</b>, la traslación del valor de eje se muestra a continuación:</p>
            <table>
                <tr>
                    <th>Posición de conmutador</th>
                    <th>AxisValue</th>
                </tr>
                <tr>
                    <td>Up</td>
                    <td>1.0</td>
                </tr>
                <tr>
                    <td>Center</td>
                    <td>0.5</td>
                </tr>
                <tr>
                    <td>Down</td>
                    <td>0.0</td>
                </tr>
            </table>
        </td>
    </tr>
    <tr>
        <td>IncludeAdjacent</td>
        <td>DWORD</td>
        <td>No</td>
        <td>
            <p>Indica que las posiciones de los conmutadores adyacentes provocarán también que el botón asignado notifique 1.0. En el ejemplo anterior, si <b>IncludeAdjacent</b> está establecida, la traslación del eje se realiza como sigue:</p>
            <table>
                <tr>
                    <th>Posición de conmutador</th>
                    <th>AxisValue</th>
                </tr>
                <tr>
                    <td>Up</td>
                    <td>1.0</td>
                </tr>
                <tr>
                    <td>UpRight</td>
                    <td>1.0</td>
                </tr>
                <tr>
                    <td>UpLeft</td>
                    <td>1.0</td>
                </tr>
                <tr>
                    <td>Center</td>
                    <td>0.5</td>
                </tr>
                <tr>
                    <td>Down</td>
                    <td>0.0</td>
                </tr>
                <tr>
                    <td>DownRight</td>
                    <td>0.0</td>
                </tr>
                <tr>
                    <td>DownLeft</td>
                    <td>0.0</td>
                </tr>
            </table>
        </td>
    </tr>
</table>

### <a name="switch-mapping"></a>Asignación de conmutador

Las posiciones de los conmutadores pueden asignarse desde un conjunto de botones de la matriz de botones **RawGameController** o desde un índice de la matriz de conmutadores. Las posiciones de los conmutadores no se pueden asignar desde los ejes.

<table>
    <tr>
        <th>Origen</th>
        <th>Nombre de valor</th>
        <th>Tipo de valor</th>
        <th>Información de valor</th>
    </tr>
    <tr>
        <td rowspan="10" style="vertical-align: middle;">Botón</td>
        <td>ButtonCount</td>
        <td>DWORD</td>
        <td>2, 4 u 8</td>
    </tr>
    <tr>
        <td>SwitchKind</td>
        <td>REG_SZ</td>
        <td><b>TwoWay</b>, <b>FourWay</b> o <b>EightWay</b>
    </tr>
    <tr>
        <td>UpButtonIndex</td>
        <td>DWORD</td>
        <td rowspan="8" style="vertical-align: middle;">Véanse los <a href="#buttonindex-values">valores de *ButtonIndex</a></td>
    </tr>
    <tr>
        <td>DownButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>LeftButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>RightButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>UpRightButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>DownRightButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>DownLeftButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>UpLeftButtonIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td rowspan="9" style="vertical-align: middle;">Eje</td>
        <td>SwitchKind</td>
        <td>REG_SZ</td>
        <td><b>TwoWay</b>, <b>FourWay</b> o <b>EightWay</b></td>
    </tr>
    <tr>
        <td>XAxisIndex</td>
        <td>DWORD</td>
        <td rowspan="2" style="vertical-align: middle;"><b>YAxisIndex</b> siempre está presente. <b>XAxisIndex</b> solo está presente cuando <b>SwitchKind</b> es <b>FourWay</b> o <b>EightWay</b>.</td>
    </tr>
    <tr>
        <td>YAxisIndex</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>XDeadZonePercent</td>
        <td>DWORD</td>
        <td rowspan="2" style="vertical-align: middle;">Indicar el tamaño de la zona muerta alrededor de la posición del centro de los ejes.</td>
    </tr>
    <tr>
        <td>YDeadZonePercent</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>XDebouncePercent</td>
        <td>DWORD</td>
        <td rowspan="2" style="vertical-align: middle;">Definir el tamaño de las ventanas alrededor de los límites superior e inferior de la zona muerta, que se usan para esperar el estado notificado del conmutador.</td>
    </tr>
    <tr>
        <td>YDebouncePercent</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>XInvert</td>
        <td>DWORD</td>
        <td rowspan="2" style="vertical-align: middle;">Indica que los valores de eje correspondientes se deben invertir antes de la zona muerta y la ventana de espera.</td>
    </tr>
    <tr>
        <td>YInvert</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td rowspan="3" style="vertical-align: middle;">Conmutador</td>
        <td>SwitchIndex</td>
        <td>DWORD</td>
        <td>Índice en la matriz de conmutadores <b>RawGameController</b>.
    </tr>
    <tr>
        <td>Invertir</td>
        <td>DWORD</td>
        <td>Informa que el conmutador informa de sus posiciones en sentido contrario a las agujas del reloj, en lugar de en el sentido de las agujas del reloj.</td>
    </tr>
    <tr>
        <td>PositionBias</td>
        <td>DWORD</td>
        <td>
            <p>Desplaza el punto de partida de cómo se notifican las posiciones en la cantidad especificada. <b>PositionBias</b> se cuenta siempre en sentido contrario a las agujas del reloj desde el punto de partida original y se aplica antes de que se invierte el orden de los valores.</p>
            <p>Por ejemplo, un conmutador que notifica posiciones comenzando por <b>DownRight</b> en orden contrario a las agujas del reloj puede normalizarse estableciendo la marca <b>Invert</b> y especificando un <b>PositionBias</b> de 5:</p>
            <table>
                <tr>
                    <th>Posición</th>
                    <th>Valor notificado</th>
                    <th>Después de los marcadores PositionBias e Invert</th>
                </tr>
                <tr>
                    <td>DownRight</td>
                    <td>0</td>
                    <td>3</td>
                </tr>
                <tr>
                    <td>Right</td>
                    <td>1</td>
                    <td>2</td>
                </tr>
                <tr>
                    <td>UpRight</td>
                    <td>2</td>
                    <td>1</td>
                </tr>
                <tr>
                    <td>Up</td>
                    <td>3</td>
                    <td>0</td>
                </tr>
                <tr>
                    <td>UpLeft</td>
                    <td>4</td>
                    <td>7</td>
                </tr>
                <tr>
                    <td>Left</td>
                    <td>5</td>
                    <td>6</td>
                </tr>
                <tr>
                    <td>DownLeft</td>
                    <td>6</td>
                    <td>5</td>
                </tr>
                <tr>
                    <td>Down</td>
                    <td>7</td>
                    <td>4</td>
                </tr>
            </table>
    </tr>
</table>

#### <a name="buttonindex-values"></a>*Valores de ButtonIndex

\*Índice de valores de ButtonIndex en la matriz de botones de **RawGameController**:

<table>
    <tr>
        <th>ButtonCount</th>
        <th>SwitchKind</th>
        <th>RequiredMappings</th>
    </tr>
    <tr>
        <td>2</td>
        <td>TwoWay</td>
        <td>
            <ul>
                <li>UpButtonIndex</li>
                <li>DownButtonIndex</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>4</td>
        <td>FourWay</td>
        <td>
            <ul>
                <li>UpButtonIndex</li>
                <li>DownButtonIndex</li>
                <li>LeftButtonIndex</li>
                <li>RightButtonIndex</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>4</td>
        <td>EightWay</td>
        <td>
            <ul>
                <li>UpButtonIndex</li>
                <li>DownButtonIndex</li>
                <li>LeftButtonIndex</li>
                <li>RightButtonIndex</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>8</td>
        <td>EightWay</td>
        <td>
            <ul>
                <li>UpButtonIndex</li>
                <li>DownButtonIndex</li>
                <li>LeftButtonIndex</li>
                <li>RightButtonIndex</li>
                <li>UpRightButtonIndex</li>
                <li>DownRightButtonIndex</li>
                <li>DownLeftButtonIndex</li>
                <li>UpLeftButtonIndex</li>
            </ul>
        </td>
    </tr>
</table>

### <a name="properties-mapping"></a>Asignación de propiedades

Estos son valores de asignación estática para diferentes tipos de asignación.

<table>
    <tr>
        <th>Asignación</th>
        <th>Nombre de valor</th>
        <th>Tipo de valor</th>
        <th>Información de valor</th>
    </tr>
    <tr>
        <td>RacingWheel</td>
        <td>MaxWheelAngle</td>
        <td>DWORD</td>
        <td>Indica el ángulo de rueda físico máximo que admite la rueda en una determinada dirección. Por ejemplo, una rueda con una rotación posible de -90 grados a 90 grados, especificaría 90.</td>
    </tr>
</table>

## <a name="labels"></a>Etiquetas

Las etiquetas deben estar presentes en la clave **Labels**, en la raíz del dispositivo. **Labels** puede tener 3 subclaves: **Buttons**, **Axes** y **Switches**.

### <a name="button-labels"></a>Etiquetas de los botones

La clave **Buttons** asigna cada una de las posiciones de botón de la matriz de botones **RawGameController** a una cadena. Cada cadena se asigna internamente al valor de enumeración correspondiente de [GameControllerButtonLabel](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamecontrollerbuttonlabel). Por ejemplo, si un controlador para juegos tiene diez botones y el orden en que la **RawGameController** analiza los botones y los presenta en el informe de botones se obtiene algo así:

```cpp
Menu,               // Index 0
View,               // Index 1
LeftStickButton,    // Index 2
RightStickButton,   // Index 3
LetterA,            // Index 4
LetterB,            // Index 5
LetterX,            // Index 6
LetterY,            // Index 7
LeftBumper,         // Index 8
RightBumper         // Index 9
```

Las etiquetas deben aparecer en este orden en la clave **Buttons**:

<table>
    <tr>
        <th>Nombre</th>
        <th>Valor (tipo: REG_SZ)</th>
    </tr>
    <tr>
        <td>Button0</td>
        <td>Menú</td>
    </tr>
    <tr>
        <td>Button1</td>
        <td>Ver</td>
    </tr>
    <tr>
        <td>Button2</td>
        <td>LeftStickButton</td>
    </tr>
    <tr>
        <td>Button3</td>
        <td>RightStickButton</td>
    </tr>
    <tr>
        <td>Button4</td>
        <td>LetterA</td>
    </tr>
    <tr>
        <td>Button5</td>
        <td>LetterB</td>
    </tr>
    <tr>
        <td>Button6</td>
        <td>LetterX</td>
    </tr>
    <tr>
        <td>Button7</td>
        <td>LetterY</td>
    </tr>
    <tr>
        <td>Button8</td>
        <td>LeftBumper</td>
    </tr>
    <tr>
        <td>Button9</td>
        <td>RightBumper</td>
    </tr>
</table>

### <a name="axis-labels"></a>Etiquetas de ejes

La clave **Axes** asignará cada una de las posiciones de eje de la matriz de ejes **RawGameController** a una de las etiquetas que se enumeran en [GameControllerButtonLabel Enum](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.gamecontrollerbuttonlabel), al igual que las con las etiquetas de los botones. Consulta el ejemplo de [Etiquetas de botones](#button-labels).

### <a name="switch-labels"></a>Etiquetas de conmutadores

Los mapas de claves **Switches** convierten las posiciones a etiquetas. Los valores siguen esta convención de nomenclatura: para etiquetar una posición de un conmutador, cuyo índice es *x* en la matriz de conmutadores **RawGameController**, agrega estos valores en la subclave **Switches**: 

* SwitchxUp 
* SwitchxUpRight 
* SwitchxRight
* SwitchxDownRight
* SwitchxDown
* SwitchxDownLeft
* SwitchxUpLeft
* SwitchxLeft

La tabla siguiente muestra un ejemplo de conjunto de etiquetas para cambiar las posiciones de un conmutador de 4 vías que aparece en el índice 0 de la **RawGameController**: 

<table>
    <tr>
        <th>Nombre</th>
        <th>Valor (tipo: REG_SZ)</th>
    </tr>
    <tr>
        <td>Switch0Up</td>
        <td>XboxUp</td>
    </tr>
    <tr>
        <td>Switch0Right</td>
        <td>XboxRight</td>
    </tr>
    <tr>
        <td>Switch0Down</td>
        <td>XboxDown</td>
    </tr>
    <tr>
        <td>Switch0Left</td>
        <td>XboxLeft</td>
    </tr>
</table>

<!--### Label strings

* XboxBack
* XboxStart
* XboxMenu
* XboxView
* XboxUp
* XboxDown
* XboxLeft
* XboxRight
* XboxA
* XboxB
* XboxX
* XboxY
* XboxLeftBumper
* XboxLeftTrigger
* XboxLeftStickButton
* XboxRightBumper
* XboxRightTrigger
* XboxRightStickButton
* XboxPaddle1
* XboxPaddle2
* XboxPaddle3
* XboxPaddle4
* Mode
* Select
* Menu
* View
* Back
* Start
* Options
* Share
* Up
* Down
* Left
* Right
* LetterA
* LetterB
* LetterC
* LetterL
* LetterR
* LetterX
* LetterY
* LetterZ
* Cross
* Circle
* Square
* Triangle
* LeftBumper
* LeftTrigger
* LeftStickButton
* Left1
* Left2
* Left3
* RightBumper
* RightTrigger
* RightStickButton
* Right1
* Right2
* Right3
* Paddle1
* Paddle2
* Paddle3
* Paddle4
* Plus
* Minus
* DownLeftArrow
* DialLeft
* DialRight
* Suspension-->

## <a name="example-registry-file"></a>Ejemplo de archivo de registro:

Para mostrar cómo todas estas asignaciones y valores se conjuntan, aquí viene un archivo de registro de ejemplo para una **RacingWheel** genérica:

```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004]
"Description" = "Example Wheel Device"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\Labels\Buttons]
"Button0" = "LetterA"
"Button1" = "LetterB"
"Button2" = "LetterX"
"Button3" = "LetterY"
"Button6" = "Menu"
"Button7" = "View"
"Button8" = "RightStickButton"
"Button9" = "LeftStickButton"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\Labels\Switches]
"Switch0Down" = "Down"
"Switch0Left" = "Left"
"Switch0Right" = "Right"
"Switch0Up" = "Up"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel]
"MaxWheelAngle" = dword:000001c2

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Brake]
"AxisIndex" = dword:00000002
"Invert" = dword:00000001

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button1]
"ButtonIndex" = dword:00000000

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button2]
"ButtonIndex" = dword:00000001

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button3]
"ButtonIndex" = dword:00000002

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button4]
"ButtonIndex" = dword:00000003

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button5]
"ButtonIndex" = dword:00000009

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button6]
"ButtonIndex" = dword:00000008

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button7]
"ButtonIndex" = dword:00000007

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Button8]
"ButtonIndex" = dword:00000006

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Clutch]
"AxisIndex" = dword:00000003
"Invert" = dword:00000001

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\DPadDown]
"IncludeAdjacent" = dword:00000001
"SwitchIndex" = dword:00000000
"SwitchPosition" = "Down"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\DPadLeft]
"IncludeAdjacent" = dword:00000001
"SwitchIndex" = dword:00000000
"SwitchPosition" = "Left"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\DPadRight]
"IncludeAdjacent" = dword:00000001
"SwitchIndex" = dword:00000000
"SwitchPosition" = "Right"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\DPadUp]
"IncludeAdjacent" = dword:00000001
"SwitchIndex" = dword:00000000
"SwitchPosition" = "Up"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\FifthGear]
"ButtonIndex" = dword:00000010

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\FirstGear]
"ButtonIndex" = dword:0000000c

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\FourthGear]
"ButtonIndex" = dword:0000000f

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\NextGear]
"ButtonIndex" = dword:00000004

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\PreviousGear]
"ButtonIndex" = dword:00000005

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\ReverseGear]
"ButtonIndex" = dword:0000000b

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\SecondGear]
"ButtonIndex" = dword:0000000d

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\SixthGear]
"ButtonIndex" = dword:00000011

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\ThirdGear]
"ButtonIndex" = dword:0000000e

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Throttle]
"AxisIndex" = dword:00000001
"Invert" = dword:00000001

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\1234567800010004\RacingWheel\Wheel]
"AxisIndex" = dword:00000000
"Invert" = dword:00000000
```

## <a name="see-also"></a>Consulta también

* [Espacio de nombres Windows.Gaming.Input](https://docs.microsoft.com/uwp/api/windows.gaming.input)
* [Espacio de nombres Windows.Gaming.Input.Custom](https://docs.microsoft.com/uwp/api/windows.gaming.input.custom)
* [Archivos INF](https://docs.microsoft.com/windows-hardware/drivers/install/inf-files)