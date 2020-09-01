---
title: Procedimientos recomendados de actividades de usuario
description: En esta guía se describen los procedimientos recomendados para crear y actualizar las actividades de los usuarios.
keywords: actividad de usuario, actividades de usuario, escala de tiempo, la recogida de Cortana en la que se quedó, la recogida de Cortana en la que se quedó, Project Roma
ms.date: 08/23/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f4e41ac4f340491e551fccc1c1d4700bbe8d8004
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172959"
---
# <a name="user-activities-best-practices"></a>Procedimientos recomendados de actividades de usuario

En esta guía se describen los procedimientos recomendados para crear y actualizar las actividades de los usuarios. Para obtener información general sobre la característica de actividades de usuario en Windows, consulte [continuar la actividad del usuario, incluso entre dispositivos](./useractivities.md). O bien, consulte la [sección actividades de usuario](/windows/project-rome/user-activities/) de Project Roma para obtener las implementaciones de actividades en otras plataformas de desarrollo.

## <a name="when-to-create-or-update-user-activities"></a>Cuándo crear o actualizar actividades de usuario

Dado que cada aplicación es diferente, depende de cada desarrollador determinar la mejor manera de asignar acciones dentro de la aplicación a las actividades de usuario. Las actividades de usuario se mostrarán en Cortana y escala de tiempo, que se centran en aumentar la productividad y la eficiencia de los usuarios al ayudarles a acceder al contenido que visitó en el pasado.

**Directrices generales**

* **Registra una única actividad para un grupo de acciones de usuario relacionadas.** Esto es especialmente relevante para listas de reproducción de música o programas de TV: una sola actividad se puede actualizar a intervalos regulares para reflejar el progreso del usuario. En este caso, tendrá una actividad de usuario único con varios elementos de historial que representan períodos de compromiso en varios días o semanas. Lo mismo se aplica a las actividades basadas en documentos en las que el usuario realiza un progreso gradual dentro de la aplicación.
* **Almacenar datos de usuario en la nube.** Si desea admitir actividades entre dispositivos, deberá asegurarse de que el contenido necesario para volver a participar en esta actividad se almacena en una ubicación en la nube. Las actividades específicas del dispositivo aparecerán en la escala de tiempo del dispositivo en el que se creó la actividad, pero es posible que no aparezca en otros dispositivos.
* **No cree actividades para las acciones que los usuarios no tendrán que reanudar.** Si la aplicación se usa para completar las operaciones simples de una sola vez que no conservan el estado, probablemente no sea necesario crear una actividad de usuario.
* **No cree actividades para acciones completadas por otros usuarios.** Si una cuenta externa envía al usuario un mensaje o un mensaje @-mentions dentro de la aplicación, no debe crear una actividad para este. Este tipo de acción se atiende mejor con las notificaciones del centro de actividades.
  * Los escenarios de colaboración son una excepción: Si varios usuarios trabajan juntos en la misma actividad (por ejemplo, un documento de Word), habrá casos en los que otro usuario haya realizado cambios después del usuario. En este caso, puede que desee actualizar la actividad existente para reflejar los cambios realizados en el documento. Esto implicaría la actualización de los datos de contenido de la actividad de usuario existente sin crear un nuevo elemento de historial.

**Directrices para tipos específicos de aplicaciones**

Aunque cada aplicación es diferente, la mayoría de las aplicaciones entran en uno de los siguientes patrones de interacción.
* **Aplicaciones basadas en documentos** : cree una actividad por documento, con uno o varios elementos de historial que reflejen los períodos de uso. Es importante actualizar la actividad a medida que se realizan cambios en el documento.
* **Juegos** : cree una actividad para cada juego o todo el mundo. Si el juego solo admite una sola secuencia de niveles, puede volver a publicar la misma actividad a lo largo del tiempo, aunque puede que desee actualizar los datos de contenido para mostrar el progreso o los logros más recientes.
* **Aplicaciones de utilidad** : Si no hay nada dentro de la aplicación que los usuarios deban dejar y reanudar, no es necesario usar actividades de usuario. Un buen ejemplo es una aplicación sencilla como la calculadora.
* **Aplicaciones de línea de negocio** : muchas aplicaciones existen para administrar flujos de trabajo o tareas simples. Cree una actividad para cada flujo de trabajo independiente al que se tiene acceso a través de la aplicación (por ejemplo, los informes de gastos serían una actividad independiente, de modo que el usuario podría hacer clic en una actividad para ver si se aprobó un informe determinado).
* **Aplicaciones de reproducción multimedia** : cree una actividad por cada agrupación lógica de contenido (como una lista de reproducción, un programa o un contenido independiente). La pregunta subyacente para los desarrolladores de aplicaciones es si cada fragmento de contenido (episodio de TV, canción) cuenta como contenido independiente o como parte de una colección. Como norma general, si el usuario opta por reproducir una colección o contenido secuencial, la colección en su totalidad es la actividad. Si optan por reproducir un solo elemento de contenido, entonces un fragmento de contenido es la actividad. Vea instrucciones más específicas a continuación.
  * **Música: álbum/intérprete/género** : Si el usuario selecciona un álbum, un artista o un género y un **juego**de visitas, esa colección es la actividad; no escriba una actividad independiente para cada canción. En el caso de colecciones cortas como un solo álbum o colecciones que se reproducen en un orden aleatorio, es posible que no tenga que actualizar la actividad para reflejar la posición actual del usuario. En el caso de la reproducción secuencial larga, como un álbum o una lista de reproducción, es posible que la grabación de su posición en el álbum tenga sentido.
  * **Música: listas de reproducción inteligentes** : las aplicaciones que reproducen música en orden aleatorio deben registrar una única actividad para la lista de reproducción. Si el usuario reproduce la lista de reproducción por segunda vez, crearía registros de historial adicionales para la misma actividad. No es necesario grabar la posición actual del usuario en la lista de reproducción porque la ordenación es aleatoria.
  * **Serie de TV** : Si la aplicación está configurada para reproducir el episodio siguiente una vez finalizada la actual, debe escribir una única actividad para la serie de TV. Al reproducir los distintos episodios en varias sesiones de visualización, actualizará la actividad para reflejar la posición actual en la serie y se crearán varios registros de historial.
  * **Película** : una película es una sola pieza de contenido y debe tener su propio registro de historial. Si el usuario deja de ver la película a través de, es conveniente grabar su posición. Cuando quiera reanudarlo en el futuro, la actividad podría reanudar la película en la que se dejaron, o incluso preguntar al usuario si desea reanudar o iniciar al principio.

## <a name="user-activity-design"></a>Diseño de la actividad del usuario

Las actividades de usuario constan de tres componentes: un URI de activación, datos visuales y metadatos de contenido.
* El URI de activación es un URI que se puede pasar a una aplicación o experiencia para reanudar la aplicación con un contexto específico. Normalmente, estos vínculos adoptan la forma de controlador de protocolo para un esquema (por ejemplo, "My-App://Page2? action = edit"). Es el desarrollador quien determina cómo controlará la aplicación los parámetros de URI. Para obtener más información, vea [controlar la activación de URI](./handle-uri-activation.md) .
* Los datos visuales, que constan de un conjunto de propiedades obligatorias y opcionales (por ejemplo: título, descripción o elementos de tarjeta adaptativa), permiten a los usuarios identificar visualmente una actividad. Consulte a continuación las instrucciones sobre la creación de objetos visuales de tarjeta adaptable para su actividad.
* Los metadatos de contenido son datos JSON que se pueden usar para agrupar y recuperar actividades en un contexto específico. Normalmente, esta toma la forma de http://schema.org datos. Consulte a continuación las instrucciones para rellenar estos datos.

### <a name="adaptive-card-design-guidelines"></a>Directrices para el diseño de tarjetas adaptables

Cuando las actividades aparecen en la escala de tiempo, se muestran mediante el [marco de trabajo de tarjeta adaptable](/adaptive-cards/). Si el desarrollador no proporciona una tarjeta adaptable para cada actividad, la escala de tiempo creará automáticamente una tarjeta simple basada en el icono o el nombre de la aplicación, el campo de título obligatorio y el campo de descripción opcional. 

Se recomienda a los desarrolladores de aplicaciones que proporcionen tarjetas personalizadas con el sencillo esquema JSON de tarjetas adaptables. Consulte la [documentación sobre tarjetas adaptables](/adaptive-cards/authoring-cards/getting-started) para obtener instrucciones técnicas sobre cómo construir objetos de tarjeta adaptables. Consulte las directrices siguientes para diseñar tarjetas adaptables en actividades de usuario.
* Usar imágenes
  * Use una imagen única para cada actividad, si es posible. El nombre y el icono de la aplicación se mostrarán automáticamente junto a la tarjeta de la actividad. las imágenes adicionales ayudarán a los usuarios a encontrar la actividad que buscan.
  * Las imágenes no deben incluir texto que se espera que lea el usuario. Este texto no estará disponible para los usuarios con necesidades de accesibilidad y no se pueden buscar.
  * Si la imagen no contiene texto y se puede recortar a aproximadamente una relación 2:1, debe utilizarla como imagen de fondo. Esto da como resultado una tarjeta de actividad en negrita que se destacará en la escala de tiempo. La imagen se oscurece ligeramente para asegurarse de que el texto permanece visible en la tarjeta y se recomienda usar solo el nombre de la actividad en este caso, ya que el texto más pequeño puede resultar difícil de leer.
  * Si la imagen no se puede recortar a 2:1, debe colocarla dentro de la tarjeta de actividad.  
    * Si la relación de aspecto es cuadrada o vertical, delimite la imagen en el lado derecho de la tarjeta sin márgenes.
    * Si la relación de aspecto es horizontal, delimite la imagen a la esquina superior derecha de la tarjeta.
* Cada actividad es necesaria para proporcionar un nombre de actividad, que siempre debe mostrarse.
  * Este nombre se debe mostrar en la esquina superior izquierda de la tarjeta con la opción de texto en negrita grande. Es importante que el nombre se pueda reconocer fácilmente, ya que esta es la única parte que verán los usuarios cuando la actividad se muestre en escenarios de Cortana. Mostrar el mismo nombre en la escala de tiempo facilita a los usuarios la exploración de un gran número de actividades.
* Use el mismo estilo visual para todas las actividades de la aplicación, de modo que los usuarios puedan localizar fácilmente las actividades de la aplicación en la escala de tiempo.
  * Por ejemplo, todas las actividades deben usar el mismo color de fondo.
* Utilice la información de texto complementaria con moderación. 
  * Evite llenar la tarjeta con texto y use solo información complementaria que ayude a los usuarios a encontrar la actividad correcta o que refleje la información de estado (por ejemplo, el progreso actual en una tarea determinada).

### <a name="content-metadata-guidelines"></a>Instrucciones de metadatos de contenido

Las actividades de usuario también pueden contener metadatos de contenido, que Windows y Cortana usan para clasificar las actividades y generar inferencias. Las actividades se pueden agrupar en torno a un tema determinado, como una ubicación (si el usuario está investigando vacaciones), el objeto (si el usuario está investigando algo) o una acción (si el usuario está comprando un producto determinado en diferentes aplicaciones y sitios web). Es una buena idea representar tanto los nombres como los verbos implicados en una actividad. 

En el ejemplo siguiente, el JSON de metadatos de contenido, según los estándares de [Schema.org](https://schema.org/), representa el escenario: "John jugó pájaros enfadados con Steve".

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

## <a name="key-apis"></a>API clave

* [Espacio de nombres UserActivities](/uwp/api/windows.applicationmodel.useractivities)

## <a name="related-topics"></a>Temas relacionados

* [Actividades de usuario (documentos de proyecto de Roma)](/windows/project-rome/user-activities/)
* [Tarjetas adaptables](/adaptive-cards/)
* [Visualizador de tarjetas adaptables, ejemplos](https://adaptivecards.io/)
* [Controlar la activación de URI](./handle-uri-activation.md)
* [Interacción con sus clientes en cualquier plataforma mediante el Microsoft Graph, la fuente de actividades y las tarjetas adaptables](https://channel9.msdn.com/Events/Connect/2017/B111)
* [Microsoft Graph](https://developer.microsoft.com/graph)