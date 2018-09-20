---
author: PatrickFarley
title: Procedimientos recomendados de las actividades de usuario
description: En esta guía se describe los procedimientos recomendados para crear y actualizar las actividades del usuario.
keywords: actividad del usuario, actividades del usuario, línea de tiempo, cortana continúa donde lo dejaste, cortana continúa desde donde lo dejé, proyecto rome
ms.author: pafarley
ms.date: 08/23/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: b11151df981a4b5ce233458581d21e405be9844c
ms.sourcegitcommit: 4f6dc806229a8226894c55ceb6d6eab391ec8ab6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/20/2018
ms.locfileid: "4083226"
---
# <a name="user-activities-best-practices"></a>Procedimientos recomendados de las actividades de usuario

En esta guía se describe los procedimientos recomendados para crear y actualizar las actividades del usuario. Para obtener información general de la característica de actividades del usuario en Windows, vea [la actividad de usuario continuar, incluso en diferentes dispositivos](https://docs.microsoft.com/windows/uwp/launch-resume/useractivities). O bien, consulte la [sección de actividades del usuario](https://docs.microsoft.com/windows/project-rome/user-activities/) de Project Rome para las implementaciones de actividades en otras plataformas de desarrollo.

## <a name="when-to-create-or-update-user-activities"></a>Cuándo se debe crear o actualizar las actividades del usuario

Dado que cada aplicación es diferente, depende de cada desarrollador para determinar la mejor manera de asignar acciones dentro de la aplicación a las actividades del usuario. Las actividades de usuario se mostrarán en Cortana y la escala de tiempo, que se centra en la eficacia y la productividad de los usuarios creciente y te ayuda a volver a contenido que ha visitado en el pasado.

**Directrices generales**

* **Registra una sola actividad para un grupo de usuario relacionada con acciones.** Esto es especialmente importante para listas de reproducción de música o de TV: una actividad solo puede actualizarse a intervalos regulares para reflejar el progreso del usuario. En este caso, tendrás una actividad de usuario único con varios elementos de historial que representan los períodos de participación en varios días o semanas. Lo mismo se aplica a las actividades de documento basado en el que el usuario realiza gradual progreso dentro de la aplicación.
* **Almacenar datos de usuario en la nube.** Si quieres admitir actividades entre dispositivos, debes asegurarte de que el contenido que se requieren para volver a activar esta actividad se almacena en una ubicación en la nube. Actividades específicas de dispositivo aparecerán en la línea de tiempo en el dispositivo donde se creó la actividad, pero podrían no aparecer en otros dispositivos.
* **¿Crear actividades para las acciones que los usuarios no necesitan que se reanude.** Si la aplicación se usa para completar operaciones simples única que no se conserva el estado, probablemente no es necesario crear una actividad de usuario.
* **No se crea actividades para acciones completadas por otros usuarios.** Si una cuenta externa envía al usuario un mensaje o @-mentions ellos dentro de la aplicación, no se debe crear una actividad para esto. Este tipo de acción es preferible las notificaciones del centro de actividades.
  * Escenarios de colaboración son una excepción: si varios usuarios están trabajando en la misma actividad juntos (por ejemplo, un documento de Word), habrá casos en los que otro usuario ha realizado cambios después de que el usuario. En este caso, puedes actualizar la actividad existente para reflejar los cambios que han hechos en el documento. Esto incluiría la actualización de los datos de contenido de actividad del usuario existentes sin tener que crear un nuevo elemento de historial.

**Directrices para determinados tipos de aplicaciones**

Aunque todas las aplicaciones son diferente, la mayoría de las aplicaciones coinciden con uno de los siguientes patrones de interacción.
* **Aplicaciones basadas en el documento** : crear una actividad por documento, con uno o más elementos de historial que refleja períodos de uso. Es importante para la actividad de actualización que se realizan cambios en el documento.
* **Juegos** : crear una actividad para cada juego guardar o el mundo. Si el juego admite solo una sola secuencia de niveles, puedes volver a publicar la misma actividad con el tiempo, aunque es posible que desees actualizar los datos de contenido para mostrar el progreso o los logros más recientes.
* **Las aplicaciones de utilidad** : si no hay nada dentro de la aplicación que los usuarios tendrían que deje y reanudar, no es necesario usar las actividades del usuario. Un buen ejemplo es una aplicación sencilla como calculadora.
* **Las aplicaciones de línea de negocio** , existen muchas aplicaciones para administrar tareas sencillas o flujos de trabajo. Crear una actividad para cada flujo de trabajo diferente al que se accede a través de la aplicación (por ejemplo, informes de gastos sería cada una actividad independiente, a continuación, el usuario podría hacer clic en una actividad para ver si se aprobó un informe en particular).
* **Las aplicaciones de reproducción de multimedia** : crear una actividad por una agrupación lógica de contenido (por ejemplo, un contenido de la lista de reproducción, programa o independiente). La pregunta subyacente para los desarrolladores de aplicaciones es si una cada parte del contenido (episodio TV, canción) se cuenta como contenido independiente o como parte de una colección. Como regla general, si el usuario opta por reproducir una colección o contenido secuencial, la colección como un todo es la actividad. Si optan para reproducir un solo elemento de contenido, que una parte del contenido es la actividad. Consulta las directrices más específicas a continuación.
  * **Música: álbum o intérprete o género** : si el usuario selecciona un álbum, el artista o el género y alcanza **Reproducir**, dicha colección es la actividad. no escribir una actividad independiente para cada canción. Para colecciones breves como un álbum o colecciones que se reproducen en orden aleatorio, no tengas que actualizar la actividad para reflejar la posición actual del usuario. Para la reproducción larga secuencial como un álbum o una lista de reproducción, grabación su posición dentro del álbum tendría sentido.
  * **Música: inteligente, listas de reproducción** : las aplicaciones que reproducen música en orden aleatorio deben registrar una actividad única para esa lista de reproducción. Si el usuario reproduce la lista de reproducción una segunda vez, debe crear registros de historial adicionales para la misma actividad. Grabación de la posición del usuario actual en la lista de reproducción no es necesaria porque la ordenación es aleatoria.
  * **Serie de TV** : si la aplicación está configurada para reproducir el episodio siguiente una vez completada la actual, debes escribir una sola actividad para la serie de TV. Como los diversos episodios se reproducción varias sesiones de visualización, actualizaremos tu actividad para reflejar la posición actual en la serie y se creará varios registros de historial.
  * **Películas** : una película es un solo elemento de contenido y debe tener su propio registro del historial. Si el usuario deja de supervisión de la forma parte de películas a través de, es conveniente para registrar su posición. Cuando quieran reanudarlo en el futuro, la actividad podría reanudar la película donde la dejó o incluso pedir al usuario si desea reanudar o iniciar al principio.

## <a name="user-activity-design"></a>Diseño de la actividad de usuario

Actividades del usuario constan de tres componentes: una activación de URI, visual datos y metadatos de contenido.
* La activación de URI es un URI que se puede pasar a una aplicación o la experiencia con el fin de reanudar la aplicación con un contexto específico. Por lo general, estos vínculos adoptan la forma de controlador de protocolo para un esquema (por ejemplo, "my-app://page2?action=edit"). Es el desarrollador para determinar cómo su aplicación administra los parámetros URI. Para obtener más información, consulta [la activación de URI de controlar](https://docs.microsoft.com/windows/uwp/launch-resume/handle-uri-activation) .
* Los datos visuales, que consta de un conjunto de propiedades necesarias y opcionales (por ejemplo: título, una descripción o elementos de la tarjeta adaptable), permitir que los usuarios identificar visualmente una actividad. Consulta a continuación para obtener instrucciones sobre cómo crear elementos visuales de tarjeta adaptable para la actividad.
* Los metadatos de contenido están datos de JSON que pueden usarse para agrupar y recuperar las actividades en un contexto específico. Por lo general, esto adopta la forma de http://schema.org datos. Consulta a continuación para obtener instrucciones sobre rellenar estos datos.

### <a name="adaptive-card-design-guidelines"></a>Directrices de diseño de tarjeta adaptables

Cuando las actividades aparecen en la línea de tiempo, se muestran con el [marco de tarjeta adaptable](https://docs.microsoft.com/adaptive-cards/). Si el desarrollador no proporciona una tarjeta adaptable para cada actividad, línea de tiempo creará automáticamente una tarjeta sencilla basada en el nombre de aplicación/icono, el campo de título obligatorio y el campo de descripción opcional. 

Los desarrolladores de aplicaciones se recomienda proporcionar tarjetas personalizadas mediante el esquema de JSON de tarjeta adaptable simple. Consulta la [documentación de tarjetas adaptables](https://docs.microsoft.com/adaptive-cards/authoring-cards/getting-started) de instrucciones técnicas sobre cómo crear objetos de tarjeta adaptable. Consulta las directrices a continuación para diseñar tarjetas adaptables en actividades del usuario.
* Usar imágenes
  * Usar una imagen única para cada actividad, si es posible. El nombre de la aplicación y el icono se mostrará automáticamente junto a la tarjeta de la actividad. imágenes adicionales te ayudará a los usuarios a localizar la actividad que buscan.
  * Las imágenes no deben incluir el texto que se espera que el usuario leer. Este texto no estará disponible para los usuarios con necesidades de accesibilidad y no se puede buscar.
  * Si la imagen no contiene texto y se puede recortar sobre una relación de 2:1, debería usarlo como una imagen de fondo. Esto da como resultado una tarjeta de actividad negrita que destaque en línea de tiempo. La imagen se oscurece ligeramente para garantizar que el texto permanece visible en la tarjeta y se recomienda usar el nombre de la actividad en este caso, solo como texto más pequeño que puede resultar difícil de leer.
  * Si no se recorte la imagen a 2:1, se debe colocar dentro de la tarjeta de actividad.  
    * Si la relación de aspecto es cuadrada, vertical, anclar la imagen en el lado derecho de la tarjeta sin márgenes.
    * Si la relación de aspecto es horizontal, la imagen en la esquina superior derecha de la tarjeta de anclaje.
* Cada actividad es necesaria para proporcionar un nombre de la actividad, que siempre se debe mostrar.
  * Este nombre debe mostrarse en la esquina superior izquierda de la tarjeta con la opción de texto en negrita grandes. Es importante que el nombre es fácilmente reconocible, ya que esto es la única parte a los usuarios verán cuando se muestre la actividad en escenarios de Cortana. Que muestra el mismo nombre en la línea de tiempo facilita a los usuarios explorar un gran número de actividades.
* Usar el mismo estilo visual para todas las actividades de la aplicación, para que los usuarios puedan encontrar fácilmente las actividades de la aplicación en la línea de tiempo.
  * Por ejemplo, las actividades deben usar el mismo color de fondo.
* Usa información de texto complementario con moderación. 
  * Evitar la tarjeta se llene con texto y solo usa información complementaria que ayuda a los usuarios en la búsqueda de la actividad derecha o refleja la información de estado (por ejemplo, el progreso actual en una tarea particular).

### <a name="content-metadata-guidelines"></a>Directrices de metadatos de contenido

Actividades del usuario también pueden contener metadatos de contenido, Windows y Cortana se usan para clasificar actividades y generar inferencias. A continuación, se pueden agrupar las actividades alrededor de un tema particular, como una ubicación (si el usuario está investigando vacaciones), el objeto (si el usuario está investigando algo) o la acción (si el usuario es comprar un producto concreto en todos los sitios Web y aplicaciones diferentes). Es una buena idea para representar los nombres y los verbos implicados en una actividad. 

En el siguiente ejemplo, los metadatos de contenido JSON, siguiendo los estándares de [Schema.org](https://schema.org/), representan el escenario: "John reproduce aves queja con Steve."

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

* [Actividades del usuario (documentos de Project Rome)](https://docs.microsoft.com/windows/project-rome/user-activities/)
* [Tarjetas adaptables](https://docs.microsoft.com/adaptive-cards/)
* [Visualizador de tarjetas adaptables, muestras](http://adaptivecards.io/)
* [Administración de la activación de URI](https://docs.microsoft.com/windows/uwp/launch-resume/handle-uri-activation)
* [Interactuar con los clientes en cualquier plataforma con Microsoft Graph, fuentes de actividades y tarjetas adaptables](https://channel9.msdn.com/Events/Connect/2017/B111)
* [Microsoft Graph](https://developer.microsoft.com/graph/)
