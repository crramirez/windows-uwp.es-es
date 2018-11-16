---
title: Dispositivos perdidos
description: Un dispositivo de Direct3D puede estar en un estado operativo o perdido.
ms.assetid: 1639CC02-8000-4208-AA95-91C1F0A3B08D
keywords:
- Dispositivos perdidos
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 92dc9437dc2417b8f3da99df7cd6d6eb0c8edd1b
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "6851486"
---
# <a name="lost-devices"></a>Dispositivos perdidos


Un dispositivo de Direct3D puede estar en un estado operativo o perdido. El estado *operativo* es el estado normal del dispositivo, en el que el dispositivo ejecuta y presenta toda la representación según lo esperado. El dispositivo realiza una transición al estado *perdido* cuando un evento, como la pérdida de foco del teclado en una aplicación de pantalla completa, hace que la representación sea imposible. El estado perdido se caracteriza por el error silencioso de todas las operaciones de representación, lo que significa que los métodos de representación pueden devolver códigos correctos, aunque no se puedan realizar las operaciones de representación.

De manera predeterminada, no se especifica el conjunto completo de los escenarios que pueden provocar que un dispositivo pase al estado perdido. Algunos ejemplos típicos incluyen la pérdida del foco, como cuando el usuario presiona ALT + TAB o cuando se inicializa un cuadro de diálogo del sistema. Los dispositivos también pueden pasar al estado perdido debido a un evento de administración de energía o cuando otra aplicación usa la operación de pantalla completa. Además, cualquier error al restablecer un dispositivo lo deja en estado perdido.

Se garantiza que todos los métodos que derivan de [**IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509) funcionarán después de que el dispositivo esté en estado perdido. Por lo general, después de que el dispositivo esté en estado perdido, cada función tiene las tres opciones siguientes:

-   Error de "dispositivo perdido": significa que la aplicación debe reconocer que el dispositivo está en estado perdido, por lo que la aplicación identifica que algo no ocurre según lo esperado.
-   Error silencioso, que devuelve S\_OK o cualquier otro código de retorno: si una función produce un error silencioso, por lo general, la aplicación no puede distinguir entre un resultado "correcto" y un "error silencioso".
-   Devuelve un código de retorno.

## <a name="span-idrespondingtoalostdevicespanspan-idrespondingtoalostdevicespanspan-idrespondingtoalostdevicespanresponding-to-a-lost-device"></a><span id="Responding_to_a_Lost_Device"></span><span id="responding_to_a_lost_device"></span><span id="RESPONDING_TO_A_LOST_DEVICE"></span>Responder a un dispositivo perdido


Un dispositivo perdido debe recrear recursos (incluidos los recursos de memoria de vídeo) después de que se haya restablecido. Si se pierde un dispositivo, la aplicación consulta al dispositivo para ver si se puede restaurar el estado operativo. De lo contrario, la aplicación espera hasta que se pueda restaurar el dispositivo.

Si no se puede restaurar el dispositivo, la aplicación prepara el dispositivo mediante la destrucción de todos los recursos de memoria de vídeo y las cadenas de intercambio. Restablecer es lo único que tiene un efecto cuando se pierde un dispositivo y es la única forma de que una aplicación pueda cambiar el dispositivo de un estado perdido a operativo. Se producirá un error en el restablecimiento a menos que la aplicación libere todos los recursos asignados, incluidos los destinos de representación y las superficies de la galería de símbolos de profundidad.

Para la mayor parte, las llamadas de alta frecuencia de Direct3D no devuelven información sobre si el dispositivo está en estado perdido. La aplicación puede continuar llamando a los métodos de representación sin recibir ninguna notificación de dispositivo perdido. Internamente, estas operaciones se descartan hasta que se restablece el dispositivo al estado operativo.

## <a name="span-idlockingoperationsspanspan-idlockingoperationsspanspan-idlockingoperationsspanlocking-operations"></a><span id="Locking_Operations"></span><span id="locking_operations"></span><span id="LOCKING_OPERATIONS"></span>Operaciones de bloqueo


Internamente, Direct3D hace lo suficiente para garantizar que una operación de bloqueo será correcta después de que un dispositivo pase a estado perdido. Sin embargo, no se garantiza que los datos de los recursos de memoria de vídeo sean precisos durante la operación de bloqueo. Se garantiza que no se devolverá ningún código de error. Esto permite escribir en las aplicaciones sin tener que preocuparse por la pérdida del dispositivo durante una operación de bloqueo.

## <a name="span-idresourcesspanspan-idresourcesspanspan-idresourcesspanresources"></a><span id="Resources"></span><span id="resources"></span><span id="RESOURCES"></span>Recursos


Los recursos pueden consumir memoria de vídeo. Dado que un dispositivo perdido se desconecta de la memoria de vídeo del adaptador, no es posible garantizar la asignación de memoria de vídeo cuando se pierde el dispositivo. Como resultado, todos los métodos de creación de recursos se implementan para que sean correctos, pero, en realidad, solo asignan memoria del sistema ficticia. Dado que los recursos de memoria de vídeo deben destruirse antes de que el dispositivo cambie de tamaño, no hay ningún problema de exceso de asignación de memoria de vídeo. Estas superficies ficticias permiten bloquear y copiar las operaciones para parecer que funcionan con normalidad hasta que la aplicación detecta que el dispositivo está en estado perdido.

Se debe liberar toda la memoria de vídeo antes de que un dispositivo se pueda restablecer desde un estado perdido a un estado operativo. Otros datos de estado se destruyen automáticamente en la transición a un estado operativo.

Te animamos a desarrollar aplicaciones con una ruta de acceso de código única para responder a la pérdida del dispositivo. Esta ruta de acceso de código es probable que sea similar, si no idéntica, a la ruta de acceso de código necesaria para inicializar el dispositivo al iniciarse.

## <a name="span-idretrieveddataspanspan-idretrieveddataspanspan-idretrieveddataspanretrieved-data"></a><span id="Retrieved_Data"></span><span id="retrieved_data"></span><span id="RETRIEVED_DATA"></span>Datos recuperados


Direct3D permite que las aplicaciones validen los estados de textura y representación frente a la representación de paso único paso del hardware.

Direct3D también permite que las aplicaciones copien imágenes generadas o escritas anteriormente de los recursos de memoria de vídeo a recursos de memoria del sistema no volátiles. Dado que las imágenes de origen de la transferencia se podrían perder en cualquier momento, Direct3D permite que estas operaciones de copia generen un error cuando se pierde el dispositivo.

Se puede producir un error en las operaciones de copia porque no hay ninguna superficie principal cuando se pierde el dispositivo. También se puede producir un error en la creación de cadenas de intercambio porque no se puede crear un búfer de reserva cuando se pierde el dispositivo.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Dispositivos](devices.md)

 

 




