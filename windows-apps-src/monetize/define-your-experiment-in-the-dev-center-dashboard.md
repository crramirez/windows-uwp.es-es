---
Description: Antes de poder ejecutar un experimento en su aplicación de plataforma Universal de Windows (UWP) con un pruebas a/b, debe definir el experimento en el centro de partners.
title: Definir el experimento en el Centro de partners
ms.assetid: 675F2ADE-0D4B-41EB-AA4E-56B9C8F32C41
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP, Microsoft Store Services SDK, pruebas A/B, experimentos
ms.localizationpriority: medium
ms.openlocfilehash: 7818d9e251233c757618d60abaa156d294afb4b5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57624170"
---
# <a name="define-your-experiment-in-partner-center"></a>Definir el experimento en el Centro de partners

Después de [crear un proyecto y definir variables remotas en el centro de partners](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md) y [codificar la aplicación para la experimentación](code-your-experiment-in-your-app.md), está listo para crear un experimento en el proyecto. Al crear el experimento, defines los objetivos y otras variaciones que los usuarios recibirán.

Para ver un tutorial que muestre de principio a fin el proceso de crear y ejecutar un experimento, consulta [Crear y ejecutar el primer experimento con pruebas A/B](create-and-run-your-first-experiment-with-a-b-testing.md).

<span id="get-an-api-key" />
<span id="create-an-experiment" />

## <a name="create-your-experiment"></a>Crear el experimento

1. Inicia sesión en el [Centro de partners](https://partner.microsoft.com/dashboard).
2. En **Tus aplicaciones**, selecciona la aplicación para la que deseas crear un experimento.
3. En el panel de navegación, selecciona **Servicios** y, a continuación, selecciona **Experimentación**.
4. En la página **Experimentación**, identifica en la tabla de proyectos el proyecto en el que quieres agregar un experimento y haz clic en el vínculo **Agregar experimento** para ese proyecto.
5. En el campo **Nombre del experimento**, escribe un nombre que puedes usar para identificar fácilmente el experimento. Después de crear un experimento, este nombre aparece en la lista de experimentos existentes en la página **Experimentación** de tu aplicación y en la página del proyecto.
6. Si quieres editar el experimento mientras está activo, haz clic en la casilla **Experimento editable**. Activa esta casilla solo si vas a crear un experimento para validar todas las variaciones a través de pruebas internas. Para obtener más información, consulta la sección [Crear un experimento para pruebas internas](define-your-experiment-in-the-dev-center-dashboard.md#test_experiments).
    > [!NOTE]
    > No actives esta casilla si vas a crear un experimento que publicarás para clientes (es decir, un experimento asociado a un id. de proyecto que se usa en una versión de tu aplicación que está disponible para clientes). La edición de un experimento mientras está activo invalida los resultados del experimento.

7. En la lista desplegable **Nombre de proyecto**, se selecciona el proyecto actual automáticamente. Si quieres agregar el nuevo experimento a otro proyecto, puedes seleccionar ese proyecto aquí. De lo contrario, deja solo esta selección.
8.   Anota el valor de [Id. de proyecto](run-app-experiments-with-a-b-testing.md#terms). Cuando se [codificar la aplicación para la experimentación](code-your-experiment-in-your-app.md), debe hacer referencia a este identificador en el código para que pueda recibir datos de variación y notificar los eventos de vista y la conversión a centro de partners.
9. En la sección **Evento de visualización**, escribe el nombre del [evento de visualización](run-app-experiments-with-a-b-testing.md#terms) para tu experimento en el campo **Nombre de evento de visualización**.
10. En la sección **Objetivos y eventos de conversión**, define al menos un objetivo para tu experimento:
  * En el campo **Nombre del objetivo** , escribe un nombre descriptivo para tu objetivo. Después de ejecutar un experimento, este nombre aparece en el resumen de resultados del experimento.
  * En el campo **Nombre de evento de conversión**, escribe el nombre del [evento de conversión](run-app-experiments-with-a-b-testing.md#terms) para este objetivo.
  * En el campo **Objetivo**, elige **Maximizar** o **Minimizar**, en función de si deseas maximizar o minimizar las repeticiones del evento de conversión. Esta información se usa en el resumen de resultados del experimento.

> [!NOTE]
> Centro de partners notifica solo el primer evento de conversión para cada vista de usuario en un período de 24 horas. Si un usuario desencadena varios eventos de conversión en tu aplicación en un período de 24 horas, solo se informa el primer evento de conversión. Esto está pensado para ayudar a evitar que un solo usuario sesgue los resultados del experimento de un grupo de muestra de usuarios cuando el objetivo es maximizar el número de usuarios que realizan una conversión.

<span id="define-the-variations-and-settings-for-the-experiment" />

### <a name="define-the-remote-variables-and-variations-for-your-experiment"></a>Definir las variables remotas y las variaciones del experimento

A continuación, define las [variables](run-app-experiments-with-a-b-testing.md#terms) remotas y las [variaciones](run-app-experiments-with-a-b-testing.md#terms) del experimento.

1. En el **remotas variables y las variaciones** sección, debería ver dos variaciones de forma predeterminada, **variación un (Control)** y **B variación**. Si desea más variaciones, haga clic en **agregar variación**. Opcionalmente, puedes cambiar el nombre de cada variación.
2. De manera predeterminada, las variaciones se distribuyen por igual a los usuarios de tu aplicación. Si deseas elegir un porcentaje de distribución específico, desactiva la casilla **Distribuir de forma equitativa** y escribe los porcentajes en la fila **% de distribución**.
3. Agrega variables remotas a tus variaciones. En el control de la lista desplegable de la parte inferior de esta sección, elige cada variable que quieres agregar y haz clic en **Agregar variable**.
    > [!NOTE]
    > Las variables que figuran en la lista de este control se heredan del proyecto para el experimento. El valor predeterminado de la variable (tal como se define en el proyecto) se asigna automáticamente a la variación de control. Si quieres crear nuevas variables que no se muestran aquí, ve a la página del proyecto relacionado y agrégalas allí.

4. Edita los valores de variable de cada variación única del experimento (es decir, las variaciones que no son la variación de control).

<span id="save-and-activate-your-experiment" />

### <a name="save-and-activate-your-experiment"></a>Guardar y activar el experimento

Cuando termines de especificar los campos necesarios para tu experimento, haz clic en **Guardar** para guardar el experimento.

Si estás satisfecho con los parámetros de tu experimento y estás listo para activarlo y, así, poder empezar la recopilación de datos del experimento de tu aplicación, haz clic en **Activar**. Cuando está activo el experimento, la aplicación puede recuperar variables variación y notificar los eventos de vista y la conversión a centro de partners. Para obtener más información, consulte [ejecutar y administrar el experimento en el centro de partners](manage-your-experiment.md).

> [!IMPORTANT]
> Un proyecto solo puede contener un experimento activo a la vez. Después de activar un experimento, ya no se pueden modificar sus parámetros a menos que hayas activado la casilla **Experimento editable** al crear el experimento. Te recomendamos que escribas el código del experimento en tu aplicación antes de activar el experimento.

<span id="test_experiments"/>

## <a name="create-an-experiment-for-internal-testing"></a>Crear un experimento para pruebas internas

Es posible que quieras probar el experimento con una audiencia controlada (por ejemplo, un conjunto de evaluadores internos) y confirmar que todas las variaciones funcionan según lo previsto antes de activar el experimento para los clientes. Puedes hacerlo al crear un experimento que tenga seleccionada la opción **Experimento editable**.

Para probar el experimento antes de publicarlo para los clientes, sigue este proceso:

1. Crea dos proyectos: uno para la versión pública de la aplicación y otro para una compilación privada de la aplicación que solo está disponible para la audiencia de prueba. Las siguientes instrucciones hacen referencia a estos proyectos como el proyecto público y el proyecto de prueba, respectivamente.
2. Cuando [escribas el código de tu aplicación para los experimentos](code-your-experiment-in-your-app.md), haz referencia al id. de proyecto de tu proyecto público en la compilación pública de la aplicación. En la compilación privada de la aplicación, haz referencia al id. de proyecto del proyecto de prueba.
3. Crea un experimento en el proyecto de prueba y selecciona la opción **Experimento editable** para el experimento.
4. Activa el experimento en el proyecto de prueba. Asigna una distribución de 100 % a una variación y comprueba si esta variación funciona según lo esperado para los evaluadores. Repite el proceso para otras variaciones.
5. Después de comprobar que las variaciones funcionan según lo previsto, realiza los cambios finales necesarios en el experimento del proyecto de prueba. Cuando estés listo para publicar el experimento para los clientes, clona el experimento en el proyecto público. En este experimento, no selecciones la opción **Experimento editable**.
4. Asegúrate de que la distribución de variación de destino es correcta en el experimento clonado.
5. Activa el experimento clonado para publicar el experimento para los clientes.

## <a name="next-steps"></a>Pasos siguientes

Después de definir el experimento en el centro de partners y el código del experimento en la aplicación, está listo para [ejecutar y administrar el experimento en el centro de partners](manage-your-experiment.md).

## <a name="related-topics"></a>Temas relacionados

* [Cree un proyecto y definir variables remotas en el centro de partners](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [Codificar la aplicación para la experimentación](code-your-experiment-in-your-app.md)
* [Administrar el experimento en el centro de partners](manage-your-experiment.md)
* [Cree y ejecute su primer experimento con un pruebas A/b](create-and-run-your-first-experiment-with-a-b-testing.md)
* [Ejecutar los experimentos de la aplicación con un pruebas A/b](run-app-experiments-with-a-b-testing.md)
