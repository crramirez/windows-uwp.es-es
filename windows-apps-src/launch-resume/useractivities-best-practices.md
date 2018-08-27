---
author: PatrickFarley
title: Procedimientos recomendados de las actividades de usuario
description: Esta guía describe los procedimientos recomendados para la creación y actualización de las actividades del usuario.
keywords: actividad del usuario, actividades del usuario, línea de tiempo, cortana continúa donde lo dejaste, cortana continúa desde donde lo dejé, proyecto rome
ms.author: pafarley
ms.date: 08/23/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: b11151df981a4b5ce233458581d21e405be9844c
ms.sourcegitcommit: 753dfcd0f9fdfc963579dd0b217b445c4b110a18
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "2854988"
---
# <a name="user-activities-best-practices"></a>Procedimientos recomendados de las actividades de usuario

Esta guía describe los procedimientos recomendados para la creación y actualización de las actividades del usuario. Para obtener información general de la característica de las actividades del usuario en Windows, vea la [actividad de usuario de continuar, incluso a través de dispositivos](https://docs.microsoft.com/windows/uwp/launch-resume/useractivities). O bien, vea la [sección actividades de usuario](https://docs.microsoft.com/windows/project-rome/user-activities/) de Project Roma para las implementaciones de actividades en otras plataformas de desarrollo.

## <a name="when-to-create-or-update-user-activities"></a>Cuándo se debe crear o actualizar las actividades del usuario

Debido a que cada aplicación es diferente, es copia de seguridad a cada programador para determinar la mejor forma de asignar acciones dentro de la aplicación a las actividades del usuario. Las actividades del usuario se mostrarán en Cortana y escala de tiempo, que se centran en eficacia y productividad de los usuarios creciente al ayudarles a ponerse en contacto con el contenido que visitado en el pasado.

**Directrices generales**

* **Registre una sola actividad para un grupo de acciones de usuario relacionados.** Esto es especialmente importante para las listas de reproducción de música o programas de TV: una sola actividad se puede actualizar a intervalos regulares para reflejar el progreso del usuario. En este caso, tendrá una actividad de usuario único con varios elementos de historial que representa los períodos de compromiso a través de varios días o semanas. El mismo se aplica a las actividades de documento basado en el que el usuario realiza gradual progreso dentro de la aplicación.
* **Almacenar datos de usuario en la nube.** Si desea admitir actividades entre el dispositivo, debe asegurarse de que el contenido que se requieren para volver a activar esta actividad se almacena en una ubicación en la nube. Actividades específicas de dispositivo aparecerán en la escala de tiempo en el dispositivo donde se creó la actividad, pero no es posible que aparezca en otros dispositivos.
* **No crear actividades para las acciones que los usuarios no necesitan para reanudar.** Si la aplicación se usa para llevar a cabo operaciones simples únicas que no se mantiene el estado, probablemente no es necesario crear una actividad de usuario.
* **No crear actividades para acciones completadas por otros usuarios.** Si una cuenta externa envía al usuario un mensaje o @-mentions ellos dentro de la aplicación, no se debe crear una actividad para esto. Este tipo de acción es preferible acción centro de notificaciones.
  * Escenarios de colaboración son una excepción: si varios usuarios están trabajando en la misma actividad juntos (por ejemplo, un documento de Word), habrá casos en los que otro usuario ha realizado cambios después de que el usuario. En este caso, es posible que desee actualizar la actividad existente para reflejar los cambios realizados en el documento. Esto podría implicar una actualización de los datos de contenido existentes de actividad del usuario sin crear un nuevo elemento de historial.

**Instrucciones para tipos específicos de aplicaciones**

Aunque cada aplicación es diferente, la mayoría de aplicaciones se inscriben en uno de los siguientes patrones de interacción.
* **Aplicaciones basadas en el documento** : crear una actividad por cada documento, con uno o varios elementos de historial que refleja períodos de uso. Es importante que se debe actualizar su actividad tal y como se realizan cambios en el documento.
* **Juegos** : crear una actividad para cada juego guardar o world. Si su juego es compatible con sólo una única secuencia de niveles, puede publicar volver a la misma actividad a través del tiempo, aunque es posible que desee actualizar los datos de contenido para mostrar el progreso o los logros más recientes.
* **Aplicaciones de utilidad** : si no hay nada dentro de la aplicación y que los usuarios tendría que deje y reanudar, no es necesario usar las actividades del usuario. Un buen ejemplo es una aplicación simple como la calculadora.
* **Aplicaciones de línea de negocio** , existen muchas aplicaciones para la administración de tareas sencillas o flujos de trabajo. Crear una actividad para cada flujo de trabajo independiente que tiene acceso a través de la aplicación (por ejemplo, informes de gastos sería cada uno una actividad independiente, por lo que podría el usuario, a continuación, haga clic en una actividad para ver si se ha aprobado un informe en particular).
* **Aplicaciones de reproducción de medios** : crear una actividad por una agrupación lógica de contenido (por ejemplo, una lista de reproducción, programa o independiente el contenido). La pregunta subyacente para desarrolladores de aplicaciones es si los recuentos de una cada parte del contenido (episodio TV, canción) como contenido independiente o como parte de una colección. Como regla general, si el usuario opta por reproducir una colección o contenido secuencial, la colección como un todo es la actividad. Si optan para reproducir un solo elemento de contenido, que una parte del contenido es la actividad. Vea directrices más específicas que aparece a continuación.
  * **Música: intérprete/álbum o género** : si el usuario selecciona un álbum, intérprete o género y aciertos de **Reproducir**, esa colección es la actividad; no escribir una actividad independiente para cada canción. Para las colecciones, cortas como un álbum o colecciones que se vuelvan a reproducir en un orden aleatorio, es posible que no necesita actualizar la actividad para reflejar la posición del usuario actual. Para la reproducción durante cuánto tiempo secuencial como un álbum o una lista de reproducción, grabación de posición en el álbum podría tener sentido.
  * **Música: listas de reproducción de smart** : aplicaciones que reproducirán música en orden aleatorio deben registrar una sola actividad para esa lista de reproducción. Si el usuario reproduce la lista de reproducción en una segunda vez, debería crear registros de historial adicionales para la misma actividad. Grabación de la posición del usuario actual en la lista de reproducción no es necesario porque el orden es aleatorio.
  * **Serie de TV** , si la aplicación está configurada para reproducir el episodio siguiente una vez finalizada la actual, debe escribir una sola actividad para la serie de TV. Como reproducir los episodios distintos a través de varias sesiones de visualización, podrá actualizar su actividad para reflejar la posición actual en la serie y se crearán varios registros de historial.
  * **Película** : una película es un único elemento de contenido y debe tener su propio registro de historial. Si el usuario deja de ver la forma parte de película a través de, es deseable para registrar su posición. Cuando desea reanudarla en el futuro, la actividad puede reanudar la película desde donde se dejaron o incluso preguntar al usuario si desea reanudar o iniciar al principio.

## <a name="user-activity-design"></a>Diseño de la actividad de usuario

Las actividades del usuario constan de tres componentes: una activación URI, datos visual y los metadatos de contenido.
* La activación URI es un identificador URI que se puede pasar a una aplicación o la experiencia con el fin de reanudar la aplicación con un contexto específico. Normalmente, estos vínculos tomar la forma de controlador de protocolo para un esquema (por ejemplo, "Mi-app://page2?action=edit"). Es el desarrollador para determinar cómo se controlarán los parámetros URI por su aplicación. Para obtener más información, vea [activación de controlar URI](https://docs.microsoft.com/windows/uwp/launch-resume/handle-uri-activation) .
* Los datos visuales, formado por un conjunto de propiedades opcionales y obligatorios (por ejemplo: título, descripción o elementos de tarjeta adaptable), permitir a los usuarios identificar visualmente una actividad. Vea más adelante para obtener instrucciones sobre la creación de elementos visuales de tarjeta adaptable para la actividad.
* Los metadatos de contenido están datos JSON que se pueden usar para agrupar y recuperar las actividades contenidas en un contexto específico. Normalmente, esto adopta la forma de http://schema.org datos. Vea más adelante para obtener instrucciones sobre rellenar estos datos.

### <a name="adaptive-card-design-guidelines"></a>Instrucciones de diseño de tarjeta adaptables

Cuando las actividades aparecen en la escala de tiempo, se muestran con el [marco de tarjeta adaptable](https://docs.microsoft.com/adaptive-cards/). Si el desarrollador no proporciona una tarjeta adaptable para cada actividad, escala de tiempo creará automáticamente una tarjeta simple basada en el nombre o icono de la aplicación, el campo de título obligatorio y el campo de descripción opcional. 

Animamos a los programadores de la aplicación para proporcionar tarjetas personalizadas mediante el esquema de JSON de tarjeta adaptable simple. Consulte la [documentación de tarjetas adaptable](https://docs.microsoft.com/adaptive-cards/authoring-cards/getting-started) para instrucciones técnicas sobre cómo se crean objetos de tarjeta adaptable. Consulte las instrucciones a continuación para el diseño de tarjetas adaptable en las actividades del usuario.
* Use imágenes
  * Usar una imagen única para cada actividad, si es posible. El nombre de la aplicación y el icono automáticamente se mostrará junto a la tarjeta de la actividad. imágenes adicionales le ayudará a los usuarios a buscar la actividad que están buscando.
  * Imágenes no deben incluir el texto que se espera que el usuario lea. Este texto no estará disponible para los usuarios con necesidades de accesibilidad y no se puede buscar.
  * Si la imagen no contiene texto y se puede recortar sobre una proporción de 2:1, se debe usar como una imagen de fondo. Esto da como resultado una tarjeta de actividad negrita que se destacan en la escala de tiempo. La imagen se va a oscurecer ligeramente para garantizar el texto permanece visible en la tarjeta, y se recomienda usar el nombre de la actividad en este caso, sólo como texto más pequeño puede llegar a ser difícil de leer.
  * Si la imagen no se puede recortar a 2:1, debería poner dentro de la tarjeta de actividad.  
    * Si la relación de aspecto es cuadrada o vertical, fijar la imagen en el lado derecho de la tarjeta sin márgenes.
    * Si la relación de aspecto es horizontal, delimitador de la imagen a la esquina superior derecha de la tarjeta.
* Cada actividad es necesario para proporcionar un nombre de actividad, que siempre se deben mostrar.
  * Este nombre se debe mostrar en la esquina superior izquierda de la tarjeta de uso de la opción de texto negrita de gran tamaño. Es importante que el nombre es fácilmente reconocible, ya que esta es la única parte a los usuarios verán cuando se muestra la actividad en escenarios de Cortana. Que muestra el mismo nombre en la escala de tiempo facilita a los usuarios examinar un gran número de actividades.
* Use el mismo estilo visual para todas las actividades de la aplicación, para que los usuarios puedan encontrar fácilmente las actividades de su aplicación en la escala de tiempo.
  * Por ejemplo, las actividades deben usar el mismo color de fondo.
* Utilice la información de texto complementario con moderación. 
  * Evitar que se llene la tarjeta con texto y usar solo información complementaria que ayuda a los usuarios buscar la actividad derecha o refleja la información de estado (por ejemplo, el progreso actual de una tarea determinada).

### <a name="content-metadata-guidelines"></a>Instrucciones de metadatos de contenido

Actividades de usuario también pueden contener los metadatos de contenido, que Windows y Cortana se usan para clasificar las actividades y generar inferencias. A continuación, se pueden agrupar actividades alrededor de un tema concreto, como una ubicación (si el usuario es a investigar vacaciones), el objeto (si el usuario es investigar algo) o la acción (si el usuario es adquirir un determinado producto a través de los sitios Web y aplicaciones diferentes). Es una buena idea para representar los nombres y los verbos implicados en una actividad. 

En el siguiente ejemplo, los metadatos de contenido JSON, siga los estándares de [Schema.org](https://schema.org/), representan el escenario: "John reproduce pájaros enojados con Steve".

```json
// John played angry birds with Steve.
{
  "@context": "http://schema.org",
  "@type": "PlayAction",
  "agent": {
    "@type": "Person",
    "name": "John"
  },
  "object": {
    "@type": "MobileApplication",
    "name": "Angry Birds."
  },
  "participant": {
    "@type": "Person",
    "name": "Steve"
  }
}
```

## <a name="key-apis"></a>API de claves

* [Espacio de nombres UserActivities](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities)

## <a name="related-topics"></a>Temas relacionados

* [Actividades del usuario (documentos de proyecto Roma)](https://docs.microsoft.com/windows/project-rome/user-activities/)
* [Tarjetas adaptables](https://docs.microsoft.com/adaptive-cards/)
* [Visualizador de tarjetas adaptables, muestras](http://adaptivecards.io/)
* [Administración de la activación de URI](https://docs.microsoft.com/windows/uwp/launch-resume/handle-uri-activation)
* [Interactuar con los clientes en cualquier plataforma con Microsoft Graph, fuentes de actividades y tarjetas adaptables](https://channel9.msdn.com/Events/Connect/2017/B111)
* [Microsoft Graph](https://developer.microsoft.com/graph/)
