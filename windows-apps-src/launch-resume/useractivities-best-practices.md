---
author: PatrickFarley
title: Mejores prácticas de las actividades del usuario
description: Esta guía describe los procedimientos recomendados para la creación y actualización de las actividades del usuario.
keywords: actividad del usuario, actividades del usuario, línea de tiempo, cortana continúa donde lo dejaste, cortana continúa desde donde lo dejé, proyecto rome
ms.author: pafarley
ms.date: 08/23/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: b11151df981a4b5ce233458581d21e405be9844c
ms.sourcegitcommit: 3727445c1d6374401b867c78e4ff8b07d92b7adc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2018
ms.locfileid: "2917576"
---
# <a name="user-activities-best-practices"></a>Mejores prácticas de las actividades del usuario

Esta guía describe los procedimientos recomendados para la creación y actualización de las actividades del usuario. Para obtener una introducción de la característica de las actividades de usuario en Windows, consulte [continuar actividad del usuario, incluso a través de dispositivos](https://docs.microsoft.com/windows/uwp/launch-resume/useractivities). O bien, consulte la [sección actividades de usuario](https://docs.microsoft.com/windows/project-rome/user-activities/) del proyecto Roma para las implementaciones de las actividades en otras plataformas de desarrollo.

## <a name="when-to-create-or-update-user-activities"></a>Cuándo se debe crear o actualizar las actividades del usuario

Dado que cada aplicación es diferente, depende de cada desarrollador para determinar la mejor forma de asignar acciones dentro de la aplicación a las actividades del usuario. Las actividades del usuario se mostrarán en Cortana y escala de tiempo, que se centran en la eficiencia y la productividad de los usuarios de aumento al ayudarles a volver a contenido que lo visitó en el pasado.

**Directrices generales**

* **Grabar una sola actividad para un grupo de acciones de usuario relacionada.** Esto es especialmente importante para las listas de reproducción de música o programas de TV: una sola actividad puede actualizarse periódicamente para reflejar el progreso del usuario. En este caso, tendrá una sola actividad de usuario con varios elementos de historial que representan períodos de contratación en varios días o semanas. Lo mismo se aplica a actividades basadas en documentos en la que el usuario realiza gradual progreso dentro de la aplicación.
* **Almacenar datos de usuario en la nube.** Si desea admitir actividades multidispositivo, debe asegurarse de que el contenido que se requieren para volver a activar esta actividad se almacena en una ubicación de la nube. Actividades específicas de dispositivo aparecerán en la línea de tiempo en el dispositivo donde se creó la actividad pero no aparezcan en otros dispositivos.
* **No crear actividades para las acciones que los usuarios no necesitarán para reanudar.** Si la aplicación se utiliza para realizar operaciones simples, una sola vez que no se conserva el estado, probablemente no necesitará crear una actividad de usuario.
* **No crear actividades para acciones completadas por otros usuarios.** Si una cuenta externa envía al usuario un mensaje o @-mentions ellos dentro de la aplicación, no debería crear una actividad para esto. Este tipo de acción es preferible acción centro de notificaciones.
  * Escenarios de colaboración son una excepción: si varios usuarios trabajan en la misma actividad juntos (por ejemplo, un documento de Word), habrá casos en los que otro usuario ha realizado cambios después de que el usuario. En este caso, desea actualizar la actividad existente para reflejar los cambios realizados en el documento. Esto implicaría que actualizar los datos de contenido existentes de actividad de los usuarios sin crear un nuevo elemento de historial.

**Instrucciones para tipos específicos de aplicaciones**

Aunque cada aplicación es diferente, mayoría de las aplicaciones se inscriben en uno de los siguientes patrones de interacción.
* **Aplicaciones basadas en el documento** , crear una actividad por cada documento, con uno o más elementos del historial de reflejar periodos de uso. Es importante actualizar la actividad cuando se realizan cambios en el documento.
* **Juegos** , crear una actividad para cada juego guardar o mundial. Si el juego admite sólo una única secuencia de niveles, puede publicar vuelve a la misma actividad con el tiempo, aunque puede que desee actualizar los datos del contenido para mostrar el progreso o los logros más recientes.
* **Aplicaciones de utilidad** , si no hay nada dentro de la aplicación que los usuarios tendrían que dejar y reanudar, no es necesario usar las actividades del usuario. Un buen ejemplo es una aplicación sencilla como calculadora.
* **Aplicaciones de línea de negocio** , existen muchas aplicaciones para administrar flujos de trabajo o tareas sencillas. Crear una actividad para cada flujo de trabajo independiente que tiene acceso a través de la aplicación (por ejemplo, informes de gastos sería cada uno una actividad independiente, por lo que el usuario pudo haga clic en una actividad para ver si se ha aprobado un informe en particular).
* **Aplicaciones de reproducción multimedia** , crear una actividad por la agrupación lógica de contenido (por ejemplo, un contenido independiente, programa o lista de reproducción). La pregunta para los desarrolladores de la aplicación subyacente es si una cada pieza de contenido (episodio de TV, canción) se cuenta como contenido independiente o como parte de una colección. Como regla general, si el usuario opta por reproducir una colección o contenido secuencial, la colección como un todo es la actividad. Si optan por reproducir un solo elemento de contenido, que una parte del contenido es la actividad. Consulte las instrucciones más específicas siguientes.
  * **Música: intérprete/álbum o género** : si el usuario selecciona un álbum, el artista o el género y llega a **jugar**, esa colección es la actividad; No escriba una actividad independiente para cada canción. Para colecciones corto como un álbum o colecciones que se vuelvan a reproducir en un orden aleatorio, no necesitará actualizar la actividad para reflejar la posición del usuario actual. Para la reproducción larga secuencial como un álbum o una lista de reproducción, grabación de posición en el álbum podría tener sentido.
  * **Música: listas de reproducción de smart** : aplicaciones que reproducirán música en orden aleatorio deben registrar una actividad única para esa lista de reproducción. Si el usuario reproduce la lista de reproducción en una segunda vez, debe crear registros históricos adicionales para la misma actividad. La posición del usuario actual en la lista de reproducción de la grabación no es necesario porque el orden es aleatorio.
  * **Serie de TV** , si su aplicación está configurada para reproducir el episodio siguiente una vez finalizada la actual, debe escribir una sola actividad para la serie de TV. Como reproducir los episodios distintos a través de varias sesiones de visualización, actualizaremos tu actividad para reflejar la posición actual en la serie y se crean varios registros de historial.
  * **Película** : una película es una parte única de contenido y debe tener su propio registro de historial. Si el usuario deja viendo la forma parte de película a través de, es conveniente registrar su posición. Cuando desea reanudar la reproducción en el futuro, la actividad pudo reanudar la película donde lo dejaron o incluso preguntar al usuario si desea reanudar o comenzar desde el principio.

## <a name="user-activity-design"></a>Diseño de la actividad de usuario

Actividades de usuario constan de tres componentes: una activación URI, visuales datos y metadatos de contenido.
* La activación de URI es un identificador URI que se puede pasar a una aplicación o experiencia para poder reanudar la aplicación con un contexto específico. Normalmente, estos vínculos tienen la forma de controlador de protocolo para un esquema (por ejemplo, "Mis-app://page2?action=edit"). Es el desarrollador para determinar cómo se tratarán los parámetros URI por su aplicación. Para obtener más información, vea [URI controlar la activación](https://docs.microsoft.com/windows/uwp/launch-resume/handle-uri-activation) .
* Los datos visuales, que consta de un conjunto de propiedades obligatorias y opcionales (por ejemplo: título, descripción o elementos de tarjeta adaptable), permiten a los usuarios identificar visualmente una actividad. Consulte a continuación para obtener instrucciones sobre cómo crear elementos visuales tarjeta adaptable para la actividad.
* El contenido de los metadatos es datos JSON que pueden utilizarse para agrupar y recuperar las actividades en un contexto específico. Normalmente, esto adopta la forma de http://schema.org datos. Consulte a continuación para obtener instrucciones sobre rellenar estos datos.

### <a name="adaptive-card-design-guidelines"></a>Instrucciones de diseño de tarjeta adaptables

Cuando las actividades aparecen en la línea de tiempo, se muestran utilizando el [marco de tarjeta adaptable](https://docs.microsoft.com/adaptive-cards/). Si el desarrollador no proporciona una tarjeta adaptable para cada actividad, escala de tiempo creará automáticamente una tarjeta simple basada en el nombre o icono de la aplicación, el campo de título obligatorio y el campo de descripción opcional. 

Animamos a los programadores de la aplicación para proporcionar tarjetas personalizadas mediante el esquema de adaptación JSON de tarjeta simple. Consulte la [documentación de tarjetas adaptable](https://docs.microsoft.com/adaptive-cards/authoring-cards/getting-started) para instrucciones técnicas sobre cómo construir objetos tarjeta adaptable. Consulte las instrucciones a continuación para diseñar tarjetas de adaptación en las actividades del usuario.
* Utilizar imágenes
  * Utilice una imagen única para cada actividad, si es posible. El icono y el nombre de la aplicación automáticamente se mostrará junto a la tarjeta de la actividad; imágenes adicionales ayudará a los usuarios a encontrar la actividad que están buscando.
  * Imágenes no deben incluir texto que el usuario va a leer. Este texto no estará disponible para los usuarios con necesidades de accesibilidad y no se puede buscar.
  * Si la imagen no contiene texto y se puede recortar sobre una proporción de 2:1, se debe utilizar como imagen de fondo. Esto resulta en una tarjeta de actividad negrita que destaque en la escala de tiempo. La imagen se oscurecerá ligeramente para asegurar el texto permanece visible en la tarjeta y se recomienda utilizar el nombre de la actividad en este caso, sólo como texto más pequeño puede convertirse en difícil de leer.
  * Si la imagen no se puede recortar a 2:1, se debe colocar dentro de la ficha de actividad.  
    * Si la proporción es cuadrada, vertical, fijar la imagen en el lado derecho de la tarjeta sin márgenes.
    * Si la proporción es horizontal, fijar la imagen a la esquina superior derecha de la tarjeta.
* Cada actividad debe proporcionar un nombre de actividad, siempre se debe mostrar.
  * Este nombre se visualizará en la esquina superior izquierda de la tarjeta con la opción negrita de gran tamaño. Es importante que el nombre sea fácilmente reconocible, ya que ésta es la única parte usuarios verán cuando se muestra la actividad en los escenarios de Cortana. Mostrando el mismo nombre en la línea de tiempo facilita a los usuarios examinar un gran número de actividades.
* Utilizar el mismo estilo visual de todas las actividades de la aplicación, para que los usuarios puedan localizar fácilmente las actividades de la aplicación en la línea de tiempo.
  * Por ejemplo, las actividades deben utilizar el mismo color de fondo.
* Utilice la información de texto complementario con moderación. 
  * Evitar que se llene la tarjeta con el texto y utilice sólo la información complementaria que ayuda a los usuarios buscar la actividad derecho o refleja la información de estado (como el progreso actual de una tarea determinada).

### <a name="content-metadata-guidelines"></a>Pautas de contenido de metadatos

Actividades de los usuarios también pueden contener metadatos de contenido, Windows y Cortana se utilizan para categorizar las actividades y generar inferencias. A continuación, se pueden agrupar actividades alrededor de un tema concreto, como una ubicación (si el usuario está investigando vacaciones), el objeto (si el usuario está investigando algo) o la acción (si el usuario es compras para un producto determinado a través de sitios Web y aplicaciones diferentes). Es una buena idea para representar los nombres y los verbos implicada en una actividad. 

En el siguiente ejemplo, los metadatos de contenido JSON, siguiendo las normas de [Schema.org](https://schema.org/), representan el escenario: "Juan juega aves enojado con Steve".

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
