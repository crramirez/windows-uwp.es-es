---
title: Procedimientos recomendados de las actividades de usuario
description: Esta guía describe los procedimientos recomendados para crear y actualizar las actividades del usuario.
keywords: actividad del usuario, actividades del usuario, línea de tiempo, cortana continúa donde lo dejaste, cortana continúa desde donde lo dejé, proyecto rome
ms.date: 08/23/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e98f5d73cf2d1afb26a823ed417c8980d118485c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589910"
---
# <a name="user-activities-best-practices"></a>Procedimientos recomendados de las actividades de usuario

Esta guía describe los procedimientos recomendados para crear y actualizar las actividades del usuario. Para obtener información general de la característica de las actividades del usuario en Windows, consulte [continuar con la actividad del usuario, incluso en dispositivos](https://docs.microsoft.com/windows/uwp/launch-resume/useractivities). O bien, consulte el [sección de actividades de usuario](https://docs.microsoft.com/windows/project-rome/user-activities/) de Roma de proyecto para las implementaciones de las actividades en otras plataformas de desarrollo.

## <a name="when-to-create-or-update-user-activities"></a>Cuándo se debe crear o actualizar las actividades del usuario

Dado que cada aplicación es diferente, depende de cada desarrollador para determinar la mejor manera de asignar acciones dentro de la aplicación a las actividades del usuario. Se mostrarán las actividades de usuario en Cortana y escala de tiempo, que se centran en la eficacia y la productividad de aumento de los usuarios al permitirles regresar al contenido que visitaban en el pasado.

**Directrices generales**

* **Registre una sola actividad para un grupo de acciones de usuario relacionada.** Esto es especialmente relevante para las listas de reproducción de música o programas de TV: una actividad solo se puede actualizar a intervalos regulares para reflejar el progreso del usuario. En este caso, tendrá una única actividad de usuario con varios elementos de historial que representan los períodos de engagement a través de varios días o semanas. Lo mismo se aplica a las actividades de documento basado en el que el usuario realiza progresos gradual dentro de la aplicación.
* **Store los datos de usuario en la nube.** Si desea admitir las actividades entre dispositivos, deberá asegurarse de que el contenido necesario para volver a interactuar con esta actividad se almacena en una ubicación en la nube. Las actividades específicas del dispositivo aparecerán en la escala de tiempo en el dispositivo donde se creó la actividad, pero no pueden aparecer en otros dispositivos.
* **No cree actividades para las acciones que los usuarios no tendrá que reanudar.** Si la aplicación se usa para completar las operaciones simples, un solo uso que no se conserva el estado, probablemente no es necesario crear una actividad de usuario.
* **No cree actividades para acciones completadas por otros usuarios.** Si una cuenta externa envía al usuario un mensaje o @-mentions ellos dentro de la aplicación, no debería crear una actividad para este. Este tipo de acción se obtienen mejores resultado notificaciones del centro de actividades.
  * Escenarios de colaboración son una excepción: Si varios usuarios trabajan en la misma actividad juntos (por ejemplo, un documento de Word), habrá casos en que otro usuario ha realizado cambios después de que el usuario. En este caso, desea actualizar la actividad existente para reflejar los cambios realizados en el documento. Esto implicaría que actualizar los datos de contenido de la actividad de usuario existentes sin necesidad de crear un nuevo elemento de historial.

**Directrices para tipos específicos de aplicaciones**

Aunque cada aplicación es diferente, la mayoría de las aplicaciones entrarán dentro de uno de los siguientes patrones de interacción.
* **Las aplicaciones basadas en documento** : creación de una actividad por documento, con uno o varios elementos de historial que refleja los períodos de uso. Es importante actualizar la actividad que se realizan cambios en el documento.
* **Juegos** : crear una actividad para cada juego guardado o el mundo. Si su juego admite solo una única secuencia de niveles, se puede volver a publicar la misma actividad con el tiempo, aunque puede que desee actualizar los datos de contenido para mostrar el progreso o los logros más reciente.
* **Las aplicaciones de utilidad** : si no hay nada dentro de la aplicación que los usuarios tendrían que salir y reanudar, no es necesario usar las actividades del usuario. Un buen ejemplo es una sencilla aplicación como la calculadora.
* **Aplicaciones de línea de negocio** , existen muchas aplicaciones para administrar tareas sencillas o flujos de trabajo. Crear una actividad para cada flujo de trabajo independiente que se tiene acceso a través de la aplicación (por ejemplo, informes de gastos sería cada uno una actividad independiente, para que el usuario, a continuación, hacer clic en una actividad para ver si se ha aprobado un informe determinado).
* **Aplicaciones de reproducción de multimedia** : crear una actividad por una agrupación lógica de contenido (por ejemplo, un contenido de la lista de reproducción, programa o independiente). La pregunta subyacente para los desarrolladores de aplicaciones es si una cada parte del contenido (episodio de televisión, canción) se considera contenido independiente o parte de una colección. Como norma general, si el usuario opta por reproducir una colección o contenido secuencial, la colección como un todo es la actividad. Si deciden contratar para reproducir un solo elemento de contenido, que una parte del contenido es la actividad. Consulte instrucciones más específicas a continuación.
  * **Música: Álbum, intérprete o género** : si el usuario selecciona un álbum, artista, o género y aciertos **reproducir**, esa colección es la actividad; ¿escribe una actividad independiente para cada canción. Para colecciones corto como un álbum o colecciones que se reproduce en orden aleatorio, es posible que no deberá actualizar la actividad para reflejar la posición actual del usuario. Para la reproducción larga secuencial como un álbum o una lista de reproducción, grabación de posición en el álbum podría tener sentido.
  * **Música: las listas de reproducción smart** : aplicaciones que reproducirán música en orden aleatorio deben registrar una sola actividad para esa lista de reproducción. Si el usuario desempeña la lista de reproducción de una segunda vez, debe crear registros de historial adicionales para la misma actividad. Grabación de la posición del usuario actual en la lista de reproducción no es necesario porque la ordenación es aleatoria.
  * **Serie de TV** : si la aplicación está configurada para reproducir el episodio siguiente una vez completada la actual, debe escribir una sola actividad de la serie de TV. Cuando reproduzca los varios episodios en varias sesiones de visualización, se actualizará la actividad para reflejar la posición actual de la serie y se crearán varios registros de historial.
  * **Película** — una película es un único fragmento de contenido y debe tener su propio registro de historial. Si el usuario detiene viendo la película parte-recorrido, es deseable para registrar su posición. Cuando desea reanudar la reproducción en el futuro, la actividad podría reanudar la película donde lo dejó o pida al usuario incluso si desea reanudar o comience desde el principio.

## <a name="user-activity-design"></a>Diseño de la actividad de usuario

Las actividades de usuario constan de tres componentes: un URI de activación, datos visuales y metadatos de contenido.
* La activación URI es un URI que se puede pasar a una aplicación o la experiencia para reanudar la aplicación con un contexto específico. Normalmente, estos vínculos adoptan la forma del controlador de protocolo para un esquema (por ejemplo, "Mi-app://page2?action=edit"). Es el desarrollador para determinar cómo se controlarán los parámetros de URI por su aplicación. Consulte [activación controlar URI](https://docs.microsoft.com/windows/uwp/launch-resume/handle-uri-activation) para obtener más información.
* Los datos visuales, que consta de un conjunto de propiedades obligatorias y opcionales (por ejemplo: elementos de la tarjeta adaptable, la descripción o título), permiten a los usuarios identificar visualmente una actividad. Consulte a continuación para obtener instrucciones sobre cómo crear objetos visuales de tarjeta adaptable para su actividad.
* Los metadatos de contenido son datos JSON que pueden utilizarse para agrupar y recuperar las actividades bajo un contexto específico. Normalmente, esto adopta la forma de http://schema.org datos. Consulte a continuación para obtener instrucciones sobre rellenar estos datos.

### <a name="adaptive-card-design-guidelines"></a>Instrucciones de diseño de tarjeta adaptables

Cuando las actividades aparecen en la escala de tiempo, se muestran mediante el [framework tarjeta adaptable](https://docs.microsoft.com/adaptive-cards/). Si el desarrollador no proporciona una tarjeta adaptable para cada actividad, escala de tiempo creará automáticamente una tarjeta simple basada en el nombre e iconos de aplicación, el campo obligatorio de título y el campo de descripción opcional. 

Los desarrolladores de aplicaciones se recomienda proporcionar tarjetas personalizadas mediante el esquema de JSON de tarjeta adaptable simple. Consulte la [documentación de las tarjetas adaptables](https://docs.microsoft.com/adaptive-cards/authoring-cards/getting-started) para obtener instrucciones técnicas sobre cómo construir objetos de la tarjeta adaptable. Consulte las instrucciones a continuación para diseñar las tarjetas adaptables en las actividades del usuario.
* Use imágenes
  * Usar una imagen única para cada actividad, si es posible. El nombre de la aplicación y un icono automáticamente se mostrarán junto a la tarjeta de la actividad. imágenes adicionales permiten que los usuarios localizar la actividad que están buscando.
  * Las imágenes no deben incluir el texto que se espera que el usuario para leer. Este texto no estará disponible para los usuarios con necesidades de accesibilidad y no se puede buscar.
  * Si la imagen no contiene texto y se puede recortar sobre una relación de 2:1, debe utilizarlo como una imagen de fondo. Esto da como resultado una tarjeta de actividad en negrita que destaque en escala de tiempo. La imagen se se oscurecerá ligeramente para garantizar el texto permanece visible en la tarjeta y se recomienda usar solo el nombre de actividad en este caso, como texto más pequeño puede ser difícil de leer.
  * Si no se puede recortar la imagen a 2:1, debe colocarlo dentro de la tarjeta de actividad.  
    * Si la relación de aspecto es cuadrada, vertical, delimite la imagen en el lado derecho de la tarjeta sin márgenes.
    * Si la relación de aspecto es horizontal, delimite la imagen a la esquina superior derecha de la tarjeta.
* Cada actividad debe proporcionar un nombre de actividad, que siempre se deben mostrar.
  * Este nombre debe mostrarse en la esquina superior izquierda de la tarjeta mediante la opción de gran tamaño de texto en negrita. Es importante que el nombre sea fácilmente reconocible, ya que esta es la única parte a los usuarios verán cuando se muestra la actividad en escenarios de Cortana. Que muestra el mismo nombre en la escala de tiempo facilita a los usuarios busquen un gran número de actividades.
* Use el mismo estilo visual para todas las actividades de la aplicación, para que los usuarios puedan encontrar fácilmente las actividades de la aplicación en la escala de tiempo.
  * Por ejemplo, las actividades deben utilizar el mismo color de fondo.
* Utilice la información de texto complementario con moderación. 
  * Evitar que se llene la tarjeta con texto y usar solo información complementaria que ayuda a los usuarios en la búsqueda de la actividad adecuada o refleja la información de estado (por ejemplo, el progreso actual en una tarea en particular).

### <a name="content-metadata-guidelines"></a>Directrices de metadatos de contenido

Las actividades de usuario también pueden contener metadatos de contenido, Windows y Cortana se usan para categorizar las actividades y generar inferencias. A continuación, se pueden agrupar actividades en torno a un tema en particular, como una ubicación (si el usuario trata de investigar vacaciones), el objeto (si el usuario trata de investigar algo) o la acción (si el usuario es todo lo necesario para un determinado producto a través de diferentes aplicaciones y sitios Web). Es una buena idea para representar los nombres y los verbos implicada en una actividad. 

En el ejemplo siguiente, los metadatos de contenido JSON, siguiendo los estándares de [Schema.org](https://schema.org/), representa el escenario: "Juan reproduce pájaros enfadado con Steve".

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

* [Actividades del usuario (docs Roma del proyecto)](https://docs.microsoft.com/windows/project-rome/user-activities/)
* [Tarjetas adaptables](https://docs.microsoft.com/adaptive-cards/)
* [Visualizador de las tarjetas adaptables, ejemplos](https://adaptivecards.io/)
* [Controlar la activación de URI](https://docs.microsoft.com/windows/uwp/launch-resume/handle-uri-activation)
* [Interactuar con sus clientes en cualquier plataforma mediante Microsoft Graph, fuente de actividades y las tarjetas adaptables](https://channel9.msdn.com/Events/Connect/2017/B111)
* [Microsoft Graph](https://developer.microsoft.com/graph/)
