---
ms.assetid: E1943DCE-833F-48AE-8402-CD48765B24FC
title: Optimizar la suspensión y reanudación
description: Crea aplicaciones para la Plataforma universal de Windows (UWP) que optimicen el uso del sistema del ciclo de vida de los procesos, para reanudarlo de forma eficaz tras su suspensión o finalización.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 06af6241bdd75efdd3ff71e02f74252d60540669
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2018
ms.locfileid: "8947433"
---
# <a name="optimize-suspendresume"></a>Optimizar la suspensión y reanudación


Crea aplicaciones para la Plataforma universal de Windows (UWP) que optimicen el uso del sistema del ciclo de vida de los procesos, para reanudarlo de forma eficaz tras su suspensión o finalización.

## <a name="launch"></a>Ejecución

Al volver a activar una aplicación después de su suspensión o finalización, comprueba si ha transcurrido mucho tiempo. Si es así, considera la posibilidad de volver a la página de aterrizaje principal de la aplicación en lugar de mostrar los datos de usuario obsoletos. Esto también resultará en un inicio más rápido.

Durante la activación, comprueba siempre el objeto PreviousExecutionState del parámetro de argumentos del evento (por ejemplo, para las activaciones iniciadas comprueba el objeto LaunchActivatedEventArgs.PreviousExecutionState). Si el valor es ClosedByUser o NotRunning, no pierdas tiempo restaurando el estado guardado anteriormente. En este caso, lo correcto es proporcionar una experiencia nueva y resultará en un inicio más rápido.

En lugar de una restaurar el estado guardado anteriormente, considera la posibilidad de hacer un seguimiento de ese estado y solo restaurarlo a petición. Por ejemplo, considera la posibilidad de una situación donde la aplicación se había suspendido anteriormente, guardó el estado de 3 páginas y luego finalizó. Al volver a iniciarla, si quieres que el usuario vuelva a la página 3, no restaura el estado de las 2 primeras páginas. En cambio, mantén este estado y úsalo solo cuando sepas que los necesitas.

## <a name="while-running"></a>Durante la ejecución

Como procedimiento recomendado, no esperes al evento de suspensión ni conserves una gran cantidad de datos de estado. En cambio, la aplicación debería persistir cantidades más pequeñas de datos de estado de manera incremental mientras se ejecuta. Esto es especialmente importante para aplicaciones de gran tamaño que corren el riesgo de quedarse sin tiempo durante la suspensión si intentan guardar todo a la vez.

Sin embargo, debes encontrar un buen equilibrio entre el guardado incremental y el rendimiento de la aplicación durante su ejecución. Un buen equilibrio es hacer un seguimiento incremental de los datos que han cambiado (y, por tanto, los que se deben guardar) y usar el evento de suspensión para realmente guardar esos datos (los que es más rápido que guardar todos los datos o examinar todo el estado de la aplicación para decidir qué guardar).

No uses los eventos de ventana Activated o VisibilityChanged para decidir cuándo guardar el estado. Cuando el usuario abandone la aplicación, la ventana se desactiva, pero el sistema espera un período breve de tiempo (aproximadamente 10 segundos) antes de suspender la aplicación. Esto es para ofrecer una experiencia con mayor capacidad de respuesta en caso de que el usuario vuelva a la aplicación rápidamente. Espera al evento de suspensión antes de ejecutar la lógica de suspensión.

## <a name="suspend"></a>Suspensión

Durante la suspensión, reduce la superficie de la aplicación. Si la aplicación usa menos memoria cuando está suspendida, el sistema global tendrá mayor capacidad de respuesta y finalizará un menor número de aplicaciones (incluida la tuya). Sin embargo, sopesa esto con la necesidad de reanudar rápidamente: no reduzcas la superficie tanto que la reanudación se ralentiza considerablemente mientras la aplicación vuelve a cargar una gran cantidad de datos en la memoria.

En el caso de las aplicaciones administradas, el sistema ejecutará una recolección de elementos no utilizados después de que se completen los controladores de suspensión de la aplicación. Asegúrate de aprovechar esto soltando las referencias a objetos, lo que ayudará a reducir la superficie de la aplicación mientras esté suspendida.

En teoría, la aplicación finalizará con la lógica de suspensión en menos de 1 segundo. Cuanto más rápido se puede suspender, mejor. Esto resultará en una experiencia de usuario más ágil para otras aplicaciones y componentes del sistema. Si es necesario, la lógica de suspensión puede tardar hasta 5 segundos en dispositivos de escritorio o 10 segundos en dispositivos móviles. Si se superan esos tiempos, la aplicación finalizará repentinamente. Esto no es algo deseado, ya que, su sucede, cuando el usuario vuelva a la aplicación, se iniciará un nuevo proceso y la experiencia dará la sensación de ser mucho más lenta en comparación con la reanudación de una aplicación suspendida.

## <a name="resume"></a>Reanudación

La mayoría de las aplicaciones no necesita hacer nada especial cuando se reanudan, por lo que normalmente no tendrás que controlar este evento. Algunas aplicaciones usan la reanudación para restaurar las conexiones cerradas durante la suspensión o para actualizar los datos posiblemente obsoletos. En lugar de hacer este tipo de trabajo, diseña tu aplicación de modo que inicie estas actividades a petición. De este modo, se producirá una experiencia más rápida cuando el usuario vuelva a una aplicación suspendida y solo harás el trabajo que el usuario realmente necesita.

## <a name="avoid-unnecessary-termination"></a>Evitar la finalización innecesaria

El sistema del ciclo de vida de los procesos UWP puede suspender o finalizar una aplicación por varias razones. Este proceso se ha diseñado para devolver rápidamente una aplicación al estado en que se encontraba antes de suspenderse o finalizarse. Cuando se realiza correctamente, el usuario no se enterará de que la aplicación dejó de funcionar en algún momento. Estos son algunos trucos que tu aplicación para UWP puede usar para ayudar al sistema a simplificar las transiciones durante el ciclo de vida de una aplicación.

Una aplicación puede suspenderse cuando el usuario la mueve a segundo plano o cuando el sistema entra en estado de baja energía. Al suspender la aplicación, se genera el evento de suspensión y deben guardarse los datos en cinco segundos como máximo. Si el controlador de eventos de suspensión de la aplicación no finaliza en cinco segundos, el sistema supone que la aplicación dejó de responder y la finaliza. Una aplicación finalizada tiene que pasar por el largo proceso de inicio en lugar de cargarse inmediatamente en la memoria cuando un usuario cambia a ella.

### <a name="serialize-only-when-necessary"></a>Serializar solo cuando sea necesario

Muchas aplicaciones serializan todos sus datos cuando están en suspensión. Si solo necesitas almacenar una pequeña cantidad de datos de configuración de la aplicación, debes usar el almacén [**LocalSettings**](https://msdn.microsoft.com/library/windows/apps/BR241622) en lugar de serializar los datos. Usa la serialización para administrar grandes cantidades de datos y para datos que no sean de configuración.

Cuando serializas tus datos, debes evitar volver a hacerlo si no han sufrido cambios. De lo contrario, la aplicación tarda más en serializar y guardar los datos, además del tiempo adicional necesario para leerlos y deserializarlos, cuando se vuelve a activar la aplicación. En lugar de ello, te recomendamos que la aplicación determine si cambió realmente su estado, y que serialice y deserialice solamente los datos que se modificaron. Una buena forma de garantizar este comportamiento consiste en serializar datos en segundo plano a medida que cambian. Cuando usas esta técnica, todo lo que necesita serializarse durante la suspensión ya se ha guardado, por lo que no queda trabajo por hacer y la aplicación se suspende rápidamente.

### <a name="serializing-data-in-c-and-visual-basic"></a>Serializar datos en C# y Visual Basic

Las opciones disponibles en cuanto a la tecnología de serialización para aplicaciones .NET son las clases [**System.Xml.Serialization.XmlSerializer**](https://msdn.microsoft.com/library/windows/apps/xaml/system.xml.serialization.xmlserializer.aspx), [**System.Runtime.Serialization.DataContractSerializer**](https://msdn.microsoft.com/library/windows/apps/xaml/system.runtime.serialization.datacontractserializer.aspx) y [**System.Runtime.Serialization.Json.DataContractJsonSerializer**](https://msdn.microsoft.com/library/windows/apps/xaml/system.runtime.serialization.json.datacontractjsonserializer.aspx).

Desde el punto de vista del rendimiento, te recomendamos usar la clase [**XmlSerializer**](https://msdn.microsoft.com/library/windows/apps/xaml/system.xml.serialization.xmlserializer.aspx). La clase **XmlSerializer** cuenta con los tiempos más bajos de serialización y deserialización, y mantiene una superficie de memoria baja. Asimismo, **XmlSerializer** tiene menos dependencias en .NET Framework. Esto significa que, en comparación con otras tecnologías de serialización, deben cargarse menos módulos en la aplicación para usar **XmlSerializer**.

[**DataContractSerializer**](https://msdn.microsoft.com/library/windows/apps/xaml/system.runtime.serialization.datacontractserializer.aspx) facilita la serialización de clases personalizadas, aunque supone un mayor impacto en el rendimiento que **XmlSerializer**. Si necesitas mejorar el rendimiento, considera cambiar de opciones. En general, no debes cargar más de un serializador y te recomendamos que uses **XmlSerializer**, a menos que necesites usar las características de otro serializador.

### <a name="reduce-memory-footprint"></a>Reducir la superficie de memoria

El sistema intenta mantener todas las aplicaciones suspendidas posibles en la memoria para que los usuarios puedan alternar entre ellas de modo rápido y fiable. Cuando una aplicación se suspende y permanece en la memoria del sistema, se la puede pasar al primer plano rápidamente para que el usuario interactúe con ella, sin que sea necesario mostrar una pantalla de presentación ni realizar una operación de carga prolongada. Si no hay recursos suficientes para mantener una aplicación en la memoria, la aplicación finaliza. Esto hace que la administración de la memoria sea importante por dos motivos:

-   La liberación de toda la memoria posible en la suspensión minimiza el riesgo de que finalice la aplicación por falta de recursos cuando está suspendida.
-   La reducción de la cantidad total de memoria que usa la aplicación minimiza el riesgo de que otras aplicaciones finalicen cuando están suspendidas.

### <a name="release-resources"></a>Liberar recursos

Determinados objetos, como archivos y dispositivos, ocupan una gran cantidad de memoria. Te recomendamos que, durante la suspensión, las aplicaciones liberen los controles de estos objetos y los recreen cuando sea necesario. Este también es un buen momento para purgar las memorias caché que no serán válidas cuando se reanude la aplicación. Otro paso que el marco XAML ejecuta automáticamente para las aplicaciones de C# y Visual Basic es la recolección de elementos no utilizados cuando es necesario. Esto garantiza que se liberen todos los objetos a los que ya no se hace referencia en el código de la aplicación.

## <a name="resume-quickly"></a>Reanudar la aplicación rápidamente

Una aplicación suspendida puede reanudarse cuando el usuario la mueve al primer plano o cuando el sistema sale del estado inactivo. Cuando se reanuda una aplicación en estado suspendido, continúa en el punto en el que estaba en el momento de la suspensión. No se pierden los datos de la aplicación, ya que se guardaron en la memoria incluso si la aplicación estuvo suspendida durante un período extenso.

La mayoría de las aplicaciones no necesitan controlar el evento [**Resuming**](https://msdn.microsoft.com/library/windows/apps/BR205859). Cuando se reanuda la aplicación, las variables y los objetos tienen exactamente el mismo estado que tenían cuando esta se suspendió. Controla el evento **Resuming** solamente si necesitas actualizar datos u objetos que pudieron haberse modificado durante la suspensión y hasta que se reanudó la aplicación como, por ejemplo, contenido (datos de fuente de actualización), conexiones de red que pudieran haber quedado obsoletas o si resulta que necesitas volver a acceder a un dispositivo (por ejemplo, una cámara web).

## <a name="related-topics"></a>Temas relacionados

* [Directrices para suspender y reanudar una aplicación](https://msdn.microsoft.com/library/windows/apps/Hh465088)
 

 




