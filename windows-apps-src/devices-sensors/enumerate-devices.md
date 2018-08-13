---
author: muhsinking
ms.assetid: 4311D293-94F0-4BBD-A22D-F007382B4DB8
title: Enumerar dispositivos
description: El espacio de nombres de enumeración te permite buscar dispositivos que están conectados en el sistema de forma interna o externa o que se pueden detectar mediante protocolos de redes o de redes inalámbricas.
ms.author: mukin
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1cf6e8fe3205d70479a590bf73f7a01cd7ac3848
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2018
ms.locfileid: "958920"
---
# <a name="enumerate-devices"></a>Enumerar dispositivos


## <a name="samples"></a>Muestras

La forma más sencilla de enumerar todos los dispositivos disponibles es hacer una instantánea con el comando [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.findallasync.aspx), que se explica detalladamente más abajo.

```CSharp
async void enumerateSnapshot(){
  DeviceInformationCollection collection = await DeviceInformation.FindAllAsync();
}
```

Para descargar una muestra que presente los usos más avanzados de las API de [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459), haz clic [aquí](http://go.microsoft.com/fwlink/?LinkID=620536).

## <a name="enumeration-apis"></a>API de enumeración

El espacio de nombres de enumeración te permite buscar dispositivos conectados internamente con el sistema, conectados externamente o detectables mediante protocolos de redes o de redes inalámbricas. Las API que se usan para enumerar los dispositivos posibles son los espacios de nombres [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459). Entre las razones para usar estas API se incluyen las siguientes:

-   Buscar un dispositivo para conectar con la aplicación.
-   Obtener información sobre los dispositivos conectados o detectables por el sistema.
-   Recibir notificaciones en una aplicación cuando se agregan, conectan o desconectan dispositivos, cuando se modifica el estado de conexión o se cambian otras propiedades.
-   Recibir desencadenadores en segundo plano en una aplicación cuando se conectan o desconectan dispositivos, cuando se modifica el estado de conexión o se cambian otras propiedades.

Estas API pueden enumerar dispositivos en cualquiera de los protocolos y buses siguientes, siempre que el dispositivo y el sistema que ejecutan la aplicación admitan esa tecnología. Esta no es una lista exhaustiva y puede que dispositivos específicos admitan otros protocolos.

-   Buses físicamente conectados. Esto incluye PCI y USB. Por ejemplo, todo lo que puedes ver en el **Administrador de dispositivos**.
-   [UPnP](https://msdn.microsoft.com/library/windows/desktop/Aa382303)
-   Digital Living Network Alliance (DLNA)
-   [**Discovery and Launch (DIAL)**](https://msdn.microsoft.com/library/windows/apps/Dn946818)
-   [**Detección de servicio DNS (DNS-SD)**](https://msdn.microsoft.com/library/windows/apps/Dn895183)
-   [Web Services on Devices (WSD)](https://msdn.microsoft.com/library/windows/desktop/Aa826001)
-   [Bluetooth](https://msdn.microsoft.com/library/windows/desktop/Aa362932)
-   [**Wi-Fi Direct**](https://msdn.microsoft.com/library/windows/apps/Dn297687)
-   WiGig
-   [**Punto de servicio**](https://msdn.microsoft.com/library/windows/apps/Dn298071)

En muchos casos, no tendrás que preocuparte por usar las API de enumeración. Esto es porque muchas de las API que usan los dispositivos seleccionarán automáticamente el dispositivo predeterminado adecuado o proporcionarán una API de enumeración optimizada. Por ejemplo, [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/BR242926) usará automáticamente el dispositivo de representación de audio predeterminado. Siempre que la aplicación pueda usar el dispositivo predeterminado, no es necesario usar las API de enumeración en la aplicación. Las API de enumeración ofrecen una forma general y flexible de detectar dispositivos disponibles y conectarse a ellos. En este tema se proporciona información sobre la enumeración de dispositivos y se describen las cuatro formas comunes de enumerar dispositivos.

-   Mediante la interfaz de usuario [**DevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn930841)
-   Enumerar una instantánea de dispositivos que son reconocibles actualmente por el sistema
-   Enumerar dispositivos que son reconocibles actualmente y ver los cambios
-   Enumerar dispositivos que son reconocibles actualmente y ver los cambios en una tarea en segundo plano

## <a name="deviceinformation-objects"></a>Objetos DeviceInformation


Al trabajar con las API de enumeración, necesitarás usar con frecuencia objetos [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393). Estos objetos contienen la mayoría de la información disponible sobre el dispositivo. La siguiente tabla explica algunas de las propiedades **DeviceInformation** que pueden interesarte. Para ver una lista completa, consulta la página de referencia **DeviceInformation**.

| Propiedad                         | Comentarios                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **DeviceInformation.Id**         | Este es el identificador único del dispositivo y se proporciona como una variable de cadena. En la mayoría de los casos, se trata de un valor opaco que se pasará de un método a otro para indicar el dispositivo específico que te interesa. También puedes usar esta propiedad y la propiedad **DeviceInformation.Kind** después de cerrar la aplicación y volver a abrirla. Esto garantiza que se pueda recuperar y volver a usar el mismo objeto [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393). |
| **DeviceInformation.Kind**       | Esto indica el tipo de objeto de dispositivo que representa el objeto [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393). No es la categoría del dispositivo o el tipo de dispositivo. Un solo dispositivo puede representarse con distintos objetos **DeviceInformation** de diferentes tipos. Los posibles valores de esta propiedad se enumeran en [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationkind.aspx) y se indica la forma en que se relacionan entre sí.                           |
| **DeviceInformation.Properties** | Este contenedor de propiedades contiene información que se solicita para el objeto [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393). Puedes hacer referencia fácilmente a las propiedades más comunes como propiedades del objeto **DeviceInformation**, como sucede, por ejemplo, con [**DeviceInformation.Name**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.name). Para obtener más información, consulta [Propiedades de información de dispositivo](device-information-properties.md).                                                                |

 

## <a name="devicepicker-ui"></a>Interfaz de usuario de DevicePicker


La clase [**DevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn930841) es un control de Windows que crea una pequeña interfaz de usuario que permite al usuario seleccionar un dispositivo de una lista. Puedes personalizar la ventana **DevicePicker** de varias maneras.

-   Puedes controlar los dispositivos que se muestran en la interfaz de usuario agregando una propiedad [**SupportedDeviceSelectors**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicepickerfilter.supporteddeviceselectors.aspx), una propiedad [**SupportedDeviceClasses**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicepickerfilter.supporteddeviceclasses.aspx) o ambas, a la propiedad [**DevicePicker.Filter**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicepicker.filter). En la mayoría de los casos, basta con agregar un selector o una clase; aunque si necesitas más de uno, puedes agregar varios. Si agregas varios selectores o clases, estos deben conectarse con una función de lógica OR.
-   Puedes especificar las propiedades que deseas recuperar para los dispositivos. Para ello, agrega las propiedades a [**DevicePicker.RequestedProperties**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicepicker.requestedproperties).
-   Puedes modificar la apariencia de la clase [**DevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn930841) mediante [**Appearance**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicepicker.appearance).
-   Puedes especificar el tamaño y la ubicación de [**DevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn930841) cuando se muestra.

Mientras se muestra la clase [**DevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn930841), el contenido de la interfaz de usuario se actualizará automáticamente si se agregan, quitan o actualizan los dispositivos.

**Nota** No puedes especificar [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationkind.aspx) mediante [**DevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn930841). Si quieres tener dispositivos con un elemento **DeviceInformationKind** determinado, tendrás que compilar una clase [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446) y proporcionar tu propia interfaz de usuario.

 

La conversión de contenido multimedia y DIAL también proporciona sus propios selectores, si quieres usarlos. Son, respectivamente, [**CastingDevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn972525) y [**DialDevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn946783).

## <a name="enumerate-a-snapshot-of-devices"></a>Enumerar una instantánea de dispositivos


En algunos casos, la clase [**DevicePicker**](https://msdn.microsoft.com/library/windows/apps/Dn930841) no será adecuada para tus necesidades y necesitarás algo más flexible. Es posible que quieras compilar tu propia interfaz de usuario o que necesites enumerar dispositivos sin mostrar una interfaz de usuario al usuario. En estas situaciones, puedes enumerar una instantánea de dispositivos. Esto implica echar un vistazo a los dispositivos que están actualmente conectados o emparejados con el sistema. Sin embargo, debes tener en cuenta que este método solo examina una instantánea de los dispositivos que están disponibles, por lo que no podrás encontrar los dispositivos que se conecten después de enumerarlos a través de la lista. Tampoco recibirás ninguna notificación si se actualiza o quita un dispositivo. Otra posible desventaja que debes tener en cuenta es que este método retendrá los resultados hasta que se complete toda la enumeración. Por este motivo, no debes usar este método cuando estés interesado en los objetos **AssociationEndpoint**, **AssociationEndpointContainer** o **AssociationEndpointService**, ya que se encuentran en un protocolo de redes o de redes inalámbricas. Esta operación puede tardar hasta 30 segundos en completarse. En ese caso, deberías usar un objeto [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446) para enumerar los dispositivos posibles.

Para enumerar una instantánea de dispositivos, usa el método [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.findallasync.aspx). Este método espera hasta que se completa todo el proceso de enumeración y devuelve todos los resultados como un objeto [**DeviceInformationCollection**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationcollection.aspx). Este método también se sobrecarga para proporcionar varias opciones para filtrar los resultados y limitarlos a los dispositivos de tu interés. Para ello, proporciona una enumeración [**DeviceClass**](https://msdn.microsoft.com/library/windows/apps/BR225381) o pasa un selector de dispositivos. El selector de dispositivos es una cadena AQS que especifica los dispositivos que quieres enumerar. Para obtener más información, consulta [Compilar un selector de dispositivos](build-a-device-selector.md).

A continuación se proporciona un ejemplo de instantánea de enumeración de dispositivos:



Además de limitar los resultados, también puedes especificar las propiedades que quieres recuperar para los dispositivos. Si lo haces, las propiedades especificadas estarán disponibles en el contenedor de propiedades de cada objeto [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) que se devuelva en la colección. Es importante tener en cuenta que no todas las propiedades están disponibles para todos los tipos de dispositivos. Para ver qué propiedades están disponibles para los distintos tipos de dispositivos, consulta [Propiedades de información de dispositivo](device-information-properties.md).



## <a name="enumerate-and-watch-devices"></a>Enumerar y ver los dispositivos


Para enumerar dispositivos mediante un método eficaz y flexible, crea una clase [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446). Esta opción proporciona la máxima flexibilidad cuando enumeras dispositivos. Te permite enumerar los dispositivos que hay actualmente y recibir notificaciones cuando los dispositivos que coinciden con el selector de dispositivos se agregan o se quitan, o si cambian las propiedades. Cuando creas una clase **DeviceWatcher**, proporcionas un selector de dispositivos. Para obtener más información sobre los selectores de dispositivos, consulta el tema [Crear un selector de dispositivos](build-a-device-selector.md). Después de crear el monitor, recibirás las notificaciones siguientes para cualquier dispositivo que coincida con los criterios proporcionados.

-   Agregar notificación cuando se agrega un nuevo dispositivo.
-   Actualizar notificación cuando se cambia una propiedad que te interesa.
-   Quitar las notificaciones cuando un dispositivo ya no esté disponible o ya no coincida con el filtro.

En la mayoría de los casos en los que se usa la clase [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446), se mantiene una lista de dispositivos de la cual se agregan, se quitan o se actualizan elementos a medida que el monitor recibe actualizaciones de los dispositivos que observas. Cuando recibas una notificación de actualización, la información actualizada estará disponible como un objeto [**DeviceInformationUpdate**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationupdate.aspx). Para actualizar la lista de dispositivos, primero busca la clase [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) que se modificó. A continuación, llama al método [**Update**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.update) de ese objeto y proporciona el objeto **DeviceInformationUpdate**. Se trata de una función de conveniencia que actualizará automáticamente el objeto **DeviceInformation**.

Dado que una clase [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446) envía notificaciones a medida que llegan los dispositivos y cuando estos cambian, te recomendamos usar este método de enumeración de dispositivos cuando estés interesado en los objetos **AssociationEndpoint**, **AssociationEndpointContainer** o **AssociationEndpointService**, ya que se enumeran a través de protocolos de redes o de redes inalámbricas.

Para crear una clase [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446), usa uno de los métodos [**CreateWatcher**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.createwatcher.aspx). Estos métodos se sobrecargan para que puedas especificar los dispositivos que te interesan. Para ello, proporciona una enumeración [**DeviceClass**](https://msdn.microsoft.com/library/windows/apps/BR225381) o pasa un selector de dispositivos. El selector de dispositivos es una cadena AQS que especifica los dispositivos que quieres enumerar. Para más información, consulta [Compilar un selector de dispositivos](build-a-device-selector.md). También puedes especificar las propiedades que deseas recuperar para los dispositivos y que te interesan. Si lo haces, las propiedades especificadas estarán disponibles en el contenedor de propiedades de cada objeto [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) que se devuelva en la colección. Es importante tener en cuenta que no todas las propiedades están disponibles para todos los tipos de dispositivos. Para ver qué propiedades están disponibles para los distintos tipos de dispositivos, consulta [Propiedades de información de dispositivo](device-information-properties.md)

## <a name="watch-devices-as-a-background-task"></a>Ver dispositivos como una tarea en segundo plano


Ver dispositivos como una tarea en segundo plano es muy similar a crear una clase [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446), tal como se describió anteriormente. De hecho, deberás crear primero un objeto **DeviceWatcher** normal, tal como se describe en la sección anterior. Una vez creado, debes llamar al método [**GetBackgroundTrigger**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicewatcher.enumerationcompleted.aspx) en vez de a [**DeviceWatcher.Start**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicewatcher.start). Cuando llames a **GetBackgroundTrigger**, debes especificar cuál de las notificaciones te interesa: agregar, quitar o actualizar. No se puede solicitar la notificación de actualizar o quitar sin solicitar la de agregar. Una vez registres el desencadenador, la clase **DeviceWatcher** comenzará a ejecutarse inmediatamente en segundo plano. De ahora en adelante, siempre que recibas una nueva notificación para la aplicación que coincida con los criterios, se desencadenará la tarea en segundo plano y te proporcionará los cambios más recientes desde la última vez que se desencadenó la aplicación.

**Importante** La primera vez que una clase [**DeviceWatcherTrigger**](https://msdn.microsoft.com/library/windows/apps/Dn913838) desencadene la aplicación, el observador cambiará al estado **EnumerationCompleted**. Esto significa que contendrá todos los resultados iniciales. Las siguientes veces que desencadene tu aplicación, solo contendrá las notificaciones de agregar, actualizar y quitar que se hayan producido desde que se activó el último desencadenador. Esta acción se diferencia de un objeto [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446) de primer plano, ya que los resultados iniciales no llegan de uno en uno y solo se entregan en conjunto después alcanzar el estado **EnumerationCompleted**.

 

Algunos protocolos de redes inalámbricas se comportan de forma diferente cuando realizan exploraciones en segundo plano frente a las de primer plano, aunque también es posible que no admitan las exploraciones en segundo plano. Hay tres posibilidades en relación con las exploraciones en segundo plano. La siguiente tabla enumeran las posibilidades y los efectos que esto puede tener en la aplicación. Por ejemplo, Bluetooth y Wi-Fi Direct no son compatibles con las exploraciones en segundo plano, por lo que, por extensión, no admiten la clase [**DeviceWatcherTrigger**](https://msdn.microsoft.com/library/windows/apps/Dn913838).

| Comportamiento                                  | Impacto                                                                                                                                  |
|-------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|
| El mismo comportamiento en segundo plano               | Ninguno                                                                                                                                    |
| Solo se pueden realizar exploraciones pasivas en segundo plano | Es posible que se tarde más en detectar el dispositivo mientras se espera a que se produzca un examen pasivo.                                                           |
| No se admiten las exploraciones en segundo plano            | [**DeviceWatcherTrigger**](https://msdn.microsoft.com/library/windows/apps/Dn913838) no podrá detectar ningún dispositivo y no se notificará ninguna actualización. |

 

Si [**DeviceWatcherTrigger**](https://msdn.microsoft.com/library/windows/apps/Dn913838) incluye un protocolo que no es compatible con la exploración como tarea en segundo plano, el desencadenador seguirá funcionando. Sin embargo, no podrás obtener actualizaciones ni resultados a través de ese protocolo. Las actualizaciones para otros protocolos o dispositivos se seguirán detectando con normalidad.

## <a name="using-deviceinformationkind"></a>Usar DeviceInformationKind


En la mayoría de casos, no deberás preocuparte por la enumeración [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationkind.aspx) de un objeto [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393). Esto se debe a que el selector de dispositivos que devuelve la API del dispositivo que usas, garantizará a menudo que recibas los tipos de objetos de dispositivo correctos para usar con su API. Sin embargo, en algunos escenarios querrás conseguir el objeto **DeviceInformation** para dispositivos, pero no hay ninguna API de dispositivo que proporcione un selector de dispositivos. En estos casos deberás compilar tu propio selector. Por ejemplo, Web Services on Devices no tiene una API dedicada, pero puede detectar estos dispositivos y obtener información sobre ellos con las API [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459) y, a continuación, usarlos con las API de socket.

Si compilas tu propio selector de dispositivos para enumerar a través de objetos de dispositivo, será importante que comprendas cómo funciona [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationkind.aspx). Todos los tipos posibles, así como la forma en que se relacionan entre ellos, se describen en la página de referencia de **DeviceInformationKind**. Uno de los usos más comunes de **DeviceInformationKind** es especificar qué tipo de dispositivos buscas al enviar una consulta junto con un selector de dispositivos. Al hacerlo, te aseguras de que solo enumeras dispositivos que coincidan con la enumeración **DeviceInformationKind** proporcionada. Por ejemplo, podrías buscar un objeto **DeviceInterface** y, a continuación, ejecutar una consulta para obtener la información del objeto primario **Device**. Además, dicho objeto primario podría contener información adicional.

Es importante tener en cuenta que las propiedades disponibles en el contenedor de propiedades de un objeto [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393), variarán en función de la clase [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformationkind.aspx) del dispositivo. Algunas propiedades solo están disponibles con determinados tipos. Para obtener más información sobre las propiedades disponibles para cada tipo, consulta el artículo [Propiedades de información de dispositivo](device-information-properties.md). Así pues, en el ejemplo anterior, si buscas el objeto primario **Device**, obtendrás acceso a información adicional que no estaba disponible en el objeto de dispositivo **DeviceInterface**. Por este motivo, cuando creas las cadenas de filtro AQS, es importante que te asegures de que las propiedades solicitadas están disponibles para los objetos **DeviceInformationKind** que estás enumerando. Para obtener más información acerca de cómo compilar un filtro, consulta el tema [Crear un selector de dispositivos](build-a-device-selector.md).

Cuando se enumeran objetos como **AssociationEndpoint**, **AssociationEndpointContainer** o **AssociationEndpointService**, la enumeración se realiza a través de un protocolo de redes o de redes inalámbricas. En estos casos, te recomendamos no usar [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.findallasync.aspx) y usar [**CreateWatcher**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.createwatcher.aspx) en su lugar. Esto se debe a que las búsquedas a través de una red a menudo dan como resultado operaciones de búsqueda cuyo tiempo de espera no finalizará en un período de 10 o más segundos antes de generar [**EnumerationCompleted**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.devicewatcher.enumerationcompleted.aspx). **FindAllAsync** no completa la operación hasta que se desencadena **EnumerationCompleted**. Si usas [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/BR225446), obtendrás resultados más próximos a los resultados en tiempo real independientemente de cuándo se llame al evento **EnumerationCompleted**.

## <a name="save-a-device-for-later-use"></a>Guardar un dispositivo para usarlo más adelante


Todos los objetos [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/BR225393) se identifican de forma exclusiva mediante una combinación de dos fragmentos de información: [**DeviceInformation.Id**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.id) y [**DeviceInformation.Kind**](https://msdn.microsoft.com/library/windows/apps/windows.devices.enumeration.deviceinformation.kind.aspx). Si mantienes estos dos fragmentos de información, puedes volver a crear un objeto **DeviceInformation** después de perderlo si se proporciona esta información a [**CreateFromIdAsync**](https://msdn.microsoft.com/library/windows/apps/br225425.aspx). De este modo, puedes guardar las preferencias del usuario para un dispositivo que se integre con la aplicación.


 

 




