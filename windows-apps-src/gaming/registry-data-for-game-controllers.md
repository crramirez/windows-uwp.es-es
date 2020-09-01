---
title: Datos del registro para dispositivos de juego
description: Obtenga información sobre los datos que puede Agregar al registro del equipo para habilitar el uso del controlador en los juegos para UWP.
ms.assetid: 2DD0B384-8776-4599-9E52-4FC0AA682735
ms.date: 04/08/2019
ms.topic: article
keywords: Windows 10, UWP, juegos, entrada, registro, personalizado
ms.localizationpriority: medium
ms.openlocfilehash: ac2ca98a067fb88dfcdc86c4e4ee4047b82206bc
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159279"
---
# <a name="registry-data-for-game-controllers"></a>Datos del registro para dispositivos de juego

> [!NOTE]
> Este tema está destinado a los fabricantes de controladores de juegos compatibles con Windows 10 y no se aplican a la mayoría de los desarrolladores.

El [espacio de nombres Windows. Gaming. Input](/uwp/api/windows.gaming.input) permite a los fabricantes de hardware independientes (IHV) agregar datos al registro del equipo, lo que permite que los dispositivos aparezcan como [controladores de juegos](/uwp/api/windows.gaming.input.gamepad), [RacingWheels](/uwp/api/windows.gaming.input.racingwheel), [ArcadeSticks](/uwp/api/windows.gaming.input.arcadestick), [FlightSticks](/uwp/api/windows.gaming.input.flightstick)y [UINavigationControllers](/uwp/api/windows.gaming.input.uinavigationcontroller) , según corresponda. Todos los IHV deben agregar estos datos para sus controladores compatibles. Al hacerlo, todos los juegos para UWP (y cualquier juego de escritorio que use la API de WinRT) podrán admitir el dispositivo de juego.

## <a name="mapping-scheme"></a>Esquema de asignación

Las asignaciones de un dispositivo con el identificador de proveedor (VID) **vvvv**, el ID. de producto (PID) **pppp**, la página de uso **UUUU**y el ID. de uso **xxxx**se leerán desde esta ubicación en el registro:

`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GameInput\Devices\VVVVPPPPUUUUXXXX`

En la tabla siguiente se explican los valores esperados en la ubicación raíz del dispositivo:

<table>
    <tr>
        <th>Nombre</th>
        <th>Tipo</th>
        <th>¿Necesario?</th>
        <th>Información</th>
    </tr>
    <tr>
        <td>Disabled</td>
        <td>DWORD</td>
        <td>No</td>
        <td>
            <p>Indica que se debe deshabilitar este dispositivo en particular.</p>
            <ul>
                <li><b>0</b>: el dispositivo no está deshabilitado.</li>
                <li><b>1</b>: el dispositivo está deshabilitado.</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>Descripción</td>
        <td>REG_SZ <td>No</td>
        <td>Breve descripción del dispositivo.</td>
    </tr>
</table>

El instalador del dispositivo debe agregar estos datos al registro (ya sea a través del programa de instalación o un [archivo INF](https://docs.microsoft.com/windows-hardware/drivers/install/inf-files)).

Las subclaves de la ubicación raíz del dispositivo se detallan en las secciones siguientes.

### <a name="gamepad"></a>Controlador para juegos

En la tabla siguiente se enumeran las subclaves obligatorias y opcionales en la subclave del **controlador de juegos** :

<table>
    <tr>
        <th>Subclave</th>
        <th>¿Necesario?</th>
        <th>Información</th>
    </tr>
    <tr>
        <td>Menú</td>
        <td>Sí</td>
        <td rowspan="18" style="vertical-align: middle;">Consulte <a href="#button-mapping">asignación de botones</a></td>
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
        <td rowspan="6" style="vertical-align: middle;">Consulte <a href="#axis-mapping">asignación de eje</a></td>
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
> Si agrega el dispositivo de juego como un controlador de **juegos**compatible, es muy recomendable que también lo agregue como **UINavigationController**compatible.

### <a name="racingwheel"></a>RacingWheel

En la tabla siguiente se enumeran las subclaves obligatorias y opcionales en la subclave **RacingWheel** :

<table>
    <tr>
        <th>Subclave</th>
        <th>¿Necesario?</th>
        <th>Información</th>
    </tr>
    <tr>
        <td>PreviousGear</td>
        <td>Sí</td>
        <td rowspan="30" style="vertical-align: middle;">Consulte <a href="#button-mapping">asignación de botones</a></td>
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
        <td rowspan="5" style="vertical-align: middle;">Consulte <a href="#axis-mapping">asignación de eje</a></td>
    </tr>
    <tr>
        <td>Limitación</td>
        <td>Sí</td>
    </tr>
    <tr>
        <td>Resistencia</td>
        <td>Sí</td>
    </tr>
    <tr>
        <td>Embrague</td>
        <td>No</td>
    </tr>
    <tr>
        <td>Handbrake</td>
        <td>No</td>
    </tr>
    <tr>
        <td>MaxWheelAngle</td>
        <td>Sí</td>
        <td>Ver la <a href="#properties-mapping">asignación de propiedades</a></td>
    </tr>
</table>

### <a name="arcadestick"></a>ArcadeStick

En la tabla siguiente se enumeran las subclaves obligatorias y opcionales en la subclave **ArcadeStick** :

<table>
    <tr>
        <th>Subclave</th>
        <th>¿Necesario?</th>
        <th>Información</th>
    </tr>
    <tr>
        <td>Acción 1</td>
        <td>Sí</td>
        <td rowspan="12" style="vertical-align: middle;">Consulte <a href="#button-mapping">asignación de botones</a></td>
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

En la tabla siguiente se enumeran las subclaves obligatorias y opcionales en la subclave **FlightStick** :

<table>
    <tr>
        <th>Subclave</th>
        <th>¿Necesario?</th>
        <th>Información</th>
    </tr>
    <tr>
        <td>FirePrimary</td>
        <td>Sí</td>
        <td rowspan="2" style="vertical-align: middle;">Consulte <a href="#button-mapping">asignación de botones</a></td>
    </tr>
    <tr>
        <td>FireSecondary</td>
        <td>Sí</td>
    </tr>
    <tr>
        <td>Volver</td>
        <td>Sí</td>
        <td rowspan="4" style="vertical-align: middle;">Consulte <a href="#axis-mapping">asignación de eje</a></td>
    </tr>
    <tr>
        <td>Inclinación</td>
        <td>Sí</td>
    </tr>
    <tr>
        <td>Eje</td>
        <td>Sí</td>
    </tr>
    <tr>
        <td>Limitación</td>
        <td>Sí</td>
    </tr>
    <tr>
        <td>HatSwitch</td>
        <td>Sí</td>
        <td>Consulte <a href="#switch-mapping">asignación de conmutadores</a></td>
    </tr>
</table>

### <a name="uinavigation"></a>UINavigation

En la tabla siguiente se enumeran las subclaves obligatorias y opcionales en la subclave **UINavigation** :

<table>
    <tr>
        <th>Subclave</th>
        <th>¿Necesario?</th>
        <th>Información</th>
    </tr>
    <tr>
        <td>Menú</td>
        <td>Sí</td>
        <td rowspan="24" style="vertical-align: middle;">Consulte <a href="#button-mapping">asignación de botones</a></td>
    </tr>
    <tr>
        <td>Ver</td>
        <td>Sí</td>
    </tr>
    <tr>
        <td>Accept</td>
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
        <td>RePág</td>
        <td>No</td>
    </tr>
    <tr>
        <td>AvPág</td>
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

Para obtener más información sobre los controladores de navegación de la interfaz de usuario y los comandos anteriores, vea [controlador de navegación de IU](https://docs.microsoft.com/windows/uwp/gaming/ui-navigation-controller).

## <a name="keys"></a>Teclas

En las siguientes secciones se explica el contenido de cada una de las subclaves en las claves de **controlador de juegos**, **RacingWheel**, **ArcadeStick**, **FlightStick**y **UINavigation** .

### <a name="button-mapping"></a>Asignación de botones

En la tabla siguiente se enumeran los valores que se necesitan para asignar un botón. Por ejemplo, si se presiona **DPadUp** en el dispositivo de juego, la asignación de **DPadUp** debe contener el valor **ButtonIndex** (el**origen** es **Button**). Si **DPadUp** debe asignarse desde una posición del conmutador, la asignación de **DPadUp** debe contener los valores **SwitchIndex** y **SwitchPosition** (el**origen** es **Switch**).

<table>
    <tr>
        <th>Source</th>
        <th>Nombre del valor</th>
        <th>Tipo de valor</th>
        <th>¿Necesario?</th>
        <th>Información del valor</th>
    </tr>
    <tr>
        <td>Botón</td>
        <td>ButtonIndex</td>
        <td>DWORD</td>
        <td>Sí</td>
        <td>Índice en la matriz de botones de <b>RawGameController</b> .</td>
    </tr>
    <tr>
        <td rowspan="4" style="vertical-align: middle;">Eje</td>
        <td>AxisIndex</td>
        <td>DWORD</td>
        <td>Sí</td>
        <td>Índice de la matriz del eje <b>RawGameController</b> .</td>
    </tr>
    <tr>
        <td>Invertir</td>
        <td>DWORD</td>
        <td>No</td>
        <td>Indica que el valor del eje debe invertirse antes de que se apliquen los factores de <b>porcentaje de umbral</b> y <b>DebouncePercent</b> .</td>
    </tr>
    <tr>
        <td>ThresholdPercent</td>
        <td>DWORD</td>
        <td>Sí</td>
        <td>Indica la posición del eje en la que el valor del botón asignado realiza la transición entre los Estados presionado y liberado. El intervalo válido de valores es de 0 a 100. Se considera que el botón está presionado si el valor del eje es mayor o igual que este valor.</td>
    </tr>
    <tr>
        <td>DebouncePercent</td>
        <td>DWORD</td>
        <td>Sí</td>
        <td>
            <p>Define el tamaño de una ventana alrededor del valor de <b>ThresholdPercent</b> , que se usa para desbotar el estado del botón indicado. El intervalo válido de valores es de 0 a 100. Las transiciones de estado del botón solo se pueden producir cuando el valor del eje cruza los límites superior e inferior de la ventana de desbote. Por ejemplo, un <b>ThresholdPercent</b> de 50 y <b>DebouncePercent</b> de 10 produce límites de desbote en el 45% y el 55% de los valores del eje de intervalo completo. El botón no puede pasar al estado presionado hasta que el valor del eje alcanza el 55% o superior, y no puede volver al estado liberado hasta que el valor del eje alcanza el 45% o inferior.</p>
            <p>Los límites de la ventana de desbote calculada están fijados entre 0% y 100%. Por ejemplo, un umbral de un 5% y una ventana de Debounce del 20% daría como resultado los límites de la ventana de desbote en el 0% y el 15%. El estado del botón para los valores de eje de 0% y 100% siempre se indica como liberado y presionado, respectivamente, independientemente de los valores de umbral y Debounce.</p>
        </td>
    </tr>
    <tr>
        <td rowspan="3" style="vertical-align: middle;">Modificador</td>
        <td>SwitchIndex</td>
        <td>DWORD</td>
        <td>Sí</td>
        <td>Índice en la matriz del modificador <b>RawGameController</b> .</td>
    </tr>
    <tr>
        <td>SwitchPosition</td>
        <td>REG_SZ</td>
        <td>Sí</td>
        <td>
            <p>Indica la posición del modificador que hará que el botón asignado informe de que se presiona. Los valores de posición pueden ser una de estas cadenas:</p>
            <ul>
                <li>Arriba</li>
                <li>UpRight</li>
                <li>Right</li>
                <li>DownRight</li>
                <li>Bajar</li>
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
        <td>Indica que las posiciones adyacentes del conmutador también harán que el botón asignado informe de que se presiona.</td>
    </tr>
</table>

### <a name="axis-mapping"></a>Asignación de eje

En la tabla siguiente se enumeran los valores que se necesitan para asignar un eje:

<table>
    <tr>
        <th>Source</th>
        <th>Nombre del valor</th>
        <th>Tipo de valor</th>
        <th>¿Necesario?</th>
        <th>Información del valor</th>
    </tr>
    <tr>
        <td rowspan="2" style="vertical-align: middle;">Botón</td>
        <td>MaxValueButtonIndex</td>
        <td>DWORD</td>
        <td>Sí</td>
        <td>
            <p>Índice en la matriz de botones <b>RawGameController</b> que se convierte en el valor del eje unidireccional asignado.</p>
            <table>
                <tr>
                    <th>MaxButton</th>
                    <th>AxisValue</th>
                </tr>
                <tr>
                    <td>FALSE</td>
                    <td>0,0</td>
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
            <p>Indica que el eje asignado es bidireccional. Los valores de <b>MaxButton</b> y <b>MinButton</b> se combinan en un único eje bidireccional, como se muestra a continuación.</p>
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
                    <td>false</td>
                    <td>true</td>
                    <td>1.0</td>
                </tr>
                <tr>
                    <td>TRUE</td>
                    <td>FALSE</td>
                    <td>0,0</td>
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
        <td>Índice de la matriz del eje <b>RawGameController</b> .</td>
    </tr>
    <tr>
        <td>Invertir</td>
        <td>DWORD</td>
        <td>No</td>
        <td>Indica que se debe invertir el valor del eje asignado antes de que se devuelva.</td>
    </tr>
    <tr>
        <td rowspan="3" style="vertical-align: middle;">Modificador</td>
        <td>SwitchIndex</td>
        <td>DWORD</td>
        <td>Sí</td>
        <td>Índice en la matriz del modificador <b>RawGameController</b> .
    </tr>
    <tr>
        <td>MaxValueSwitchPosition</td>
        <td>REG_SZ</td>
        <td>Sí</td>
        <td>
            <p>Una de las cadenas siguientes:</p>
            <ul>
                <li>Arriba</li>
                <li>UpRight</li>
                <li>Right</li>
                <li>DownRight</li>
                <li>Bajar</li>
                <li>DownLeft</li>
                <li>Left</li>
                <li>UpLeft</li>
            </ul>
            <p>Indica la posición del modificador que hace que el valor del eje asignado se notifique como 1,0. La dirección opuesta de <b>MaxValueSwitchPosition</b> se trata como 0,0. Por ejemplo, si <b>MaxValueSwitchPosition</b> está <b>activo</b>, la traducción del valor del eje se muestra a continuación:</p>
            <table>
                <tr>
                    <th>Posición del conmutador</th>
                    <th>AxisValue</th>
                </tr>
                <tr>
                    <td>Arriba</td>
                    <td>1.0</td>
                </tr>
                <tr>
                    <td>Center</td>
                    <td>0.5</td>
                </tr>
                <tr>
                    <td>Bajar</td>
                    <td>0,0</td>
                </tr>
            </table>
        </td>
    </tr>
    <tr>
        <td>IncludeAdjacent</td>
        <td>DWORD</td>
        <td>No</td>
        <td>
            <p>Indica que las posiciones adyacentes del conmutador también harán que el eje asignado informe de 1,0. En el ejemplo anterior, si se establece <b>IncludeAdjacent</b> , la traducción del eje se realiza de la siguiente manera:</p>
            <table>
                <tr>
                    <th>Posición del conmutador</th>
                    <th>AxisValue</th>
                </tr>
                <tr>
                    <td>Arriba</td>
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
                    <td>Bajar</td>
                    <td>0,0</td>
                </tr>
                <tr>
                    <td>DownRight</td>
                    <td>0,0</td>
                </tr>
                <tr>
                    <td>DownLeft</td>
                    <td>0,0</td>
                </tr>
            </table>
        </td>
    </tr>
</table>

### <a name="switch-mapping"></a>Asignación de conmutadores

Las posiciones de los conmutadores se pueden asignar desde un conjunto de botones de la matriz de botones de **RawGameController** o desde un índice de la matriz de modificadores. Las posiciones de los conmutadores no se pueden asignar desde ejes.

<table>
    <tr>
        <th>Source</th>
        <th>Nombre del valor</th>
        <th>Tipo de valor</th>
        <th>Información del valor</th>
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
        <td><b>TwoWay</b>, <b>FourWay</b>o <b>EightWay</b>
    </tr>
    <tr>
        <td>UpButtonIndex</td>
        <td>DWORD</td>
        <td rowspan="8" style="vertical-align: middle;">Consulte <a href="#buttonindex-values">* valores de ButtonIndex</a></td>
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
        <td><b>TwoWay</b>, <b>FourWay</b>o <b>EightWay</b></td>
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
        <td rowspan="2" style="vertical-align: middle;">Indica el tamaño de la zona muerta en torno a la posición central de los ejes.</td>
    </tr>
    <tr>
        <td>YDeadZonePercent</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>XDebouncePercent</td>
        <td>DWORD</td>
        <td rowspan="2" style="vertical-align: middle;">Defina el tamaño de las ventanas en torno a los límites superior e inferior de la zona muerta, que se usan para desbotar el estado del conmutador indicado.</td>
    </tr>
    <tr>
        <td>YDebouncePercent</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td>XInvert</td>
        <td>DWORD</td>
        <td rowspan="2" style="vertical-align: middle;">Indica que los valores de eje correspondientes deben invertirse antes de que se apliquen los cálculos de las ventanas de zona muerta y de desbote.</td>
    </tr>
    <tr>
        <td>YInvert</td>
        <td>DWORD</td>
    </tr>
    <tr>
        <td rowspan="3" style="vertical-align: middle;">Modificador</td>
        <td>SwitchIndex</td>
        <td>DWORD</td>
        <td>Índice en la matriz del modificador <b>RawGameController</b> .
    </tr>
    <tr>
        <td>Invertir</td>
        <td>DWORD</td>
        <td>Indica que el modificador informa de sus posiciones en un orden en el sentido contrario a las agujas del reloj, en lugar del orden en el sentido de las agujas del reloj.</td>
    </tr>
    <tr>
        <td>PositionBias</td>
        <td>DWORD</td>
        <td>
            <p>Desplaza el punto inicial de cómo se informan las posiciones en la cantidad especificada. <b>PositionBias</b> siempre se cuenta en el sentido de las agujas del reloj desde el punto inicial original y se aplica antes de que se invierta el orden de los valores.</p>
            <p>Por ejemplo, un modificador que indica posiciones que se inician con <b>DownRight</b> en el orden en el sentido contrario a las agujas del reloj se puede normalizar estableciendo la marca de <b>inversión</b> y especificando un valor de <b>PositionBias</b> de 5:</p>
            <table>
                <tr>
                    <th>Posición</th>
                    <th>Valor indicado</th>
                    <th>Después de PositionBias e invertir marcas</th>
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
                    <td>Arriba</td>
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
                    <td>Bajar</td>
                    <td>7</td>
                    <td>4</td>
                </tr>
            </table>
    </tr>
</table>

#### <a name="buttonindex-values"></a>* Valores de ButtonIndex

\*ButtonIndex valores de índice en la matriz de botones del **RawGameController**:

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

Se trata de valores de asignación estáticos para diferentes tipos de asignación.

<table>
    <tr>
        <th>Asignación</th>
        <th>Nombre del valor</th>
        <th>Tipo de valor</th>
        <th>Información del valor</th>
    </tr>
    <tr>
        <td>RacingWheel</td>
        <td>MaxWheelAngle</td>
        <td>DWORD</td>
        <td>Indica el ángulo de rueda física máximo admitido por la rueda en una sola dirección. Por ejemplo, una rueda con un posible giro de-90 grados a 90 grados especificaría 90.</td>
    </tr>
</table>

## <a name="labels"></a>Etiquetas

Las etiquetas deben estar presentes en la clave **etiquetas** bajo la raíz del dispositivo. Las **etiquetas** pueden tener 3 subclaves: **botones**, **ejes**y **Modificadores**.

### <a name="button-labels"></a>Etiquetas de los botones

La tecla **botones** asigna a una cadena cada una de las posiciones de los botones de la matriz de botones de **RawGameController**. Cada cadena se asigna internamente al valor de enumeración [GameControllerButtonLabel](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamecontrollerbuttonlabel) correspondiente. Por ejemplo, si un controlador de juegos tiene diez botones y el orden en el que el **RawGameController** analiza los botones y los presenta en el informe de botones es como el siguiente:

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

Las etiquetas deben aparecer en este orden bajo la tecla **botones** :

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
        <td>Carta a</td>
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
        <td>Carta</td>
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

### <a name="axis-labels"></a>Etiquetas de eje

La clave de los **ejes** asignará cada una de las posiciones del eje de la matriz de ejes de **RawGameController**a una de las etiquetas enumeradas en la [enumeración GameControllerButtonLabel](https://docs.microsoft.com/uwp/api/windows.gaming.input.gamecontrollerbuttonlabel) , al igual que las etiquetas de botón. Vea el ejemplo en [etiquetas de botón](#button-labels).

### <a name="switch-labels"></a>Cambiar etiquetas

Los **Modificadores** de las asignaciones de teclas cambian de posición a etiquetas. Los valores siguen esta Convención de nomenclatura: para etiquetar una posición de un modificador, cuyo índice es *x* en la matriz de modificadores de **RawGameController**, agregue estos valores en la subclave **switches** :

* SwitchxUp
* SwitchxUpRight
* SwitchxRight
* SwitchxDownRight
* SwitchxDown
* SwitchxDownLeft
* SwitchxUpLeft
* SwitchxLeft

En la tabla siguiente se muestra un ejemplo de un conjunto de etiquetas para las posiciones de los conmutadores de un conmutador de 4 vías que se muestra en el índice 0 de **RawGameController**:

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

## <a name="example-registry-file"></a>Archivo de registro de ejemplo

Para mostrar cómo se reúnen todas estas asignaciones y valores, este es un archivo de registro de ejemplo para un **RacingWheel**genérico:

```text
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

## <a name="see-also"></a>Vea también

* [Espacio de nombres Windows. Gaming. Input](/uwp/api/windows.gaming.input)
* [Espacio de nombres Windows. Gaming. Input. Custom](/uwp/api/windows.gaming.input.custom)
* [Archivos INF](/windows-hardware/drivers/install/inf-files)