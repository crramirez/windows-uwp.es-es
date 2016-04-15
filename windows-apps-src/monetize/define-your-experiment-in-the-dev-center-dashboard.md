---
Description: Antes de poder ejecutar un experimento en tu aplicación de la Plataforma universal de Windows (UWP) con pruebas A/B, debes definir tu experimento en el panel del Centro de desarrollo.
title: Define tu experimento en el panel del Centro de desarrollo
ms.assetid: 675F2ADE-0D4B-41EB-AA4E-56B9C8F32C41
---

# Define tu experimento en el panel del Centro de desarrollo

Para ejecutar un experimento en tu aplicación de la Plataforma universal de Windows (UWP) con pruebas A/B, empieza por definir tu experimento en el panel del Centro de desarrollo

Las siguientes secciones describen el proceso general para definir un experimento en el panel. Para ver un tutorial que muestra de principio a fin el proceso de crear y ejecutar un experimento, consulta [Crea y ejecuta tu primer experimento con pruebas A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

## Obtén una clave de API

Para empezar, ve a la página **Experimentos** del panel del Centro de desarrollo y obtén una *clave de API* para tu experimento.

Una clave de API es un identificador único que asocia tu aplicación con un experimento en tu cuenta del Centro de desarrollo. Cada experimento está asociado con exactamente una clave de API. Sin embargo, una aplicación puede tener varias claves de API y cada clave de API puede asociarse con múltiples experimentos. Puedes usar claves de API para ayudar a diferenciar entre distintos conjuntos de experimentos. Por ejemplo, podría tener una serie de experimentos que lanza para los evaluadores de su organización y otra serie de experimentos que lanza únicamente para los usuarios externos de su aplicación.

Debes usar esta clave de API para conectar con el servicio de pruebas A/B en el código de tu aplicación. Para obtener más información, consulta [Escribe el código de tu aplicación para los experimentos](code-your-experiment-in-your-app.md).

1. Inicia sesión en el [panel del Centro de desarrollo](https://dev.windows.com/overview).
2. En **Tus aplicaciones**, selecciona la aplicación para la que deseas crear un experimento.
3. En el panel de navegación, selecciona **Servicios** y, a continuación, selecciona **Experimentación**.
4. La sección **Claves de API** proporciona una clave de API predeterminada para tu aplicación que se denomina **Clave de API 1 #**. Si deseas usar esta clave, opcionalmente, escribe un nombre descriptivo para esta clave y cópiala para usarla en el código de tu aplicación. Para generar una nueva clave de API, seleccione **Nueva clave de API** y escribe un nombre descriptivo para la clave de API.

## Crea un experimento

A continuación, crea un nuevo experimento y define los objetivos del mismo.

1. En la sección **Experimentos**, haz clic en el botón **Nuevo experimento**.
2. En la sección **Nombre de la clave de API**, selecciona la clave de API que deseas asociar con este experimento. Si solo tienes una clave de API, se seleccionará ese clave de API de manera predeterminada.
3. En el campo **Nombre del experimento**, escriba un nombre que puedes usar para identificar fácilmente el experimento. Después de crear un experimento, este nombre aparece en la lista de experimentos en borrador, activos y finalizados en la página **Experimentos**.
4. Si quieres crear un experimento de prueba, haz clic en la casilla de verificación **Probar experimento**. La diferencia entre los experimentos de prueba y los experimentos normales es que solo los experimentos de prueba se pueden cambiar después de que se activan.

  Los experimentos de prueba están pensados para ayudarte a probar todas las variaciones en un dispositivo del cliente antes de lanzar el experimento a los clientes. Para asegurarte de que una variación se ejecute en los clientes según lo previsto, puedes activar un experimento de prueba con el 100% de la distribución asignada a una variación y el 0% asignada a otras variaciones. Después de comprobar esta variación, puedes repetir el proceso para las otras variaciones.
  > **Nota** Marca esta casilla solo si vas a crear un experimento de prueba para validar parámetros a través de pruebas internas. No actives esta casilla si vas a crear un experimento que lanzarás a los clientes.

5. En el campo **Nombre de evento de vista** , escribe el nombre del *evento de vista* para tu experimento. El evento de vista es una cadena arbitraria que representa una actividad cuando el usuario comienza a visualizar una variación que forma parte de tu experimento. El código de tu aplicación enviará esta cadena de evento de vista al Centro de desarrollo cuando el usuario comience a visualizar una variación. Para obtener más información, consulta [Escribe el código de tu aplicación para los experimentos](code-your-experiment-in-your-app.md).
6. En la sección **Objetivos y eventos de conversión**, define al menos un objetivo para tu experimento:
  * En el campo **Nombre del objetivo** , escribe un nombre descriptivo para tu objetivo. Después de ejecutar un experimento, este nombre aparece en el resumen de resultados del experimento.
  * En el campo **Nombre de evento de conversión**, escribe el nombre del *evento de conversión* para este objetivo. Un evento de conversión es una cadena arbitraria que representa una meta de este objetivo. El código de tu aplicación enviará esta cadena de evento de conversión al Centro de desarrollo cuando el usuario alcanza un objetivo. Para obtener más información, consulta [Escribe el código de tu aplicación para los experimentos](code-your-experiment-in-your-app.md).
  * En el campo **Objetivo** , elige **Maximizar** o **Minimizar**, en función de si deseas maximizar o minimizar las repeticiones del evento de conversión. Esta información se usa en el resumen de resultados del experimento.

  >**Nota** El Centro de desarrollo solo notifica el primer evento de conversión para cada vista de usuario en un período de 24 horas. Si un usuario desencadena varios eventos de conversión en tu aplicación en un período de 24 horas, solo se informa el primer evento de conversión. Esto está pensado para ayudar a evitar que un solo usuario sesgue los resultados del experimento de un grupo de muestra de usuarios cuando el objetivo es maximizar el número de usuarios que realizan una conversión.

## Define las variaciones y la configuración del experimento

A continuación, define las variaciones y la configuración de tu prueba.

* Una *variación* es una colección de una o más opciones de configuración que estás probando en el experimento. Cada experimento debe tener al menos una opción de configuración y dos variaciones (incluido el control). Un experimento puede tener hasta cinco variaciones.
* Una *configuración* es un valor que tu aplicación usa para inicializar una variable de programa. Durante el experimento, el valor de la configuración cambia de variación en variación. Después de finalizar el experimento, se le asigna a la configuración el valor de la variación que elijas lanzar a todos los usuarios de tu aplicación. Las configuraciones puede tener los siguientes tipos: cadena, booleano, doble y entero.

Para definir las variaciones y la configuración de tu experimento:
1. En la sección **Variaciones y configuración**, verás dos variaciones predeterminadas, **Variación A (Control)** y **Variación B**. Si quieres más variaciones, haz clic en **Agregar variación**. Opcionalmente, puedes cambiar el nombre de cada variación.
2. A continuación, crea las configuraciones de tus variaciones. Haz clic en **Agregar configuración** para crear cada nueva configuración y escribe el nombre y el valor de la misma en cada variación.
3. De manera predeterminada, las variaciones se distribuyen por igual a los usuarios de tu aplicación. Si deseas elegir un porcentaje de distribución específico, desactiva la casilla de verificación **Distribuir de forma equitativa** y escribe los porcentajes en la fila**(%) de distribución**.

## Guarda tu experimento

Cuando termines de escribir los campos necesarios para tu experimento, haz clic en **Guardar** para guardar el experimento.

> **Importante** Después de guardar un experimento, ya no puedes cambiar su clave de API, incluso si aún no lo hubieras activado.

Si estás satisfecho con los parámetros de tu experimento y estás listo para activarlo para poder empezar la recopilación de datos del experimento para tu aplicación, haz clic en **Activar**. Cuando el experimento esté activo, tu aplicación puede recuperar la configuración de la variación y notificar los eventos de vista y conversión al Centro de desarrollo.

> **Importante** Después de activar un experimento, ya no se pueden modificar sus parámetros a menos de que sea un experimento de prueba (hiciste clic en la casilla de verificación **Experimento de prueba** cuando lo creaste). Te recomendamos que escribas el código del experimento en tu aplicación antes de activar el experimento.

## Pasos siguientes

Después de definir el experimento en el panel del Centro de desarrollo, estás listo para los siguientes pasos:
1. [Escribe el código de tu aplicación para los experimentos](code-your-experiment-in-your-app.md). Usa una API en el SDK de monetización y la participación de Microsoft Store para obtener la configuración de la variación para el experimento, usa estos datos para modificar el comportamiento de la característica que estás probando y envía los eventos de vista y conversión al Centro de desarrollo.
2. [Ejecuta y administra tu experimento en el panel del Centro de desarrollo](manage-your-experiment.md). Usa el panel para revisar los resultados y completar el experimento.

## Temas relacionados

  * [Escribe el código de tu aplicación para los experimentos.](code-your-experiment-in-your-app.md)
  * [Administra tu experimento en el panel del Centro de desarrollo](manage-your-experiment.md)
  * [Crea y ejecuta tu primer experimento con pruebas A/B](create-and-run-your-first-experiment-with-a-b-testing.md)
  * [Ejecuta experimentos para aplicaciones con pruebas A/B](run-app-experiments-with-a-b-testing.md)


<!--HONumber=Mar16_HO5-->


