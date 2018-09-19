---
author: muhsinking
title: Acceder a sensores y dispositivos desde una tarea en segundo plano
description: DeviceUseTrigger permite que aplicación universal de Windows acceda a dispositivos periféricos y sensores en segundo plano, incluso cuando la aplicación en primer plano esté suspendida.
ms.assetid: B540200D-9FF2-49AF-A224-50877705156B
ms.author: mukin
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, tarea en segundo plano
ms.localizationpriority: medium
ms.openlocfilehash: 99f853da53302d4080bfa9462da0ec524e8d2064
ms.sourcegitcommit: 68fcac3288d5698a13dbcbd57f51b30592f24860
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2018
ms.locfileid: "4060335"
---
# <a name="access-sensors-and-devices-from-a-background-task"></a>Acceder a sensores y dispositivos desde una tarea en segundo plano




[**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) permite que la aplicación universal de Windows acceda a dispositivos periféricos y sensores en segundo plano, aunque la aplicación en primer plano esté suspendida. Por ejemplo, en función de dónde se ejecute la aplicación, podrías usar una tarea en segundo plano para sincronizar datos con dispositivos o sensores de monitores. Para ahorrar batería y garantizar el consentimiento apropiado por parte del usuario, el uso de [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) está sujeto a las directivas descritas en este tema.

Para acceder a dispositivos periféricos o sensores en segundo plano, crea una tarea en segundo plano que use [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337). Para ver un ejemplo de esto en un equipo, consulta la [muestra de dispositivo USB personalizado](http://go.microsoft.com/fwlink/p/?LinkId=301975 ). Para ver un ejemplo en un teléfono, consulta la [muestra de sensores en segundo plano](http://go.microsoft.com/fwlink/p/?LinkId=393307).

> [!Important]
> **DeviceUseTrigger** no se puede usar con tareas en segundo plano dentro del proceso. La información de este tema solo se aplica a las tareas en segundo plano que se ejecutan fuera del proceso.

## <a name="device-background-task-overview"></a>Introducción a las tareas en segundo plano del dispositivo

Cuando la aplicación deje de ser visible para el usuario, Windows la suspenderá o finalizará para recuperar recursos de CPU y memoria. Esto permite ejecutar otras aplicaciones en primer plano y reduce el consumo de batería. Cuando suceda esto, sin ayuda de una tarea en segundo plano, todos los eventos de datos en curso se perderán. Windows proporciona el desencadenador de tarea en segundo plano, [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337), para que la aplicación pueda realizar operaciones de sincronización y supervisión de larga duración en dispositivos y sensores de manera segura en segundo plano, aunque la aplicación esté suspendida. Para obtener más información sobre el ciclo de vida de la aplicación, consulta [Iniciar, reanudar y tareas en segundo plano](index.md). Para obtener más información sobre las tareas en segundo plano, consulta [Dar soporte a tu aplicación mediante tareas en segundo plano](support-your-app-with-background-tasks.md).

**Nota**: En una aplicación universal de Windows, para sincronizar un dispositivo en segundo plano, el usuario debe haber aprobado la sincronización en segundo plano de la aplicación. El dispositivo también debe estar conectado o emparejado con el equipo, con E/S activa, y se le permite un máximo de 10minutos de actividad en segundo plano. Más adelante en este tema se proporcionan más detalles sobre la aplicación de directivas.

### <a name="limitation-critical-device-operations"></a>Limitación: operaciones críticas del dispositivo

Algunas operaciones críticas del dispositivo, como las actualizaciones del firmware que se ejecutan durante mucho tiempo, no se pueden realizar con [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337). Esas operaciones solo se pueden realizar en el equipo y solo las puede realizar una aplicación privilegiada que use [**DeviceServicingTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297315). Una *aplicación privilegiada* es una aplicación que ha recibido la autorización del fabricante del dispositivo para realizar esas operaciones. Los metadatos del dispositivo se usan para especificar qué aplicación, si es el caso, se ha designado como aplicación privilegiada para un dispositivo. Para obtener más información, consulta [Device sync and update for Microsoft Store device apps](http://go.microsoft.com/fwlink/p/?LinkId=306619) (Sincronización y actualización de dispositivos para aplicaciones para dispositivo de Microsoft Store).

## <a name="protocolsapis-supported-in-a-deviceusetrigger-background-task"></a>Protocolos o API admitidos en una tarea en segundo plano DeviceUseTrigger

Las tareas en segundo plano que usan [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) permiten que tu aplicación se comunique a través de un gran número de protocolos o API, cuando muchos de ellos no se admiten en tareas en segundo plano desencadenadas por el sistema. Los siguientes componentes son compatibles con una aplicación universal de Windows.

| Protocolo         | DeviceUseTrigger en una aplicación universal de Windows                                                                                                                                                    |
|------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| USB              | ![este protocolo es compatible.](images/ap-tools.png)                                                                                                                                            |
| HID              | ![este protocolo es compatible.](images/ap-tools.png)                                                                                                                                            |
| Bluetooth RFCOMM | ![este protocolo es compatible.](images/ap-tools.png)                                                                                                                                            |
| GATT de Bluetooth   | ![este protocolo es compatible.](images/ap-tools.png)                                                                                                                                            |
| MTP              | ![este protocolo es compatible.](images/ap-tools.png)                                                                                                                                            |
| Red cableada    | ![este protocolo es compatible.](images/ap-tools.png)                                                                                                                                            |
| Red Wi-Fi    | ![este protocolo es compatible.](images/ap-tools.png)                                                                                                                                            |
| IDeviceIOControl | ![deviceservicingtrigger es compatible con ideviceiocontrol](images/ap-tools.png)                                                                                                                       |
| API de sensores      | ![deviceservicingtrigger admite API de sensores universales](images/ap-tools.png) (limitado a los sensores de la [familia de dispositivos universal](https://msdn.microsoft.com/library/windows/apps/dn894631)) |

## <a name="registering-background-tasks-in-the-app-package-manifest"></a>Registrar tareas en segundo plano en el manifiesto del paquete de la aplicación

La aplicación realizará operaciones de sincronización y actualización en código que se ejecuta como parte de una tarea en segundo plano. Este código está insertado en una clase de Windows Runtime que implementa [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794) (o en una página JavaScript dedicada para las aplicaciones JavaScript). Para usar una tarea en segundo plano de [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337), la aplicación debe declararla en el archivo de manifiesto de la aplicación de una aplicación en primer plano, como en el caso de las tareas en segundo plano desencadenadas por el sistema.

En este ejemplo de un archivo de manifiesto de paquete de la aplicación, **DeviceLibrary.SyncContent** es el punto de entrada de una tarea en segundo plano que usa [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337).

```xml
<Extensions>
  <Extension Category="windows.backgroundTasks" EntryPoint="DeviceLibrary.SyncContent">
    <BackgroundTasks>
      <m2:Task Type="deviceUse" />
    </BackgroundTasks>
  </Extension>
</Extensions>
```

## <a name="introduction-to-using-deviceusetrigger"></a>Introducción al uso de DeviceUseTrigger

Para usar [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337), sigue estos pasos básicos. Para obtener más información sobre las tareas en segundo plano, consulta [Dar soporte a tu aplicación mediante tareas en segundo plano](support-your-app-with-background-tasks.md).

1.  Tu aplicación registra su tarea en segundo plano en el manifiesto de la aplicación e inserta el código de la tarea en segundo plano en una clase de Windows en tiempo de ejecución que implementa IBackgroundTask o en una página JavaScript dedicada para aplicaciones JavaScript.
2.  Cuando la aplicación se inicie, creará y configurará un objeto desencadenador del tipo [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) y almacenará la instancia del desencadenador para usarla en el futuro.
3.  La aplicación comprueba si la tarea en segundo plano se ha registrado anteriormente y, en caso negativo, la registra con el desencadenador. Ten en cuenta que no se permite que tu aplicación establezca condiciones en la tarea asociada con este desencadenador.
4.  Cuando la aplicación necesita desencadenar la tarea en segundo plano, primero debe llamar a [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) para comprobar si puede solicitar una tarea en segundo plano.
5.  Si la aplicación puede solicitar la tarea en segundo plano, llama al método de activación [**RequestAsync**](https://msdn.microsoft.com/library/windows/apps/dn297341) en el objeto desencadenador del dispositivo.
6.  La tarea en segundo plano no está limitada como otras tareas en segundo plano del sistema (no hay cuota de tiempo de la CPU), pero se ejecutará con prioridad reducida para que las aplicaciones en primer plano mantengan su capacidad de respuesta.
7.  A continuación, Windows validará, en función del tipo de desencadenador, que se hayan cumplido las directivas necesarias, incluida la solicitud de consentimiento por parte del usuario para la operación antes de iniciar la tarea en segundo plano.
8.  Windows supervisa las condiciones del sistema y el tiempo de ejecución de la tarea y, si es necesario, cancela la tarea si ya no se cumplen las condiciones necesarias.
9.  Cuando las tareas en segundo plano informan de progreso o finalización, la aplicación recibirá estos eventos por medio de eventos de progreso y completados en la tarea registrada.

**Importante**  
Ten en cuenta estos puntos importantes al usar [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337):

-   La capacidad de desencadenar tareas en segundo plano mediante programación que usan [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) se incluyó por primera vez en Windows8.1 y WindowsPhone8.1.

-   Windows aplica ciertas directivas para garantizar el consentimiento por parte del usuario al actualizar dispositivos periféricos en el equipo.

-   Se aplican directivas adicionales para ahorrar batería al sincronizar y actualizar dispositivos periféricos.

-   Windows podría cancelar las tareas en segundo plano que usan [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) cuando ya no se cumplan ciertos requisitos de directivas, incluida una cantidad máxima de tiempo en segundo plano (tiempo de reloj). Es importante tener en cuenta dichos requisitos de directivas al usar estas tareas en segundo plano para interactuar con el dispositivo periférico.

**Sugerencia**: Para ver cómo funcionan estas tareas en segundo plano, descarga una muestra. Para ver un ejemplo de esto en un equipo, consulta la [muestra de dispositivo USB personalizado](http://go.microsoft.com/fwlink/p/?LinkId=301975 ). Para ver un ejemplo en un teléfono, consulta la [muestra de sensores en segundo plano](http://go.microsoft.com/fwlink/p/?LinkId=393307).
 
## <a name="frequency-and-foreground-restrictions"></a>Restricciones de frecuencia y primer plano

No hay límites en cuanto a la frecuencia con la que tu aplicación puede iniciar operaciones, pero la aplicación solo puede ejecutar una operación de tarea en segundo plano de [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) a la vez (esto no afecta a otros tipos de tareas en segundo plano) y solo puede iniciar una tarea en segundo plano mientras la aplicación se encuentre en primer plano. Si la aplicación no está en primer plano, no puede iniciar una tarea en segundo plano con **DeviceUseTrigger**. La aplicación no puede iniciar una segunda tarea en segundo plano de **DeviceUseTrigger** antes de que se haya completado la primera.

## <a name="device-restrictions"></a>Restricciones del dispositivo

Aunque cada aplicación está limitada para registrar y ejecutar solo una tarea en segundo plano de [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337), el dispositivo (en el que se ejecuta la aplicación) puede permitir que varias aplicaciones se registren y ejecuten tareas en segundo plano de **DeviceUseTrigger**. En función del dispositivo, puede haber límites en el número total de tareas en segundo plano de **DeviceUseTrigger** de todas las aplicaciones. Esto ayuda a ahorrar batería en dispositivos con restricciones de recursos. Consulta la tabla siguiente para obtener más información.

Desde una sola tarea en segundo plano de [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337), la aplicación puede acceder a una cantidad ilimitada de dispositivos periféricos o sensores (limitada solo por las API y los protocolos compatibles enumerados en la tabla anterior).

## <a name="background-task-policies"></a>Directivas de tareas en segundo plano

Windows aplica directivas cuando la aplicación usa una tarea en segundo plano de [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337). Si no se cumplen estas directivas, es posible que la tarea en segundo plano se cancele. Es importante tener en cuenta estos requisitos de directivas al usar este tipo de tarea en segundo plano para interactuar con dispositivos o sensores.

### <a name="task-initiation-policies"></a>Directivas de inicio de tareas

En la siguiente tabla se indican las directivas de inicio de tareas que se aplican a una aplicación universal de Windows.

| Directiva | DeviceUseTrigger en una aplicación universal de Windows |
|--------|---------------------------------------------|
| La tarea se encuentra en primer plano al desencadenar la tarea en segundo plano. | ![se aplica la directiva](images/ap-tools.png) |
| El dispositivo está conectado al sistema (o dentro del alcance en el caso de un dispositivo inalámbrico). | ![se aplica la directiva](images/ap-tools.png) |
| La aplicación puede acceder al dispositivo con las API de periféricos de dispositivos compatibles (las API de Windows en tiempo de ejecución para USB, HID, Bluetooth, sensores, etc.). Si la aplicación no puede acceder al dispositivo o sensor, se deniega el acceso a la tarea en segundo plano. | ![se aplica la directiva](images/ap-tools.png) |
| El punto de entrada de la tarea en segundo plano proporcionado por la aplicación se registra en el manifiesto del paquete de la aplicación. | ![se aplica la directiva](images/ap-tools.png) |
| Se ejecuta una sola tarea en segundo plano de [DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/dn297337) por aplicación. | ![se aplica la directiva](images/ap-tools.png) |
| Todavía no se ha alcanzado la cantidad máxima de tareas en segundo plano de [DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/dn297337) en el dispositivo (en el que se ejecuta la aplicación). | **Familia de dispositivos de escritorio**: se puede registrar y ejecutar en paralelo una cantidad ilimitada de tareas. **Familia de dispositivos móviles**: 1 tarea en un dispositivo de 512MB; de lo contrario, pueden registrarse 2 tareas y ejecutarse en paralelo. |
| La cantidad máxima de dispositivos periféricos o sensores a los que puede acceder la aplicación desde una sola tarea en segundo plano de [DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/dn297337) al usar las API o los protocolos compatibles. | Ilimitada |
| La tarea en segundo plano consume 400ms de tiempo de CPU (dando por hecho una CPU a 1GHz) cada minuto cuando la pantalla está bloqueada o cada 5 minutos cuando la pantalla no está bloqueada. Si no se cumple esta directiva, se puede cancelar la tarea. | ![se aplica la directiva](images/ap-tools.png) |

### <a name="runtime-policy-checks"></a>Comprobaciones de directivas en tiempo de ejecución

Windows aplica los siguientes requisitos de directivas en tiempo de ejecución mientras la tarea se ejecuta en segundo plano. Si cualquiera de los requisitos en tiempo de ejecución deja de cumplirse, Windows cancelará la tarea en segundo plano del dispositivo.

En esta tabla se indican las directivas en tiempo de ejecución que se aplican a una aplicación universal de Windows.

| Comprobación de directivas | DeviceUseTrigger en una aplicación universal de Windows |
|--------------|:-------------------------------------------:|
| El dispositivo está conectado al sistema (o dentro del alcance en el caso de un dispositivo inalámbrico). | ![La comprobación de directiva se aplica.](images/ap-tools.png) |
| La tarea realiza E/S regular en el dispositivo (1 E/S cada 5 segundos). | ![La comprobación de directiva se aplica.](images/ap-tools.png) |
| La aplicación no ha cancelado la tarea. | ![La comprobación de directiva se aplica.](images/ap-tools.png) |
| Límite de tiempo de reloj: el total de tiempo durante el que la tarea de la aplicación se puede ejecutar en segundo plano. | **Familia de dispositivos de escritorio**: 10 minutos. **Familia de dispositivos móviles**: ningún límite de tiempo. Para conservar los recursos, no pueden ejecutarse más de 1 o 2 tareas al mismo tiempo. |
| La aplicación no se ha cerrado. | ![La comprobación de directiva se aplica.](images/ap-tools.png) |

## <a name="best-practices"></a>Procedimientos recomendados

A continuación se indican los procedimientos recomendados para las aplicaciones que usan las tareas en segundo plano de [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337).

### <a name="programming-a-background-task"></a>Programar una tarea en segundo plano

Al usar la tarea en segundo plano de [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) desde la aplicación, se garantiza que las operaciones de sincronización o supervisión iniciadas desde la aplicación en primer plano sigan ejecutándose en segundo plano si los usuarios cambian de aplicación y Windows suspende la aplicación en primer plano. Te recomendamos adoptar el siguiente modelo general para registrar, desencadenar tareas en segundo plano y anular su registro:

1.  Llama a [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) para comprobar si la aplicación puede solicitar una tarea en segundo plano. Se debe hacer antes de registrar una tarea en segundo plano.

2.  Registra la tarea en segundo plano antes de solicitar el desencadenador.

3.  Conecta los controladores de eventos de progreso y finalización al desencadenador. Cuando la aplicación vuelva de una suspensión, Windows le proporcionará los eventos de progreso o finalización en cola que se pueden usar para determinar el estado de las tareas en segundo plano.

4.  Cierra los objetos de dispositivo o sensor abiertos al desencadenar la tarea en segundo plano de [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337) para que esta los pueda abrir y usar.

5.  Registra el desencadenador.

6.  Analiza detenidamente las consecuencias para la batería del acceso a un dispositivo o sensor desde una tarea en segundo plano. Por ejemplo, si el intervalo de informes de un sensor se ejecuta con demasiada frecuencia, la tarea se podría ejecutar con tal frecuencia que agotaría rápidamente la batería del teléfono.

7.  Cuando se complete la tarea en segundo plano, anula su registro.

8.  Regístrate en los eventos de cancelación de la clase de tarea de segundo plano. El registro en los eventos de cancelación permitirá al código de la tarea en segundo plano detener de manera limpia la tarea en segundo plano en ejecución cuando la cancele Windows o la aplicación en primer plano.

9.  Al salir de una aplicación (no al suspenderla), anula el registro y cancela las tareas en ejecución si la aplicación ya no las necesita. En sistemas con restricción de recursos, como en teléfonos con poca memoria, esto permitirá que otras aplicaciones usen una tarea en segundo plano de [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337).

    -   Cuando se cierre la aplicación, anula el registro de las tareas en ejecución y cancélalas.

    -   Cuando se cierre la aplicación, las tareas en segundo plano se cancelarán y los controladores de eventos se desconectarán de las tareas en segundo plano existentes. Esto te impedirá determinar el estado de las tareas en segundo plano. Si anulas el registro de la tarea en segundo plano y la cancelas, el código de cancelación podrá detener de manera limpia las tareas en segundo plano.

### <a name="cancelling-a-background-task"></a>Cancelar una tarea en segundo plano

Para cancelar una tarea que se ejecuta en segundo plano desde la aplicación en primer plano, usa el método Unregister en el objeto [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786) que usas en la aplicación para registrar la tarea en segundo plano de [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/dn297337). Al anular el registro de la tarea en segundo plano con el método [**Unregister**](https://msdn.microsoft.com/library/windows/apps/br229869) en **BackgroundTaskRegistration**, la infraestructura de tareas en segundo plano cancelará dicha tarea en segundo plano.

Además, el método [**Unregister**](https://msdn.microsoft.com/library/windows/apps/br229869) usa un valor booleano True o False para indicar si las instancias en ejecución de la tarea en segundo plano se deben cancelar sin permitir que finalicen. Para obtener más información, consulta la referencia de API para **Unregister**.

Además del método [**Unregister**](https://msdn.microsoft.com/library/windows/apps/br229869), la aplicación también debe llamar a [**BackgroundTaskDeferral.Complete**](https://msdn.microsoft.com/library/windows/apps/hh700504). Esto notifica al sistema que ha finalizado la operación asincrónica asociada a una tarea en segundo plano.
