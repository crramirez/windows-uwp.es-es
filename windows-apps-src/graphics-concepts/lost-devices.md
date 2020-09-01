---
title: Dispositivos perdidos
description: Un dispositivo Direct3D puede estar en un estado operativo o en un estado perdido.
ms.assetid: 1639CC02-8000-4208-AA95-91C1F0A3B08D
keywords:
- Dispositivos perdidos
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d7f2c06564e0477fcec67418cae739e2fae123d7
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158859"
---
# <a name="lost-devices"></a>Dispositivos perdidos


Un dispositivo Direct3D puede estar en un estado operativo o en un estado perdido. El estado *operativo* es el estado normal del dispositivo en el que se ejecuta el dispositivo y presenta todas las representaciones según lo previsto. El dispositivo realiza una transición al estado *perdido* cuando un evento, como la pérdida del foco de teclado en una aplicación de pantalla completa, hace que la representación sea imposible. El estado perdido se caracteriza por el error silencioso de todas las operaciones de representación, lo que significa que los métodos de representación pueden devolver códigos correctos aunque se produzca un error en las operaciones de representación.

Por diseño, no se especifica el conjunto completo de escenarios que pueden provocar que se pierda un dispositivo. Algunos ejemplos típicos incluyen la pérdida del foco, como cuando el usuario presiona ALT + TAB o cuando se inicializa un cuadro de diálogo de sistema. Los dispositivos también se pueden perder debido a un evento de administración de energía o cuando otra aplicación asume una operación de pantalla completa. Además, si se produce un error al restablecer un dispositivo, el dispositivo pierde el estado.

Se garantiza que todos los métodos que se derivan de [**IUnknown**](/windows/desktop/api/unknwn/nn-unknwn-iunknown) funcionan después de que se pierda un dispositivo. Después de la pérdida del dispositivo, cada función tiene generalmente las tres opciones siguientes:

-   Error con un "dispositivo perdido": Esto significa que la aplicación debe reconocer que el dispositivo se perdió, por lo que la aplicación identifica que algo no está ocurriendo según lo esperado.
-   Un error en modo silencioso, \_ que devuelve un valor correcto o cualquier otro código de retorno; si una función produce un error en modo silencioso, la aplicación no suele distinguir entre el resultado de "Success" y un "error silencioso".
-   Devuelve un código de retorno.

## <a name="span-idresponding_to_a_lost_devicespanspan-idresponding_to_a_lost_devicespanspan-idresponding_to_a_lost_devicespanresponding-to-a-lost-device"></a><span id="Responding_to_a_Lost_Device"></span><span id="responding_to_a_lost_device"></span><span id="RESPONDING_TO_A_LOST_DEVICE"></span>Responder a un dispositivo perdido


Un dispositivo perdido debe volver a crear los recursos (incluidos los recursos de memoria de vídeo) una vez que se haya restablecido. Si se pierde un dispositivo, la aplicación consulta el dispositivo para ver si se puede restaurar al estado operativo. Si no es así, la aplicación espera hasta que el dispositivo se pueda restaurar.

Si el dispositivo se puede restaurar, la aplicación prepara el dispositivo destruyendo todos los recursos de memoria de vídeo y cualquier cadena de intercambio. Restablecer es el único procedimiento que tiene un efecto cuando se pierde un dispositivo y es la única manera en que una aplicación puede cambiar el dispositivo de un estado de pérdida a operativo. El restablecimiento producirá un error a menos que la aplicación libere todos los recursos asignados, incluidos los destinos de representación y las superficies de estarcido de profundidad.

En su mayor parte, las llamadas de alta frecuencia de Direct3D no devuelven ninguna información acerca de si el dispositivo se ha perdido. La aplicación puede seguir llamando a métodos de representación, sin recibir la notificación de un dispositivo perdido. Internamente, estas operaciones se descartan hasta que el dispositivo se restablece al estado operativo.

## <a name="span-idlocking_operationsspanspan-idlocking_operationsspanspan-idlocking_operationsspanlocking-operations"></a><span id="Locking_Operations"></span><span id="locking_operations"></span><span id="LOCKING_OPERATIONS"></span>Operaciones de bloqueo


De forma interna, Direct3D hace suficientes trabajos para asegurarse de que una operación de bloqueo se realizará correctamente después de que se pierda un dispositivo. Sin embargo, no se garantiza que los datos del recurso de memoria de vídeo serán precisos durante la operación de bloqueo. Se garantiza que no se devolverá ningún código de error. Esto permite escribir aplicaciones sin preocuparse por la pérdida de dispositivos durante una operación de bloqueo.

## <a name="span-idresourcesspanspan-idresourcesspanspan-idresourcesspanresources"></a><span id="Resources"></span><span id="resources"></span><span id="RESOURCES"></span>Recursos


Los recursos pueden consumir memoria de vídeo. Dado que un dispositivo perdido está desconectado de la memoria de vídeo propiedad del adaptador, no es posible garantizar la asignación de memoria de vídeo cuando se pierde el dispositivo. Como resultado, todos los métodos de creación de recursos se implementan correctamente, pero en realidad asignan solo la memoria ficticia del sistema. Dado que los recursos de memoria de vídeo deben destruirse antes de que se cambie el tamaño del dispositivo, no hay ningún problema de asignación excesiva de memoria de vídeo. Estas superficies ficticias permiten que las operaciones de bloqueo y copia parezcan funcionar con normalidad hasta que la aplicación detecte que el dispositivo se ha perdido.

Se debe liberar toda la memoria de vídeo antes de que un dispositivo se pueda restablecer desde un estado de pérdida a un estado operativo. Los demás datos de estado se destruyen automáticamente por la transición a un estado operativo.

Se recomienda desarrollar aplicaciones con una única ruta de acceso de código para responder a la pérdida del dispositivo. Es probable que esta ruta de acceso del código sea similar, si no idéntica, a la ruta de acceso del código tomada para inicializar el dispositivo en el inicio.

## <a name="span-idretrieved_dataspanspan-idretrieved_dataspanspan-idretrieved_dataspanretrieved-data"></a><span id="Retrieved_Data"></span><span id="retrieved_data"></span><span id="RETRIEVED_DATA"></span>Datos recuperados


Direct3D permite que las aplicaciones validen las texturas y los Estados de representación en la representación de un solo paso por parte del hardware.

Direct3D también permite que las aplicaciones copien imágenes generadas o escritas previamente desde recursos de memoria de vídeo a recursos de memoria del sistema no volátiles. Dado que las imágenes de origen de tales transferencias podrían perderse en cualquier momento, Direct3D permite que se produzcan errores en las operaciones de copia cuando se pierde el dispositivo.

Se pueden producir errores en las operaciones de copia, ya que no hay ninguna superficie principal cuando se pierde el dispositivo. También se puede producir un error en la creación de cadenas de intercambio porque no se puede crear un búfer de reserva cuando se pierde el dispositivo.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Dispositivos](devices.md)

 

 